# Geographic Cleaning of Coordinates from Biologic Collections

Cleaning geographic coordinates by multiple empirical tests to flag
potentially erroneous coordinates, addressing issues common in
biological collection databases.

## Usage

``` r
clean_coordinates(
  x,
  lon = "decimalLongitude",
  lat = "decimalLatitude",
  species = "species",
  countries = NULL,
  tests = c("capitals", "centroids", "equal", "gbif", "institutions", "outliers", "seas",
    "zeros"),
  capitals_rad = 10000,
  centroids_rad = 1000,
  centroids_detail = "both",
  inst_rad = 100,
  outliers_method = "quantile",
  outliers_mtp = 5,
  outliers_td = 1000,
  outliers_size = 7,
  range_rad = 0,
  zeros_rad = 0.5,
  capitals_ref = NULL,
  centroids_ref = NULL,
  country_ref = NULL,
  country_refcol = "iso_a3",
  country_buffer = NULL,
  inst_ref = NULL,
  range_ref = NULL,
  seas_ref = NULL,
  seas_scale = 50,
  seas_buffer = NULL,
  urban_ref = NULL,
  aohi_rad = NULL,
  value = "spatialvalid",
  verbose = TRUE,
  report = FALSE
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

  a character string. A vector of the same length as rows in x, with the
  species identity for each record. If NULL, `tests` must not include
  the "outliers" or "duplicates" tests.

- countries:

  a character string. The column with the country assignment of each
  record in three letter ISO code. Default = “countrycode”. If missing,
  the countries test is skipped.

- tests:

  a vector of character strings, indicating which tests to run. See
  details for all tests available. Default = c("capitals", "centroids",
  "equal", "gbif", "institutions", "outliers", "seas", "zeros")

- capitals_rad:

  numeric. The radius around capital coordinates in meters. Default =
  10000.

- centroids_rad:

  numeric. The radius around centroid coordinates in meters. Default =
  1000.

- centroids_detail:

  a `character string`. If set to ‘country’ only country (adm-0)
  centroids are tested, if set to ‘provinces’ only province (adm-1)
  centroids are tested. Default = ‘both’.

- inst_rad:

  numeric. The radius around biodiversity institutions coordinates in
  metres. Default = 100.

- outliers_method:

  The method used for outlier testing. See details.

- outliers_mtp:

  numeric. The multiplier for the interquartile range of the outlier
  test. If NULL `outliers.td` is used. Default = 5.

- outliers_td:

  numeric. The minimum distance of a record to all other records of a
  species to be identified as outlier, in km. Default = 1000.

- outliers_size:

  numerical. The minimum number of records in a dataset to run the
  taxon-specific outlier test. Default = 7.

- range_rad:

  buffer around natural ranges. Default = 0.

- zeros_rad:

  numeric. The radius around 0/0 in degrees. Default = 0.5.

- capitals_ref:

  a `data.frame` with alternative reference data for the country
  capitals test. If missing, the `countryref` dataset is used.
  Alternatives must be identical in structure.

- centroids_ref:

  a `data.frame` with alternative reference data for the centroid test.
  If NULL, the `countryref` dataset is used. Alternatives must be
  identical in structure.

- country_ref:

  a `SpatVector` as alternative reference for the countries test. If
  NULL, the `rnaturalearth:ne_countries('medium', returnclass = "sf")`
  dataset is used.

- country_refcol:

  the column name in the reference dataset, containing the relevant ISO
  codes for matching. Default is to "iso_a3_eh" which referes to the
  ISO-3 codes in the reference dataset. See notes.

- country_buffer:

  numeric. Units are in meters. If provided, a buffer is created around
  each country polygon.

- inst_ref:

  a `data.frame` with alternative reference data for the biodiversity
  institution test. If NULL, the `institutions` dataset is used.
  Alternatives must be identical in structure.

- range_ref:

  a `SpatVector` of species natural ranges. Required to include the
  'ranges' test. See
  [`cc_iucn`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_iucn.md)
  for details.

- seas_ref:

  a `SpatVector` as alternative reference for the seas test. If NULL,
  the rnaturalearth::ne_download(scale = 110, type = 'land', category =
  'physical', returnclass = "sf") dataset is used.

- seas_scale:

  The scale of the default landmass reference. Must be one of 10,
  50, 110. Higher numbers equal higher detail. Default = 50.

- seas_buffer:

  numeric. Units are in meters. If provided, a buffer is created around
  sea polygon.

- urban_ref:

  a `SpatVector` as alternative reference for the urban test. If NULL,
  the test is skipped. See details for a reference gazetteers.

- aohi_rad:

  numeric. The radius around aohi coordinates in meters. Default = 1000.

- value:

  a character string defining the output value. See the value section
  for details. one of ‘spatialvalid’, ‘summary’, ‘clean’. Default =
  ‘`spatialvalid`’.

- verbose:

  logical. If TRUE reports the name of the test and the number of
  records flagged.

- report:

  logical or character. If TRUE a report file is written to the working
  directory, summarizing the cleaning results. If a character, the path
  to which the file should be written. Default = FALSE.

## Value

Depending on the output argument:

- “spatialvalid”:

  an object of class `spatialvalid` similar to x with one column added
  for each test. TRUE = clean coordinate entry, FALSE = potentially
  problematic coordinate entries. The .summary column is FALSE if any
  test flagged the respective coordinate.

- “flagged”:

  a logical vector with the same order as the input data summarizing the
  results of all test. TRUE = clean coordinate, FALSE = potentially
  problematic (= at least one test failed).

- “clean”:

  a `data.frame` similar to x with potentially problematic records
  removed

## Details

The function needs all coordinates to be formally valid according to
WGS84. If the data contains invalid coordinates, the function will stop
and return a vector flagging the invalid records. TRUE = non-problematic
coordinate, FALSE = potentially problematic coordinates.

- capitals tests a radius around adm-0 capitals. The radius is
  `capitals_rad`.

- centroids tests a radius around country centroids. The radius is
  `centroids_rad`.

- countries tests if coordinates are from the country indicated in the
  country column. *Switched off by default.*

- duplicates tests for duplicate records. This checks for identical
  coordinates or if a species vector is provided for identical
  coordinates within a species. All but the first records are flagged as
  duplicates. *Switched off by default.*

- equal tests for equal absolute longitude and latitude.

- gbif tests a one-degree radius around the GBIF headquarters in
  Copenhagen, Denmark.

- institutions tests a radius around known biodiversity institutions
  from `instiutions`. The radius is `inst_rad`.

- outliers tests each species for outlier records. Depending on the
  `outliers_mtp` and `outliers.td` arguments either flags records that
  are a minimum distance away from all other records of this species
  (`outliers_td`) or records that are outside a multiple of the
  interquartile range of minimum distances to the next neighbour of this
  species (`outliers_mtp`). Three different methods are available for
  the outlier test: "If “outlier” a boxplot method is used and records
  are flagged as outliers if their *mean* distance to all other records
  of the same species is larger than mltpl \* the interquartile range of
  the mean distance of all records of this species. If “mad” the median
  absolute deviation is used. In this case a record is flagged as
  outlier, if the *mean* distance to all other records of the same
  species is larger than the median of the mean distance of all points
  plus/minus the mad of the mean distances of all records of the species
  \* mltpl. If “distance” records are flagged as outliers, if the
  *minimum* distance to the next record of the species is \> `tdi`.

- ranges tests if records fall within provided natural range polygons on
  a per species basis. See
  [`cc_iucn`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_iucn.md)
  for details.

- seas tests if coordinates fall into the ocean.

- urban tests if coordinates are from urban areas. *Switched off by
  default*

- validity checks if coordinates correspond to a lat/lon coordinate
  reference system. This test is always on, since all records need to
  pass for any other test to run.

- zeros tests for plain zeros, equal latitude and longitude and a radius
  around the point 0/0. The radius is `zeros.rad`.

## Note

Always tests for coordinate validity: non-numeric or missing coordinates
and coordinates exceeding the global extent (lon/lat, WGS84). See
<https://ropensci.github.io/CoordinateCleaner/> for more details and
tutorials.

The country_refcol argument allows to adapt the function to the
structure of alternative reference datasets. For instance, for
`rnaturalearth::ne_countries(scale = "small", returnclass = "sf")`, the
default will fail, but country_refcol = "iso_a3" will work.

## See also

Other Wrapper functions:
[`clean_dataset()`](https://docs.ropensci.org/CoordinateCleaner/reference/clean_dataset.md),
[`clean_fossils()`](https://docs.ropensci.org/CoordinateCleaner/reference/clean_fossils.md)

## Examples

``` r


exmpl <- data.frame(species = sample(letters, size = 250, replace = TRUE),
                    decimalLongitude = runif(250, min = 42, max = 51),
                    decimalLatitude = runif(250, min = -26, max = -11))

test <- clean_coordinates(x = exmpl, 
                          tests = c("equal"))
#> Testing coordinate validity
#> Flagged 0 records.
#> Testing equal lat/lon
#> Flagged 0 records.
#> Flagged 0 of 250 records, EQ = 0.
                                    
if (FALSE) { # \dontrun{
#run more tests
test <- clean_coordinates(x = exmpl, 
                          tests = c("capitals", 
                          "centroids","equal", 
                          "gbif", "institutions", 
                          "outliers", "seas", 
                          "zeros"))
} # }
                                 
                                    
summary(test)
#>     .val     .equ .summary 
#>        0        0        0 
```
