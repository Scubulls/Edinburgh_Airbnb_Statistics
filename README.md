# Edinburgh_Airbnb_Statistics
DS2 Homework1
---
title: "Homework 1"
author: Arda Kara
date: 04 Nisan 2023
header-includes:
    - \usepackage{fancyhdr}
    - \pagestyle{fancy}
    - \fancyfoot[LE,RO]{\thepage}
    - \fancyfoot[LO,RE]{\thepage}
output:
    html_document:
        keep_md: true
        toc: true
---

```{r setup, include=FALSE}
library(tidyverse)
library(lubridate)
library(ggplot2)
library(dplyr)
library(visdat)
library(naniar)
library(readr)
library(evaluate)
library(rmarkdown)
library(markdown)

knitr::opts_chunk$set(out.width = "100%")
```

```{r unsplash, fig.margin = TRUE, echo = FALSE, fig.cap = "Photo by Peiheng Yang on Unsplash"}
knitr::include_graphics("peiheng-yang-wa8U2Y01nmY-unsplash.jpg")
```


In this assignment, you will get to put your newly acquired skills of data wrangling and some visualization into use, by benefiting from the tidyverse ecosystem.

## Prerequisites

- This assignment assumes that you have worked through all materials up to and including week 4. 

- Make sure you are familiar with this content. 

- In some questions, maybe, you need to search additional functionalities from tidyverse packages.


## Packages

This assignment will use the following packages:

-   `tidyverse`: a collection of packages for doing data analysis in a "tidy" way
-   `ggplot2`: a package that contains the function for the grammer of graphics
-   `lubridate`: a package for formatting dates in R (in case if you need)

We use the `library()` function to load packages.
In your R Markdown document you should see an R chunk labelled `load-packages` which has the necessary code for loading both packages.

```{r load-packages}
library(tidyverse)
library(lubridate)
library(ggplot2)
library(dplyr)
library(visdat)
library(naniar)
library(readr)
```


- You should also load these packages in your Console, which you can do by sending the code to your Console by clicking on the **Run Current Chunk** icon (green arrow pointing right icon).

Note that these packages are also get loaded in your R Markdown environment when you **Knit** your R Markdown document.

# Airbnb listings in Edinburgh


Airbnb is an online marketplace where people can book short-term stays in hotels or in other people's houses. 
Recent developments in Edinburgh regarding the growth of Airbnb and its impact on the housing market means a better understanding of the Airbnb listings is needed.
Using data provided by Airbnb, we can explore how Airbnb availability and prices vary by neighbourhood.

------

A copy of the Edinburgh Airbnb data set has been available (because of dsbox package version problem in Rstudio). To read this data, ensure that you have the following code near the beginning of your R Markdown file, from your directory having the data file (ie. `data/`).

```{r load-edibnb-data, message = FALSE}
edibnb <- read_csv("edibnb.csv")
```


------

You can view the dataset as a spreadsheet using the `View()` function.
Note that you should _not_ put this function in your R Markdown document, but instead type it directly in the Console, as it pops open a new window.
When you run this in the console, you will see the following **data viewer** window pop up.

```{r view-data, eval = FALSE}
View(edibnb)
```

- You can find out more about the dataset by inspecting its documentation, which you can access by running `?edibnb` in the Console or using the Help menu in RStudio to search for `edibnb` (If you can install and call `dsbox` package correctly). 

