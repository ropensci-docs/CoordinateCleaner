# Identify Records with Identical lat/lon

Removes or flags records with equal latitude and longitude coordinates,
either exact or absolute. Equal coordinates can often indicate data
entry errors.

## Usage

``` r
cc_equ(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  test = "absolute",
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

- test:

  character string. Defines if coordinates are compared exactly
  (“identical”) or on the absolute scale (i.e. -1 = 1, “absolute”).
  Default is to “absolute”.

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
[`cc_dupl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_dupl.md),
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
                decimalLongitude = runif(100, -180, 180), 
                decimalLatitude = runif(100, -90,90))

cc_equ(x)
#> Testing equal lat/lon
#> Removed 0 records.
#>     species decimalLongitude decimalLatitude
#> 1         a      -47.0750153      24.9990719
#> 2         b     -134.4000298       0.6111147
#> 3         c       86.3541020     -44.9058133
#> 4         d      164.1118629      86.2260794
#> 5         e       60.7228209      28.1843248
#> 6         f      -11.4878344     -73.8271978
#> 7         g       13.7492808      23.4977275
#> 8         h      -60.8566673      67.8181791
#> 9         i       46.3255588      77.4506764
#> 10        j      -17.0811102     -14.4925465
#> 11        a      139.6854639      -0.7992234
#> 12        b      102.3126551     -26.5131555
#> 13        c     -130.2985617     -45.2277239
#> 14        d      -78.7272985      82.5710218
#> 15        e       74.5007393     -25.2071779
#> 16        f      -20.8127087     -54.1824765
#> 17        g       63.9714683      40.7313571
#> 18        h       72.2573948     -59.7361589
#> 19        i     -155.3736890     -49.2333607
#> 20        j      165.0612674      62.1972099
#> 21        a      -91.9859324     -27.7463518
#> 22        b      153.4620005     -77.8824309
#> 23        c        0.9889476     -85.1364983
#> 24        d      156.7861711     -68.7519775
#> 25        e      -22.0321323     -87.5348337
#> 26        f      -53.8248468      42.4312863
#> 27        g       -8.4585282      76.3129378
#> 28        h      167.8174290      86.7943078
#> 29        i      -73.0217063     -17.0513633
#> 30        j      -75.3666090     -41.4549660
#> 31        a      129.5000091      -2.1080429
#> 32        b     -110.4074399     -80.8087585
#> 33        c      111.2790137     -89.5541706
#> 34        d      -48.4248555      -0.7497585
#> 35        e      -89.0281249     -46.1175690
#> 36        f      128.1891650     -43.9651334
#> 37        g     -133.1912270       9.4173799
#> 38        h      105.0867578     -44.7160269
#> 39        i      -51.8754563      23.1210300
#> 40        j      167.1637816     -33.0992088
#> 41        a     -137.2740447      76.4136706
#> 42        b      -15.0621662      73.1685060
#> 43        c        8.5141572     -73.4454368
#> 44        d      121.3422213      42.8031274
#> 45        e     -167.9385465      12.5532972
#> 46        f     -164.4017618      35.3679436
#> 47        g      105.1570600      36.1381506
#> 48        h     -107.0283727     -82.8106821
#> 49        i     -155.2336387     -12.3259673
#> 50        j      131.1987864      56.5616458
#> 51        a     -152.2476506      11.9855299
#> 52        b      -24.8387693      15.8844911
#> 53        c      170.5041209      68.4556258
#> 54        d       25.5446438      39.4772334
#> 55        e       91.5863924      78.5572008
#> 56        f      -92.7164415     -12.7023095
#> 57        g       45.7768317     -38.4862814
#> 58        h     -119.3707429      48.7157730
#> 59        i      -19.8757321      20.9697546
#> 60        j     -120.7169157     -61.3305454
#> 61        a     -137.4203581     -17.7123590
#> 62        b      -47.7128194     -89.7036320
#> 63        c      160.7612058     -14.7661136
#> 64        d      135.1478748      47.5081849
#> 65        e      -86.3787915      53.0059800
#> 66        f       56.6274374     -46.3343363
#> 67        g     -136.1287348      46.6507717
#> 68        h      -65.2147060     -84.7612524
#> 69        i       44.3019709     -11.2786591
#> 70        j      -19.0809251       5.4737077
#> 71        a       74.9856405     -12.6303568
#> 72        b      140.4104400      55.6734526
#> 73        c     -142.4496586      76.0616313
#> 74        d       46.9482188      31.5042252
#> 75        e      122.9612078     -73.0857253
#> 76        f      -48.8655247     -54.4566029
#> 77        g       44.2522001      -4.4018774
#> 78        h      -14.1308949      38.5868942
#> 79        i     -121.1715794       6.8542712
#> 80        j        4.9920703      67.5689419
#> 81        a     -128.4240822     -37.1546257
#> 82        b       54.1075323     -49.5422230
#> 83        c     -124.0939688      34.3652096
#> 84        d        2.9915504     -33.7807071
#> 85        e     -149.0866400      -3.0063279
#> 86        f        1.0895987     -68.0766048
#> 87        g     -166.1141220      34.2013023
#> 88        h      116.6518358     -33.1683472
#> 89        i      -77.0451933      56.9137829
#> 90        j      -91.1696878       8.2647789
#> 91        a      -36.7223061      80.5170931
#> 92        b       47.5534351     -83.0652468
#> 93        c      161.4883843      26.5758978
#> 94        d      -57.5570722     -75.6685780
#> 95        e       45.8517548     -12.0724615
#> 96        f      116.9695422      46.9607641
#> 97        g     -127.7584643     -37.1234088
#> 98        h      168.1700870     -70.6802593
#> 99        i     -149.0259637     -76.8610005
#> 100       j      -93.4339555     -25.8633761
cc_equ(x, value = "flagged")
#> Testing equal lat/lon
#> Flagged 0 records.
#>   [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [16] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [31] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [46] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [61] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [76] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
#>  [91] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
```
