# Python

## File I/O

### Text

```python
# read
text = open('textfile', 'r').read()
text = open('textfile', 'r').readlines()

with open('textfile', 'r') as f:
    for line in f:
        print line

# write
f = open('textfile', 'w')
for line in lines:
    f.write(line)
```

### CSV

```python
# read
import csv

csv_file = []
with open('file.csv', 'rb') as f:
    reader = csv.reader(f)
    for line in reader:
        csv_file.append(line)

csv_file = []
with open('file.csv', 'rb') as f:
    reader = csv.DictReader(f)
    for line in reader:
        csv_file.append([line['col1'], line['col2']])

# write
with open('file.csv', 'wb') as f:
    writer = csv.writer(f)
    writer.writerows(iterable)

list_of_dicts = [{'first': 'a', 'second': 1},
                 {'first': 'b', 'second': 2}]
with open('file.csv', 'wb') as f:
    fieldnames = ['second']
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    for line in list_of_dicts:
        line.writerow(line)
```

## Lists

```python
a = ['red', 'blue', 'green']      # manually initialization
b = list(range(5))                # initialize from iteratable
c = [nu**2 for nu in b]           # list comprehension
d = [nu**2 for nu in b if nu < 3] # conditioned list comprehension
e = c[0]                          # access element
f = c[1:2]                        # access a slice of the list
g = c[-1]                         # access last element
h = ['re', 'bl'] + ['gr']         # list concatenation
i = ['re'] * 5                    # repeat a list
['re', 'bl'].index('re')          # returns index of 're'
a.append('yellow')                # add new element to end of list
a.extend(b)                       # add elements from list `b` to end of list `a`
a.insert(1, 'yellow')             # insert element in specified position
're' in ['re', 'bl']              # true if 're' in list
'fi' not in ['re', 'bl']          # true if 'fi' not in list
sorted([3, 2, 1])                 # returns sorted list
a.pop(2)                          # remove and return item at index (default last)
```

## Dictionaries

```python
a = {'red': 'rouge', 'blue': 'bleu'}         # dictionary
b = a['red']                                 # translate item
'red' in a                                   # true if dictionary a contains key 'red'
c = [value for key, value in a.items()]      # loop through contents
d = a.get('yellow', 'no translation found')  # return default
a.setdefault('extra', []).append('cyan')     # init key with default
a.update({'green': 'vert', 'brown': 'brun'}) # update dictionary by data from another one
a.keys()                                     # get list of keys
a.values()                                   # get list of values
a.items()                                    # get list of key-value pairs
del a['red']                                 # delete key and associated with it value
a.pop('blue')                                # remove specified key and return the corresponding value
```

## Sets

```python
a = {1, 2, 3}                     # initialize manually
b = set(range(5))                 # initialize from iteratable
a.add(13)                         # add new element to set
a.discard(13)                     # discard element from set
a.update([21, 22, 23])            # update set with elements from iterable
a.pop()                           # remove and return an arbitrary set element
2 in {1, 2, 3}                    # true if 2 in set
5 not in {1, 2, 3}                # true if 5 not in set
a.issubset(b)                     # test whether every element in a is in b
a <= b                            # issubset in operator form
a.issuperset(b)                   # test whether every element in b is in a
a >= b                            # issuperset in operator form
a.intersection(b)                 # return the intersection of two sets as a new set
a.difference(b)                   # return the difference of two or more sets as a new set
a - b                             # difference in operator form
a.symmetric_difference(b)         # return the symmetric difference of two sets as a new set
a.union(b)                        # return the union of sets as a new set
c = frozenset()                   # the same as set but immutable
```

## Operators

```python
a = 2                             # assignment
a += 1 (*=, /=)                   # change and assign
3 + 2                             # addition
3 / 2                             # integer (python2) or float (python3) division
3 // 2                            # integer division
3 * 2                             # multiplication
3 ** 2                            # exponent
3 % 2                             # remainder
abs(a)                            # absolute value
1 == 1                            # equal
2 > 1                             # larger
2 < 1                             # smaller
1 != 2                            # not equal
1 != 2 and 2 < 3                  # logical AND
1 != 2 or 2 < 3                   # logical OR
not 1 == 2                        # logical NOT
'a' in b                          # test if a is in b
a is b                            # test if objects point to the same memory (id)
```

