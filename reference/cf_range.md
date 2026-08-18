# Identify Fossils with Extreme Age Ranges

Removes or flags records with an unexpectedly large temporal range,
based on a quantile outlier test.

## Usage

``` r
cf_range(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  min_age = "min_ma",
  max_age = "max_ma",
  taxon = "accepted_name",
  method = "quantile",
  mltpl = 5,
  size_thresh = 7,
  max_range = 500,
  uniq_loc = FALSE,
  value = "clean",
  verbose = TRUE
)
```

## Arguments

- x:

  data.frame. Containing fossil records with taxon names, ages, and
  geographic coordinates.

- lon:

  character string. The column with the longitude coordinates. To
  identify unique records if `uniq_loc = TRUE`. Default =
  “decimalLongitude”.

- lat:

  character string. The column with the longitude coordinates. Default =
  “decimalLatitude”. To identify unique records if `uniq_loc = T`.

- min_age:

  character string. The column with the minimum age. Default = “min_ma”.

- max_age:

  character string. The column with the maximum age. Default = “max_ma”.

- taxon:

  character string. The column with the taxon name. If “”, searches for
  outliers over the entire dataset, otherwise per specified taxon.
  Default = “accepted_name”.

- method:

  character string. Defining the method for outlier selection. See
  details. Either “quantile” or “mad”. Default = “quantile”.

- mltpl:

  numeric. The multiplier of the interquartile range
  (`method == 'quantile'`) or median absolute deviation
  (`method == 'mad'`) to identify outliers. See details. Default = 5.

- size_thresh:

  numeric. The minimum number of records needed for a dataset to be
  tested. Default = 10.

- max_range:

  numeric. A absolute maximum time interval between min age and max age.
  Only relevant for `method` = “time”.

- uniq_loc:

  logical. If TRUE only single records per location and time point (and
  taxon if `taxon` != "") are used for the outlier testing. Default = T.

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

Other fossils:
[`cf_age()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_age.md),
[`cf_equal()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_equal.md),
[`cf_outl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_outl.md),
[`write_pyrate()`](https://docs.ropensci.org/CoordinateCleaner/reference/write_pyrate.md)

## Examples

``` r

minages <- runif(n = 11, min = 0.1, max = 25)
x <- data.frame(species = c(letters[1:10], "z"),
                lng = c(runif(n = 9, min = 4, max = 16), 75, 7),
                lat = c(runif(n = 11, min = -5, max = 5)),
                min_ma = minages, 
                max_ma = minages + c(runif(n = 10, min = 0, max = 5), 25))

cf_range(x, value = "flagged", taxon = "")
#> Warning: lat not found. Using lng instead.
#> Warning: lng not found. Using lng instead.
#> Testing temporal range outliers on dataset level
#> Flagged 1 records.
#>  [1]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
```
