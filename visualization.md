visualization
================
Wenzhao Wu
10/5/2020

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.2     v purrr   0.3.4
    ## v tibble  3.0.3     v dplyr   1.0.2
    ## v tidyr   1.1.2     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.5.0

    ## -- Conflicts ------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(ggridges)

knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = 1,
  out.width = "90%"
)
```

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: C:\Users\WUWENZHAO\AppData\Local\cache/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2020-09-12 17:07:45 (7.533)

    ## file min/max dates: 1869-01-01 / 2020-09-30

    ## using cached file: C:\Users\WUWENZHAO\AppData\Local\cache/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2020-10-05 23:57:09 (1.703)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: C:\Users\WUWENZHAO\AppData\Local\cache/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2020-09-12 17:10:03 (0.879)

    ## file min/max dates: 1999-09-01 / 2020-09-30

``` r
weather_df
```

    ## # A tibble: 1,095 x 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2017-01-01     0   8.9   4.4
    ##  2 CentralPark_NY USW00094728 2017-01-02    53   5     2.8
    ##  3 CentralPark_NY USW00094728 2017-01-03   147   6.1   3.9
    ##  4 CentralPark_NY USW00094728 2017-01-04     0  11.1   1.1
    ##  5 CentralPark_NY USW00094728 2017-01-05     0   1.1  -2.7
    ##  6 CentralPark_NY USW00094728 2017-01-06    13   0.6  -3.8
    ##  7 CentralPark_NY USW00094728 2017-01-07    81  -3.2  -6.6
    ##  8 CentralPark_NY USW00094728 2017-01-08     0  -3.8  -8.8
    ##  9 CentralPark_NY USW00094728 2017-01-09     0  -4.9  -9.9
    ## 10 CentralPark_NY USW00094728 2017-01-10     0   7.8  -6  
    ## # ... with 1,085 more rows

# Basic scatterplot

``` r
ggplot(weather_df, aes(x = tmin, y = tmax))
```

<img src="visualization_files/figure-gfm/unnamed-chunk-2-1.png" width="90%" />

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="visualization_files/figure-gfm/unnamed-chunk-3-1.png" width="90%" />

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="visualization_files/figure-gfm/unnamed-chunk-4-1.png" width="90%" />

# Advanced scatterplot

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name))
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="visualization_files/figure-gfm/unnamed-chunk-5-1.png" width="90%" />

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = 0.5) +
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="visualization_files/figure-gfm/unnamed-chunk-6-1.png" width="90%" />

``` r
ggplot(weather_df, aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  geom_smooth(se = FALSE) + 
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="visualization_files/figure-gfm/unnamed-chunk-7-1.png" width="90%" />

``` r
ggplot(weather_df, aes(x = date, y = tmax, color = name)) + 
  geom_point(aes(size = prcp), alpha = .5) +
  geom_smooth(se = FALSE) + 
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

<img src="visualization_files/figure-gfm/unnamed-chunk-8-1.png" width="90%" />

``` r
weather_df %>%
  mutate(
    tmin = 9 * tmin / 5 + 32,
    tmax = 9 * tmax / 5 + 32
  )
```

    ## # A tibble: 1,095 x 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2017-01-01     0  48.0  39.9
    ##  2 CentralPark_NY USW00094728 2017-01-02    53  41    37.0
    ##  3 CentralPark_NY USW00094728 2017-01-03   147  43.0  39.0
    ##  4 CentralPark_NY USW00094728 2017-01-04     0  52.0  34.0
    ##  5 CentralPark_NY USW00094728 2017-01-05     0  34.0  27.1
    ##  6 CentralPark_NY USW00094728 2017-01-06    13  33.1  25.2
    ##  7 CentralPark_NY USW00094728 2017-01-07    81  26.2  20.1
    ##  8 CentralPark_NY USW00094728 2017-01-08     0  25.2  16.2
    ##  9 CentralPark_NY USW00094728 2017-01-09     0  23.2  14.2
    ## 10 CentralPark_NY USW00094728 2017-01-10     0  46.0  21.2
    ## # ... with 1,085 more rows

``` r
centralpark = 
  subset(weather_df, name %in% ("CentralPark_NY"))

ggplot(centralpark,aes(x = tmax, y = tmin, color = name)) + 
  geom_point() +
  geom_smooth(se = FALSE, color = "blue")
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

<img src="visualization_files/figure-gfm/unnamed-chunk-9-1.png" width="90%" />

# Odds and ends

``` r
ggplot(weather_df, aes(x = tmax, y = tmin)) + 
  geom_hex()
```

    ## Warning: Removed 15 rows containing non-finite values (stat_binhex).

<img src="visualization_files/figure-gfm/unnamed-chunk-10-1.png" width="90%" />

``` r
ggplot(weather_df) + geom_point(aes(x = tmax, y = tmin), color = "blue")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="visualization_files/figure-gfm/unnamed-chunk-11-1.png" width="90%" />

``` r
ggplot(weather_df) + geom_point(aes(x = tmax, y = tmin, color = "blue"))
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="visualization_files/figure-gfm/unnamed-chunk-11-2.png" width="90%" />

# Univariate plots

``` r
ggplot(weather_df, aes(x = tmax)) + 
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 3 rows containing non-finite values (stat_bin).

<img src="visualization_files/figure-gfm/unnamed-chunk-12-1.png" width="90%" />

``` r
ggplot(weather_df, aes(x = tmax, fill = name)) + 
  geom_histogram(position = "dodge", binwidth = 2)
```

    ## Warning: Removed 3 rows containing non-finite values (stat_bin).

<img src="visualization_files/figure-gfm/unnamed-chunk-13-1.png" width="90%" />

``` r
ggplot(weather_df, aes(x = tmax, fill = name)) + 
  geom_density(alpha = .4, adjust = 0.4, color = "blue") +
  geom_rug()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_density).

