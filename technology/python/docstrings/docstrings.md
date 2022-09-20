---
permalink: /python/documenting-code/docstrings/
---

# Documentation and Docstrings

[Back to Python](/python/)

You should never write code that is impossible to read. Python makes it easy to NOT do that, but sometimes, stuff gets complex. Documentation is there to make your code more **readable and understandable**.

## Table of Contents

- [The Zen of Python](#the-zen-of-python)
- [Docstrings](#docstrings)

## The Zen of Python

If you open a Python repl and `import this`, you will be presented with text that describes some of the design philosophy behind Python.

Some key takeaways:

* If the implementation is hard to explain, it's a bad idea.
* If the implementation is easy to explain, it *may* be a good idea.

## Docstrings

A docstring is a string that describes a module, function, class, or method. It is surrounded by triple quotation marks (either single or double).

Since everything is an object in Python, the docstring is directly associated with the object as the built-in member object.__doc__, which means it’s viewable with help() as well as built-in function documentation viewers.

**Examples:**

```python
"""This is a one-liner docstring."""

"""
Here's another docstring except
spread over multiple lines.
"""
```

Docstrings are seperated into sections. Each section describes something specific such as function parameters or return types.

**Example of a Docstring with Sections:**

```python
def sum_all(elem: list[int]) -> int:
    """
    Sums all the elements of the list.

    Parameters
    ----------
    elem : list[int]
        The list of elements to sum.

    Returns
    -------
    sum : int
        The sum of the elements of elem.
    """
    total = 0
    for e in elem:
        total += e
    return total
```

Docstrings follow the following general syntax:

```python
"""
Description

Heading Name
------------
Heading Body

Heading Name 2
--------------
Heading Body 2
"""
```

### General Description Section

Each docstring should have a general description section. This section is pretty simple. 

- It doesn't need a heading.
- Should be clear and descriptive.

It can be a short or long description of object, but keep it simple and understandable. It may also include extended summary after it, also without a heading.

Note that a single one-liner description would have the triple quotes all on the same line.

**Example:**

```python
"""This is a one-liner docstring with only a description."""


"""
Here is a highly descriptive extended summary that requires more
than one line and is followed by the next section heading.

Example Section
---------------
"""
```

### Docstrings for Functions

We’re gonna use the numpydoc documentation style (with some additional modern typing standards).

- All sections are separated by an empty line.
- The Description is always required, and DOES NOT need a heading
- Parameters, Returns, Yields, Receives, Other Parameters, Raises, and Warns are all required only if they’re relevant
- Everything else is optional

**Sections in Order:**

```python
"""
<Description>
[Parameters]
[Returns]
[Yields]
[Receives]
[Other Parameters]
[Raises]
[Warns]
[Warnings]
[See Also]
[Notes]
[References]
[Examples]
"""
```

#### Parameters Section

- List of parameters the function can take on
- Can omitted if there are no parameters

**Syntax:**

```python
"""
Parameters
----------
param1_name : param1_type
    param1_description
param2_name : param2_type
    param2_description
similar_param3_name, similar_param4_name : common_param_type
    common_param_description
"""
```

**A much more complicated example:**

```python
"""
Parameters
----------
val : int
    A good description of val
vals : list[int]
    A good description of what’s in vals
some_quadruplet : tuple[bool, bool, bool, bool]
    A good description of what each value of the tuple means
(val1, val2, name) : tuple[int, int, str]
    A good description of what each value of the tuple means
some_choice : {"Choice A", "Choice B", "Choice C"}
    A good description of each possibility of some choice
my_bool : bool, default=False
    A good description of my_bool
my_float : float, optional (or float, default=None)
    A good description of my_float and what happens when it’s None
*args : tuple[MyType]
    Description of variable-length positional arguments parameter, may list possibilities or example
**kwargs : dict[str, MyType]
    Description of variable-length keyword arguments parameter, may list possibilities

"""
```

#### Returns Section

- Basically the same as Parameters, except return_name can be whatever you want, but should be descriptive and can be the return variable’s name
- This section may be omitted for NotImplemented return values or if the function has no return value (returns None always)

**Syntax:**

```python
"""
Returns
-------
return_name : return_type
    return_description
"""
```

**Examples:**

```python
# typed return: int
"""
Returns
-------
summation : int
    sum of all values
"""

# typed return: Union[int, float, bool, None]
"""
Returns
-------
int | float | bool | None
    summation : int | float | bool
        The sum of the values or logical or in the case of bools
    None (or alternatively “none : None”)
        result if any of the values were 0
"""

```

**A much more complicated example:**

```python
# typed return: Dict[str, Tuple[int, int, List[str]]]
"""
Returns
-------
locations : dict[str, tuple[int, int, list[str]]]
    the locations of the buildings on the map
    name : str
        the identifying name of the building
    bldg_info : tuple[int, int list[str]]
        row : int
            the row value of the building on the map
        col : int
            the column value of the building on the map
        managers : list[str]
            the names of the building managers
            name : str
                name of a building manager in the form of "FIRST MIDDLE LAST"
"""
```

#### Yields Section

- Only relevant for generators
- Similar to returns, except the return name isn’t required, but of course, the type is


**Syntax:**

```python
"""
Yields
------
yield_type
    a good description of the yield and how it relates to the next value
"""

# or

"""
Yields
------
yield_name : yield_type
    a good description of yield_name and how it relates to the next value
"""
```

**Examples:**

```python
"""
Yields
------
str
    the value of the next alphabetic letter
"""

"""
Yields
------
error : tuple[int, Optional[str]]
    err_code : int
        Non-zero value indicates error code, or zero on success.
    err_msg : Optional[str]
        Human readable error message, or None on success.
"""
```

#### Receives Section

- Also for generators only
- What the generator’s .send() method is expecting to receive.
- Similar to Parameters, but essentially follows same requisites as Yields

**Syntax:**

```python
"""
Receives
--------
receive_type
    a good description of the value expected to be received and how it affects the generator
"""

"""
Receives
--------
receive_name : receive_type
    a good description of the value expected to be received and how it affects the generator
"""
```

#### Other Parameters Section

- An separate additional (optional) section if you have infrequently used default parameters or a large number of keyword arguments
- Used primarily to the prevent cluttering of the main Parameters section
- Follows same rules as Parameters section

**Syntax:**

```python
"""
Other Parameters
----------------
etc.
"""
```

#### Raises Section

- A section listing all possible exceptions that a function could throw and under which conditions such errors get thrown
- Not needed if you’re raising only a NotImplementedError
- If you end up having a lot of raised errors, only list ones that are not obvious or are most likely to get raised

**Syntax:**

```python
"""
Raises
------
Exception1
    A good description of the exact circumstance(s) that would cause Exception1 to get thrown
Exception2
    A good description of the exact circumstance(s) that would cause Exception2 to get thrown
Etc.
"""
```

#### Warns Section

- Lists which warnings could get thrown by your function
- Warnings are called by the warn() function (from the warnings built-in module)
- Follows same rules as Raises section

**Syntax:**

```python
"""
Warns
-----
WarningName
    A good description of the circumstances that would cause WarningName
"""
```

#### Warnings Section

- Another optional section, non-essential, but could be nice to have
- Not to be confused with Warns section
- Simply a list of cautions to the user

**Syntax:**

```python
"""
Warnings
--------
- Don't do this one thing
- Don't do this other thing
- Be careful if you decide to do this thing
- Etc. etc. etc.
"""
```

#### See Also Section

- Another optional section, also non-essential, but exists in numpydoc
- Use it if there are other functions with slightly similar purpose, but better in a certain circumstance

**Syntax:**

```python
"""
See Also
--------
other_func : optional super-short desc of what other_func does
package.other_module.other_submodule.func
Potentially longer description of the func
other_func1, other_func2, other_func3
"""
```

#### Notes Section

- Another optional section that provides information about the code
- This section may include a discussion of the algorithm, calculations, mathematical equations in LaTeX format, and images
- See [numpydoc Notes section reference](https://numpydoc.readthedocs.io/en/latest/format.html#Notes) for a more detailed explanation of how to incorporate code-like math expressions with LaTeX and images in the Notes

#### References Section

- Optional section, also non-essential, but also part of numpydoc
- A place for references from your Notes section
- See [numpydoc References section reference](https://numpydoc.readthedocs.io/en/latest/format.html#References) for info and examples

#### Examples Section

- Optional section, also non-essential, but also part of numpydoc
- A place to put example usages of your function and the result
- See [numpydoc Examples reference](https://numpydoc.readthedocs.io/en/latest/format.html#References) for more advanced things like doctest


### Docstrings for Classes

Class docstrings must include a description, an attributes section, and optionally, a methods section.

- The methods section is only really necessary if there are a ton of public class methods and you want to distinctively reference major ones.
- Class variable attributes should be typed as ClassVar[Type] as usual, so you don’t need to mention class-variable-ness or instance-variable-ness

**Example:**

```python
class Photo(ndarray):
    """
    Array with associated photographic information.

    Attributes
    ----------
    exposure : float
        Exposure in seconds.

    Methods
    -------
    colorspace(c='rgb')
        Represent the photo in the given colorspace.
    gamma(n=1.0)
        Change the photo's gamma exposure.
    """
```

### Docstrings for Methods

- Never include self in the list of Parameters
- If the method executes a simple task such as a getter/setter decorator, then it may only include a description. Additional optional parameters, returns, or yields sections may be added if more detail is required. Otherwise, it follows the same convention as functions

### Docstrings for Modules

Modules should at the very least include a summary in the docstring at the very top of the file

**Sections:**

1. Summary (required, no section heading)
2. Extended Summary (optional, no section heading)
4. Constants (required for public constants, with heading)
3. Routine Listings (optional, no section heading)
4. See Also (optional, with heading)
5. Notes (optional, with heading)
6. References (optional, with heading)
7. Examples (optional, with heading)

### Additional Docstring Resources

#### autoDocstring VSCode Extension

For those of you who using VSCode for programming, there is an extension called autoDocstring, which can auto-format a templated layout of a docstring and auto-fill components based on type annotations. For this you have to set in the extension settings to use numpydoc.

Also, autoDocstring obviously won’t get everything, but very helpful and convenient to get a templated layout of the start to your docstring.

autoDocstring works by you typing triple quotes then pressing enter when you see the prompt.

#### A Note on NumPyDoc

For more information on numpydoc, reST (reStructuredText) markup, or some other examples of numpydoc, see the [numpydoc docs](https://numpydoc.readthedocs.io/en/latest/format.html#other-points-to-keep-in-mind)

Do keep in mind that the documentation we’re using is slightly different than numpydoc in cases like constants don’t have their own docstrings, types are referenced with structural subtyping, and other smaller nuances.

