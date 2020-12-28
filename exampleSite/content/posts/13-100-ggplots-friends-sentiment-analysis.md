+++
date = 2020-12-28T05:57:00Z
description = "Bump charts are good to use to plot ranking over time, or other examples when the path between two nodes have no statistical significance."
image = "/uploads/13.png"
tags = ["dataviz", "ggplot2", "100-ggplots"]
title = "#13 100-ggplots Friends Sentiment Analysis"

+++

Library:

```{r}
pacman::p_load(tidyverse, tidytext, ggwordcloud, extrafont)
extrafont::loadfonts(device = 'win',  quiet = TRUE)
```


Data:

```{r}
friends <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-08/friends.csv')
```


Data Crunching:

```{r}
six_friends <- friends %>% 
   filter(
    speaker %in% c(
      "Rachel Green",
      "Ross Geller",
      "Chandler Bing",
      "Monica Geller",
      "Joey Tribbiani",
      "Phoebe Buffay"
    )
  )
```


Getting unique sentiments:


```{r}

six_friends_crunched <- six_friends %>% 
  unnest_tokens(word, text)  %>% 
  anti_join(stop_words, by = "word") %>% 
  group_by(speaker, word) %>% 
  count() %>% 
  ungroup() %>% 
  inner_join(get_sentiments(), by = "word") %>% 
  group_by(word) %>% 
  filter(n_distinct(speaker) != 6, n > 3) %>% 
  group_by(speaker) %>% 
  mutate(n2 = scales::rescale(n)) %>% 
  arrange(-n2) %>% 
  ungroup()

six_friends_crunched
```

Visualization:



```{r fig.width= 10, fig.height=6}

set.seed(99)

six_friends_crunched %>% 
  ggplot(aes(
    label = word,
    size = n2,
    color = sentiment,
    alpha = n2
  )) +
  ggwordcloud::geom_text_wordcloud_area(area_corr_power = 1) +
  facet_wrap(~ speaker) +
  scale_radius(range = c(3, 15)) +
  scale_color_manual(values = c("#f14c38ff", "#01b0f1ff")) +
  labs(
    title = "The one with sentiment analysis",
    subtitle = "<span style='color:#01b0f1ff'>Positive</span> and <span style='color:#f14c38ff'>Negative</span>)<br><br>"
  ) +
  theme_void() + 
  theme(
    plot.margin = margin(1,1,1,1, unit = "cm"),
    plot.background = element_rect(fill = "#393536ff", color = NA),
    strip.text = element_text(size =  20, color = "white", family = "Gabriel Weiss\' Friends Font"),
    plot.title = element_text(family = "Gabriel Weiss\' Friends Font", size = 30, color = "#f4c93cff", hjust = .5, vjust = 2),
    plot.subtitle = ggtext::element_markdown(
      hjust = .5,
      color = "white",
      size = 15
    )
  )
```


```{r}
ggsave(here::here("output", "13.png"), plot = last_plot(), width = 10, height = 6, type="cairo")

```



![](/uploads/13.png)

From: [Jack Davison](https://github.com/jack-davison/TidyTuesday/blob/master/R/2020_09_08_Friends.R)