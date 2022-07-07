---
layout: default
permalink: /python/type-annotations/full-guide.html
---


# **Python Type Annotations Full Guide**
This notebook explores how and when to use Python's type annotations to enhance your code.
Note: Unless otherwise specified, any code written in this notebook is written in the Python 3.10 version of the Python programming language. Lines of code that are feature-specific to versions 3.9 and 3.10 will be annotated accordingly. \
***IMPORTANT: Type annotations only need to be done on the first occurrence of a variable's name in a scope.***

---
 
<br>

# Table of Contents
- [Introduction](#introduction)
- [Basic Variable Type Annotations](#basic-variable-type-annotations)
  - [Dynamically (Union) Typed Variables](#dynamically-union-typed-variables)
    - [How To Use Union-typed Variables](#how-to-use-union-typed-variables)
  - [Optional Variables](#optional-variables)
- [Collections and Structural Sub-typing](#collections-and-structural-sub-typing)
  - [Nested Collections](#nested-collections)
  - [Tuple Unpacking](#tuple-unpacking)
- [Function Definitions and Callables](#function-definitions-and-callables)
  - [Callables](#callables)
  - [Larger Function Declaration Example](#larger-function-declaration-example)
- [Class Type Annotations](#class-type-annotations)
  - [Inheritance](#inheritance)
- [Iterators and Generators](#iterators-and-generators)
  - [Iterators](#iterators)
  - [Generators](#generators)
- [Advanced Python Data Types](#advanced-python-data-types)
  - [Enums](#enums)
  - [NamedTuples](#namedtuples)
  - [Dataclasses](#dataclasses)
  - [Numpy Arrays](#numpy-arrays)
    - [Shape Typing](#shape-typing)
    - [Data Type Typing](#data-type-typing)
  - [Other Advanced Types](#other-advanced-types)
- [Advanced Python Type Annotations](#advanced-python-type-annotations)
  - [Type Aliases](#type-aliases)
  - [New Types](#new-types)
  - [Type Variables](#type-variables)
  - [Generic Collections (ABC)](#generic-collections-abc)
  - [User-Defined Generics](#user-defined-generics)

---

<br>

# Introduction
Python **Type Annotations**, also known as type signatures or “type hints”, are a way to indicate the intended data types associated with variable names. In addition to writing more readable, understandable, and maintainable code, type annotations can also be used by static type checkers like **[mypy](https://mypy.readthedocs.io/en/stable/)** to verify type consistency and to catch programming errors before they are found the traditional way, at runtime. It should be noted that type annotations create no new logic at runtime and thus are designed to generate nearly zero runtime overhead, so there’s no risk of decreased performance.

The **[typing](https://docs.python.org/3/library/typing.html)** module is the core Python module to handle advanced type annotations.
Introduced in Python 3.5, this module adds extra functionality on top of the built-in type annotations to account for more specific type circumstances such as pre-python 3.9 structural subtyping, pre-python 3.10 union types, callables, generics, and others.

---

<br>


# Basic Variable Type Annotations
General form (with or without assigned value):
```yaml
<variable_name>: <typename> = initial_value
<unassigned_var>: <typename2>  # assign the var later but declare type now
```

Here are some examples of basic annotated types:
```py
# Basic types
count: int = 0
is_required: bool
price: float = 9.99
manager_name: str = "Bob"
```

<br>

## Dynamically (Union) Typed Variables
If the dynamic typing is needed, use `Union` (pre-Python 3.10, imported from `typing`) or the pipe operator, `|` (Python 3.10+):
```py
## Dynamic (Union) types
from typing import Union # not required Python 3.10+

dynamic: Union[int, str] = "I can be either int or str"  # pre-Python 3.10
dynamic_new: int | str = 100  # new alt. syntax in Python 3.10+
```
Since for Union types, you have no way of knowing/ensuring exactly what type a variable is at compile-time, you must use either `assert isinstance(...)` or `if isinstance(...)` statements to fulfill the runtime type-checking and type-safety that type checkers can’t verify. See examples below.

### How To Use Union-typed Variables
```py
import math

def add_five(val: int | str) -> int | str:
    result: int | str
    if isinstance(val, int):
        result = val + 5
    elif isinstance(val, str):
        result = int(val) + 5
    else:  # else not required if type checking, but it's a good practice
        raise TypeError(f"Unexpected type {type(val)} for val, must be int or str.")
    return result

def add_five_get_first_dig(val: int) -> int:
    val_res: int | str = add_five(val)
    # even though we passed add_five an int, we can't guarantee it returned an int
    assert isinstance(val_res, int) # assert this value should only ever be an int
    return val_res // 10**(round(math.log10(val_res))-1)
```

<br>

## Optional Variables
Oftentimes, values need the option to end up in a "null" or empty state. These are known as **optional** values, which use the type format `Optional[T]` where `T` is the possible non-None type. Alternatively, new to Python 3.10, the new `T | None` syntax may be used, as seen below.
```py
from typing import Optional # not required Python 3.10+

floaty_or_not: Optional[float] = None  # pre-Python 3.10
floaty_or_not_new: float | None = None  # new alt. syntax in Python 3.10+
```
For the same reason as union types, optional types should be only used after its exact type has been resolved at runtime. As a best practice, this means utilizing Python's `is` operator instead of the `==` operator to check for **identity** instead of equality. See example below.

<br>

*More info on basic variable type annotations: [PEP 526](https://peps.python.org/pep-0526/)*

---

<br>


# Collections and Structural Sub-typing
When making type annotations for a collection, it is important to also annotate the type of data that is stored within that collection. While collections should almost always be typed to their "deepest" known sub-type, there's a point where type annotations lose their elegance and instead may transform into monstrous nested strings of death. In such cases, Type Aliases may be used to reduce clutter (more on that later).


![red_bullet](https://via.placeholder.com/12/df0000/df0000.png) With your collections, don't do this:

```py
poorly_typed_list: list = ["the", "WRONG", "way", "to", "type"] # BAD
```

![green_bullet](https://via.placeholder.com/12/00df00/00df00.png) Instead, do this:

```py
pet_names: list[str] = ["Skippy", "Skittles", "Stewie"]  # list of strings

triple: tuple[int, int, str] = (1, 2, "skip-a-few")  # (int, int, str) triple

# undefined/varying length tuples
# may be later re-assigned to a different-sized tuple of the same type
amounts: tuple[int, ...] = (70, 80, 96, 110, 250) 
stuff: tuple[float | str, ...] = ("one", 0.5, 0.25, 0.125, "one-sixteenth")

data: set[int] = {1, 2, 10, 200, 1000}  # set of integers

fruit_probs: dict[str, float] = {  # dict mapping strings to floats
    "apple": 0.5,
    "orange": 0.3,
    "banana": 0.2,
}
```
In the case of JSON files and other cases where there are unknown types from a function call, annotate as far as is known about the result as possible (ex. at the least, we know `json.load` will return a dict mapping str to objects):
```py
from typing import Any
import json
...
config: dict[str, Any] = json.load(file)
# or alternatively,
config: dict[str, object] = json.load(file)  # no import needed, but requires assertions
...
```

*Note: For pre-Python 3.9 code, built-in collection types' annotations are imported from the typing module as
their uppercase variants (i.e. `List[int]`)*

<br>

## Nested Collections
```py
# dict mapping strings to tuples of int and int
restaurants: dict[str, tuple[int, int]] = {
    "McDonalds": (2, 2),
    "Taco Bell": (4, 6),
    "Applebee's": (11, 11),
}
```
```py
# if it gets crazy, create a TypeAlias or a NewType (more on that later):
from typing import TypeAlias # Available Python 3.10+

RestaurantsType: TypeAlias = dict[str, tuple[int, int]]  # Note: no "TypeAlias" annotation pre-Py3.10

restaurants: RestaurantsType = {
    ...  # etc. etc. etc.
}
```

<br>

## Tuple Unpacking
```py
# annotate the variables first, then perform the unpacking
mcds_x: int
mcds_y: int
mcds_x, mcds_y = restaurants["McDonalds"]
```
*Note: This is the only real way to do tuple unpacking right now (see [PEP 526](https://peps.python.org/pep-0526/#global-and-local-variable-annotations)). Hopefully in a future release they devise a more elegant method.*

---

<br>


# Function Definitions and Callables
Function definition type annotations are as follows. In this example, val3 has a default value of `"nice"`, and the return type of this function is `float`. As shown, return values from function calls are also annotated.
```py
def my_func(val1: int, val2: float, val3: str = "nice") -> float:
    return (val1 + val2) / 69 if val3 == "nice" else (val1 + val2)

result: float = my_func(1, 2.0)
```
Functions designed to never return look like this:
```py
from typing import NoReturn
def just_raise() -> NoReturn:
    raise RuntimeError("This function should never return")
```
## Callables
Callables are special types of objects that can be called. This object can be a function or an instance of a class that implements the `__call__` method. The type annotation is written as `Callable[[P], R]` where `P` is a comma-separated list of types corresponding to the types of the input parameters of the callable, in order, and `R` is the return type. Here are some examples of callables in practice:
```py
from typing import Callable

this_func: Callable[[int, float, str], float] = my_func # silly use-case, but gets the point across

cube_root: Callable[[int], float] = lambda x: x**(1/3) # lambda functions are callables

my_obj: Callable[[str], bool] = MyObject() # MyObject is a class that implements __call__
```
*More info: [Python `typing` - Callable](https://docs.python.org/3/library/typing.html#callable)*

## Larger Function Declaration Example
This example shows how Type Aliases and stacked parameters are used to reduce clutter. \
*Note: for pre-Python 3.10, remove the "TypeAlias" annotation, and use `Union`/`Optional` where relevant*
```py
from typing import TypeAlias

MyType: TypeAlias = int | str | float

def other_func(
    arg1: int,
    arg2: MyType,
    arg3: str | None = None,
    arg4: MyType | None = None,
) -> set[MyType | None]:
    return {arg1, arg2, arg3, arg4}
```

---

<br>


# Class Type Annotations
Classes are typed as you would expect although there are some nuances that are handled more explicitly. For instance, class variables must be explicitly typed as `ClassVar[T]` where `T` is the type of the class variable.
```py
from typing import ClassVar

class ThisObj:
    class_var: ClassVar[str] = "This is a class variable" # class variables are typed with ClassVar

    def __init__(self, var1: str) -> None:  # __init__ returns None
        self.instance_var1: str = var1  # instance variables (attributes) are typed as expected

    def __len__(self) -> int: # never type annotate self
        return len(self.instance_var1)
    
    def copy(self) -> ThisObj: # return type is a class
        return ThisObj(self.instance_var1)

# User-defined types are still typed as usual
my_this_obj: ThisObj = ThisObj("This is my type of type")
```
*Note: the method return-typed with the class name is a feature added in Python 3.10. Pre-Python 3.10 code can use this feature as well if the line `from __future__ import annotations` is written at the top of the file to enable it.*
## Inheritance
In cases where subtypes of a class are used, the subtype must be annotated with the supertype if the intention is to re-assign the variable between the subtypes of the supertype.
```py
class NewObj1(ThisObj): ...
class NewObj2(ThisObj): ...
```
![red_bullet](https://via.placeholder.com/12/df0000/df0000.png) Don't do this:
```py
obj1: NewObj1 = NewObj1("This is a type of object") # OK on its own
obj2: NewObj2 = NewObj2("This is another type of object") # OK on its own
obj2 = obj1 # BAD: this will fail the type checker :(
```
![green_bullet](https://via.placeholder.com/12/00df00/00df00.png) Instead, do this:
```py
new_obj1: ThisObj = NewObj1("This is a new type of object") # typed with supertype
new_obj2: ThisObj = NewObj2("This is another new type of object")  # typed with supertype
new_obj1 = new_obj2 # OK: satisfies "is a" relationship, passes type checker :)
```

---

<br>

# Iterators and Generators
 and generators are objects that implement `__next__` or are functions that include the keyword `yield` in the body.
## Iterators
Iterators are classes in python that implement `__iter__` and `__next__`. These are usually iterated over with for loops, but you can use them in other ways such as casting them directly to a sequence or even using the built-in `next` function to iterate manually. Iterator types are annotated as `Iterator[T]` where `T` is the type(s) of the items yielded.

Here is an example of an iterator that counts up in triplets until the max_val passed. 
```py
from typing import Iterator

class Tripler:
    def __init__(self, max_val: int) -> None:
        self.curr = 0
        self.max = max_val

    def __iter__(self) -> Iterator[int]: # this is the iterator of integers
        return self

    def __next__(self) -> int:
        self.curr += 3
        if self.curr >= self.max:
            raise StopIteration
        return self.curr

tripling: Iterator[int] = Tripler(30)

for i in tripling: ... # etc.
```

<br>

## Generators
Generators are like iterators in that they continuously "return" a next value, but they differ in that they can return objects instead of just numerical values, and they can be written as functions. Generator return types are annotated as `Generator[Y, S, R]` where `Y` is the type of the yielded values, `S` is the type of the values expected to be sent to the generator (if applicable), and `R` is the type of the return value of the generator (if applicable). Not all generators have send or return values, so these may be replaced with `None` if not applicable.

In the case below, the generator function only returns integers, so we can type it as an iterator of integers for simplicity's sake. However, if desired, it can also use the traditional method of generator typing.
```py
from typing import Iterator
def squares(max_val: int) -> Iterator[int]: # alt. Generator[int, None, None]
    i = 0
    while i < max_val:
        yield i**2
        i += 1
```
This next example cannot be typed as an iterator because it returns objects, so it's a generator.
```py
from typing import Generator

# Generator type format: Generator[YieldType, SendType, ReturnType]
def abcs(max_letter: str = "z") -> Generator[str, None, None]:
    for i in range(ord(max_letter) - ord("a") + 1):
        yield string.ascii_lowercase[i]
```
Here we have a generator with yield, send, and return support. This example's parameter `max_num` starts at infinity, and can be passed as either a an int or a float.
```py
from typing import Generator

# Generator type format: Generator[YieldType, SendType, ReturnType]
def n_divs(max_num: int | float = float("inf")) -> Generator[float, int, str]:
    i: int = 0
    while i <= max_num:
        reset_val = yield 1 / i # can be sent an int mid-iteration, yields floats
        if reset_val:
            i = reset_val
        elif reset_val == 0:
            return "FAIL" # returns strings
        else:
            i += 1
    return "SUCCESS"
```
*Note: the pipe ( | ) operator between types used above is not supported pre-Python3.10 (See [Dynamically Typed (Union) Variables](#dynamically-union-typed-variables)).*

---

<br>

# Advanced Python Data Types
## Enums
Enums don't need to be typed in their construction, but they do need to be annotated when used.
```py
from enum import Enum

class Direction(Enum):
    NORTH = 1
    EAST = 2
    SOUTH = 3
    WEST = 4

curr_direction: Direction = Direction.NORTH
```

<br>

## NamedTuples
NamedTuples are typed normally and constructed as a class inheriting from `typing.NamedTuple`.
```py
from typing import NamedTuple

class Color(NamedTuple):
    red: int
    green: int
    blue: int

purple: Color = Color(red=255, green=0, blue=255)
```
*Note: while there is an alternative way of creating namedtuples using the `collections` module, it is **not recommended** as it requires you to enter the type name as an argument string and does not support type annotation.*

<br>

## Dataclasses
Dataclasses are also typed as expected.
```py
from dataclasses import dataclass, field

class Employee:
    name: str
    age: int
    salary: float
    allergies: list[str] = field(default_factory=list)

my_empl: Employee = Employee(name="John", age=30, salary=100000, allergies=["peanuts"])
```

<br>

## Numpy Arrays
Numpy arrays are typed with the PyPI package [**nptyping**](https://pypi.org/project/nptyping/) (ver. 2.0.0+). This is so that we get explicit shape typing, and an overall cleaner annotation system. Unfortunately, this means that type checkers like mypy can't actually check the details of the typed numpy array (only whether the variable is or is not an ndarray), so at the moment, it's almost purely a glorified comment.

Type annotations are formatted as `NDArray[S, T]` where `S` is the intended shape of the array ([see nptyping Shape expressions](https://github.com/ramonhagenaars/nptyping/blob/master/USERDOCS.md#Shape-expressions)), and `T` is the intended data type of the array ([see nptyping dtypes](https://github.com/ramonhagenaars/nptyping/blob/master/USERDOCS.md#dtypes)). Additionally, the structure of an array can also be annotated ([see nptyping Structure expressions](https://github.com/ramonhagenaars/nptyping/blob/master/USERDOCS.md#Structure-expressions)).

### Shape Typing
Shapes are represented as strings containing a comma-separated list of integers corresponding to the shape of the ndarray. For example, a 2D array of shape (3, 4) would be represented as `"3, 4"`. In addition, shapes can also be more dynamically typed with wildcards (*) in place of single dimension numbers to represent any length for that dimension, and they can also be labeled and named. A full detailing of Shape expressions can be found [here](https://github.com/ramonhagenaars/nptyping/blob/master/USERDOCS.md#Shape-expressions).

### Data Type Typing
Data types are imported from `nptyping` explicitly. Some commonly used types that can be imported are `Int`, `UInt8`, `Float`, `Bool`, `String`, and `Any`. A full list of available dtypes can be found [here](https://github.com/ramonhagenaars/nptyping/blob/master/USERDOCS.md#dtypes)

Here are some examples of type annotated numpy arrays.
```py
import numpy as np
from nptyping import NDArray, Shape, Int, UInt8, Int16, Int32, String, Float32
from typing import Any

literally_anything: NDArray[Any, Any] = ... # any shape, any type

image: NDArray[Shape["1080, 1920, 3"], UInt8] = ... # 3-channel 8-bit image of 1920x1080 pixels

two_dimensional_arr: NDArray[Shape["*, *"], Int] = ... # any length but only 2 dimensions, Int32
square_arr: NDArray[Shape["T, T"], Int] = ... # repeated dimension sizes (T can anything)
n_dimensions_of_4: NDArray[Shape["4, ..."], Int] = ... # any n number of 4-long dimensions, Int32

nums: NDArray[Shape["5"], Int32] = np.arange(5) # shape=(5,) (Note: Int <=> Int32)

phrases: NDArray[Shape["*"], String] = np.array(["hello, world", "goodbye, world"]) # any 1D shape
phrases = np.append(phrases, "there is no world") # OK

points: NDArray[Shape["*, 2"], Int] = np.array([[1, 2], [3, 4]]) # 2 rows, any number of columns
better_pts: NDArray[Shape["*, [x, y]"], Int] = ... # type same as above, but w/ dimension breakdown

unlabeled_coords: NDArray[Shape["5, 2"], Float32] # 5x2 array of coordinates
labeled_coords: NDArray[Shape["5 coordinates, [x, y] wgs84"], Float32] = ... # 5 wgs84 coordinates
```
*See [Nptyping Documentation](https://github.com/ramonhagenaars/nptyping/blob/master/USERDOCS.md#user-documentation) for more info on how to use nptyping.*

*Note: While numpy does have its own `numpy.typing` library, for a variety of reasons, we no longer use this library and thus do not recommend it.*

<br>

## Other Advanced Types
Here are a list of other advanced types that are not covered in the above sections with links to their type annotation documentation:
* [attrs](https://mypy.readthedocs.io/en/stable/additional_features.html#the-attrs-package)
* [TypedDicts](https://mypy.readthedocs.io/en/stable/more_types.html#typeddict)
* [Awaitables & Asynchronous Iterators/Generators](https://mypy.readthedocs.io/en/stable/more_types.html#typing-async-await)
* [Final (Uninheritable) Attributes](https://mypy.readthedocs.io/en/stable/final_attrs.html)
* [metaclasses](https://mypy.readthedocs.io/en/stable/metaclasses.html)
* [Literals](https://mypy.readthedocs.io/en/stable/literal_types.html)

---

<br>

# Advanced Python Type Annotations
## Type Aliases
A Type Alias is simply a synonym for a type, and is used to make the code more readable. To create one, simply assign a type annotation to a variable name. Beginning in Python 3.10, this assigned variable can be typed with `typing.TypeAlias`. Here is an example.
```py
from typing import TypeAlias

ChestOfThings: TypeAlias = dict[str, tuple[list[int], str, float]] # Py3.10+ syntax
ChestOfThingsOld = dict[str, tuple[list[int], str, float]] # pre-Py3.10 syntax w/o annotation
# ChestOfThings = Dict[str, Tuple[List[int], str, float]] # pre-Py3.9 (types imported from typing)

my_chest: ChestOfThings = {}
```

<br>

## New Types
New Types are a way to definte types that *wrap* existing types in Python. What this means is that you can define a new type that is a **subtype** of an existing type with almost no class/inheritance overhead, and then use that new type in place of the existing type.
```py
from typing import NewType

PhoneNumber = NewType("PhoneNumber", int)
my_number = PhoneNumber(5733414111)

Email = NewType("Email", str)
my_email = Email("multirotor@mst.edu")
```

<br>

## Type Variables
Type Variables are a way to define a type that can be used in place of a type (with or without constraints on what those types may be), but is not a type. This is useful for defining generic types. \
Let's take a look at what the class signature for `typing.TypeVar`.
```py
TypeVar(
    name, # The name of the type variable
    *constraints, # optional specific constraints, type has to be one of these
    bound: __class__ = None, # upper bound (only this and subclasses of this)
    covariant: bool = False, # allow usage of more derived subtypes?
    contravariant: bool = False, # allow usage of less derived supertypes?
)
# By convention, append a _co to covariant and a _contra to contravariant type variable names
```
Here's an example of `TypeVar` in practice:
```py
from typing import TypeVar

T = TypeVar('T')  # Can be anything
A = TypeVar('A', str, bytes)  # Must be str or bytes

# here we can refer to the same arbitrary type 'T' throughout the function
def repeat(x: T, n: int) -> Sequence[T]:
    """Return a list containing n references to x."""
    return [x]*n

def longest(x: A, y: A) -> A:
    """Return the longest of two strings or bytes."""
    return x if len(x) >= len(y) else y
```

<br>

## Generic Collections (ABC)
Also known as "duck types", generic collections are a way of defining a type of collection that fits a certain set of operations. These types are all the **Abstract Base Classes** (ABCs) of common Python collections. \
For example, a `list` is an generic `Sequence`, and a `dict` is a generic `Mapping` \
Here are some examples of some common generic collections:
```py
from typing import Iterable, Sequence, Mapping, MutableMapping

# iterables support iteration (__iter__)
def add_two(vals: Iterable[int]) -> list[int]:
    return [(val + 2) for val in vals]

# any sequence is an iterable, but not all iterables are sequences
# sequence supports iteration , subscripting (__getitem__), length (__len__), in (__contains__),
# and reversed (__reversed__), and may also support the index and count methods.
def get_mid_val(my_seq: Sequence[float]) -> float:
    mid_arg = len(my_seq) // 2
    return my_seq[mid_arg]

# a mapping supports subscripting, iteration, length, in, the keys, items, values, and get methods,
# and the equality operators == and != (__eq__ and __ne__ respectively)
def dictify(my_mapping: Mapping[str, bool]) -> dict[str, bool]:
    result: dict[str, bool] = {}
    for key, val in my_mapping.items():
        result[key] = val
    return result

# Mutable Mappings are the same as Mappings except they also support subscripting with assignment 
# (__setitem__) and del  on an item(__delitem__)
def double_values(my_mut: MutableMapping[int, float]) -> MutableMapping[int, float]:
    for key, val in my_mut.items():
        my_mut[key] = 2*val
    return my_mut
```
*More information and other abstract base classes can be found [here](https://docs.python.org/3/library/collections.abc.html#collections-abstract-base-classes).*

<br>

## User-Defined Generics
Oftentimes when you want to create your own collection, you want to be adapatable as to what types it can take. In this case, we combine `TypeVar` and `Generic` to create a generic collection. \
Here are some simple examples:
```py
from typing import TypeVar, Generic

T = TypeVar('T')
class Jar(Generic[T]):
    def __init__(self, content: T) -> None:
        self.content: T = content
```
```py
from typing import TypeVar, Generic

T = TypeVar('T')
class Stack(Generic[T]):
    def __init__(self) -> None:
        self.items: list[T] = []

    def push(self, item: T) -> None:
        self.items.append(item)

    def pop(self) -> T:
        return self.items.pop()
```
*Note: the usage of `T` as a type variable is a convention and can be substituted with any name. Similarly, the convention for user-defined generic mappings or other paired values is `K`, `V` (usually used as Key, Value)*

---

<br>