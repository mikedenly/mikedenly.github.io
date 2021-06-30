---
title: "Using tidylog and anti_join to Ensure Merges Go Through"
date: '2021-06-30'
permalink: /posts/2021/06/making-merges-go-through/
output:
  html_document:
    df_print: paged
  md_document:
    variant: gfm
tags:
- R
- merging
- tidylog
- anti_join
---

Packages in the `tidyverse` suite, including `dplyr`, represent amazing contributions data science. One thing that constantly vexed me, though, was the inability of `dplyr`'s [merge functions](https://dplyr.tidyverse.org/reference/join.html)--`left_join`, `full_join`, etc.-- to actually let you know if the merges went through without having to do extra work. It just seemed so cumbersome to have to individually inspect each data frame after each merge. Well, thankfully, that is no longer necessary due to `tidylog`.

To explain how `tidylog` helps with [ensuring merges go through](https://cran.r-project.org/web/packages/tidylog/readme/README.html) as desired, let's start by loading two data frames, `df1` and `df2`. First, let's load and inspect `df1`:

```{r}
# create df1
df1 = data.frame(country = c("Brazil", "USA"),
                 year = c(2020,2020),
                 population_millions = c(213,331))

# inspect df1
head(df1)
##   country year population_millions
1     Brazil 2020                 213
2     USA 2020                 331
```

Then, let's load and inspect `df2`:

```{r}
# create df2
df2 = data.frame(country = c("Brasil", "USA"),
                 year = c(2020,2020),
                 poverty_rate = c(21,9))

# inspect df2
head(df2)
```

As should be apparent, they are both data frames with demographic information on the poverty rates and population of the US and Brazil in the year 2020. Using the `dplyr` package, we could merge `df1` and `df2` together using `left_join`:

```{r}
# suppress warnings (unnecessary, but makes the script cleaner)
options(warn=-1)
options(tidyverse.quiet = TRUE)

# load library
library(tidyverse)

# merge
initial_merged_df = left_join(df1, df2, by=c("country", "year"))

# inspect the data frame
head(initial_merged_df)
```

As is clear from above, the poverty rate data is `NA` for Brazil. Why did this happen? The `tidylog` package helps us answer that without even having to inspect the data frame, which is really useful when working with larger data frames. To better understand the benefits of `tidylog`, let's re-run the merge, but this time let's load the `tidylog` package first.

```{r}
# load tidylog
library(tidylog, warn.conflicts = FALSE)

# re-run the merge
initial_merged_df = left_join(df1, df2, by=c("country", "year"))
```

Loading the `tidylog` package lets us know that our merge did not fully go through. Which observations? To figure that out, let's use `anti\_join`, which tells us which observations from `df2` did not merge over to `df1`. To do that, just flip the order of `df1` and `df2` as follows:

```{r}
# checking for problematic observations with anti_join
checking = anti_join(df2, df1, by=c("country", "year"))

# inspecting the data frame
head(checking)
```

As we can clearly see, the Brazil observation has been spelled with an "s" instead of an "z". Because we are working in English, let's correct the spelling (in English):

```{r}
# correct the spelling
df2$country[df2$country=="Brasil"] = "Brazil"
```

Now, we can re-run the merge for a final time, and everything will go through:

```{r}
# final time
final_merged_df = left_join(df1, df2, by=c("country", "year"))

# inspecting the data frame
head(final_merged_df)
```

Clearly, `tidylog` was not 100% necessary for merging the toy data frames above, `df1` and `df2`. With larger data frames, though, combining `tidylog` and `anti_join` will be crucial to ensuring that all of your data actually merge.    


