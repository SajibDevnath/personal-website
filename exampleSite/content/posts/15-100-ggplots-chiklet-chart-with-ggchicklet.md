+++
date = 2020-12-30T19:57:00Z
description = "Bump charts are good to use to plot ranking over time, or other examples when the path between two nodes have no statistical significance."
draft = true
image = "/uploads/15.png"
tags = ["dataviz", "ggplot2", "100-ggplots"]
title = "#15 100-ggplots Chiklet Chart with ggchicklet"

+++
Libraries:

```r
pacman::p_load(tidyverse, extrafont, patchwork)
extrafont::loadfonts(device = 'win',  quiet = TRUE)
```

Data:

```r
ninja_warrior <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-12-15/ninja_warrior.csv')
```

## Data crunching:

```r
ninja_warrior %>% 
  count(obstacle_name, sort = TRUE) %>% 
  head(13) %>% 
  mutate(obstacle_num = seq.int(nrow(.)),
         obstacle_num = obstacle_num %>% reorder(n)) -> top_ten
```




```r
plot_radial <- top_ten %>% 
  ggplot(aes(obstacle_num, n)) +
  geom_bar(stat = "identity", fill = "#0F4B77", width = .7) +
  coord_polar(theta = "y") + 
  ylim(0, 90) +
  geom_text(aes(x = 1:13, y = 0, label = 13:1), size = 4,  hjust = 1.4, vjust = 0.5, color =  "#0F4B77", family = "Lato Semibold", nudge_y = .05) +
  theme_void() +
  theme(
    aspect.ratio = 1,
    panel.grid = element_blank()
  )

plot_radial

p1 <- ggplotGrob(plot_radial)
```




```r
ninja_text <- top_ten %>% 
  ggplot() +
  geom_text(aes(label = paste0(obstacle_num, ". ", obstacle_name, " [", n, "]"), x = 0, y = 1:13), hjust = 0, color = "gray30", family = "Lato Semibold") +
  scale_y_reverse(limits = c(18,  -4)) +
  xlim(0, 20) + 
  annotation_custom(grob = p1, xmin = 6, xmax = 20) +
  labs(
    title = "American Ninja Warrior",
    subtitle = "Top Ten Obstacle",
    caption = "Source: Data.World"
  ) +
  theme_void() +
  theme(
    plot.title = element_text(family = "Calisto MT", size = 25, face = "bold", color = "#FF2602"),
    plot.subtitle = element_text(family = "Calisto MT", size = 18, face = "bold"),
    plot.caption = element_text(family = "Calisto MT", size = 9),
    plot.margin = margin(.5, .5, .5, .5, unit = "cm"),
    text = element_text(color = "gray30")
  )

ninja_text

```


```r
p2 <- ggbackground(
  ninja_text, here::here("img", "ninja_warrior.png"),
  image_fun = function(x) magick::image_colorize(x, opacity = 96, color = 'white')
)

p2
```
![](/uploads/15.png)

```r
ggsave(here::here("output", "15.png"), plot = last_plot(), width = 8, height = 6, type="cairo", dpi = 600)
```


From : [Eliane Mitchell](https://github.com/elianemitchell/mytidytuesdaycode/blob/main/warrior_week51_2020.R)