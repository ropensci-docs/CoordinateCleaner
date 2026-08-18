# Country Centroids and Country Capitals

A `data.frame` with coordinates of country and province centroids and
country capitals as reference for the
[`clean_coordinates`](https://docs.ropensci.org/CoordinateCleaner/reference/clean_coordinates.md),
[`cc_cen`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cen.md)
and
[`cc_cap`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_cap.md)
functions. Coordinates are based on the Central Intelligence Agency
World Factbook <https://www.cia.gov/the-world-factbook/>,
<https://thematicmapping.org/downloads/world_borders.php> and geolocate
<https://geo-locate.org>.

## Format

A data frame with 5,305 observations on 13 variables. \#'

- iso3:

  ISO-3 code for each country, in case of provinces also referring to
  the country.

- iso2:

  ISO-2 code for each country, in case of provinces also referring to
  the country.

- adm1_code:

  adm code for countries and provinces.

- name:

  a factor; name of the country or province.

- type:

  identifying if the entry refers to a country or province level.

- centroid.lon:

  Longitude of the country centroid.

- centroid.lat:

  Latitude of the country centroid.

- capital:

  Name of the country capital, empty for provinces.

- capital.lon:

  Longitude of the country capital.

- capital.lat:

  Latitude of the country capital.

- area_sqkm:

  The area of the country or province.

- uncertaintyRadiusMeters:

  The uncertainty of the country centroid.

- source:

  The data source. Currently only available for <https://geo-locate.org>

## Source

CENTRAL INTELLIGENCE AGENCY (2014) *The World Factbook*, Washington, DC.

<https://www.cia.gov/the-world-factbook/>
<https://thematicmapping.org/downloads/world_borders.php>
<https://geo-locate.org>

## Examples

``` r

data(countryref)
head(countryref)
#>   iso3 iso2 adm1_code          name    type centroid.lon centroid.lat capital
#> 1  AFG   AF       AFG   Afghanistan country       65.000       33.000   Kabul
#> 2  AFG   AF       AFG   Afghanistan country       66.000       33.000   Kabul
#> 3  AFG   AF       AFG   Afghanistan country       65.216       33.677   Kabul
#> 4  ALA   AX       ALA Aland Islands country       19.952       60.198    <NA>
#> 5  ALA   AX       ALA Aland Islands country       20.000       60.250    <NA>
#> 6  ALB   AL       ALB       Albania country       20.000       41.000  Tirana
#>   capital.lon capital.lat area_sqkm uncertaintyRadiusMeters    source
#> 1       69.18       34.52 642181.62                     301 geolocate
#> 2       69.18       34.52 642181.62                     301 geolocate
#> 3       69.18       34.52 642181.62                    <NA>      <NA>
#> 4          NA          NA        NA                    <NA>      <NA>
#> 5          NA          NA        NA                    <NA>      <NA>
#> 6       19.82       41.32  28335.85                     301 geolocate
```
