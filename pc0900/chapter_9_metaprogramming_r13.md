## Problem

You want to change the way in which instances are created in order to implement singletons, caching, or other similar features.

## Solution

As Python programmers know, if you define a class, you call it like a function to create instances. For example:

```
class Spam(object):
    def __init__(self, name):
        self.name = name

a = Spam('Guido')
b = Spam('Diana')
```{{execute}}

If you want to customize this step, you can do it by defining a metaclass and reimplementing its `_call_()` method in some way. To illustrate, suppose that you didn’t want anyone creating instances at all:

```
class NoInstances(type):
    def __call__(self, *args, **kwargs):
        raise TypeError("Can't instantiate directly")

# Example
class Spam(metaclass=NoInstances):
    @staticmethod
    def grok(x):
        print('Spam.grok')
```{{execute}}

In this case, users can call the defined static method, but it’s impossible to create an instance in the normal way. For example:

```
>>> Spam.grok(42)
Spam.grok
>>> s = Spam()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "example1.py", line 7, in __call__
    raise TypeError("Can't instantiate directly")
TypeError: Can't instantiate directly
>>>
```{{execute}}

Now, suppose you want to implement the singleton pattern (i.e., a class where only one instance is ever created). That is also relatively straightforward, as shown here:

```
class Singleton(type):
    def __init__(self, *args, **kwargs):
        self.__instance = None
        super().__init__(*args, **kwargs)

    def __call__(self, *args, **kwargs):
        if self.__instance is None:
            self.__instance = super().__call__(*args, **kwargs)
            return self.__instance
        else:
            return self.__instance

# Example
class Spam(metaclass=Singleton):
    def __init__(self):
        print('Creating Spam')
```{{execute}}

In this case, only one instance ever gets created. For example:

```
>>> a = Spam()
Creating Spam
>>> b = Spam()
>>> a is b
True
>>> c = Spam()
>>> a is c
True
>>>
```{{execute}}

Finally, suppose you want to create cached instances, as described in [\[caching\]](#caching). Here’s a metaclass that implements it:

```
import weakref

class Cached(type):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.__cache = weakref.WeakValueDictionary()

    def __call__(self, *args):
        if args in self.__cache:
            return self.__cache[args]
        else:
            obj = super().__call__(*args)
            self.__cache[args] = obj
            return obj

# Example
class Spam(metaclass=Cached):
    def __init__(self, name):
        print('Creating Spam({!r})'.format(name))
        self.name = name
```{{execute}}

Here’s an example showing the behavior of this class:

```
>>> a = Spam('Guido')
Creating Spam('Guido')
>>> b = Spam('Diana')
Creating Spam('Diana')
>>> c = Spam('Guido')       # Cached
>>> a is b
False
>>> a is c                  # Cached value returned
True
>>>
```{{execute}}

## Discussion

Using a metaclass to implement various instance creation patterns can often be a much more elegant approach than other solutions not involving metaclasses. For example, if you didn’t use a metaclass, you might have to hide the classes behind some kind of extra factory function. For example, to get a singleton, you might use a hack such as the following:

```
class _Spam(object):
    def __init__(self):
        print('Creating Spam')

_spam_instance = None
def Spam():
    global _spam_instance
    if _spam_instance is not None:
        return _spam_instance
    else:
        _spam_instance = _Spam()
        return _spam_instance
```{{execute}}

Although the solution involving metaclasses involves a much more advanced concept, the resulting code feels cleaner and less hacked together.

See [\[caching\]](#caching) for more information on creating cached instances, weak references, and other details.