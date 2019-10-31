## Problem

You want to operate on an entire vector at once.

## Solution

The usual arithmetic operators can perform element-wise operations on entire vectors. Many functions operate on entire vectors, too, and return a vector result.

## Discussion

Vector operations are one of R’s great strengths. All the basic arithmetic operators can be applied to pairs of vectors. They operate in an element-wise manner; that is, the operator is applied to corresponding elements from both vectors:

```
v <- c(11, 12, 13, 14, 15)
w <- c(1, 2, 3, 4, 5)
v + w
#> [1] 12 14 16 18 20
v - w
#> [1] 10 10 10 10 10
v * w
#> [1] 11 24 39 56 75
v / w
#> [1] 11.00  6.00  4.33  3.50  3.00
w^v
#> [1] 1.00e+00 4.10e+03 1.59e+06 2.68e+08 3.05e+10
```{{execute}}

Observe that the length of the result here is equal to the length of the original vectors. The reason is that each element comes from a pair of corresponding values in the input vectors.

If one operand is a vector and the other is a scalar, then the operation is performed between every vector element and the scalar:

```
w
#> [1] 1 2 3 4 5
w + 2
#> [1] 3 4 5 6 7
w - 2
#> [1] -1  0  1  2  3
w * 2
#> [1]  2  4  6  8 10
w / 2
#> [1] 0.5 1.0 1.5 2.0 2.5
2^w
#> [1]  2  4  8 16 32
```{{execute}}

For example, you can recenter an entire vector in one expression simply by subtracting the mean of its contents:

```
w
#> [1] 1 2 3 4 5
mean(w)
#> [1] 3
w - mean(w)
#> [1] -2 -1  0  1  2
```{{execute}}

Likewise, you can calculate the _z_\-score of a vector in one expression—subtract the mean and divide by the standard deviation:

```
w
#> [1] 1 2 3 4 5
sd(w)
#> [1] 1.58
(w - mean(w)) / sd(w)
#> [1] -1.265 -0.632  0.000  0.632  1.265
```{{execute}}

Yet the implementation of vector-level operations goes far beyond elementary arithmetic. It pervades the language, and many functions operate on entire vectors. The functions `sqrt` and `log`, for example, apply themselves to every element of a vector and return a vector of results:

```
w <- 1:5
w
#> [1] 1 2 3 4 5
sqrt(w)
#> [1] 1.00 1.41 1.73 2.00 2.24
log(w)
#> [1] 0.000 0.693 1.099 1.386 1.609
sin(w)
#> [1]  0.841  0.909  0.141 -0.757 -0.959
```{{execute}}

There are two great advantages to vector operations. The first and most obvious is convenience. Operations that require looping in other languages are one-liners in R. The second is speed. Most vectorized operations are implemented directly in C code, so they are substantially faster than the equivalent R code you could write.

## See Also

Performing an operation between a vector and a scalar is actually a special case of the Recycling Rule; see [\[recipe-id050\]](#recipe-id050).