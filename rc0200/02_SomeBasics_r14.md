## Problem

You want to avoid some of the common mistakes made by beginning users—and by experienced users, for that matter!

## Discussion

Here are some easy ways to make trouble for yourself.

##### Forgetting the parentheses after a function invocation

You call an R function by putting parentheses after the name. For instance, this line invokes the `ls` function:

```
ls()
```{{execute}}

However, if you omit the parentheses, R does not execute the function. Instead, it shows the function definition, which is almost never what you want:

```
ls

```{{execute}}

##### Mistyping “<-” as “<(space)-”

The assignment operator is `←`, with no space between the `<` and the `-`:

```
x <- pi # Set x to 3.1415926...
```{{execute}}

If you accidentally insert a space between `<` and `-`, the meaning changes completely:

```
x < -pi # Oops! We are comparing x instead of setting it!

```{{execute}}

This is now a comparison (`<`) between `x` and `-pi` (negative \\(\\pi\\)). It does not change `x`. If you are lucky, `x` is undefined and R will complain, alerting you that something is fishy:

```
x < -pi

```{{execute}}

If `x` is defined, R will perform the comparison and print a logical value, `TRUE` or `FALSE`. That should alert you that something is wrong, as an assignment does not normally print anything:

```
x <- 0 # Initialize x to zero
x < -pi # Oops!

```{{execute}}

##### Incorrectly continuing an expression across lines

R reads your typing until you finish a complete expression, no matter how many lines of input that requires. It prompts you for additional input using the `+` prompt until it is satisfied. This example splits an expression across two lines:

```
total <- 1 + 2 + 3 + # Continued on the next line
  4 + 5
print(total)

```{{execute}}

Problems begin when you accidentally finish the expression prematurely, which can easily happen:

```
total <- 1 + 2 + 3 # Oops! R sees a complete expression
+ 4 + 5 # This is a new expression; R prints its value

print(total)

```{{execute}}

There are two clues that something is amiss: R prompted you with a normal prompt (`>`), not the continuation prompt (`+`), and it printed the value of 4 + 5.

This common mistake is a headache for the casual user. It is a nightmare for programmers, however, because it can introduce hard-to-find bugs into R scripts.

##### Using = instead of ==

Use the double-equals operator (`==`) for comparisons. If you accidentally use the single-equals operator (`=`), you will irreversibly overwrite your variable:

```
v <- 1 # Assign 1 to v
v == 0 # Compare v against zero

v = 0 # Assign 0 to v, overwriting previous contents
print(v)

```{{execute}}

##### Writing 1:n+1 when you mean 1:(n+1)

You might think that `1:n+1` is the sequence of numbers 1, 2, …​, _n_, _n_+1. It’s not. It is the sequence 1, 2, …​, _n_ with 1 added to every element, giving 2, 3, …​, _n_, _n_+1. This happens because R interprets `1:n+1` as `(1:n)+1`. Use parentheses to get exactly what you want:

```
n <- 5
1:n + 1

1:(n + 1)

```{{execute}}

##### Getting bitten by the Recycling Rule

Vector arithmetic and vector comparisons work well when both vectors have the same length. However, the results can be baffling when the operands are vectors of differing lengths. Guard against this possibility by understanding and remembering the Recycling Rule .

##### Installing a package but not loading it with library or require

Installing a package is the first step toward using it, but one more step is required. Use `library` or `require` to load the package into your search path. Until you do so, R will not recognize the functions or datasets in the package :

```
x <- rnorm(100)
n <- 5
truehist(x, n)

```{{execute}}

However, if you load the library first, then the code runs and you get the chart shown in [Example truehist](#histexample):

```
library(MASS) # Load the MASS package into R
truehist(x, n)
```{{execute}}

We typically use `library` instead of `require`. The reason is that if you create an R script that uses `library` and the desired package is not already installed, R will return an error. In contrast, `require` will simply return `FALSE` if the package is not installed.

![truehist example](danf/scenarios/rc0200/assets/rcbk_0203.png)

Figure 3. Example truehist

##### Writing lst\[n\] when you mean lst\[\[n\]\] or vice versa

If the variable `lst` contains a list, it can be indexed in two ways: lst\[\[\_\_n\_\_\]\] is the _n_th element of the list, whereas lst\[\_\_n\_\_\] is a list whose only element is the _n_th element of `lst`. That’s a big difference.

##### Using & instead of &&, or vice versa; same for | and ||

Use `&` and `|` in logical expressions involving the logical values `TRUE` and `FALSE`. 

Use `&&` and `||` for the flow-of-control expressions inside `if` and `while` statements.

Programmers accustomed to other programming languages may reflexively use `&&` and `||` everywhere because “they are faster.” But those operators give peculiar results when applied to vectors of logical values, so avoid them unless you are sure that they do what you want.

##### Passing multiple arguments to a single-argument function

What do you think is the value of `mean(9,10,11)`? No, it’s not 10. It’s 9. The `mean` function computes the mean of the first argument. The second and third arguments are being interpreted as other positional arguments. To pass multiple items into a single argument, we put them in a vector with the `c` operator. `mean(c(9,10,11))` will return 10, as you might expect.

Some functions, such as `mean`, take one argument. Other arguments, such as `max` and `min`, take multiple arguments and apply themselves across all arguments. Be sure you know which are which.

##### Thinking that max behaves like pmax, or that min behaves like pmin

The `max` and `min` functions have multiple arguments and return one value: the maximum or minimum of all their arguments.

The `pmax` and `pmin` functions have multiple arguments but return a vector with values taken element-wise from the arguments. 

##### Misusing a function that does not understand data frames

Some functions are quite clever regarding data frames. They apply themselves to the individual columns of the data frame, computing their result for each individual column. Sadly, not all functions are that bright. This includes the `mean`, `median`, `max`, and `min` functions. They will lump together every value from every column and compute their result from the lump, or possibly just return an error. Be aware of which functions are savvy to data frames and which are not. When in doubt, read the documentation for the function you are considering.

##### Using a single backslash (\\) in Windows paths

It’s common to copy and paste filepaths into your R scripts, but if you’re using R on Windows you need to take care. Windows File Explorer may show you that your path is _C:\\temp\\my\_file.csv_, but if you try to tell R to read that file, you’ll get a cryptic message:

Error: '\\m' is an unrecognized escape in character string starting "'.\\temp\\m"

This is because R sees backslashes as special characters. You can get around this by using either forward slashes (`/`) or double backslashes (`\\`):

```
read_csv(`./temp/my_file.csv`)
read_csv(`.\\temp\\my_file.csv`)
```

This is only an issue on Windows because both Mac and Linux use forward slashes as path separators.

##### Posting a question to Stack Overflow or the mailing list before searching for the answer

Don’t waste your time. Don’t waste other people’s time. Before you post a question to a mailing list or to Stack Overflow, do your homework and search the archives. Odds are, someone has already answered your question. If so, you’ll see the answer in the discussion thread for the question. 

