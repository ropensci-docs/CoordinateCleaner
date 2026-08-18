# Identify Coordinates Outside their Reported Country

Removes or flags mismatches between geographic coordinates and
additional country information (usually this information is reliably
reported with specimens). Such a mismatch can occur for example, if
latitude and longitude are switched.

## Usage

``` r
cc_coun(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  iso3 = "countrycode",
  value = "clean",
  ref = NULL,
  ref_col = "iso_a3",
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

- iso3:

  a character string. The column with the country assignment of each
  record in three letter ISO code. Default = “countrycode”.

- value:

  character string. Defining the output value. See value.

- ref:

  SpatVector (geometry: polygons). Providing the geographic gazetteer.
  Can be any SpatVector (geometry: polygons), but the structure must be
  identical to
  `rnaturalearth::ne_countries(scale = "medium", returnclass = "sf")`.
  Default =
  `rnaturalearth::ne_countries(scale = "medium", returnclass = "sf")`

- ref_col:

  the column name in the reference dataset, containing the relevant ISO
  codes for matching. Default is to "iso_a3_eh" which refers to the
  ISO-3 codes in the reference dataset. See notes.

- verbose:

  logical. If TRUE reports the name of the test and the number of
  records flagged.

- buffer:

  numeric. Units are in meters. If provided, a buffer is created around
  each country polygon.

## Value

Depending on the ‘value’ argument, either a `data.frame` containing the
records considered correct by the test (“clean”) or a logical vector
(“flagged”), with TRUE = test passed and FALSE = test failed/potentially
problematic . Default = “clean”.

## Note

The ref_col argument allows to adapt the function to the structure of
alternative reference datasets. For instance, for
`rnaturalearth::ne_countries(scale = "small")`, the default will fail,
but ref_col = "iso_a3" will work.

With the default reference, records are flagged if they fall outside the
terrestrial territory of countries, hence records in territorial waters
might be flagged. See <https://ropensci.github.io/CoordinateCleaner/>
for more details and tutorials.

## See also

Other Coordinates:
[`cc_aohi()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_aohi.md),
[`cc_cap()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cap.md),
[`cc_cen()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cen.md),
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
                decimalLongitude = runif(100, -20, 30),
                decimalLatitude = runif(100, 35,60),
                countrycode = "RUS")

cc_coun(x, value = "flagged")#non-terrestrial records are flagged as wrong.
} # }
```
