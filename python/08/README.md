# 08 - Using type hints for managing objects
# Syllabus
- the theory of type hints
- type annotations syntax
- type annotations for built-ins
- custom types
- type checking using `mypy`

# Lesson compendium
## The theory of type hints
Based on **PEP-483** and **PEP-484**:

**_What is it?_**
> Gradual typing is a type system that allows parts of a program to be dynamically typed and other
parts to be statically typed. The programmer controls which parts are which by either leaving out
type annotations or by adding them in.

**_How does type work?_**
> We define a new relationship, is-consistent-with, which is similar to is-subtype-of, except it
is not transitive when the new type Any is involved. Assigning a_value to a_variable is OK if
the type of a_value is consistent with the type of a_variable. The is-consistent-with relationship
is defined by three rules:
> - A type `t1` is consistent with a type `t2` if `t1` is a subtype of `t2`.
> - `Any` is consistent with every type. But `Any` is not a subtype of every type.
> - Every type is consistent with `Any`. But every type is not a subtype of `Any`.

**_Types VS Classes_**
> A class is a dynamic, runtime concept while a type appears as variable, function annotations which
are used by static type checkers.
Every class is a type. But not every type can be a class. For instance, `int` is a class and a type.
But `Any` is a type but not a proper class.

**_Fundamental building blocks_**
1. `Any`. Every type is consistent with Any; and it is also consistent with every type.
2. `Union[t1, t2, ...]`. Types that are subtype of at least one of t1 etc. are subtypes of this.
3. `Optional[t1]`. Alias for `Union[t1, None]`, i.e. `Union[t1, type(None)]`.
4. `Tuple[t1, t2, ..., tn]`. A tuple whose items are instances of `t1`, etc.
   Example: `Tuple[int, float]` means a tuple of two items, the first is an int, the second
   is a float; e.g., `(42, 3.14)`.
5. `Callable[[t1, t2, ..., tn], tr]`. A function with positional argument types `t1` etc., and
   return type `tr`. You can say the argument list is entirely unchecked by writing `Callable[..., tr]`.

## Type annotations syntax
Function annotations syntax (PEP-3107)
```python
# python --version >= 3.5
# parameters
def sum(a: expression, b: expression = 5):
    ...
# return value
def sum(a: expression, b: expression = 5) -> expression:
    ...
# excess parameters
def foo(*args: expression, **kwargs: expression):
    ...
# access annotations
sum.__annotations__
```
Variable annotation syntax (PEP-526)
```python
# python --version >= 3.6
var: annotation
var: annotation = value
```

## Type annotations for built-ins
```python
from typing import List, Set, Dict, Tuple

# For simple built-in types, just use the name of the type
a: int = 1
b: float = 1.0
c: bool = True
d: str = "test"
e: bytes = b"test"

# For collections, the name of the type is capitalized, and the
# name of the type inside the collection is in brackets
f: List[int] = [1]
h: Set[int] = {6, 7}

# For mappings, we need the types of both keys and values
g: Dict[str, float] = {'field': 2.0}

# For tuples, we specify the types of all the elements
j: Tuple[int, str, float] = (3, "yes", 7.5)
```

## Custom types
```python
# a link for myself
from typing import Sequence, Iterator


class Node:
    def __init__(self, name: str, children: Sequence['Node'] = ()) -> None:
        self.name = name
        self._children = children
    
    def has_members(self) -> bool:
        return len(self._children) > 0
    
    def members(self) -> Iterator['Node']:
        return iter(self._children)
    
    def __str__(self) -> str:
        if self.members():
            return f"{self.name} [{','.join(map(str, self._children))}]"
        return self.name


tree: Node = Node("a number", [Node(str(number)) for number in range(5)])
print(tree)

# type aliases
from typing import List, Union
Vector = List[Union[int, float]]


def scale(scalar: int, vector: Vector) -> Vector:
    return [scalar * num for num in vector]


print(scale(2, [1, 0.2, 0.3]))

# simple unique types with NewType
from typing import Union, NewType

Multiplier = NewType('Multiplier', int)
Multiplicand = NewType('Multiplicand', Union[int, str])


def multiplication(multiplicand: Multiplicand, multiplier: Multiplier) -> Multiplicand:
    return multiplicand * multiplier


print(multiplication(Multiplicand("ab"), Multiplier(3)))
print(multiplication(Multiplicand(4), Multiplier(3)))
```

