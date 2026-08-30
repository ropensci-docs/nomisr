# Search Nomis datasets

A function to search for datasets on given topics. In the case of
multiple search parameters, returns metadata on all datasets matching
one or more of the parameters. The wildcard character `*` can be added
to the beginning and/or end of each search string.

## Usage

``` r
nomis_search(
  name = NULL,
  description = NULL,
  keywords = NULL,
  content_type = NULL,
  units = NULL,
  tidy = FALSE
)
```

## Arguments

- name:

  A string or character vector of strings to search for in available
  dataset names. Defaults to `NULL`.

- description:

  A string or character vector of strings to search for in available
  dataset descriptions. Note that `description` looks for complete
  matches, so wildcards should be used at the start and end of each
  string. Defaults to `NULL`.

- keywords:

  A string or character vector of strings to search for in available
  dataset keywords. Defaults to `NULL`.

- content_type:

  A string or character vector of strings to search for in available
  dataset content types. `content_type` can include an optional ID for
  that content type. Defaults to `NULL`.

- units:

  A string or character vector of strings to search for in available
  dataset units. Defaults to `NULL`.

- tidy:

  If `TRUE`, converts tibble names to snakecase.

## Value

A tibble with details on all datasets matching the search query.

## See also

[`nomis_content_type()`](https://docs.ropensci.org/nomisr/reference/nomis_content_type.md)

## Examples

``` r
# \donttest{
x <- nomis_search(name = "*seekers*")

y <- nomis_search(keywords = "Claimants")

# Return metadata of all datasets with content_type "sources".
a <- nomis_search(content_type = "sources")


# Return metadata of all datasets with content_type "sources" and
# source ID "acses"
b <- nomis_search(content_type = "sources-acses")
# }
```
