## Problem

You want to create a vector.

## Solution

Use the `c(…​)` operator to construct a vector from given values.

## Discussion

Vectors are a central component of R, not just another data structure. A vector can contain either numbers, strings, or logical values, but not a mixture.

The `c(…​)` operator can construct a vector from simple elements:

```
c(1, 1, 2, 3, 5, 8, 13, 21)

c(1 * pi, 2 * pi, 3 * pi, 4 * pi)

c("My", "twitter", "handle", "is", "@cmastication")

c(TRUE, TRUE, FALSE, TRUE)

```{{execute}}

If the arguments to `c(…​)` are themselves vectors, it flattens them and combines them into one single vector:

```
v1 <- c(1, 2, 3)
v2 <- c(4, 5, 6)
c(v1, v2)

```{{execute}}

Vectors cannot contain a mix of data types, such as numbers and strings. If you create a vector from mixed elements, R will try to accommodate you by converting one of them:

```
v1 <- c(1, 2, 3)
v3 <- c("A", "B", "C")
c(v1, v3)

```{{execute}}

Here, we tried to create a vector from both numbers and strings. R converted all the numbers to strings before creating the vector, thereby making the data elements compatible. Note that R does this without warning or complaint.

Technically speaking, two data elements can coexist in a vector only if they have the same _mode_. The modes of `3.1415` and `"foo"` are `numeric` and `character`, respectively:

```
mode(3.1415)

mode("foo")

```{{execute}}

Those modes are incompatible. To make a vector from them, R converts `3.1415` to `character` mode so it will be compatible with `"foo"`:

```
c(3.1415, "foo")

mode(c(3.1415, "foo"))

```{{execute}}

Warning

`c` is a generic operator, which means that it works with many data types and not just vectors. However, it might not do exactly what you expect, so check its behavior before applying it to other data types and objects.
