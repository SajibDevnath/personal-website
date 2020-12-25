+++
date = 2020-12-25T17:58:00Z
description = "How old were astronauts on their most recent mission?"
image = "/uploads/10.png"
tags = ["dataviz", "ggplot2", "100-ggplots"]
title = "#10 100-ggplots Astronauts"

+++
Library:

```r
pacman::p_load(tidyverse, ggtext, extrafont)
```

Loading Data:

```r
astronauts <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-07-14/astronauts.csv')
```

Data Wrangling:

```r
astro <- astronauts %>% 
   mutate(age_selected = year_of_selection - year_of_birth,
         age_mission = year_of_mission - year_of_birth)
```

Finding means:

```r
astro_means <- astro %>% 
  group_by(name) %>% 
  filter(mission_number == max(mission_number)) %>% 
  pivot_longer(cols = c(age_mission, age_selected),
               names_to = "agetype",
               values_to = "age") %>% 
  group_by(agetype) %>% 
  summarise(mean_age = as.integer(mean(age))) %>% 
  mutate(
    agetype = str_remove(agetype, "age_"),
    agetype = ifelse(agetype == "selected", "selection", "mission")
  )
  
astro_means
```

Finding the most recent mission:

```r
for_plot <- astro %>%
  group_by(name) %>%
  filter(mission_number == max(mission_number)) %>%
  pivot_longer(cols = c(age_mission, age_selected),
               names_to = "agetype")
```

Visualization:

```{r fig.width=10, fig.height=6}
for_plot %>% 
  ggplot(aes(value, fill = agetype)) +
  geom_histogram(alpha = 0.9, binwidth = 1, show.legend = FALSE, position = position_identity()) + 
  geom_vline(data = astro_means, aes(xintercept = mean_age), linetype = "dashed", color = "white") +
  labs(
    y = "",
    x = "Age of Astronaut / Years",
    caption = "Data from the Astronaut Database (https://data.mendeley.com/datasets/86tsnnbv2w/1)",
    title = '"How old were astronauts on their most recent mission?"',
    subtitle = "Age of astronauts when they were <b style='color:#1CCAD8'>selected</b>
         and when they were sent on their <b style='color:#eeff00'>mission</b><br>"
  ) +
  scale_fill_manual(values = c("#eeff00", "#1CCAD8")) +
  scale_color_manual(values = c("#eeff00", "#1CCAD8")) +
  scale_y_continuous(expand = c(0, 0), limits = c(0, 67)) +
  geom_text(data = astro_means, aes(x = mean_age + 1, y = 60, label = paste0("Mean age at\n", agetype, " = ", mean_age), color = agetype, label = ), inherit.aes = FALSE, hjust = 0) +
  annotate(
    geom = "text",
    x = 72,
    y = 38,
    label = "John Glenn was 77\non his last mission:\nthe oldest person to\ntravel in space!",
    colour = "white",
    vjust = 0.5
  ) + 
  annotate(
    geom = "curve",
    xend = 77,
    yend = 4,
    x = 75,
    y = 26,
    curvature = -.2,
    arrow = arrow(type = "closed", length = unit(1, "lines")),
    colour = "white"
  ) +
  theme(
    rect = element_rect(fill = "#373F51"),
    text = element_text(colour = "white", family = "Fira Sans Condensed Light", size = 15),
    panel.background = element_rect(fill = "#373F51"),
    axis.text = element_text(colour = "white"),
    panel.grid = element_blank(),
    plot.subtitle = element_markdown(family = "Fira Sans Condensed Light", size = 14),
    legend.position = "none",
    axis.title.x = element_text(hjust = 1),
    plot.margin = unit(c(1, 1, 1, 1), "cm"),
    axis.ticks = element_blank(),
    panel.grid.major = element_line(colour = "#3e4c6d"),
    plot.title = element_text(family = "Fira Sans Condensed Light", size = 22),
    plot.caption = element_text(family = "Fira Sans Condensed Light", size = 12, hjust = 0, colour = "#7383a8")
  ) -> plotx
```

```r
ggsave(here::here("output", "10.png"), plot = plotx, width = 10, height = 6, type="cairo", dpi = 300)
```

  
![](/uploads/10.png)

From: [Jake Davison](https://github.com/jack-davison/TidyTuesday/blob/master/R/2020_07_14_Astronauts.R)