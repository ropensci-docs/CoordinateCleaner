# Identify Records Assigned to GBIF Headquarters

Removes or flags records within 0.5 degree radius around the GBIF
headquarters in Copenhagen, DK.

## Usage

``` r
cc_gbif(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  species = "species",
  buffer = 1000,
  geod = TRUE,
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

  numerical. The buffer around the GBIF headquarters, where records
  should be flagged as problematic. Units depend on geod. Default = 100
  m.

- geod:

  logical. If TRUE the radius is calculated based on a sphere, buffer is
  in meters. If FALSE the radius is calculated in degrees. Default = T.

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

## Details

Not recommended if working with records from Denmark or the Copenhagen
area.

## See also

Other Coordinates:
[`cc_aohi()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_aohi.md),
[`cc_cap()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cap.md),
[`cc_cen()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cen.md),
[`cc_coun()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_coun.md),
[`cc_dupl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_dupl.md),
[`cc_equ()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_equ.md),
[`cc_inst()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_inst.md),
[`cc_iucn()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_iucn.md),
[`cc_outl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_outl.md),
[`cc_sea()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_sea.md),
[`cc_urb()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_urb.md),
[`cc_val()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_val.md),
[`cc_zero()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_zero.md)

## Examples

``` r

x <- data.frame(species = "A", 
                decimalLongitude = c(12.58, 12.58), 
                decimalLatitude = c(55.67, 30.00))
                
cc_gbif(x)
#> Testing GBIF headquarters, flagging records around Copenhagen
#> Removed 1 records.
#>   species decimalLongitude decimalLatitude
#> 2       A            12.58              30
cc_gbif(x, value = "flagged")
#> Testing GBIF headquarters, flagging records around Copenhagen
#> Flagged 1 records.
#> [1] FALSE  TRUE
```
