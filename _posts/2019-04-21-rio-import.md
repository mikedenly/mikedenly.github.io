---
title: 'Importing Data in R'
date: 2019-04-21
permalink: /posts/2019/04/rio-import/
tags:
  - R
  - importing data
  - rio package
---

Hello, world! I have finally figured out GitHub and how to make a free website with Jekyll. It's a great day!

Today, I would like to point everyone to my favorite R package for importing data: the [rio package](https://cran.r-project.org/web/packages/rio/vignettes/rio.html). It's my favorite because the syntax is basically the same, no matter what type of data file you are trying to load.

To import a .csv file:
```r
library(rio)
data1 <- import("data1.csv")
head(data1)
```

To import a Stata dataset, all you need to do is change the extension to .dta.
```r
library(rio)
data2 <- import("data2.dta")
head(data2)
}
```

For Excel files, you just need to change the extension to .xlsx or .xls (depending on the file) , as well as specify which tab you are attempting to reference with "which". This is necessary because Excel files can have multiple tabs, whereas that is not possible for s .csv file. Here is some code for referencing the second tab in an Excel workbook:
```r
library(rio)
data3 <- import("data3.xlsx", which=2)
head(data3)
```

I hope this information helps someone. For more information, refer to the [rio package help file](https://cran.r-project.org/web/packages/rio/rio.pdf).
