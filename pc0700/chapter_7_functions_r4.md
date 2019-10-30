## Problem

You want to return multiple values from a function.

## Solution

To return multiple values from a function, simply return a tuple. For example:

```
>>> def myfun():
...     return 1, 2, 3
...
>>> a, b, c = myfun()
>>> a
1
>>> b
2
>>> c
3
```{{execute}}

## Discussion

Although it looks like myfun() returns multiple values, a tuple is actually being created. It looks a bit peculiar, but it’s actually the comma that forms a tuple, not the parentheses. For example:

```
>>> a = (1, 2)     # With parentheses
>>> a
(1, 2)
>>> b = 1, 2       # Without parentheses
>>> b
(1, 2)
>>>
```{{execute}}

When calling functions that return a tuple, it is common to assign the result to multiple variables, as shown. This is simply tuple unpacking, as described in [\[iterableunpack\]](#iterableunpack). The return value could also have been assigned to a single variable:

```
>>> x = myfun()
>>> x
(1, 2, 3)
>>>
```{{execute}}