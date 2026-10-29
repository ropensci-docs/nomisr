# Retrieve Nomis datasets

To find the code options for a given dataset, use
[`nomis_get_metadata()`](https://docs.ropensci.org/nomisr/reference/nomis_get_metadata.md)
for specific codes, and
[`nomis_codelist()`](https://docs.ropensci.org/nomisr/reference/nomis_codelist.md)
for code values.

This can be a slow process if querying significant amounts of data.
Guest users are limited to 25,000 rows per query, although `nomisr`
identifies queries that will return more than 25,000 rows, sending
individual queries and combining the results of those queries into a
single tibble. In interactive sessions, `nomisr` will warn you if guest
users are requesting more than 350,000 rows of data, and if registered
users are requesting more than 1,500,000 rows.

Note the difference between the `time` and `date` parameters. The `time`
and `date` parameters should not be used at the same time. If they are,
the function will retrieve data based on the the `date` parameter. If
given more than one query, `time` will return all data available between
those queries, inclusively, while `date` will only return data for the
exact queries specified. So `time = c("first", "latest")` will return
all data, while `date = c("first", "latest")` will return only the
earliest and latest data published.

## Usage

``` r
nomis_get_data(
  id,
  time = NULL,
  date = NULL,
  geography = NULL,
  sex = NULL,
  measures = NULL,
  additional_queries = NULL,
  exclude_missing = FALSE,
  select = NULL,
  tidy = FALSE,
  tidy_style = "snake_case",
  query_id = NULL,
  ...
)
```

## Arguments

- id:

  A string containing the ID of the dataset to retrieve, in `"nm_***_*"`
  format. The `id` parameter is not case sensitive.

- time:

  Parameter for selecting dates and date ranges. Accepts either a single
  date value, or two date values and returns all data between the two
  date values, There are two styles of values that can be used to query
  time.

  The first is one or two of `"latest"` (returns the latest available
  data), `"previous"` (the date prior to `"latest"`), `"prevyear"` (the
  date one year prior to `"latest"`) or `"first"` (the oldest available
  data for the dataset).

  The second style is to use or a specific date or multiple dates, in
  the style of the time variable codelist, which can be found using the
  [`nomis_get_metadata()`](https://docs.ropensci.org/nomisr/reference/nomis_get_metadata.md)
  function.

  Values for the `time` and `date` parameters should not be used at the
  same time. If they are, the function will retrieve data based on the
  the `date` parameter.

  Defaults to `NULL`.

- date:

  Parameter for selecting specific dates. Accepts one or more date
  values. If given multiple values, only data for the given dates will
  be returned, but there is no limit to the number of data values. For
  example, `date=c("latest, latestMINUS3, latestMINUS6")` will return
  the latest data, data from three months prior to the latest data and
  six months prior to the latest data. There are two styles of values
  that can be used to query time.

  The first is one or more of `"latest"` (returns the latest available
  data), `"previous"` (the date prior to `"latest"`), `"prevyear"` (the
  date one year prior to `"latest"`) or `"first"` (the oldest available
  data for the dataset).

  The second style is to use or a specific date or multiple dates, in
  the style of the time variable codelist, which can be found using the
  [`nomis_get_metadata()`](https://docs.ropensci.org/nomisr/reference/nomis_get_metadata.md)
  function.

  Values for the `time` and `date` parameters should not be used at the
  same time. If they are, the function will retrieve data based on the
  the `date` parameter.

  Defaults to `NULL`.

- geography:

  The code of the geographic area to return data for. If `NULL`, returns
  data for all available geographic areas, subject to other parameters.
  Defaults to `NULL`. In the rare instance that a geographic variable
  does not exist, if not `NULL`, the function will return an error.

- sex:

  The code for sexes/genders to include in the dataset. Accepts a string
  or number, or a vector of strings or numbers. `nomisr` automatically
  voids any queries for sex if it is not an available code in the
  requested dataset. Defaults to `NULL` and returns all available
  sex/gender data.

  There are two different codings used for sex, depending on the
  dataset. For datasets using `"SEX"`, `7` will return results for males
  and females, `6` only females and `5` only males. Defaults to `NULL`,
  equivalent to `c(5,6,7)` for datasets where sex is an option. For
  datasets using `"C_SEX"`, `0` will return results for males and
  females, `1` only males and `2` only females. Some datasets use
  `"GENDER"` with the same values as `"SEX"`, which works with both
  `sex = <code>` and `gender = <code>` as a dot parameter.

- measures:

  The code for the statistical measure(s) to include in the data.
  Accepts a single string or number, or a list of strings or numbers. If
  `NULL`, returns data for all available statistical measures subject to
  other parameters. Defaults to `NULL`.

- additional_queries:

  Any other additional queries to pass to the API. See
  <https://www.nomisweb.co.uk/api/v01/help> for instructions on query
  structure. Defaults to `NULL`. Deprecated in package versions greater
  than 0.2.0 and will eventually be removed in a future version.

- exclude_missing:

  If `TRUE`, excludes all missing values. Defaults to `FALSE`.

- select:

  A character vector of one or more variables to include in the returned
  data, excluding all others. `select` is not case sensitive.

- tidy:

  Logical parameter. If `TRUE`, converts variable names to `snake_case`,
  or another style as specified by the `tidy_style` parameter. Defaults
  to `FALSE`. The default variable name style from the API is
  SCREAMING_SNAKE_CASE.

- tidy_style:

  The style to convert variable names to, if `tidy = TRUE`. Accepts one
  of `"snake_case"`, `"camelCase"` and `"period.case"`, or any of the
  `case` options accepted by
  [`snakecase::to_any_case()`](https://rdrr.io/pkg/snakecase/man/to_any_case.html).
  Defaults to `"snake_case"`.

- query_id:

  Results can be labelled as belonging to a certain query made to the
  API. `query_id` accepts any value as a string, and will be included in
  every row of the tibble returned by `nomis_get_data` in a column
  labelled "QUERY_ID" in the default SCREAMING_SNAKE_CASE used by the
  API. Defaults to `NULL`.

- ...:

  Use to pass any other parameters to the API. Useful for passing
  concepts that are not available through the default parameters. Only
  accepts concepts identified in
  [`nomis_get_metadata()`](https://docs.ropensci.org/nomisr/reference/nomis_get_metadata.md)
  and concept values identified in
  [`nomis_codelist()`](https://docs.ropensci.org/nomisr/reference/nomis_codelist.md).
  Parameters can be quoted or unquoted. Each parameter should have a
  name and a value. For example, `CAUSE_OF_DEATH = 10300` when querying
  dataset `"NM_161_1"`. Parameters are not case sensitive. Note that R
  using partial matching for function variables, and so passing a
  parameter with the same opening characters as one of the above-named
  parameters can cause an error unless the value of the named parameter
  is specified, including as `NULL`. See example below:

## Value

A tibble containing the selected dataset. By default, all tibble columns
except for the `"OBS_VALUE"` column are parsed as characters.

## See also

[`nomis_data_info()`](https://docs.ropensci.org/nomisr/reference/nomis_data_info.md)

[`nomis_get_metadata()`](https://docs.ropensci.org/nomisr/reference/nomis_get_metadata.md)

[`nomis_codelist()`](https://docs.ropensci.org/nomisr/reference/nomis_codelist.md)

[`nomis_overview()`](https://docs.ropensci.org/nomisr/reference/nomis_overview.md)

## Examples

``` r
# \donttest{

# Return data on Jobseekers Allowance for each country in the UK
jobseekers_country <- nomis_get_data(
  id = "NM_1_1", time = "latest",
  geography = "TYPE499",
  measures = c(20100, 20201), sex = 5
)

# Return data on Jobseekers Allowance for Wigan
jobseekers_wigan <- nomis_get_data(
  id = "NM_1_1", time = "latest",
  geography = "1879048226",
  measures = c(20100, 20201), sex = "5"
)

# annual population survey - regional - employment by occupation
emp_by_occupation <- nomis_get_data(
  id = "NM_168_1", time = "latest",
  geography = "2013265925", sex = "0",
  select = c(
    "geography_code",
    "C_OCCPUK11H_0_NAME", "obs_vAlUE"
  )
)

# Deaths in 2016 and 2015 by three specified causes,
# identified with nomis_get_metadata()
death <- nomis_get_data("NM_161_1",
  date = c("2016", "2015"),
  geography = "TYPE480",
  cause_of_death = c(10300, 102088, 270)
)

# All causes of death in London in 2016
london_death <- nomis_get_data("NM_161_1",
  date = c("2016"),
  geography = "2013265927", sex = 1, age = 0
)
#> Retrieving additional pages 1 of 2
#> Retrieving additional pages 2 of 2
# }
if (FALSE) { # \dontrun{
# Results in an error because `measure` is mistaken for `measures`
mort_data1 <- nomis_get_data(
  id = "NM_161_1", date = "2016",
  geography = "TYPE464", sex = 0, cause_of_death = "10381",
  age = 0, measure = 6
)

# Does not error because `measures` is specified
mort_data2 <- nomis_get_data(
  id = "NM_161_1", date = "2016",
  geography = "TYPE464", sex = 0, measures = NULL,
  cause_of_death = "10381", age = 0, measure = 6
)
} # }
```
