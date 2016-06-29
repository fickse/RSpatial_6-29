# Spatial Data in R
workshop notes for working with spatial data in R
SEI R-users group 6-29-2016

## Goals 
Go over some common workflows relevant to members of the group. In particular...
* Getting Data in -- importing data from common GIS formats `.shp` and `.tif`
* Basic manipulations -- projecting, extracting, cropping, overlay
* Visualizing it -- some tips and tricks for interacting with data
* A (slightly more advanced) discussion of interpolation and cross validation

## Resources
* lets try using [Etherpad](http://etherpad.org/), a tool for collaborative note-taking, during the presentation. Click on the link [here]( insert_url ), to post comments, notes, and questions throughout the session. 
* Data used for the examples are hosted in this repository. The zipped file contains the following:
    - `dem.tif` : an elevation raster for Hawaii
    - `stations.csv`: a csv of climate stations and locations with monthly rainfall
    - `watersheds.shp`: a shapefile of polygons with level x watersheds.
You can download/unzip the data by hand or run the following code to download and unzip the data to a temporary directory

```
setwd( tempdir() )  # set working directory to temporary directory 
rcurl ... # download zipped data
unzip( f , exdir = '.')
```

# 0. Prerequisites
Most of the functions we will use come from the `raster` package. Raster provides some 'high-level' interface to spatial data. Also `maps` comes in handy for quick visualization. You can download these packages with...

```
install.packages('raster', repos = "http://cran.cnr.berkeley.edu/")
install.packages('maps', repos = "http://cran.cnr.berkeley.edu/")
```
