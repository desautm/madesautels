---
title: Le package prenoms
author: ''
date: '2021-04-27'
slug: le-package-prenoms
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2021-04-27T16:28:34-04:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

Mon premier package, `prenoms`, vient tout juste d'arriver sur [CRAN](https://cran.r-project.org/). Le package est en fait une base de données de tous les prénoms donnés aux enfants du Québec entre 1980 et 2020.

Pour l'installer, rien de plus facile:
  ```{r eval=FALSE}
install.packages("prenoms")
```

Vous pouvez ensuite l'utiliser comme ceci:
```{r}
library(prenoms)
```

Nous allons utiliser le package `tidyverse` pour visualiser les données:
```{r}
# install.packages("tidyverse") si le package n'est pas encore installé
library(tidyverse)
```


Une base de données sous la forme d'un `tibble` sera maintenant accessible sous le noms de `prenoms`.
```{r}
glimpse(prenoms)
```

```{r}
prenoms
```

# Quelques exemples

## Les 5 prénoms masculins les plus populaires en 2020

```{r}
prenoms %>% 
  filter(year == 2020 & sex == "M") %>% 
  arrange(desc(n)) %>% 
  head(5)
```

## Les 5 prénoms féminins les plus populaires en 2020

```{r}
prenoms %>% 
  filter(year == 2020 & sex == "F") %>% 
  arrange(desc(n)) %>% 
  head(5)
```

## La popularité des prénoms de ma famille entre 1980 et 2020

```{r}
family <- prenoms %>%
  filter(
    name == "Marc-Andre" & sex == "M" |
    name == "Laurent" & sex == "M" |
    name == "Melanie" & sex == "F" |
    name == "Anna" & sex == "F"
  ) %>%
  group_by(name, year, sex) %>%
  summarise(n = sum(n)) %>%
  arrange(year)
```

```{r}
ggplot(data = family, aes(x = year, y = n, color = name))+
  geom_line()+
  scale_x_continuous( breaks = seq(1980, 2020, by = 5))
```
