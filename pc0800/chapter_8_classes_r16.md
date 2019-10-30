## Problem

 You’re writing a class, but you want users to be able to create instances in more than the one way provided by `_init_()`.

## Solution

To define a class with more than one constructor, you should use a class method. Here is a simple example:

```
import time

class Date(object):
    # Primary constructor
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    # Alternate constructor
    @classmethod
    def today(cls):
        t = time.localtime()
        return cls(t.tm_year, t.tm_mon, t.tm_mday)
```{{execute}}

To use the alternate constructor, you simply call it as a function, such as `Date.today()`. Here is an example:

```
a = Date(2012, 12, 21)      # Primary
b = Date.today()            # Alternate
```{{execute}}

## Discussion

One of the primary uses of class methods is to define alternate constructors, as shown in this recipe. A critical feature of a class method is that it receives the class as the first argument (`cls`). You will notice that this class is used within the method to create and return the final instance. It is extremely subtle, but this aspect of class methods makes them work correctly with features such as inheritance. For example:

```
class NewDate(Date):
    pass

c = Date.today()      # Creates an instance of Date (cls=Date)
d = NewDate.today()   # Creates an instance of NewDate (cls=NewDate)
```{{execute}}

When defining a class with multiple constructors, you should make the `_init_()` function as simple as possible—​doing nothing more than assigning attributes from given values. Alternate constructors can then choose to perform advanced operations if needed.

Instead of defining a separate class method, you might be inclined to implement the `_init_()` method in a way that allows for different calling conventions. For example:

```
class Date(object):
    def __init__(self, *args):
        if len(args) == 0:
	    t = time.localtime()
	    args = (t.tm_year, t.tm_mon, t.tm_mday)
        self.year, self.month, self.day = args
```{{execute}}

Although this technique works in certain cases, it often leads to code that is hard to understand and difficult to maintain. For example, this implementation won’t show useful help strings (with argument names). In addition, code that creates `Date` instances will be less clear. Compare and contrast the following:

```
a = Date(2012, 12, 21)   # Clear. A specific date.
b = Date()               # ??? What does this do?

# Class method version
c = Date.today()         # Clear. Today's date.
```{{execute}}

As shown, the `Date.today()` invokes the regular `Date._init_()` method by instantiating a `Date()` with suitable year, month, and day arguments. If necessary, instances can be created without ever invoking the `_init_()` method. This is described in the next recipe.