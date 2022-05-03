# 01 - Writing a Python script
# Syllabus
- Python Enhancement Proposals
- built-in types
  - numeric
  - sequence
  - iterator
  - mapping
- operations on built-in types
  - math
  - boolean
  - comparison
- functions
  - declaration
  - arguments
  - default arguments
  - return
- flow control statements
- Python module structure

# Lesson compendium
## Python Enhancement Proposals
A language development is managed by Python Enhancement Proposals located at
<https://www.python.org/dev/peps/>. `PEP 8 - Style Guide for Python Code` must be read by all.
It's available at <https://www.python.org/dev/peps/pep-0008/>.

## Built-in types
### Numeric types
```python
# integers; unlimited precision
int(3)
type(3)
int(4.99999)

# floating numbers; limited precision
float(3)
float(3.14)
type(3.14)
float(3.14).is_integer()

# boolean; subtype of integer
True # equal to 1
False # equal to 0
```

### Sequence types
```python
# Strings are immutable sequences of Unicode code points.
# Definitions:
#  - single quotes
#  - double quotes
#  - triple quoted
'allows embedded "double" quotes'
"allows embedded 'single' quotes"
'''Three single quotes'''
"""Three double quotes"""
str('line')

# tuple; immutable structure
(1,)
(1, 'a', 3.4)
tuple('hello')

# list; mutable sequence with duplicates
[1, 2, 3]
list('hello')
list((1, 'a'))

a = list('1')
a.append(' ')
a.append('.')
a.append('.')
a.append('l')
a.append('g')
a
a.remove('l')
a
# sublist; from first element (included) to third one (excluded)
b = a[1:3]
b

# set; unique mutable sequence
set('hello')
```

### Iterator types
```python
# iterator
i = iter([1, 2,])
next(i)
next(i)
next(i)

# iterator and sequences
word = 'new'
si = word.__iter__()
next(si)
```

### Mapping types
```python
dict(a=1,b=4,v=5)
d = {"a":13, "b":14}
d
d["a"]
d["c"]
d["c"] = 56
d
'b' in d
'b' in d.keys()
'v' in d
```

## Operations on built-in types
### Math operators
```python
"f"+"d"
"f"*3
2+2
2-2
2*2
2/2
3%2
3**2
```

### Boolean operators
```python
# logical operators
#  - and
#  - or
#  - not
True
True and True
True and False
False or True
not True
not (True or False and True)
```

### Comparison operators
```python
2 < 2
2 > 2
2 <= 2
2 >= 2
2 == 2
2 != 2
2 is 2

# object comparison
a = [1,]
b = [1,]
# compares objects in the memory
a is b
a is a
# compares values
a == b
```

### Operators implementation
```python
# import 'operator' module of standard library
import operator
# Addition
operator.add(1,3) == 1 + 3
# left part - the implementation
# right part - Python's operator
```

## Functions
```python
# func without arguments
def hello():
    print('hello')
hello
hello()
hello.__name__
# assign func to a variable
say_hello = hello
say_hello
say_hello()
hello()

# func with arguments
def user(name, age):
    print(name, ' ', age)
user("d", 12)
user("d", "d")
user(hello, "d")
user(None, "d")

# func with default arguments
def user_def(name, age = 12):
    print(name, ' ', age)
user_def("dd")
user_def("dd", 14)

# return statement
def sum(a, b):
    return a + b
print(sum(4,5))

def none():
    return None
print(none())

# empty func
def nothing():
    pass
nothing
nothing()
print(nothing())
```

## Flow control statements
```python
# if else; boolean conditions
def score(n):
    if n > 3:
        print("Great! :-)")
    elif n < 2:
        print("Bad! :(")
    else:
        print("Good! :)")
score(5)
score(2)
score(-21)

# for; iteration over sequence
for number in [1,3,4,5,6]:
    print(number)

for letter in "Python is great!":
    print(3 * letter)


# break & for + else
def check_a(line):
    for i in line:
        if 'a' == i:
            print("There is at least one 'a'")
            break
    else:
        print("There is no 'a'")

check_a('gghjdghdghdg')
check_a('adfgadfgadg')

# continue & for
def odds(numbers):
    for number in numbers:
        if number % 2 == 0:
            continue
        print(number)
odds([1,2,3,4,5])


# while
def print_until(number):
    print('Start cycle')
    counter = 0
    while counter < number:
        print(counter)
        counter += 1
    print('End cycle')
print_until(3)
print_until(-2)
```

## Python module structure
```python
# file: poem.py
students = "Students"

def say(actor, what):
    print(actor, ": ", what)

def main(parameters):
    if parameters:
        program = "program"
        say(program, "You provided me with the following arguments:")
        say(program, parameters)
    else:
        teacher = "Teacher"
        say(teacher, "Run 'python poem.py'")
        interpreter = "Python"
        say(interpreter, "I read *.py file starting from the beginning of the file.")
        say(interpreter, "First, assign 'students' variable.")
        say(interpreter, "Then, I remember 'say' and 'entry' functions.")
        say(interpreter, "Finally, I executes \"if __name__ == '__main__':\" section.")
        say(teacher, "This is the entry point for our module.")
        say(teacher, "'__main__' function executes only if you run the module.")
        say(students, "Wow! But what if we import this module?")
        say(teacher, "'__main__' function won't be executed.")
        say(students, "Great!")


# main block
if __name__ == '__main__':
    import sys
    main(sys.argv[1:])

```

Run the module
```bash
# no arguments
python poem.py
# with arguments
python poem.py hello
python poem.py 1 2 3
python poem.py "An argument with spaces"
```

Import the module
```python
import poem
poem.say("Interactive", "Hello again!")
poem.main(1, 2, 3, "and something")
```

# References
- <https://www.python.org/dev/peps/>
- <https://www.python.org/dev/peps/pep-0008/>
- <https://docs.python.org/3/library/stdtypes.html>
- <https://docs.python.org/3/tutorial/interpreter.html>
- <https://docs.python.org/3/tutorial/controlflow.html>
- <https://docs.python.org/3/library/operator.html#mapping-operators-to-functions>
- <https://docs.python.org/3/tutorial/modules.html?highlight=__name__#executing-modules-as-scripts>

# Practice
## Task description
Please write a script which prints pairs of numbers if their sum is = `10` for a given collection of numbers.
For instance, if `python pairs.py 1 2 3 4 5 5 6 7` is executed, the following is printed:
```bash
3 + 7
4 + 6
5 + 5
```
Please take into account, that the logic of calculation has to be defined outside main block.
Assume that all arguments (if any) are valid numbers (exception handling will be covered later in the course).

## Review items
Please send `pairs.py` along with commands you have used to test the functionality.
