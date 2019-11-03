## Problem

You want to calculate basic statistics: mean, median, standard deviation, variance, correlation, or covariance.

## Solution

Use one of these functions, assuming that `x` and `y` are vectors:

* `mean(x)`
    
* `median(x)`
    
* `sd(x)`
    
* `var(x)`
    
* `cor(x, y)`
    
* `cov(x, y)`
    

## Discussion

When you first use R you might open the documentation and begin searching for material entitled “Procedures for Calculating Standard Deviation.” It seems that such an important topic would likely require a whole chapter.

It’s not that complicated.

Standard deviation and other basic statistics are calculated by simple functions. Ordinarily, the function argument is a vector of numbers and the function returns the calculated statistic:

```
x <- c(0, 1, 1, 2, 3, 5, 8, 13, 21, 34)
mean(x)

median(x)

sd(x)

var(x)

```{{execute}}

The `sd` function calculates the sample standard deviation, and `var` calculates the sample variance.

The `cor` and `cov` functions can calculate the correlation and covariance, respectively, between two vectors:

```
x <- c(0, 1, 1, 2, 3, 5, 8, 13, 21, 34)
y <- log(x + 1)
cor(x, y)

cov(x, y)

```{{execute}}

All these functions are picky about values that are not available (NA). Even one NA value in the vector argument causes any of these functions to return NA or even halt altogether with a cryptic error:

```
x <- c(0, 1, 1, 2, 3, NA)
mean(x)

sd(x)

```{{execute}}

It’s annoying when R is that cautious, but it is appropriate. You must think carefully about your situation. Does an NA in your data invalidate the statistic? If yes, then R is doing the right thing. If not, you can override this behavior by setting `na.rm=TRUE`, which tells R to ignore the NA values:

```
x <- c(0, 1, 1, 2, 3, NA)
sd(x, na.rm = TRUE)

```{{execute}}

In older versions of R, `mean` and `sd` were smart about data frames. They understood that each column of the data frame is a different variable, so they calculated their statistics for each column individually. This is no longer the case and, as a result, you may read confusing comments online or in older books (like the first edition of this book). In order to apply the functions to each column of a data frame we now need to use a helper function. The tidyverse family of helper functions for this sort of thing is in the `purrr` package. As with other tidyverse packages, this gets loaded when you run `library(tidyverse)`. The function we’ll use to apply a function to each column of a data frame is `map_dbl`:

```
data(cars)

map_dbl(cars, mean)

map_dbl(cars, sd)

map_dbl(cars, median)

```{{execute}}

Notice in this example that `mean` and `sd` each return two values, one for each column defined by the data frame. (Technically, they return a two-element vector whose `names` attribute is taken from the columns of the data frame.)

The `var` function understands data frames without the help of a mapping function. It calculates the covariance between the columns of the data frame and returns the covariance matrix:

```
var(cars)

```{{execute}}

Likewise, if `x` is either a data frame or a matrix, then `cor(x)` returns the correlation matrix and `cov(x)` returns the covariance matrix:

```
cor(cars)

cov(cars)

```{{execute}}

