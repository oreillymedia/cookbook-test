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

v - w

v * w

v / w

w^v

```{{execute}}

Observe that the length of the result here is equal to the length of the original vectors. The reason is that each element comes from a pair of corresponding values in the input vectors.

If one operand is a vector and the other is a scalar, then the operation is performed between every vector element and the scalar:

```
w

w + 2

w - 2

w * 2

w / 2

2^w

```{{execute}}

For example, you can recenter an entire vector in one expression simply by subtracting the mean of its contents:

```
w

mean(w)

w - mean(w)

```{{execute}}

Likewise, you can calculate the _z_\-score of a vector in one expression—subtract the mean and divide by the standard deviation:

```
w

sd(w)

(w - mean(w)) / sd(w)

```{{execute}}

Yet the implementation of vector-level operations goes far beyond elementary arithmetic. It pervades the language, and many functions operate on entire vectors. The functions `sqrt` and `log`, for example, apply themselves to every element of a vector and return a vector of results:

```
w <- 1:5
w

sqrt(w)

log(w)

sin(w)

```{{execute}}

There are two great advantages to vector operations. The first and most obvious is convenience. Operations that require looping in other languages are one-liners in R. The second is speed. Most vectorized operations are implemented directly in C code, so they are substantially faster than the equivalent R code you could write.

