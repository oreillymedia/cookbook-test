## Problem

 A decorator has been applied to a function, but you want to "undo" it, gaining access to the original unwrapped function.

## Solution

Assuming that the decorator has been implemented properly using `@wraps` (see [Preserving Function Metadata When Writing Decorators](#decoratormeta)), you can usually gain access to the original function by accessing the `_wrapped_` attribute. For example:

```
>>> @somedecorator
>>> def add(x, y):
...     return x + y
...
>>> orig_add = add.__wrapped__
>>> orig_add(3, 4)
7
>>>
```{{execute}}

## Discussion

Gaining direct access to the unwrapped function behind a decorator can be useful for debugging, introspection, and other operations involving functions. However, this recipe only works if the implementation of a decorator properly copies metadata using `@wraps` from the `functools` module or sets the `_wrapped_` attribute directly.

If multiple decorators have been applied to a function, the behavior of accessing `_wrapped_` is currently undefined and should probably be avoided. In Python 3.3, it bypasses all of the layers. For example, suppose you have code like this:

```
from functools import wraps

def decorator1(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print('Decorator 1')
        return func(*args, **kwargs)
    return wrapper

def decorator2(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print('Decorator 2')
        return func(*args, **kwargs)
    return wrapper

@decorator1
@decorator2
def add(x, y):
    return x + y
```{{execute}}

Here is what happens when you call the decorated function and the original function through `_wrapped_`:

```
>>> add(2, 3)
Decorator 1
Decorator 2
5
>>> add.__wrapped__(2, 3)
5
>>>
```{{execute}}

However, this behavior has been reported as a bug (see [http://bugs.python.org/issue17482](http://bugs.python.org/issue17482)) and may be changed to explose the proper decorator chain in a future release.

Last, but not least, be aware that not all decorators utilize `@wraps`, and thus, they may not work as described. In particular, the built-in decorators    `@staticmethod` and `@classmethod` create descriptor objects that don’t follow this convention (instead, they store the original function in a `_func_` attribute). Your mileage may vary.