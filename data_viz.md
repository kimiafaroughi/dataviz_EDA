viz_1
================
Kimia Faroughi
2025-09-25

Import the weather data.

``` r
data("weather_df")
```

Making our first plot :-)

``` r
ggplot(data = weather_df, mapping = aes(x = tmin, y = tmax)) + 
  geom_point() #ppl never write data = or mapping = arguments
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_viz_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_viz_files/figure-gfm/unnamed-chunk-2-2.png)<!-- -->

``` r
weather_df |> #using pipe operator instead
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_viz_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
ggp_weather_scatterplot = 
  weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point()

ggp_weather_scatterplot #prints the plot
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_viz_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Check that some rows are missing.

``` r
weather_df |> 
  filter(is.na(tmax)) #lists na obs for tmax
```

    ## # A tibble: 17 × 6
    ##    name         id          date        prcp  tmax  tmin
    ##    <chr>        <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 Molokai_HI   USW00022534 2022-05-31    NA    NA    NA
    ##  2 Waterhole_WA USS0023B17S 2021-03-09    NA    NA    NA
    ##  3 Waterhole_WA USS0023B17S 2021-12-07    51    NA    NA
    ##  4 Waterhole_WA USS0023B17S 2021-12-31     0    NA    NA
    ##  5 Waterhole_WA USS0023B17S 2022-02-03     0    NA    NA
    ##  6 Waterhole_WA USS0023B17S 2022-08-09    NA    NA    NA
    ##  7 Waterhole_WA USS0023B17S 2022-08-10    NA    NA    NA
    ##  8 Waterhole_WA USS0023B17S 2022-08-11    NA    NA    NA
    ##  9 Waterhole_WA USS0023B17S 2022-08-12    NA    NA    NA
    ## 10 Waterhole_WA USS0023B17S 2022-08-13    NA    NA    NA
    ## 11 Waterhole_WA USS0023B17S 2022-08-14    NA    NA    NA
    ## 12 Waterhole_WA USS0023B17S 2022-08-15    NA    NA    NA
    ## 13 Waterhole_WA USS0023B17S 2022-08-16    NA    NA    NA
    ## 14 Waterhole_WA USS0023B17S 2022-08-17    NA    NA    NA
    ## 15 Waterhole_WA USS0023B17S 2022-08-18    NA    NA    NA
    ## 16 Waterhole_WA USS0023B17S 2022-08-19    NA    NA    NA
    ## 17 Waterhole_WA USS0023B17S 2022-12-31    76    NA    NA

## Fancier scatterplots!

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax, color = name)) + #gives different color for each name variable in dataset
  geom_point(alpha = 0.3) + #makes points a little see through
  geom_smooth(se = FALSE) #adds smooth lines for each subcategory and se = FALSE removes suspect error band (don't trust them)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_viz_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Where you define aesthetics can matter.

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point(aes(color = name, size = prcp), alpha = 0.3) + #changes size of every point based on precipitation var
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_viz_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
#produces only one smooth line (not divided by subcategory) because moved the aesthetic of color to geom_point rather than the main ggplot line
#communicate one message at a time with one or two aesthetic mappings, don't need to include lots of different options because you want your "audience" to understand the plot
```

Use faceting.

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point(aes(color = name), alpha = 0.3, size = 0.8) +
  geom_smooth(se = FALSE) +
  facet_grid(. ~ name) #separates into different plots based on variable name (by column)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_viz_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point(aes(color = name), alpha = 0.3, size = 0.8) +
  geom_smooth(se = FALSE) +
  facet_grid(name ~ .) #separates into different plots based on variable name (by row)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).
    ## Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_viz_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->

Let’s make a somewhat more interesting scatterplot.

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax, color = name, shape = name)) + #shape = name creates different shapes as the points based on variable name
  geom_point(aes(size = prcp), alpha = 0.3) +
  geom_smooth(se = FALSE) +
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](data_viz_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

Learning Assessment

``` r
weather_df |> 
  filter(name == "CentralPark_NY") |> 
  mutate(
    tmax_F = (tmax * (9/5)) + 32,
    tmin_F = (tmin * (9/5)) + 32
  ) |> 
  ggplot(aes(x = tmin_F, y = tmax_F)) +
  geom_point() +
  geom_smooth(se = FALSE, method = "lm") #adds linear regression line
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](data_viz_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->
