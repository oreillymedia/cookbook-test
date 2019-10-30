## Problem

You have a program that has grown beyond a simple script into an application involving multiple files. You’d like to have some easy way for users to run the program.

## Solution

If your application program has grown into multiple files, you can put it into its own directory and add a _\_\_main\_\_.py_ file. For example, you can create a directory like this:

    myapplication/
         spam.py
         bar.py
         grok.py
         \_\_main\_\_.py

If _\_\_main\_\_.py_ is present, you can simply run the Python interpreter on the top-level directory like this:

bash % python3 myapplication

The interpreter will execute the _\_\_main\_\_.py_ file as the main program.

This technique also works if you package all of your code up into a zip file. For example:

    bash % ls
    spam.py    bar.py   grok.py   \_\_main\_\_.py
    bash % zip -r myapp.zip \*.py
    bash % python3 myapp.zip
    ... output from \_\_main\_\_.py ...

## Discussion

Creating a directory or zip file and adding a _\_\_main\_\_.py_ file is one possible way to package a larger Python application. It’s a little bit different than a package in that the code isn’t meant to be used as a standard library module that’s installed into the Python library. Instead, it’s just this bundle of code that you want to hand someone to execute.

Since directories and zip files are a little different than normal files, you may also want to add a supporting shell script to make execution easier. For example, if the code was in a file named _myapp.zip_, you could make a top-level script like this:

    #!/usr/bin/env python3 /usr/local/bin/myapp.zip