# Identify Zero Coordinates

Removes or flags records with either zero longitude or latitude and a
radius around the point at zero longitude and zero latitude. These
problems are often due to erroneous data-entry or geo-referencing and
can lead to typical patterns of high diversity around the equator.

## Usage

``` r
cc_zero(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  buffer = 0.5,
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

- buffer:

  numerical. The buffer around the 0/0 point, where records should be
  flagged as problematic, in decimal degrees. Default = 0.5.

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
[`cc_urb()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_urb.md),
[`cc_val()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_val.md)

## Examples

``` r

x <- data.frame(species = "A", 
                decimalLongitude = c(0,34.84, 0, 33.98), 
                decimalLatitude = c(23.08, 0, 0, 15.98))
                
cc_zero(x)
#> Testing zero coordinates
#> Warning: [vect] guessed crs
#> Removed 3 records.
#>   species decimalLongitude decimalLatitude
#> 4       A            33.98           15.98
cc_zero(x, value = "flagged")
#> Testing zero coordinates
#> Warning: [vect] guessed crs
#> Flagged 3 records.
#> [1] FALSE FALSE FALSE  TRUE
```
