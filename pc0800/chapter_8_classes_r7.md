## Problem

 You want to invoke a method in a parent class in place of a method that has been overridden in a subclass.

## Solution

To call a method in a parent (or superclass), use the `super()` function. For example:

```
class A(object):
    def spam(self):
        print('A.spam')

class B(A):
    def spam(self):
        print('B.spam')
        super().spam()      # Call parent spam()
```{{execute}}

A very common use of `super()` is in the handling of the `_init_()` method to make sure that parents are properly initialized:

```
class A(object):
    def __init__(self):
        self.x = 0

class B(A):
    def __init__(self):
        super().__init__()
        self.y = 1
```{{execute}}

Another common use of `super()` is in code that overrides any of Python’s special methods. For example:

```
class Proxy(object):
    def __init__(self, obj):
        self._obj = obj

    # Delegate attribute lookup to internal obj
    def __getattr__(self, name):
        return getattr(self._obj, name)

    # Delegate attribute assignment
    def __setattr__(self, name, value):
        if name.startswith('_'):
            super().__setattr__(name, value)    # Call original __setattr__
        else:
            setattr(self._obj, name, value)
```{{execute}}

In this code, the implementation of `_setattr_()` includes a name check. If the name starts with an underscore (\_), it invokes the original implementation of `__setattr__()` using `super()`. Otherwise, it delegates to the internally held object `self._obj`. It looks a little funny, but `super()` works even though there is no explicit base class listed.

## Discussion

Correct use of the `super()` function is actually one of the most poorly understood aspects of Python. Occasionally, you will see code written that directly calls a method in a parent like this:

```
class Base(object):
    def __init__(self):
        print('Base.__init__')

class A(Base):
    def __init__(self):
        Base.__init__(self)
        print('A.__init__')
```{{execute}}

Although this "works" for most code, it can lead to bizarre trouble in advanced code involving multiple inheritance. For example, consider the following:

```
class Base(object):
    def __init__(self):
        print('Base.__init__')

class A(Base):
    def __init__(self):
        Base.__init__(self)
        print('A.__init__')

class B(Base):
    def __init__(self):
        Base.__init__(self)
        print('B.__init__')

class C(A,B):
    def __init__(self):
        A.__init__(self)
        B.__init__(self)
        print('C.__init__')
```{{execute}}

If you run this code, you’ll see that the `Base._init_()` method gets invoked twice, as shown here:

```
>>> c = C()
Base.__init__
A.__init__
Base.__init__
B.__init__
C.__init__
>>>
```{{execute}}

Perhaps double-invocation of `Base._init_()` is harmless, but perhaps not. If, on the other hand, you change the code to use `super()`, it all works:

```
class Base(object):
    def __init__(self):
        print('Base.__init__')

class A(Base):
    def __init__(self):
        super().__init__()
        print('A.__init__')

class B(Base):
    def __init__(self):
        super().__init__()
        print('B.__init__')

class C(A,B):
    def __init__(self):
        super().__init__()     # Only one call to super() here
        print('C.__init__')
```{{execute}}

When you use this new version, you’ll find that each `_init_()` method only gets called once:

```
>>> c = C()
Base.__init__
B.__init__
A.__init__
C.__init__
>>>
```{{execute}}

To understand why it works, we need to step back for a minute and discuss how Python implements inheritance. For every class that you define, Python computes what’s known as a method resolution order (MRO) list. The MRO list is simply a linear ordering of all the base classes. For example:

```
>>> C.__mro__
(<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>,
<class '__main__.Base'>, <class 'object'>)
>>>
```{{execute}}

To implement inheritance, Python starts with the leftmost class and works its way left-to-right through classes on the MRO list until it finds the first attribute match.

The actual determination of the MRO list itself is made using a technique known as C3 Linearization. Without getting too bogged down in the mathematics of it, it is actually a merge sort of the MROs from the parent classes subject to three constraints:

*   Child classes get checked before parents
    
*   Multiple parents get checked in the order listed.
    
*   If there are two valid choices for the next class, pick the one from the first parent.
    

Honestly, all you really need to know is that the order of classes in the MRO list "makes sense" for almost any class hierarchy you are going to define.

When you use the `super()` function, Python continues its search starting with the next class on the MRO. As long as every redefined method consistently uses `super()` and only calls it once, control will ultimately work its way through the entire MRO list and each method will only be called once. This is why you don’t get double calls to `Base._init_()` in the second example.

A somewhat surprising aspect of `super()` is that it doesn’t necessarily go to the direct parent of a class next in the MRO and that you can even use it in a class with no direct parent at all. For example, consider this class:

```
class A(object):
    def spam(self):
        print('A.spam')
        super().spam()
```{{execute}}

If you try to use this class, you’ll find that it’s completely broken:

```
>>> a = A()
>>> a.spam()
A.spam
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 4, in spam
AttributeError: 'super' object has no attribute 'spam'
>>>
```{{execute}}

Yet, watch what happens if you start using the class with multiple inheritance:

```
>>> class B(object):
...     def spam(self):
...         print('B.spam')
...
>>> class C(A,B):
...     pass
...
>>> c = C()
>>> c.spam()
A.spam
B.spam
>>>
```{{execute}}

Here you see that the use of `super().spam()` in class A has, in fact, called the `spam()` method in class B—​a class that is completely unrelated to A! This is all explained by the MRO of class C:

```
>>> C.__mro__
(<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>,
<class 'object'>)
>>>
```{{execute}}

Using `super()` in this manner is most common when defining mixin classes. See Recipes [#datamodel](#datamodel) and [#mixins](#mixins).

However, because `super()` might invoke a method that you’re not expecting, there are a few general rules of thumb you should try to follow. First, make sure that all methods with the same name in an inheritance hierarchy have a compatible calling signature (i.e., same number of arguments, argument names). This ensures that `super()` won’t get tripped up if it tries to invoke a method on a class that’s not a direct parent. Second, it’s usually a good idea to make sure that the topmost class provides an implementation of the method so that the chain of lookups that occur along the MRO get terminated by an actual method of some sort.

Use of `super()` is sometimes a source of debate in the Python community. However, all things being equal, you should probably use it in modern code. Raymond Hettinger has written an excellent blog post ["Python’s super() Considered Super!"](http://rhettinger.wordpress.com/2011/05/26/super-considered-super) that has even more examples and reasons why `super()` might be super-awesome.