# Identify Fossils with Outlier Age

Removes or flags records that are temporal outliers based on
interquantile ranges.

## Usage

``` r
cf_age(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  min_age = "min_ma",
  max_age = "max_ma",
  taxon = "accepted_name",
  method = "quantile",
  size_thresh = 7,
  mltpl = 5,
  replicates = 5,
  flag_thresh = 0.5,
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

- size_thresh:

  numeric. The minimum number of records needed for a dataset to be
  tested. Default = 10.

- mltpl:

  numeric. The multiplier of the interquartile range
  (`method == 'quantile'`) or median absolute deviation
  (`method == 'mad'`) to identify outliers. See details. Default = 5.

- replicates:

  numeric. The number of replications for the distance matrix
  calculation. See details. Default = 5.

- flag_thresh:

  numeric. The fraction of passed replicates necessary to pass the test.
  See details. Default = 0.5.

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

## Details

The outlier detection is based on an interquantile range test. A
temporal distance matrix among all records is calculated based on a
single point selected by random between the minimum and maximum age for
each record. The mean distance for each point to all neighbours is
calculated and the sum of these distances is then tested against the
interquantile range and flagged as an outlier if \\x \> IQR(x) + q_75 \*
mltpl\\. The test is replicated ‘replicates’ times, to account for
dating uncertainty. Records are flagged as outliers if they are flagged
by a fraction of more than ‘flag.thresh’ replicates. Only datasets/taxa
comprising more than ‘size_thresh’ records are tested. Distance are
calculated as Euclidean distance.

## Note

See <https://ropensci.github.io/CoordinateCleaner/> for more details and
tutorials.

## See also

Other fossils:
[`cf_equal()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_equal.md),
[`cf_outl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_outl.md),
[`cf_range()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_range.md),
[`write_pyrate()`](https://docs.ropensci.org/CoordinateCleaner/reference/write_pyrate.md)

## Examples

``` r

minages <- c(runif(n = 11, min = 10, max = 25), 62.5)
x <- data.frame(species = c(letters[1:10], rep("z", 2)),
                min_ma = minages,
                max_ma = c(minages[1:11] + runif(n = 11, min = 0, max = 5), 65))

cf_age(x, value = "flagged", taxon = "")
#> Testing temporal outliers on dataset level
#> Flagged 1 records.
#>     1     2     3     4     5     6     7     8     9    10    11    12 
#>  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE 

# unique locations only
x <- data.frame(species = c(letters[1:10], rep("z", 2)),
                decimalLongitude = c(runif(n = 10, min = 4, max = 16), 75, 7),
                decimalLatitude = c(runif(n = 12, min = -5, max = 5)),
                min_ma = minages, 
                max_ma = c(minages[1:11] + runif(n = 11, min = 0, max = 5), 65))

cf_age(x, value = "flagged", taxon = "", uniq_loc = TRUE)
#> Testing temporal outliers on dataset level
#> Flagged 1 records.
#>     1     2     3     4     5     6     7     8     9    10    11    12 
#>  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE 
```
