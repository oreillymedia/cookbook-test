## Problem

You want to define a function or method where one or more of the arguments are optional and have a default value.

## Solution

On the surface, defining a function with optional arguments is easy—​simply assign values in the definition and make sure that default arguments appear last. For example:

```
def spam(a, b=42):
    print(a, b)

spam(1)      # Ok. a=1, b=42
spam(1, 2)   # Ok. a=1, b=2
```{{execute}}

If the default value is supposed to be a mutable container, such as a list, set, or dictionary, use `None` as the default and write code like this:

```
# Using a list as a default value
def spam(a, b=None):
    if b is None:
        b = []
    ...
```{{execute}}

If, instead of providing a default value, you want to write code that merely tests whether an optional argument was given an interesting value or not, use this idiom:

```
_no_value = object()

def spam(a, b=_no_value):
    if b is _no_value:
        print('No b value supplied')
    ...
```{{execute}}

Here’s how this function behaves:

```
>>> spam(1)
No b value supplied
>>> spam(1, 2)     # b = 2
>>> spam(1, None)  # b = None
>>>
```{{execute}}

Carefully observe that there is a distinction between passing no value at all and passing a value of `None`.

## Discussion

Defining functions with default arguments is easy, but there is a bit more to it than meets the eye.

First, the values assigned as a default are bound only once at the time of function definition. Try this example to see it:

```
>>> x = 42
>>> def spam(a, b=x):
...     print(a, b)
...
>>> spam(1)
1 42
>>> x = 23     # Has no effect
>>> spam(1)
1 42
>>>
```{{execute}}

Notice how changing the variable `x` (which was used as a default value) has no effect whatsoever. This is because the default value was fixed at function definition time.

Second, the values assigned as defaults should always be immutable objects, such as `None`, `True`, `False`, numbers, or strings. Specifically, never write code like this:

```
def spam(a, b=[]):     # NO!
    ...
```{{execute}}

If you do this, you can run into all sorts of trouble if the default value ever escapes the function and gets modified. Such changes will permanently alter the default value across future function calls. For example:

```
>>> def spam(a, b=[]):
...     print(b)
...     return b
...
>>> x = spam(1)
>>> x
[]
>>> x.append(99)
>>> x.append('Yow!')
>>> x
[99, 'Yow!']
>>> spam(1)       # Modified list gets returned!
[99, 'Yow!']
>>>
```{{execute}}

That’s probably not what you want. To avoid this, it’s better to assign `None` as a default and add a check inside the function for it, as shown in the solution.

The use of the `is` operator when testing for `None` is a critical part of this recipe. Sometimes people make this mistake:

```
def spam(a, b=None):
    if not b:      # NO! Use 'b is None' instead
        b = []
    ...
```{{execute}}

The problem here is that although `None` evaluates to `False`, many other objects (e.g., zero-length strings, lists, tuples, dicts, etc.) do as well. Thus, the test just shown would falsely treat certain inputs as missing. For example:

```
>>> spam(1)        # OK
>>> x = []
>>> spam(1, x)     # Silent error. x value overwritten by default
>>> spam(1, 0)     # Silent error. 0 ignored
>>> spam(1, '')    # Silent error. '' ignored
>>>
```{{execute}}

The last part of this recipe is something that’s rather subtle—​a function that tests to see whether a value (any value) has been supplied to an optional argument or not. The tricky part here is that you can’t use a default value of `None`, `0`, or `False` to test for the presence of a user-supplied argument (since all of these are perfectly valid values that a user might supply). Thus, you need something else to test against.

To solve this problem, you can create a unique private instance of `object`, as shown in the solution (the `_no_value` variable). In the function, you then check the identity of the supplied argument against this special value to see if an argument was supplied or not. The thinking here is that it would be extremely unlikely for a user to pass the `_no_value` instance in as an input value. Therefore, it becomes a safe value to check against if you’re trying to determine whether an argument was supplied or not.

The use of `object()` might look rather unusual here. `object` is a class that serves as the common base class for almost all objects in Python. You can create instances of `object`, but they are wholly uninteresting, as they have no notable methods nor any instance data (because there is no underlying instance dictionary, you can’t even set any attributes). About the only thing you can do is perform tests for identity. This makes them useful as special values, as shown in the solution.