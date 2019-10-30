## Problem

You want to install a third-party package, but you don’t have permission to install packages into the system Python. Alternatively, perhaps you just want to install a package for your own use, not all users on the system.

## Solution

Python has a per-user installation directory that’s typically located in a directory such as _~/.local/lib/python3.3/site-packages_. To force packages to install in this directory, give the `--user` option to the installation command. For example:

     python3 setup.py install --user

or

     pip install --user packagename

The user _site-packages_ directory normally appears before the system _site-packages_ directory on `sys.path`. Thus, packages you install using this technique take priority over the packages already installed on the system (although this is not always the case depending on the behavior of third-party package managers, such as `distribute` or `pip`).

## Discussion

Normally, packages get installed into the system-wide _site-packages_ directory, which is found in a location such as _/usr/local/lib/python3.3/site-packages_. However, doing so typically requires administrator permissions and use of the `sudo` command. Even if you have permission to execute such a command, using `sudo` to install a new, possibly unproven, package might give you some pause.

Installing packages into the per-user directory is often an effective workaround that allows you to create a custom installation.

As an alternative, you can also create a virtual environment, which is discussed in the next recipe.