## Type checking using `mypy`
Install mypy
```bash
pip install mypy
```
Recommended `mypy` configuration
```text
# file: mypy.ini
[mypy]
ignore_missing_imports = True
disallow_untyped_defs = True
disallow_incomplete_defs = True
disallow_any_generics = True
warn_unused_ignores = True
warn_unused_configs = True
show_column_numbers = True
```
```python
# file: sample.py
from typing import List, Union, Tuple
Vector = List[Union[int, float]]


def scale(scalar: int, vector: Vector) -> Vector:
    return [scalar * num for num in vector]


print(scale(2, [1, 0.2, 0.3]))
print(scale(2, [1, "aaa", None]))

data: Tuple[str, int, float] = ('name',)
vector: Tuple[int, int] = (1, 1.3) #type: ignore
diapason: Tuple[int, int] = (1, 1.3)
```
Validation
```bash
python -m mypy --config-file mypy.ini sample.py
```

# References
- <https://www.python.org/dev/peps/pep-3107>
- <https://www.python.org/dev/peps/pep-0526>
- <https://docs.python.org/3/library/typing.html>
- <https://github.com/python/typing>
- <https://www.python.org/dev/peps/pep-0483>
- <https://www.python.org/dev/peps/pep-0484>
- <http://www.mypy-lang.org>
- <https://mypy.readthedocs.io/en/latest/cheat_sheet_py3.html>
- <https://veekaybee.github.io/2019/07/08/python-type-hints>

# Practice
## Task description
There is `circles.py` module (see below) which has some errors.

You have to run `mypy` with provided above `mypy.ini` configuration file. Try to fix all errors by
adding type annotations for variables, functions, and classes. Repeat this "run - fix" cycle until
`mypy` shows no errors.

Then, run `python circles.py` and correct code if there are some errors. After, feel free to send
the result for the review.

Acceptance criteria are both the module works without errors and `mypy` shows no errors.
```python
# file: circles.py
import math


def safe_radius_of(number):
    if not (
            isinstance(number, int) or
            isinstance(number, float)
    ):
        raise TypeError('Radius should be either "int" or "float" data type!')
    if number < 0:
        raise ValueError('Radius value should be more that "0"!')
    return number


def calculate_square_of(number):
    return number ** 2


def multiply_by_pi(number):
    return math.pi * number


def double_of(number):
    return number * 2


class Circle:
    def __init__(self, identifier, radius, color):
        self.identifier = identifier
        self._radius = lambda: safe_radius_of(radius)
        self._color = color
    
    def change_color(self, color):
        if not isinstance(color, str):
            raise TypeError('Color should be "str" data type!')
        self._color = color
    
    def diameter(self):
        return double_of(self._radius())
    
    def area(self):
        return multiply_by_pi(calculate_square_of(self._radius()))
    
    def perimeter(self):
        return double_of(multiply_by_pi(self._radius()))
    
    def __str__(self):
        return f'Circle(radius={self._radius()}, color="{self._color}")'


class Circles:
    def __init__(self, circles):
        self._circles = circles
    
    def update_colors(self, new_colors):
        for identifier, color in new_colors.items():
            this = self.find(identifier)
            this.change_color(color)
            print(this)
    
    def find(self, identifier):
        for this in self._circles:
            if this.identifier == identifier:
                return this
    
        raise ValueError(f"There is no circle with this identifier: {identifier}")
    
    def show_diameters(self):
        for circle in self._circles:
            print(f"A diameter of {circle} is {circle.diameter()}")
    
    def show_areas(self):
        for circle in self._circles:
            print(f"An area of {circle} is {circle.area()}")
    
    def show_perimeters(self):
        for circle in self._circles:
            print(f"A perimeter of {circle} is {circle.perimeter()}")


if __name__ == "__main__":
    circles = Circles(
        Circle(1, 1, 'red'),
        Circle("2", 0.2, 'super-red'),
        Circle(3, 3, 'green'),
        Circle(4, 5, 'super-green'),
    )
    circles.show_diameters()
    circles.show_areas()
    circles.show_perimeters()
    circles.update_colors([
        'blue',
        'red',
        'black',
        'purple'
    ])
```

## Review items
Please send `circles.py` module which works without errors and has type-annotated code.
