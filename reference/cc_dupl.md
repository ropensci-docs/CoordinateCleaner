# Identify Duplicated Records

Removes or flags duplicated records based on species name and
coordinates, as well as user-defined additional columns. True (specimen)
duplicates or duplicates from the same species can make up the bulk of
records in a biological collection database, but are undesirable for
many analyses. Both can be flagged with this function, the former given
enough additional information.

## Usage

``` r
cc_dupl(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  species = "species",
  additions = NULL,
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

  a character string. The column with the species name. Default =
  “species”.

- additions:

  a vector of character strings. Additional columns to be included in
  the test for duplication. For example as below, collector name and
  collector number.

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

## See also

Other Coordinates:
[`cc_aohi()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_aohi.md),
[`cc_cap()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cap.md),
[`cc_cen()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cen.md),
[`cc_coun()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_coun.md),
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

x <- data.frame(species = letters[1:10], 
                decimalLongitude = sample(x = 0:10, size = 100, replace = TRUE), 
                decimalLatitude = sample(x = 0:10, size = 100, replace = TRUE),
                collector = "Bonpl",
                collector.number = c(1001, 354),
                collection = rep(c("K", "WAG","FR", "P", "S"), 20))

cc_dupl(x, value = "flagged")
#> Testing duplicates
#> Flagged 3 records.
#>   [1]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [13]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [25]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [37]  TRUE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [49]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [61]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [73]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [85] FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [97]  TRUE FALSE  TRUE  TRUE
cc_dupl(x, additions = c("collector", "collector.number"))
#> Testing duplicates
#> Removed 3 records.
#>     species decimalLongitude decimalLatitude collector collector.number
#> 1         a               10               4     Bonpl             1001
#> 2         b                7               4     Bonpl              354
#> 3         c                1               0     Bonpl             1001
#> 4         d                5               4     Bonpl              354
#> 5         e                5               8     Bonpl             1001
#> 6         f                0               9     Bonpl              354
#> 7         g                4               3     Bonpl             1001
#> 8         h                4              10     Bonpl              354
#> 9         i                5               5     Bonpl             1001
#> 10        j                8               9     Bonpl              354
#> 11        a                0               2     Bonpl             1001
#> 12        b                0               2     Bonpl              354
#> 13        c                8               0     Bonpl             1001
#> 14        d                3               7     Bonpl              354
#> 15        e                1               4     Bonpl             1001
#> 16        f                7               8     Bonpl              354
#> 17        g                6               3     Bonpl             1001
#> 18        h                6               2     Bonpl              354
#> 19        i               10               8     Bonpl             1001
#> 20        j                9               9     Bonpl              354
#> 21        a                6               8     Bonpl             1001
#> 22        b                5               0     Bonpl              354
#> 23        c                4               7     Bonpl             1001
#> 24        d                9               5     Bonpl              354
#> 25        e                8               9     Bonpl             1001
#> 26        f                5               5     Bonpl              354
#> 27        g                7               7     Bonpl             1001
#> 28        h                2               2     Bonpl              354
#> 29        i                9               4     Bonpl             1001
#> 30        j                9               8     Bonpl              354
#> 31        a               10               5     Bonpl             1001
#> 32        b                1               9     Bonpl              354
#> 33        c                9               0     Bonpl             1001
#> 34        d                6               8     Bonpl              354
#> 35        e                6               3     Bonpl             1001
#> 36        f                3               5     Bonpl              354
#> 37        g                6               5     Bonpl             1001
#> 38        h                9               0     Bonpl              354
#> 39        i                1              10     Bonpl             1001
#> 40        j                8               2     Bonpl              354
#> 41        a                9              10     Bonpl             1001
#> 43        c                9               8     Bonpl             1001
#> 44        d                1              10     Bonpl              354
#> 45        e                1               0     Bonpl             1001
#> 46        f                8               9     Bonpl              354
#> 47        g                0               1     Bonpl             1001
#> 48        h                0               7     Bonpl              354
#> 49        i                0               9     Bonpl             1001
#> 50        j                0               9     Bonpl              354
#> 51        a                8               0     Bonpl             1001
#> 52        b                1               3     Bonpl              354
#> 53        c                2               8     Bonpl             1001
#> 54        d                8              10     Bonpl              354
#> 55        e                4               0     Bonpl             1001
#> 56        f                7               1     Bonpl              354
#> 57        g                5               2     Bonpl             1001
#> 58        h               10               1     Bonpl              354
#> 59        i                0               0     Bonpl             1001
#> 60        j                3               7     Bonpl              354
#> 61        a                9               8     Bonpl             1001
#> 62        b                2               6     Bonpl              354
#> 63        c                1               4     Bonpl             1001
#> 64        d                6               1     Bonpl              354
#> 65        e                9               6     Bonpl             1001
#> 66        f                9               9     Bonpl              354
#> 67        g                1               6     Bonpl             1001
#> 68        h                4               9     Bonpl              354
#> 69        i                5               9     Bonpl             1001
#> 70        j                9               1     Bonpl              354
#> 71        a                6              10     Bonpl             1001
#> 72        b                5               3     Bonpl              354
#> 73        c                2               9     Bonpl             1001
#> 74        d                3               1     Bonpl              354
#> 75        e                2               7     Bonpl             1001
#> 76        f                5              10     Bonpl              354
#> 77        g                3               7     Bonpl             1001
#> 78        h                9               3     Bonpl              354
#> 79        i                6               7     Bonpl             1001
#> 80        j               10               8     Bonpl              354
#> 81        a                8               1     Bonpl             1001
#> 82        b                8               3     Bonpl              354
#> 83        c                0               4     Bonpl             1001
#> 84        d               10               7     Bonpl              354
#> 86        f                2               7     Bonpl              354
#> 87        g                9               3     Bonpl             1001
#> 88        h                0               5     Bonpl              354
#> 89        i               10               3     Bonpl             1001
#> 90        j                9               7     Bonpl              354
#> 91        a                4               5     Bonpl             1001
#> 92        b                4               5     Bonpl              354
#> 93        c               10              10     Bonpl             1001
#> 94        d                6              10     Bonpl              354
#> 95        e                6               8     Bonpl             1001
#> 96        f                7               6     Bonpl              354
#> 97        g                4               6     Bonpl             1001
#> 99        i                5               3     Bonpl             1001
#> 100       j                1               3     Bonpl              354
#>     collection
#> 1            K
#> 2          WAG
#> 3           FR
#> 4            P
#> 5            S
#> 6            K
#> 7          WAG
#> 8           FR
#> 9            P
#> 10           S
#> 11           K
#> 12         WAG
#> 13          FR
#> 14           P
#> 15           S
#> 16           K
#> 17         WAG
#> 18          FR
#> 19           P
#> 20           S
#> 21           K
#> 22         WAG
#> 23          FR
#> 24           P
#> 25           S
#> 26           K
#> 27         WAG
#> 28          FR
#> 29           P
#> 30           S
#> 31           K
#> 32         WAG
#> 33          FR
#> 34           P
#> 35           S
#> 36           K
#> 37         WAG
#> 38          FR
#> 39           P
#> 40           S
#> 41           K
#> 43          FR
#> 44           P
#> 45           S
#> 46           K
#> 47         WAG
#> 48          FR
#> 49           P
#> 50           S
#> 51           K
#> 52         WAG
#> 53          FR
#> 54           P
#> 55           S
#> 56           K
#> 57         WAG
#> 58          FR
#> 59           P
#> 60           S
#> 61           K
#> 62         WAG
#> 63          FR
#> 64           P
#> 65           S
#> 66           K
#> 67         WAG
#> 68          FR
#> 69           P
#> 70           S
#> 71           K
#> 72         WAG
#> 73          FR
#> 74           P
#> 75           S
#> 76           K
#> 77         WAG
#> 78          FR
#> 79           P
#> 80           S
#> 81           K
#> 82         WAG
#> 83          FR
#> 84           P
#> 86           K
#> 87         WAG
#> 88          FR
#> 89           P
#> 90           S
#> 91           K
#> 92         WAG
#> 93          FR
#> 94           P
#> 95           S
#> 96           K
#> 97         WAG
#> 99           P
#> 100          S
```
