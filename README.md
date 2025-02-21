Below is a comprehensive reStructuredText (reST) documentation template tailored for Python projects. It outlines best practices for module-, class-, and function-level docstrings; proper inline and block comments; and special cases such as async functions, decorators, context managers, dynamic properties, and more. Each section is accompanied by realistic Python examples that illustrate how Sphinx-friendly directives and reST markup can be used to maintain clarity, consistency, and readability.

========================================
Python Documentation Template (reST)

Overview

This template demonstrates how to document Python code using reST for Sphinx (or similar tools). It covers:
	•	Module-Level Docstrings: Guidelines for package and module headers.
	•	Class Documentation: Best practices for normal classes, nested classes, inherited classes, abstract base classes, mixins, and metaclasses.
	•	Function & Method Documentation: Documenting return types (single, tuples, optional), async functions, decorated functions, and context managers.
	•	Commenting Practices: Recommendations for inline comments, block comments, and strategies to avoid comment clutter.
	•	Edge Cases & Special Situations: Documenting dynamic attributes, magic methods, external references, and version control markers.
	•	Additional Layers: Guidelines for package-level docstrings, multi-lingual documentation, and documenting custom exceptions.

	1.	Module- and Package-Level Docstrings

Module docstring example (in your .py file):

.. code-block:: python

"""
.. module:: my_module
   :synopsis: This module provides advanced data processing functionalities.

This module contains classes and functions for processing data streams.
It supports multi-lingual documentation and integrates with Sphinx for auto-generation of docs.

.. note::
   Ensure that environment-specific configurations are set via environment variables.

"""

Package-level (in init.py):

.. code-block:: python

"""
MyPackage
=========

This package offers a suite of tools for data analysis and machine learning.
It is structured into modules for preprocessing, modeling, and evaluation.

.. versionadded:: 1.0
"""

	2.	Class Documentation

Basic class with attributes defined outside init and nested classes:

.. code-block:: python

class MyBaseClass:
    """
    MyBaseClass serves as the foundation for further extensions.

    Attributes:
        common_attr (str): A class-level attribute shared across instances.
    """

    # Class attribute defined outside __init__
    common_attr = "default_value"

    def __init__(self, param1: int):
        """
        Initializes MyBaseClass.

        :param param1: The primary parameter for initialization.
        :type param1: int
        """
        self.param1 = param1

    def some_method(self) -> str:
        """
        Returns a string representation of the parameter.

        :return: String representation of param1.
        :rtype: str
        """
        return str(self.param1)

    class NestedClass:
        """
        NestedClass is defined within MyBaseClass to encapsulate helper functionality.
        """

        def nested_method(self) -> bool:
            """
            Performs a nested operation.

            :return: Boolean result of the operation.
            :rtype: bool
            """
            return True

Subclass with inheritance and proper use of super():

.. code-block:: python

class DerivedClass(MyBaseClass):
    """
    DerivedClass extends MyBaseClass by adding new parameters and behavior.

    .. versionchanged:: 2.0
       Updated initialization to include param2.
    """

    def __init__(self, param1: int, param2: float):
        """
        Initializes DerivedClass.

        :param param1: Inherited parameter.
        :type param1: int
        :param param2: Additional parameter for extended functionality.
        :type param2: float
        """
        super().__init__(param1)
        self.param2 = param2

Abstract Base Class, Mixins, and Metaclasses:

.. code-block:: python

import abc

class AbstractProcessor(abc.ABC):
    """
    AbstractProcessor defines the interface for processing data.

    .. note::
       This is an abstract base class and should not be instantiated directly.
    """

    @abc.abstractmethod
    def process(self, data: dict) -> dict:
        """
        Processes input data and returns the processed output.

        :param data: Input data as a dictionary.
        :type data: dict
        :return: Processed data.
        :rtype: dict
        """
        pass

