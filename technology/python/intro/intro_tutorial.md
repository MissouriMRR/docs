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

