## Problem

 You need to create an instance, but want to bypass the execution of the `_init_()` method for some reason.

## Solution

A bare uninitialized instance can be created by directly calling the `_new_()` method of a class. For example, consider this class:

```
class Date(object):
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day
```{{execute}}

Here’s how you can create a `Date` instance without invoking `_init_()`:

```
>>> d = Date.__new__(Date)
>>> d
<__main__.Date object at 0x1006716d0>
>>> d.year
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Date' object has no attribute 'year'
>>>
```{{execute}}

As you can see, the resulting instance is uninitialized. Thus, it is now your responsibility to set the appropriate instance variables. For example:

```
>>> data = {'year':2012, 'month':8, 'day':29}
>>> for key, value in data.items():
...     setattr(d, key, value)
...
>>> d.year
2012
>>> d.month
8
>>>
```{{execute}}

## Discussion

The problem of bypassing `_init_()` sometimes arises when instances are being created in a nonstandard way such as when deserializing data or in the implementation of a class method that’s been defined as an alternate constructor. For example, on the `Date` class shown, someone might define an alternate constructor `today()` as follows:

```
from time import localtime

class Date(object):
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    @classmethod
    def today(cls):
        d = cls.__new__(cls)
        t = localtime()
        d.year = t.tm_year
        d.month = t.tm_mon
        d.day = t.tm_mday
        return d
```{{execute}}

Similarly, suppose you are deserializing JSON data and, as a result, produce a dictionary like this:

```
data = { 'year': 2012, 'month': 8, 'day': 29 }
```{{execute}}

If you want to turn this into a `Date` instance, simply use the technique shown in the solution.

When creating instances in a nonstandard way, it’s usually best to not make too many assumptions about their implementation. As such, you generally don’t want to write code that directly manipulates the underlying instance dictionary `_dict_` unless you know it’s guaranteed to be defined. Otherwise, the code will break if the class uses `_slots_`, properties, descriptors, or other advanced techniques. By using `setattr()` to set the values, your code will be as general purpose as possible.