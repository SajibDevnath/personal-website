+++
date = 2020-12-24T06:00:00Z
description = "How much currencies were under-or over-value relative to the US dollar, %, from 2010 to 2020."
image = "/uploads/8.png"
tags = ["dataviz", "ggplot2", "100-ggplots"]
title = "#8 100-ggplots Slope Chart - Big Mac Index"

+++
Library:

```r
  
library(ggplot2)
library(tidyverse)
library(ggtext)
library(forcats)
library(extrafont)
library(showtext)
```

Fonts:

```r
font_add_google(name = "PT Serif", family = "pt")
font_add_google(name = "Marvel", family = "marvel")
showtext_auto()
```

Data:

```r
members <- data.table::fread('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-22/members.csv')
```

## Preparing the data:

```r
members %>% 
  filter(peak_name == "Everest") %>% 
  group_by(sex, season, solo, died, year, oxygen_used) %>% 
  summarise(nums = n()) -> dfo

dfo$oxygen_used <- fct_rev(as.factor(dfo$oxygen_used))
  
```

## Visualization:

```r

ggplot(dfo, aes(x = year, y = nums, fill = oxygen_used)) + 
  geom_bar(stat = "identity", alpha = 0.8, width = .8) + 
  xlim(1924, 2019) +
  scale_fill_manual(values = c("#393b44", "white"), labels = c("YES", "NO")) +
  scale_y_continuous(position = "right", expand = c(0, 0)) + 
  theme_minimal() + 
  labs(title = "Everest, give me air!",
       subtitle = "Every member of every expedition (failed or not) \nto the mount Everest, by Oxygen used or not",
       x = "",
       y = "members",
       fill = "Oxygen?",
       caption = "Data from The Himalayan Database | @LauraNavarroSol") +
  theme(
    plot.background = element_rect(fill = "#80bdab", color = NA),
    legend.position = "bottom",
    panel.grid = element_blank(),
    text = element_text(family = "marvel", size = 10, face = "bold"),
    plot.title = element_text(color = "white", size = 35, family = "pt", hjust = 0.11, vjust = -0.25),
        plot.subtitle = element_text(color = "white", size = 12, hjust = 0.04, vjust = -0.30),
        axis.text = element_text(size = 8, color = "white"),
        axis.title.y = element_text(size = 8, color = "white", hjust = 0),
        plot.caption = element_text(size = 8, color = "white"),
        legend.key.size = unit(0.4, "cm"),
        legend.title = element_text(size = 10),) + 
        annotate(
    geom = "curve", x = 1947, y = 275, xend = 1951, yend = 55, 
    curvature = .3, arrow = arrow(length = unit(2, "mm")), color = "#393b44"
  ) +
  annotate(geom = "text", x = 1925, y = 355, 
           label = "Tom Bourdillon and Charles Evans \nused closed-circuit oxygen apparatus", 
           hjust = "left", color = "#393b44", size = 3.5)

ggsave(
  filename = here::here("output", "6.pdf"),
  plot = last_plot(),
  width = 6,
  height = 6,
  dpi = 300
)

  
```

![](/uploads/8.png)