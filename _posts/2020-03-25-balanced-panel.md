---
title: 'Balancing Panel Data and Removing Duplicate Unique IDs'
date: 2020-03-25
permalink: /posts/2020/03/balanced-panel/
output: 
  md_document:
    variant: markdown_github
tags:
  - R
  - balanced panel
  - panel data
  - duplicate couples
---

Properly balancing panel data and removing duplicate unique identifiers is a recurring challenge for students in my Research Practicum course. Accordingly, I thought it would be helpful to provide an example for everyone.

Some may ask: What are *properly* balanced panel data? Generally, a *properly* balanced panel means that all possible ID variables and have data for the same (or appropriate) time period. In the example that follows, we will be working with a dataset of United States (US) foreign aid disbursements to all African countries for the 2001-2017 period. The data come from the US Foreign Aid Explorer (FAE), and we have deflated them to constant 2010 dollars to account for potential inflationary pressures. For these data, we want to ensure that all existent African countries have a foreign aid disbursement value for each year between 2001 and 2017.

To start, let's import our data frame and take a quick look at it:

``` r
rm(list=ls(all=TRUE)) # clear the environment
library(rio) # load package
fae_all <- import("african_aid.csv") # import the data
summary(fae_all) # summarize the data
```

    ##  country_name            year       aid_constant       
    ##  Length:911         Min.   :2001   Min.   :    -38845  
    ##  Class :character   1st Qu.:2005   1st Qu.:   9568481  
    ##  Mode  :character   Median :2009   Median :  58087880  
    ##                     Mean   :2009   Mean   : 165454902  
    ##                     3rd Qu.:2013   3rd Qu.: 167795723  
    ##                     Max.   :2017   Max.   :3173867164  
    ##                                    NA's   :         1

``` r
head(fae_all) # take a quick peak at the data
```

    ##   country_name year aid_constant
    ## 1      Algeria 2001       563682
    ## 2       Angola 2001     88977561
    ## 3        Benin 2001     41040486
    ## 4     Botswana 2001      3786002
    ## 5 Burkina Faso 2001     19766559
    ## 6      Burundi 2001     28287903

Let's check how many countries are in the dataset:

``` r
length(unique(fae_all$country_name))
```

    ## [1] 54

Let's also check how many years are in the dataset:

``` r
length(unique(fae_all$year))
```

    ## [1] 17

Now, let's check whether the data are balanced:

``` r
suppressMessages(library(plm)) # to remove note about dependencies
is.pbalanced(fae_all) # are the data balanced?
```

    ## Warning in pdata.frame(x, index): duplicate couples (id-time) in resulting pdata.frame
    ##  to find out which, use e.g. table(index(your_pdataframe), useNA = "ifany")

    ## Warning in is.pbalanced.default(id, time): duplicate couples (id-time)

    ## [1] FALSE

Let's first find the duplicate couple(s):

``` r
fae_all$unique_id <- paste(fae_all$country_name,fae_all$year) # concatenate to make unique ID
fae_all$duplicate = duplicated(fae_all$unique_id) # generate the duplicate variable
subset(fae_all, duplicate=="TRUE") # find the duplicate
```

    ##    country_name year aid_constant  unique_id duplicate
    ## 17        Egypt 2001            0 Egypt 2001      TRUE

The only duplicate value is for Egypt in 2001. It has an aid disbursement value of 0, so we can simply summarize the data. Stata users would say "collapse" the data. In this case, summarizing or collapsing the data means that we add together the two values for Egypt in 2001 and delete the duplicate row in the process. The process is generally the same if there were more duplicates in our data frame:

``` r
suppressMessages(library(tidyverse))
fae_all <-
  fae_all %>% # refer to the fae_all data frame
  group_by(country_name,year) %>% # tell R the unique ID
  select(-c(unique_id,duplicate)) %>% # drop (now) useless variables
  summarize(aid_constant = sum(aid_constant, na.rm=TRUE)) # summarize
```

Did that get rid of our duplicates couples problem?

``` r
library(plm)
is.pbalanced(fae_all) # duplicate couple issue now gone
```

    ## [1] FALSE

We are no longer getting the duplicates couples warning, so we can now concentrate on balancing the panel. Before proceeding, let's first create a numeric country ID variable:

``` r
fae_all$country_id <- as.numeric(factor(fae_all$country_name)) # numeric country ID
```

We need to create a numeric country ID variable because the `plm` package needs two numeric variables to constitute its ID-time index. With that step out of the way, let's now balance the panel:

``` r
fae_balanced <- make.pbalanced(fae_all, 
                               balance.type = c("fill"), 
                               index = c("country_id","year")) 
```

Since we used the numeric `country\_id` variable to balance the panel, it means that we are going to have some missing `country\_name` observations. Let's first figure out which countries do not have a country name attached to them:

