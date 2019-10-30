## Problem

You have code organized as a package and want to import a submodule from one of the other package submodules without hardcoding the package name into the import statement.

## Solution

To import modules of a package from other modules in the same package, use a package-relative import. For example, suppose you have a package `mypackage` organized as follows on the filesystem:

    mypackage/
        \_\_init\_\_.py
        A/
            \_\_init\_\_.py
            spam.py
            grok.py
        B/
            \_\_init\_\_.py
            bar.py

If the module `mypackage.A.spam` wants to import the module `grok` located in the same directory, it should include an import statement like this:

```
# mypackage/A/spam.py

from . import grok
```{{execute}}

If the same module wants to import the module `B.bar` located in a different directory, it can use an import statement like this:

```
# mypackage/A/spam.py

from ..B import bar
```{{execute}}

Both of the import statements shown operate relative to the location of the _spam.py_ file and do not include the top-level package name.

## Discussion

Inside packages, imports involving modules in the same package can either use fully specified absolute names or a relative imports using the syntax shown. For example:

```
# mypackage/A/spam.py

from mypackage.A import grok      # OK
from . import grok                # OK
import grok                       # Error (not found)
```{{execute}}

The downside of using an absolute name, such as `mypackage.A`, is that it hardcodes the top-level package name into your source code. This, in turn, makes your code more brittle and hard to work with if you ever want to reorganize it. For example, if you ever changed the name of the package, you would have to go through all of your files and fix the source code. Similarly, hardcoded names make it difficult for someone else to move the code around. For example, perhaps someone wants to install two different versions of a package, differentiating them only by name. If relative imports are used, it would all work fine, whereas everything would break with absolute names.

The `.` and `..` syntax on the `import` statement might look funny, but think of it as specifying a directory name. `.` means look in the current directory and `..B` means look in the _../B_ directory. This syntax only works with the `from` form of import. For example:

```
from . import grok         # OK
import .grok               # ERROR
```{{execute}}

Although it looks like you could navigate the filesystem using a relative import, they are not allowed to escape the directory in which a package is defined. That is, combinations of dotted name patterns that would cause an import to occur from a non-package directory cause an error.

Finally, it should be noted that relative imports only work for modules that are located inside a proper package. In particular, they do not work inside simple modules located at the top level of scripts. They also won’t work if parts of a package are executed directly as a script. For example:

% python3 mypackage/A/spam.py      # Relative imports fail

On the other hand, if you execute the preceding script using the `-m` option to Python, the relative imports will work properly. For example:

% python3 -m mypackage.A.spam      # Relative imports work

For more background on relative package imports, see [PEP 328](http://www.python.org/dev/peps/pep-0328).