Map polygon and feature extractor
==============
#### An NYPL Labs project

**Author:** Mauricio Giraldo Arteaga [@mgiraldo] / NYPL Labs [@nypl_labs]

A open-source map vectorizer. Provided **as is** by [NYPL Labs](http://www.nypl.org/collections/labs). Project based on a workflow suggested by Michael Resig.

## Like OCR for maps

This project aims to automate the manual process of geographic polygon and attribute data extraction from maps (i.e. georectified images) including those from [insurance atlases](http://digitalcollections.nypl.org/search/index?filters%5Btitle_uuid_s%5D%5B%5D=Maps%20of%20the%20city%20of%20New%20York%7C%7C323e4180-c603-012f-0c9f-58d385a7bc34&keywords=&layout=false#/?scroll=24) published in the 19th and early 20th centuries. [Here is some background](http://www.nypl.org/blog/2012/06/13/nyc-historical-gis-project) on why we're doing this and [here is one of the maps](http://digitalcollections.nypl.org/items/510d47e0-c7cc-a3d9-e040-e00a18064a99) we're extracting polygons from. This [example map layer](http://maps.nypl.org/warper/layers/859) shows what these atlases look like once geo-rectified, i.e. geographically normalized.

[The New York Public Library](http://www.nypl.org) has **hundreds of atlases** with **tens of thousands** of these sheets and there is no way we can extract data manually in a reasonable amount of time. 

Just so you get an idea, it took NYPL staff coordinating a small army of volunteers **three years to produce** 170,000 polygons with attributes (from just four of hundreds of atlases at NYPL).

It now takes a period of time closer to **24 hours** to generate a comparable number of polygons with some basic metadata.

The goal is to extract the following data (✔ = mostly solved so far, ✢ = in progress):

* ✔ shape
* ✔ color
* ✢ dot presence
* ✢ dot count
* ✢ dot type (full vs outline)
* skylights
* numbers (not optimistic, but maybe **one of you** knows how extract numbers from these images)

#### Example input
![Example input map](https://raw.github.com/NYPL/map-vectorizer/master/example_input.png)

#### Example output
![The resulting shapefile output superimposed](https://raw.github.com/NYPL/map-vectorizer/master/example_output.png)

#### Extra feature detection
![Extra feature detection for the polygon](https://raw.github.com/NYPL/map-vectorizer/master/feature_detection.png)

## Dependencies

A few things to be installed in your system in order to work properly. So far it has been **tested on Mac OS X Lion** so these instructions apply to that configuration only. I am sure you will be able to adapt it to your current configuration.

* [Python] with [OpenCV]
* [ImageMagick] with the `libtiff` delegate. If using `brew` this means:
    * `brew install libtiff`
    * `brew install imagemagick --with-libtiff`
    * To verify all is well:
    * `convert -list format` should display `TIFF` and `TIFF64` in the list 
* [R] - Make sure it is in your PATH (so you can run it via command-line by typing `R`).
* You'll need the following R packages. On OS X simply navigate to `Packages & Data`, choose your CRAN mirror region, then search for and install:
    * `rgdal`
    * `alphahull` (you will need `tripack`, `sgeostat`, `splancs` as dependencies)
    * `igraph` 
    * `shapefiles` 
    * On the CLI (by typing `R` in a terminal window) you will know you have all the required dependencies if you can run this with no errors:

```
    library(rgdal)
    library(alphahull)
    library(igraph)
    library(shapefiles)
    quit() # this will quit R
```

* [GIMP]
* [GDAL Tools], on OS X try [version 1.9](http://www.kyngchaos.com/files/software/frameworks/GDAL_Complete-1.9.dmg) 
* It is also a good idea to install [QGIS] to test your results

## First run

These step by step instructions should work as-is. If not, **check all the above are working** before submitting an issue.

1. Take note of the path where the GIMP executable is installed (the default value in the vectorizer is the Mac OS location: `/Applications/Gimp.app/Contents/MacOS/gimp-2.8`).
2. Run the script on the provided test GeoTIFF:
`python vectorize_map.py test.tif`
3. Accept the GIMP folder location or input a different one and press ENTER.

**NOTE:** The vectorizer has problems with *filenames that contain spaces*. This will be supported eventually.

This should take about 70 seconds to process. **If it takes less there might be an error** (or your machine rulez). Take a look at the console output to find the possible culprit.

If it works, you will see a `test` folder with a `test-traced` set of files (`.shp`, `.dbf`, `.prj` and `.shx`) and two log files.

## Acknowledgements

* Michael Resig
* Chris Garrard for his [sample code to assemble and disassemble shapefiles](http://cosmicproject.org/OGR/cris_example_write.html)
* Barry Rowlingson for his [tutorial on converting alpha shapes to polygons](http://rpubs.com/geospacedman/alphasimple)

## Change log

* 0.6: Fixed bug introduced in 0.5. Removed mention to GIMP preferences in README.
* 0.5: Added support for absolute paths.
* 0.4: Added a config file (rename `vectorize_config_default.txt` to `vectorize_config.txt`).
* 0.3: Added `consolidator.py` to assemble a set of shapefiles in a folder into a single file.
* 0.2: Added very rough OpenCV circle and cross detection (not working very well but it is a starting point).
* 0.1: Added GeoJSON output.

[@mgiraldo]: https://twitter.com/mgiraldo
[@nypl_labs]: https://twitter.com/nypl_labs
[Python]: http://www.python.org/
[OpenCV]: http://opencv.org/
[ImageMagick]: http://www.imagemagick.org/script/download.php
[R]: http://www.r-project.org/
[GIMP]: http://www.gimp.org/
[GDAL Tools]: http://trac.osgeo.org/gdal/wiki/DownloadingGdalBinaries
[QGIS]: http://qgis.org/
