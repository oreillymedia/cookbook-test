## Problem

You want to change the output produced by printing or viewing instances to something more sensible.

## Solution

To change the string representation of an instance, define the `_str_()` and `_repr_()` methods. For example:

```
class Pair(object):
    def __init__(self, x, y):
        self.x = x
        self.y = y
    def __repr__(self):
        return 'Pair({0.x!r}, {0.y!r})'.format(self)
    def __str__(self):
        return '({0.x!s}, {0.y!s})'.format(self)
```{{execute}}

The `_repr_()` method returns the code representation of an instance, and is usually the text you would type to re-create the instance. The built-in `repr()` function returns this text, as does the interactive interpreter when inspecting values. The `_str_()` method converts the instance to a string, and is the output produced by the `str()` and `print()` functions. For example:

```
>>> p = Pair(3, 4)
>>> p
Pair(3, 4)         # __repr__() output
>>> print(p)
(3, 4)             # __str__() output
>>>
```{{execute}}

The implementation of this recipe also shows how different string representations may be used during formatting. Specifically, the special `!r` formatting code indicates that the output of `_repr_()` should be used instead of `_str_()`, the default. You can try this experiment with the preceding class to see this:

```
>>> p = Pair(3, 4)
>>> print('p is {0!r}'.format(p))
p is Pair(3, 4)
>>> print('p is {0}'.format(p))
p is (3, 4)
>>>
```{{execute}}

## Discussion

Defining `_repr_()` and `_str_()` is often good practice, as it can simplify debugging and instance output. For example, by merely printing or logging an instance, a programmer will be shown more useful information about the instance contents.

It is standard practice for the output of `_repr_()` to produce text such that `eval(repr(x)) == x`. If this is not possible or desired, then it is common to create a useful textual representation enclosed in `<` and `>` instead. For example:

```
>>> f = open('file.dat')
>>> f
<_io.TextIOWrapper name='file.dat' mode='r' encoding='UTF-8'>
>>>
```{{execute}}

If no `_str_()` is defined, the output of `_repr_()` is used as a fallback.

The use of `format()` in the solution might look a little funny, but the format code `{0.x}` specifies the `x`\-attribute of argument 0. So, in the following function, the `0` is actually the instance `self`:

```
def __repr__(self):
    return 'Pair({0.x!r}, {0.y!r})'.format(self)
```{{execute}}

As an alternative to this implementation, you could also use the `%` operator and the following code:

```
def __repr__(self):
    return 'Pair(%r, %r)' % (self.x, self.y)
```{{execute}}