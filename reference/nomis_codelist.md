# Nomis codelists

Nomis uses its own internal coding for query concepts. `nomis_codelist`
returns the codes for a given concept in a `tibble`, given a dataset ID
and a concept name. Note that some codelists, particularly geography,
can be very large.

## Usage

``` r
nomis_codelist(id, concept, search = NULL)
```

## Arguments

- id:

  A string with the ID of the particular dataset. Must be specified.

- concept:

  A string with the variable concept to return options for. If left
  empty, returns all the variables for the dataset specified by `id`.
  Codes are not case sensitive and must be specified.

- search:

  Search for codes that contain a given string. The wildcard character
  `*` can be added to the beginning and/or end of each search string.
  Search strings are not case sensitive. Defaults to `NULL`. Note that
  the search function is not very powerful for some datasets.

## Value

A tibble with the codes used to query specific concepts.

## See also

[`nomis_data_info()`](https://docs.ropensci.org/nomisr/reference/nomis_data_info.md)

[`nomis_get_metadata()`](https://docs.ropensci.org/nomisr/reference/nomis_get_metadata.md)

[`nomis_overview()`](https://docs.ropensci.org/nomisr/reference/nomis_overview.md)

## Examples

``` r
# \donttest{
x <- nomis_codelist("NM_1_1", "item")


# Searching for codes ending with "london"
y <- nomis_codelist("NM_1_1", "geography", search = "*london")


z <- nomis_codelist("NM_161_1", "cause_of_death")
# }
```
