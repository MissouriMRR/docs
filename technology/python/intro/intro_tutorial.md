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
- [Set types](#set-types)
- [Mappings](#mappings)
- Callables
- Modules
- Custom classes
- Class instances
- Internal types
- Special types

### Numbers

- Numbers are immutable, once created their value cannot change
- Integral Numbers
    - Integers (`int`) - unlimited size, defined literally with nothing or + or - then only digits 
        - eg. 105, -6, +29934893274827348982374, 0)
    - Booleans (`bool`) - can only have a value of True or False
- Real Numbers (`float`) 
    - Double-precision floating-point numbers (w/ max value of 1.7976931348623157e+308)
    - Defined literally with decimal (integer then . then integer) or decimal then E then integer. Also defined as `float("inf")`, `float("-inf")`, or `float("nan")` for special values
        - eg. 0.5, 800.45, 45, 2.5E-100, 2584375.58324E25, 20., 0.0
- Complex Numbers (`complex`)
    - A pair of double-precision floats with `complex.real` and `complex.imag` component attributes
    - Defined literally with float then a + then a float with a j appended (ex. 2.5 + 10j, 0j, 5+4j, 500.0j)
        - ex. For a complex number `z`, the `z.real` and `z.imag` attributes can be used to access components

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
- A sequence `seq` can be indexed with value `i` using the bracket notation: `seq[i]` which will return the `(i+1)`th value in the sequence
- The built-in function `len()`, when passed a sequence, returns the number of elements in that sequence
- A sequence seq can be sliced using the notation `seq[start:stop:step]` where start, stop, and step are indices in seq corresponding to a slice of the sequence starting at start, up to (not including) stop, with a step of step
    - eg. if a sequence seq contains the elements "a", "b", "c", "d", "e", "f", "g", "h", then `seq[2:6:2]` returns the elements "c", "e" in a sequence of same type as seq
- Sequences are distinguished based on their mutability

#### More on Slicing

- A sequence seq can be sliced with notation seq[start:stop:step] 
- Some of these values have defaults and can thus be omitted.
- `start` defaults to the value `0` (the beginning of the sequence)
- `stop` defaults to `len(seq)-1` (the last index of the sequence)
- `step` defaults to 1 (traversing over each element consecutively)
- Using any of these values in a slice is redundant, so they should be omitted
- Examples:
    - `seq[:]` or `seq[::]` - a full (shallow) copy of the sequence seq is returned
    - `seq[start:]` - a slice of seq starting at start up to the end of seq is returned
    - `seq[:stop]` - a slice of seq starting at 0 up to (not including) stop is returned
    - `seq[::2]` - a slice of seq starting at 0 containing every 2nd value to the end is returned

#### Immutable Sequences

- Sequences whose values cannot change once created
- Strings (`str`)
    - Defined with a sequence of unicode “character” values surrounded by string literals (" or ')
        - eg. "0123", 'best string', "My name is 'Sam'", "The letter 'A' is my favorite"
        - ^ yes the opposite surrounding literal (" or ') can be used if such a character is desired
    - The empty string is formed with a pair of string literals as "" or ''.
- Tuples (`tuple`)
    - Defined as a comma-separated list of any arbitrary Python objects surrounded by parentheses
        - eg. (1, 2, 3, 4, 5) or ("A", "G",) or (1, 2, 3, 4, "too", True, False, "Hope") or (5,)
    - Note that the single-valued tuple must have a trailing comma, without it the parentheses do nothing (and are technically actually just used for operator precedence instead)
    - The empty tuple is formed with a pair of parentheses as ()
- Bytes (`bytes`)
    - An array of 8-bit bytes, represented by integers in the range [0, 256)
    - Defined with bytes literals: single string literals prefixed with a b (ex. b'abc' or b'100')

##### More on Strings

- Strings are UTF-8, so their range of values are U+0000 to U+10FFFF
- Python doesn’t have a char (character) type, instead each character in a string represents a (unicode) code point of length 1
- The built-in function `ord()` can be passed a single character in string literals and returns the corresponding unicode value (or ascii if applicable)
- Conversely, the built-in function `chr()` can be passed an integer and it will return the corresponding single-character string (or ascii character if applicable) of that number
- Escape sequences are used to escape certain string features such as if a single quote (\'), double quote (\"), newline (\n), tab space (\t), a backslash (\\\\), etc.
- Note that \' and \" should be avoided if possible in favor of choosing the opposite surrounding literal (" or ')

#### Mutable Sequences

- Mutable sequences can be changed after creation
- Indexing and slicing can be used for assignment and del (delete) statements
    - ex. For Mut. Sequence `seq`, `seq[5] = 10`, `del seq[5]`, and `seq[2:5] = [1,2,3]` are valid
- Lists (`list`)
    - An arbitrary sequence of Python objects, defined in CPython as a pointer array
    - Declared literally with a comma-separated list of objects surrounded by square brackets ([])
    - No special trailing-comma required for single-length lists
    - The empty list is declared with empty brackets []
- Byte Arrays (`bytearray`)
    - A mutable equivalent of the bytes object, created with built-in bytearray() constructor, when passed a bytes object
- Modules array and collections can be used for more mutable sequences

### Set Types

- Unordered finite sets of unique immutable objects
- They can’t be indexed or sliced, but they can be iterated and len() still works
- Sets are fast for membership testing, removing duplicates from sequences, and set operations (intersection, union, difference, symmetric difference, etc.)
- Sets (`set`)
    - A mutable mathematical set.
    - Constructed with a comma-separated list of unique objects surrounded by curly braces ({})
    - An empty set is created with the `set()` constructor. This can be modified later with new items.
- Frozen Set (`frozenset`)
    - An immutable mathematical set.
    - Constructed by passing a set (or nothing for empty) to `frozenset()`
    Since frozen sets are immutable, they can be elements of sets (or other frozen sets)

### Mappings

- Finite sets of objects indexed by arbitrary index sets.
- For every `item` in a mapping `my_map`, this maps a key `k` to a value `v` such that `my_map[k]` returns `v`
- Mutable mappings allow the assignment of new values to keys
- Dictionary (`dict`)
    - A mutable mapping representing a finite set of objects indexed by (nearly) arbitrary values
    - Implemented internally as a hash table (Means keys can be any hashable (immutable) object)
    - Dictionaries preserve insertion order, and iterating over a dictionary iterates over its keys
    - Defined literally with a comma-separated key-value pair list surrounded by curly braces where each element in a pair is separated by a colon
        - eg. `fruit_count = {"apples": 5, "oranges": 7, "grapes": 30}`
    - The empty dictionary is initialized with an empty pair of curly braces as {}

## If-Else

If-Else statements in Python are formatted as follows:

```python
if condition1:
    <block1>
elif condition2:
    <block2>
else:
    <block3>
```

The `elif` condition is only checked if the `if` condition is `False`. The `else` block is only executed if the `if` and `elif` are both `False`. The `elif` and `else` are optional.

## Loops

There are 2 kinds of loops in Python, for loops and while loops.

### For Loops

For loops interate over the elements of an iterator. The `range` function can be used to iterate over a range of numbers.

```python
for i in range(10):
    print(i) # will print the numbers 0 through 9
```

You can also iterate over the elements of a set, such as a `list`.

```python
l = ['a', 'b', 'c']
for character in l:
    print(character) # will print the elements of l
```

### While Loops

While loops will continue while a condition is `True`.

```python
number = 10
while number > 5:
    print(number) # will print out 10 through 6
    number -= 1
```

## Functions

Functions in Python are defined using the `def` keyword and can be defined with any number of parameters. However, since Python has dynamic typing, the type of the parameter cannot be enforced without adding additional statements to check. You can, though, use [type hints](/docs/python/type-annotations/full-guide.html) to specify what kind of input a function expects.

```python
def func1():
    print("This function has no parameters!")

def func2(var1):
    print("This function has a parameter with a value of", var1, ".")

def func3(var1: str):
    print("This function has a string parameter with a value of", var1, ".")

# Calling the functions
func1()

func2(5)

func3("my value")
func3(7) # NOTE: even though func3's var1 has a type hint of string,
         # it isn't enforced by the interpretor
```

### Lambda

Lambda functions are a type of function in Python that is anonymous. A `lambda` function is defined with a number of arguments and can have one expression.

```python
my_func = lambda x : x + 1

print(my_func(1)) # prints out 2
```

## Classes

Users can define types in Python with classes. A `class` can have any number of data elements and methods associated with it. The `__init__` function is called when an instance of that class is created. The `self` keyword refers to a specific instance of that class.

```python
class my_custom_class:
    def __init__(self, y):
        self.var1 = 5
        self.y = y # self.y refers to the instance, y refers to the parameter
    
    def mult_var1(self, x):
        self.var1 = self.var1 * x
    
    def get_y(self):
        return self.y

y = 7
my_obj = my_custom_class(y)

my_obj.mult_var1(8) # multiply var1 by 8
print(my_obj.var1) # print its new value

print(my_obj.get_y()) # get the value of y
```
