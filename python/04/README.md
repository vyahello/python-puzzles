# 04 - Using classes and objects for complex tasks
# Syllabus
- class definition and instantiation
- using objects
  - operating with a new unique object
  - defining object-specific behavior
- types of class methods
  - instance methods and `self`
  - `classmethod`s
  - `staticmethod`s
  - properties
  - magic methods
- types of class variables
- specific cases
  - named tuples
  - `__slots__` and performance optimization
  - enums

# Lesson compendium
## Class definition and instantiation
An object is either a real or imaginary creature.

A class is unified description of an object.

An instance (of a class / an object) is a concrete creature.
```python
class Empty:
    pass

dir(Empty)
type(Empty)

e = Empty()
type(e)

isinstance(e, Empty)
isinstance(e, str)
```

## Using objects
### Operating with a new unique object
An object allows composing a new type based on existing ones.
```python
# built-in data structures
employees = (
    ('Jon', 'Dee', 'IT support'),
    ('Barbara', 'Rock', 'Accounting'),
    ('Alice', 'Merton', 'Singer'),
)

singer = employees[2]
# remember that index 1 stands for a last name
singer[1]

# classes
class Employee:
    def __init__(self, first_name, last_name, department):
        self.first_name = first_name
        self.last_name = last_name
        self.department = department

employees2 = (
    Employee('Jon', 'Dee', 'IT support'),
    Employee('Barbara', 'Rock', 'Accounting'),
    Employee('Alice', 'Merton', 'Singer'),
)

singer2 = employees2[2]
singer2.last_name
```

### Defining object-specific behavior
An object allows binding a type-specific behavior.
```python
# built-in data structures
employees = (
    ('Jon', 'Dee', 'IT support'),
    ('Barbara', 'Rock', 'Accounting'),
    ('Alice', 'Merton', 'Singer'),
)

def full_name(employee):
    # depend on structure of external `employee`
    return ' '.join((employee[0], employee[1]))

singer = employees[2]
full_name(singer)

# classes
class Employee:
    def __init__(self, first_name, last_name, department):
        self.first_name = first_name
        self.last_name = last_name
        self.department = department
    
    def full_name(self):
        # depend on internal structure only
        # all in one place
        return ' '.join((self.first_name, self.last_name))

employees2 = (
    Employee('Jon', 'Dee', 'IT support'),
    Employee('Barbara', 'Rock', 'Accounting'),
    Employee('Alice', 'Merton', 'Singer'),
)

singer2 = employees2[2]
singer2.full_name()
```

## Types of class methods
### Instance methods and `self`
Instance methods are expressing object's behavior (what it can do).
```python
class Employee:
    def __init__(self, first_name, last_name, department):
        print(self)
        self.first_name = first_name
        self.last_name = last_name
        self.department = department
    
    def full_name(self):
        print(self)
        return ' '.join((self.first_name, self.last_name))

e1 = Employee('Jon', 'Dee', 'IT support')
e1
e1.full_name()
e2 = Employee('Alice', 'Merton', 'Singer')
e2.full_name()
```

### `classmethod`s
Class methods are expressing alternate ways of objects creation mostly.
```python
class Employee:
    def __init__(self, first_name, last_name, department):
        print(self)
        self.first_name = first_name
        self.last_name = last_name
        self.department = department
    
    def full_name(self):
        print(self)
        return ' '.join((self.first_name, self.last_name))
    
    @classmethod
    def from_csv_line(cls, line):
        print(cls)
        return cls(*line.split(','))

e1 = Employee('Jon', 'Dee', 'IT support')
e1.full_name()
e2 = Employee.from_csv_line('Alice,Merton,Singer')
e2.full_name()
```

### `staticmethod`s
Static methods exist, but just use functions instead of them.
```python
# wrong
class Employee:
    
    @staticmethod
    def add(a, b):
        return a + b

Employee.add(2,3)

# right
def add(a, b):
    return a + b

add(2, 3)
```

### Properties
Properties allows adding extra logic for object's variables without modification of public interface.  
```python
# `department` always has to be in lover case.
class Employee:
    def __init__(self, first_name, last_name, department):
        self.first_name = first_name
        self.last_name = last_name
        self.department = department
    
    def full_name(self):
        return ' '.join((self.first_name, self.last_name))
    
    @property
    def department(self):
        return self._department
    
    @department.setter
    def department(self, department):
        self._department = department.lower()


jon = Employee('Jon', 'Dee', 'IT support')
jon.department
jon.department = "SALES"
jon.department
```

### Magic methods
A method that is called implicitly by Python to execute a certain operation on a type, such as
addition. Such methods have names starting and ending with double underscores.
```python
class Employee:
    def __init__(self, first_name, last_name, department):
        self.first_name = first_name
        self.last_name = last_name
        self.department = department
    
    def full_name(self):
        return ' '.join((self.first_name, self.last_name))
    
    def __str__(self):
        return self.full_name()


class Employees:
    def __init__(self, company, *employees):
        self.company = company
        self._data = employees
    
    def __len__(self):
        return len(self._data)
    
    def __contains__(self, employee):
        return employee in self._data
    
    def __iter__(self):
        return iter(self._data)
    
    def __str__(self):
        return f'"{self.company}"" size is {len(self)} employees.'


jon = Employee('Jon', 'Dee', 'IT support')
allo_employees = Employees(
    "ALLO",
    jon,
    Employee('Barbara', 'Rock', 'Accounting'),
    Employee('Alice', 'Merton', 'Singer'),
)
print(allo_employees)
print("Does Jon work in the company?", jon in allo_employees)
print("List of all employees:")
for employee in allo_employees:
    print('- ', employee)
```

