## Problem

You want to put a wrapper layer around a function that adds extra processing (e.g., logging, timing, etc.).

## Solution

If you ever need to wrap a function with extra code, define a decorator function. For example:

```
import time
from functools import wraps

def timethis(func):
    '''
    Decorator that reports the execution time.
    '''
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(func.__name__, end-start)
        return result
    return wrapper
```{{execute}}

Here is an example of using the decorator:

```
>>> @timethis
... def countdown(n):
...     '''
...     Counts down
...     '''
...     while n > 0:
...             n -= 1
...
>>> countdown(100000)
countdown 0.008917808532714844
>>> countdown(10000000)
countdown 0.87188299392912
>>>
```{{execute}}

## Discussion

A decorator is a function that accepts a function as input and returns a new function as output. Whenever you write code like this:

```
@timethis
def countdown(n):
    ...
```{{execute}}

it’s the same as if you had performed these separate steps:

```
def countdown(n):
    ...
countdown = timethis(countdown)
```{{execute}}

As an aside, built-in decorators such as `@staticmethod`, `@classmethod`, and `@property` work in the same way. For example, these two code fragments are equivalent:

```
class A(object):
    @classmethod
    def method(cls):
        pass

class B(object):
    # Equivalent definition of a class method
    def method(cls):
        pass
    method = classmethod(method)
```{{execute}}

The code inside a decorator typically involves creating a new function that accepts any arguments using `**args**` **and** `*kwargs`, as shown with the `wrapper()` function in this recipe. Inside this function, you place a call to the original input function and return its result. However, you also place whatever extra code you want to add (e.g., timing). The newly created function `wrapper` is returned as a result and takes the place of the original function.

It’s critical to emphasize that decorators generally do not alter the calling signature or return value of the function being wrapped. The use of `*args` and `**kwargs**` **is there to make sure that any input arguments can be accepted. The return value of a decorator is almost always the result of calling `func(*args,`** `kwargs)`, where `func` is the original unwrapped function.

When first learning about decorators, it is usually very easy to get started with some simple examples, such as the one shown. However, if you are going to write decorators for real, there are some subtle details to consider. For example, the use of the decorator `@wraps(func)` in the solution is an easy to forget but important technicality related to preserving function metadata, which is described in the next recipe. The next few recipes that follow fill in some details that will be important if you wish to write decorator functions of your own.