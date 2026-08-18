# Identify Outlier Records in Space and Time

Removes or flags records of fossils that are spatio-temporal outliers
based on interquantile ranges. Records are flagged if they are either
extreme in time or space, or both.

## Usage

``` r
cf_outl(
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

The outlier detection is based on an interquantile range test. In a
first step a distance matrix of geographic distances among all records
is calculate. Subsequently a similar distance matrix of temporal
distances among all records is calculated based on a single point
selected by random between the minimum and maximum age for each record.
The mean distance for each point to all neighbours is calculated for
both matrices and spatial and temporal distances are scaled to the same
range. The sum of these distanced is then tested against the
interquantile range and flagged as an outlier if \\x \> IQR(x) + q_75 \*
mltpl\\. The test is replicated ‘replicates’ times, to account for
temporal uncertainty. Records are flagged as outliers if they are
flagged by a fraction of more than ‘flag.thres’ replicates. Only
datasets/taxa comprising more than ‘size_thresh’ records are tested.
Note that geographic distances are calculated as geospheric distances
for datasets (or taxa) with fewer than 10,000 records and approximated
as Euclidean distances for datasets/taxa with 10,000 to 25,000 records.
Datasets/taxa comprising more than 25,000 records are skipped.

## Note

See <https://ropensci.github.io/CoordinateCleaner/> for more details and
tutorials.

## See also

Other fossils:
[`cf_age()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_age.md),
[`cf_equal()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_equal.md),
[`cf_range()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_range.md),
[`write_pyrate()`](https://docs.ropensci.org/CoordinateCleaner/reference/write_pyrate.md)

## Examples

``` r

minages <- c(runif(n = 11, min = 10, max = 25), 62.5)
x <- data.frame(species = c(letters[1:10], rep("z", 2)),
                lng = c(runif(n = 10, min = 4, max = 16), 75, 7),
                lat = c(runif(n = 12, min = -5, max = 5)),
                min_ma = minages, 
                max_ma = c(minages[1:11] + runif(n = 11, min = 0, max = 5), 65))

cf_outl(x, value = "flagged", taxon = "")
#> Testing spatio-temporal outliers on dataset level
#> Warning: decimalLatitude not found. Using lat instead.
#> Warning: decimalLongitude not found. Using lat instead.
#> Flagged 2 records.
#>     1     2     3     4     5     6     7     8     9    10    11    12 
#>  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE FALSE 
```
