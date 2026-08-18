# Identify Coordinates in Vicinity of Country Capitals.

Removes or flags records within a certain radius around country
capitals. Poorly geo-referenced occurrence records in biological
databases are often erroneously geo-referenced to capitals.

## Usage

``` r
cc_cap(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  species = "species",
  buffer = 10000,
  geod = TRUE,
  ref = NULL,
  verify = FALSE,
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

- species:

  character string. The column with the species identity. Only required
  if verify = TRUE.

- buffer:

  The buffer around each capital coordinate (the centre of the city),
  where records should be flagged as problematic. Units depend on geod.
  Default = 10 kilometres.

- geod:

  logical. If TRUE the radius around each capital is calculated based on
  a sphere, buffer is in meters and independent of latitude. If FALSE
  the radius is calculated assuming planar coordinates and varies
  slightly with latitude. Default = TRUE. See
  https://seethedatablog.wordpress.com/ for detail and credits.

- ref:

  SpatVector (geometry: polygons). Providing the geographic gazetteer.
  Can be any SpatVector (geometry: polygons), but the structure must be
  identical to
  [`countryref`](https://docs.ropensci.org/CoordinateCleaner/reference/countryref.md).
  Default =
  [`countryref`](https://docs.ropensci.org/CoordinateCleaner/reference/countryref.md).

- verify:

  logical. If TRUE records are only flagged if they are the only record
  in a given species flagged close to a given reference. If FALSE, the
  distance is the only criterion

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
[`cc_cen()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cen.md),
[`cc_coun()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_coun.md),
[`cc_dupl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_dupl.md),
[`cc_equ()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_equ.md),
[`cc_gbif()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_gbif.md),
[`cc_inst()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_inst.md),
[`cc_iucn()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_iucn.md),
[`cc_outl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_outl.md),
[`cc_sea()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_sea.md),
[`cc_urb()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_urb.md),
[`cc_val()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_val.md),
[`cc_zero()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_zero.md)

## Examples

``` r
if (FALSE) { # \dontrun{
x <- data.frame(species = letters[1:10],
                decimalLongitude = c(runif(99, -180, 180), -47.882778),
                decimalLatitude = c(runif(99, -90, 90), -15.793889))

cc_cap(x)
cc_cap(x, value = "flagged")
} # }
```