## Idioms

fatten list of lists

```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flat = [x for row in matrix for x in row]
>>> [1, 2, 3, 4, 5, 6, 7, 8, 9]

from itertools import chain
flat = list(chain.from_iterable(map(lambda x: x, matrix)))
>>> [1, 2, 3, 4, 5, 6, 7, 8, 9]

flat = list(chain(*[x for x in mat]))
>>> [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## Docstring Style Guide

```python
# -*- coding: utf-8 -*-
"""Example NumPy style docstrings.

This module demonstrates documentation as specified by the `NumPy
Documentation HOWTO`_. Docstrings may extend over multiple lines. Sections
are created with a section header followed by an underline of equal length.

Example
-------
Examples can be given using either the ``Example`` or ``Examples``
sections. Sections support any reStructuredText formatting, including
literal blocks::

    $ python example_numpy.py


Section breaks are created with two blank lines. Section breaks are also
implicitly created anytime a new section starts. Section bodies *may* be
indented:

Notes
-----
    This is an example of an indented section. It's like any other section,
    but the body is indented to help it stand out from surrounding text.

If a section is indented, then a section break is created by
resuming unindented text.

Attributes
----------
module_level_variable1 : int
    Module level variables may be documented in either the ``Attributes``
    section of the module docstring, or in an inline docstring immediately
    following the variable.

    Either form is acceptable, but the two should not be mixed. Choose
    one convention to document module level variables and be consistent
    with it.


.. _NumPy Documentation HOWTO:
   https://github.com/numpy/numpy/blob/master/doc/HOWTO_DOCUMENT.rst.txt

"""

module_level_variable1 = 12345

module_level_variable2 = 98765
"""int: Module level variable documented inline.

The docstring may span multiple lines. The type may optionally be specified
on the first line, separated by a colon.
"""


def function_with_types_in_docstring(param1, param2):
    """Example function with types documented in the docstring.

    `PEP 484`_ type annotations are supported. If attribute, parameter, and
    return types are annotated according to `PEP 484`_, they do not need to be
    included in the docstring:

    Parameters
    ----------
    param1 : int
        The first parameter.
    param2 : str
        The second parameter.

    Returns
    -------
    bool
        True if successful, False otherwise.

    .. _PEP 484:
        https://www.python.org/dev/peps/pep-0484/

    """


def function_with_pep484_type_annotations(param1: int, param2: str) -> bool:
    """Example function with PEP 484 type annotations.

    The return type must be duplicated in the docstring to comply
    with the NumPy docstring style.

    Parameters
    ----------
    param1
        The first parameter.
    param2
        The second parameter.

    Returns
    -------
    bool
        True if successful, False otherwise.

    """


def module_level_function(param1, param2=None, *args, **kwargs):
    """This is an example of a module level function.

    Function parameters should be documented in the ``Parameters`` section.
    The name of each parameter is required. The type and description of each
    parameter is optional, but should be included if not obvious.

    If \*args or \*\*kwargs are accepted,
    they should be listed as ``*args`` and ``**kwargs``.

    The format for a parameter is::

        name : type
            description

            The description may span multiple lines. Following lines
            should be indented to match the first line of the description.
            The ": type" is optional.

            Multiple paragraphs are supported in parameter
            descriptions.

    Parameters
    ----------
    param1 : int
        The first parameter.
    param2 : :obj:`str`, optional
        The second parameter.
    *args
        Variable length argument list.
    **kwargs
        Arbitrary keyword arguments.

    Returns
    -------
    bool
        True if successful, False otherwise.

        The return type is not optional. The ``Returns`` section may span
        multiple lines and paragraphs. Following lines should be indented to
        match the first line of the description.

        The ``Returns`` section supports any reStructuredText formatting,
        including literal blocks::

            {
                'param1': param1,
                'param2': param2
            }

    Raises
    ------
    AttributeError
        The ``Raises`` section is a list of all exceptions
        that are relevant to the interface.
    ValueError
        If `param2` is equal to `param1`.

    """
    if param1 == param2:
        raise ValueError('param1 may not be equal to param2')
    return True


