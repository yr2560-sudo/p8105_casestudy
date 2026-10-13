p8105_casestudy
================
Yiman Rui
2026-10-13

``` r
library(p8105.datasets)
library(tidyverse)
```

    ## Warning: package 'tibble' was built under R version 4.5.2

    ## Warning: package 'dplyr' was built under R version 4.5.2

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.1.5
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.0     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggplot2)
data(nyc_airbnb)
```

# Some questions

- where are the most expensive airbnbs in nyc?
- do description correlate with price?
- what about text reviews?

``` r
nyc_airbnb |>
  filter(neighbourhood_group =="Manhattan") |>
  count(room_type, neighbourhood) |>
  pivot_wider(
    names_from = room_type,
    values_from = n
  )
```

    ## # A tibble: 32 × 4
    ##    neighbourhood      `Entire home/apt` `Private room` `Shared room`
    ##    <chr>                          <int>          <int>         <int>
    ##  1 Battery Park City                 44             19             2
    ##  2 Chelsea                          774            274            24
    ##  3 Chinatown                        195            164             1
    ##  4 Civic Center                      25             17            NA
    ##  5 East Harlem                      433            558            57
    ##  6 East Village                    1093            742            23
    ##  7 Financial District               229            145            17
    ##  8 Flatiron District                 77             12             1
    ##  9 Gramercy                         185            116             6
    ## 10 Greenwich Village                287             93             3
    ## # ℹ 22 more rows

# price

``` r
nyc_airbnb |>
  group_by(neighbourhood_group) |>
  mutate(price_rank = min_rank(desc(price))) |>
  filter(price < 3)
```

    ## # A tibble: 0 × 18
    ## # Groups:   neighbourhood_group [0]
    ## # ℹ 18 variables: id <dbl>, review_scores_location <dbl>, name <chr>,
    ## #   host_id <dbl>, host_name <chr>, neighbourhood_group <chr>,
    ## #   neighbourhood <chr>, lat <dbl>, long <dbl>, room_type <chr>, price <dbl>,
    ## #   minimum_nights <dbl>, number_of_reviews <dbl>, last_review <date>,
    ## #   reviews_per_month <dbl>, calculated_host_listings_count <dbl>,
    ## #   availability_365 <dbl>, price_rank <int>

``` r
nyc_airbnb |>
  filter(price < 1000) |>
  ggplot(aes(x=price)) +
  geom_histogram() +
  facet_grid(room_type ~ neighbourhood)
```

    ## `stat_bin()` using `bins = 30`. Pick better value `binwidth`.

![](p8105_casestudy_files/figure-gfm/unnamed-chunk-4-1.png)<!-- --> \##
Host stuff

``` r
nyc_airbnb |>
  filter(neighbourhood_group == "Manhattan") |>
  filter(price < 500) |>
  ggplot(aes (x = neighbourhood, y = price)) +
  geom_boxplot()
```

![](p8105_casestudy_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->
