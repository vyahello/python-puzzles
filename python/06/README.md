# 06 - Managing unpredictable behavior with exceptions
# Syllabus
- what an exception is
- exceptions hierarchy
- handling exceptions
  - `try`...`except`
  - `try`...`else`...`finally`
  - `contextlib.suppress`
  - re-raising Exceptions
- custom exceptions

# Lesson compendium
## What exception is
An exception is an error that happens during execution of a program. When that error occurs,
Python generate an exception that can be handled, which avoids your program to crash.
```python
# Wrong type
2 + '1'
# Wrong math
100 / 0
# Wrong list access
numbers = [1,2,3]
numbers[1]
numbers[4]
numbers['apples']
# Errors mechanism
error = Exception("Just a error.")
type(error)
repr(error)
error.args
raise error
# how to raise
raise Exception
raise Exception()
```

## Exceptions hierarchy
Visit on <https://docs.python.org/3/library/exceptions.html#exception-hierarchy> so see the latest one.
```txt
BaseException
 +-- SystemExit
 +-- KeyboardInterrupt
 +-- GeneratorExit
 +-- Exception
      +-- StopIteration
      +-- StopAsyncIteration
      +-- ArithmeticError
      |    +-- FloatingPointError
      |    +-- OverflowError
      |    +-- ZeroDivisionError
      +-- AssertionError
      +-- AttributeError
      +-- BufferError
      +-- EOFError
      +-- ImportError
      |    +-- ModuleNotFoundError
      +-- LookupError
      |    +-- IndexError
      |    +-- KeyError
      +-- MemoryError
      +-- NameError
      |    +-- UnboundLocalError
      +-- OSError
      |    +-- BlockingIOError
      |    +-- ChildProcessError
      |    +-- ConnectionError
      |    |    +-- BrokenPipeError
      |    |    +-- ConnectionAbortedError
      |    |    +-- ConnectionRefusedError
      |    |    +-- ConnectionResetError
      |    +-- FileExistsError
      |    +-- FileNotFoundError
      |    +-- InterruptedError
      |    +-- IsADirectoryError
      |    +-- NotADirectoryError
      |    +-- PermissionError
      |    +-- ProcessLookupError
      |    +-- TimeoutError
      +-- ReferenceError
      +-- RuntimeError
      |    +-- NotImplementedError
      |    +-- RecursionError
      +-- SyntaxError
      |    +-- IndentationError
      |         +-- TabError
      +-- SystemError
      +-- TypeError
      +-- ValueError
      |    +-- UnicodeError
      |         +-- UnicodeDecodeError
      |         +-- UnicodeEncodeError
      |         +-- UnicodeTranslateError
      +-- Warning
           +-- DeprecationWarning
           +-- PendingDeprecationWarning
           +-- RuntimeWarning
           +-- SyntaxWarning
           +-- UserWarning
           +-- FutureWarning
           +-- ImportWarning
           +-- UnicodeWarning
           +-- BytesWarning
           +-- ResourceWarning
```

## Handling exceptions
### `try`...`except`
```python

def wrap(func):
    try:
        func()
    except ZeroDivisionError as error:
        print(f'{type(error)}: {error}')
    except TypeError as typeerr:
        print(typeerr)
    except (NameError, RuntimeError):
        pass
    except:
        print("Something terrible happened")
    print('Done')


wrap(lambda : 2 + '1')
wrap(lambda : 1 / 0)
wrap(lambda : [1,2,3][4])
```

### `try`...`else`...`finally`
```python
def split_5_on(number):
    try:
        print("try: Work started!")
        5 / int(number)
    except ZeroDivisionError:
        print("except: You can't divide by zero!")
    except ValueError:
        print("except: You can't convert word to a number!")
    except Exception as e:
        print("except: Exception: " + repr(e))
    else:
        print("else: No exception.")
    finally:
        print("finally: Cleaning the environment.")

# try...except...finally
split_5_on(0)
# try...else...finally
split_5_on(1)
# try...except...finally
split_5_on("spam")
```

### `contextlib.suppress`
We need to remove file, if it's present, and run the logic.
```python
import os


def no_error_handling():
    print("Remove file.")
    os.remove('sfsdfsdfsdfsdf')
    print("Run the logic...")


no_error_handling()

# use "try"
def error_handling_with_try():
    try:
        print("Remove file.")
        os.remove('sfsdfsdfsdfsdf')
    except FileNotFoundError:
        pass
    print("Run the logic...")


error_handling_with_try()


# use "contextlib"
import contextlib


def error_handling_with_contextlib():
    with contextlib.suppress(FileNotFoundError):
        print("Remove file.")
        os.remove('sfsdfsdfsdfsdf')
    print("Run the logic...")

error_handling_with_contextlib()
```

### Re-raising Exceptions
```python
# bad way
try:
    raise ValueError
except:
    print("No value")
    raise Exception("work ended with an error!")


# good way
try:
    raise ValueError
except ValueError as error:
    print("No value")
    raise error
```

## Custom exceptions
A custom exception allows differentiating a specific error from standard error.

**Case**: implement an entrance which won't allow people to enter a party if they are either
too young or too old.

Step 1: ask for an age until it is a number
```python
import contextlib

def asked_age():
    answer = input("How old are you? ")
    with contextlib.suppress(Exception):
        return int(answer)
    print('The age has to be a number. Got:', answer)
    return asked_age()
```

Step 2: raise appropriate errors in case of wrong age
```python
class AgeError(ValueError):
    pass


class TooOldPersonError(ValueError):
    def __init__(self, name):
        super().__init__(f"Sorry {name}, you are too old!")


class TooYoungPersonError(ValueError):
    def __init__(self, name):
        super().__init__(f"Sorry {name}, you are too young!")


def enter_party(name, age):
    if age < 18:
        raise TooYoungPersonError(name)
    elif age > 60:
        raise TooOldPersonError(name)
    else:
        print(f"    {name}, welcome to the party!")
```

Step 3: pass people through the entrance
```python
def pass_people():
    print("Type empty name to stop")
    while True:
        try:
            name = input("What is your name? ")
            if not name:
                break
            enter_party(name, asked_age())
        except (TooYoungPersonError, TooOldPersonError) as error:
            print(f"    {error}")


pass_people()
```

# References
- <https://docs.python.org/3/library/exceptions.html>
- <https://docs.python.org/3/tutorial/errors.html>
- <https://docs.python.org/3/library/contextlib.html#contextlib.suppress>

# Practice
## Task description
There is a decorator which measures execution time of decorated function
```python
import functools
import time

def timer(func):
    """Prints the runtime of the decorated function."""
    @functools.wraps(func)
    def wrapper_timer(*args, **kwargs):
        start_time = time.perf_counter()
        value = func(*args, **kwargs)
        end_time = time.perf_counter()
        run_time = end_time - start_time
        print(f"Finished {func.__name__!r} in {run_time:.4f} secs")
        return value
    return wrapper_timer
```
The current implementation will measure execution time only if a decorated function doesn't raise
an error. But the current execution time won't be shown if some error will occur.

So, you need to update existing code in order to make it showing execution time regardless of
errors in decorated function. And if an error happens, it has to be raised.

## Review items
Please send a Python module for review which proves that updated decorator works regardless of
errors in decorated function. The module should be executable using `python <module>.py`.