<img src="visualization_files/figure-gfm/unnamed-chunk-14-1.png" width="90%" />

``` r
ggplot(weather_df, aes(x = name, y = tmax)) + 
  geom_boxplot()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

<img src="visualization_files/figure-gfm/unnamed-chunk-15-1.png" width="90%" />

``` r
ggplot(weather_df, aes(x = name, y = tmax)) + 
  geom_violin(aes(fill = name), alpha = .5) + 
  stat_summary(fun = "median", color = "blue")
```

    ## Warning: Removed 3 rows containing non-finite values (stat_ydensity).

    ## Warning: Removed 3 rows containing non-finite values (stat_summary).

    ## Warning: Removed 3 rows containing missing values (geom_segment).

<img src="visualization_files/figure-gfm/unnamed-chunk-16-1.png" width="90%" />

``` r
ggplot(weather_df, aes(x = tmax, y = name)) + 
  geom_density_ridges(scale = .85)
```

    ## Picking joint bandwidth of 1.84

    ## Warning: Removed 3 rows containing non-finite values (stat_density_ridges).

<img src="visualization_files/figure-gfm/unnamed-chunk-17-1.png" width="90%" />

# Assessment

``` r
ggplot(weather_df, aes(x = prcp, fill = name)) + 
  geom_histogram(position = "dodge", binwidth = 5, breaks = seq(0,945,50)) +
  scale_y_continuous(
    trans = "sqrt")
```

    ## Warning: Removed 3 rows containing non-finite values (stat_bin).

<img src="visualization_files/figure-gfm/unnamed-chunk-18-1.png" width="90%" />

``` r
ggplot(weather_df, aes(x = prcp, fill = name)) + 
  geom_density(alpha = .4, adjust = 0.4, color = "yellow") +
  scale_y_continuous(
    trans = "sqrt")
```

    ## Warning: Removed 3 rows containing non-finite values (stat_density).

<img src="visualization_files/figure-gfm/unnamed-chunk-18-2.png" width="90%" />

``` r
ggplot(weather_df, aes(x = name, y = prcp)) + 
  geom_boxplot(outlier.shape = 5, outlier.size = 1) +
  scale_y_continuous(
    trans = "sqrt")
```

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

<img src="visualization_files/figure-gfm/unnamed-chunk-18-3.png" width="90%" />

``` r
ggplot(weather_df, aes(x = name, y = prcp)) + 
  geom_violin(aes(fill = name), alpha = .5) + 
  stat_summary(fun = "median", color = "blue") +
  scale_y_continuous(
    trans = "sqrt")
```

    ## Warning: Removed 3 rows containing non-finite values (stat_ydensity).

    ## Warning: Removed 3 rows containing non-finite values (stat_summary).

    ## Warning: Removed 3 rows containing missing values (geom_segment).

<img src="visualization_files/figure-gfm/unnamed-chunk-18-4.png" width="90%" />

``` r
ggplot(weather_df, aes(x = prcp, y = name)) + 
  geom_density_ridges(scale = .85)
```

    ## Picking joint bandwidth of 4.61

    ## Warning: Removed 3 rows containing non-finite values (stat_density_ridges).

<img src="visualization_files/figure-gfm/unnamed-chunk-18-5.png" width="90%" />

# Saving and embedding plots

``` r
weather_plot = ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) 

ggsave("weather_plot.pdf", weather_plot, width = 8, height = 5)
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

``` r
weather_plot
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

<img src="visualization_files/figure-gfm/unnamed-chunk-20-1.png" width="90%" />
