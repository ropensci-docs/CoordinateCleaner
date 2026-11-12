# Geographic and Temporal Cleaning of Records from Fossil Collections

Cleaning records by multiple empirical tests to flag potentially
erroneous coordinates and time-spans, addressing issues common in fossil
collection databases. Individual tests can be activated via the tests
argument:

## Usage

``` r
clean_fossils(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  min_age = "min_ma",
  max_age = "max_ma",
  taxon = "accepted_name",
  tests = c("agesequal", "centroids", "equal", "gbif", "institutions", "spatiotemp",
    "temprange", "validity", "zeros"),
  countries = NULL,
  centroids_rad = 0.05,
  centroids_detail = "both",
  inst_rad = 0.001,
  outliers_method = "quantile",
  outliers_threshold = 5,
  outliers_size = 7,
  outliers_replicates = 5,
  zeros_rad = 0.5,
  centroids_ref = NULL,
  country_ref = NULL,
  inst_ref = NULL,
  value = "spatialvalid",
  verbose = TRUE,
  report = FALSE
)
```

## Arguments

- x:

  data.frame. Containing fossil records, containing taxon names, ages,
  and geographic coordinates..

- lon:

  character string. The column with the longitude coordinates. Default =
  “decimalLongitude”.

- lat:

  character string. The column with the latitude coordinates. Default =
  “decimalLatitude”.

- min_age:

  character string. The column with the minimum age. Default = “min_ma”.

- max_age:

  character string. The column with the maximum age. Default = “max_ma”.

- taxon:

  character string. The column with the taxon name. If “”, searches for
  outliers over the entire dataset, otherwise per specified taxon.
  Default = “accepted_name”.

- tests:

  vector of character strings, indicating which tests to run. See
  details for all tests available. Default = c("centroids", "equal",
  "gbif", "institutions", "temprange", "spatiotemp", "agesequal",
  "zeros")

- countries:

  a character string. The column with the country assignment of each
  record in three letter ISO code. Default = “countrycode”. If missing,
  the countries test is skipped.

- centroids_rad:

  numeric. The radius around centroid coordinates in meters. Default =
  1000.

- centroids_detail:

  a `character string`. If set to ‘country’ only country (adm-0)
  centroids are tested, if set to ‘provinces’ only province (adm-1)
  centroids are tested. Default = ‘both’.

- inst_rad:

  numeric. The radius around biodiversity institutions coordinates in
  metres. Default = 100.

- outliers_method:

  The method used for outlier testing. See details.

- outliers_threshold:

  numerical. The multiplier for the interquantile range for outlier
  detection. The higher the number, the more conservative the outlier
  tests. See
  [`cf_outl`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_outl.md)
  for details. Default = 3.

- outliers_size:

  numerical. The minimum number of records in a dataset to run the
  taxon-specific outlier test. Default = 7.

- outliers_replicates:

  numeric. The number of replications for the distance matrix
  calculation. See details. Default = 5.

- zeros_rad:

  numeric. The radius around 0/0 in degrees. Default = 0.5.

- centroids_ref:

  a `data.frame` with alternative reference data for the centroid test.
  If NULL, the `countryref` dataset is used. Alternatives must be
  identical in structure.

- country_ref:

  a `SpatVector` as alternative reference for the countries test. If
  NULL, the `rnaturalearth:ne_countries('medium', returnclass = "sf")`
  dataset is used.

- inst_ref:

  a `data.frame` with alternative reference data for the biodiversity
  institution test. If NULL, the `institutions` dataset is used.
  Alternatives must be identical in structure.

- value:

  a character string defining the output value. See the value section
  for details. one of ‘spatialvalid’, ‘summary’, ‘clean’. Default =
  ‘`spatialvalid`’.

- verbose:

  logical. If TRUE reports the name of the test and the number of
  records flagged.

- report:

  logical or character. If TRUE a report file is written to the working
  directory, summarizing the cleaning results. If a character, the path
  to which the file should be written. Default = FALSE.

## Value

Depending on the output argument:

- “spatialvalid”:

  an object of class `spatialvalid` similar to x with one column added
  for each test. TRUE = clean coordinate entry, FALSE = potentially
  problematic coordinate entries. The .summary column is FALSE if any
  test flagged the respective coordinate.

