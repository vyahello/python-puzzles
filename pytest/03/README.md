# 03 - Changing pytest behavior with custom plugins
# Syllabus
- Write plugin
- Use plugin
- Add custom parameters
- Write hooks
- Write tests with external plugin

# Presentation
## Write plugin
```python
# plugin01.py
import pytest


@pytest.fixture(scope='module')
def rng():
    return range(5)
```

## Use plugin
```python
# load plugin in conftest.py
pytest_plugins = 'plugin01'


# operations.py
def calculate_sum(rng):
    """Calculate sum of a given iterable."""
  
    return sum(rng)
 
 
def calculate_length(rng):
    """Calculate length of a given iterable."""
  
    return len(rng)


# test_operations.py
from operations import calculate_sum_of, calculate_length_of
  
pytest_plugins = 'plugin01'  # load plugin if conftest.py above is NOT configured


def test_total(rng):
    assert total(rng) == 10
 
 
def test_count(rng):
    assert count(rng) == 5
```

Run tests with plugin
```bash
# Run test_operations.py
pytest -v test_operations.py
``` 

## Add custom parameters
```python
# checks.py
import os
 
 
def ping_host(host):
    """Ping a certain host.
       Return exit code.
    """
  
    return os.system(f"ping {host} -c 1")
  
  
# plugin02.py
import pytest
 
 
def pytest_addoption(parser):
    """Add custom parameter option `--host/-H` to the command line."""
  
    parser.addoption("--host", "-H",
                     action="store",
                     default="localhost",
                     help="A host should be provided as a hostname (google.com) or ip address (172.217.13.238)")
 
 
@pytest.fixture(scope="module")
def host(request):
    """Return a certain host value from a `--host/-H` custom parameter."""
  
    return request.config.getoption("host")
 
 
# conftest.py
pytest_plugins = 'plugin02'


# test_checks.py
from checks import ping_host
 
 
def test_ping_host(host):
    assert ping_host(host) == 0
```

Run tests with custom parameter
```bash
# Check just created custom parameter (--host/-H should be provided)
pytest --help
  
# Run test_checks.py with custom parameter
pytest test_ping.py -v --host pytest.org
```

## Write hooks
```python
# hooks_plugin.py
def pytest_addoption(parser):
    """Add custom parameter `--skip-marker/-S` to skip certain pytest marker."""
 
    parser.addoption("--skip-marker",
                     "-S",
                     action="store",
                     default=None,
                     help="skip test(s) with certain marker.")
 
 
def pytest_runtest_setup(item):
    """Skip test(s) that belong(s) to specific marker with `--skip-marker/-S` command line option."""
 
    marker = item.config.getvalue("skip_marker")
    if marker in item.keywords:
        pytest.skip(f"Skipping [@{marker}] pytest marker")
 
  
def pytest_configure(config):
    """Enable verbose output when running tests. Simulate `-v` cmd option."""
 
    config.option.verbose = 1
 
 
def pytest_report_header(config):
    """Add `project name` and `owner` to test report header."""
 
    if config.option.verbose > 0:
        return ["Project: pytest-03-hooks", "Written by: Volodymyr Yahello"]


# conftest.py
pytest_plugins = "hooks_plugin"
  
  
# test_dir.py
import os
import pytest
 
 
@pytest.fixture(scope='module', autouse=True)
def setup_dir(request):
    """Create directory before execution of all tests."""
  
    os.mkdir('test-dir', mode=0o777)
 
    def teardown_dir():
        """Delete directory after all executed tests."""
  
        os.rmdir('test-dir-1')
 
    request.addfinalizer(teardown_dir)
 
 
@pytest.mark.dir
def test_rename_dir():
    os.rename('test-dir', 'test-dir-1')
    assert os.path.basename('test-dir-1') == 'test-dir-1'
 
 
@pytest.mark.dir
def test_list_dir():
    assert os.listdir('test-dir-1') == []
 
 
# test_system.py
import sys
import pytest
 
 
@pytest.mark.system
def test_platform():
    assert sys.platform == 'darwin'
 
 
@pytest.mark.system
def test_python_version():
    assert sys.version_info[:3] == (3, 6, 5)
```

