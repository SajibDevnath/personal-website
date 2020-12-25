+++
date = 2020-12-24T06:00:00Z
description = "How much currencies were under-or over-value relative to the US dollar, %, from 2010 to 2020."
image = "/uploads/9-2.png"
tags = ["dataviz", "ggplot2", "100-ggplots"]
title = "#8 100-ggplots Bump Chart - Dog Breed Popularity"

+++
## Tutorial:

```r
df <- tibble(country = c("India", "India", "India", "Sweden", "Sweden", "Sweden", "Germany", "Germany", "Germany", "Finland", "Finland", "Finland"),
             year = c(2011, 2012, 2013, 2011, 2012, 2013, 2011, 2012, 2013, 2011, 2012, 2013),
             value = c(492, 246, 246, 369, 123, 492, 246, 369, 123, 123, 492, 369))

knitr::kable(head(df))
```

```r
df <- df %>% 
  group_by(year) %>% 
  mutate(rank = rank(value, ties.method = "random")) %>% 
  ungroup()
```

```{r, fig.width=8, fig.height=4}
ggplot(df, aes(year, rank, color = country)) +
  geom_bump(size = 2, smooth = 8) +
  geom_point(size = 3) +
  geom_text(
    data = df %>% filter(year == min(year)),
    aes(x = year - .1, label = country),
    size = 5,
    hjust = 1
  ) + 
  geom_text(
    data = df %>% filter(year == max(year)),
    aes(x = year + .1, label = country),
    size = 5,
    hjust = 0
  ) +
  scale_x_continuous(breaks = 2011:2013, limits = c(2010.6, 2013.4)) +
  scale_color_manual(values = wes_palette(n = 4, name = "GrandBudapest1")) +
  ggthemes::theme_solid() +
  theme(
    legend.position = "none"
  ) -> plox
```

```r
ggsave(here::here("output", "9-1.png"), plot = plox, width = 8, height = 4, type="cairo")
```
![](/uploads/9-1.png)
\##2: Dog breed popularity bump chart

```r
dogranks <-
  tibble(
    Breed = c(
      "Retrievers (Labrador)", "German Shepherd Dogs",
      "Retrievers (Golden)", "French Bulldogs", "Bulldogs",
      "Beagles", "Poodles", "Rottweilers", "Yorkshire Terriers",
      "Pointers (German Shorthaired)", "Pembroke Welsh Corgis"
    ),
    r2019 = c(1L, 2L, 3L, 4L, 5L, 7L, 6L, 8L, 12L, 9L, 10L),
    r2018 = c(1L, 2L, 3L, 4L, 5L, 6L, 7L, 8L, 10L, 9L, 13L),
    r2017 = c(1L, 2L, 3L, 4L, 5L, 6L, 7L, 8L, 9L, 10L, 15L),
    r2016 = c(1L, 2L, 3L, 6L, 4L, 5L, 7L, 8L, 9L, 11L, 18L),
    r2015 = c(1L, 2L, 3L, 6L, 4L, 5L, 8L, 9L, 7L, 11L, 20L),
    r2014 = c(1L, 2L, 3L, 9L, 4L, 5L, 7L, 10L, 6L, 12L, 22L),
    r2013 = c(1L, 2L, 3L, 11L, 5L, 4L, 8L, 9L, 6L, 13L, 24L)
  )

knitr::kable(dogranks)
```

```r
dogs_long <- dogranks %>%
  pivot_longer(
    cols = r2019:r2013,
    names_to = "year", values_to = "rank"
  ) %>%
  mutate(year = readr::parse_number(year))

knitr::kable(dogs_long)
```

```r
library(showtext)
font_add_google(name = "IBM Plex Sans", family = "Arial")
showtext_auto()
```

```{r, fig.height=8, fig.width=12}
plot2 <- ggplot(dogs_long) +
  geom_bump(aes(year, rank, color = Breed), size = 2, smooth = 6) +
  scale_y_reverse() +
  scale_color_scico_d(palette = "hawaii", guide = FALSE) +
  scale_x_continuous(breaks = c(2013:2019), expand = expansion(add = c(1, 0.4))) +
  geom_text(
    data = dogranks, aes(y = r2013, x = 2013, label = Breed),
    hjust = "right", nudge_x = -0.15, size = 5.4, color = "white", family = "Arial"
  )  +
  geom_text(
    data = dogranks, aes(y = r2013, x = 2008, label = r2013),
    size = 5, color = "#a0c4ff", family = "Arial", fontface = "bold", 
  ) + 
  geom_text(
    data = dogranks, aes(y = r2019, x = 2019, label = r2019),
    nudge_x = 0.2, size = 5, color = "#a0c4ff", family = "Arial", fontface = "bold", nudge_y = 0.03
  )  +
   labs(
    x = "", y = "",
    title = "American Kennel Club most popular breeds",
    caption = "source: AKC registration statistics  https://www.akc.org/expert-advice/dog-breeds/2020-popular-breeds-2019/\nby @LuisDVerde (www.liomys.mx)"
  ) +
  theme(
    text = element_text(family = "Arial"),
    axis.text.x = element_text(size = 14, color = "grey55", face = "bold"),
    axis.text.y = element_blank(), 
    axis.ticks = element_blank(),
    panel.grid = element_blank(), 
    plot.caption = element_text(size = 8),
    plot.title = element_text(size = 25, hjust = .5, color = "grey70"),
    plot.background = element_rect(fill = "#252a32"),
    panel.background = element_rect(fill = "#252a32")
  ) 
  

plot2
```

```r
ggsave(here::here("output", "9-2.png"), plot = plot2, width = 12, height = 8, type="cairo", dpi = 300)
```

![](/uploads/9-2.png)