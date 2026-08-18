# Identify Invalid lat/lon Coordinates

Removes or flags non-numeric and not available coordinates as well as
lat \>90, lat \<-90, lon \> 180 and lon \< -180 are flagged.

## Usage

``` r
cc_val(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
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

This test is obligatory before running any further tests of
CoordinateCleaner, as additional tests only run with valid coordinates.

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
[`cc_zero()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_zero.md)

## Examples

``` r

x <- data.frame(species = letters[1:10], 
                decimalLongitude = c(runif(106, -180, 180), NA, "13W33'", "67,09", 305), 
                decimalLatitude = runif(110, -90,90))
                
cc_val(x)
#> Testing coordinate validity
#> Removed 4 records.
#>     species decimalLongitude decimalLatitude
#> 1         a      -109.950343      77.1032266
#> 2         b      -103.494926     -15.5801781
#> 3         c       -90.421157     -66.4254207
#> 4         d        89.281369     -78.9906592
#> 5         e       -33.392246      69.4932971
#> 6         f        22.202571      16.5975311
#> 7         g        90.183510      40.0341934
#> 8         h        18.431196       2.2307398
#> 9         i        -9.594151     -36.7076425
#> 10        j      -157.453247      25.1600522
#> 11        a      -146.179423      68.8578171
#> 12        b        28.865743      80.3071315
#> 13        c       -41.156686      37.6950017
#> 14        d       -94.852476      76.2517037
#> 15        e         7.033548     -87.0801135
#> 16        f        50.835575     -59.1339080
#> 17        g       118.764086     -51.4818424
#> 18        h       150.873894      62.1207341
#> 19        i       154.584055     -46.2020919
#> 20        j        52.312948       2.7090638
#> 21        a       101.922340      65.3737483
#> 22        b      -158.862254     -82.6994383
#> 23        c       -50.989852      -7.1949194
#> 24        d        -7.421025     -60.1191273
#> 25        e        80.313036      74.7691811
#> 26        f        70.704972     -16.3037118
#> 27        g       133.455800      25.2957921
#> 28        h        75.170108     -60.3326858
#> 29        i       -52.270031     -57.4235737
#> 30        j       110.610979      89.3614883
#> 31        a       -52.996008      26.1199695
#> 32        b        91.125391      75.2577151
#> 33        c       146.934390      53.1117802
#> 34        d       -23.261325      74.3574753
#> 35        e      -100.711030     -19.6992751
#> 36        f      -166.614406     -25.6679112
#> 37        g        71.299467      20.5850932
#> 38        h      -126.059755     -55.3606657
#> 39        i        15.277541     -77.0690705
#> 40        j       106.838051     -34.8780713
#> 41        a        79.064881     -58.5762579
#> 42        b        82.148276      69.7970023
#> 43        c      -162.786182      -0.3873195
#> 44        d       -17.504743      34.9034565
#> 45        e       105.937185      67.3588848
#> 46        f       116.864037     -86.7443806
#> 47        g      -104.741437     -21.2423301
#> 48        h      -119.445648       2.4467444
#> 49        i       172.898721     -81.4999213
#> 50        j        36.241203      54.5571198
#> 51        a      -125.396532      24.1356481
#> 52        b       -74.614076      48.8390785
#> 53        c       -18.698176       0.3174737
#> 54        d       116.606817      38.0319774
#> 55        e       168.985559     -73.4582906
#> 56        f       -73.652701     -70.9364206
#> 57        g       -96.231693     -49.5155013
#> 58        h       -62.826826     -14.8205604
#> 59        i      -143.866037     -28.0776478
#> 60        j       -61.118870      29.5570722
#> 61        a      -110.664467     -10.1945391
#> 62        b       -83.205491     -34.8869588
#> 63        c       138.070564     -41.5177188
#> 64        d        61.424338     -41.3232129
#> 65        e      -153.244416     -72.4158625
#> 66        f        66.711313      88.5707913
#> 67        g        36.293613     -71.1852230
#> 68        h        74.210571     -11.7920771
#> 69        i       -11.460236      33.2437627
#> 70        j      -176.044344     -30.6321259
#> 71        a       101.986450     -18.0823157
#> 72        b        43.867187      35.8481775
#> 73        c       131.781146     -46.3458405
#> 74        d       151.146841     -85.0436852
#> 75        e       -62.856387     -77.3837960
#> 76        f       149.674344     -85.7334102
#> 77        g      -151.003833     -52.8118012
#> 78        h       127.986365      35.7995378
#> 79        i       -70.189568      86.6490413
#> 80        j        85.090695      50.6972844
#> 81        a       158.746525      18.9854229
#> 82        b       129.968365     -52.4948922
#> 83        c       -48.898756      47.4621290
#> 84        d       148.508454      57.3814794
#> 85        e        91.051993      47.0002157
#> 86        f        50.933667      59.1528817
#> 87        g       106.362119     -88.4683279
#> 88        h       174.589821     -74.0121775
#> 89        i        44.052926      75.5784225
#> 90        j        19.917535      24.9113612
#> 91        a       165.983941     -14.9425643
#> 92        b       149.630298      36.3663946
#> 93        c        37.179216     -61.0152975
#> 94        d       -67.840522     -21.9762296
#> 95        e        19.854765      85.5210270
#> 96        f       157.535188     -17.3600679
#> 97        g       -48.586625      40.2766736
#> 98        h       -58.092373     -20.6658217
#> 99        i        97.086082     -19.9640788
#> 100       j       -71.516771     -66.6644820
#> 101       a        46.670744     -38.7448736
#> 102       b       108.899901     -66.4660992
#> 103       c        15.204803      -4.6711850
#> 104       d        47.159124      86.4016542
#> 105       e      -111.676007     -59.6987217
#> 106       f      -106.757032     -57.3231120
cc_val(x, value = "flagged")
#> Testing coordinate validity
#> Flagged 4 records.
#>   [1]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [13]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [25]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [37]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [49]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [61]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [73]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [85]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [97]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE FALSE
#> [109] FALSE FALSE
```
