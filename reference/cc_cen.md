# Identify Coordinates in Vicinity of Country and Province Centroids

Removes or flags records within a radius around the geographic centroids
of political countries and provinces. Poorly geo-referenced occurrence
records in biological databases are often erroneously geo-referenced to
centroids.

## Usage

``` r
cc_cen(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  species = "species",
  buffer = 1000,
  geod = TRUE,
  test = "both",
  ref = NULL,
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

  numerical. The buffer around each province or country centroid, where
  records should be flagged as problematic. Units depend on geod.
  Default = 1 kilometre.

- geod:

  logical. If TRUE the radius around each capital is calculated based on
  a sphere, buffer is in meters and independent of latitude. If FALSE
  the radius is calculated assuming planar coordinates and varies
  slightly with latitude. Default = TRUE. See
  https://seethedatablog.wordpress.com/ for detail and credits.

- test:

  a character string. Specifying the details of the test. One of
  c(“both”, “country”, “provinces”). If both tests for country and
  province centroids.

- ref:

  SpatVector (geometry: polygons). Providing the geographic gazetteer.
  Can be any SpatVector (geometry: polygons), but the structure must be
  identical to
  [`countryref`](https://docs.ropensci.org/CoordinateCleaner/reference/countryref.md).
  Default =
  [`countryref`](https://docs.ropensci.org/CoordinateCleaner/reference/countryref.md).

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

## Note

See <https://ropensci.github.io/CoordinateCleaner/> for more details and
tutorials.

## See also

Other Coordinates:
[`cc_aohi()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_aohi.md),
[`cc_cap()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cap.md),
[`cc_coun()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_coun.md),
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

x <- data.frame(species = letters[1:10], 
                decimalLongitude = c(runif(99, -180, 180), -47.92), 
                decimalLatitude = c(runif(99, -90,90), -15.78))
cc_cen(x, geod = FALSE)
#> Testing country centroids
#> Removed 0 records.
#>     species decimalLongitude decimalLatitude
#> 1         a       -41.986559      -67.231328
#> 2         b       179.874885       78.719439
#> 3         c       -54.252343      -51.051559
#> 4         d       161.034576       29.896562
#> 5         e      -102.204009      -53.297878
#> 6         f      -168.446625        8.746459
#> 7         g      -127.686296       58.950255
#> 8         h       127.578200      -67.039623
#> 9         i      -103.266248      -42.986499
#> 10        j      -104.288134      -38.084935
#> 11        a      -165.772552      -87.341577
#> 12        b       160.118929       63.670271
#> 13        c       -91.825925      -15.471391
#> 14        d       101.204125       45.182492
#> 15        e       -76.234619       39.422512
#> 16        f       135.128848        2.763954
#> 17        g       -73.529966       85.470526
#> 18        h       174.069147      -25.060039
#> 19        i        32.341520      -59.328809
#> 20        j        93.297017       50.965321
#> 21        a       120.987110      -84.433699
#> 22        b        94.615009       52.095665
#> 23        c       -29.782827      -60.011338
#> 24        d      -130.293057      -84.831738
#> 25        e      -150.895814       51.471210
#> 26        f        56.153745       58.555415
#> 27        g        36.721389       83.743275
#> 28        h        56.518499      -21.844588
#> 29        i       -61.445821      -58.610461
#> 30        j       172.610719       18.675072
#> 31        a        77.467008       54.983472
#> 32        b       134.146910      -83.397706
#> 33        c       173.982149       41.923920
#> 34        d      -101.317323      -51.250922
#> 35        e        59.230823      -87.119434
#> 36        f       -39.756946      -66.851536
#> 37        g      -163.417090       33.491359
#> 38        h        42.089241       25.549428
#> 39        i        35.450998      -31.079066
#> 40        j       -33.532693      -20.253243
#> 41        a       128.998134       37.350071
#> 42        b         6.365227       35.577977
#> 43        c       172.545628       76.618218
#> 44        d      -173.874352       -7.376810
#> 45        e        62.441217       17.271756
#> 46        f       -46.342842      -60.298022
#> 47        g       150.483829       12.043761
#> 48        h        64.072114       71.653927
#> 49        i        59.454886       17.005011
#> 50        j        92.174792       59.704185
#> 51        a        15.421376       16.813506
#> 52        b       -93.856285       50.214714
#> 53        c         3.201686      -18.401104
#> 54        d       -29.784826       62.978899
#> 55        e        81.701587       43.532205
#> 56        f        49.566799      -32.797761
#> 57        g       -37.292416      -69.897560
#> 58        h       165.413739      -71.802834
#> 59        i       -72.483109       54.017595
#> 60        j      -161.927580      -21.609586
#> 61        a        27.427471      -80.513825
#> 62        b      -101.553909       87.571566
#> 63        c      -134.691743       18.751314
#> 64        d       157.734967      -63.219110
#> 65        e       108.459046        6.976831
#> 66        f        92.899303      -67.357939
#> 67        g        11.723459       83.372716
#> 68        h        16.849719      -81.551697
#> 69        i      -145.466459      -60.946347
#> 70        j       -40.194089       78.567737
#> 71        a      -117.953320       84.722645
#> 72        b        68.661305       37.849329
#> 73        c        63.075061       69.316367
#> 74        d       160.666145       85.837557
#> 75        e      -109.360972      -83.718064
#> 76        f       168.709501      -11.023692
#> 77        g       -40.645340       26.004557
#> 78        h        54.123804       89.069833
#> 79        i       113.254633      -33.360728
#> 80        j      -154.452682       64.058754
#> 81        a         9.658917        7.260783
#> 82        b        94.850937       67.215123
#> 83        c       -23.260809        2.805412
#> 84        d        18.890041       66.502674
#> 85        e      -106.548967       64.174067
#> 86        f      -168.830634      -27.777027
#> 87        g       169.094541      -89.981336
#> 88        h      -115.699286      -53.067646
#> 89        i       100.185403       80.139688
#> 90        j       138.855887      -39.359968
#> 91        a       121.120650       68.568159
#> 92        b        37.932639       83.000930
#> 93        c       146.476605      -89.276678
#> 94        d      -167.072469        9.522037
#> 95        e      -132.689336      -50.537525
#> 96        f      -146.149068       26.410886
#> 97        g        70.770117       40.986368
#> 98        h       -33.937660       67.065740
#> 99        i      -156.370810      -21.243322
#> 100       j       -47.920000      -15.780000

if (FALSE) { # \dontrun{
cc_inst(x, value = "flagged", buffer = 50000) #geod = T
} # }
```
