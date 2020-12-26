+++
date = 2020-12-26T05:00:00Z
description = "Bump charts are good to use to plot ranking over time, or other examples when the path between two nodes have no statistical significance."
image = "/uploads/12.png"
tags = ["dataviz", "ggplot2", "100-ggplots"]
title = "#12 100-ggplots Women in Everest"

+++

Library:

```r
pacman::p_load(tidyverse, extrafont, magick, ggtext, ggimage)

extrafont::loadfonts(device = 'win')
```


Data:

```r
members <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-22/members.csv')
expeditions <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-22/expeditions.csv')
peaks <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-22/peaks.csv')
```


Data Wrangling:

```r
women_peaks <- members %>% 
  filter(sex == "F") %>% 
  inner_join(peaks) %>% 
  filter(peak_name == "Everest") %>% 
  mutate(highpoint_metres = ifelse(is.na(highpoint_metres),4500,highpoint_metres))
```

```r
women_year <- women_peaks %>% 
  group_by(highpoint_metres, year) %>% 
  summarise(n = n())
```


```r
p1 <- ggplot(women_peaks) +
  geom_bar(aes(age, fill =..count..), color = NA, show.legend = F) +
  scico::scale_fill_scico(palette = 'acton', end = 0.7) +
  scale_x_continuous('', n.breaks = 8)  +
  annotate(geom = 'text', x = 65, y = 50, color = 'grey30', size = 5, label = 'Age',  family = "Fira Sans Condensed Light") +
  theme(
    panel.border = element_blank(),
    panel.grid = element_blank(),
    panel.background = element_rect(fill = 'transparent'),
    plot.background = element_rect(fill = 'transparent', color = 'transparent'),
    text = element_text( family = "Fira Sans Condensed Light", size = 10)
    
  )

p1 <- ggplotGrob(p1)
```


```r
p2 <- ggplot(women_year, aes(highpoint_metres, x = year, color = n)) + 
  geom_point(shape = 16, size = 1.5) + 
  geom_vline(aes(xintercept = 1953), color = 'grey60', linetype = 'longdash') +
  geom_text(aes(label = 'First successful ascent: 1953', x = 1953, y = 7600), angle = 90, size = 3, nudge_x = -1, color = 'grey50') +
  scale_y_continuous(
    position = 'right', 
    breaks = c(4500, 5000, 6000, 7000, 8000, 8850), 
    labels = c('No highpoint data', '5000 m', '6000 m', '7000 m', '8000 m', '8850 m - Summit')
  ) +
  
  annotation_custom(grob = p1, xmin = 2010, xmax = 2035, ymin = 8800, ymax = 10200) + 
  labs(
    title = "Women on top of the world!",
    subtitle = "From 1950 to 2019, <span style='color:#8C2981FF'>**1760 women**</span> attempted to climb<br>the highest peak in the world, **Mount Everest**. That is <br>8% of all climbing attempts. <span style='color:#8C2981FF'>**699 women**</span> have made<br>the summit, i.e. 7% of all the recorded summits.",
    caption = "Data: The Himalayan Database. Created by: @loreabad6", 
    x = 'Year of attempt', y = 'Highpoint reached') + 
   scale_x_continuous(n.breaks = 7) +
  scale_color_viridis_c(
    'No. of women', option = 'magma', 
    n.breaks = 5, direction = -1, end = 0.8,
    guide = guide_colorsteps(
      barheight = unit(2, 'mm'), 
      show.limits = T, title.position = 'top'
  )) +
  theme(
    plot.subtitle = element_markdown(size = 10, family = "Fira Sans Condensed Light"),
    plot.title = element_text(size = 18, family = "Fira Sans Condensed Light"),
    plot.caption = element_text(hjust = 0, size = 8, family = "Fira Sans Condensed Light"),
    
    ## legend modifying
    legend.text = element_text(color = 'grey50'),
    legend.title.align = 0.5,
    legend.position = c(0.6, 1.05),
    legend.direction = 'horizontal',
    legend.background = element_rect(fill = 'transparent'),
    plot.margin = margin(1,1,1,1, unit = "cm"),
    text = element_text(family = "Fira Sans Condensed Light")
  )


p3 <- ggbackground(
  p2, here::here("img", "tt_38.png"),
  image_fun = function(x) magick::image_colorize(x, opacity = 85, color = 'white')
) 
  
p3
```
    
    
```r
ggsave(here::here("output", "12.png"), plot = last_plot(), width = 8, height = 6.5, type="cairo")
```
  
![](/uploads/12.png)

From: [Lorena Abad](https://github.com/loreabad6/TidyTuesday/blob/master/R/2020/week_39.Rmd)