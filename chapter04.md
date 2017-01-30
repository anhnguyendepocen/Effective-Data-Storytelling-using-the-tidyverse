---
title       : Histograms & Boxplots
description : Make and interpret different plots relating one categorical variable to one numerical variable via the ggplot2 package using datasets in and derived from the fivethirtyeight R package

--- type:NormalExercise lang:r xp:100 skills:1 key:yl6eto7bme

## Creating a histogram

Recall the `police_join_cost` data frame that was created in the **Tidy Data** chapter and is available for interactive viewing [here](https://ismayc.github.io/Effective-Data-Storytelling-using-the-tidyverse/police_join_cost.html).

*** =instructions
- Create a histogram for viewing the distribution of residency of white police officers.
- The histogram should have five bins, fill color of "navyblue", and border color of "white".

*** =hint
- Remember to load the `ggplot2` package via `library(ggplot2)`
- Remember to use `?geom_histogram` to look over the different options for creating a histogram.  For example, use the `bins` argument to specify the number of bins.
- When you want to color or fill based on a specified color, make sure
to add quotatation marks around the name of the color.

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
data(police_locals)
library(readr)
library(dplyr)
ideology <- read_csv("http://ismayc.github.io/Effective-Data-Storytelling-using-the-tidyverse/datasets/ideology.csv")
police_join <- inner_join(x = police_locals, y = ideology, by = "city")
cost_of_living <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3085/datasets/cost_of_living.csv")
police_join_cost <- inner_join(x = police_join, y = cost_of_living, by = "state")
```

*** =sample_code
```{r}

```

*** =solution
```{r}
library(ggplot2)
ggplot(data = police_join_cost, mapping = aes(x = white)) +
  geom_histogram(bins = 5, fill = "navyblue", color = "white")
```

*** =sct
```{r}
test_library_function("ggplot2")
test_ggplot(check_data = TRUE, check_aes = TRUE, check_geom = TRUE,
  geom_fail_msg = "Make sure you are using `geom_histogram` here and that you have correctly specified its arguments.  Take Hint if you need help.")

test_error()
```

--- type:NormalExercise lang:r xp:100 skills:1 key:80cf9c93cf

## Making an appropriate plot

Recall the `police_join_cost` data frame that was created in the **Tidy Data** chapter and is available for interactive viewing [here](https://ismayc.github.io/Effective-Data-Storytelling-using-the-tidyverse/police_join_cost.html).  The plot from the previous question looking at the distribution of the residency of white police officers is also loaded.

*** =instructions
- Create an appropriate plot for viewing the distribution of residency of non-white police officers.
- The plot should group the data in the same way as the plot shown with the bins having yellow color and a black border.

*** =hint
- Remember to load the `ggplot2` package via `library(ggplot2)`
- Remember to use `?geom_histogram` to look over the different options for creating a histogram.  For example, use the `bins` argument to specify the number of bins.
- When you want to color or fill based on a specified color, make sure
to add quotatation marks around the name of the color.

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
data(police_locals)
library(readr)
library(dplyr)
ideology <- read_csv("http://ismayc.github.io/Effective-Data-Storytelling-using-the-tidyverse/datasets/ideology.csv")
police_join <- inner_join(x = police_locals, y = ideology, by = "city")
cost_of_living <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3085/datasets/cost_of_living.csv")
police_join_cost <- inner_join(x = police_join, y = cost_of_living, by = "state")
library(ggplot2)
ggplot(data = police_join_cost, mapping = aes(x = white)) +
  geom_histogram(bins = 5, fill = "navyblue", color = "white")
```

*** =sample_code
```{r}

```

*** =solution
```{r}
library(ggplot2)
ggplot(data = police_join_cost, mapping = aes(x = non_white)) +
  geom_histogram(bins = 5, fill = "yellow", color = "black")
```

*** =sct
```{r}
test_library_function("ggplot2")
test_ggplot(check_data = TRUE, check_aes = TRUE, check_geom = TRUE,
  geom_fail_msg = "Make sure you are using `geom_histogram` here and that you have correctly specified its arguments.  Take Hint if you need help.")

test_error()
```

--- type:NormalExercise lang:r xp:100 skills:1 key:ouyqgxeewi

## Problems with non-tidy data

We've seen in the previous two exercises how to produce plots of the distributions for both `white` and `non_white` police residency percentages in 75 cities throughout the US.  But what if we'd like to be able to plot them on the same plot? 

Recall that `police_join_cost` is not a tidy data set.  In fact, `white` and `non_white` being the names of variables that contain percentage values doesn't really make sense either and the Grammar of Graphics framework makes plotting this non-tidy data set challenging.

It would make more sense to have a variable called `race` with entries of `white` and `non_white` along with the corresponding value for each city of police residency percentages stored in a `reside_perc` variable. Observe this tidied data set [here](https://ismayc.github.io/Effective-Data-Storytelling-using-the-tidyverse/police_tidy.html) that has been given the name `police_tidy` and loaded for you here.

*** =instructions
- Produce a boxplot looking at the relationship between `race` and `reside_perc`.
- THINK ABOUT IT:  What stands out to you as you look over this plot from a social perspective?  What new information does this plot give you that was missing in the plots in the previous two exercises?
- THINK ABOUT IT:  Why does using the tidy data set make more sense here?  How does tidy data help with plotting?

*** =hint
- Remember to load the `ggplot2` package via `library(ggplot2)`
- Remember to use `?geom_boxplot` to look over the different options for creating a boxplot.


*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(readr)
library(dplyr)
library(tidyr)
data(police_locals)
ideology <- read_csv("http://ismayc.github.io/Effective-Data-Storytelling-using-the-tidyverse/datasets/ideology.csv")
police_join <- inner_join(x = police_locals, y = ideology, by = "city")
cost_of_living <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3085/datasets/cost_of_living.csv")
police_join_cost <- inner_join(x = police_join, y = cost_of_living, by = "state")
police_tidy <- police_join_cost %>% 
  gather(key = "race", value = "reside_perc", all:asian) %>% 
  filter(race %in% c("white", "non_white"))
```

*** =sample_code
```{r}

```

*** =solution
```{r}
library(ggplot2)
ggplot(data = police_tidy, mapping = aes(x = race, y = reside_perc)) +
  geom_boxplot()
```

*** =sct
```{r}
test_library_function("ggplot2")
test_ggplot(check_data = TRUE, check_aes = TRUE, check_geom = TRUE)

test_error()
```

## Explanatory and Response Variables

--- type:NormalExercise lang:r xp:100 skills:1 key:96ucqpbqdc