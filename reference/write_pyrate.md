# Create Input Files for PyRate

Creates the input necessary to run Pyrate, based on a data.frame with
fossil ages (as derived e.g. from clean_fossils) and a vector of the
extinction status for each sample. Creates files in the working
directory!

## Usage

``` r
write_pyrate(
  x,
  status,
  fname,
  taxon = "accepted_name",
  min_age = "min_ma",
  max_age = "max_ma",
  trait = NULL,
  path = getwd(),
  replicates = 1,
  cutoff = NULL,
  random = TRUE
)
```

## Arguments

- x:

  data.frame. Containing fossil records with taxon names, ages, and
  geographic coordinates.

- status:

  a vector of character strings of length `nrow(x)`. Indicating for each
  record “extinct” or “extant”.

- fname:

  a character string. The prefix to use for the output files.

- taxon:

  character string. The column with the taxon name. Default =
  “accepted_name”.

- min_age:

  character string. The column with the minimum age. Default = “min_ma”.

- max_age:

  character string. The column with the maximum age. Default = “max_ma”.

- trait:

  a numeric vector of length `nrow(x)`. Indicating trait values for each
  record. Optional. Default = NULL.

- path:

  a character string. giving the absolute path to write the output
  files. Default is the working directory.

- replicates:

  a numerical. The number of replicates for the randomized age
  generation. See details. Default = 1.

- cutoff:

  a numerical. Specify a threshold to exclude fossil occurrences with a
  high temporal uncertainty, i.e. with a wide temporal range between
  min_age and max_age. Examples: cutoff=NULL (default; all occurrences
  are kept in the data set) cutoff=5 (all occurrences with a temporal
  range of 5 Myr or higher are excluded from the data set)

- random:

  logical. Specify whether to take a random age (between MinT and MaxT)
  for each occurrence or the midpoint age. Note that this option
  defaults to TRUE if several replicates are generated (i.e. replicates
  \> 1). Examples: random = TRUE (default) random = FALSE (use midpoint
  ages)

## Value

PyRate input files in the working directory.

## Details

The replicate option allows the user to generate several replicates of
the data set in a single input file, each time re-drawing the ages of
the occurrences at random from uniform distributions with boundaries
MinT and MaxT. The replicates can be analysed in different runs (see
PyRate command -j) and combining the results of these replicates is a
way to account for the uncertainty of the true ages of the fossil
occurrences. Examples: replicates=1 (default, generates 1 data set),
replicates=10 (generates 10 random replicates of the data set).

## Note

See <https://github.com/dsilvestro/PyRate/wiki> for more details and
tutorials on PyRate and PyRate input.

## See also

Other fossils:
[`cf_age()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_age.md),
[`cf_equal()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_equal.md),
[`cf_outl()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_outl.md),
[`cf_range()`](https://docs.ropensci.org/CoordinateCleaner/reference/cf_range.md)

## Examples

``` r

minages <- runif(250, 0, 65)
exmpl <- data.frame(accepted_name = sample(letters, size = 250, replace = TRUE),
                    lng = runif(250, min = 42, max = 51),
                    lat = runif(250, min = -26, max = -11),
                    min_ma = minages,
                    max_ma = minages + runif(250, 0.1, 65))

#a vector with the status for each record, 
#make sure species are only classified as either extinct or extant, 
#otherwise the function will drop an error

status <- sample(c("extinct", "extant"), size = nrow(exmpl), replace = TRUE)

#or from a list of species
status <- sample(c("extinct", "extant"), size = length(letters), replace = TRUE)
names(status) <- letters
status <- status[exmpl$accepted_name]

if (FALSE) { # \dontrun{
write_pyrate(x = exmpl,fname = "test", status = status)
} # }
```
