# Nomis metadata concepts and types

Retrieve all concept code options of all Nomis datasets, concept code
options for a given dataset, or the all the options for a given concept
variable from a particular dataset. Specifying `concept` will return all
the options for a given variable in a particular dataset.

If looking for a more detailed overview of all available metadata for a
given dataset, see
[`nomis_overview()`](https://docs.ropensci.org/nomisr/reference/nomis_overview.md).

## Usage

``` r
nomis_get_metadata(
  id,
  concept = NULL,
  type = NULL,
  search = NULL,
  additional_queries = NULL,
  ...,
  tidy = FALSE
)
```

## Arguments

- id:

  The ID of the particular dataset. Returns no data if not specified.

- concept:

  A string with the variable concept to return options for. If left
  empty, returns all the variables for the dataset specified by `id`.
  Codes are not case sensitive. Defaults to `NULL`.

- type:

  A string with options for a particular code value, to return types of
  variables available for a given code. Defaults to `NULL`. If
  `concept == NULL`, `type` will be ignored.

- search:

  A string or character vector of strings to search for in the metadata.
  Defaults to `NULL`. As in
  [`nomis_search()`](https://docs.ropensci.org/nomisr/reference/nomis_search.md),
  the wildcard character `*` can be added to the beginning and/or end of
  each search string.

- additional_queries:

  Any other additional queries to pass to the API. See
  <https://www.nomisweb.co.uk/api/v01/help> for instructions on query
  structure. Defaults to `NULL`. Deprecated in package versions greater
  than 0.2.0 and will eventually be removed.

- ...:

  Use to pass any other parameters to the API.

- tidy:

  If `TRUE`, converts tibble names to snakecase.

## Value

A tibble with metadata options for queries using
[`nomis_get_data()`](https://docs.ropensci.org/nomisr/reference/nomis_get_data.md).

## See also

[`nomis_data_info()`](https://docs.ropensci.org/nomisr/reference/nomis_data_info.md)

[`nomis_get_data()`](https://docs.ropensci.org/nomisr/reference/nomis_get_data.md)

[`nomis_overview()`](https://docs.ropensci.org/nomisr/reference/nomis_overview.md)

## Examples

``` r
# \donttest{
a <- nomis_get_metadata("NM_1_1")

print(a)
#> # A tibble: 6 × 3
#>   codelist         conceptref isfrequencydimension
#>   <chr>            <chr>      <chr>               
#> 1 CL_1_1_GEOGRAPHY GEOGRAPHY  false               
#> 2 CL_1_1_SEX       SEX        false               
#> 3 CL_1_1_ITEM      ITEM       false               
#> 4 CL_1_1_MEASURES  MEASURES   false               
#> 5 CL_1_1_FREQ      FREQ       true                
#> 6 CL_1_1_TIME      TIME       false               

b <- nomis_get_metadata("NM_1_1", "geography")

tibble::glimpse(b)
#> Rows: 7
#> Columns: 4
#> $ id             <chr> "2092957697", "2092957698", "2092957699", "2092957700",…
#> $ parentCode     <chr> NA, NA, NA, "2092957700", "2092957701", "2092957702", NA
#> $ label.en       <chr> "United Kingdom", "Great Britain", "England", "Wales", …
#> $ description.en <chr> "United Kingdom", "Great Britain", "England", "Wales", …

# returns all types of geography
c <- nomis_get_metadata("NM_1_1", "geography", "TYPE")

tibble::glimpse(c)
#> Rows: 122
#> Columns: 3
#> $ id             <chr> "TYPE1", "TYPE8", "TYPE18", "TYPE27", "TYPE33", "TYPE45…
#> $ label.en       <chr> "1991 frozen wards", "parliamentary constituencies 1983…
#> $ description.en <chr> "1991 frozen wards", "parliamentary constituencies 1983…

# returns geography types available within Wigan
d <- nomis_get_metadata("NM_1_1", "geography", "1879048226")

tibble::glimpse(d)
#> Rows: 13
#> Columns: 4
#> $ id             <chr> "1879048226", "1879048226TYPE236", "1879048226TYPE297",…
#> $ parentCode     <chr> "2013265922", NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
#> $ label.en       <chr> "Wigan", "2011 census frozen wards within Wigan", "2011…
#> $ description.en <chr> "Wigan", "2011 census frozen wards within Wigan", "2011…

e <- nomis_get_metadata("NM_1_1", "item", geography = 1879048226, sex = 5)

print(e)
#> # A tibble: 5 × 4
#>   id    parentCode label.en                 description.en          
#>   <chr> <chr>      <chr>                    <chr>                   
#> 1 1     NA         Total claimants          Total claimants         
#> 2 2     1          Students on vacation     Students on vacation    
#> 3 3     1          Temporarily stopped      Temporarily stopped     
#> 4 4     1          Claimants under 18 years Claimants under 18 years
#> 5 9     1          Married females          Married females         

f <- nomis_get_metadata("NM_1_1", "item", search = "*married*")

tibble::glimpse(f)
#> Rows: 1
#> Columns: 4
#> $ id             <chr> "9"
#> $ parentCode     <chr> "1"
#> $ label.en       <chr> "Married females"
#> $ description.en <chr> "Married females"
# }
```
