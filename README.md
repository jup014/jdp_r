Answers to JDPers’ R Questions
================

## Disclaimer

R is an open-source project. Anyone can contribute to its codebase,
which makes it possible to use multiple ways to achieve a certain
purpose. In programmers way, we call it **There’s more than one way to
do it (TMTOWTDI)**.
(<https://en.wikipedia.org/wiki/There%27s_more_than_one_way_to_do_it>)
Sometimes, they bring us the exactly same results. Sometimes not. I’ll
try not to list all of them here, but if it is well-known that there’s a
noticeable differences between meathods, I’ll let you know.

## Introduction

This document is for UCSD/SDSU JDPers. Anybody can edit the answers.

``` r
print("hello JDPers")
```

    ## [1] "hello JDPers"

``` r
plot(sin(seq(0, 10, by=0.1)), type = 'l')
```

![](README_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

## Package Installation and Loading

There are a few ways to install & load packages. This is the easiest
way.

  - Install a ***pacman*** package, if it is not installed in your
    computer. This is a one-time thing. If you have done it before, you
    don’t have to do it again.

<!-- end list -->

``` r
if ("pacman" %in% rownames(installed.packages()) == FALSE) {
  install.packages("pacman")
}
```

  - From now on, you don’t have to do the ***“if”*** things above for
    the package installation ever. Use this:

<!-- end list -->

``` r
pacman::p_load(tidyverse, readxl, writexl)
```

## File Read

### CSV File read

You can read CSV files with ***read.csv*** function.

``` r
dt2 <- read.csv("test.csv")
dt2
```

    ##   X c1 c2 c3
    ## 1 1  1  2  3
    ## 2 2  2  3  4
    ## 3 3  3  4  5
    ## 4 4  4  5  6
    ## 5 5  5  6  7

### Read XLSX file

You can read XLSX files with ***read\_xlsx*** function in readxl
library.

``` r
pacman::p_load(readxl)

dt3 <- read_excel(path = "test.xlsx")
dt3
```

    ## # A tibble: 5 x 3
    ##      c1    c2    c3
    ##   <dbl> <dbl> <dbl>
    ## 1     1     2     3
    ## 2     2     3     4
    ## 3     3     4     5
    ## 4     4     5     6
    ## 5     5     6     7

### Read .Rdata file (Fastest)

.RData file is a raw data type specifically designed for R. Thus, it is
fastest way to save or load the R data object.

It has a single serious downside. You can’t change the variable name
when you load it up. It automatically loads the data object under
exactly same variable name. Let’s try it here.

``` r
# get the list of current variables
list_of_variable = ls()
list_of_variable
```

    ## [1] "dt2" "dt3"

``` r
if ("dt1" %in% list_of_variable) {
  print("We already has the dt1 variable. I'm going to delete it.")
  rm(dt1)
} else {
  print("We don't have the dt1 variable. Let's proceed.")
}
```

    ## [1] "We don't have the dt1 variable. Let's proceed."

``` r
# let's get the list of variables again
list_of_variable = ls()
list_of_variable
```

    ## [1] "dt2"              "dt3"              "list_of_variable"

``` r
print("We can see there's no dt1 variable")
```

    ## [1] "We can see there's no dt1 variable"

``` r
# let's load the RData file
load("test.Rdata")


# let's get the list of variables again
list_of_variable = ls()
list_of_variable
```

    ## [1] "dt1"              "dt2"              "dt3"              "list_of_variable"

``` r
print("We can see we have dt1 variable again")
```

    ## [1] "We can see we have dt1 variable again"

### \[FYI\] Sample File Generation

This is only for sample data generation. You don’t have to get it now.

``` r
c1 <- c(1,2,3,4,5)
c2 <- c(2,3,4,5,6)
c3 <- c(3,4,5,6,7)
dt1 <- data.frame(c1=c1, c2=c2, c3=c3)
rm(list=c("c1", "c2", "c3"))

# save it to csv
write.csv(x = dt1, file = "test.csv")

# save it to xlsx format
pacman::p_load(writexl)
write_xlsx(x = dt1, path = "test.xlsx")


# save it to Rdata format (THIS IS FASTEST!!)
save(dt1, file = "test.Rdata")
```
