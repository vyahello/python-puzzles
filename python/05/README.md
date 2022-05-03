# 05 - Optimizing code with generators, decorators, and context managers
# Syllabus
- generators
  - `yield` keyword
  - generator expression
  - coroutines
- decorators
  - basics of decoration logic
  - preserve original function information
  - decorators with arguments
- context managers
  - `with` statement
  - context management protocol
  - `contextlib`

# Lesson compendium
## Generators
Generators are a special class of functions that simplify the task of writing iterators. Regular
functions compute a value and return it, but generators return an iterator that returns a stream
of values.

### `yield` keyword
Any function containing a `yield `keyword is a generator function.
```python
def values():
    current = 0
    while True:
        yield current
        current += 1
        if current > 2:
            break

# "numbers" is iterator
numbers = values()
type(numbers)
next(numbers)
next(numbers)
next(numbers)
next(numbers)

# loop
for number in values():
    print('number', number)

# generator from iterable object
def up_to_3():
    yield from range(3)


for number in up_to_3():
    print('number', number)

# class as generator
class Countdown:
    def __init__(self, start):
        self.start = start
    
    def __iter__(self):
        current = self.start
        while current > 0:
            yield current
            current -= 1


for number in Countdown(5):
    print(number)
```

### Generator expression
Generator expressions return an iterator that computes the values as necessary, not needing to
materialize all the values at once.
```python
# remove spare spaces
lines = ['  line 1\n', 'line 2  \n', 'line 3']
normalized_lines = (line.strip() for line in lines)
type(normalized_lines)
next(normalized_lines)
```

### Coroutines
Coroutines are functions which could consume and, in some cases, produce data. They can
pause a stream of execution till new message is sent.

Coroutine consumes data. Case: seeking of a specific word.
```python
def grep(pattern):
    print("Searching for", pattern)
    while True:
        line = (yield)
        if pattern in line:
            print(line)


search = grep('coroutine')
next(search)
search.send("I love you")
search.send("Don't you love me?")
search.send("I love coroutines instead!")
```
Coroutine consumes and produces data. Case: jumping counter.
```python
def counter(maximum):
    i = 0
    while i < maximum:
        val = (yield i)
        # If value provided, change counter
        if val is not None:
            i = val
        else:
            i += 1

it = counter(100)
next(it)
next(it)
next(it)
# change internal state
it.send(6)
next(it)
# disallow iterations
it.close()
next(it)
```
## Decorators
A decorator is a function that allows us to wrap another function in order to extend the behavior
of the wrapped function, without permanently modifying it.

### Basics of decoration logic
```python
# external code decoration
s = sum((1, 2, 4))


## decorator which prints invocation
def verbose(func):
    def wrapper(*args, **kwargs):
        name = func.__name__
        print("{}: *args: {}, **kwargs: {}".format(name, args, kwargs))
        result = func(*args, **kwargs)
        print("{}: return: {}".format(name, result))
        return result
    return wrapper


## decorating function: external or internal
verbose_sum = verbose(sum)
sv = verbose_sum((1, 2, 4))
compare = verbose(lambda a, b: a == b)
compare(s, sv)

# decorating function:  internal only
@verbose
def concatenate(*values):
    return "".join(map(str, values))


concatenate(1, "3ddd", 3.4)
```

### Preserve original function information
```python
print(help(sum))
print(help(verbose_sum))

import functools


def verbose(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        name = func.__name__
        print("{}: *args: {}, **kwargs: {}".format(name, args, kwargs))
        result = func(*args, **kwargs)
        print("{}: return: {}".format(name, result))
        return result
    return wrapper


print(help(verbose(sum)))
```

### Decorators with arguments
```python
def verbose(input=True, output=True):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            name = func.__name__
            if input:
                print("{}: *args: {}, **kwargs: {}".format(name, args, kwargs))
            result = func(*args, **kwargs)
            if output:
                print("{}: return: {}".format(name, result))
            return result
        return wrapper
    return decorator


params = verbose(output=False)(sum)
params((1, 2, 4))


@verbose(input=False, output=True)
def multiply(a, b):
    return a * b


multiply(12, 'abba')
```

## Context managers
A context manager is a Python object that provides extra contextual information to an action.

### `with` statement
```python
with open("data.txt", "w") as data:
    print(data.writable())

```

### Context management protocol
```python
# temporary file
import os


class TemporaryFile:
    def __init__(self, name):
        self.file = open(name, "w")
    
    def __enter__(self):
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()
        os.remove(self.file.name)


def verify(path):
    print("Is {} present?: {}.".format(path, os.path.exists(path)))


file_name = "test.txt"

verify(file_name)

with TemporaryFile(file_name) as storage:
    print(storage.writable())
    verify(file_name)

verify(file_name)
```

### `contextlib`
```python
from contextlib import contextmanager

@contextmanager
def temporary_file(filename):
    tmp_file = open(filename, "w")
    try:
        print(f"{filename} is ready to use.")
        yield tmp_file
    finally:
        tmp_file.close()
        os.remove(filename)
        print(f"{filename} is removed.")


with temporary_file(".secret.txt") as secrets, \
        temporary_file(".public.txt") as public, \
        temporary_file(".generic.txt") as generic:
    secrets.write("....")
    public.write(".....")
    generic.write("...")
```

# References
- <https://www.python.org/dev/peps/pep-0289>
- <https://www.python.org/dev/peps/pep-0342>
- <https://docs.python.org/3/howto/functional.html#generator-expressions-and-list-comprehensions>
- <https://realpython.com/introduction-to-python-generators>
- <https://stackoverflow.com/questions/40925797/why-do-we-need-coroutines-in-python>
- <http://www.dabeaz.com/coroutines/index.html>
- <https://docs.python.org/2.5/whatsnew/pep-342.html>
- <https://www.python.org/dev/peps/pep-0318>
- <https://www.python.org/dev/peps/pep-3129>
- <https://realpython.com/primer-on-python-decorators>
- <https://sumit-ghosh.com/articles/demystifying-decorators-python>
- <https://www.python.org/dev/peps/pep-0343>
- <https://docs.python.org/3/reference/compound_stmts.html#with>
- <https://docs.python.org/3/library/contextlib.html>

# Practice
## Task description
Please write a "questionnaire" application which transforms text files into interactive
command-line questionnaire and reports answers and statistic.

Logical data structure:
- a quiz consists at least from one section
- each section consists at least from one question

Physical data structure:
- a quiz is a directory
- a section is a file in the directory
- a question is a line in the file

The application runs quiz based on given directory and stores all answers in a separate file using
`<question> : <answer>` format. Also, it stores timing (how long it takes) for each individual
section and for the whole quiz in a separate file.

The order of section or questions in not important.

## Review items
Please send `.zip` archive of the project along with usage instructions.