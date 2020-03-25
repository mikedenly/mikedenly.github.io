---
title: 'Balancing Panel Data and Removing Duplicate Unique IDs'
date: 2020-03-25
permalink: /posts/2020/03/balanced-panel/
tags:
  - R
  - balanced panel
  - panel data
  - duplicate couples
---

### Properly Balancing Panel Data and Removing Duplicate Unique IDs

Properly balancing panel data and removing duplicate unique identifiers is a recurring challenge for students in my Research Practicum course. Accordingly, I thought it would be helpful to provide an example for everyone.

Some may ask: What are *properly* balanced panel data? Generally, a *properly* balanced panel means that all possible ID variables and have data for the same (or appropriate) time period. In the example that follows, we will be working with a dataset of United States (US) foreign aid disbursements to all African countries for the 2001-2017 period. The data come from the US Foreign Aid Explorer (FAE), and we have deflated them to constant 2010 dollars to account for potential inflationary pressures. For these data, we want to ensure that all existent African countries have a foreign aid disbursement value for each year between 2001 and 2017. 

To start, let's import our data frame and take a quick look at it:
```{r}
rm(list=ls(all=TRUE)) # clear the environment
library(rio) # load package
fae_all <- import("african_aid.csv") # import the data
summary(fae_all) # summarize the data
head(fae_all) # take a quick peak at the data
```

Now, let's check whether the data are balanced:
```{r}
suppressMessages(library(plm)) # to remove note about dependencies
is.pbalanced(fae_all) # are the data balanced?
```

Let's first find the duplicate:
```{r}
fae_all$unique_id <- paste(fae_all$country_name, fae_all$year) # concatenate
fae_all$duplicate = duplicated(fae_all$unique_id) # generate the duplicate variable
subset(fae_all, duplicate=="TRUE") # find the duplicate
```

The only duplicate value is for Egypt in 2001. It has a disbursement value of 0, so we can simply summarize the data. Stata users would say collapse the data. In this case, summarizing or collapsing the data means that we add together the two values for Egypt in 2001 and delete the duplicate row in the process. The process is generally the same if there were more duplicates in our data frame:
```{r}
suppressMessages(library(tidyverse))
fae_all <-
  fae_all %>% # refer to the fae_all data frame
  group_by(country_name,year) %>% # tell R the unique ID
  select(-c(unique_id,duplicate)) %>% # drop (now) useless variables
  summarize(aid_constant = sum(aid_constant, na.rm=TRUE)) # summarize
```

Now that we have taken care of the duplicate unique ID (couple), we can now concentrate on balancing the panel. Before proceeding, let's first create a numeric country ID variable. We need to create a numeric country ID variable because the plm package needs two numeric variables for each index. Here is the code for both the numeric country ID and the creation of the balanced panel.
```{r}
library(plm)
is.pbalanced(fae_all) # duplicate couple issue now gone
fae_all$country_id <- as.numeric(factor(fae_all$country_name)) # numeric country ID
fae_balanced <- make.pbalanced(fae_all, 
                               balance.type = c("fill"), 
                               index = c("country_id","year")) # balance

```

To ensure everything went through as planned, let's ensure that the balanced data frame contains 54 countries for our 17 years of data (2001-2017). We also want to replace non-deserving missing identifiers, which we do for South Sudan here:
```{r}
table(fae_balanced$country_name) # countries OK?
fae_balanced$country_name[fae_balanced$country_id==47] = "South Sudan"

table(fae_balanced$year) # years OK
is.pbalanced(fae_balanced) # are data balanced?
```

As shown above, there are missing (NA) values. The Foreign Aid Explorer (FAE) is a very comprehensive data source, and the US government provides very transparent data about how its spends taxpayer money. It is thus impossible that there are missing data. For this reason, let's initially replace the missing with 0, indicating that the country did not receive any foreign aid for the given years.
```{r}
fae_balanced$aid_constant <- ifelse(is.na(fae_balanced$aid_constant), 0, 
                                  fae_balanced$aid_constant) # replace NA with 0
is.pbalanced(fae_balanced) # all balanced now
```

Was this the right decision? Unfortunately, it was a mistake: South Sudan only became a recognized country in 2011, and we don't want to have data for the country when it did not exist. Let's check the foreign aid disbursement values for South Sudan:
```{r}
subset(fae_balanced, country_name=="South Sudan")
```

Interestingly, South Sudan has aid values from 2006-2017, indicating that the US government figured out which funds supported South Sudan even before it became a recognized country in 2011. We should thus not automatically delete anything prior to 2006. By the same token, we need to delete values from 2001-2005, since South Sudan could not receive any foreign aid for that period:
```{r}
library(tidyverse)
fae_final <- 
  fae_balanced %>% 
  filter(!(country_name=="South Sudan" & year<=2005))
```

That is how to properly balance panel data. I hope this helps!