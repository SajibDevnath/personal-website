+++
date = 2020-04-03T18:00:00Z
description = ""
image = "/uploads/unnamed-chunk-3-1.png"
tags = ["dataviz"]
title = "Visualizing Cricket World Cup 2019 Boundaries"

+++
Importing required libraries:

```{r, warning = FALSE, message = FALSE}
library(tidyr)
library(dplyr)
library(tidyr)
library(dplyr)
library(ggplot2)
library(ggthemes)
library(ggdark)
```

Importing Data:

```{r cu, echo=TRUE, message=FALSE, warning=FALSE}
world_cup_boundaries <- read.csv('data/world_cup_boundary.csv')
```

Tidy the data for analysis:

```{r message=FALSE, warning=FALSE, include=TRUE}

wcb_tidy <- world_cup_boundaries %>% 
  select(Name, Six, Four) %>% 
  gather(Boundary, Total, -Name)
```

Visualising with ggplot:

```{r, warning = FALSE, message = FALSE}

ggplot(wcb_tidy, aes(reorder(Name, Total), Total, fill = Boundary)) +
  geom_bar(stat = 'identity', width = .7) +
  coord_flip() +
  dark_theme_minimal(base_family = "Helvetica", base_size = 16) +
  labs(title = 'World Cup 2019 Boundaries', x = 'Player') +
  scale_fill_excel()
```

![](/uploads/unnamed-chunk-3-1.png)