## Types of class variables
An instance variable allows having different value of the variable across all objects (like `first_name` below).

A class variable allows having the same value of the variable across all objects (like `CSV_DELIMITER` below).

```python
class Employee:
    CSV_DELIMITER = ','
    
    def __init__(self, first_name, last_name, department):
        self.first_name = first_name
        self.last_name = last_name
        self.department = department
    
    def full_name(self):
        return ' '.join((self.first_name, self.last_name))
    
    @classmethod
    def from_csv_line(cls, line):
        return cls(*line.split(cls.CSV_DELIMITER))

Employee.CSV_DELIMITER

Employee.from_csv_line('Alice,Merton,Singer').first_name

# redefine for whole program
Employee.CSV_DELIMITER = ';'
Employee.CSV_DELIMITER
Employee.from_csv_line('Alice,Merton,Singer').first_name
Employee.from_csv_line('Alice;Merton;Singer').first_name
```

## Specific cases
### Named tuples
Named tuples are basically easy-to-create, lightweight object types.

```python
from collections import namedtuple
Department = namedtuple('Department', ('name', 'abbreviation'))

accounting = Department('Accounting', 'ACC')
isinstance(accounting, Department)
isinstance(accounting, tuple)
# access abbreviation
accounting.abbreviation
accounting[1]
```
Benefit of named tuples is built-in immutability:
```python
accounting.abbreviation = 'ddd'
```
Disadvantage is a behaviour like tuple:
```python
name, abbreviation = accounting
# what if Department is updated
Department = namedtuple('Department', ('id', 'name', 'abbreviation'))
accounting = Department(123, 'Accounting', 'ACC')
name, abbreviation = accounting
```
So, use them carefully. Or, better, don't use at all.

### `__slots__` and performance optimization
`slots` provides decrease of memory usage and faster access to class variables (attributes).

```python
class Department:
    __slots__ = 'name', 'abbreviation'
    
    def __init__(self, name, abbreviation):
        self.name = name
        self.abbreviation = abbreviation


accounting = Department('Accounting', 'ACC')
# unable assigning of new attributes
accounting.id = 123


class FullName:
    def __init__(self, forename, surname):
        self.forename = forename
        self.surname = surname


barbara = FullName('Barbara', 'Rock')

# see slots
set(dir(accounting)) - set(dir(barbara))
# see regular
set(dir(barbara)) - set(dir(accounting))
```

### Enums
Enums are useful if there is a limited list of choices/options/selections. They allow having them as
a set of objects bonded to the same type.
```python
from enum import Enum

class Department(Enum):
    ACCOUNTING = 'accounting'
    IT_SUPPORT = 'it support'
    UNDEFINED = 'undefined'
    
    @classmethod
    def from_str(cls, department):
        for available in cls:
            if available.value == department.lower():
                return available
        return cls.UNDEFINED


# type
type(Department)
type(Department.ACCOUNTING)
# representation
Department.ACCOUNTING
Department.ACCOUNTING.name
Department.ACCOUNTING.value
# comparison
singer = Department.from_str('singer')
singer is Department.UNDEFINED
singer == Department.UNDEFINED


# instance method of a enum member
class ANSI(Enum):
    BLACK = 30
    RED = 31
    GREEN = 32
    
    def print(self, message):
        print(f"\033[0;{self.value};44m{message}\033[0;0m")

def say_hello(color):
    color.print("Hello, mate!")

say_hello(ANSI.BLACK)
say_hello(ANSI.RED)
say_hello(ANSI.GREEN)
```

# References
- <https://docs.python.org/3.7/tutorial/classes.html>
- <https://docs.python.org/3/library/functions.html#classmethod>
- <https://docs.python.org/3/library/functions.html#staticmethod>
- <https://realpython.com/instance-class-and-static-methods-demystified>
- <https://docs.python.org/3/library/functions.html#property>
- <https://rszalski.github.io/magicmethods>
- <https://docs.python.org/3/reference/datamodel.html#specialnames>
- <https://docs.python.org/3/library/collections.html#namedtuple-factory-function-for-tuples-with-named-fields>
- <https://docs.python.org/3/reference/datamodel.html#slots>
- <https://docs.python.org/3.7/library/enum.html>

# Practice
## Task description
Please write a program which calculates project estimates using <https://en.wikipedia.org/wiki/Three-point_estimation>.
It asks a user for providing available tasks (at least one) with 3 required estimates (`a`, `m`, `b`). After, it
calculates an estimate (E) and a standard deviation (SD) for each task using the following formulae:
```text
E(task) = (a + 4m + b) / 6
SD(task) = (b − a) / 6
```
Finally, it calculates the 95% confidence interval (CI) for the project based on
<https://en.wikipedia.org/wiki/Three-point_estimation#Project_management>. It means that `E(project)` and `SE
(project)` (`SE(task)` is equal to `SD(task)` above) have to be calculated before making the final evaluation of 
`CI(project) = E(project) ± 2 × SE(project)`. The final values have to be printed like:
```text
Project's 95% confidence interval: 100 ... 120 points
```
where `100` and `120` are values of Min and Max `CI(project)`.

## Review items
Please send `.zip` archive of the project along with usage instructions.
