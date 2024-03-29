## Problem

You have a large base of code with parts possibly maintained and distributed by different people. Each part is organized as a directory of files, like a package. However, instead of having each part installed as a separated named package, you would like all of the parts to join together under a common package prefix.

## Solution

Essentially, the problem here is that you would like to define a top-level Python package that serves as a namespace for a large collection of separately maintained subpackages. This problem often arises in large application frameworks where the framework developers want to encourage users to distribute plug-ins or add-on packages.

To unify separate directories under a common namespace, you organize the code just like a normal Python package, but you omit _\_\_init\_\_.py_ files in the directories where the components are going to join together. To illustrate, suppose you have two different directories of Python code like this:

    foo-package/
        spam/
             blah.py

    bar-package/
        spam/
             grok.py

In these directories, the name `spam` is being used as a common namespace. Observe that there is no _\_\_init\_\_.py_ file in either directory.

Now watch what happens if you add both `foo-package` and `bar-package` to the Python module path and try some imports:

```
>>> import sys
>>> sys.path.extend(['foo-package', 'bar-package'])
>>> import spam.blah
>>> import spam.grok
>>>
```{{execute}}

You’ll observe that, by magic, the two different package directories merge together and you can import either `spam.blah` or `spam.grok`. It just works.

## Discussion

The mechanism at work here is a feature known as a "namespace package." Essentially, a namespace package is a special kind of package designed for merging different directories of code together under a common namespace, as shown. For large frameworks, this can be useful, since it allows parts of a framework to be broken up into separately installed downloads. It also enables people to easily make third-party add-ons and other extensions to such frameworks.

The key to making a namespace package is to make sure there are no _\_\_init\_\_.py_ files in the top-level directory that is to serve as the common namespace. The missing _\_\_init\_\_.py_ file causes an interesting thing to happen on package import. Instead of causing an error, the interpreter instead starts creating a list of all directories that happen to contain a matching package name. A special namespace package module is then created and a read-only copy of the list of directories is stored in its `_path_` variable. For example:

```
>>> import spam
>>> spam.__path__
_NamespacePath(['foo-package/spam', 'bar-package/spam'])
>>>
```{{execute}}

The directories on `_path_` are used when locating further package subcomponents (e.g., when importing `spam.grok` or `spam.blah`).

An important feature of namespace packages is that anyone can extend the namespace with their own code. For example, suppose you made your own directory of code like this:

    my-package/
         spam/
             custom.py

If you added your directory of code to `sys.path` along with the other packages, it would just seamlessly merge together with the other `spam` package directories:

```
>>> import spam.custom
>>> import spam.grok
>>> import spam.blah
>>>
```{{execute}}

As a debugging tool, the main way that you can tell if a package is serving as a namespace package is to check its `_file_` attribute. If it’s missing altogether, the package is a namespace. This will also be indicated in the representation string by the word "namespace":

```
>>> spam.__file__
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'module' object has no attribute '__file__'
>>> spam
<module 'spam' (namespace)>
>>>
```{{execute}}

Further information about namespace packages can be found in [PEP 420](http://www.python.org/dev/peps/pep-0420).