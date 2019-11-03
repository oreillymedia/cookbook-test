## Problem

You want to create a sequence of numbers.

## Solution

Use an \_\_n\_\_:\_\_m\_\_ expression to create the simple sequence _n_, _n_+1, _n_+2, …​, _m_:

```
1:5

```{{execute}}

Use the `seq` function for sequences with an increment other than 1:

```
seq(from = 1, to = 5, by = 2)

```{{execute}}

Use the `rep` function to create a series of repeated values:

```
rep(1, times = 5)

```{{execute}}

## Discussion

The colon operator (\_\_n\_\_:\_\_m\_\_) creates a vector containing the sequence _n_, _n_+1, _n_+2, …​, _m_:

```
0:9

10:19

9:0

```{{execute}}

R was clever with the last expression (`9:0`). Because 9 is larger than 0, it counts backward from the starting to ending value. You can also use the colon operator directly with the pipe to pass data to another function:

```
10:20 %>% mean()
```

The colon operator works for sequences that grow by 1 only. The `seq` function also builds sequences but supports an optional third argument, which is the increment:

```
seq(from = 0, to = 20)

seq(from = 0, to = 20, by = 2)

seq(from = 0, to = 20, by = 5)

```{{execute}}

Alternatively, you can specify a length for the output sequence and then R will calculate the necessary increment:

```
seq(from = 0, to = 20, length.out = 5)

seq(from = 0, to = 100, length.out = 5)

```{{execute}}

The increment need not be an integer. R can create sequences with fractional increments, too:

```
seq(from = 1.0, to = 2.0, length.out = 5)

```{{execute}}

For the special case of a “sequence” that is simply a repeated value, you should use the `rep` function, which repeats its first argument:

```
rep(pi, times = 5)

```{{execute}}

