## Problem

You want a function to only accept certain arguments by keyword.

## Solution

This feature is easy to implement if you place the keyword arguments after a **argument or a single unnamed** . For example:

```
def recv(maxsize, *, block):
    'Receives a message'
    pass

recv(1024, True)        #  TypeError
recv(1024, block=True)  # Ok
```{{execute}}

This technique can also be used to specify keyword arguments for functions that accept a varying number of positional arguments. For example:

```
def mininum(*values, clip=None):
    m = min(values)
    if clip is not None:
        m = clip if clip > m else m
    return m

minimum(1, 5, 2, -5, 10)          # Returns -5
minimum(1, 5, 2, -5, 10, clip=0)  # Returns 0
```{{execute}}

## Discussion

Keyword-only arguments are often a good way to enforce greater code clarity when specifying optional function arguments. For example, consider a call like this:

```
msg = recv(1024, False)
```{{execute}}

If someone is not intimately familiar with the workings of the `recv()`, they may have no idea what the `False` argument means. On the other hand, it is much clearer if the call is written like this:

```
msg = recv(1024, block=False)
```{{execute}}

The use of keyword-only arguments is also often preferrable to tricks involving `**kwargs`, since they show up properly when the user asks for help:

```
>>> help(recv)
Help on function recv in module __main__:

recv(maxsize, *, block)
    Receives a message
```{{execute}}

Keyword-only arguments also have utility in more advanced contexts. For example, they can be used to inject arguments into functions that make use of the `**args**` **and** `*kwargs` convention for accepting all inputs. See [\[decoratoraddarg\]](#decoratoraddarg) for an example.