- Otherwise, you can also find this information [here](https://rstudio-education.github.io/dsbox/reference/edibnb.html).

The `edibnb` data set contain `r nrow(edibnb)` listings in Edinburgh, with values about `r ncol(edibnb)` different variables that includes the property neighborhood location, number of rooms and review ratings.


Edinburgh council collects the second data set from their assessments of a randomly selected sample of listings. The variables in this data set are:
  -   `id`, the ID number of the listing (according to `edibnb`)
  -   `rating`, the council's rating of the listing of the property either being good, adequate or poor.
  -   `assessment_date`, the date the council performed their assessment of the property.
  
The data is contained in the file `council_assessments.csv` within the `data/` directory. It can be loaded into your workflow using the following code (or any other directory name that you have instead of `/data`).

```{r message = FALSE}
council <- read_csv("council_assessments.csv")
```


## Exercise 1 (15 pts)

- In the `edibnb` data set, what is the ID of the listing that has the highest number of reviews with a perfect review score of 100%? 

- Which variables have the missing observations in the `edibnb` data set. 

- Visualize the missing observations using some functions from visdat or naniar packages (Notes from Week 4 might be useful)

✏️️🧶 ✅ ⬆️ *Write your answer in your R Markdown document , knit the document, make sure that your added R code chunk works well and you did not face with any knitting problem*

```{r, Question1}
edibnb1 <- edibnb %>% subset(review_scores_rating == 100)


View(edibnb1)


summary(edibnb1)

str(edibnb1)

vis_dat(edibnb)

vis_miss(edibnb)

```

**Firstly, we take all 100% ratings in a subset then we are visualize the data set according to their classes and according to missing data.**

## Exercise 2 (25 pts)

Calculate the minimum, maximum and average price from the Airbnb properties in Southside for a single night stay for four people (Try to use the `summarise` function).

✏️️🧶 ✅ ⬆️ *Write your answer in your R Markdown document , knit the document, make sure that your added R code chunk works well and you did not face with any knitting problem*

```{r, Question2}
edibnb2 <- edibnb %>% subset(neighbourhood == "Southside" & accommodates == 4)

Southside_Calculation <- summarise(edibnb2, min = min(price), max = max(price), mean = mean(price))

str(Southside_Calculation)

```

**Secondly, again we collect the requested data in a subset then we do the minimum, maximum and average transactions requested from us.**

## Exercise 3 (30 pts)

When looking at the data you will notice that some of the listings have a value for the number of bathrooms that is not a whole number, e.g. 1.5 or 2.5. 

- Mutate the `bathrooms` variable to round the number of bathrooms _up_ to the nearest whole number (hint, look at `ceiling()`). 

- Using this mutated variable, how many listings have more bathrooms than bedrooms?

✏️️🧶 ✅ ⬆️ *Write your answer in your R Markdown document , knit the document, make sure that your added R code chunk works well and you did not face with any knitting problem*

```{r, Question3}
edibnb3 <- edibnb %>% mutate(bathrooms = ceiling(bathrooms))


edibnb4 <- edibnb3 %>% subset(bathrooms > bedrooms )

summary(edibnb4)

print("Number of Rows: ")
print(nrow(edibnb4))



```

**Thirdly, we round the double data of the bathrooms to the integer and use the mutate and ceiling functions for this. Then we create a subset of houses with less than a bedroom bathroom. And finally, we count rows to see how many pieces of data there are.**

## Exercise 4 (40 pts)

Join the `edibnb` to the `council` data frames. Create a bar plot of the `neighbourhood` variable for the properties that have been assessed by the council (remember to iterate the data visualization to make it as informative as possible). 

- What does the data visualization suggest? 

- Is the council targeting all neighborhoods within Edinburgh equally?


✏️️🧶 ✅ ⬆️ *Write your answer in your R Markdown document , knit the document, make sure that your added R code chunk works well and you did not face with any knitting problem*

```{r, Question4}
edibnbCouncil <- left_join(edibnb, council, by = "id")



ediConReview <- edibnbCouncil %>% filter(!is.na(id))



ggplot(data = ediConReview , aes(x = neighbourhood)) +
        geom_bar(fill = "navyblue") +
        labs(title = "Neighbourhoods",x = "Neighbourhood", y = "Count")



```
```{r}

```

**Finally, we prepare a data set to see a city planning and visualize this data set in the form of a bar chart and interpret it. As a result, if we have to interpret the final graph, we can clearly see that not all regions are given the necessary attention, some regions are given great importance, some regions are given as much importance as a quarter of them, and finally there is a huge support for the Letih region.**

## Note

**I change file routes because I import all files in one folder.**
