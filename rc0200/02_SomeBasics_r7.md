## Problem

You want to create a sequence of numbers.

## Solution

Use an \_\_n\_\_:\_\_m\_\_ expression to create the simple sequence _n_, _n_+1, _n_+2, …​, _m_:

```
1:5
#> [1] 1 2 3 4 5
```{{execute}}

Use the `seq` function for sequences with an increment other than 1:

```
seq(from = 1, to = 5, by = 2)
#> [1] 1 3 5
```{{execute}}

Use the `rep` function to create a series of repeated values:

```
rep(1, times = 5)
#> [1] 1 1 1 1 1
```{{execute}}

## Discussion

The colon operator (\_\_n\_\_:\_\_m\_\_) creates a vector containing the sequence _n_, _n_+1, _n_+2, …​, _m_:

```
0:9
#>  [1] 0 1 2 3 4 5 6 7 8 9
10:19
#>  [1] 10 11 12 13 14 15 16 17 18 19
9:0
#>  [1] 9 8 7 6 5 4 3 2 1 0
```{{execute}}

R was clever with the last expression (`9:0`). Because 9 is larger than 0, it counts backward from the starting to ending value. You can also use the colon operator directly with the pipe to pass data to another function:

```
10:20 %>% mean()
```{{execute}}

The colon operator works for sequences that grow by 1 only. The `seq` function also builds sequences but supports an optional third argument, which is the increment:

```
seq(from = 0, to = 20)
#>  [1]  0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
seq(from = 0, to = 20, by = 2)
#>  [1]  0  2  4  6  8 10 12 14 16 18 20
seq(from = 0, to = 20, by = 5)
#> [1]  0  5 10 15 20
```{{execute}}

Alternatively, you can specify a length for the output sequence and then R will calculate the necessary increment:

```
seq(from = 0, to = 20, length.out = 5)
#> [1]  0  5 10 15 20
seq(from = 0, to = 100, length.out = 5)
#> [1]   0  25  50  75 100
```{{execute}}

The increment need not be an integer. R can create sequences with fractional increments, too:

```
seq(from = 1.0, to = 2.0, length.out = 5)
#> [1] 1.00 1.25 1.50 1.75 2.00
```{{execute}}

For the special case of a “sequence” that is simply a repeated value, you should use the `rep` function, which repeats its first argument:

```
rep(pi, times = 5)
#> [1] 3.14 3.14 3.14 3.14 3.14
```{{execute}}

## See Also

See [\[recipe-id047\]](#recipe-id047) for creating a sequence of `Date` objects.