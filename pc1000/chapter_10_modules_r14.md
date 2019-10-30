## Problem

You want to create a new Python environment in which you can install modules and packages. However, you want to do this without installing a new copy of Python or making changes that might affect the system Python installation.

## Solution

You can make a new "virtual" environment using the `pyvenv` command. This command is installed in the same directory as the Python interpreter or possibly in the _Scripts_ directory on Windows. Here is an example:

    bash % pyvenv Spam
    bash %

The name supplied to `pyvenv` is the name of a directory that will be created. Upon creation, the _Spam_ directory will look something like this:

    bash % cd Spam
    bash % ls
    bin		      include	     	  lib		pyvenv.cfg
    bash %

In the _bin_ directory, you’ll find a Python interpreter that you can use. For example:

    bash % Spam/bin/python3
    Python 3.3.0 (default, Oct  6 2012, 15:45:22)
    \[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)\] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>> from pprint import pprint
    >>> import sys
    >>> pprint(sys.path)
    \['',
     '/usr/local/lib/python33.zip',
     '/usr/local/lib/python3.3',
     '/usr/local/lib/python3.3/plat-darwin',
     '/usr/local/lib/python3.3/lib-dynload',
     '/Users/beazley/Spam/lib/python3.3/site-packages'\]
    >>>

A key feature of this interpreter is that its _site-packages_ directory has been set to the newly created environment. Should you decide to install third-party packages, they will be installed here, not in the normal system _site-packages_ directory.

## Discussion

The creation of a virtual environment mostly pertains to the installation and management of third-party packages. As you can see in the example, the `sys.path` variable contains directories from the normal system Python, but the _site-packages_ directory has been relocated to a new directory.

With a new virtual environment, the next step is often to install a package manager, such as `distribute` or `pip`. When installing such tools and subsequent packages, you just need to make sure you use the interpreter that’s part of the virtual environment. This should install the packages into the newly created _site-packages_ directory.

Although a virtual environment might look like a copy of the Python installation, it really only consists of a few files and symbolic links. All of the standard library files and interpreter executables come from the original Python installation. Thus, creating such environments is easy, and takes almost no machine resources.

By default, virtual environments are completely clean and contain no third-party add-ons. If you would like to include already installed packages as part of a virtual environment, create the environment using the `--system-site-packages` option. For example:

    bash % pyvenv --system-site-packages Spam
    bash %

More information about `pyvenv` and virtual environments can be found in [PEP 405](http://www.python.org/dev/peps/pep-0405).