class MixinExample:
    """
    MixinExample provides supplementary methods to enhance class functionality.

    .. warning::
       Use this mixin only with classes that adhere to the expected interface.
    """

    def mixin_method(self) -> str:
        """
        Demonstrates a mixin method.

        :return: A string result from the mixin method.
        :rtype: str
        """
        return "Mixin result"

	3.	Function & Method Documentation

Documenting a simple function with single return type:

.. code-block:: python

def compute_sum(a: int, b: int) -> int:
    """
    Computes the sum of two integers.

    :param a: First integer.
    :param b: Second integer.
    :return: The sum of a and b.
    :rtype: int
    """
    result = a + b  # Inline comment: adding the two numbers
    return result

Handling multiple return types (tuple with mixed types):

.. code-block:: python

from typing import Tuple

def process_data(x: int) -> Tuple[int, str]:
    """
    Processes the input and returns both a numeric result and a descriptive message.

    :param x: An integer input.
    :return: A tuple where the first element is the processed integer and the second is a description.
    :rtype: Tuple[int, str]
    """
    processed = x * 10
    message = "Processing complete"
    return processed, message

Documenting an optional parameter:

.. code-block:: python

def optional_example(param: str = None) -> None:
    """
    Demonstrates documentation for an optional parameter.

    :param param: Optional string parameter. Defaults to None.
    :type param: str, optional
    """
    if param is None:
        # If no parameter provided, use default behavior
        param = "default"
    print(param)

Async function (coroutine) documentation:

.. code-block:: python

import asyncio

async def async_function(param: int) -> str:
    """
    An asynchronous function that simulates a delay.

    :param param: An integer parameter.
    :return: A formatted result string.
    :rtype: str
    :async:
    """
    await asyncio.sleep(1)
    return f"Result: {param}"

Documenting decorated functions (using functools.wraps):

.. code-block:: python

import functools

def my_decorator(func):
    """
    A decorator that preserves the original function's metadata.

    :param func: The function to be decorated.
    :return: The wrapped function.
    """
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        # Additional behavior can be added here.
        return func(*args, **kwargs)
    return wrapper

@my_decorator
def decorated_function(x: int) -> int:
    """
    A function decorated by my_decorator.

    :param x: An integer input.
    :return: The result of processing x.
    :rtype: int
    """
    return x * 2

Context manager documentation (enter and exit):

.. code-block:: python

class FileHandler:
    """
    FileHandler demonstrates a context manager for file operations.

    Usage example:

    .. code-block:: python

        with FileHandler("example.txt") as f:
            data = f.read()

    :param filename: The name of the file to handle.
    :type filename: str
    """

    def __init__(self, filename: str):
        """
        Initializes the FileHandler with a filename.

        :param filename: The file to open.
        :type filename: str
        """
        self.filename = filename

    def __enter__(self):
        """
        Opens the file and returns the file object.

        :return: The opened file.
        :rtype: file
        """
        self.file = open(self.filename, "r")
        return self.file

    def __exit__(self, exc_type, exc_value, traceback):
        """
        Ensures the file is properly closed.

        :param exc_type: Exception type, if raised.
        :param exc_value: Exception value.
        :param traceback: Traceback information.
        """
        self.file.close()

	4.	Commenting Best Practices

Inline Comments:

.. code-block:: python

def compute_product(a: int, b: int) -> int:
    """
    Returns the product of two numbers.

    :param a: First number.
    :param b: Second number.
    :return: The product.
    :rtype: int
    """
    product = a * b  # Multiply a and b to compute the product
    return product

Block Comments:

.. code-block:: python

# The following function implements a custom algorithm to accumulate values.
# The steps are:
# 1. Initialize the accumulator.
# 2. Iterate through each value in the list.
# 3. Update the accumulator.
# 4. Return the final sum.
def accumulate(values: list[int]) -> int:
    """
    Sums all integers in a list.

    :param values: A list of integers.
    :return: The accumulated sum.
    :rtype: int
    """
    total = 0
    for value in values:
        total += value  # Update total with the current value
    return total

	5.	Edge Cases & Special Situations

