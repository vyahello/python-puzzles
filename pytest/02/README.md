# 02 - Mastering prerequisites and postconditions
# Syllabus
- Basic fixtures
- Scope of fixture
- Autouse fixture
- Conftest fixture

# Presentation

## Basic fixtures
```python
# test_basic_fixtures.py
 
def setup():
    print("basic setup - executes before every test")
 
 
def teardown():
    print("basic teardown - executes after every test")
 
 
def setup_module():
    print("module setup - executes once before every module")
 
 
def teardown_module():
    print("module teardown - executes once after every module")

 
def increment(number):
    """Increment a given number by `1`."""
    return number + 1
 
 
def test_increment():
    print("running test_increment test")
    assert increment(2) == 3
 
 
class TestFixtures:
    def setup_class(cls):
        print("basic class setup - executes once before all tests")
 
    def teardown_class(cls):
        print("basic class teardown - executes once after all tests")
 
    def setup(self):
        print("basic setup - executed once before every test")
 
    def teardown(self):
        print("basic teardown - executed once after every test")
 
    def test_increment_in_class(self):
        print("running test_increment_in_class test")
        assert increment(4) == 5
```

Run basic fixtures
```bash
# Run all basic fixtures
pytest test_basic_fixtures.py -s -v
```

## Scope of fixture
```python
# test_scope_of_fixture.py
  
import pytest
 
 
@pytest.fixture(scope="module")
def module_setup():
    print("executes once for a module before all tests")
    return "module_setup"
 
 
@pytest.fixture(scope="function")
def function_setup():
    print("executes for every test function")
    return "function_setup"
 
 
@pytest.fixture(scope="session")
def session_setup():
    print("executes once for a session")
    return "session_setup"
 
 
@pytest.fixture(scope="class")
def class_setup():
    print("executes before every class")
    return "class_setup"
 
 
def test_basic_scopes1(module_setup, function_setup, session_setup):
    print('running test_basic_scopes1')
    assert module_setup == "module_setup"
    assert function_setup == "function_setup"
    assert session_setup == "session_setup"
 
 
def test_basic_scopes2(module_setup, function_setup, session_setup):
    print('running test_basic_scopes2')
    assert module_setup == "module_setup"
    assert function_setup == "function_setup"
    assert session_setup == "session_setup"
 
 
class TestOne:
    def test_class_one(self, class_setup):
        print('running test_class_one')
        assert class_setup == "class_setup"
 
 
class TestTwo:
    def test_class_one(self, class_setup):
        print('running test_class_two')
        assert class_setup == "class_setup"
```

Run scope of fixture
```bash
# Run scope of fixture
pytest test_scope_of_fixture.py -s -v
  
# See all available fixtures for `test_scope_of_fixture.py`
pytest --fixtures test_scope_of_fixture.py
```

## Autouse fixture
```python
# test_autouse_fixture.py
 
import pytest
 
 
class Storage(object):
    def __init__(self):
        self.list = list()
 
    def append(self, value):
        self.list.append(value)
 
 
@pytest.fixture(scope="module")
def storage():
    return Storage()
 
 
class TestStorage(object):
    @pytest.fixture(autouse=True)
    def start(self, request, storage):
        storage.append(request.function.__name__)
 
    def test_method1(self, storage):
        assert storage.list == ['test_method1']
 
    def test_method2(self, storage):
        assert storage.list == ['test_method1', 'test_method2']
```

Run autouse fixture
```bash
# Run autouse fixture
pytest test_autouse_fixture.py -s -v
  
# See all available fixtures for `test_autouse_fixture.py`
pytest --fixtures test_autouse_fixture.py
```

## Conftest fixture
```python
# conftest.py
 
import pytest
 
 
@pytest.fixture(scope="module")
def num():
    return 3
  
  
# square.py
  
def square_list(n):
    return [i**2 for i in range(1, n)]
 
 
# test_square.py
  
from square import square_list


def test_square_list(num):
    assert square_list(num) == [1, 4]
 
 
def test_len_square_list(num):
    assert len(square_list(num)) == 2
```

Run conftest fixture
```bash
# Run `conftest.py` fixture
pytest test_square.py -s -v
  
# See `conftest.py` module as a fixture for `test_square.py`
pytest --fixtures test_square.py
```

# Additional materials
- [https://docs.pytest.org/en/3.5.0/fixture.html](https://docs.pytest.org/en/3.5.0/fixture.html)
- [https://docs.pytest.org/en/latest/example/simple.html](https://docs.pytest.org/en/latest/example/simple.html)

# Control
## Task description
Write unittests that cover functions from `list_actions.py` module
  - Unittests has to contain fixture that runs once before all tests
  - Fixture has to be located in `conftest.py` module

```python
# list_actions.py

def add_to_list(array):
    """
    Add number `1` into the end of given list.
    
    Return a list.
    """
    array.append(1)
    return array


def length_of_list(array):
    """Count elements in a given list."""
    return len(array)
```

## Review items
Send on review:
  - `conftest.py` module with a fixture.
  - Python test module with tests.
  - Command to run all tests.
