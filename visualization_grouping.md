visualization\_grouping
================
Jiayi Shen
10/04/2018

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(c("USW00094728", "USC00519397", "USS0023B17S"),
                      var = c("PRCP", "TMIN", "TMAX"), 
                      date_min = "2016-01-01",
                      date_max = "2016-12-31") %>%
  mutate(
    name = recode(id, USW00094728 = "CentralPark_NY", 
                      USC00519397 = "Waikiki_HA",
                      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10,
    month = lubridate::floor_date(date, unit = "month")) %>%
  select(name, id, date, month, everything())
```

    ## file path:          /Users/s/Library/Caches/rnoaa/ghcnd/USW00094728.dly

    ## file last updated:  2018-09-26 23:31:54

    ## file min/max dates: 1869-01-01 / 2018-09-30

    ## file path:          /Users/s/Library/Caches/rnoaa/ghcnd/USC00519397.dly

    ## file last updated:  2018-09-26 23:32:07

    ## file min/max dates: 1965-01-01 / 2018-09-30

    ## file path:          /Users/s/Library/Caches/rnoaa/ghcnd/USS0023B17S.dly

    ## file last updated:  2018-09-26 23:32:12

    ## file min/max dates: 1999-09-01 / 2018-09-30

`group_by`

``` r
weather_df %>%
  group_by(name, month) 
```

    ## # A tibble: 1,098 x 7
    ## # Groups:   name, month [36]
    ##    name           id          date       month       prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2016-01-01 2016-01-01     0   5.6   1.1
    ##  2 CentralPark_NY USW00094728 2016-01-02 2016-01-01     0   4.4   0  
    ##  3 CentralPark_NY USW00094728 2016-01-03 2016-01-01     0   7.2   1.7
    ##  4 CentralPark_NY USW00094728 2016-01-04 2016-01-01     0   2.2  -9.9
    ##  5 CentralPark_NY USW00094728 2016-01-05 2016-01-01     0  -1.6 -11.6
    ##  6 CentralPark_NY USW00094728 2016-01-06 2016-01-01     0   5    -3.8
    ##  7 CentralPark_NY USW00094728 2016-01-07 2016-01-01     0   7.8  -0.5
    ##  8 CentralPark_NY USW00094728 2016-01-08 2016-01-01     0   7.8  -0.5
    ##  9 CentralPark_NY USW00094728 2016-01-09 2016-01-01     0   8.3   4.4
    ## 10 CentralPark_NY USW00094728 2016-01-10 2016-01-01   457  15     4.4
    ## # ... with 1,088 more rows

Counting things

``` r
weather_df %>%
  group_by(month) %>%
  summarize(n = n())
```

    ## # A tibble: 12 x 2
    ##    month          n
    ##    <date>     <int>
    ##  1 2016-01-01    93
    ##  2 2016-02-01    87
    ##  3 2016-03-01    93
    ##  4 2016-04-01    90
    ##  5 2016-05-01    93
    ##  6 2016-06-01    90
    ##  7 2016-07-01    93
    ##  8 2016-08-01    93
    ##  9 2016-09-01    90
    ## 10 2016-10-01    93
    ## 11 2016-11-01    90
    ## 12 2016-12-01    93

The fact that summarize() produces a dataframe is important (and consistent with other functions in the tidyverse). You can incorporate grouping and summarizing within broader analysis pipelines.

try
