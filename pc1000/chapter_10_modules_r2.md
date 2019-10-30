## Problem

You want precise control over the symbols that are exported from a module or package when a user uses the `from module import *` statement.

## Solution

Define a variable `_all_` in your module that explicitly lists the exported names. For example:

```
# somemodule.py

def spam():
    pass

def grok():
    pass

blah = 42

# Only export 'spam' and 'grok'
__all__ = ['spam', 'grok']
```{{execute}}

## Discussion

Although the use of `from module import *` is strongly discouraged, it still sees frequent use in modules that define a large number of names. If you don’t do anything, this form of import will export all names that don’t start with an underscore. On the other hand, if `_all_` is defined, then only the names explicitly listed will be exported.

If you define `_all_` as an empty list, then nothing will be exported. An `AttributeError` is raised on import if `_all_` contains undefined names.