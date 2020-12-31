+++
date = 2020-12-28T19:57:00Z
description = "Bump charts are good to use to plot ranking over time, or other examples when the path between two nodes have no statistical significance."
draft = true
image = "/uploads/14.png"
tags = ["dataviz", "ggplot2", "100-ggplots"]
title = "#15 100-ggplots Ninja Warrior"

+++

Libraries:

```r
pacman::p_load(tidyverse, lubridate, ragg, extrafont)
extrafont::loadfonts(device = 'win',  quiet = TRUE)
```


Data:

```r
weather <- data.table::fread(here::here("data", "daily_weather_2020.csv"))
```


```r

rainfall <- data.frame(
  date = seq(as.Date("2020-01-01"), as.Date("2020-12-31"), 1),
  rain = sample.int(30, 366, replace = TRUE)
) %>% 
  mutate(rain = ifelse(rain %% 3 == 0 | rain %% 7 == 0, 0, rain))
```


```r
rainfall_2 <- rainfall %>% 
  mutate(
    weekday = wday(date, label = TRUE, week_start = 1),
    month = month(date, label = TRUE, abbr = TRUE),
    week = isoweek(date),
    day = day(date)
  )
```


```r
rainfall_3 <- rainfall_2 %>% 
  mutate(
    week = case_when(month == "December" & week == 1 ~ 53,
                     month == "January" & week %in% 52:53 ~ 0,
                     TRUE ~ week),
    raincat = cut(rain, c(-1, 0, .5, 1:5, 7, 9, 15, 20, 25, 30, 300)),
    text_color = if_else(raincat %in% c("(15,20]", "(20,25]", "(25,30]", "(30,300]"), "white", "black")
    
  )
```

Color Palette:

```r
pubu <- RColorBrewer::brewer.pal(9, "PuBu")
col_p <- colorRampPalette(pubu)
```



```r

rain_plot <- rainfall_3 %>% 
  ggplot(aes(weekday, -week, fill = raincat)) +
  geom_tile(color = "white", size = .4) +
  geom_text(aes(label = day, colour = text_color), size = 2.5) +
  guides(fill = guide_colorsteps(barwidth = 25, 
                                 barheight = .4,
                                 title.position = "top")) +
  scale_colour_manual(values = c("black", "white"), guide = FALSE) +
  scale_fill_manual(values = c("white", col_p(13))) +
  facet_wrap(~ month, nrow = 4, ncol = 3, scales = "free") +
  labs(title = "How is 2020 being in Santiago?", 
       subtitle = "Rainfall",
       caption = "Data: Meteogalicia",
       fill = "mm") 

rain_plot
```

```r

rain_plot +
  theme(
    legend.position = "top",
    axis.title = element_blank(),
    axis.ticks = element_blank(),
    axis.text.y = element_blank(),
    panel.grid = element_blank(),
    panel.background = element_blank(),
    strip.background = element_blank(),
    text = element_text(family = "Fira Sans Condensed Light"),
    strip.text = element_text(family = "Fira Sans Condensed Light", face = "bold", size = 15),
    plot.title = element_text(size = 20, face = "bold", hjust = .5),
    plot.subtitle = element_text(size = 14, hjust = .5)
  )

```

![](/uploads/14.png)


From : Dominic Royé](https://dominicroye.github.io/en/2020/a-heatmap-as-calendar/)

