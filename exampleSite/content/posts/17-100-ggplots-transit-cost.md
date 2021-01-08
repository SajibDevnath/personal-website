+++
date = 2021-01-06T19:57:00Z
description = "Construction timelines for the most expensive transit-infrastructure projects in each country"
image = "/uploads/17.png"
tags = ["dataviz", "ggplot2", "100-ggplots"]
title = "#17 100-ggplots Transit Cost"

+++

Libraries:

```r
pacman::p_load(tidyverse, extrafont, countrycode)
extrafont::loadfonts(device = 'win',  quiet = TRUE)
theme_set(theme_minimal())
```



```r
transit_cost <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-01-05/transit_cost.csv')

```

```r
transit_cost %>% 
  filter(!is.na(country)) %>% 
  mutate(country_name = if_else(country != "UK", countrycode(country, origin = "iso2c", destination = "country.name", warn = FALSE), "United Kingdom")) %>% 
  group_by(country_name) %>% 
  summarise(mean_cost = mean(cost_km_millions)) %>% 
  arrange(-mean_cost) %>% 
  head(20) %>% 
  ggplot(aes(reorder(country_name, mean_cost), mean_cost)) +
  geom_col() +
  coord_flip()
```




```r
transit_cost %>% 
  filter(!is.na(country)) %>% 
  mutate(country_name = if_else(country != "UK", countrycode(country, origin = "iso2c", destination = "country.name", warn = FALSE), "United Kingdom")) %>% 
  group_by(country_name) %>% 
  top_n(1, cost_km_millions) %>% 
  ungroup() %>% 
  select(country_name, start_year, end_year, cost_km_millions) %>% 
  mutate_at(c("start_year", "end_year"), as.numeric) %>% 
  drop_na() -> transit_plot
  
```

```r
transit_plot %>% 
  arrange(-cost_km_millions) %>% 
  head(30) %>% 
  ggplot() +
  geom_segment(aes(x = start_year, xend = end_year, y = country_name, yend = country_name, color = cost_km_millions), size = 4) +
  scale_x_continuous(breaks = seq(2003, 2030, by = 3)) +
  scale_color_gradientn(
    colors = c("#F9F871", "#FFC75F", "#FF9671", "#FF6F91", "#D65DB1"),
    values = (c(100, 200, 250, 300, 400, 4000)/10000),
    name = "Cost in Millions"
  ) +
  labs(title = "Transit-infrastructure Costs", 
                              subtitle = "Construction timelines for the most expensive transit-infrastructure \nprojects in each country", x = "", y = "") +
  theme(
    legend.position = "none",
    text = element_text(size = 13, family = "Imperator"),
    plot.title = element_text(size = 22),
    plot.subtitle = element_text(size = 14),
    plot.margin = margin(1,1,1,.5, unit = "cm")
  )
```


![](/uploads/17.png)



From: [Nicola Rennie](https://github.com/nrennie/tidytuesday)



























