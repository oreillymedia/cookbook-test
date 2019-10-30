## Problem

You want to organize your code into a package consisting of a hierarchical collection of modules.

## Solution

Making a package structure is simple. Just organize your code as you wish on the file-system and make sure that every directory defines an _\_\_init\_\_.py_ file. For example:

    graphics/
        \_\_init\_\_.py
        primitive/
             \_\_init\_\_.py
             line.py
             fill.py
             text.py
        formats/
             \_\_init\_\_.py
             png.py
             jpg.py

Once you have done this, you should be able to perform various `import` statements, such as the following:

```
import graphics.primitive.line
from graphics.primitive import line
import graphics.formats.jpg as jpg
```{{execute}}

## Discussion

Defining a hierarchy of modules is as easy as making a directory structure on the filesystem. The purpose of the _\_\_init\_\_.py_ files is to include optional initialization code that runs as different levels of a package are encountered. For example, if you have the statement `import graphics`, the file _graphics/\_\_init\_\_.py_ will be imported and form the contents of the `graphics` namespace. For an import such as `import graphics.formats.jpg`, the files _graphics/\_\_init\_\_.py_ and _graphics/formats/\_\_init\_\_.py_ will both be imported prior to the final import of the _graphics/formats/jpg.py_ file.

More often that not, it’s fine to just leave the _\_\_init\_\_.py_ files empty. However, there are certain situations where they might include code. For example, an _\_\_init\_\_.py_ file can be used to automatically load submodules like this:

```
# graphics/formats/__init__.py

from . import jpg
from . import png
```{{execute}}

For such a file, a user merely has to use a single `import graphics.formats` instead of a separate import for `graphics.formats.jpg` and `graphics.formats.png`.

Other common uses of _\_\_init\_\_.py_ include consolidating definitions from multiple files into a single logical namespace, as is sometimes done when splitting modules. This is discussed in [Splitting a Module into Multiple Files](#modulesplitting).

Astute programmers will notice that Python 3.3 still seems to perform package imports even if no _\_\_init\_\_.py_ files are present. If you don’t define _\_\_init\_\_.py_, you actually create what’s known as a "namespace package," which is described in [Making Separate Directories of Code Import Under a Common Namespace](#namespacepackage). All things being equal, include the _\_\_init\_\_.py_ files if you’re just starting out with the creation of a new package.