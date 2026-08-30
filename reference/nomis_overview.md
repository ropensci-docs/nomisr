# Nomis dataset overview

Returns an overview of available metadata for a given dataset.

## Usage

``` r
nomis_overview(id, select = NULL)
```

## Arguments

- id:

  The ID of the particular dataset. Returns no data if not specified.

- select:

  A string or character vector of one or more overview parts to select,
  excluding all others. `select` is not case sensitive. The options for
  `select` are described below, and are taken from the [Nomis API help
  page](https://www.nomisweb.co.uk/api/v01/help).

## Value

A tibble with two columns, one a character vector with the name of the
metadata category, and the other a list column of values for each
category.

## Overview part options

- DatasetInfo:

  General dataset information such as name, description,
  sub-description, mnemonic, restricted access and status

- Coverage:

  Shows the geographic coverage of the main geography dimension in this
  dataset (e.g. United Kingdom, England and Wales etc.)

- Keywords:

  The keywords allocated to the dataset

- Units:

  The units of measure supported by the dataset

- ContentTypes:

  The classifications allocated to this dataset

- DateMetadata:

  Information about the first release, last update and next update

- Contact:

  Details for the point of contact for this dataset

- Analyses:

  Show the available analysis breakdowns of this dataset

- Dimensions:

  Individual dimension information (e.g. sex, geography, date, etc.)

- Dimension-concept:

  Allows a specific dimension to be selected (e.g. dimension-geography
  would allow information about geography dimension). This is not used
  if "Dimensions" is specified too.

- Codes:

  Full list of selectable codes, excluding Geography, which as a list of
  Types instead. (Requires "Dimensions" to be selected too)

- Codes-concept:

  Full list of selectable codes for a specific dimension, excluding
  Geography, which as a list of Types instead. This is not used if
  "Codes" is specified too (Requires "Dimensions" or equivalent to be
  selected too)

- DimensionMetadata:

  Any available metadata attached at the dimensional level (Requires
  "Dimensions" or equivalent to be selected too)

- Make:

  Information about whether user defined codes can be created with the
  MAKE parameter when querying data (Requires "Dimensions" or equivalent
  to be selected too)

- DatasetMetadata:

  Metadata attached at the dataset level

## See also

[`nomis_data_info()`](https://docs.ropensci.org/nomisr/reference/nomis_data_info.md)

[`nomis_get_metadata()`](https://docs.ropensci.org/nomisr/reference/nomis_get_metadata.md)

## Examples

``` r
# \donttest{
library(dplyr)
#> 
#> Attaching package: ‘dplyr’
#> The following objects are masked from ‘package:stats’:
#> 
#>     filter, lag
#> The following objects are masked from ‘package:base’:
#> 
#>     intersect, setdiff, setequal, union

q <- nomis_overview("NM_1650_1")

q %>%
  tidyr::unnest(name) %>%
  glimpse()
#> Rows: 21
#> Columns: 2
#> $ name  <chr> "analyses", "analysisname", "analysisnumber", "contact", "conten…
#> $ value <list> [[<data.frame[3 x 3]>]], "output data for a single date or rang…

s <- nomis_overview("NM_1650_1", select = c("Units", "Keywords"))

s %>%
  tidyr::unnest(name) %>%
  glimpse()
#> Rows: 3
#> Columns: 2
#> $ name  <chr> "id", "keywords", "units"
#> $ value <list> "NM_1650_1", "Year last worked", [["Persons"]]
# }
```
