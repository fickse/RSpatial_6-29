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

# 1 Get the data in

```
library(raster)
library(maps)
```

## Loading a raster
```
dem <- raster('dem.tif')
```
that was easy!

as a side note, raster has a useful function `getData` for downloading commonly used geophysical data including elevation, climate and geopolitical boundaries. The previous dataset could have been loaded with the following:

```
dem <- getData('alt', country = 'USA')[[4]] # the [[4]] selects the 4th item in the list, which is Hawaii
```

## Loading a shapefile
```
w <- shapefile('watersheds.shp')
```
also easy!

## Loading points from a csv file
```
d <- read.csv('stations.csv')

# make d into a SpatialPointsDataFrame
coordinates(d) <- d[,c('lon','lat')]
```

# 2 Manipulations

## reprojection

First lets find out if our data is in the correct projection. Raster provides a nice tool to extract this data.
```
projection(dem)
projection(w)
projection(d)
```

```
