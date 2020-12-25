+++
date = 2020-12-24T06:00:00Z
description = "How much currencies were under-or over-value relative to the US dollar, %, from 2010 to 2020."
image = "/uploads/8.png"
tags = ["dataviz", "ggplot2", "100-ggplots"]
title = "#8 100-ggplots Slope Chart - Big Mac Index"

+++
Libraries:
```r
library(tidyverse)
library(lubridate)
library(showtext)
library(ggthemes)
theme_set(theme_tufte())
```

```r
big_mac <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-12-22/big-mac.csv')
```

```r
countries <- c("Argentina", "Switzerland", "Canada", "United States", "South Africa", "South Korea", "Thailand", "Euro area","China")

big_mac %>% 
  mutate(dollar_price = round(dollar_price, 2)) %>% 
  filter(name %in% countries, date == as.Date("2010-07-01") | date == as.Date("2020-07-01")) %>% 
  mutate(year = year(date)) %>% 
  select(year, name, dollar_price) %>% 
  mutate(raw_index = if_else(
    year == 2010, ((dollar_price * 100) / 3.73) - 100, ((dollar_price * 100) / 5.71) - 100
  )) %>% 
  select(-dollar_price) %>% 
  pivot_wider(names_from = year, values_from = raw_index) %>% 
  janitor::clean_names() -> big_mac2
  
```

```r
right_label <- paste(big_mac2$name, round(big_mac2$x2020, 2), sep =", ")
left_label <- paste(big_mac2$name, round(big_mac2$x2010, 2), sep =", ")
plot_colors <- c("#B0413E", "#02C3BD", "#F49E4C", "#697A21", "#8C429E", "#AE6F8F", "#5C52CB", "#08A045", "#0000E0")
```

```r
font_add_google(name = "IBM Plex Sans", family = "plex")
showtext_auto()
```

```r
plot <- ggplot(big_mac2) +
  geom_segment(aes(
    x = 1,
    y = x2010,
    xend = 2,
    yend = x2020
  ), color = plot_colors, size = 1, lineend = "round") +
  geom_text(label = left_label, aes(y = x2010, x = 1), hjust = 1.1, size = 4, family = "plex") +
  geom_text(label = right_label, aes(y = x2020, x = 2), hjust = -0.1, size = 4, family = "plex") +
  geom_text(label = "2010", aes(y = 80, x = 1), hjust = 1.1, size = 6, family = "plex") +
  geom_text(label = "2020", aes(y = 80, x = 2), hjust = -0.1, size = 6, family = "plex") +
  geom_vline(xintercept = 1, linetype = "dashed", size = .5, alpha = .5) +
  geom_vline(xintercept = 2, linetype = "dashed", size = .5, alpha = .5) + 
  scale_x_continuous(limits = c(.5, 2.5)) +
  scale_y_continuous(limits = c(-70, 80)) +
  labs(x = "", y = "",
       title = "Burgernomics from 2010 to 2020",
       subtitle = "How much currencies were under-or over-valued\nrelative to the US dollar, %, from 2010 to 2020.\n",
       caption = "Made by @luisfreii | Data: The Economist") +
  theme(
    aspect.ratio = 1,
    text = element_text(family = "plex"),
    axis.text.x = element_blank(),
    plot.title = element_text(size = 22),
    plot.subtitle = element_text(size = 11),
    plot.margin = margin(1,2,1,2, unit = "in")
  )
  
plot
```


```r
ggsave(
  plot = plot,
  filename = "7.png",
  dpi = 600,
  height = 12,
  width = 8
  
)
```


![](/uploads/8.png)