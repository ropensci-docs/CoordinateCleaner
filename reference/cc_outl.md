# Identify Geographic Outliers in Species Distributions

Removes out or flags records that are outliers in geographic space
according to the method defined via the `method` argument. Geographic
outliers often represent erroneous coordinates, for example due to data
entry errors, imprecise geo-references, individuals in
horticulture/captivity.

## Usage

``` r
cc_outl(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  species = "species",
  method = "quantile",
  mltpl = 5,
  tdi = 1000,
  value = "clean",
  sampling_thresh = 0,
  verbose = TRUE,
  min_occs = 7,
  thinning = FALSE,
  thinning_res = 0.5
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

  character string. The column with the species name. Default =
  “species”.

- method:

  character string. Defining the method for outlier selection. See
  details. One of “distance”, “quantile”, “mad”. Default = “quantile”.

- mltpl:

  numeric. The multiplier of the interquartile range
  (`method == 'quantile'`) or median absolute deviation
  (`method == 'mad'`)to identify outliers. See details. Default = 5.

- tdi:

  numeric. The minimum absolute distance (`method == 'distance'`) of a
  record to all other records of a species to be identified as outlier,
  in km. See details. Default = 1000.

- value:

  character string. Defining the output value. See value.

- sampling_thresh:

  numeric. Cut off threshold for the sampling correction. Indicates the
  quantile of sampling in which outliers should be ignored. For
  instance, if `sampling_thresh` == 0.25, records in the 25 (no sampling
  correction).

- verbose:

  logical. If TRUE reports the name of the test and the number of
  records flagged.

- min_occs:

  Minimum number of geographically unique datapoints needed for a
  species to be tested. This is necessary for reliable outlier
  estimation. Species with fewer than min_occs records will not be
  tested and the output value will be 'TRUE'. Default is to 7. If
  `method == 'distance'`, consider a lower threshold.

- thinning:

  forces a raster approximation for the distance calculation. This is
  routinely used for species with more than 10,000 records for
  computational reasons, but can be enforced for smaller datasets, which
  is recommended when sampling is very uneven.

- thinning_res:

  The resolution for the spatial thinning in decimal degrees. Default =
  0.5.

## Value

Depending on the ‘value’ argument, either a `data.frame` containing the
records considered correct by the test (“clean”) or a logical vector
(“flagged”), with TRUE = test passed and FALSE = test failed/potentially
problematic . Default = “clean”.

## Details

The method for outlier identification depends on the `method` argument.
If “quantile”: a boxplot method is used and records are flagged as
outliers if their *mean* distance to all other records of the same
species is larger than mltpl \* the interquartile range of the mean
distance of all records of this species. If “mad”: the median absolute
deviation is used. In this case a record is flagged as outlier, if the
*mean* distance to all other records of the same species is larger than
the median of the mean distance of all points plus/minus the mad of the
mean distances of all records of the species \* mltpl. If “distance”:
records are flagged as outliers, if the *minimum* distance to the next
record of the species is \> `tdi`. For species with records from \>
10000 unique locations a random sample of 1000 records is used for the
distance matrix calculation. The test skips species with fewer than
`min_occs`, geographically unique records.

The likelihood of occurrence records being erroneous outliers is linked
to the sampling effort in any given location. To account for this, the
sampling_cor option fetches the number of occurrence records available
from www.gbif.org, per country as a proxy of sampling effort. The
outlier test (the mean distance) for each records is than weighted by
the log transformed number of records per square kilometre in this
country. See for
<https://besjournals.onlinelibrary.wiley.com/doi/full/10.1111/2041-210X.13152>
an example and further explanation of the outlier test.

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
[`cc_sea()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_sea.md),
[`cc_urb()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_urb.md),
[`cc_val()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_val.md),
[`cc_zero()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_zero.md)

## Examples

``` r

x <- data.frame(species = letters[1:10],
                decimalLongitude = runif(100, -180, 180),
                decimalLatitude = runif(100, -90,90))

cc_outl(x)
#> Testing geographic outliers
#> Removed 0 records.
#>     species decimalLongitude decimalLatitude
#> 1         a       141.907373      41.7730578
#> 2         b      -100.689707     -16.3607806
#> 3         c       -75.025282      23.5544050
#> 4         d       112.643550     -78.2199030
#> 5         e      -164.303161     -63.2354127
#> 6         f         1.897799     -39.6536823
#> 7         g       -93.003883      63.1182257
#> 8         h        32.091271     -86.1226595
#> 9         i       169.610605     -44.4906216
#> 10        j       -96.220439      28.2080702
#> 11        a       132.270486      52.0468937
#> 12        b      -140.125773      37.5922166
#> 13        c       108.537041     -78.3732614
#> 14        d        56.852804       2.0360419
#> 15        e        54.739547     -53.8328970
#> 16        f       -98.523848      56.5408067
#> 17        g       127.432667      27.6804137
#> 18        h       -23.181466     -26.4146432
#> 19        i       -75.858575      17.3870813
#> 20        j        49.530481      40.9867872
#> 21        a       -84.765795      89.2023371
#> 22        b       -40.943384      -0.3500511
#> 23        c        42.909829     -48.5469415
#> 24        d        15.004110     -35.7742237
#> 25        e        -5.460840      39.6840018
#> 26        f        95.146171     -42.9470247
#> 27        g       165.969697     -70.6146573
#> 28        h       -82.589568     -42.7639118
#> 29        i        83.016080     -56.3928870
#> 30        j      -131.700403       7.6395023
#> 31        a      -162.576638     -78.2066746
#> 32        b        73.298545      68.4693246
#> 33        c      -127.003034     -39.7417433
#> 34        d        96.895920      31.0866609
#> 35        e      -116.513960     -79.1764427
#> 36        f      -140.126760      87.9273990
#> 37        g       157.803267     -39.1904224
#> 38        h       124.729610       9.7402260
#> 39        i        25.611689      70.2151208
#> 40        j        64.473626     -85.8956301
#> 41        a      -147.843992      31.9839217
#> 42        b       102.734685     -29.3292366
#> 43        c       -98.295319     -64.5847030
#> 44        d       -18.655753     -26.5384822
#> 45        e      -121.958051     -17.7399258
#> 46        f      -116.599792      70.0045834
#> 47        g      -108.637876     -33.7294462
#> 48        h       -51.259927     -78.9021525
#> 49        i      -114.720047      32.3075275
#> 50        j        22.012150     -21.9307376
#> 51        a        57.269883      15.9066627
#> 52        b        57.691227      -1.4438637
#> 53        c      -179.135716     -38.5601501
#> 54        d       177.640572     -88.4844729
#> 55        e        45.896123       9.1393735
#> 56        f      -174.733079      56.3815634
#> 57        g      -106.135842      25.8434254
#> 58        h        58.707599     -77.5688395
#> 59        i       -13.052149      41.1689289
#> 60        j       -50.274663      30.9800157
#> 61        a        68.171901      57.5641784
#> 62        b       -86.810116      74.6318804
#> 63        c       126.433812      34.1947225
#> 64        d        -7.198311      -1.4439955
#> 65        e        13.415900      26.4471571
#> 66        f        58.190363      -7.8273078
#> 67        g       -71.692376     -67.4196081
#> 68        h       -85.139454      18.7856953
#> 69        i       -70.183393      58.8288125
#> 70        j       134.984976      21.4880852
#> 71        a        78.637398      61.3896679
#> 72        b       -33.508910     -13.0956106
#> 73        c      -173.030590      48.4716067
#> 74        d      -161.935071      38.1853424
#> 75        e        72.810372     -78.5939025
#> 76        f      -156.106703     -13.3190575
#> 77        g      -166.992347     -74.1100242
#> 78        h      -172.744670     -52.5029725
#> 79        i       -45.798805     -48.9130428
#> 80        j       -98.760455     -80.9204141
#> 81        a        -2.909484      11.1278254
#> 82        b       -19.207744      34.9643060
#> 83        c        68.473764     -48.9703222
#> 84        d       123.386393      82.8564489
#> 85        e       -44.699880      -1.2893056
#> 86        f       174.560288      40.1775634
#> 87        g       162.873479      -1.5546451
#> 88        h       176.363463      62.1639146
#> 89        i       -20.473792     -85.4802606
#> 90        j      -124.645013       8.7120437
#> 91        a       160.101836      81.9818670
#> 92        b         7.007909     -64.7096640
#> 93        c       -15.971542      37.8492525
#> 94        d      -100.767528     -67.2476600
#> 95        e      -129.894342      60.4181300
#> 96        f      -102.183163     -36.3276893
#> 97        g       -33.955678     -21.3608250
#> 98        h       -95.855547     -45.3871196
#> 99        i      -170.976111     -27.9033031
#> 100       j        90.843105     -14.4389833
cc_outl(x, method = "quantile", value = "flagged")
#> Testing geographic outliers
#> Flagged 0 records.
#>   [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [16] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [31] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [46] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [61] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [76] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [91] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
cc_outl(x, method = "distance", value = "flagged", tdi = 10000)
#> Testing geographic outliers
#> Flagged 1 records.
#>   [1]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [13]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [25]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [37]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [49]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [61]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [73]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [85]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
#>  [97]  TRUE  TRUE  TRUE  TRUE
cc_outl(x, method = "distance", value = "flagged", tdi = 1000)
#> Testing geographic outliers
#> Flagged 92 records.
#>   [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [13] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE
#>  [25] FALSE FALSE  TRUE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [37] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [49] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [61]  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE
#>  [73] FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
#>  [85] FALSE FALSE FALSE FALSE FALSE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE
#>  [97] FALSE FALSE FALSE FALSE
```
