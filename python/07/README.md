# 07 - Applying OOD in Python's code
# Syllabus
- Encapsulation
  - Encapsulation in a class
  - Encapsulation in a module
  - Encapsulation in a package
- Inheritance
  - Multiple inheritance and Method Resolution Order at a glance
  - `super` keyword and its usage
- Abstraction and polymorphism
- Design patterns

# Lesson compendium
## Encapsulation

### Encapsulation in a class
There are two possible ways to make a class member private
- `_` before a reference name, by convention
- `__` before a reference name, by Python's design

And `@property` allows immutability.

```python
class User:
    def __init__(self, identifier, full_name, email):
        self.__identifier = identifier
        self.full_name = full_name
        self._email = email

    @property
    def identifier(self):
        """Reads encapsulated email."""
        return self.__identifier

    @property
    def email(self):
        """Reads encapsulated email."""
        return f"{self.full_name} <{self._email}>"


jon = User(1, "Jon Dee", "jd@company.com")
# public
jon.full_name
jon.full_name = "Peter"
jon.full_name
# private by convention
jon.email
jon.email = 'other@email'
jon._email                  # don't do this
jon._email = 'other@email'  # don't do this
jon.email

# private by Python's design
jon.identifier
jon.__identifier            # don't work :)
jon._User__identifier       # don't do this
jon._User__identifier = 3   # don't do this
jon.identifier
```
### Encapsulation in a module
There are two possible ways to make a module member private
- `_` before a reference name, by convention
- `__` before a reference name, by Python's design

```python
# url.py
_http_prefix = "http"
__file_prefix = "file"
http_protocol = _http_prefix
```
```python
import url
dir(url)
url.http_protocol
url._http_prefix    # don't do this
url.__file_prefix   # don't do this
```

### Encapsulation in a package
There is one possible way to make a package member private. It is by putting
`_` before a module name, by convention.

```python
# en/__init__.py
print("init")

# en/__super_private.py
print("super private")

# en/_private.py
print("private")

# en/public.py
print("public")
```
```python
import en
dir()
from en import public
dir()
from en import _private            # don't do this
dir()
from en import __super_private     # don't do this
dir()
```

## Inheritance
### Multiple inheritance and Method Resolution Order at a glance
For most purposes, in the simplest cases, you can think of the search for attributes inherited from
a parent class as depth-first, left-to-right, not searching twice in the same class where there is
an overlap in the hierarchy.

```python
class First:
    def do(self):
        print("do first")

class Second:
    def do(self):
        print("do second")

class FirstAction(First, Second):
    pass

fa = FirstAction()
fa.do()

class SecondAction(Second, First):
    pass

sa = SecondAction()
sa.do()
```

### `super` keyword and its usage
`super()` return a proxy object that delegates method calls to a parent or sibling class of type.
This is useful for accessing inherited methods that have been overridden in a class.

```python
class Action:
    def __init__(self, times):
        self._times = times

    def do(self):
        for _ in range(self._times):
            print("do something")

class ComplicatedAction(Action):
    def __init__(self, times):
        super().__init__(times)

    def do(self):
        print("complicated flow starts")
        super().do()
        print("complicated flow ends")

class ComplexAction(ComplicatedAction):
    def __init__(self):
        super().__init__(3)

    def do(self):
        print("super complex flow")
        super().do()


action = ComplexAction()
action.do()
```

> Don't write code like the above. It's too hard to maintain and extend.

## Abstraction and polymorphism
```python
from abc import ABC, abstractmethod

class Action(ABC):
    @abstractmethod
    def do(self):
        pass

# try to initialize
action = Action()

# several implementation
class RepeatableAction(Action):
    def __init__(self, times):
        self._times = times

    def do(self):
        for _ in range(self._times):
            print("do something")

class ComplicatedAction(Action):
    def __init__(self, times):
        self._action = RepeatableAction(times)

    def do(self):
        print("complicated flow starts")
        self._action.do()
        print("complicated flow ends")

class ComplexAction(Action):
    def __init__(self):
        self._action = ComplicatedAction(3)

    def do(self):
        print("super complex flow")
        self._action.do()

# run several actions
for action in (
    ComplexAction(),
    ComplicatedAction(1),
    ComplexAction(),
    RepeatableAction(5)
):
    print("<<< next action >>>")
    action.do()
```

## Design patterns

```python
import this
```

And some other
- <https://refactoring.guru/design-patterns/python>
- <https://www.toptal.com/python/python-design-patterns>
- <https://python-patterns.guide>
- <https://github.com/faif/python-patterns>


# References
- <https://docs.python.org/3/reference/expressions.html?highlight=two%20underscores#atom-identifiers>
- <https://docs.python.org/3/tutorial/classes.html?highlight=multiple%20inheritance#private-variables>
- <https://docs.python.org/3/library/functions.html#property>
- <https://docs.python.org/3/tutorial/classes.html?highlight=multiple%20inheritance#multiple-inheritance>
- <https://www.python.org/download/releases/2.3/mro/>
- <https://docs.python.org/3/library/functions.html#super>
- <https://realpython.com/oop-in-python-vs-java/>
- <https://realpython.com/inheritance-composition-python/>


# Practice
## Task description
Write a program which filters a given file or annotates its lines with relevant rules. It should
run like
```bash
python -m silters (filter | annotate) <a file path>
```
If `filter` option is requested, the program should display lines based on rules below. If
`annotate` option is requested, the program has to display the information about which rules are
applicable for each line using `<line number>: [rule] ...` format. For instance, like this
```text
1: FP001
2:
3: FP001 FP002 FN203
4: FN203
```

There are the following rules for the content:
- display a line if it
    - ends with a dot (FP001)
    - is less than 100 characters (FP002)
    - has at least 5 `a` letters (FP003)
- don't display a line if it
    - has more than 3 `z` letters (FN201)
    - is an empty line (FN202)
    - consists only from non-letter characters (FN203)

The "display" rules have a priority over the "non-display" rules. For instance, if there is a line
like `zaz zara Ararat ZULU Ozz and more amazing words`, it should be printed.

Also, each rule has to be implemented by extending the following abstraction
```python
from abc import ABC, abstractmethod


class Filter(ABC):
    @abstractmethod
    def name(self) -> str:
        """Provides a name of the rule (like FP005)."""
        pass

    @abstractmethod
    def matches(self, line: str) -> bool:
        """Returns True if a given line matches the filter, otherwise, returns False."""
        pass
```
Also, try to create an abstraction to express different options (we have `filter` and `annotate`)
now. And don't forget to apply other OOD design approaches if any. Ideally, the implementation
should have no functions, only classes.

## Review items
Please send a zip archive with the code.
