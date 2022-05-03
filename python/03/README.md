# 03 - Organizing a project
# Syllabus
- using `module`s
- using `package`s
  - package structure
  - package execution
- layout of a Python project
- managing packages with `pip`
- managing environments with `virtualenv`
- documenting the code
  - formatting types
  - document a package
  - explore the documentation

# Lesson compendium
## Using `module`s
```python
# file: greetings.py
def hello():
    print("Hello!")


def hi():
    print("Hi!")


def morning():
    print("Good morning!")

```
Import mechanism
```python
# import module
dir()
import greetings
dir()
dir(greetings)
greetings.hello()

# import function
from greetings import hi
dir()
hi()

# renaming
from greetings import morning as always
dir()
always()
```

## Using `package`s
### Package structure
A package structure
```
.                        <- the root project direcory
└── phrases              <- a package
    ├── __init__.py      <- special module, says "phrases" is the package
    └── greetings.py     <- a module of "phrases" package
```
Make `greetings.py` a part of the package
```bash
mkdir phrases
mv greetings.py phrases/
touch phrases/__init__.py
```
Import mechanism
```python
# import package
dir()
import phrases
dir()
help(phrases)
phrases.greetings.hello()

# import module
dir()
from phrases import greetings
dir()
dir(greetings)
greetings.morning()

# import function
from phrases.greetings import hi
dir()
hi()
```

### Package execution
```
.
└── phrases
    ├── __init__.py
    ├── __main__.py      <- special module which defines default execution
    └── greetings.py
```
```bash
cat <<CODE > phrases/__main__.py
from phrases.greetings import morning


if __name__ == "__main__":
    morning()
CODE
```
```bash
python -m phrases
```

## Layout of a Python project
A sample project
```
.                          <-  the root project direcory
├── emailboot             <-  a package
│   ├── __init__.py        <-  special module, says "emailboot" is the package
│   ├── __main__.py        <-  a default execution
│   ├── extra              <-  an inner package
│   │   ├── __init__.py    <-  special module, says "extra" is the inner package for "emailboot"
│   │   ├── users.py       <-  a module of "extra" package
│   │   └── templates.py   <-  a module of "extra" package
│   ├── data               <-  an inner directory
│   │   └── template.txt   <-  a file
│   ├── actions.py         <-  a module of "emailboot" package
│   ├── items.py           <-  a module of "emailboot" package
│   └── workflows.py       <-  a module of "emailboot" package
├── setup.py               <-  a packaging configuration (optional)
├── README.md              <-  a description
└── requirements.txt       <-  used packages (can be part of setup.py)
```

## Managing packages with `pip`
<https://pypi.org> is a public package repository.
```bash
python -m pip
python -m pip list -h
python -m pip freeze -h
python -m pip list
python -m pip freeze
python -m pip install virtualenv
python -m pip freeze
python -m pip freeze > requirements.txt
```

## Managing environments with `virtualenv`
```bash
python -m pip install virtualenv
python -m virtualenv
python -m virtualenv venv
source venv/bin/activate
python -m pip install selenium
python -m pip list
deactivate
python -m pip list
```

## Documenting the code
### Formatting types
Important PEPs:
- <https://www.python.org/dev/peps/pep-0257/>
- <https://www.python.org/dev/peps/pep-0287/>

Formatting type | Description | Formal specification
---|---|---
[Google docstrings](https://github.com/google/styleguide/blob/gh-pages/pyguide.md#38-comments-and-docstrings) | Google’s recommended form of documentation | No
[reStructured Text](http://docutils.sourceforge.net/rst.html) | Official Python documentation standard | Yes
[NumPy/SciPy docstrings](https://numpydoc.readthedocs.io/en/latest/format.html) | NumPy’s combination of reStructured and Google Docstrings | Yes
[Epytext](http://epydoc.sourceforge.net/epytext.html) | A Python adaptation of Epydoc; Great for Java developers | Yes

**reStructured Text** is recommended. <http://www.sphinx-doc.org/en/master/>  is the most used tool for the generation of
documentation based on reST format.

### Document a package
```python
# file: phrases/__init__.py
"""
The package contains well-known phrases.

For instance, if you need greetings, run ``from phrases import greetings`` and use them.
"""
```
```python
# file: phrases/greetings.py
"""This module has greetings only."""
def hello():
    """Prints "Hello!"."""
    print("Hello!")


def hi():
    """
    Says "Hi!".

    Yes, just "Hi!".
    """
    print("Hi!")


def morning():
    print("Good morning!")

```

### Explore the documentation
```python
import phrases
help()
# package
phrases
# module
phrases.greetings
# function
phrases.greetings.hi
phrases.greetings.morning
```
### Validate the documentation
```bash
source venv/bin/activate
python -m pip install pydocstyle
python -m pydocstyle phrases
```
Please treat <https://github.com/extsoft/dp-151/commit/ef2995c2fd80ac433d240c79e9d9d030e9c24ca5> as a recommended
configuration for `pydocstyle`.

# References
- <https://docs.python.org/3/tutorial/modules.html>
- <http://docs.python-guide.org/en/latest/writing/structure>
- <http://extsoft.pro/python-project-structure/>
- <https://docs.python.org/3/library/__main__.html>
- <https://docs.python.org/3/installing/index.html>
- <https://pypi.python.org/pypi/pip>
- <https://snarky.ca/why-you-should-use-python-m-pip/>
- <https://virtualenv.pypa.io/en/stable/>
- <https://github.com/pyenv/pyenv>
- <https://github.com/pyenv/pyenv-virtualenv>
- <https://github.com/bees-hive/pem>
- <https://www.python.org/dev/peps/pep-0257/>
- <https://www.python.org/dev/peps/pep-0287/>
- <http://www.sphinx-doc.org/en/master/>
- <http://www.pydocstyle.org>
- <https://extsoft.pro/static-code-analysis-in-python/>

# Practice
## Task description
Please write a program which will greet a user, ask him/her for a number, and translate the number into words
representation. For instance, if the user gives `9`, it prints `nine`. In order to achieve this, you need
- find a package on PYPI which can convert numbers into letters
- add `requirements.txt`
- create a package with the logic (it has to be distributed across several modules)
- add documentation which passes default `pydocstyle` rules
- configure execution via `python -m ...`

## Review items
Please send `.zip` archive of the project along with usage instructions.
