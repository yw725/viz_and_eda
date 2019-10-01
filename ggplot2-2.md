ggplot2-2
================
Yuning Wang
10/1/2019

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(c("USW00094728", "USC00519397", "USS0023B17S"),
                      var = c("PRCP", "TMIN", "TMAX"), 
                      date_min = "2017-01-01",
                      date_max = "2017-12-31") %>%
  mutate(
    name = recode(id, USW00094728 = "CentralPark_NY", 
                      USC00519397 = "Waikiki_HA",
                      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'crul':
    ##   method                 from
    ##   as.character.form_file httr

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## file path:          /Users/yuning_wang/Library/Caches/rnoaa/ghcnd/USW00094728.dly

    ## file last updated:  2019-09-26 10:36:43

    ## file min/max dates: 1869-01-01 / 2019-09-30

    ## file path:          /Users/yuning_wang/Library/Caches/rnoaa/ghcnd/USC00519397.dly

    ## file last updated:  2019-09-26 10:36:52

    ## file min/max dates: 1965-01-01 / 2019-09-30

    ## file path:          /Users/yuning_wang/Library/Caches/rnoaa/ghcnd/USS0023B17S.dly

    ## file last updated:  2019-09-26 10:36:55

    ## file min/max dates: 1999-09-01 / 2019-09-30

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
    ## # … with 1,085 more rows

## Making new plots

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5)
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

add labels :

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5)+
  labs(
    title = "Temperature plot",
    x = "Minimum Temp (C)",
    y = "Maximum temp (C)",
    caption = "Data from the rnoaa package"
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

x axis tick marks:

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5)+
  labs(
    title = "Temperature plot",
    x = "Minimum Temp (C)",
    y = "Maximum temp (C)",
    caption = "Data from the rnoaa package"
  ) +
  scale_x_continuous(
    breaks = c(-15, 0, 15), 
    labels = c("-15º C", "0", "15")) +
  scale_y_continuous(
    trans = "sqrt", 
    position = "right"
  )
```

    ## Warning in self$trans$transform(x): 产生了NaNs

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 90 rows containing missing values (geom_point).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

## Change the color

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) + 
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    caption = "Data from the rnoaa package") +
  scale_color_hue(
    name = "Location",
    h = c(100, 200)
  )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

the viridis package

``` r
ggp_base = 
  weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = .5) + 
  labs(
    title = "Temperature plot",
    x = "Minimum daily temperature (C)",
    y = "Maxiumum daily temperature (C)",
    caption = "Data from the rnoaa package"
  ) + 
  viridis::scale_color_viridis(
    name = "Location", 
    discrete = TRUE
  )

ggp_base
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Themes

``` r
ggp_base +
  theme_bw() +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
ggp_base +
  theme_minimal() +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-6-2.png)<!-- -->

## More than one dataset

``` r
central_park = 
  weather_df %>%
  filter(name == "CentralPark_NY")

waikiki = 
  weather_df %>%
  filter(name == "Waikiki_HA")

ggplot(data = waikiki, aes(x = date, y = tmax,
                           color = name)) +
  geom_point() +
  geom_line(data = central_park)
```

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

brief aside about colors

``` r
waikiki %>%
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point()
```

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
waikiki %>%
  ggplot(aes(x = date, y = tmax), color = red) +
  geom_point()
```

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->

## Multi-panel plots

``` r
ggp_scatter = 
  weather_df %>%
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point()

ggp_density = 
  weather_df %>%
  ggplot(aes(x = tmin)) +
  geom_density()

ggp_box = 
  weather_df %>%
  ggplot(aes(x = name, y = tmax)) +
  geom_boxplot()

(ggp_scatter + ggp_box )/ ggp_density
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

    ## Warning: Removed 15 rows containing non-finite values (stat_density).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

## Data manipulation

``` r
weather_df %>%
  mutate(
    name = factor(name),
    name = fct_relevel(name, "Waikiki_HA",
                       "CentralPark, NY")
  ) %>%
  ggplot(aes(x = name, y = tmax, color = name)) +
  geom_boxplot()
```

    ## Warning: Unknown levels in `f`: CentralPark, NY

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

``` r
weather_df %>%
  mutate(
    name = factor(name),
    name = fct_reorder(name, tmax)
  ) %>%
  ggplot(aes(x = name, y = tmax, color = name)) +
  geom_boxplot()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-10-2.png)<!-- -->

``` r
weather_df %>%
  mutate(name = forcats::fct_relevel(name, c("Waikiki_HA", "CentralPark_NY", "Waterhole_WA"))) %>% 
  ggplot(aes(x = name, y = tmax)) + 
  geom_violin(aes(fill = name), color = "blue", alpha = .5) + 
  theme(legend.position = "bottom")
```

    ## Warning: Removed 3 rows containing non-finite values (stat_ydensity).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-10-3.png)<!-- -->

## Restructure then plot

``` r
weather_df %>%
  select(name, tmax, tmin) %>% 
  pivot_longer(
    tmax:tmin,
    names_to = "observation", 
    values_to = "temp") %>% 
  ggplot(aes(x = temp, fill = observation)) +
  geom_density(alpha = .5) + 
  facet_grid(~name) + 
  viridis::scale_fill_viridis(discrete = TRUE)
```

    ## Warning: Removed 18 rows containing non-finite values (stat_density).

![](ggplot2-2_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

Example

``` r
pup_data = 
  read_csv("./data/FAS_pups.csv", col_types = "ciiiii") %>%
  janitor::clean_names() %>%
  mutate(sex = recode(sex, `1` = "male", `2` = "female")) 

litter_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  separate(group, into = c("dose", "day_of_tx"), sep = 3) %>%
  mutate(wt_gain = gd18_weight - gd0_weight,
         day_of_tx = as.numeric(day_of_tx))

fas_data = left_join(pup_data, litter_data, by = "litter_number") 

fas_data %>% 
  select(sex, dose, day_of_tx, pd_ears:pd_walk) %>% 
  pivot_longer(
    pd_ears:pd_walk,
    names_to = "outcome", 
    values_to = "pn_day") %>% 
  drop_na() %>% 
  mutate(outcome = forcats::fct_reorder(outcome, day_of_tx, median)) %>% 
  ggplot(aes(x = dose, y = pn_day)) + 
  geom_violin() + 
  facet_grid(day_of_tx ~ outcome)
```

![](ggplot2-2_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->
