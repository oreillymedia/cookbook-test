## Problem

You want to reload an already loaded module because you’ve made changes to its source.

## Solution

To reload a previously loaded module, use `imp.reload()`. For example:

```
>>> import spam
>>> import imp
>>> imp.reload(spam)
<module 'spam' from './spam.py'>
>>>
```{{execute}}

## Discussion

Reloading a module is something that is often useful during debugging and development, but which is generally never safe in production code due to the fact that it doesn’t always work as you expect.

Under the covers, the `reload()` operation wipes out the contents of a module’s underlying dictionary and refreshes it by re-executing the module’s source code. The identity of the module object itself remains unchanged. Thus, this operation updates the module everywhere that it has been imported in a program.

However, `reload()` does not update definitions that have been imported using statements such as `from module import name`. To illustrate, consider the following code:

```
# spam.py

def bar():
    print('bar')

def grok():
    print('grok')
```{{execute}}

Now start an interactive session:

```
>>> import spam
>>> from spam import grok
>>> spam.bar()
bar
>>> grok()
grok
>>>
```{{execute}}

Without quitting Python, go edit the source code to _spam.py_ so that the function `grok()` looks like this:

```
def grok():
    print('New grok')
```{{execute}}

Now go back to the interactive session, perform a reload, and try this experiment:

```
>>> import imp
>>> imp.reload(spam)
<module 'spam' from './spam.py'>
>>> spam.bar()
bar
>>> grok()             # Notice old output
grok
>>> spam.grok()        # Notice new output
New grok
>>>
```{{execute}}

In this example, you’ll observe that there are two versions of the `grok()` function loaded. Generally, this is not what you want, and is just the sort of thing that eventually leads to massive headaches.

For this reason, reloading of modules is probably something to be avoided in production code. Save it for debugging or for interactive sessions where you’re experimenting with the interpreter and trying things out.