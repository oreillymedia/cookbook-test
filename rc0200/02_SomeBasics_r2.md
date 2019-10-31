## Problem

You want to save a value in a variable.

## Solution

Use the assignment operator (`←`). There is no need to declare your variable first:

```
x <- 3
```{{execute}}

## Discussion

Using R in “calculator mode” gets old pretty fast. Soon you will want to define variables and save values in them. This reduces typing, saves time, and clarifies your work.

There is no need to declare or explicitly create variables in R. Just assign a value to the name and R will create the variable:

```
x <- 3
y <- 4
z <- sqrt(x^2 + y^2)
print(z)
#> [1] 5
```{{execute}}

Notice that the assignment operator is formed from a less-than character (`<`) and a hyphen (`-`) with no space between them.

When you define a variable at the command prompt like this, the variable is held in your workspace. The workspace is held in the computer’s main memory but can be saved to disk. The variable definition remains in the workspace until you remove it.

R is a _dynamically typed language_, which means that we can change a variable’s data type at will. We could set `x` to be numeric, as just shown, and then turn around and immediately overwrite that with (say) a vector of character strings. R will not complain:

```
x <- 3
print(x)
#> [1] 3

x <- c("fee", "fie", "foe", "fum")
print(x)
#> [1] "fee" "fie" "foe" "fum"
```{{execute}}

In some R functions you will see assignment statements that use the strange-looking assignment operator `<←`:

```
x <<- 3
```{{execute}}

That forces the assignment to a global variable rather than a local variable. Scoping is a bit, well, out of scope for this discussion, however.

In the spirit of full disclosure, we will reveal that R also supports two other forms of assignment statements. A single equals sign (`=`) can be used as an assignment operator. A rightward assignment operator (`→`) can be used anywhere the leftward assignment operator (`←`) can be used (but with the arguments reversed):

```
foo <- 3
print(foo)
#> [1] 3
```{{execute}}

```
5 -> fum
print(fum)
#> [1] 5
```{{execute}}

We recommend that you avoid these as well. The equals-sign assignment is easily confused with the test for equality. The rightward assignment can be useful in certain contexts, but it can be confusing to those not used to seeing it.

## See Also

See Recipes [#recipe-id075](#recipe-id075), [#recipe-id025](#recipe-id025), and [#recipe-id009](#recipe-id009). See also the help page for the `assign` function.