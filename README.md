Recreating Gapminder
================

``` r
library(ggplot2)
library(gapminder)
```

Here is some narrative.

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
