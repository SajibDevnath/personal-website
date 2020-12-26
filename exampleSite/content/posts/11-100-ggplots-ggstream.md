+++
date = 2020-12-26T05:00:00Z
description = "Bump charts are good to use to plot ranking over time, or other examples when the path between two nodes have no statistical significance."
image = "/uploads/11-2.png"
tags = ["dataviz", "ggplot2", "100-ggplots"]
title = "#11 100-ggplots ggstream"

+++

Library:

```r
pacman::p_load(tidyverse, ggtext, extrafont, cowplot, treemapify, ggsci, ggstream)
```

Data: 

```r
key_crop_yields <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-01/key_crop_yields.csv')
```



Data Wrangling: 

```r
spain <- key_crop_yields %>% 
  filter(Entity == "Spain") %>% 
  pivot_longer(cols = 4:14, names_to = "Crop", values_to = "tonnes") %>% 
  select(Year, Crop, tonnes) %>% 
  mutate(Crop = str_remove(Crop, " \\(tonnes per hectare\\)")) %>% 
  filter(!Crop %in% c("Beans", "Cassava")) 

spain
```



Visualization:

```r
spain %>% 
  ggplot(aes(Year, tonnes, fill = Crop, color = Crop)) +
  geom_stream(type = "ridge", alpha = .7) +
  geom_stream_label(aes(label = Crop), type = "ridge", color = "grey10", hjust = -.05, size = 5) +
  scale_color_igv() +
  scale_fill_igv() + 
  theme_minimal() + 
  theme(
    legend.position = "none",
    text = element_text(size = 15),
    plot.margin = margin(2,2,2,2, unit  = "cm")
  )

```
![](/uploads/11.png)

```r
spain %>% 
  filter(Year == 2018) %>% 
  arrange(desc(tonnes)) %>% 
  ggplot(aes(area = tonnes, fill = Crop, label = Crop)) +
  geom_treemap() +
  geom_treemap_text(colour = "white", place = "topleft") +
  theme(
    legend.position = "none"
  )
```




```r
ggsave(here::here("output", "11-2.png"), plot = last_plot(), width = 12, height = 10, type="cairo")
```
![](/uploads/11-2.png)
























