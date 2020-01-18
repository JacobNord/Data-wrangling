---
title: 'Case Study 2: Wealth and Life Expectancy (Gapminder)'
author: "Jacob Nordstrom"
date: "January 16, 2020"
output: 
  html_document:
    keep_md: true
---



## Backhround

The primary thing I learned while making these graphs was how you can overlay things on ggplot. I didn't fully grasp how much better ggplot was than base R until doing this case study. The versatility of ggplot is amazing.

## Images


```r
gapminder %>%
  filter(country != "Kuwait") %>% 
  ggplot() + 
  geom_point(mapping = aes(x = lifeExp, y = gdpPercap, color = continent, size = pop)) +
  facet_wrap(~ year, nrow = 1) +
  theme_bw() +
  scale_y_continuous(trans = "sqrt") +
  labs(x = "Life Expectancy", y = "GDP per Capita")
```

![](caseStudy2_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

```r
ggsave("Plot1.png", device = "png", width = 15, units = "in")
```

```
## Saving 15 x 5 in image
```


```r
gapminder2 <- gapminder %>% 
  group_by(year, continent) %>% 
  summarise(avg = weighted.mean(x = gdpPercap, w = pop), 
            avgpop = weighted.mean(pop))

gapminder %>% 
  filter(country != "Kuwait") %>% 
  ggplot(mapping = aes(x = year, y = gdpPercap, color = continent)) + 
  geom_point() +
  geom_line(mapping = aes(x = year, y = gdpPercap, color = continent, group = country)) +
  theme_bw() +
  facet_wrap (~ continent, nrow = 1) +
  scale_y_continuous(trans = "sqrt") +
  labs(x = "Year", y = "GDP per Capita") + 
  geom_line( data = gapminder2, mapping = aes(x = year, y = avg), color = "black")+
  geom_point( data =  gapminder2, mapping = aes(x = year, y = avg, size = avgpop), color = "black") +
  guides(color = guide_legend("Continent"), size = guide_legend( "Population (100k)"))
```

![](caseStudy2_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
ggsave("Plot2.png", device = "png", width = 15, units = "in")
```

```
## Saving 15 x 5 in image
```

