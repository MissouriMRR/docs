---
permalink: /python/intro/tutorial/
---

# Python Intro

## Overview of Python

- High level
- Interpreted (and compiled)
- Dynamically typed
- Multi-paradigm
- Object-oriented Programming
- Structured Programming
- Imperative/Procedural Programming
- Functional Programming
- Parallel Computing
- Reference counted & Garbage-collected
- It’s design emphasises readability
- See [The Zen of Python](https://peps.python.org/pep-0020/)

To get install Python and set up your environment, check out [Setting up your Environment](/docs/python/set_up/).

## Running Python Code

You can run Python code by running a `.py` file or by running the REPL.

To run a Python file, use `python <filename>` on Windows or `python3 <filename>` on Linux. Python is an interpreted language, so when a file is run through the python interpreter, it is processed line-by-line until the end.

To open the REPL, just run `python` or `python3` in a terminal. The REPL is also known as the interpretor or "python in Interactive Mode". It is an interactive line-by-line-interpreting shell that reads an expression from the user into a data structure in internal memory, evaluates the data structure, yielding a result, and prints to standard output the result yielded by eval.

## Syntax

- Statements in python end in a newline
- Statements can be expanded over multiple lines using \\, parentheses, etc. for comma-separated or operator-separated expressions
- Blocks are indented 4 spaces (or 1 tab)
- Single-line comments begin with a #
- Multi-line comments (aka docstrings) are surrounded by either """ or '''

```python
message = "Hello World"

if x == 5:
    print("Number:", x)

if ( # single-line comment
    x == 5
    and y == 7
    and z == 10
):
    print(
        "Numbers:",
        x, y, z,
    )

"""
multi-line
comment
"""
```

### Naming Conventions

- **Variables** - snake_case
    - Use a trailing underscore instead of weird abbreviations to avoid naming collisions, eg. map_
- **Constants** - ALL_CAPS_SNAKE_CASE
- **Functions/Methods** - snake_case
    - Verbs are preferred over nouns. Function names should describe an action. 
        - Eg. is_epic() or collect_data()
    - Special or “dunder” methods are surrounded by double-underscores, eg. __init__
- **Classes** - PascalCase (aka. TitleCase or Capitalized camelCase)
    - Private/protected member attributes start with a leading underscore
- **Modules** - short, lowercase word(s), separated by underscores for readability
- **Packages** - short, lowercase word(s). Do not separate words w/ underscores.

### Basic I/O and REPL Functions

- **print()** - Prints a string or strings to the standard output. Passed either a string, string-castable object, or comma-separated list of such. If passed nothing, a new line is printed.
- **input()** - Takes a string from standard input. Can be optionally passed a “prompt” string or nothing to display to the user next to the input request.
- **help()** - Passed either nothing or a python class, object, function, package, module, etc. Pretty much anything. Use q to exit help() called on objects.
- **dir()** - If passed nothing, shows all of the names in the current local scope. If passed an object, it returns a list of the names of the all its attributes.
- **vars()** and **locals()**

### Dynamic Typing

- Python is dynamically typed, so you don’t have to declare types **explicitly**
- For example, in C++, to declare an integer variable, you must do this: `int count = 5;`, stating exactly what type you want before give it a value.
- Later on, setting the variable `count` to something like `true`, will throw an error
- However, in Python, the equivalent looks like this: `count = 5`, making the interpreter infer the type based on the value assigned.
- Later on in the code, you are then free to re-assign count freely with something like `count = "hello"`, although this is usually not a good practice

## Python Data Model

- Since python is object-oriented, __everything__ in python is an object and stems from the ultimate base python class `object`.
- This includes modules, classes, functions, methods, variables, constants.
- Since everything is an object, everything has attributes and methods
- All attributes of an object can be viewed with `dir(obj)`, which returns a list of attributes associated with obj (useful in REPL)
- Note: `dir()` can also be used with no arguments to get a list of all names in the current local scope

## Basic Python Data Types

- [Numbers](#numbers)
- [Sequences](#sequences)
- [Set types]()
- [Mappings]()
- [Callables]()
- [Modules]()
- [Custom classes]()
- [Class instances]()
- [Internal types]()
- [Special types]()

### Numbers

- Numbers are immutable, once created their value cannot change
- Integral Numbers
    - Integers (int) - unlimited size, defined literally with nothing or + or - then only digits 
        - eg. 105, -6, +29934893274827348982374, 0)
    - Booleans (bool) - can only have a value of True or False
- Real Numbers (float) 
    - Double-precision floating-point numbers (w/ max value of 1.7976931348623157e+308)
    - Defined literally with decimal (integer then . then integer) or decimal then E then integer. Also defined as float("inf"), float("-inf"), or float("nan") for special values
        - eg. 0.5, 800.45, 45, 2.5E-100, 2584375.58324E25, 20., 0.0
- Complex Numbers (complex)
    - A pair of double-precision floats with complex.real and complex.imag component attributes
    - Defined literally with float then a + then a float with a j appended (ex. 2.5 + 10j, 0j, 5+4j, 500.0j)
        - ex. For a complex number z, the z.real and z.imag attributes can be used to access components

#### More on Integers

- Integers can be represented in various ways
- If you have a long integer, you can section it up for readability w/ underscores
    - eg. 1_000_000 or 299_792_458 or even technically 1_24_673_23 (don’t do this tho pls)
- The underscores are completely ignored at runtime and are for readability only
- Integers can also be represented in binary, octal, and hexadecimal with prefixes 0b, 0o, and 0x accordingly:
    - eg. Binary: 0b0010001, Octal: 0o7214, Hexadecimal: 0xFF0FF

### Sequences

- Sequences represent a finite ordered set indexed by non-negative integers
- These are zero-indexed, so the first element is at index 0
- A sequence seq can be indexed with value i using the bracket notation: seq[i] which will return the (i+1)th value in the sequence
- The built-in function len(), when passed a sequence, returns the number of elements in that sequence
- A sequence seq can be sliced using the notation seq[start:stop:step] where start, stop, and step are indices in seq corresponding to a slice of the sequence starting at start, up to (not including) stop, with a step of step
    - eg. if a sequence seq contains the elements "a", "b", "c", "d", "e", "f", "g", "h", then seq[2:6:2] returns the elements "c", "e" in a sequence of same type as seq
- Sequences are distinguished based on their mutability
