+++
date = 2021-01-05T19:57:00Z
description = "Bump charts are good to use to plot ranking over time, or other examples when the path between two nodes have no statistical significance."
image = "/uploads/16.png"
tags = ["dataviz", "ggplot2", "100-ggplots"]
title = "#15 100-ggplots Chiklet Chart with ggchicklet"

+++

Libraries:

```r
pacman::p_load(tidyverse, ggchicklet, extrafont, hrbrthemes, ggtext, grid)
extrafont::loadfonts(device = 'win',  quiet = TRUE)
```


Data: 

```r
dt_rand <- tibble(
  Sport = c("NFL", "NFL", "NFL", "MLB", "MLB", "MLB", "NBA", "NBA",
            "NBA", "NHL", "NHL", "NHL", "EPL", "EPL", "EPL"),
  Type = c("Game Action", "Nonaction", "Commercials", "Game Action", 
            "Nonaction", "Commercials", "Game Action", "Nonaction", "Commercials", 
            "Game Action", "Nonaction", "Commercials", "Game Action", "Nonaction", 
            "Commercials"),
  Time = c(18, 140.6, 49.9, 22.5, 150.9, 51.8, 49.6, 61.8,
           33.5, 63, 56.6, 37.4, 58.7, 47.8, 10.1)
  )
```




```r
p <- dt_rand %>% 
  group_by(Sport) %>% mutate(Percent = Time/sum(Time)) %>% ungroup() %>% 
  mutate(Sport = fct_relevel(
    Sport,
    rev(c("NFL", "MLB", "NBA", "NHL", "EPL")))
  ) %>% 
  mutate(Type = fct_relevel(
    Type,
    c("Commercials","Nonaction","Game Action"))) %>% 
  ggplot(aes(Sport, Percent, label = Time)) +
  geom_chicklet(aes(fill = Type), position = ggplot2::position_fill()) +
  scale_fill_manual(values = c("#222831", "#30475e", "#f05454")) +
  coord_flip() +
  annotate(
    geom = "curve", xend = .5, yend = .05, x = 0, y = .12,
    curvature = -.3,
    size = .7,
    color = "black",
    arrow = arrow(type = "open", length = unit(.5, "lines")),
  ) +
  annotate(
    geom = "richtext", x = 0, y = .45, 
    size = 4,
    label.color = NA,
    fill = NA,
    label = "<span style='font-size:13pt;'>The average share of broadcast time showing <strong style='color:#FA759F'>GAME ACTION</strong> is highest in<br>the English Premier League - but there is more total action in an average<br>National Hockey League game, which lasts longer.</span>"
  ) +
  labs(
     title = "<b>NFL and MLB games are long, slow affairs</b>",
    subtitle = "Minutes by broadcast by what is shown on screen across five major men's sports leagues"
  ) + theme_minimal() +
  theme(
    legend.position = "top", 
    axis.text.x = element_blank(),
    axis.title = element_blank(),
    axis.ticks = element_blank(),
    panel.grid = element_blank(),
    plot.title = element_markdown(hjust = .5, size = 18),
    plot.subtitle = element_markdown(hjust = .5),
    plot.margin = margin(1,1,2,1, unit = "lines")
  ) 

gt <- ggplot_gtable(ggplot_build(p))
gt$layout$clip[gt$layout$name == "panel"] <- "off"
grid.draw(gt)

```

![](/uploads/16.png)

```r
ggsave(here::here("output", "16.png"), plot = gt, width = 10, height = 5, type="cairo", dpi = 600)
```


From: [Michael Lee](https://www.mikelee.co/posts/2020-02-08-recreate-fivethirtyeight-chicklet-stacked-bar-chart-in-ggplot2/)