def example_generator(n):
    """Generators have a ``Yields`` section instead of a ``Returns`` section.

    Parameters
    ----------
    n : int
        The upper limit of the range to generate, from 0 to `n` - 1.

    Yields
    ------
    int
        The next number in the range of 0 to `n` - 1.

    Examples
    --------
    Examples should be written in doctest format, and should illustrate how
    to use the function.

    >>> print([i for i in example_generator(4)])
    [0, 1, 2, 3]

    """
    for i in range(n):
        yield i


class ExampleError(Exception):
    """Exceptions are documented in the same way as classes.

    The __init__ method may be documented in either the class level
    docstring, or as a docstring on the __init__ method itself.

    Either form is acceptable, but the two should not be mixed. Choose one
    convention to document the __init__ method and be consistent with it.

    Note
    ----
    Do not include the `self` parameter in the ``Parameters`` section.

    Parameters
    ----------
    msg : str
        Human readable string describing the exception.
    code : :obj:`int`, optional
        Numeric error code.

    Attributes
    ----------
    msg : str
        Human readable string describing the exception.
    code : int
        Numeric error code.

    """

    def __init__(self, msg, code):
        self.msg = msg
        self.code = code


class ExampleClass(object):
    """The summary line for a class docstring should fit on one line.

    If the class has public attributes, they may be documented here
    in an ``Attributes`` section and follow the same formatting as a
    function's ``Args`` section. Alternatively, attributes may be documented
    inline with the attribute's declaration (see __init__ method below).

    Properties created with the ``@property`` decorator should be documented
    in the property's getter method.

    Attributes
    ----------
    attr1 : str
        Description of `attr1`.
    attr2 : :obj:`int`, optional
        Description of `attr2`.

    """

    def __init__(self, param1, param2, param3):
        """Example of docstring on the __init__ method.

        The __init__ method may be documented in either the class level
        docstring, or as a docstring on the __init__ method itself.

        Either form is acceptable, but the two should not be mixed. Choose one
        convention to document the __init__ method and be consistent with it.

        Note
        ----
        Do not include the `self` parameter in the ``Parameters`` section.

        Parameters
        ----------
        param1 : str
            Description of `param1`.
        param2 : :obj:`list` of :obj:`str`
            Description of `param2`. Multiple
            lines are supported.
        param3 : :obj:`int`, optional
            Description of `param3`.

        """
        self.attr1 = param1
        self.attr2 = param2
        self.attr3 = param3  #: Doc comment *inline* with attribute

        #: list of str: Doc comment *before* attribute, with type specified
        self.attr4 = ["attr4"]

        self.attr5 = None
        """str: Docstring *after* attribute, with type specified."""

    @property
    def readonly_property(self):
        """str: Properties should be documented in their getter method."""
        return "readonly_property"

    @property
    def readwrite_property(self):
        """:obj:`list` of :obj:`str`: Properties with both a getter and setter
        should only be documented in their getter method.

        If the setter method contains notable behavior, it should be
        mentioned here.
        """
        return ["readwrite_property"]

    @readwrite_property.setter
    def readwrite_property(self, value):
        value

    def example_method(self, param1, param2):
        """Class methods are similar to regular functions.

        Note
        ----
        Do not include the `self` parameter in the ``Parameters`` section.

        Parameters
        ----------
        param1
            The first parameter.
        param2
            The second parameter.

        Returns
        -------
        bool
            True if successful, False otherwise.

        """
        return True

    def __special__(self):
        """By default special members with docstrings are not included.

        Special members are any methods or attributes that start with and
        end with a double underscore. Any special member with a docstring
        will be included in the output, if
        ``napoleon_include_special_with_doc`` is set to True.

        This behavior can be enabled by changing the following setting in
        Sphinx's conf.py::

            napoleon_include_special_with_doc = True

        """
        pass

    def __special_without_docstring__(self):
        pass

    def _private(self):
        """By default private members are not included.

        Private members are any methods or attributes that start with an
        underscore and are *not* special. By default they are not included
        in the output.

        This behavior can be changed such that private members *are* included
        by changing the following setting in Sphinx's conf.py::

            napoleon_include_private_with_doc = True

        """
        pass

    def _private_without_docstring(self):
        pass
```
