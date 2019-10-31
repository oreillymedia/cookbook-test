## Problem

You want to compare two vectors, or you want to compare an entire vector against a scalar.

## Solution

The comparison operators (`==`, `!=`, `<`, `>`, `⇐`, `>=`) can perform an element-by-element comparison of two vectors. They can also compare a vector’s element against a scalar. The result is a vector of logical values in which each value is the result of one element-wise comparison.

## Discussion

R has two logical values, `TRUE` and `FALSE`. These are often called _Boolean_ values in other programming languages.

The comparison operators compare two values and return `TRUE` or `FALSE`, depending upon the result of the comparison:

```
a <- 3
a == pi # Test for equality
#> [1] FALSE
a != pi # Test for inequality
#> [1] TRUE
a < pi
#> [1] TRUE
a > pi
#> [1] FALSE
a <= pi
#> [1] TRUE
a >= pi
#> [1] FALSE
```{{execute}}

You can experience the power of R by comparing entire vectors at once. R will perform an element-by-element comparison and return a vector of logical values, one for each comparison:

```
v <- c(3, pi, 4)
w <- c(pi, pi, pi)
v == w # Compare two 3-element vectors
#> [1] FALSE  TRUE FALSE
v != w
#> [1]  TRUE FALSE  TRUE
v < w
#> [1]  TRUE FALSE FALSE
v <= w
#> [1]  TRUE  TRUE FALSE
v > w
#> [1] FALSE FALSE  TRUE
v >= w
#> [1] FALSE  TRUE  TRUE
```{{execute}}

You can also compare a vector against a single scalar, in which case R will expand the scalar to the vector’s length and then perform the element-wise comparison. The previous example can be simplified in this way:

```
v <- c(3, pi, 4)
v == pi # Compare a 3-element vector against one number
#> [1] FALSE  TRUE FALSE
v != pi
#> [1]  TRUE FALSE  TRUE
```{{execute}}

This is an application of the Recycling Rule discussed in [\[recipe-id050\]](#recipe-id050).

After comparing two vectors, you often want to know whether _any_ of the comparisons were true or whether _all_ the comparisons were true. The `any` and `all` functions handle those tests. They both test a logical vector. The `any` function returns `TRUE` if any element of the vector is `TRUE`. The `all` function returns `TRUE` if all elements of the vector are `TRUE`:

```
v <- c(3, pi, 4)
any(v == pi) # Return TRUE if any element of v equals pi
#> [1] TRUE
all(v == 0) # Return TRUE if all elements of v are zero
#> [1] FALSE
```{{execute}}

## See Also

See [Selecting Vector Elements](#recipe-id039).