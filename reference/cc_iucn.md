# Identify Records Outside Natural Ranges

Removes or flags records outside of the provided natural range polygon,
on a per species basis. Expects one entry per species. See the example
or <https://www.iucnredlist.org/resources/spatial-data-download> for the
required polygon structure.

## Usage

``` r
cc_iucn(
  x,
  range,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  species = "species",
  buffer = 0,
  value = "clean",
  verbose = TRUE
)
```

## Arguments

- x:

  data.frame. Containing geographical coordinates and species names.

- range:

  a SpatVector of natural ranges for species in x. Must contain a column
  named as indicated by `species`. See details.

- lon:

  character string. The column with the longitude coordinates. Default =
  “decimalLongitude”.

- lat:

  character string. The column with the latitude coordinates. Default =
  “decimalLatitude”.

- species:

  a character string. The column with the species name. Default =
  “species”.

- buffer:

  numerical. The buffer around each species' range, from where records
  should be flagged as problematic, in meters. Default = 0.

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

Download natural range maps in suitable format for amphibians, birds,
mammals and reptiles from
<https://www.iucnredlist.org/resources/spatial-data-download>. Note: the
buffer radius is in degrees, thus will differ slightly between different
latitudes.

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
[`cc_outl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_outl.md),
[`cc_sea()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_sea.md),
[`cc_urb()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_urb.md),
[`cc_val()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_val.md),
[`cc_zero()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_zero.md)

## Examples

``` r
library(terra)
#> terra 1.9.34

x <- data.frame(species = c("A", "B"),
decimalLongitude = runif(100, -170, 170),
decimalLatitude = runif(100, -80,80))

range_species_A <- cbind(c(-45,-45,-60,-60,-45), c(-10,-25,-25,-10,-10))
rangeA <- terra::vect(range_species_A, "polygons")
range_species_B <- cbind(c(15,15,32,32,15), c(10,-10,-10,10,10))
rangeB <- terra::vect(range_species_B, "polygons")
range <- terra::vect(list(rangeA, rangeB))
range$binomial <- c("A", "B")

cc_iucn(x = x, range = range, buffer = 0)
#> Testing natural ranges
#> Warning: no projection information for reference found, 
#>               assuming '+proj=longlat +datum=WGS84 +no_defs'
#> Removed 100 records.
#> [1] species          decimalLongitude decimalLatitude 
#> <0 rows> (or 0-length row.names)
```
