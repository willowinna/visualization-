Visualization
================

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggridges)

library(p8105.datasets)
data("weather_df")
```

Now we have everything we need.

``` r
weather_df
```

    ## # A tibble: 2,190 × 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1  
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6
    ## # ℹ 2,180 more rows

scatter plot

``` r
ggplot(weather_df, aes(x = tmin, y = tmax))
```

![](01_viz_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) +
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](01_viz_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
ggp_temp_scatterplot =
  weather_df |>
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point()

ggp_temp_scatterplot
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](01_viz_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

color label(colour = ), transparent (alpha = ), smooth curve(geom_smooth
= )

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax, colour = name)) + 
  geom_point(alpha = 0.25)+
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](01_viz_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

\#把`colour/color`放到`ggplot(aes())`：**点和拟合线都会按 name
分组，多条线**
\#把`colour/color`放到`geom_point(aes())`：**只有点分组上色，平滑曲线用全部数据画一条**

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name),alpha = 0.25)+
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](01_viz_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Show faceting(separate by …)

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax, colour = name)) + 
  geom_point(alpha = 0.5)+
  facet_grid(. ~ name)
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](01_viz_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax, colour = name)) + 
  geom_point(alpha = 0.5)+
  facet_grid(name ~ .)
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](01_viz_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax, colour = name)) + 
  geom_point(alpha = 0.5)+
  facet_grid(cols = vars(name))
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](01_viz_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

let’s look at something else.

``` r
weather_df |>
  ggplot(aes(x = date, y = tmax, colour = name)) + 
  geom_point(aes(size = prcp),alpha = 0.5) +
  geom_smooth(se = FALSE) +
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](01_viz_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
weather_df |>
  filter(name == "CentralPark_NY") |>
  mutate(
    tmax = tmax * (9 / 5) + 32,
    tmax = tmax * (9 / 5) + 32

  ) |>
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point()
```

![](01_viz_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->
`mutate()`：**新增列，或者原地修改现有列**，返回完整的数据集（保留原来所有列）

what’s a hexplot

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax)) +
  geom_hex()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_binhex()`).

![](01_viz_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

## univariate plots

``` r
weather_df |>
  ggplot(aes(x = tmax)) +
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](01_viz_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmax, fill = name)) +
  geom_histogram(pos = "dodge")
```

    ## `stat_bin()` using `bins = 30`. Pick better value `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](01_viz_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmax, fill = name)) +
  geom_histogram() + 
  facet_grid(. ~ name)
```

    ## `stat_bin()` using `bins = 30`. Pick better value `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](01_viz_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

Density plots

``` r
weather_df |>
  ggplot(aes(x = tmax, colour = name)) +
  geom_density()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](01_viz_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

fill 填色

``` r
weather_df |>
  ggplot(aes(x = tmax, fill = name)) +
  geom_density(alpha = 0.3)
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](01_viz_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

boxplot

``` r
weather_df |>
  ggplot(aes(y = tmax)) +
  geom_boxplot()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](01_viz_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

加x会根据x生成多个

``` r
weather_df |>
  ggplot(aes(x = name, y = tmax)) +
  geom_boxplot()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](01_viz_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

Violin

``` r
weather_df |>
  ggplot(aes(x = name, y = tmax)) +
  geom_violin()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_ydensity()`).

![](01_viz_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

ridge plots

``` r
weather_df |>
  ggplot(aes(x = tmax, y = name)) +
  geom_density_ridges()
```

    ## Picking joint bandwidth of 1.54

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_density_ridges()`).

![](01_viz_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

\##save some of my plots

``` r
ggp_weather =
  weather_df |>
  ggplot(aes(x = date, y = tmax, colour = name)) +
  geom_point(aes(size = prcp), alpha = 0.5) +
  facet_grid(. ~ name)

ggsave("images/ggp_weather.pdf",ggp_weather)
```

    ## Saving 7 x 5 in image

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

``` r
weather_df |>
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](01_viz_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->
