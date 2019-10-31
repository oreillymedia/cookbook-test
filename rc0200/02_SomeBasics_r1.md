## Problem

You want to display the value of a variable or expression.

## Solution

If you simply enter the variable name or expression at the command prompt, R will print its value. Use the `print` function for generic printing of any object. Use the `cat` function for producing custom-formatted output.

## Discussion

It’s very easy to ask R to print something—just enter it at the command prompt:

```
pi
#> [1] 3.14
sqrt(2)
#> [1] 1.41
```{{execute}}

When you enter expressions like these, R evaluates the expression and then implicitly calls the `print` function. So the previous example is identical to this:

```
print(pi)
#> [1] 3.14
print(sqrt(2))
#> [1] 1.41
```{{execute}}

The beauty of `print` is that it knows how to format any R value for printing, including structured values such as matrices and lists:

```
print(matrix(c(1, 2, 3, 4), 2, 2))
#>      [,1] [,2]
#> [1,]    1    3
#> [2,]    2    4
print(list("a", "b", "c"))
#> [[1]]
#> [1] "a"
#>
#> [[2]]
#> [1] "b"
#>
#> [[3]]
#> [1] "c"
```{{execute}}

This is useful because you can always view your data: just `print` it. You need not write special printing logic, even for complicated data structures.

The `print` function has a significant limitation, however: it prints only one object at a time. Trying to `print` multiple items gives this mind-numbing error message:

```
print("The zero occurs at", 2 * pi, "radians.")
#> Error in print.default("The zero occurs at", 2 * pi, "radians."):
#>     invalid 'quote' argument
```{{execute}}

The only way to `print` multiple items is to print them one at a time, which probably isn’t what you want:

```
print("The zero occurs at")
#> [1] "The zero occurs at"
print(2 * pi)
#> [1] 6.28
print("radians")
#> [1] "radians"
```{{execute}}

The `cat` function is an alternative to `print` that lets you concatenate multiple items into a continuous output:

```
cat("The zero occurs at", 2 * pi, "radians.", "\n")
#> The zero occurs at 6.28 radians.
```{{execute}}

Notice that `cat` puts a space between each item by default. You must provide a newline character (`\n`) to terminate the line.

The `cat` function can print simple vectors, too:

```
fib <- c(0, 1, 1, 2, 3, 5, 8, 13, 21, 34)
cat("The first few Fibonacci numbers are:", fib, "...\n")
#> The first few Fibonacci numbers are: 0 1 1 2 3 5 8 13 21 34 ...
```{{execute}}

Using `cat` gives you more control over your output, which makes it especially useful in R scripts that generate output consumed by others. A serious limitation, however, is that it cannot print compound data structures such as matrices and lists. Trying to `cat` them only produces another mind-numbing message:

```
cat(list("a", "b", "c"))
#> Error in cat(list("a", "b", "c")): argument 1 (type 'list') cannot
#>     be handled by 'cat'
```{{execute}}

## See Also

See [\[recipe-id036\]](#recipe-id036) for controlling output format.