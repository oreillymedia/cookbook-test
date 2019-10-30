## Problem

You’ve written a useful library, and you want to be able to give it away to others.

## Solution

If you’re going to start giving code away, the first thing to do is to give it a unique name and clean up its directory structure. For example, a typical library package might look something like this:

    projectname/
         README.txt
         Doc/
             documentation.txt
         projectname/
            \_\_init\_\_.py
            foo.py
            bar.py
            utils/
                 \_\_init\_\_.py
                 spam.py
                 grok.py
         examples/
            helloworld.py
            ...

To make the package something that you can distribute, first write a _setup.py_ file that looks like this:

```
# setup.py
from distutils.core import setup

setup(name='projectname',
      version='1.0',
      author='Your Name',
      author_email='you@youraddress.com',
      url='http://www.you.com/projectname',
      packages=['projectname', 'projectname.utils'],
)
```{{execute}}

Next, make a file _MANIFEST.in_ that lists various nonsource files that you want to include in your package:

    # MANIFEST.in
    include \*.txt
    recursive-include examples \*
    recursive-include Doc \*

Make sure the _setup.py_ and _MANIFEST.in_ files appear in the top-level directory of your package. Once you have done this, you should be able to make a source distribution by typing a command such as this:

```
% bash python3 setup.py sdist
```{{execute}}

This will create a file such as _projectname-1.0.zip_ or _projectname-1.0.tar.gz_, depending on the platform. If it all works, this file is suitable for giving to others or uploading to the [Python Package Index](http://pypi.python.org).

## Discussion

For pure Python code, writing a plain _setup.py_ file is usually straightforward. One potential gotcha is that you have to manually list every subdirectory that makes up the packages source code. A common mistake is to only list the top-level directory of a package and to forget to include package subcomponents. This is why the specification for `packages` in _setup.py_ includes the list `packages=['projectname', 'projectname.utils']`.

As most Python programmers know, there are many third-party packaging options, including setuptools, distribute, and so forth. Some of these are replacements for the `distutils` library found in the standard library. Be aware that if you rely on these packages, users may not be able to install your software unless they also install the required package manager first. Because of this, you can almost never go wrong by keeping things as simple as possible. At a bare minimum, make sure your code can be installed using a standard Python 3 installation. Additional features can be supported as an option if additional packages are available.

Packaging and distribution of code involving C extensions can get considerably more complicated. [\[ch15cextensions\]](#ch15cextensions) on C extensions has a few details on this. In particular, see [\[simpleextension\]](#simpleextension).