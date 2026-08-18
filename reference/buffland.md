# Global Coastlines buffered by 1 degree

A `SpatVector` with global coastlines, with a 1 degree buffer to extent
coastlines as alternative reference for
[`cc_sea`](https://docs.ropensci.org/CoordinateCleaner/reference/cc_sea.md).
Can be useful to identify species in the sea, without flagging records
in mangroves, marshes, etc.

## Source

<https://www.naturalearthdata.com/downloads/10m-physical-vectors/>

## Examples

``` r

data("buffland")
```
