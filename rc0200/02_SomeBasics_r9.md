## Problem

You want to extract one or more elements from a vector.

## Solution

Select the indexing technique appropriate for your problem:

*   Use square brackets to select vector elements by their position, such as `v[3]` for the third element of `v`.
    
*   Use negative indexes to exclude elements.
    
*   Use a vector of indexes to select multiple values.
    
*   Use a logical vector to select elements based on a condition.
    
*   Use names to access named elements.
    

## Discussion

Selecting elements from vectors is another powerful feature of R. Basic selection is handled just as in many other programming languages—use square brackets and a simple index:

```
fib <- c(0, 1, 1, 2, 3, 5, 8, 13, 21, 34)
fib

fib[1]

fib[2]

fib[3]

fib[4]

fib[5]

```{{execute}}

Notice that the first element has an index of 1, not 0 as in some other programming languages.

A cool feature of vector indexing is that you can select multiple elements at once. The index itself can be a vector, and each element of that indexing vector selects an element from the data vector:

```
fib[1:3] # Select elements 1 through 3

fib[4:9] # Select elements 4 through 9

```{{execute}}

An index of `1:3` means select elements 1, 2, and 3, as just shown. The indexing vector needn’t be a simple sequence, however. You can select elements anywhere within the data vector—as in this example, which selects elements 1, 2, 4, and 8:

```
fib[c(1, 2, 4, 8)]

```{{execute}}

R interprets negative indexes to mean _exclude_ a value. An index of `–1`, for instance, means exclude the first value and return all other values:

```
fib[-1] # Ignore first element

```{{execute}}

You can extend this method to exclude whole slices by using an indexing vector of negative indexes:

```
fib[1:3] # As before

fib[-(1:3)] # Invert sign of index to exclude instead of select

```{{execute}}

Another indexing technique uses a logical vector to select elements from the data vector. Everywhere that the logical vector is `TRUE`, an element is selected:

```
fib < 10 # This vector is TRUE wherever fib is less than 10

fib[fib < 10] # Use that vector to select elements less than 10

fib %% 2 == 0 # This vector is TRUE wherever fib is even

fib[fib %% 2 == 0] # Use that vector to select the even elements

```{{execute}}

Ordinarily, the logical vector should be the same length as the data vector so you are clearly either including or excluding each element. (If the lengths differ, then you need to understand the Recycling Rule.)

By combining vector comparisons, logical operators, and vector indexing, you can perform powerful selections with very little R code.

For example, you can select all elements greater than the median:

```
v <- c(3, 6, 1, 9, 11, 16, 0, 3, 1, 45, 2, 8, 9, 6, -4)
v[ v > median(v)]

```{{execute}}

or select all elements in the lower and upper 5%:

```
v[ (v < quantile(v, 0.05)) | (v > quantile(v, 0.95)) ]

```{{execute}}

The previous example uses the `|` operator, which means "or" when indexing. If you wanted "and," you would use the `&` operator.

You can also select all elements that exceed ±1 standard deviations from the mean:

```
v[ abs(v - mean(v)) > sd(v)]

```{{execute}}

or select all elements that are neither `NA` nor `NULL`:

```
v <- c(1, 2, 3, NA, 5)
v[!is.na(v) & !is.null(v)]

```{{execute}}

One final indexing feature lets you select elements by name. It assumes that the vector has a `names` attribute, defining a name for each element. You can define the names by assigning a vector of character strings to the attribute:

```
years <- c(1960, 1964, 1976, 1994)
names(years) <- c("Kennedy", "Johnson", "Carter", "Clinton")
years

```{{execute}}

Once the names are defined, you can refer to individual elements by name:

```
years["Carter"]

years["Clinton"]

```{{execute}}

This generalizes to allow indexing by vectors of names; R returns every element named in the index:

```
years[c("Carter", "Clinton")]

```{{execute}}