- “flagged”:

  a logical vector with the same order as the input data summarizing the
  results of all test. TRUE = clean coordinate, FALSE = potentially
  problematic (= at least one test failed).

- “clean”:

  a `data.frame` similar to x with potentially problematic records
  removed

## Details

- agesequal tests for equal minimum and maximum age.

- centroids tests a radius around country centroids. The radius is
  `centroids_rad`.

- countries tests if coordinates are from the country indicated in the
  country column. *Switched off by default.*

- equal tests for equal absolute longitude and latitude.

- gbif tests a one-degree radius around the GBIF headquarters in
  Copenhagen, Denmark.

- institutions tests a radius around known biodiversity institutions
  from `instiutions`. The radius is `inst_rad`.

- spatiotemp test for records which are outlier in time and space. See
  below for details.

- temprange tests for records with unexpectedly large temporal ranges,
  using a quantile-based outlier test.

- validity checks if coordinates correspond to a lat/lon coordinate
  reference system. This test is always on, since all records need to
  pass for any other test to run.

- zeros tests for plain zeros, equal latitude and longitude and a radius
  around the point 0/0. The radius is `zeros_rad`. The outlier detection
  in ‘spatiotemp’ is based on an interquantile range test. In a first
  step a distance matrix of geographic distances among all records is
  calculate. Subsequently a similar distance matrix of temporal
  distances among all records is calculated based on a single point
  selected by random between the minimum and maximum age for each
  record. The mean distance for each point to all neighbours is
  calculated for both matrices and spatial and temporal distances are
  scaled to the same range. The sum of these distanced is then tested
  against the interquantile range and flagged as an outlier if \\x \>
  IQR(x) + q_75 \* mltpl\\. The test is replicated ‘replicates’ times,
  to account for temporal uncertainty. Records are flagged as outliers
  if they are flagged by a fraction of more than ‘flag_thresh’
  replicates. Only datasets/taxa comprising more than ‘size.thresh’
  records are tested. Note that geographic distances are calculated as
  geospheric distances for datasets (or taxa) with fewer than 10,000
  records and approximated as Euclidean distances for datasets/taxa with
  10,000 to 25,000 records. Datasets/taxa comprising more than 25,000
  records are skipped.

## Note

Always tests for coordinate validity: non-numeric or missing coordinates
and coordinates exceeding the global extent (lon/lat, WGS84).

See <https://ropensci.github.io/CoordinateCleaner/> for more details and
tutorials.

## See also

Other Wrapper functions:
[`clean_coordinates()`](https://docs.ropensci.org/CoordinateCleaner/reference/clean_coordinates.md),
[`clean_dataset()`](https://docs.ropensci.org/CoordinateCleaner/reference/clean_dataset.md)

## Examples

``` r

minages <- runif(250, 0, 65)
exmpl <- data.frame(accepted_name = sample(letters, size = 250, replace = TRUE),
                    decimalLongitude = runif(250, min = 42, max = 51),
                    decimalLatitude = runif(250, min = -26, max = -11),
                    min_ma = minages,
                    max_ma = minages + runif(250, 0.1, 65))

test <- clean_fossils(x = exmpl)
#> Testing coordinate validity
#> Flagged 0 records.
#> Testing equal lat/lon
#> Flagged 0 records.
#> Testing zero coordinates
#> Warning: [vect] guessed crs
#> Flagged 0 records.
#> Testing country centroids
#> Flagged 0 records.
#> Testing spatio-temporal outliers on taxon level
#> Flagged 0 records.
#> Testing temporal range outliers on dataset level
#> Flagged 0 records.
#> Testing temporal range outliers on taxon level
#> Flagged 0 records.
#> Testing age validity
#> Flagged 0 records.
#> Testing GBIF headquarters, flagging records around Copenhagen
#> Flagged 0 records.
#> Testing biodiversity institutions
#> Flagged 0 records.
#> Flagged 0 of 250 records, EQ = 0

summary(test)
#>     .aeq     .cen     .equ     .gbf    .inst     .spt     .trg     .zer 
#>        0        0        0        0        0        0        0        0 
#> .summary 
#>        0 
```
