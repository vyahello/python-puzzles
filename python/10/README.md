# 01 - Writing tests with `unittest` framework
# Syllabus
- Test definition syntax
- Build-in assertions
- Command-line interface
- Some useful `unittest` decorators
- Tests suites

# Lesson compendium
## Test definition syntax
```python
# one.py
import unittest
from unittest import TestCase


class TestScenario(TestCase):
    @classmethod
    def setUpClass(cls):
        print("setUpClass")

    @classmethod
    def tearDownClass(cls):
        print("tearDownClass")

    def setUp(self) -> None:
        """Configuration"""
        self.number = 1
        print("self.number = 1")

    def tearDown(self) -> None:
        """Clean up"""
        del self.number
        print("del self.number")

    def test_number_is_set(self):
        print("test_number_is_set")
        self.assertIsNotNone(self.number)

    def test_number_greater_then_5(self):
        print("test_number_greater_then_5")
        self.assertGreater(1, 5)

    def test_number_greater_then_0(self):
        print("test_number_greater_then_0")
        self.assertGreater(1, 0)


if __name__ == '__main__':
    unittest.main()
```
```bash
python one.py
```

## Build-in assertions
```python
# assertions.py

import unittest
from unittest import TestCase


class TestAssertion(TestCase):

    def test_truth(self):
        self.assertTrue(1)
        self.assertFalse(0)
        self.assertFalse(0 == 1)

        self.assertIs("", str())
        self.assertIsNot(4.01, int())
        self.assertIsNone(None)
        self.assertIsNotNone(list())
        self.assertIsInstance("s", str)
        self.assertNotIsInstance("s", int)

        self.assertEqual(8.88 / 4, 2.22)
        self.assertNotEqual(8.88 / 4, 2.21)
        self.assertLess(8.88 / 4, 2.23)
        self.assertLessEqual(8.88 / 4, 2.22)
        self.assertGreater(8.88 / 4, 2.21)
        self.assertGreaterEqual(8.88 / 4, 2.22)

        self.assertIn(1, range(1, 5))
        self.assertNotIn(5, range(1, 5))

        with self.assertRaises(ZeroDivisionError):
            print(1 / 0)

        self.assertRegex("400", "\d+")
        self.assertNotRegex("400", "\s+")

        self.assertSequenceEqual(range(3), (0, 1, 2))
        self.assertListEqual(list(range(3)), [0, 1, 2])
        self.assertTupleEqual(tuple(range(3)), (0, 1, 2))
        self.assertSetEqual(set(range(3)), {0, 1, 2})
        self.assertDictEqual({2: "two", 1: "one"}, {1: "one", 2: "two"})

    def test_fail(self):
        self.assertDictEqual({2: "two", 1: "one"}, {1: "one", 2: "word"})


if __name__ == '__main__':
    unittest.main()
```
```bash
python assertions.py
```

## Command-line interface
```bash
python -m unittest -h
# run module
python -m unittest -v assertions.py
# run test class
python -m unittest -v assertions.TestAssertion
# run test case
python -m unittest -v assertions.TestAssertion.test_truth
# find some tests
python -m unittest discover
```

## Some useful `unittest` decorators
```python
# decorators.py
import unittest
import datetime


class TestBehaviour(unittest.TestCase):

    @unittest.skip('Skip test')
    def test_hard_skip(self):
        pass

    @unittest.skipUnless(
        datetime.datetime.now().strftime("%A") == "Wednesday",
        "Run on Wednesdays only"
    )
    def test_run_on_wednesdays(self):
        pass

    @unittest.skipIf(
        datetime.datetime.now().strftime("%A") == "Wednesday",
        "Don't run on Wednesdays"
    )
    def test_skip_on_wednesdays(self):
        pass

    @unittest.expectedFailure
    def test_expected_failure(self):
        self.assertTrue(False)

```
Run tests
```bash
python -m unittest -v decorators.py
```

## Tests suites
```python
# smoke_suite.py
import unittest
from one import TestScenario
from assertions import TestAssertion


def suite():
    suite = unittest.TestSuite()
    suite.addTest(TestScenario("test_number_is_set"))
    suite.addTest(TestScenario("test_number_greater_then_0"))
    suite.addTest(TestAssertion("test_truth"))
    return suite


if __name__ == '__main__':
    runner = unittest.TextTestRunner()
    runner.run(suite())
```
```bash
python smoke_suite.py
```

# References
- <https://docs.python.org/3/library/unittest.html>
- <https://www.blog.pythonlibrary.org/2016/07/07/python-3-testing-an-intro-to-unittest>
- <https://realpython.com/python-testing/>

# Practice
## Task description
Write tests for all functions located in `calculation.py` module.
```python
# calculation.py
def add(num1, num2):
    return num1 + num2


def subtract(num1, num2):
    return num1 - num2
```
Please use one test class for each function. Also, you need to write 3 tests for each function:
- all arguments are greater than 0
- all arguments are less than 0
- at least one argument is 0

After, please create two suites:
- a suite which runs tests where at least one argument is 0
- a suite which runs tests where all arguments are greater than 0

## Review items
Please send `.zip` archive of the project along with usage instructions.
