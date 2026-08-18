# Global Locations of Biodiversity Institutions

A global gazetteer for biodiversity institutions from various sources,
including zoos, museums, botanical gardens, GBIF contributors, herbaria,
university collections.

## Format

A data frame with 12170 observations on 12 variables.

## Source

Compiled from various sources:

- Global Biodiversity Information Facility <https://www.gbif.org/>

- Wikipedia <https://www.wikipedia.org/>

- Geonames <https://www.geonames.org/>

- The Global Registry of Biodiversity Repositories

- Index Herbariorum <https://sweetgum.nybg.org/science/ih/>

- Botanic Gardens Conservation International <https://www.bgci.org/>

## Examples

``` r

data(institutions)
str(institutions)
#> tibble [11,601 × 12] (S3: tbl_df/tbl/data.frame)
#>  $ name                       : chr [1:11601] "A.N. Severtsov Institute of Ecology and Evolution, RUSSIAN ACADEMY OF SCIENCES" "Aarhus University" "Aarhus University, Science Museums" "Aathal Dinosaur Museum" ...
#>  $ decimalLongitude           : num [1:11601] 37.58 12.1 10.2 8.76 31.6 ...
#>  $ decimalLatitude            : num [1:11601] 55.7 55.7 56.2 47.3 40.7 ...
#>  $ city                       : chr [1:11601] "Moscow" "Aarhus C" "Aarhus" "Aathal" ...
#>  $ country                    : chr [1:11601] "RUS" "DNK" "DNK" "CHE" ...
#>  $ address                    : chr [1:11601] NA NA NA NA ...
#>  $ source                     : chr [1:11601] "GBIF" "grbioorg" "indexherbariorum" "wikipedia" ...
#>  $ type                       : chr [1:11601] "Research_centre" "University" "Herbarium" "Museum" ...
#>  $ geocoding.precision.m      : num [1:11601] 1000 10 1000 1000 10 10 1000 1000 1000 10 ...
#>  $ geocoding.issue            : chr [1:11601] NA NA NA NA ...
#>  $ geocoding.source           : chr [1:11601] "RS" "automatic" "RS" "CD" ...
#>  $ inside.ptoected.area.WDPAID: int [1:11601] NA NA NA NA NA NA NA NA 555575282 NA ...
#>  - attr(*, "problems")= tibble [1 × 5] (S3: tbl_df/tbl/data.frame)
#>   ..$ row     : int 1547
#>   ..$ col     : chr "inside.ptoected.area.WDPAID"
#>   ..$ expected: chr "no trailing characters"
#>   ..$ actual  : chr ".00E+03"
#>   ..$ file    : chr "'institutions.csv'"
#>  - attr(*, "spec")=
#>   .. cols(
#>   ..   name = col_character(),
#>   ..   decimallongitude = col_double(),
#>   ..   decimallatitude = col_double(),
#>   ..   city = col_character(),
#>   ..   country = col_character(),
#>   ..   address = col_character(),
#>   ..   source = col_character(),
#>   ..   type = col_character(),
#>   ..   geocoding.precision.m = col_double(),
#>   ..   geocoding.issue = col_character(),
#>   ..   geocoding.source = col_character(),
#>   ..   inside.ptoected.area.WDPAID = col_integer()
#>   .. )
```
