## Problem

Your package includes a datafile that your code needs to read. You need to do this in the most portable way possible.

## Solution

Suppose you have a package with files organized as follows:

mypackage/
    \_\_init\_\_.py
    somedata.dat
    spam.py

Now suppose the file _spam.py_ wants to read the contents of the file _somedata.dat_. To do it, use the following code:

```
# spam.py

import pkgutil
data = pkgutil.get_data(__package__, 'somedata.dat')
```{{execute}}

The resulting variable `data` will be a byte string containing the raw contents of the file.

## Discussion

To read a datafile, you might be inclined to write code that uses built-in I/O functions, such as `open()`. However, there are several problems with this approach.

First, a package has very little control over the current working directory of the interpreter. Thus, any I/O operations would have to be programmed to use absolute filenames. Since each module includes a `_file_` variable with the full path, it’s not impossible to figure out the location, but it’s messy.

Second, packages are often installed as _.zip_ or _.egg_ files, which don’t preserve the files in the same way as a normal directory on the filesystem. Thus, if you tried to use `open()` on a datafile contained in an archive, it wouldn’t work at all.

The `pkgutil.get_data()` function is meant to be a high-level tool for getting a datafile regardless of where or how a package has been installed. It will simply "work" and return the file contents back to you as a byte string.

The first argument to `get_data()` is a string containing the package name. You can either supply it directly or use a special variable, such as `_package_`. The second argument is the relative name of the file within the package. If necessary, you can navigate into different directories using standard Unix filename conventions as long as the final directory is still located within the package.