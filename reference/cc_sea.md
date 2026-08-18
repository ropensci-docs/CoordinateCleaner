# Identify Non-terrestrial Coordinates

Removes or flags coordinates outside the reference landmass. Can be used
to restrict datasets to terrestrial taxa, or exclude records from the
open ocean, when depending on the reference (see details). Often records
of terrestrial taxa can be found in the open ocean, mostly due to
switched latitude and longitude.

## Usage

``` r
cc_sea(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  ref = NULL,
  scale = 110,
  value = "clean",
  speedup = TRUE,
  verbose = TRUE,
  buffer = NULL
)
```

## Arguments

- x:

  data.frame. Containing geographical coordinates and species names.

- lon:

  character string. The column with the longitude coordinates. Default =
  “decimalLongitude”.

- lat:

  character string. The column with the latitude coordinates. Default =
  “decimalLatitude”.

- ref:

  SpatVector (geometry: polygons). Providing the geographic gazetteer.
  Can be any SpatVector (geometry: polygons), but the structure must be
  identical to rnaturalearth::ne_download(scale = 110, type = 'land',
  category = 'physical', returnclass = 'sf'). Default =
  rnaturalearth::ne_download(scale = 110, type = 'land', category =
  'physical', returnclass = 'sf').

- scale:

  the scale of the default reference, as downloaded from natural earth.
  Must be one of 10, 50, 110. Higher numbers equal higher detail.
  Default = 110.

- value:

  character string. Defining the output value. See value.

- speedup:

  logical. Using heuristic to speed up the analysis for large data sets
  with many records per location.

- verbose:

  logical. If TRUE reports the name of the test and the number of
  records flagged.

- buffer:

  numeric. Units are in meters. If provided, a buffer is created around
  the sea polygon, or ref provided.

## Value

Depending on the ‘value’ argument, either a `data.frame` containing the
records considered correct by the test (“clean”) or a logical vector
(“flagged”), with TRUE = test passed and FALSE = test failed/potentially
problematic . Default = “clean”.

## Details

In some cases flagging records close of the coastline is not
recommendable, because of the low precision of the reference dataset,
minor GPS imprecision or because a dataset might include coast or
marshland species. If you only want to flag records in the open ocean,
consider using a buffered landmass reference, e.g.:
[`buffland`](https://docs.ropensci.org/CoordinateCleaner/reference/buffland.md).

## Note

See <https://ropensci.github.io/CoordinateCleaner/> for more details and
tutorials.

## See also

Other Coordinates:
[`cc_aohi()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_aohi.md),
[`cc_cap()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cap.md),
[`cc_cen()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cen.md),
[`cc_coun()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_coun.md),
[`cc_dupl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_dupl.md),
[`cc_equ()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_equ.md),
[`cc_gbif()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_gbif.md),
[`cc_inst()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_inst.md),
[`cc_iucn()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_iucn.md),
[`cc_outl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_outl.md),
[`cc_urb()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_urb.md),
[`cc_val()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_val.md),
[`cc_zero()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_zero.md)

## Examples

``` r
x <- data.frame(species = letters[1:10], 
                decimalLongitude = runif(10, -30, 30), 
                decimalLatitude = runif(10, -30, 30))
                
cc_sea(x, value = "flagged")
#> Testing sea coordinates
#> Reading ne_110m_land.zip from naturalearth...
#> Flagged 6 records.
#>  [1] FALSE  TRUE FALSE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE
```
