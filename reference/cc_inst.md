# Identify Records in the Vicinity of Biodiversity Institutions

Removes or flags records assigned to the location of zoos, botanical
gardens, herbaria, universities and museums, based on a global database
of ~10,000 such biodiversity institutions. Coordinates from these
locations can be related to data-entry errors, false automated
geo-reference or individuals in captivity/horticulture.

## Usage

``` r
cc_inst(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  species = "species",
  buffer = 100,
  geod = FALSE,
  ref = NULL,
  verify = FALSE,
  verify_mltpl = 10,
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

  numerical. The buffer around each institution, where records should be
  flagged as problematic, in decimal degrees. Default = 100m.

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
  [`institutions`](https://docs.ropensci.org/CoordinateCleaner/reference/institutions.md).
  Default =
  [`institutions`](https://docs.ropensci.org/CoordinateCleaner/reference/institutions.md)

- verify:

  logical. If TRUE, records close to institutions are only flagged, if
  there are no other records of the same species in the greater vicinity
  (a radius of buffer \* verify_mltpl).

- verify_mltpl:

  numerical. indicates the factor by which the radius for verify exceeds
  the radius of the initial test. Default = 10, which might be suitable
  if geod is TRUE, but might be too large otherwise.

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

## Details

Note: the buffer radius is in degrees, thus will differ slightly between
different latitudes.

## See also

Other Coordinates:
[`cc_aohi()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_aohi.md),
[`cc_cap()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cap.md),
[`cc_cen()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cen.md),
[`cc_coun()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_coun.md),
[`cc_dupl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_dupl.md),
[`cc_equ()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_equ.md),
[`cc_gbif()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_gbif.md),
[`cc_iucn()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_iucn.md),
[`cc_outl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_outl.md),
[`cc_sea()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_sea.md),
[`cc_urb()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_urb.md),
[`cc_val()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_val.md),
[`cc_zero()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_zero.md)

## Examples

``` r

x <- data.frame(species = letters[1:10],
                decimalLongitude = c(runif(99, -180, 180), 37.577800),
                decimalLatitude = c(runif(99, -90,90), 55.710800))

#large buffer for demonstration, using geod = FALSE for shorter runtime
cc_inst(x, value = "flagged", buffer = 10, geod = FALSE)
#> Testing biodiversity institutions
#> Flagged 1 records.
#>   [1]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [13]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [25]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [37]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [49]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [61]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [73]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [85]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [97]  TRUE  TRUE  TRUE FALSE

if (FALSE) { # \dontrun{
#' cc_inst(x, value = "flagged", buffer = 50000) #geod = T
} # }
```
