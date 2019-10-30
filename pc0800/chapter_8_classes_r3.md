## Problem

You want to make your objects support the context-management protocol (the `with` statement).

## Solution

In order to make an object compatible with the `with` statement, you need to implement `_enter_()` and `_exit_()` methods. For example, consider the following class, which provides a network connection:

```
from socket import socket, AF_INET, SOCK_STREAM

class LazyConnection(object):
    def __init__(self, address, family=AF_INET, type=SOCK_STREAM):
        self.address = address
        self.family = family
        self.type = type
        self.sock = None

    def __enter__(self):
        if self.sock is not None:
            raise RuntimeError('Already connected')
        self.sock = socket(self.family, self.type)
        self.sock.connect(self.address)
        return self.sock

    def __exit__(self, exc_ty, exc_val, tb):
        self.sock.close()
        self.sock = None
```{{execute}}

The key feature of this class is that it represents a network connection, but it doesn’t actually do anything initially (e.g., it doesn’t establish a connection). Instead, the connection is established and closed using the `with` statement (essentially on demand). For example:

```
from functools import partial

conn = LazyConnection(('www.python.org', 80))
# Connection closed
with conn as s:
    # conn.__enter__() executes: connection open
    s.send(b'GET /index.html HTTP/1.0\r\n')
    s.send(b'Host: www.python.org\r\n')
    s.send(b'\r\n')
    resp = b''.join(iter(partial(s.recv, 8192), b''))
    # conn.__exit__() executes: connection closed
```{{execute}}

## Discussion

The main principle behind writing a context manager is that you’re writing code that’s meant to surround a block of statements as defined by the use of the `with` statement. When the `with` statement is first encountered, the `_enter_()` method is triggered. The return value of `_enter_()` (if any) is placed into the variable indicated with the `as` qualifier. Afterward, the statements in the body of the `with` statement execute. Finally, the `_exit_()` method is triggered to clean up.

This control flow happens regardless of what happens in the body of the `with` statement, including if there are exceptions. In fact, the three arguments to the `_exit_()` method contain the exception type, value, and traceback for pending exceptions (if any). The `_exit_()` method can choose to use the exception information in some way or to ignore it by doing nothing and returning `None` as a result. If `_exit_()` returns `True`, the exception is cleared as if nothing happened and the program continues executing statements immediately after the `with` block.

One subtle aspect of this recipe is whether or not the `LazyConnection` class allows nested use of the connection with multiple `with` statements. As shown, only a single socket connection at a time is allowed, and an exception is raised if a repeated `with` statement is attempted when a socket is already in use. You can work around this limitation with a slightly different implementation, as shown here:

```
from socket import socket, AF_INET, SOCK_STREAM

class LazyConnection(object):
    def __init__(self, address, family=AF_INET, type=SOCK_STREAM):
        self.address = address
        self.family = AF_INET
        self.type = SOCK_STREAM
        self.connections = []

    def __enter__(self):
        sock = socket(self.family, self.type)
        sock.connect(self.address)
	self.connections.append(sock)
	return sock

    def __exit__(self, exc_ty, exc_val, tb):
        self.connections.pop().close()

# Example use
from functools import partial

conn = LazyConnection(('www.python.org', 80))
with conn as s1:
     ...
     with conn as s2:
          ...
          # s1 and s2 are independent sockets
```{{execute}}

In this second version, the `LazyConnection` class serves as a kind of factory for connections. Internally, a list is used to keep a stack. Whenever `_enter_()` executes, it makes a new connection and adds it to the stack. The `_exit_()` method simply pops the last connection off the stack and closes it. It’s subtle, but this allows multiple connections to be created at once with nested `with` statements, as shown.

Context managers are most commonly used in programs that need to manage resources such as files, network connections, and locks. A key part of such resources is they have to be explicitly closed or released to operate correctly. For instance, if you acquire a lock, then you have to make sure you release it, or else you risk deadlock. By implementing `_enter_()`, `_exit_()`, and using the `with` statement, it is much easier to avoid such problems, since the cleanup code in the `_exit_()` method is guaranteed to run no matter what.

An alternative formulation of context managers is found in the `contextmanager` module. See [\[contextmanager\_decorator\]](#contextmanager_decorator). A thread-safe version of this recipe can be found in [\[thread\_local\]](#thread_local).