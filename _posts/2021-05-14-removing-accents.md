---
title: 'Removing Accents in R'
date: 2021-05-25
permalink: /posts/2021/05/removing-accents-in-R/
output: 
  md_document:
    variant: gfm
tags:
  - R
  - removing accents
---



R is a fantastic open-source program that allows users to do just about anything, but sometimes the program requires some tricks to accomplish seemingly simple tasks. Removing accents is a case in point, so I'd like to provide everyone with some tricks to overcome some of the thornier accent removal issues. 

## Typical One- and Two-Character Substitutions

Before even getting into the accent removal, the first order of business is to ensure that your R Studio is using UTF-8 file encoding:

``` r
![](images/encoding1.png)
```

In my case, everything was already in UTF-8, but it was good to check just in case:

``` r
![](images/encoding2.png)
```

Now, let's create a data frame that will allow us to remove different types of accents that we will encounter:

``` r
# clear environment
rm(list=ls(all=TRUE)) 

# create data frame
df <- data.frame(
  country = c("Argentina", "Honduras","Germany"),
  city = c("San Martín", "San José","Aßlar"))

head(df)
```

I specifically chose those cities above because they embody different types of accent fixes that you may need to perform. Removing the "é" from San José and "í" from "San Martín" are typical cases that won't cause many problems--regardless of how you chose to remove them. The "ß" in "Aßlar", however, requires a two-character substitution: the "ß" needs to be replaced with "ss" when it is transcribed into English. 

So that we can take care of the one- and two-character substitutions all at once, let's  write a function called `remove.accents`. The `old1` vector contains the letter with the accent in the original language for the one-character substitutions, and the `new1` vector contains the respective replacements without the accents. Note how `old1` and `new1` are kept in order of the replacements. The logic is similar for `old2` and `new2`.

``` r
# define the function
remove.accents <- function(s) {
  
  # 1 character substitutions
  old1 <- "éí"
  new1 <- "ei"
  s1 <- chartr(old1, new1, s)
  
  # 2 character substitutions 
  old2 <- c("ß")
  new2 <- c("ss")
  s2 <- s1
  
  # finalize the function
  for(i in seq_along(old2)) s2 <- gsub(old2[i], new2[i], s2, fixed = TRUE)
  
  s2
}

```

Given that the function is now defined, let's execute it and see whether it works:

``` r
# finish the accent 
df$city = remove.accents(df$city)

# examine the data frame
head(df)
```

## When the Above Doesn't Work

Sometimes, the above tricks won't work. I recently ran into such an instance while cleaning Moldova data in Romanian for my [project on natural resources and subnational public goods provision](https://mikedenly.com/research/natural-resources-subnational-public-goods). To show how I overcame this challenge, let me load the shapefile with the `sf` package and only keep the `admin1` column with the accented variables:

``` r
# load libraries
library(sf)
library(dplyr)

# load shapefile
moldova = st_read("Moldova_SHP/MDA_adm1.shp")

# keep only the admin1 column with the accented characters
moldova <-  
  moldova %>% 
  dplyr::select(NAME_1) %>% 
  st_set_geometry(NULL) %>% # drop latitude & longitude pairs separately
  dplyr::rename(admin1 = NAME_1)
```

Let's see what these accented characters look like. Incidentally, because R Markdown has a tough time reading them, with use a screenshot here:

``` r
![](images/moldovaaccents.png)
```

In such instances, simply use the `make_clean_names` function from the `janitor` package to remove the accents:

``` r
# load library
library(janitor)

# perform fixes
moldova$admin1 = make_clean_names(moldova$admin1)

# make sure it goes through
table(moldova$admin1)
```

To conclude, let's just change the underscore back to a space:

``` r
# perform replacement
moldova$admin1 = gsub("_", " ", moldova$admin1)

# make sure it goes through
table(moldova$admin1)
```