``` r
subset(fae_balanced, is.na(country_name))
```

    ##     country_name year aid_constant country_id
    ## 273         <NA> 2001         <NA>         17
    ## 477         <NA> 2001         <NA>         29
    ## 478         <NA> 2002         <NA>         29
    ## 479         <NA> 2003         <NA>         29
    ## 783         <NA> 2001         <NA>         47
    ## 784         <NA> 2002         <NA>         47
    ## 786         <NA> 2004         <NA>         47
    ## 787         <NA> 2005         <NA>         47

Note that we have `country\_id` observations with missing country names, so we can use those IDs to identify the corresponding missing countries:

``` r
subset(fae_balanced, country_id==17|country_id==29|country_id==47) 
```

    ##          country_name year aid_constant country_id
    ## 273              <NA> 2001         <NA>         17
    ## 274 Equatorial Guinea 2002        81590         17
    ## 275 Equatorial Guinea 2003       300441         17
    ## 276 Equatorial Guinea 2004        42583         17
    ## 277 Equatorial Guinea 2005       247406         17
    ## 278 Equatorial Guinea 2006        76640         17
    ## 279 Equatorial Guinea 2007        29844         17
    ## 280 Equatorial Guinea 2008        15953         17
    ## 281 Equatorial Guinea 2009       191463         17
    ## 282 Equatorial Guinea 2010       143462         17
    ## 283 Equatorial Guinea 2011        55248         17
    ## 284 Equatorial Guinea 2012       414390         17
    ## 285 Equatorial Guinea 2013       207469         17
    ## 286 Equatorial Guinea 2014       204699         17
    ## 287 Equatorial Guinea 2015       244107         17
    ## 288 Equatorial Guinea 2016       948035         17
    ## 289 Equatorial Guinea 2017      3321453         17
    ## 477              <NA> 2001         <NA>         29
    ## 478              <NA> 2002         <NA>         29
    ## 479              <NA> 2003         <NA>         29
    ## 480             Libya 2004        52165         29
    ## 481             Libya 2005       161604         29
    ## 482             Libya 2006     30792152         29
    ## 483             Libya 2007      4806971         29
    ## 484             Libya 2008     16698323         29
    ## 485             Libya 2009      6528419         29
    ## 486             Libya 2010     10736737         29
    ## 487             Libya 2011    236478945         29
    ## 488             Libya 2012    162328736         29
    ## 489             Libya 2013     54600235         29
    ## 490             Libya 2014     21013668         29
    ## 491             Libya 2015     25773448         29
    ## 492             Libya 2016     17908764         29
    ## 493             Libya 2017     40775906         29
    ## 783              <NA> 2001         <NA>         47
    ## 784              <NA> 2002         <NA>         47
    ## 785       South Sudan 2003            0         47
    ## 786              <NA> 2004         <NA>         47
    ## 787              <NA> 2005         <NA>         47
    ## 788       South Sudan 2006       262143         47
    ## 789       South Sudan 2007      4463780         47
    ## 790       South Sudan 2008      4803235         47
    ## 791       South Sudan 2009      3111907         47
    ## 792       South Sudan 2010     36194887         47
    ## 793       South Sudan 2011    107385654         47
    ## 794       South Sudan 2012    406072707         47
    ## 795       South Sudan 2013    511596816         47
    ## 796       South Sudan 2014    650811758         47
    ## 797       South Sudan 2015    750664797         47
    ## 798       South Sudan 2016    638801967         47
    ## 799       South Sudan 2017    942560453         47

Now that we know which country IDs correspond to the missing country names, let's make the appropriate fixes and check to ensure that there are no longer any missing country names:

``` r
fae_balanced$country_name[fae_balanced$country_id==17] = "Equatorial Guinea"
fae_balanced$country_name[fae_balanced$country_id==29] = "Libya"
fae_balanced$country_name[fae_balanced$country_id==47] = "South Sudan"

subset(fae_balanced, is.na(country_name)) # nothing now: all good
```

    ## [1] country_name year         aid_constant country_id  
    ## <0 rows> (or 0-length row.names)

Returning to the actual data balancing, let's now ensure that everything went through as planned:

``` r
is.pbalanced(fae_balanced) # are data balanced?
```

    ## [1] TRUE

Although the above command indicates that the data are now balanced, let's table the country and year variables to verify. Recall from above that the original (unbalanced) dataset contains 54 distinct countries and 17 distinct years. Accordingly, the new (balanced) data frame should contain 54 countries for our 17 years of data (2001-2017). To verify, let's proceed one step at a time, starting with the years:

``` r
table(fae_balanced$year) # years OK?
```

    ## 
    ## 2001 2002 2003 2004 2005 2006 2007 2008 2009 2010 2011 2012 2013 2014 2015 
    ##   54   54   54   54   54   54   54   54   54   54   54   54   54   54   54 
    ## 2016 2017 
    ##   54   54

