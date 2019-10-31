## Problem

You want to calculate basic statistics: mean, median, standard deviation, variance, correlation, or covariance.

## Solution

Use one of these functions, assuming that `x` and `y` are vectors:

*   `mean(x)`
    
*   `median(x)`
    
*   `sd(x)`
    
*   `var(x)`
    
*   `cor(x, y)`
    
*   `cov(x, y)`
    

## Discussion

When you first use R you might open the documentation and begin searching for material entitled “Procedures for Calculating Standard Deviation.” It seems that such an important topic would likely require a whole chapter.

It’s not that complicated.

Standard deviation and other basic statistics are calculated by simple functions. Ordinarily, the function argument is a vector of numbers and the function returns the calculated statistic:

```
x <- c(0, 1, 1, 2, 3, 5, 8, 13, 21, 34)
mean(x)
#> [1] 8.8
median(x)
#> [1] 4
sd(x)
#> [1] 11
var(x)
#> [1] 122
```{{execute}}

The `sd` function calculates the sample standard deviation, and `var` calculates the sample variance.

The `cor` and `cov` functions can calculate the correlation and covariance, respectively, between two vectors:

```
x <- c(0, 1, 1, 2, 3, 5, 8, 13, 21, 34)
y <- log(x + 1)
cor(x, y)
#> [1] 0.907
cov(x, y)
#> [1] 11.5
```{{execute}}

All these functions are picky about values that are not available (NA). Even one NA value in the vector argument causes any of these functions to return NA or even halt altogether with a cryptic error:

```
x <- c(0, 1, 1, 2, 3, NA)
mean(x)
#> [1] NA
sd(x)
#> [1] NA
```{{execute}}

It’s annoying when R is that cautious, but it is appropriate. You must think carefully about your situation. Does an NA in your data invalidate the statistic? If yes, then R is doing the right thing. If not, you can override this behavior by setting `na.rm=TRUE`, which tells R to ignore the NA values:

```
x <- c(0, 1, 1, 2, 3, NA)
sd(x, na.rm = TRUE)
#> [1] 1.14
```{{execute}}

In older versions of R, `mean` and `sd` were smart about data frames. They understood that each column of the data frame is a different variable, so they calculated their statistics for each column individually. This is no longer the case and, as a result, you may read confusing comments online or in older books (like the first edition of this book). In order to apply the functions to each column of a data frame we now need to use a helper function. The tidyverse family of helper functions for this sort of thing is in the `purrr` package. As with other tidyverse packages, this gets loaded when you run `library(tidyverse)`. The function we’ll use to apply a function to each column of a data frame is `map_dbl`:

```
data(cars)

map_dbl(cars, mean)
#> speed  dist
#>  15.4  43.0
map_dbl(cars, sd)
#> speed  dist
#>  5.29 25.77
map_dbl(cars, median)
#> speed  dist
#>    15    36
```{{execute}}

Notice in this example that `mean` and `sd` each return two values, one for each column defined by the data frame. (Technically, they return a two-element vector whose `names` attribute is taken from the columns of the data frame.)

The `var` function understands data frames without the help of a mapping function. It calculates the covariance between the columns of the data frame and returns the covariance matrix:

```
var(cars)
#>       speed dist
#> speed    28  110
#> dist    110  664
```{{execute}}

Likewise, if `x` is either a data frame or a matrix, then `cor(x)` returns the correlation matrix and `cov(x)` returns the covariance matrix:

```
cor(cars)
#>       speed  dist
#> speed 1.000 0.807
#> dist  0.807 1.000
cov(cars)
#>       speed dist
#> speed    28  110
#> dist    110  664
```{{execute}}

## See Also

See [Avoiding Some Common Mistakes](#recipe-id025), [\[recipe-id057\]](#recipe-id057), and [\[recipe-id132\]](#recipe-id132).