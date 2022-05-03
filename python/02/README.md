# 02 - Mastering the functions and built-in types
# Syllabus
- lambda function
- variable-length argument list
- built-in functions
  - operations with numbers
  - calculating logical expressions
  - handling the sequences
  - exploring objects
  - interactive flows
- comprehensions

# Lesson compendium
## Lambda function
```python
# regular function
def always_true():
    return True
always_true
always_true()
# lambda with no args
always_correct = lambda : True
always_correct()
always_correct
# lambda with args
multiply = lambda x : x * 4
multiply(2)

# do something with 2 numbers
def reduce(operation, a, b):
    return operation(a,b)

reduce(lambda a, b: a + b, 2 , 4)
reduce(lambda a, b: a - b, 2 , 4)
reduce(lambda a, b: a * b, 2 , 4)

# alternative with regular function
def add(a,b):
    return a + b

reduce(add, 2 , 4)
```

## Variable-length argument list
Positional arguments
```python
def magic(a, b, c=1):
    print(a*b-c)
magic(3,4,1)
magic(c=5,a=1,b=7)
# *args - varargs
def say(*args):
    print("type is", type(args))
    for element in args:
        print(type(element), element)
say(1)
say(1,2,3)
# unpacking
data = [4,5,7]
say(data)
say(*data)
```
Keyword arguments
```
# **kwargs - 'keywords' arguments
def explain(**kwargs):
    print(type(kwargs))
    for key, value in kwargs.items():
        print(key, value)
explain(word=1)
explain(first=1, second=None)
# unpacking
letters = {"a": 1, "b": 2, "c": 3}
explain(**letters)
```
Unpacking for regular functions
```
magic(**letters)
```

## Built-in functions
### Operations with numbers
```python
# return the absolute value of a number
abs(3)
abs(3.1)
# return x to the power y
pow(2, 3)
# return number rounded
round(3.14)
round(3.14, 1)
```

### Calculating logical expressions
```python
# return True if all elements of the iterable are true (or if the iterable is empty)
all((True, True, False))
all((True, True))
# return True if any element of the iterable is true. If the iterable is empty, return False.
any((False, False))
any((False, True))
```

### Handling the sequences
```python
# a sequence of numbers
for element in range(2, 10, 3):
    print(element)
# do 3 times
for _ in range(3):
    print("Python")
# sum
sum(range(5))
# filtering
list(filter(lambda x: x % 2==0, range(5)))
# sequence size
len(range(4))
# mapping
list(map(lambda x: str(x * 3), range(4)))
# limits
max(range(7))
min(range(7))
# reverse
next(reversed(range(7)))
# sorting
sorted('hello')
# aggregation of sequences
list(zip(("a","b","c"), range(1,4)))
dict(zip(("a","b","c"), range(1,4)))
```

### Exploring objects
```python
help()
help(dir)
dir(list)
```

### Interactive flows
```python
def say(name, something):
    print(f"{name}: {something}")

say("Script", "Please answer the 2 questions.\n")
say(input("What's your name?"), input("Do you want tee?"))
```

## Comprehensions
```python
# task: obtain a list of all the integers in a sequence and then square them
numbers = [1, '4', 9, 'a', 0, 4, 0]
## standard functions
list(
    map(
        lambda e: e**2,
        filter(lambda e: type(e) == int, numbers)
    )
)
## list comprehension
[e**2 for e in numbers if type(e) == int]
## set comprehension: only unique numbers
{e**2 for e in numbers if type(e) == int}

# task: obtain keys with non-empty values
mapping = {
"a": None,
"b": 1,
"c": "",
"d": 2,
"f": 4,
}
# dict comprehension
{key:value for key, value in mapping.items() if value}
```

# References
- <https://docs.python.org/3/library/functions.html#built-in-functions>
- <https://docs.python.org/3/tutorial/controlflow.html#more-on-defining-functions>
- <https://realpython.com/python-lambda/>
- <https://www.python.org/dev/peps/pep-3102/>
- <https://www.python.org/dev/peps/pep-0570/>
- <https://docs.python.org/3/tutorial/datastructures.html?highlight=comprehensions#list-comprehensions>
- <https://docs.python.org/3/howto/functional.html?highlight=comprehensions#generator-expressions-and-list-comprehensions>
- <http://book.pythontips.com/en/latest/comprehensions.html>
- <https://docs.python.org/3/library/stdtypes.html>

# Practice
## Task description
Write a program which provides the statistic about given file. It has to print the following statistic about an
arbitrary file:
1. a number of lines
2. a number of empty lines
3. a number of lines with letter "z"
4. a number of "z" letters in the file
5. a number of lines with "and" group (for instance, "andromeda" has "and" as well as "you and me").

Sample file (`./storage/run2019.txt`):
```
zzzzzzzzz
zzz
zzzzzzzzzzzzzzzzzz

andromeda

and

z and a and z

```
Sample statistic:
```
>>>>
File: ./storage/run2019.txt
  total lines:      10
  empty lines:      4
  lines with "z":   4
  "z" count":       34
  lines with "and": 3
>>>>
```

Please take into account, that the program
- has to ask a user about a path to a file
- has to ask a user if one more file needs to be analyzed
- has to stop only if a user wants to stop

## Review items
Please send Python module with the implementation.