The years look right, and we can say the same for the countries:

``` r
table(fae_balanced$country_name) # countries OK?
```

    ## 
    ##                  Algeria                   Angola                    Benin 
    ##                       17                       17                       17 
    ##                 Botswana             Burkina Faso                  Burundi 
    ##                       17                       17                       17 
    ##               Cabo Verde                 Cameroon Central African Republic 
    ##                       17                       17                       17 
    ##                     Chad                  Comoros      Congo (Brazzaville) 
    ##                       17                       17                       17 
    ##         Congo (Kinshasa)            Cote d'Ivoire                 Djibouti 
    ##                       17                       17                       17 
    ##                    Egypt        Equatorial Guinea                  Eritrea 
    ##                       17                       17                       17 
    ##                 Eswatini                 Ethiopia                    Gabon 
    ##                       17                       17                       17 
    ##                   Gambia                    Ghana                   Guinea 
    ##                       17                       17                       17 
    ##            Guinea-Bissau                    Kenya                  Lesotho 
    ##                       17                       17                       17 
    ##                  Liberia                    Libya               Madagascar 
    ##                       17                       17                       17 
    ##                   Malawi                     Mali               Mauritania 
    ##                       17                       17                       17 
    ##                Mauritius                  Morocco               Mozambique 
    ##                       17                       17                       17 
    ##                  Namibia                    Niger                  Nigeria 
    ##                       17                       17                       17 
    ##                   Rwanda    Sao Tome and Principe                  Senegal 
    ##                       17                       17                       17 
    ##               Seychelles             Sierra Leone                  Somalia 
    ##                       17                       17                       17 
    ##             South Africa              South Sudan           Sudan (former) 
    ##                       17                       17                       17 
    ##                 Tanzania                     Togo                  Tunisia 
    ##                       17                       17                       17 
    ##                   Uganda                   Zambia                 Zimbabwe 
    ##                       17                       17                       17

Lastly, let's remedy our missing data problem for our foreign aid disbursement variable:

``` r
subset(fae_balanced, is.na(aid_constant))
```

    ##          country_name year aid_constant country_id
    ## 273 Equatorial Guinea 2001         <NA>         17
    ## 477             Libya 2001         <NA>         29
    ## 478             Libya 2002         <NA>         29
    ## 479             Libya 2003         <NA>         29
    ## 783       South Sudan 2001         <NA>         47
    ## 784       South Sudan 2002         <NA>         47
    ## 786       South Sudan 2004         <NA>         47
    ## 787       South Sudan 2005         <NA>         47

As shown above, there are missing (NA) values. The Foreign Aid Explorer (FAE) is a very comprehensive data source, and the US government provides very transparent data about how its spends taxpayer money. It is thus impossible that there are missing data. For this reason, let's initially replace the missing values with 0, indicating that the country did not receive any foreign aid for the given years.

``` r
fae_balanced$aid_constant[is.na(fae_balanced$aid_constant)] <- 0
```

Was this the right decision? Unfortunately, it was a mistake: South Sudan only became an internationally-recognized country in 2011, and we don't want to have data for the country when it did not exist. Accordingly, let's check the foreign aid disbursement values for South Sudan:

``` r
subset(fae_balanced, country_name=="South Sudan")
```

    ##     country_name year aid_constant country_id
    ## 783  South Sudan 2001            0         47
    ## 784  South Sudan 2002            0         47
    ## 785  South Sudan 2003            0         47
    ## 786  South Sudan 2004            0         47
    ## 787  South Sudan 2005            0         47
    ## 788  South Sudan 2006       262143         47
    ## 789  South Sudan 2007      4463780         47
    ## 790  South Sudan 2008      4803235         47
    ## 791  South Sudan 2009      3111907         47
    ## 792  South Sudan 2010     36194887         47
    ## 793  South Sudan 2011    107385654         47
    ## 794  South Sudan 2012    406072707         47
    ## 795  South Sudan 2013    511596816         47
    ## 796  South Sudan 2014    650811758         47
    ## 797  South Sudan 2015    750664797         47
    ## 798  South Sudan 2016    638801967         47
    ## 799  South Sudan 2017    942560453         47

Interestingly, South Sudan has aid values from 2006-2017, indicating that the US government figured out which funds supported South Sudan even before it became a recognized country in 2011. We should thus not automatically delete anything prior to 2006. By the same token, we need to delete the zero values from 2001-2005, since South Sudan could not receive any foreign aid for that period:

``` r
library(tidyverse)
fae_final <- 
  fae_balanced %>% 
  filter(!(country_name=="South Sudan" & year<=2005))
```

We now have properly balanced panel data. In other words, (i) we have a panel dataset that has the appropriate number of countries and years; and (ii) we have carefully examined the extent to which missing/NA values should be part of our dataset. I hope this helps!
