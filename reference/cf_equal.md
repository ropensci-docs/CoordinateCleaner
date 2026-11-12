# Identify Fossils with equal min and max age

Removes or flags records with equal minimum and maximum age.

## Usage

``` r
cf_equal(
  x,
  min_age = "min_ma",
  max_age = "max_ma",
  value = "clean",
  verbose = TRUE
)
```

## Arguments

- x:

  data.frame. Containing fossil records with taxon names, ages, and
  geographic coordinates.

- min_age:

  character string. The column with the minimum age. Default = “min_ma”.

- max_age:

  character string. The column with the maximum age. Default = “max_ma”.

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
[`cf_outl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_outl.md),
[`cf_range()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_range.md),
[`write_pyrate()`](https://docs.ropensci.org/CoordinateCleaner/reference/write_pyrate.md)

## Examples

``` r

minages <- runif(n = 10, min = 0.1, max = 25)
x <- data.frame(species = letters[1:10], 
                min_ma = minages, 
                max_ma = minages + runif(n = 10, min = 0, max = 10))
x <- rbind(x, data.frame(species = "z", 
                min_ma = 5, 
                max_ma = 5))
                
cf_equal(x, value = "flagged")
#> Testing age validity
#> Flagged 1 records.
#>  [1]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
```
