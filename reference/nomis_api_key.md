# Nomis API Key

Assign or reassign API key for Nomis.

## Usage

``` r
nomis_api_key(check_env = FALSE)
```

## Arguments

- check_env:

  If TRUE, will check the environment variable `NOMIS_API_KEY` first
  before asking for user input.

## Details

The Nomis API has an optional key. Using the key means that 100,000 rows
can be returned per call, which can speed up larger data requests and
reduce the chances of being rate limited or having requests timing out.

By default, `nomisr` will look for the environment variable
`NOMIS_API_KEY` when the package is loaded. If found, the API key will
be stored in the session option `nomisr.API.key`. If you would like to
reload the API key or would like to manually enter one in, this function
may be used.

You can sign up for an API key
[here](https://www.nomisweb.co.uk/myaccount/userjoin.asp).
