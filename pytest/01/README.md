# 01 - Writing tests
# Syllabus
- Install pytest
- Files structure
- Syntax
- Test sample
- Run test(s)
- Asserts
- Markers

# Presentation
## Install pytest
```bash
# Install latest pytest dependencies
pip install -U pytest
  
# Check installed pytest version
pytest --version
```

## Files structure
```
.
└── tests                      # tests directory
    ├── test_module.py         # test module
    │   ├── test_item1()       # test function
    │   └── test_item2()       # test function
    │   
    └── module_test.py         # test module
        ├── test_item3()       # test function
        └── test_item4()       # test function
```

## Syntax
```bash

def test_target(): 
    assert tested_target == expected_result
```

## Basic test sample
```python
#  test_basic.py
 
def increment(number):
    """Increment a given number by `1`."""
    return number + 1
 
 
def test_increment():
    assert increment(2) == 3
```

## Run test(s)
```bash
# Basic test run
pytest test_basic.py
  
# Run test in verbose mode
pytest test_basic.py -v
  
# Run test in quite mode
pytest test_basic.py -q
 
# Run specific test function in a test module
pytest test_basic.py::test_increment
  
# Run all tests in current directory and it's subdirectories
pytest
  
# More pytest options
pytest --help
```

## Asserts
```python
#  test_asserts.py
  
import pytest
 
def to_int(element):
    """Convert given element into `int` data type."""
    return int(element)
 
 
def test_to_int():
    assert to_int('5') == 5
 
 
def test_to_int_two_asserts():
    assert to_int('5') == 5
    assert to_int(10.0) == 10


def test_to_int_with_error_message():
    assert to_int('6') == 7, "6 is not equal to 7"


def test_to_int_expected_exception():
    with pytest.raises(TypeError, message="Expecting TypeError exception"):
        to_int(None)
```

## Markers
```python
#  test_markers.py
 
import pytest
import sys
 
def length(itr):
    """Count elements in iterable object."""
    return len(itr)
 
 
@pytest.mark.skip(reason="I just want to skip this test.")
def test_skip_length():
    assert length('12') == 2
 
 
@pytest.mark.skipif(sys.platform == 'win32', reason='Does not support 32bit Windows platform')
def test_skipif_length():
    assert length('12') == 2
 
 
@pytest.mark.xfail(reason="Expected failure.")
def test_exp_fail_length():
    assert length(None) == 1
 
 
@pytest.mark.parametrize("test_input, expected_output", [
    ('', 0),
    ('1', 1),
    ('123', 3)
])
def test_param_length(test_input, expected_output):
    assert length(test_input) == expected_output
 
 
@pytest.mark.own_marker
def test_own_marker_length():
```

Run every marker
```bash
# Run all markers
pytest test_markers.py -v
  
# Run skip tests
pytest test_markers.py -v -m skip
  
# Run skipif tests
pytest test_markers.py -v -m skipif
  
# Run expected failure tests
pytest test_markers.py -v -m xfail
  
# Run parametrize tests
pytest test_markers.py -v -m parametrize
  
# Run test with my own marker
pytest test_markers.py -v -m own_marker
```

# Additional materials
- [https://docs.pytest.org/en/latest/contents.html](https://docs.pytest.org/en/latest/contents.html)
- [https://docs.pytest.org/en/latest/getting-started.html](https://docs.pytest.org/en/latest/getting-started.html)
- [https://docs.pytest.org/en/latest/assert.html](https://docs.pytest.org/en/latest/assert.html)
- [https://docs.pytest.org/en/latest/mark.html#mark](https://docs.pytest.org/en/latest/mark.html#mark)

# Control


Write unittests for `square()` and `half()` functions in `operations.py` module including both positive and negative test scenarios (tests should be passed even if errors are occurred).
  ```python
# operations.py

def square(number):
    """Calculate square of a given number."""
    return number**2
  
  
def half(number):
    """Calculate half of a given number."""
    return number / 2
```
1. Positive scenario:
  - Create `test_positive_operations.py` test module.
  - Create `test_square()` and `test_half()` test functions.
  - Parametrize at least one of test functions.
  - All tests have to be marked with `@positive` marker.
2. Negative scenario:
  - Create `test_negative_operations.py` test module.
  - Create `test_square()` and `test_half()` test functions.
  - Parametrize at least one of test functions.
  - Use either `with pytest.raises(...)` statement or `@xfail` marker to capture expected test failure.
  - All tests have to be marked with `@negative` marker.
3. Send on review:
  - Command to run all tests.
  - Command to run only positive tests.
  - Command to run only negative tests.
  - Python's test modules with test functions.
