## Problem

You want scripts and simple programs to write diagnostic information to log files.

## Solution

The easiest way to add logging to simple programs is to use the logging module. For example:

```
import logging

def main():
    # Configure the logging system
    logging.basicConfig(
        filename='app.log',
        level=logging.ERROR
    )

    # Variables (to make the calls that follow work)
    hostname = 'www.python.org'
    item = 'spam'
    filename = 'data.csv'
    mode = 'r'

    # Example logging calls (insert into your program)
    logging.critical('Host %s unknown', hostname)
    logging.error("Couldn't find %r", item)
    logging.warning('Feature is deprecated')
    logging.info('Opening file %r, mode=%r', filename, mode)
    logging.debug('Got here')

if __name__ == '__main__':
    main()
```{{execute}}

The five logging calls (`critical()`, `error()`, `warning()`, `info()`, `debug()`) represent different severity levels in decreasing order. The `level` argument to `basicConfig()` is a filter. All messages issued at a level lower than this setting will be ignored.

The argument to each logging operation is a message string followed by zero or more arguments. When making the final log message, the `%` operator is used to format the message string using the supplied arguments.

If you run this program, the contents of the file _app.log_ will be as follows:

    CRITICAL:root:Host www.python.org unknown
    ERROR:root:Could not find 'spam'

If you want to change the output or level of output, you can change the parameters to the `basicConfig()` call. For example:

```
logging.basicConfig(
     filename='app.log',
     level=logging.WARNING,
     format='%(levelname)s:%(asctime)s:%(message)s')
```{{execute}}

As a result, the output changes to the following:

    CRITICAL:2012-11-20 12:27:13,595:Host www.python.org unknown
    ERROR:2012-11-20 12:27:13,595:Could not find 'spam'
    WARNING:2012-11-20 12:27:13,595:Feature is deprecated

As shown, the logging configuration is hardcoded directly into the program. If you want to configure it from a configuration file, change the `basicConfig()` call to the following:

```
import logging
import logging.config

def main():
    # Configure the logging system
    logging.config.fileConfig('logconfig.ini')
    ...
```{{execute}}

Now make a configuration file _logconfig.ini_ that looks like this:

    \[loggers\]
    keys=root

    \[handlers\]
    keys=defaultHandler

    \[formatters\]
    keys=defaultFormatter

    \[logger\_root\]
    level=INFO
    handlers=defaultHandler
    qualname=root

    \[handler\_defaultHandler\]
    class=FileHandler
    formatter=defaultFormatter
    args=('app.log', 'a')

    \[formatter\_defaultFormatter\]
    format=%(levelname)s:%(name)s:%(message)s

If you want to make changes to the configuration, you can simply edit the _logconfig.ini_ file as appropriate.

## Discussion

Ignoring for the moment that there are about a million advanced configuration options for the `logging` module, this solution is quite sufficient for simple programs and scripts. Simply make sure that you execute the `basicConfig()` call prior to making any logging calls, and your program will generate logging output.

If you want the logging messages to route to standard error instead of a file, don’t supply any filename information to `basicConfig()`. For example, simply do this:

```
logging.basicConfig(level=logging.INFO)
```{{execute}}

One subtle aspect of `basicConfig()` is that it can only be called once in your program. If you later need to change the configuration of the logging module, you need to obtain the root logger and make changes to it directly. For example:

```
logging.getLogger().level = logging.DEBUG
```{{execute}}

It must be emphasized that this recipe only shows a basic use of the `logging` module. There are significantly more advanced customizations that can be made. An excellent resource for such customization is the ["Logging Cookbook"](http://docs.python.org/3/howto/logging-cookbook.html).