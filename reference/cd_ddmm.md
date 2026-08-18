# Identify Datasets with a Degree Conversion Error

This test flags datasets where a significant fraction of records has
been subject to a common degree minute to decimal degree conversion
error, where the degree sign is recognized as decimal delimiter.

## Usage

``` r
cd_ddmm(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  ds = "dataset",
  pvalue = 0.025,
  diff = 1,
  mat_size = 1000,
  min_span = 2,
  value = "clean",
  verbose = TRUE,
  diagnostic = FALSE
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

- ds:

  a character string. The column with the dataset of each record. In
  case `x` should be treated as a single dataset, identical for all
  records. Default = “dataset”.

- pvalue:

  numeric. The p-value for the one-sided t-test to flag the test as
  passed or not. Both ddmm.pvalue and diff must be met. Default = 0.025.

- diff:

  numeric. The threshold difference for the ddmm test. Indicates by
  which fraction the records with decimals below 0.6 must outnumber the
  records with decimals above 0.6. Default = 1

- mat_size:

  numeric. The size of the matrix for the binomial test. Must be changed
  in decimals (e.g. 100, 1000, 10000). Adapt to dataset size, generally
  100 is better for datasets \< 10000 records, 1000 is better for
  datasets with 10000 - 1M records. Higher values also work reasonably
  well for smaller datasets, therefore, default = 1000. For large
  datasets try 10000.

- min_span:

  numeric. The minimum geographic extent of datasets to be tested.
  Default = 2.

- value:

  character string. Defining the output value. See value.

- verbose:

  logical. If TRUE reports the name of the test and the number of
  records flagged.

- diagnostic:

  logical. If TRUE plots the analyses matrix for each dataset.

## Value

Depending on the ‘value’ argument, either a `data.frame` with summary
statistics and flags for each dataset (“dataset”) or a `data.frame`
containing the records considered correct by the test (“clean”) or a
logical vector (“flags”), with TRUE = test passed and FALSE = test
failed/potentially problematic. Default = “clean”.

## Details

If the degree sign is recognized as decimal delimiter during coordinate
conversion, no coordinate decimals above 0.59 (59') are possible. The
test here uses a binomial test to test if a significant proportion of
records in a dataset have been subject to this problem. The test is best
adjusted via the diff argument. The lower `diff`, the stricter the test.
Also scales with dataset size. Empirically, for datasets with \< 5,000
unique coordinate records `diff = 0.1` has proven reasonable flagging
most datasets with \>25% problematic records and all dataset with \>50%
problematic records. For datasets between 5,000 and 100,000 geographic
unique records `diff = 0.01` is recommended, for datasets between
100,000 and 1 M records diff = 0.001, and so on.

## Note

See <https://ropensci.github.io/CoordinateCleaner/> for more details and
tutorials.

## See also

Other Datasets:
[`cd_round()`](https://docs.ropensci.org/CoordinateCleaner/reference/cd_round.md)

## Examples

``` r

clean <- data.frame(species = letters[1:10], 
                decimalLongitude = runif(100, -180, 180), 
                decimalLatitude = runif(100, -90,90),
                dataset = "FR")
                
cd_ddmm(x = clean, value = "flagged")
#> Testing for dd.mm to dd.dd conversion errors
#> Flagged 0 records
#>   [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [16] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [31] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [46] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [61] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [76] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [91] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE

#problematic dataset
lon <- sample(0:180, size = 100, replace = TRUE) + runif(100, 0,0.59)
lat <- sample(0:90, size = 100, replace = TRUE) + runif(100, 0,0.59)

prob <-  data.frame(species = letters[1:10], 
                decimalLongitude = lon, 
                decimalLatitude = lat,
                dataset = "FR")
                
cd_ddmm(x = prob, value = "flagged")
#> Testing for dd.mm to dd.dd conversion errors
#> Flagged 100 records
#>   [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [13] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [25] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [37] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [49] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [61] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [73] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [85] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [97] FALSE FALSE FALSE FALSE
```
