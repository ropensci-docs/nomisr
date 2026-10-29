# Nomis dataset content types

Nomis content type metadata is included in annotation tags, in the form
of `contenttype/<contenttype>` in the `annotationtitle` column in the
`annotations.annotation` list-column returned from
[`nomis_data_info()`](https://docs.ropensci.org/nomisr/reference/nomis_data_info.md).
For example, the content types returned from dataset "NM_1658_1", using
`nomis_data_info("NM_1658_1")`, are "geoglevel", "2001census" and
"sources".

## Usage

``` r
nomis_content_type(content_type, id = NULL)
```

## Arguments

- content_type:

  A string with the content type to return metadata on.

- id:

  A string with an optional `content_type` id.

## Value

A tibble with metadata on a given content type.

## See also

[`nomis_search()`](https://docs.ropensci.org/nomisr/reference/nomis_search.md)

[`nomis_data_info()`](https://docs.ropensci.org/nomisr/reference/nomis_data_info.md)

## Examples

``` r
# \donttest{
a <- nomis_content_type("sources")

tibble::glimpse(a)
#> Rows: 18
#> Columns: 6
#> $ description <chr> "A count of home Civil Service employees. It excludes the …
#> $ id          <chr> "acses", "aps", "ashe", "bres", "census", "cc", "dwp", "ho…
#> $ name        <chr> "Annual Civil Service Employment Survey", "Annual Populati…
#> $ type        <chr> "DataSource", "DataSource", "DataSource", "DataSource", "D…
#> $ item        <list> <NULL>, <NULL>, <NULL>, <NULL>, [<data.frame[9 x 5]>], <N…
#> $ status      <chr> NA, NA, NA, NA, NA, NA, "archived", NA, NA, "archived", NA…

b <- nomis_content_type("sources", id = "census")

tibble::glimpse(b)
#> Rows: 9
#> Columns: 8
#> $ id               <chr> "census", "census", "census", "census", "census", "ce…
#> $ item.description <chr> "The 2021 Census was taken on 21 March 2021.", "The 2…
#> $ item.id          <chr> "census_2021", "census_2011", "census_2001", "census_…
#> $ item.item        <list> [<data.frame[14 x 5]>], [<data.frame[13 x 5]>], [<da…
#> $ item.name        <chr> "2021 Census", "2011 Census", "2001 Census", "1991 Ce…
#> $ item.type        <chr> "DataSourceSubGroup", "DataSourceSubGroup", "DataSour…
#> $ name             <chr> "Census of Population", "Census of Population", "Cens…
#> $ type             <chr> "DataSource", "DataSource", "DataSource", "DataSource…
# }
```
