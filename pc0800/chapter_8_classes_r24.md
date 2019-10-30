## Problem

You’d like to be able to compare instances of your class using the standard comparison operators (e.g., `>=`, `!=`, `⇐`, etc.), but without having to write a lot of special methods.

## Solution

Python classes can support comparison by implementing a special method for each comparison operator. For example, to support the `>=` operator, you define a `_ge_()` method in the classes. Although defining a single method is usually no problem, it quickly gets tedious to create implementations of every possible comparison operator.

The `functools.total_ordering` decorator can be used to simplify this process. To use it, you decorate a class with it, and define `_eq_()` and one other comparison method (`_lt_`, `_le_`, `_gt_`, or `_ge_`). The decorator then fills in the other comparison methods for you.

As an example, let’s build some houses and add some rooms to them, and then perform comparisons based on the size of the houses:

```
from functools import total_ordering
class Room(object):
    def __init__(self, name, length, width):
        self.name = name
        self.length = length
        self.width = width
        self.square_feet = self.length * self.width

@total_ordering
class House(object):
    def __init__(self, name, style):
        self.name = name
        self.style = style
        self.rooms = list()

    @property
    def living_space_footage(self):
        return sum(r.square_feet for r in self.rooms)

    def add_room(self, room):
        self.rooms.append(room)

    def __str__(self):
        return '{}: {} square foot {}'.format(self.name,
	                                      self.living_space_footage,
					      self.style)

    def __eq__(self, other):
        return self.living_space_footage == other.living_space_footage

    def __lt__(self, other):
        return self.living_space_footage < other.living_space_footage
```{{execute}}

Here, the `House` class has been decorated with `@total_ordering`. Definitions of `_eq_()` and `_lt_()` are provided to compare houses based on the total square footage of their rooms. This minimum definition is all that is required to make all of the other comparison operations work. For example:

```
# Build a few houses, and add rooms to them
h1 = House('h1', 'Cape')
h1.add_room(Room('Master Bedroom', 14, 21))
h1.add_room(Room('Living Room', 18, 20))
h1.add_room(Room('Kitchen', 12, 16))
h1.add_room(Room('Office', 12, 12))

h2 = House('h2', 'Ranch')
h2.add_room(Room('Master Bedroom', 14, 21))
h2.add_room(Room('Living Room', 18, 20))
h2.add_room(Room('Kitchen', 12, 16))

h3 = House('h3', 'Split')
h3.add_room(Room('Master Bedroom', 14, 21))
h3.add_room(Room('Living Room', 18, 20))
h3.add_room(Room('Office', 12, 16))
h3.add_room(Room('Kitchen', 15, 17))
houses = [h1, h2, h3]

print('Is h1 bigger than h2?', h1 > h2) # prints True
print('Is h2 smaller than h3?', h2 < h3) # prints True
print('Is h2 greater than or equal to h1?', h2 >= h1) # Prints False
print('Which one is biggest?', max(houses)) # Prints 'h3: 1101-square-foot Split'
print('Which is smallest?', min(houses)) # Prints 'h2: 846-square-foot Ranch'
```{{execute}}

## Discussion

If you’ve written the code to make a class support all of the basic comparison operators, then total\_ordering probably doesn’t seem all that magical: it literally defines a mapping from each of the comparison-supporting methods to all of the other ones that would be required. So, if you defined `_lt_()` in your class as in the solution, it is used to build all of the other comparison operators. It’s really just filling in the class with methods like this:

```
class House(object):
    def __eq__(self, other):
        ...
    def __lt__(self, other):
        ...

    # Methods created by @total_ordering
    __le__ = lambda self, other: self < other or self == other
    __gt__ = lambda self, other: not (self < other or self == other)
    __ge__ = lambda self, other: not (self < other)
    __ne__ = lambda self, other: not self == other
```{{execute}}

Sure, it’s not hard to write these methods yourself, but `@total_ordering` simply takes the guesswork out of it.