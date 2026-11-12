# Identify Records Inside Urban Areas

Removes or flags records from inside urban areas, based on a geographic
gazetteer. Often records from large databases span substantial time
periods (centuries) and old records might represent habitats which today
are replaced by city area.

## Usage

``` r
cc_urb(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  ref = NULL,
  value = "clean",
  verbose = TRUE
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

  a SpatVector. Providing the geographic gazetteer with the urban areas.
  See details. By default rnaturalearth::ne_download(scale = 'medium',
  type = 'urban_areas', returnclass = "sf"). Can be any `SpatVector`,
  but the structure must be identical to
  [`rnaturalearth::ne_download()`](https://docs.ropensci.org/rnaturalearth/reference/ne_download.html).

- value:

  character string. Defining the output value. See value.

- verbose:

  logical. If TRUE reports the name of the test and the number of
  records flagged.

## Value

Depending on the ‘value’ argument, either a `data.frame` containing the
records considered correct by the test (“clean”) or a logical vector
(“flagged”), with TRUE = test passed and FALSE = test failed/potentially
problematic . Default = “clean”.

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
[`cc_sea()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_sea.md),
[`cc_val()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_val.md),
[`cc_zero()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_zero.md)

## Examples

``` r

if (FALSE) { # \dontrun{
x <- data.frame(species = letters[1:10],
                decimalLongitude = runif(100, -180, 180),
                decimalLatitude = runif(100, -90,90))

cc_urb(x)
cc_urb(x, value = "flagged")
} # }
```
