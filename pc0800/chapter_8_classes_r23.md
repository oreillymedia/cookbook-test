## Problem

Your program creates data structures with cycles (e.g., trees, graphs, observer patterns, etc.), but you are experiencing problems with memory management.

## Solution

A simple example of a cyclic data structure is a tree structure where a parent points to its children and the children point back to their parent. For code like this, you should consider making one of the links a weak reference using the `weakref` library. For example:

```
import weakref

class Node(object):
    def __init__(self, value):
        self.value = value
        self._parent = None
        self.children = []

    def __repr__(self):
        return 'Node({!r:})'.format(self.value)

    # property that manages the parent as a weak-reference
    @property
    def parent(self):
        return self._parent if self._parent is None else self._parent()

    @parent.setter
    def parent(self, node):
        self._parent = weakref.ref(node)

    def add_child(self, child):
        self.children.append(child)
        child.parent = self
```{{execute}}

This implementation allows the parent to quietly die. For example:

```
>>> root = Node('parent')
>>> c1 = Node('child')
>>> root.add_child(c1)
>>> print(c1.parent)
Node('parent')
>>> del root
>>> print(c1.parent)
None
>>>
```{{execute}}

## Discussion

Cyclic data structures are a somewhat tricky aspect of Python that require careful study because the usual rules of garbage collection often don’t apply. For example, consider this code:

```
# Class just to illustrate when deletion occurs
class Data(object):
    def __del__(self):
        print('Data.__del__')

# Node class involving a cycle
class Node(object):
    def __init__(self):
        self.data = Data()
        self.parent = None
        self.children = []
    def add_child(self, child):
        self.children.append(child)
        child.parent = self
```{{execute}}

Now, using this code, try some experiments to see some subtle issues with garbage collection:

```
>>> a = Data()
>>> del a               # Immediately deleted
Data.__del__
>>> a = Node()
>>> del a               # Immediately deleted
Data.__del__
>>> a = Node()
>>> a.add_child(Node())
>>> del a               # Not deleted (no message)
>>>
```{{execute}}

As you can see, objects are deleted immediately all except for the last case involving a cycle. The reason is that Python’s garbage collection is based on simple reference counting. When the reference count of an object reaches 0, it is immediately deleted. For cyclic data structures, however, this never happens. Thus, in the last part of the example, the parent and child nodes refer to each other, keeping the reference count nonzero.

To deal with cycles, there is a separate garbage collector that runs periodically. However, as a general rule, you never know when it might run. Consequently, you never really know when cyclic data structures might get collected. If necessary, you can force garbage collection, but doing so is a bit clunky:

```
>>> import gc
>>> gc.collect()     # Force collection
Data.__del__
Data.__del__
>>>
```{{execute}}

An even worse problem occurs if the objects involved in a cycle define their own `_del_()` method. For example, suppose the code looked like this:

```
# Class just to illustrate when deletion occurs
class Data(object):
    def __del__(self):
        print('Data.__del__')

# Node class involving a cycle
class Node(object):
    def __init__(self):
        self.data = Data()
        self.parent = None
        self.children = []

    # NEVER DEFINE LIKE THIS.
    # Only here to illustrate pathological behavior
    def __del__(self):
        del self.data
        del.parent
        del.children

    def add_child(self, child):
        self.children.append(child)
        child.parent = self
```{{execute}}

In this case, the data structures will never be garbage collected at all and your program will leak memory! If you try it, you’ll see that the `Data._del_` message never appears at all—​even after a forced garbage collection:

```
>>> a = Node()
>>> a.add_child(Node()
>>> del a             # No message (not collected)
>>> import gc
>>> gc.collect()      # No message (not collected)
>>>
```{{execute}}

Weak references solve this problem by eliminating reference cycles. Essentially, a weak reference is a pointer to an object that does not increase its reference count. You create weak references using the `weakref` library. For example:

```
>>> import weakref
>>> a = Node()
>>> a_ref = weakref.ref(a)
>>> a_ref
<weakref at 0x100581f70; to 'Node' at 0x1005c5410>
>>>
```{{execute}}

To dereference a weak reference, you call it like a function. If the referenced object still exists, it is returned. Otherwise, `None` is returned. Since the reference count of the original object wasn’t increased, it can be deleted normally. For example:

```
>>> print(a_ref())
<__main__.Node object at 0x1005c5410>
>>> del a
Data.__del__
>>> print(a_ref())
None
>>>
```{{execute}}

By using weak references, as shown in the solution, you’ll find that there are no longer any reference cycles and that garbage collection occurs immediately once a node is no longer being used. See [Creating Cached Instances](#caching) for another example involving weak references.