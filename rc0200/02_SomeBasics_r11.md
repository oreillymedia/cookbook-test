## Problem

Your R expression is producing a curious result, and you wonder if operator precedence is causing problems.

## Solution

The full list of operators is shown in Operator precedence, listed in order of precedence from highest to lowest. Operators of equal precedence are evaluated from left to right except where indicated.

Table 1. Operator precedence  

Operator

Meaning

See also

`[ [[`

Indexing

[Selecting Vector Elements](#recipe-id039)

`:: :::`

Access variables in a namespace (environment)

`$ @`

Component extraction, slot extraction

`^`

Exponentiation (right to left)

`- +`

Unary minus and plus

`:`

Sequence creation

[Creating Sequences](#recipe-id021), [\[recipe-id047\]](#recipe-id047)

%\_\_any\_\_% (including `%>%`)

Special operators

Discussion (this recipe)

`* /`

Multiplication, division

Discussion (this recipe)

`+ -`

Addition, subtraction

`== != < > ⇐ >=`

Comparison

[Comparing Vectors](#recipe-id020)

`!`

Logical negation

`& &&`

Logical “and,” short-circuit “and”

&#x7c; &#x7c;&#x7c;

Logical “or,” short-circuit “or”

`~`

Formula

[\[recipe-id203\]](#recipe-id203)

`→ →>`

Rightward assignment

[Setting Variables](#recipe-id016)

`=`

Assignment (right to left)

[Setting Variables](#recipe-id016)

`← <←`

Assignment (right to left)

[Setting Variables](#recipe-id016)

`?`

Help

[\[recipe-id003\]](#recipe-id003)

It’s not important that you know what every one of these operators does, or what they mean. The list here is intended simply to expose you to the idea that different operators have different precedence.

## Discussion

Getting your operator precedence wrong in R is a common problem. It certainly happens to us a lot. We unthinkingly expect that the expression `0:n-1` will create a sequence of integers from 0 to _n_–1, but it does not:

```
n <- 10
0:n - 1
#>  [1] -1  0  1  2  3  4  5  6  7  8  9
```{{execute}}

It creates the sequence from –1 to _n_–1 because R interprets it as `(0:n)-1`.

You might not recognize the notation %\_any\_% in the table. R interprets any text between percent signs (`%`…​`%`) as a binary operator. Several such operators have predefined meanings:

`%%`

Modulo operator

`%/%`

Integer division

`%*%`

Matrix multiplication

`%in%`

Returns `TRUE` if the left operand occurs in its right operand; `FALSE` otherwise

`%>%`

Pipe that passes results from the left to a function on the right

You can also define new binary operators using the `%`…​`%` notation; see [\[recipe-id114\]](#recipe-id114). The point here is that all such operators have the same precedence.

## See Also

See [Performing Vector Arithmetic](#recipe-id019) for more about vector operations, [\[recipe-id139\]](#recipe-id139) for more about matrix operations, and [\[recipe-id114\]](#recipe-id114) to define your own operators. See also the Arithmetic and Syntax topics in the R help pages as well as Chapters 5 and 6 of [_R in a Nutshell_](https://oreil.ly/2wUtwyf).