Run tests with hooks
```bash
# Skip tests with `dir` pytest marker
pytest --skip-marker dir -rs
  
# Skip tests with `system` pytest marker
pytest --skip-marker system -rs
```

## Write tests with external plugin

Install external plugin (pytest-bdd)
```bash
# Install pytest-bdd plugin with pip
pip install pytest-bdd
```

Write tests with pytest-bdd plugin
```gherkin
# bucket_of_fruits.feature
Feature: A bucket of fruits
 
Scenario: Counting fruits in a bucket
    Given There are 9 fruits in a bucket
    When I have 2 friends
    And I give 3 fruits to my 1st friend
    And I give 3 fruits to my 2nd friend
    Then I should have 3 fruits for myself
```
```python
# test_bucket_of_fruits.py
from pytest_bdd import scenario, given, when, then, parsers
 
 
@scenario('bucket_of_fruits.feature', 'Counting fruits in a bucket')
def test_bucket_of_fruits():
    pass
 
 
@given(parsers.parse('There are {amount:d} fruits in a bucket'))
def initial_amount_of_fruits(amount):
    return {'fruits': amount}
 
 
@when(parsers.parse('I have {amount:d} friends'))
def amount_of_friends(amount):
    assert amount == 2
 
 
@when(parsers.parse('I give {amount:d} fruits to my 1st friend'))
def give_fruits_to_1st_friend(initial_amount_of_fruits, amount):
    initial_amount_of_fruits['fruits'] -= amount
 
 
@when(parsers.parse('I give {amount:d} fruits to my 2nd friend'))
def give_fruits_to_2st_friend(initial_amount_of_fruits, amount):
    initial_amount_of_fruits['fruits'] -= amount
 
 
@then(parsers.parse('I should have {amount:d} fruits for myself'))
def amount_fruits_for_myself(initial_amount_of_fruits, amount):
    assert initial_amount_of_fruits['fruits'] == amount
```

Run tests with pytest-bdd plugin
```bash
# Run test_bucket_of_fruits.py
pytest -vv --gherkin-terminal-reporter test_bucket_of_fruits.py
```

# Additional materials
- [https://docs.pytest.org/en/latest/writing_plugins.html](https://docs.pytest.org/en/latest/writing_plugins.html)
- [https://docs.pytest.org/en/latest/plugins.html](https://docs.pytest.org/en/latest/plugins.html)
- [https://docs.pytest.org/en/latest/example/simple.html](https://docs.pytest.org/en/latest/example/simple.html)
- [https://docs.pytest.org/en/latest/writing_plugins.html#writing-hook-functions](https://docs.pytest.org/en/latest/writing_plugins.html#writing-hook-functions)
- [https://docs.pytest.org/en/3.7.1/_modules/_pytest/hookspec.html](https://docs.pytest.org/en/3.7.1/_modules/_pytest/hookspec.html)
- [https://pytest.readthedocs.io/en/2.7.3/plugins_index/index.html](https://pytest.readthedocs.io/en/2.7.3/plugins_index/index.html)
- [http://pytest-bdd.readthedocs.io/en/latest/](http://pytest-bdd.readthedocs.io/en/latest/)

# Control
## Task description

Write plugin for `test_outcomes.py` test module that will store report file for either passed or failed or skipped tests:
  - Plugin has to contain report header hook with your initials and home work name
  - Plugin has to be loaded in `conftest.py` module


```python
# test_outcomes.py
import pytest


@pytest.mark.skip
def test_skipped1():
    assert 1
    

@pytest.mark.skip
def test_skipped2():
    assert 1
    

def test_failed1():
    assert 0


def test_failed2():
    assert 0


def test_passed1():
    assert 1


def test_passed2():
    assert 1
```


## Review items
- Plugin module
- `conftest.py` module
- Command to store only passed tests report
- Command to store only failed tests report
- Command to store only skipped tests report
