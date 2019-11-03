## Problem

You want to remove unneeded variables or functions from your workspace or to erase its contents completely.

## Solution

Use the `rm` function.

## Discussion

Your workspace can get cluttered quickly. The `rm` function removes, permanently, one or more objects from the workspace:

```
x <- 2 * pi
x

rm(x)
x  # Error

```{{execute}}

There is no “undo”; once the variable is gone, it’s gone.

You can remove several variables at once:

```
rm(x, y, z)
```{{execute}}

You can even erase your entire workspace at once. The `rm` function has a `list` argument consisting of a vector of names of variables to remove. Recall that the `ls` function returns a vector of variable names; hence, you can combine `rm` and `ls` to erase everything:

```
ls()

rm(list = ls())
ls()

```{{execute}}

Alternatively, you could click the broom icon at the top of the Environment pane in RStudio.

![Environment pane in RStudio](/rc0200/assets/rcbk_0201.png)

Figure 1. Environment pane in RStudio

Warning

Never put `rm(list=ls())` into code you share with others, such as a library function or sample code sent to a mailing list or Stack Overflow. Deleting all the variables in someone else’s workspace is worse than rude and will make you extremely unpopular.

