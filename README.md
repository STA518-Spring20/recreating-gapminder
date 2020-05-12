Recreating Gapminder
================

``` r
library(ggplot2)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(gapminder)
```

## The Visualization

``` r
ggplot(data = gapminder) +
  geom_point(mapping = aes(x = gdpPercap, y = lifeExp, size = pop, color = continent), alpha = 0.5) +
  labs(x = "Income",
       y = "Life Expectancy",
       color = "World Region",
       size = "Population",
       title = "Gapminder Bubble Chart",
       subtitle = "For all years")
```

![](README_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

## More with dplyr

### `filter()`, `select()`, and `arrange()`

``` r
gapminder
```

    ## # A tibble: 1,704 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # … with 1,694 more rows

Create a copy of gapminder.

``` r
my_gm <- gapminder
my_gm
```

    ## # A tibble: 1,704 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # … with 1,694 more rows

View only USA

``` r
my_gm %>% 
  filter(country == "United States")
```

    ## # A tibble: 12 x 6
    ##    country       continent  year lifeExp       pop gdpPercap
    ##    <fct>         <fct>     <int>   <dbl>     <int>     <dbl>
    ##  1 United States Americas   1952    68.4 157553000    13990.
    ##  2 United States Americas   1957    69.5 171984000    14847.
    ##  3 United States Americas   1962    70.2 186538000    16173.
    ##  4 United States Americas   1967    70.8 198712000    19530.
    ##  5 United States Americas   1972    71.3 209896000    21806.
    ##  6 United States Americas   1977    73.4 220239000    24073.
    ##  7 United States Americas   1982    74.6 232187835    25010.
    ##  8 United States Americas   1987    75.0 242803533    29884.
    ##  9 United States Americas   1992    76.1 256894189    32004.
    ## 10 United States Americas   1997    76.8 272911760    35767.
    ## 11 United States Americas   2002    77.3 287675526    39097.
    ## 12 United States Americas   2007    78.2 301139947    42952.

Plot `gdpPercap` over time (`year`) for only USA

``` r
my_gm %>% 
  filter(country == "United States") %>% 
  ggplot(aes(x = year, y = gdpPercap)) +
  geom_line()
```

![](README_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Subset of gapminder with only USA

``` r
us_gm <- my_gm %>% 
  filter(country == "United States")
```

View only year, life expectancy, and gdp percap only for USA

``` r
us_gm %>% 
  select(year, lifeExp, gdpPercap) %>% 
  arrange(desc(lifeExp))
```

    ## # A tibble: 12 x 3
    ##     year lifeExp gdpPercap
    ##    <int>   <dbl>     <dbl>
    ##  1  2007    78.2    42952.
    ##  2  2002    77.3    39097.
    ##  3  1997    76.8    35767.
    ##  4  1992    76.1    32004.
    ##  5  1987    75.0    29884.
    ##  6  1982    74.6    25010.
    ##  7  1977    73.4    24073.
    ##  8  1972    71.3    21806.
    ##  9  1967    70.8    19530.
    ## 10  1962    70.2    16173.
    ## 11  1957    69.5    14847.
    ## 12  1952    68.4    13990.

Reorgaize US subset with life expectancy first and drop continent

``` r
us_gm %>% 
  select(lifeExp, everything(), -continent)
```

    ## # A tibble: 12 x 5
    ##    lifeExp country        year       pop gdpPercap
    ##      <dbl> <fct>         <int>     <int>     <dbl>
    ##  1    68.4 United States  1952 157553000    13990.
    ##  2    69.5 United States  1957 171984000    14847.
    ##  3    70.2 United States  1962 186538000    16173.
    ##  4    70.8 United States  1967 198712000    19530.
    ##  5    71.3 United States  1972 209896000    21806.
    ##  6    73.4 United States  1977 220239000    24073.
    ##  7    74.6 United States  1982 232187835    25010.
    ##  8    75.0 United States  1987 242803533    29884.
    ##  9    76.1 United States  1992 256894189    32004.
    ## 10    76.8 United States  1997 272911760    35767.
    ## 11    77.3 United States  2002 287675526    39097.
    ## 12    78.2 United States  2007 301139947    42952.

rename variables

``` r
us_gm %>% 
  select(life_exp = lifeExp, everything())
```

    ## # A tibble: 12 x 6
    ##    life_exp country       continent  year       pop gdpPercap
    ##       <dbl> <fct>         <fct>     <int>     <int>     <dbl>
    ##  1     68.4 United States Americas   1952 157553000    13990.
    ##  2     69.5 United States Americas   1957 171984000    14847.
    ##  3     70.2 United States Americas   1962 186538000    16173.
    ##  4     70.8 United States Americas   1967 198712000    19530.
    ##  5     71.3 United States Americas   1972 209896000    21806.
    ##  6     73.4 United States Americas   1977 220239000    24073.
    ##  7     74.6 United States Americas   1982 232187835    25010.
    ##  8     75.0 United States Americas   1987 242803533    29884.
    ##  9     76.1 United States Americas   1992 256894189    32004.
    ## 10     76.8 United States Americas   1997 272911760    35767.
    ## 11     77.3 United States Americas   2002 287675526    39097.
    ## 12     78.2 United States Americas   2007 301139947    42952.

Show the entries for Burundi after 1996 for only the variables `yr`,
`life_exp`, and `pop`.

``` r
my_gm %>% 
  filter(country == "Burundi" & year > 1996) %>% 
  select(yr = year, life_exp = lifeExp, pop)
```

    ## # A tibble: 3 x 3
    ##      yr life_exp     pop
    ##   <int>    <dbl>   <int>
    ## 1  1997     45.3 6121610
    ## 2  2002     47.4 7021078
    ## 3  2007     49.6 8390505