Dynamic Attributes / Properties:

.. code-block:: python

class Temperature:
    """
    Represents a temperature value with conversion to Fahrenheit.

    .. versionadded:: 1.1
    """

    def __init__(self, celsius: float):
        """
        Initializes Temperature with a Celsius value.

        :param celsius: Temperature in Celsius.
        :type celsius: float
        """
        self._celsius = celsius

    @property
    def fahrenheit(self) -> float:
        """
        Converts Celsius to Fahrenheit.

        :return: Temperature in Fahrenheit.
        :rtype: float
        """
        return (self._celsius * 9 / 5) + 32

Dunder (Magic) Methods:

.. code-block:: python

class Person:
    """
    Represents an individual with a name and age.
    """

    def __init__(self, name: str, age: int):
        """
        Initializes a Person instance.

        :param name: The person's name.
        :param age: The person's age.
        :type name: str
        :type age: int
        """
        self.name = name
        self.age = age

    def __str__(self) -> str:
        """
        Returns a user-friendly string representation.

        :return: A readable string.
        :rtype: str
        """
        return f"{self.name}, {self.age}"

    def __repr__(self) -> str:
        """
        Returns an unambiguous string representation for debugging.

        :return: Detailed string representation.
        :rtype: str
        """
        return f"Person(name={self.name!r}, age={self.age})"

Linking External References:

.. code-block:: python

def compute_factorial(n: int) -> int:
    """
    Computes the factorial of a non-negative integer.

    For more details, refer to the `Factorial Wikipedia page
    <https://en.wikipedia.org/wiki/Factorial>`_.

    :param n: A non-negative integer.
    :return: The factorial of n.
    :rtype: int
    """
    if n < 0:
        raise ValueError("n must be non-negative")
    if n == 0:
        return 1
    return n * compute_factorial(n - 1)

Version Control Markers and Custom Exceptions:

.. code-block:: python

def new_feature(x: int) -> int:
    """
    Implements a new feature with updated algorithm.

    .. versionadded:: 2.0
    .. versionchanged:: 2.1
       Improved performance by optimizing the loop.

    :param x: An integer input.
    :return: The processed value.
    :rtype: int
    """
    return x * 2

class CustomError(Exception):
    """
    CustomError is raised when a custom error condition is met.

    Attributes:
        message (str): Explanation of the error.
    """

    def __init__(self, message: str):
        """
        Initializes CustomError with an error message.

        :param message: Description of the error.
        :type message: str
        """
        super().__init__(message)
        self.message = message

	6.	Additional Documentation Layers

Concurrency Patterns and Environment Variables:

.. code-block:: python

import threading
import os

def threaded_function(name: str) -> None:
    """
    Function to be executed in a separate thread.

    :param name: Name or identifier for the thread.
    :type name: str
    """
    # Thread-specific logic here.
    pass

def get_config() -> dict:
    """
    Retrieves configuration from the environment variable ``APP_CONFIG``.

    :return: Configuration dictionary.
    :rtype: dict
    """
    config = os.getenv("APP_CONFIG", "{}")
    # Parse and return configuration details.
    return config

Usage with Sphinx

To generate documentation using Sphinx, ensure that your project’s conf.py is configured to include:
	•	The path to your modules.
	•	Extensions like sphinx.ext.autodoc for automatic docstring extraction.
	•	Options to process reST directives (e.g., versionadded, note, warning).

Example Sphinx snippet in conf.py:

.. code-block:: python

extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.napoleon',  # Supports Google and NumPy style docstrings if needed
]
autodoc_member_order = 'bysource'

Conclusion

This template serves as a practical guide to ensuring that Python code is well-documented, maintainable, and Sphinx-friendly. By adhering to these conventions, developers can ensure clarity and consistency across their codebase, making it easier for others (and future you) to understand and maintain the project.

Use this template as a starting point, adapting sections to the needs of your specific project.
