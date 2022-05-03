# "python" puzzles

## python-01 Writing a Python script 
**Goal:** The puzzle introduces syntax of Python language, explains the definition of variables and functions. Then it 
shows the numeric, sequence, set, iterator, and mapping types as well as boolean, math and comparison operations. 
And then, puzzle demonstrates usage of all those items with control flow statements like `for`, `while`, `if`. Finally,
the proper executable Python's script will be created.

**Dependency:** 

**Materials:** [01/README.md](01/README.md)

## python-02 Manipulating build-in types with standard Python functions 
**Goal:** The puzzle explains applying of the built-in functions of standard Python library to built-in types. It 
covers cases of filtering, sorting, and working with analytic operations (like `sum`, `max`, etc.). Also, the concept of 
 variable-length argument list (`*args`,`**kwargs`) is explained as well as a lambda function definition. Finally, 
 the puzzle shows how to use comprehensions with sequences and mappings. 

**Dependency:** `python-01` 

**Materials:** [02/README.md](02/README.md)

## python-03 Organizing a Python project
**Goal:** The puzzle explains how to make a proper setup of Python project. In order to achieve this, it starts from
the concepts of a `module` and a `package` as well as from the import mechanism. After, the puzzle explains
the Python Package Index (PYPI) and usage of `pip` (packages installer for Python) together with the concept of
a virtual environment for managing multiple projects. Finally, the puzzle shows documenting of code as well as
validation of the correctness of the project's documentation.

**Dependency:** `python-02`

**Materials:** [03/README.md](03/README.md)

## python-04 Using classes and complex objects for advanced tasks
**Goal:** The puzzle starts from explaining what a `class`/`object` is as well as aligning object concept to practical 
tasks. After, it demonstrates how different types of methods (including static, class, instance, magic, etc.) and 
variables work in practical application. Finally, the puzzle explains specific cases when you have to use either enums
or named tuples and even apply performance optimizations.

**Dependency:** `python-03`

**Materials:** [04/README.md](04/README.md)

## python-05 Optimizing code with generators, decorators, and context managers
**Goal:** The puzzle explains how to use some standard Python capabilities for solving wide range of coding tasks. It
starts by showing how to use resources efficiently with generators. Then, the puzzle shows how to enhance functions
behavior without explicitly modifying them by using decorators. Finally, it demonstrates using context managers which
making automatic pre/post actions around some executable logic.

**Dependency:** `python-04`

**Materials:** [05/README.md](05/README.md)

## python-06 Managing unpredictable behavior with exceptions
**Goal:** The puzzle explains how to deal with errors detected during code execution. It starts by an explanation of
what an exception is as well as what are built-in exceptions. Then, the puzzle shows how to handle them to achieve
different behavior of code execution flow. Finally, it shows how to craft a user-defined exception and work with it.

**Dependency:** `python-05`

**Materials:** [06/README.md](06/README.md)

## python-07 Applying OOD in Python's code
**Goal:** The puzzle demonstrates how to apply object-oriented programming to a Python code. It shows creating 
pure abstractions and practical examples of their usage along with polymorphism, inheritance, and association. Finally,
the puzzle explains how encapsulation works in Python and what kind of options are available for practical usage.

**Dependency:** `python-04`

**Materials:** [07/README.md](07/README.md)

## python-08 Using type hints for reducing runtime errors
**Goal:** The puzzle explains type definition syntax which gives initial code documentation and ability to prevent
runtime errors due to incorrect objects usage. Besides standards types (provided by `typing` module), the puzzle
explains how to create and use custom types. Finally, it demonstrates how to use `mypy` (static type checker for Python)
for your code even if type hints are used for small part of the code.

**Dependency:** `python-05`, `python-07`

**Materials:** [08/README.md](08/README.md)

## python-09 Configuring proper logging capabilities for the code
**Goal:** The puzzle explains how to use `logging` module within a code base. It covers logging levels, modification of
the output with filters and formatters, and write to different layouts. Also, the puzzle demonstrates configurations for
a Python project where logs configurations need to be separated by packages or modules.

**Dependency:** `python-04`

**Materials:** [09/README.md](09/README.md)

## python-10 Writing tests with `unittest` framework
**Goal:** The puzzle shows all capabilities of Python's built-in `unittest` framework. It explains a test definition
syntax and how to run tests from command-line interface. Also, demonstrates the most useful assertions and some
important decorators. Finally, the puzzle shows how to create custom test suits for different purposes.

**Dependency:** `python-05`

**Materials:** [10/README.md](10/README.md)
