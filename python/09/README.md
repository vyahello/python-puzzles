# 09 - Configuring proper logging capabilities for the code
# Syllabus
- Logging levels
- Logging objects
- Configuring logging through a configuration file
    - Configuring logging in Python script
    - Configuring logging in Python package
- Logging tips

# Lesson compendium
## Logging levels
There are 5 default standard logging levels:
- `DEBUG`: Detailed information, typically of interest only when diagnosing problems.
- `INFO`: Confirmation that things are working as expected.
- `WARNING`: An indication that something unexpected happened, or indicative of some problem in the near future (e.g. ‘disk space low’). The software is still working as expected.
- `ERROR`: Due to a more serious problem, the software has not been able to perform some function.
- `CRITICAL`: A serious error, indicating that the program itself may be unable to continue running.

```python
import logging

__logger = logging.getLogger(__name__)

def print_messages(logger):
    logger.debug("The temperature today is 23 degrees by celsius.")
    logger.info("Today's weather is great!")
    logger.warning("It may be raining today...")
    logger.error("The thermometer is broken! Can't get a temperature.")
    logger.critical("A hurricane is coming.")

# the default logging level is `WARNING`
print_messages(__logger)

# enable "debug" logging
__logger.setLevel(logging.DEBUG)
print_messages(__logger)
```

## Logging objects
Python’s logging system:
- each module/package can have its own `Logger`
- each `Logger` could be equipped with multiple `Handler`
- each `Handler` could have its own `Formatter` and `Filter`

Task: we need to make a GET request to <https://google.com> and print the results to
a console and a file.

Implementation:
1. install `requests` - `python -m pip install requests`
2. configure the logging

The code
```python
import requests
import logging

requests.get("https://google.com")

# "requests" package uses "urllib3" package to make HTTP calls.
# That's why we need to configure logging for the "urllib3".
urllib3_logger = logging.getLogger("urllib3")
urllib3_logger.setLevel(logging.DEBUG)

# Logging to a console
## prepare handler
terminal = logging.StreamHandler()
## prepare formatter
terminal.setFormatter(logging.Formatter('%(asctime)s - %(message)s'))
## add handler
urllib3_logger.addHandler(terminal)

requests.get("https://google.com")

# Logging to a file
## prepare handler
requests_log = logging.FileHandler("requests.log")
## prepare formatter
requests_log.setFormatter(logging.Formatter('%(asctime)s - %(levelname)s - %(message)s'))
## add handler
urllib3_logger.addHandler(requests_log)

requests.get("https://google.com")
# check console and file content
```

**Useful Handlers**
1. `StreamHandler` instances send messages to streams (file-like objects).
2. `FileHandler` instances send messages to disk files.
3. `BaseRotatingHandler` is the base class for handlers that rotate log files at a certain point.
4. `RotatingFileHandler` instances send messages to disk files, with support for maximum log file sizes and log file rotation.
5. `TimedRotatingFileHandler` instances send messages to disk files, rotating the log file at certain timed intervals.
6. `SocketHandler` instances send messages to TCP/IP sockets.
7. `DatagramHandler` instances send messages to UDP sockets.
8. `SMTPHandler` instances send messages to a designated email address.
9. `SysLogHandler` instances send messages to a Unix syslog daemon, possibly on a remote machine.
10. `NTEventLogHandler` instances send messages to a Windows NT/2000/XP event log.
11. `MemoryHandler` instances send messages to a buffer in memory, which is flushed whenever specific criteria are met.
12. `HTTPHandler` instances send messages to an HTTP server using either GET or POST semantics.
13. `WatchedFileHandler` instances watch the file they are logging to.
14. `QueueHandler` instances send messages to a queue, such as those implemented in the queue or multiprocessing modules.
15. `NullHandler` instances do nothing with error messages. They are used by library developers who want to use logging.

The `NullHandler`, `StreamHandler` and `FileHandler` classes are defined in the core `logging`
package. The other handlers are defined in a sub-module, `logging.handlers`.

## Correct logging configuration
When you run code, always there is an entry point. And logging configuration should be in it.

### Configuring logging in Python script
Assume you have a `work.py` module
```python
# work.py
import logging
import sys

__log = logging.getLogger(__name__)


def do_work():
    __log.info("I'm ....")


if __name__ == "__main__":
    __log.setLevel(logging.INFO)
    console = logging.StreamHandler(sys.stdout)
    console.setFormatter(logging.Formatter("%(asctime)s - %(message)s"))
    __log.addHandler(console)
    do_work()

```
```bash
python work.py
```

### Configuring logging in Python package
We have `t` package which can be used as a library or just executed.

`__init__.py`: configure silent mode for all loggers of `t`.
```python
# t/__init__.py
import logging

logging.getLogger(__name__).addHandler(logging.NullHandler())

```
`a.py`: a module in `t` package
```python
# t/a.py
import logging

__log = logging.getLogger(__name__)


def awork():
    __log.info("Perform something in 'a' package ....")

```
`b.py`: a module in `t` package
```python
# t/b.py
import logging

__log = logging.getLogger(__name__)


def bwork():
    __log.info("Perform something, even more important, from 'b' package ....")

```
`__main__.py`: an entry point for `t` package
```python
# t/__main__.py
import logging
import sys

from t.a import awork
from t.b import bwork

if __name__ == "__main__":
    log = logging.getLogger("t")
    log.setLevel(logging.INFO)
    console = logging.StreamHandler(sys.stdout)
    console.setFormatter(logging.Formatter("%(asctime)s - %(message)s"))
    log.addHandler(console)
    awork()
    bwork()

```
**Execution mode**
```bash
python -m t
```

**Library mode**
```python
python
from t.a import awork
awork()
```

## Logging tips
**Complex messages formatting**
```python
import logging

log = logging.getLogger(__name__)

a = 100
b = 16.12345

log.warning("a: %s, b: %s", a, b)
```
**Exceptions logging**
```python
def do(show_error):
    try:
        raise RuntimeError("some error")
    except:
        log.error("The error!")
        if show_error:
            log.exception("Will handle the error below!")
        log.warning("Done!")

do(False)
do(True)

```

# References
- <https://docs.python.org/3/library/logging.html>
- <https://docs.python.org/3/howto/logging.html>
- <https://docs.python.org/3/howto/logging-cookbook.html>
- <https://realpython.com/python-logging-source-code/>
- <https://docs.python-guide.org/writing/logging/>
- <https://www.machinelearningplus.com/python/python-logging-guide/>

# Practice
## Task description
There is package called `unrealmath`. It has the following modules:
```python
# unrealmath/addition.py
import random


def add(first, second):
    return first + second - random.randint(1,10)

```
```python
# unrealmath/subtraction.py
import random

def reduce(minuend, subtrahend):
    return minuend - subtrahend + random.randint(1,10)

```
Please
1. add INFO logs for each function which display provided arguments
2. add correct logging configuration to the package
3. create a separate script (`python demo.py`) that will call each module at least once. Also, it has to
write logs in the files separated per module.

The final project structure has to look like
```text
.
├── unrealmath
│   ├── __init__.py
│   ├── addition.py
│   └── subtraction.py
├── addition.log
├── subtraction.log
└── demo.py
```

## Review items

Please send a zip archive with all required files.
