
<!-- README.md is generated from README.Rmd. Please edit that file -->
smoothr
=======

[![lifecycle](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental) [![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0) [![Travis build status](https://travis-ci.org/mstrimas/smoothr.svg?branch=master)](https://travis-ci.org/mstrimas/smoothr) [![AppVeyor build status](https://ci.appveyor.com/api/projects/status/github/mstrimas/smoothr?branch=master&svg=true)](https://ci.appveyor.com/project/mstrimas/smoothr) [![Coverage status](https://codecov.io/gh/mstrimas/smoothr/branch/master/graph/badge.svg)](https://codecov.io/github/mstrimas/smoothr?branch=master)

The goal of `smoothr` is to offer a variety of methods for smoothing spatial features (i.e. polygons and lines). The application is to to make edges appear more natural or aesthetically pleasing, especially when converting raster data to vector format. This package offers support for both `sp` and `sf` spatial objects, and the following smoothing methods:

-   **Bézier splines**: smoothing using Bézier spline interpolation via the `spline()` function. This method interpolates between existing vertices and the resulting smoothed feature will pass through the vertices of the input feature.

Installation
------------

You can install smoothr from github with:

``` r
# install.packages("devtools")
devtools::install_github("mstrimas/smoothr")
```

Example
-------

Two example feature sets are included in this package. `jagged_polygons` contains 9 polygons with sharp edges for smoothing, some have holes and some are multipart polygons. These can be smoothed with:

``` r
library(sf)
library(smoothr)
par(mar = c(0, 0, 0, 0), oma = c(0, 0, 2, 0), mfrow = c(3, 3))
for (i in 1:nrow(jagged_polygons)) {
  p <- jagged_polygons[i, ]
  smoothed <- smooth(p, method = "spline")
  plot(st_geometry(smoothed), col = NA, border = NA)
  plot(st_geometry(p), col = "grey20", border = NA, add = TRUE)
  plot(st_geometry(smoothed), col = NA, border = "red", lwd = 2, add = TRUE)
  title("Smoothed Polygons (Bézier Spline)", cex.main = 2, outer = TRUE)
}
```

![](README-smooth-polygons-1.png)

`jagged_polygons` contains 9 lines with sharp edges for smoothing, some are closed loops requiring special treatment of the endpoints and some are multipart lines. These can be smoothed with:

``` r
par(mar = c(0, 0, 0, 0), oma = c(0, 0, 2, 0), mfrow = c(3, 3))
for (i in 1:nrow(jagged_lines)) {
  l <- jagged_lines[i, ]
  smoothed <- smooth(l, method = "spline")
  plot(st_geometry(smoothed), col = NA)
  plot(st_geometry(l), col = "grey20", lwd = 2, add = TRUE)
  plot(st_geometry(smoothed), col = "red", lwd = 2, add = TRUE)
  title("Smoothed Lines (Bézier Spline)", cex.main = 2, outer = TRUE)
}
```

![](README-smooth-lines-1.png)