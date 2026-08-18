# Identify Coordinates in Artificial Hotspot Occurrence Inventory

Removes or flags records within Artificial Hotspot Occurrence Inventory.
Poorly geo-referenced occurrence records in biological databases are
often erroneously geo-referenced to highly recurring coordinates that
were assessed by Park et al 2022. See the reference for more details.

## Usage

``` r
cc_aohi(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  species = "species",
  taxa = c("Aves", "Insecta", "Mammalia", "Plantae"),
  buffer = 10000,
  geod = TRUE,
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

- taxa:

  Artificial Hotspot Occurrence Inventory (AHOI) were created based on
  four different taxa, birds, insecta, mammalia, and plantae. Users can
  choose to keep all, or any specific taxa subset to define the AHOI
  locations. Default is to keep all: c("Aves", "Insecta", "Mammalia",
  "Plantae").

- buffer:

  The buffer around each capital coordinate (the centre of the city),
  where records should be flagged as problematic. Units depend on geod.
  Default = 10 kilometres.

- geod:

  logical. If TRUE the radius around each capital is calculated based on
  a sphere, buffer is in meters and independent of latitude. If FALSE
  the radius is calculated assuming planar coordinates and varies
  slightly with latitude. Default = TRUE. See
  https://seethedatablog.wordpress.com/ for detail and credits.

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

## References

Park, D. S., Xie, Y., Thammavong, H. T., Tulaiha, R., & Feng, X. (2023).
Artificial Hotspot Occurrence Inventory (AHOI). Journal of Biogeography,
50, 441–449. [doi:10.1111/jbi.14543](https://doi.org/10.1111/jbi.14543)

## See also

Other Coordinates:
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
[`cc_val()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_val.md),
[`cc_zero()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_zero.md)

## Examples

``` r

x <- data.frame(species = letters[1:10], 
                decimalLongitude = c(runif(99, -180, 180), -47.92), 
                decimalLatitude = c(runif(99, -90,90), -15.78))
cc_aohi(x)
#> Testing Artificial Hotspot Occurrence Inventory
#> Removed 0 records.
#>     species decimalLongitude decimalLatitude
#> 1         a     -150.9299505     -5.51082603
#> 2         b      120.3598934     55.02240553
#> 3         c       36.2739190     56.52923581
#> 4         d     -123.4049611    -17.29601964
#> 5         e     -177.3362012    -50.68241843
#> 6         f      -12.0983410    -14.69494758
#> 7         g       -0.8001401     30.39673460
#> 8         h      -75.6837919      1.37705076
#> 9         i       83.8375153     28.86467517
#> 10        j       98.1077440      2.12243648
#> 11        a      134.8562379     60.39943866
#> 12        b     -117.0213743     37.58060904
#> 13        c     -167.6731202     67.35706936
#> 14        d      -64.6611369    -87.93368317
#> 15        e      -35.1618342     69.88492223
#> 16        f     -109.5588595     89.34244540
#> 17        g      -34.7262777      0.03447016
#> 18        h     -157.0818754    -25.38593561
#> 19        i      -40.0675273     49.48434401
#> 20        j      171.1972207     15.20554516
#> 21        a      -75.6387737     24.11574678
#> 22        b       64.2169539     64.55990786
#> 23        c       84.7150556     12.04098097
#> 24        d     -109.4555761    -44.46053652
#> 25        e      172.9942829     75.38457862
#> 26        f       86.9477505     66.12303687
#> 27        g     -161.4793405    -45.26303456
#> 28        h       10.8764869    -17.48138187
#> 29        i       70.4965964     48.53343168
#> 30        j       67.8801612    -68.49263258
#> 31        a     -168.7570829    -54.95490690
#> 32        b      -98.7974876    -60.37753532
#> 33        c      -71.7009098     29.37718463
#> 34        d       49.1276214     64.18350081
#> 35        e       -7.5511621     76.77836071
#> 36        f      -24.4183471      9.42796703
#> 37        g       74.3161816     13.87182499
#> 38        h      161.4875675     33.74059421
#> 39        i     -115.0780435    -45.95071867
#> 40        j     -101.9160445    -81.96891150
#> 41        a       64.8586503     73.77382020
#> 42        b       -0.4155802    -77.27738054
#> 43        c       51.0045654     89.44046522
#> 44        d       57.7023657     20.13343514
#> 45        e     -145.4313031    -58.93940777
#> 46        f       95.6160590     73.69937373
#> 47        g       97.0829295    -83.25878996
#> 48        h      176.6564324     16.83968226
#> 49        i      169.3875250    -47.34404013
#> 50        j      -39.8942062     73.13350801
#> 51        a      -13.9728727     57.39713713
#> 52        b      -66.5129691     35.96928430
#> 53        c     -117.1166782    -50.39994074
#> 54        d       11.3664747     41.03836890
#> 55        e       -2.2906742    -50.92479679
#> 56        f      100.5511053     -7.87856431
#> 57        g     -106.4957966    -30.09604351
#> 58        h       76.8230204     12.30348034
#> 59        i     -156.5221998    -44.60296953
#> 60        j      -52.4855524     -6.47755799
#> 61        a      117.0717916     75.17889124
#> 62        b      -81.4254317     85.11195922
#> 63        c       25.2161824     57.43484039
#> 64        d      -59.1411310     72.52628367
#> 65        e       34.6546040     14.64588792
#> 66        f     -111.0535087     49.14152660
#> 67        g      161.1950176     89.12214476
#> 68        h       15.2929471     37.97482495
#> 69        i       16.0572216    -51.31033278
#> 70        j      -79.7050246    -37.48362661
#> 71        a      -19.1871111     39.91675128
#> 72        b      -46.2559736     65.99082660
#> 73        c     -169.8980492    -47.07844083
#> 74        d      -12.2446113    -89.19066455
#> 75        e      -39.5887006     79.83296356
#> 76        f     -172.7765216    -11.13530392
#> 77        g      -44.2904660     45.10859908
#> 78        h       21.5686224     30.20683760
#> 79        i      128.5500910    -16.56482373
#> 80        j      -41.4685040    -26.77521331
#> 81        a       10.0501328     42.85648107
#> 82        b       36.2295085     29.57138871
#> 83        c      -85.9063110    -74.65955397
#> 84        d      -75.5819418     64.10378820
#> 85        e       -7.1729374    -76.14300210
#> 86        f      151.2019966     63.51206461
#> 87        g      -35.7407335    -70.85754704
#> 88        h     -103.2578240     -2.73549177
#> 89        i       61.8360537    -45.50056013
#> 90        j     -158.8989200     33.58245791
#> 91        a      178.9448887    -60.54782433
#> 92        b     -126.3472318     81.50846397
#> 93        c        6.6803888    -32.06618080
#> 94        d      124.6032197    -24.92385898
#> 95        e       78.5771007     69.79021515
#> 96        f      -93.1269528     59.04259520
#> 97        g       16.9356126    -71.88183761
#> 98        h      120.5286535     73.08928400
#> 99        i     -169.9358308     49.09146560
#> 100       j      -47.9200000    -15.78000000
```
