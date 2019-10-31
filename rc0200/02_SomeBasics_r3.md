## Problem

You want to know what variables and functions are defined in your workspace.

## Solution

Use the `ls` function. Use `ls.str` for more details about each variable. You can also see your variables and functions in the Environment pane in RStudio, shown in the next recipe in [Environment pane in RStudio](#environmentPanel).

## Discussion

The `ls` function displays the names of objects in your workspace:

```
x <- 10
y <- 50
z <- c("three", "blind", "mice")
f <- function(n, p) sqrt(p * (1 - p) / n)
ls()
#> [1] "f" "x" "y" "z"
```{{execute}}

Notice that `ls` returns a vector of character strings in which each string is the name of one variable or function. When your workspace is empty, `ls` returns an empty vector, which produces this puzzling output:

```
ls()
#> character(0)
```{{execute}}

That is R’s quaint way of saying that `ls` returned a zero-length vector of strings; that is, it returned an empty vector because nothing is defined in your workspace.

If you want more than just a list of names, try `ls.str`; this will also tell you something about each variable:

```
x <- 10
y <- 50
z <- c("three", "blind", "mice")
f <- function(n, p) sqrt(p * (1 - p) / n)
ls.str()
#> f : function (n, p)
#> x :  num 10
#> y :  num 50
#> z :  chr [1:3] "three" "blind" "mice"
```{{execute}}

The function is called `ls.str` because it is both listing your variables and applying the `str` function to them, showing their structure (see [\[recipe-id202\]](#recipe-id202)).

Ordinarily, `ls` does not return any name that begins with a dot (`.`). Such names are considered hidden and are not normally of interest to users. (This mirrors the Unix convention of not listing files whose names begin with a dot.) You can force `ls` to list everything by setting the `all.names` argument to `TRUE`:

```
ls()
#> [1] "f" "x" "y" "z"
ls(all.names = TRUE)
#> [1] ".Random.seed" "f"            "x"            "y"
#> [5] "z"
```{{execute}}

The Environment pane in RStudio also hides objects with names that begin with a dot.

## See Also

See [Deleting Variables](#recipe-id075) for deleting variables and [\[recipe-id202\]](#recipe-id202) for inspecting your variables.