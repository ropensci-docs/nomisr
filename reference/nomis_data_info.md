# Nomis data structures

Retrieve metadata on the structure and available variables for all
available data sets or the information available in a specific dataset
based on its ID.

## Usage

``` r
nomis_data_info(id, tidy = FALSE)
```

## Arguments

- id:

  Dataset ID. If empty, returns data on all available datasets. If the
  ID of a dataset, returns metadata for that particular dataset.

- tidy:

  If `TRUE`, converts tibble names to snakecase.

## Value

A tibble with all available datasets and their metadata.

## See also

[`nomis_get_data()`](https://docs.ropensci.org/nomisr/reference/nomis_get_data.md)

[`nomis_get_metadata()`](https://docs.ropensci.org/nomisr/reference/nomis_get_metadata.md)

[`nomis_overview()`](https://docs.ropensci.org/nomisr/reference/nomis_overview.md)

[`nomis_codelist()`](https://docs.ropensci.org/nomisr/reference/nomis_codelist.md)

## Examples

``` r
# \donttest{

# Get info on all datasets
x <- nomis_data_info()

tibble::glimpse(x)
#> Rows: 1,617
#> Columns: 14
#> $ agencyid                             <chr> "NOMIS", "NOMIS", "NOMIS", "NOMIS…
#> $ id                                   <chr> "NM_1_1", "NM_2_1", "NM_3_1", "NM…
#> $ uri                                  <chr> "Nm-1d1", "Nm-2d1", "Nm-3d1", "Nm…
#> $ version                              <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
#> $ annotations.annotation               <list> [<data.frame[9 x 2]>], [<data.fr…
#> $ components.attribute                 <list> [<data.frame[7 x 4]>], [<data.fr…
#> $ components.dimension                 <list> [<data.frame[5 x 3]>], [<data.fr…
#> $ components.primarymeasure.conceptref <chr> "OBS_VALUE", "OBS_VALUE", "OBS_VA…
#> $ components.timedimension.codelist    <chr> "CL_1_1_TIME", "CL_2_1_TIME", "CL…
#> $ components.timedimension.conceptref  <chr> "TIME", "TIME", "TIME", "TIME", "…
#> $ description.value                    <chr> "Records the number of people cla…
#> $ description.lang                     <chr> "en", "en", "en", "en", "en", "en…
#> $ name.value                           <chr> "Jobseeker's Allowance with rates…
#> $ name.lang                            <chr> "en", "en", "en", "en", "en", "en…

# Get info on a particular dataset
y <- nomis_data_info("NM_1658_1")

tibble::glimpse(y)
#> Rows: 1
#> Columns: 12
#> $ agencyid                             <chr> "NOMIS"
#> $ id                                   <chr> "NM_1658_1"
#> $ uri                                  <chr> "Nm-1658d1"
#> $ version                              <dbl> 1
#> $ annotations.annotation               <list> [<data.frame[14 x 2]>]
#> $ components.attribute                 <list> [<data.frame[7 x 4]>]
#> $ components.dimension                 <list> [<data.frame[4 x 3]>]
#> $ components.primarymeasure.conceptref <chr> "OBS_VALUE"
#> $ components.timedimension.codelist    <chr> "CL_1658_1_TIME"
#> $ components.timedimension.conceptref  <chr> "TIME"
#> $ name.value                           <chr> "UV035 - Distance travelled to wo…
#> $ name.lang                            <chr> "en"
# }
```
