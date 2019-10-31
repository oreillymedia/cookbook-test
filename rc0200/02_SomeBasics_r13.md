## Problem

Creating many intermediate variables in your code is tedious and overly verbose, while nesting R functions makes the code nearly unreadable.

## Solution

Use the pipe operator (`%>%`) to make your expressions easier to read and write. The pipe operator, created by Stefan Bache and found in the `magrittr` package, is used extensively in many `tidyverse` functions as well.

Use the pipe operator to combine multiple functions together into a "pipeline" of functions without intermediate variables:

```
library(tidyverse)
data(mpg)

mpg %>%
  filter(cty > 21) %>%
  head(3) %>%
  print()
#> # A tibble: 3 x 11
#>   manufacturer model  displ  year   cyl trans drv     cty   hwy fl    class
#>   <chr>        <chr>  <dbl> <int> <int> <chr> <chr> <int> <int> <chr> <chr>
#> 1 chevrolet    malibu   2.4  2008     4 auto~ f        22    30 r     mids~
#> 2 honda        civic    1.6  1999     4 manu~ f        28    33 r     subc~
#> 3 honda        civic    1.6  1999     4 auto~ f        24    32 r     subc~
```{{execute}}

Using the pipe is much cleaner and easier to read than using intermediate temporary variables:

```
temp1 <- filter(mpg, cty > 21)
temp2 <- head(temp1, 3)
print(temp2)
#> # A tibble: 3 x 11
#>   manufacturer model  displ  year   cyl trans drv     cty   hwy fl    class
#>   <chr>        <chr>  <dbl> <int> <int> <chr> <chr> <int> <int> <chr> <chr>
#> 1 chevrolet    malibu   2.4  2008     4 auto~ f        22    30 r     mids~
#> 2 honda        civic    1.6  1999     4 manu~ f        28    33 r     subc~
#> 3 honda        civic    1.6  1999     4 auto~ f        24    32 r     subc~
```{{execute}}

## Discussion

The pipe operator does not provide any new functionality to R, but it can greatly improve the readability of code. It takes the output of the function or object on the left of the operator and passes it as the first argument of the function on the right.

Writing this:

```
x %>% head()
```{{execute}}

is functionally the same as writing this:

```
head(x)
```{{execute}}

In both cases `x` is the argument to `head`. We can supply additional arguments, but `x` is always the _first_ argument. These two lines are also functionally identical:

```
x %>% head(n = 10)

head(x, n = 10)
```{{execute}}

This difference may seem small, but with a more complicated example, the benefits begin to accumulate. If we had a workflow where we wanted to use `filter` to limit our data to values, then `select` to keep only certain variables, followed by `ggplot` to create a simple plot, we could use intermediate variables:

```
library(tidyverse)

filtered_mpg <- filter(mpg, cty > 21)
selected_mpg <- select(filtered_mpg, cty, hwy)
ggplot(selected_mpg, aes(cty, hwy)) + geom_point()
```{{execute}}

This incremental approach is fairly readable but creates a number of intermediate data frames and requires the user to keep track of the state of many objects, which can add cognitive load. But the code does produce the desired graph.

An alternative is to nest the functions together:

```
ggplot(select(filter(mpg, cty > 21), cty, hwy), aes(cty, hwy)) + geom_point()
```{{execute}}

While this is very concise since it’s only one line, this code requires much more attention to read and understand what’s going on. Code that is difficult for the user to parse mentally can introduce potential for error, and can also be harder to maintain in the future. Instead, we can use pipes:

```
mpg %>%
  filter(cty > 21) %>%
  select(cty, hwy) %>%
  ggplot(aes(cty, hwy)) + geom_point()
```{{execute}}

The preceding code starts with the `mpg` dataset and pipes it to the `filter` function, which keeps only records where the city mpg value (`cty`) is greater than 21. Those results are piped into the `select` command, which keeps only the listed variables `cty` and `hwy`, and in turn those are piped into the `ggplot` command, which produces the point plot in [Plotting with pipes example](#pipeplotexample).

![rcbk 0202](images/rcbk_0202.png)

Figure 2. Plotting with pipes example

If you want the argument going into your target (righthand side) function to be somewhere other than the first argument, use the dot (`.`) operator. So this:

```
iris %>% head(3)
```{{execute}}

is the same as:

```
iris %>% head(3, x = .)
```{{execute}}

However, in the second example we passed the `iris` data frame into the second named argument using the dot operator. This can be handy for functions where the input data frame goes in a position other than the first argument.

Throughout this book we use pipes to hold together data transformations with multiple steps. We typically format the code with a line break after each pipe and then indent the code on the following lines. This makes the code easily identifiable as parts of the same data pipeline.