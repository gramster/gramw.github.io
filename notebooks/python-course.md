.. title: A Python Crash Course
.. date: 2018-04-12 20:10
.. author: Graham Wheeler
.. category: Programming
.. comments: enabled

I've been teaching a crash course in data science with Python, which starts off with learning Python itself. The target audience is Java programmers (generally senior level) so its assumed that things like classes and methods are well understood. The focus is mostly on what is different with Python. I teach it using Jupyter notebooks but the content is useful as a blog post too so here we go:

## Introduction

### Python's Origins

Python was conceived in the late 1980s, and its implementation began in December 1989 by Guido van Rossum at Centrum Wiskunde & Informatica (CWI) in the Netherlands as a successor to the ABC language. It takes its name from Monty Python's Flying Circus.

Python is a dynamic language but is strongly typed (i.e. variables are untyped but refer to objects of fixed type).

> ### How Python Evolves
> 
> Python evolves in a fairly straightforward way, more-or-less like this:
> 
> - people propose changes by writing *Python Enhancement Proposals* (PEPs): https://www.python.org/dev/peps/
> - the Python core committee will assign a 'dictator' who will decide whether the PEP is worthy of becoming part of the standard, and if so it does, after some amount of discussion and revision
> - disagreements are finally settled by Guido van Rossum, Python's inventor and the 'Benevolent Dictator for Life' (BDFL)
> 
> An important standard PEP is the Style Guide, PEP-8 (https://www.python.org/dev/peps/pep-0008/). By default, PyCharm will warn of any PEP-8 violations. There are external tools such as `flake8` (https://gitlab.com/pycqa/flake8) that can be used to check code for compliance in other environments.


### StackOverflow is your friend!

For Python questions and Python data science questions, make use of StackOverflow. Pay attention to comments on suggested answers; the "accepted answer" is often not the best. Look for comments about whether it is the "most Pythonic". Python has an idiomatic style different to many other languages and so a novice coming from another language will often accept an answer that is closer to idiomatic in that other language rather than Python.

https://stackoverflow.com/questions/tagged/python

Also, if you're struggling to understand some code in your early days with Python, you may find this 'execution visualizer' helpful:

http://pythontutor.com/

### "Batteries Included"

Python is often described as having "batteries included". This is a reference to the rich set of libraries (packages)included in the standard distribution as well as the vast collection of freely available packages that can be used to bootstrap your development. Or, as Randall Munroe puts it:

![](https://imgs.xkcd.com/comics/python.png)

There are many thousands of Python packages available, often giving you many choices for similar purposes. One way to find quality packages is to look the curated lists at https://python.libhunt.com/ and https://awesome-python.com/

### Python 2.7 or Python 3.x?

You can use conda to create a Python 2.7 virtual environment for when you have to use 2.7, but all new projects should be Python 3.5 or later. Python 2.7 is the end of the 2.x line and will be end-of-lifed on Jan 1, 2020. Avoid it; the only reason to use it is if there is a package you really need that hasn't been ported yet.

## Python docs

https://docs.python.org/3/ has very detailed documentation.

Most Python packages have good documentation at https://readthedocs.org/

If you use Python a lot on a Mac you may find Dash useful; it is a utility that gives you fast access to context-sensitive help for many libraries: https://kapeli.com/dash

That said, Python has a help() function that is very useful.

## Using the REPL

To start the REPL (read-execute-print loop, or interactive interpreter), just type `python` at the command line.

Use the `help()` function to read the documentation for a module/class/function. As a standalone invocation, you enter the help system and can explore various topics.

Python scripts are stored in plain text files with `.py` extensions. You can run the script `foo.py` at the command line by invoking:

    python foo.py
    
When you do so the Python interpreter will compile the script to an intermediate bytecode, and the result will be stored in a file with the same base name and a `.pyc` extension. As an optimisation, the interpreter will look to see if a `.pyc` file with a more recent file modification date exists when you invoke it to run a script and use that if it does. In Python 2.x these files were saved alongside the Python source files but in Python 3.x they are stored in a subdirectory named `__pycache__`.

> ### A better REPL: bpython
> 
> bpython is an alternative REPL that adds a number of useful features at the command line, like syntax highlighting and auto-completion. 
> 
> https://www.bpython-interpreter.org/
> 
> You can install with `pip install bpython`.
> 
> If you're going to use the command line repl I recommend it, although there are other options too that I haven't tried:
> 
> - ptpython https://github.com/jonathanslenders/ptpython
> - DreamPie http://dreampie.sourceforge.net/
> 
> Yet another alternative to the REPL, of course, is Jupyter.
> 
> For the hard-core Pythonista, you can replace your entire shell with one based on Python; see http://xon.sh/.

## Quickstart - A Simple Example

Before diving into the details, let's look at a simple Python script to get a quick taste of what's to come. We're not going to go into details here but have annotated the code with some comments and if you are familiar with other object-oriented languages this should be quite easy to understand. Some things that may be unusual to you:

- No braces; in Python whitespace is significant. This can take some getting used to but isn't as bad as it seems once you do.
- Instance methods require an explicit "this" argument which in Python by convention is called `self` .
- Static methods have a `@staticmethod` decorator.
- The class constructor - of which there can only be one - is called `__init__`.
- Docstrings are specified using actual string literals inline rather than in comments.
- The method to convert to string is named `__str__` not `toString`.
- String formatting is done using embedded code in {} and preceding the string with 'f' (this is new to Python 3.6).


```python
import math  # import math module
from IPython.display import SVG, display

"""
A simple turtle graphics example that produces SVG output that can
be displayed in Jupyter.
"""

class Turtle:
    " Turtle graphics drawing to SVG path "  # class docstring
    
    DEG2RAD = math.pi/180  # class level variable
    
    @staticmethod
    def deg2rad(d):  # static method
        """ Convert degrees to radians """
        return d * Turtle.DEG2RAD
    
    def __init__(self):  # class constructor; "self" is like "this"
        # We don't declare instance variables explicitly in Python; we simply
        # assign values to them during construction. In this case we will
        # do all of that in the reset() method.
        self.reset()
        
    def reset(self):
        self.draw = True  # instance variable
        self.path = "M0,0 "
        self.x = self.y = 0
        self.turnto(0.0)
    
    def turnto(self, angle):
        " Turn to absolute angle. "
        self.angle = angle % 360.0
        self.dx = math.sin(Turtle.deg2rad(self.angle))
        self.dy = math.cos(Turtle.deg2rad(self.angle))
        
    def right(self, angle):
        " Relative turn "
        self.turnto(self.angle + angle)

    def left(self, angle):
        self.right(angle)
        
    def up(self):
        self.draw = False
        
    def down(self):
        self.draw = True
        
    def move(self, distance):
        " Relative move by distance "
        self.x = int(distance * self.dx)
        self.y = int(distance * self.dy)
        self.path += f"{'l' if self.draw else 'm'}{self.x},{self.y} "

    def moveto(self, x, y):
        " Absolute move to (x, y)"
        self.x = x
        self.y = y
        self.path += f"{'L' if self.draw else 'M'}{self.x},{self.y} "
        
    def svg(self):
        return '<svg id="doc" xmlns="http://www.w3.org/2000/svg" ' +\
            'version="1.1" width="500" height="500"><path d="' +\
            self.path +\
            '" stroke="green" fill="none" vector-effect="non-scaling-stroke" /></svg>'
            
    def __str__(self):
        " Convert to string representation. "
        return f"Turtle at {self.x},{self.y} facing {self.angle}"

            
def swisscross(turtle, level):  # top-level function
    " Swiss cross is a space filling curve. "
    if level >= 0:
        swisscross(turtle, level - 1)
        t.right(90)
        swisscross(turtle, level - 1)
        t.move(10)
        swisscross(turtle, level - 1)
        t.right(90)
        swisscross(turtle, level - 1)
        

t = Turtle()  # create class instance; note no 'new' 
t.up()
t.moveto(20, 30)
t.turnto(315)
t.down()
swisscross(t, 5)
t.move(10)
swisscross(t, 5)

# Display the result using SVG
display(SVG(t.svg()))
        
# final state
print(t)
```


![svg](/images/output_4_0.svg)


    Turtle at -7,-7 facing 315.0


## Installing Third-Party Packages

The standard way to install packages is with `pip install`. However, if you have installed `conda` you should use `conda install` first and only if that fails use `pip install`. Conda has a smaller set of packages which is why it doesn't always succeed, but the ones it does have have been built for Conda so installing that way is preferred.

Use `conda uninstall` or `pip uninstall` to remove packages.

To see what packages are installed use `pip freeze`.

There's a lot more to package installation than this but this is enough for 90%+ of what you will do.

## Python is an OOPL

Python is a pure object-oriented language. Operators like `+` are simply methods on a class. The Python interpreter will convert an infix operator to an instance method call.

For example, there is an `int` class for integers. There is an `__add__` method defined on that class for addition. So:    


```python
3 + 4
```




    7



is the same as:


```python
(3).__add__(4)
```




    7



The double underscore in Python is called *dunder* and is used extensively internally; `__add__` is called a *dunder-method*. Dunder-methods are important to understand if you want to take full advantage of Python hence this early introduction.

You can see the methods on a class by using the `dir` function, for example `dir(int)`.

We will discuss how to define new classes later. A key takeaway here is that this use of dunder-methods allows us to override many operators simply by overriding the associated dunder-method. Two particularly useful ones are `__str__` (cast to string) and `__repr__` (cast to text representation); these are typically the same for a class but need not be. For example, notice the differences here:


```python
a = "abc"
print(a.__str__())  # Equivalent to str(a)
print(a.__repr__())
```

    abc
    'abc'


## Indentation and Comments

Python does not use {} for demarcating blocks of code; instead it uses indentation. This distinguishes it from most other programming languages and can take some getting used to. In particular, it requires care when pasting code in an editor (most Python editors are smart about this but other editors are not). The reason for this choice is that Guido originally designed Python as a teaching language and favored readability.

The convention in Python is to indent with spaces, not tabs (this avoids tab settings causing misnterpretation of code). Indentation standard is 4 spaces at a time, although some companies have different conventions (usually 2, if not 4).

Comments start with # and continue to the end of the line. By convention if # is used on the same line as code it should be preceded by at least two spaces.

## Simple Functions

Python named functions are defined with `def`:


```python
def add(a, b):
    return a + b

add(2, 3)
```




    5




```python
add("cat", "hat")  # This is entirely legitimate; + concatenates strings
```




    'cathat'




```python
add("cat", 3)  # This is not allowed; Python typecasting must almost always be explicit
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-7-94b2f852ae18> in <module>()
    ----> 1 add("cat", 3)  # This is not allowed; Python typecasting must almost always be explicit
    

    <ipython-input-5-1315785ad0b1> in add(a, b)
          1 def add(a, b):
    ----> 2     return a + b
          3 
          4 add(2, 3)


    TypeError: must be str, not int


### import

Python code is packaged in the form of _packages_ consisting of one of more _modules_. A module is a single Python file, while a package is a directory of Python modules containing an additional `__init__.py` file, to distinguish a package from a directory that just happens to contain a bunch of Python scripts.

You install a package with `pip` or `conda`. Once installed, to use the package you must import it. You can also import modules although this is less common. 

There are several common ways of importing. Let's say we want to import a package `foo` that defines a class `Widget`:

* `import foo` will import the `foo` package; any reference to modules/classes/functions will need to be prefixed with `foo.`; e.g. `foo.Widget`
* `import foo as bar` will import the `foo` package with the alias `bar`; any reference to modules/classes/functions will need to be prefixed with `bar.`; e.g. `bar.Widget`
* `from foo import Widget` can be used to import a specific module/class/function from `foo` and it will be available as `Widget`
* `from foo import *` will import every item in `foo` into the current namespace; this is bad practice, don't do it.

### Writing a main function and handling command line arguments

The `sys` module lets us access command line arguments as `sys.argv:

```python
    #!/usr/bin/python

    import sys

    def main():
        # print command line arguments
        for arg in sys.argv[1:]:
            print arg

    if __name__ == "__main__":
        main()
```

The `__name__` variable is set to the name of the executing module, or `"__main__"` if this is the top-level module. The pattern shown, where we test `__name__` before executing any code, is a common one; it allows other Python scripts to safely import this one, improving reuse.

If you want to parse command-line arguments like flags etc, there is an `argparse` library as part of the standard distribution but a much easier way IMO is to use `docopt`: just write the help string and `docopt` generates the parse for you: http://docopt.org/. Another option to look at is `click`; it seems to be gaining popularity but I have not used it: http://click.pocoo.org/5/

## An Overview of Python Types

See https://docs.python.org/3/library/stdtypes.html for detailed documentation.

The main types are:

| TYPE      | GROUP     | MUTABLE? |
|-----------|-----------|----------|
| int       | Numerics  | N        |
| float     | Numerics  | N        |
| complex   | Numerics  | N        |
| str       | Sequences | N        |
| bytes     | Sequences | N        |
| bytearray | Sequences | Y        |
| list      | Sequences | Y        |
| tuple     | Sequences | N        |
| range     | Sequences | N        |
| set       | Sets      | Y        |
| frozenset | Sets      | N        |
| dict      | Mapping   | Y        |

In addition, modules, classes, instances, methods, and functions are all types. The Boolean constants `True` and `False`, and the value `None`, are instances of their own special types, and there are several other special cases like this. See the link above for more. Note that there is a string type but not a character type; characters are not treated any differently from other strings.

### The Boolean Truth Value of Types

Any object can be tested for truth value, for use in an `if` or `while` condition or as operand in a Boolean expression.

By default, an object is considered true unless its class defines either a `__bool__()` method that returns False or a `__len__()` method that returns zero, when called with the object. Zero numeric values are considered False, as are empty collections or sequences, and vice-versa.

Operations and built-in functions that have a Boolean result always return `0` or `False` for false and `1` or `True` for true, unless otherwise stated.

Important exception: the Boolean operations `or` and `and` always return one of their operands. This allows for useful defaults using Boolean expressions with `or`:


```python
s = None

name = s or "N/A"

print(name)
```

    N/A


### None

Python has no null object, but has a special object instance `None`.

To test if an object is `None`, use `is` or `is not`, not `==` or `!=`.


```python
a = None
print(a is None)
print(a is not None)
```

    True
    False


`is` tests if the arguments refer to the same object, while `==` tests if they have the same value (in general; in reality it does whatever the `__eq__` dunder-method on the left-hand-side argument defines). Python keeps a pool of string literals and reuses them if it can, so in the example below `a` and `b` both refer to the same string literal while `c` does not:


```python
a = "3"
b = "3"
c = f"{3}"
print(a == b)
print(a is b)
print(a == c)
print(a is c)
```

    True
    True
    True
    False


### Numbers

Most of the typical operators you know from other languages are supported. Here are some more-specific to Python:


```python
print(bool(3))  # Convert to Boolean
print(str(3))  # Convert to string
print(bool(0))
```

    True
    3
    False



```python
print(3 // 2)  # Integer division with truncation
print(3 / 2)  # Float division
```

    1
    1.5



```python
print(int(2.5)) # Convert to int with truncation
print(round(2.5))  # Convert to int with rounding (this one is odd; I'd expect it to round up)
print(round(2.5001))  # Convert to int with rounding
```

    2
    2
    3



```python
print(2 ** 3)  # Exponentiation
print(~3)  # Bitwise inverse
print(2**120)  # Python ints are arbitrary precision, not 64-bit
```

    8
    -4
    1329227995784915872903807060280344576



```python
print(2.0.is_integer())
print(2.5.as_integer_ratio())  # Convert to fraction tuple; we'll cover tuples later
```

    True
    (5, 2)


Note that `+=` and `-=` (and `*=`, etc) are supported but `++` and `--` are not. Use `+=1` and `-=1` instead.

Because even integer literals are objects with some overhead, Python has an optimization where it makes singleton instances of all small integers from -5 to 256. This can in rare situations trip you up. 


```python
a = 256
b = 257
c = -5
d = -6
print(a is 256)
print(b is 257)
print(c is -5)
print(d is -6)
```

    True
    False
    True
    False


### Strings

Python 3 strings are unicode. String literals can use single our double quotes (but must use same type to close as to open). Multi-line strings are most easily written using triple quotes.


```python
print('foo')
print("bar")
print('"foo"')
print("'bar'")
print("""I am a 
multiline string""")
```

    foo
    bar
    "foo"
    'bar'
    I am a 
    multiline string


You can use the usual suspects of `\n`, `\t`, etc in strings, and use `\` to escape special characters like quotes and `\` itself.


```python
a = "the cat sat on the mat"
print(len(a))  # len gets the length of the string; implemented by __len__
```

    22



```python
print("cat" in a)  # 'in' is implemented by __contains__
print("dog" in a)
```

    True
    False



```python
print(a[0])  # Implemented by __getitem__
a[0] = "t"  # No can do; strings are immutable.
```

    t



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-20-b63b8912561d> in <module>()
          1 print(a[0])  # Implemented by __getitem__
    ----> 2 a[0] = "t"  # No can do; strings are immutable.
    

    TypeError: 'str' object does not support item assignment



```python
# Some useful functions. Note these all return copies of the string; strings are immutable!
print(a.lower())
print(a.upper())
print(a.capitalize())  # Capitalize first letter
```

    the cat sat on the mat
    THE CAT SAT ON THE MAT
    The cat sat on the mat



```python
# Like any object that supports __len__ and __getitem__, strings are sliceable.
# Slicing uses [start:end] or [start:end:increment] where any of these are optional
# start defaults to 0, end to __len__(), and increment to 1. 
# start and end can be positive (from start of string) or negative (from end of string).

print(a[2:])   # skip first two characters
print(a[-7:])  # the last 7 characters
print(a[2:6])  # 4 characters starting after 2nd character
print(a[::2])  # Every second character
```

    e cat sat on the mat
    the mat
    e ca
    tectsto h a



```python
# Use find and rfind to find first/last occurence of a string; return offset or -1 if not found
# You can also use index/rindex which are similar but raise ValueError exception if not found.

print(a.find('he'))
print(a.rfind('he'))
print(a.find('cat'))
print(a.find('dog'))
```

    1
    16
    4
    -1



```python
# You can convert from character to ordinal or vice-versa with ord() and chr()
print(chr(65))
print(ord('A'))
```

    A
    65



```python
# Python has no character type, just string. So functions that would apply to just 
# a character in other languages apply to entire string in Python.
print("123".isdigit())
print("1X3".isdigit())
print("NOOOOooo".isupper())
```

    True
    False
    False


There are many more string operations available; these are just the basics. You can encode and decode strings using other encodings; see https://docs.python.org/3/howto/unicode.html for details.

### Lists

Lists are ordered, mutable sequences. They can be indexed, sliced (more on that below), appended to, have elements deleted, and sorted. They are heterogeneous. Examples:


```python
a = [1, 2, 3, "cat"]

print(a)
print(len(a))  # len() gives the length of the list
print(a[1])  # [] can be used to index in to the list; implemented by list.__getitem__; assignment uses list.__setitem__
print(a[-1])  # negative indices can be used to index from the end of the list (-1 for last element)
```

    [1, 2, 3, 'cat']
    4
    2
    cat



```python
# * can be used to create multiple concanenated copies of a list; implemented by list.__mul__
    
print(a)
a = a * 2 
print(a)
```

    [1, 2, 3, 'cat']
    [1, 2, 3, 'cat', 1, 2, 3, 'cat']



```python
# `in` can be used to check for membership; implemented by list.__contains__

print(a)
print('cat' in a)  
print('dog' in a)
```

    [1, 2, 3, 'cat', 1, 2, 3, 'cat']
    True
    False



```python
print(a)
print(['dog'] + a)  # + can be used to concanetenate lists; implemented by list.__add__
a.append('dog')  # append() can be used for concatenating elements
print(a)
```

    [1, 2, 3, 'cat', 1, 2, 3, 'cat']
    ['dog', 1, 2, 3, 'cat', 1, 2, 3, 'cat']
    [1, 2, 3, 'cat', 1, 2, 3, 'cat', 'dog']



```python
print(a)
print(a.index('dog')) # Get index of first matching entry; throws exception if not found
print(a.count('cat'))  # Count the number of instances of an element
```

    [1, 2, 3, 'cat', 1, 2, 3, 'cat', 'dog']
    8
    2



```python
print(a)
a.remove('dog')  # Remove first matching instance of element
print(a)
del a[-1]  # Remove element at index; implementedby list.__del__
```

    [1, 2, 3, 'cat', 1, 2, 3, 'cat', 'dog']
    [1, 2, 3, 'cat', 1, 2, 3, 'cat']



```python
# reverse() reverses the order of the list in place; implemented by list.__reversed__
print(a)
a.reverse()  
print(a)
```

    [1, 2, 3, 'cat', 1, 2, 3]
    [3, 2, 1, 'cat', 3, 2, 1]



```python
# for..in iterates over elements
    
print(a)
for elt in a: 
    print(elt)
```

    [3, 2, 1, 'cat', 3, 2, 1]
    3
    2
    1
    cat
    3
    2
    1



```python
# enumerate() will return tuples of index, value
print(a)
for i, v in enumerate(a):
    print(f'Value at index {i} is {v}')  # f'' is a format string that can contain code in {}
```

    [3, 2, 1, 'cat', 3, 2, 1]
    Value at index 0 is 3
    Value at index 1 is 2
    Value at index 2 is 1
    Value at index 3 is cat
    Value at index 4 is 3
    Value at index 5 is 2
    Value at index 6 is 1



```python
b = list(a)  # Makes a shallow copy; can also use b = a.copy()
print(b)
print(a == b)  # Elementwise comparison; implemented by list.__eq__
b[-1] += 1  # Add 1 to last element
print(a == b)
print(a > b)  # Compares starting from first element; implemented by list.__gt__
print(a < b)  # Compares starting from first element; implemented by list.__lt__
```

    [3, 2, 1, 'cat', 3, 2, 1]
    True
    False
    False
    True



```python
print(a)
a.pop()  # Removes last element
print(a)
a.pop(0)  # removes element at index 0
print(a)
```

    [3, 2, 1, 'cat', 3, 2, 1]
    [3, 2, 1, 'cat', 3, 2]
    [2, 1, 'cat', 3, 2]



```python
# You can join a list of words into a string
','.join(['cat', 'dog'])
```




    'cat,dog'




```python
# Like any object that supports __len__ and __getitem__, lists are sliceable.
# Slicing uses [start:end] or [start:end:increment] where any of these are optional
# start defaults to 0, end to __len__(), and increment to 1. 
# start and end can be positive (from start of string) or negative (from end of string).
x = [1, 2, 3, 4, 5, 6]
print(x[2:])
print(x[1:3])
print(x[-3:])
print(x[::2])
```

    [3, 4, 5, 6]
    [2, 3]
    [4, 5, 6]
    [1, 3, 5]



```python
# Use insert() to insert at some position. This is done in-place.
x.insert(2, 'A')
print(x)
x.insert(3, [1, 2])  # Note: insert() is for elements, so [1, 2] is a single element, not expanded
print(x)
```

    [1, 2, 'A', 3, 4, 5, 6]
    [1, 2, 'A', [1, 2], 3, 4, 5, 6]



```python
a.clear()  # empty the list
print(a)
```

    []


### Dicts

Dictionaries are mutable mappings of keys to values. Keys must be hashable, but values can be any object. 

---
_Under the hood_

A hashable object is one that defines a `__hash__` dunder-method, and an `__eq__` dunder method; if two objects are equal their hashes must be the same or the results may be unpredictable. 

---



```python
# dict literals (actually a list of dicts in this example)

contacts = [
    {
        'name': 'Alice',
        'phone': '555-123-4567'
    },
    {
        'name': 'Bob',
        'phone': '555-987-6543'        
    }
]
contacts
```




    [{'name': 'Alice', 'phone': '555-123-4567'},
     {'name': 'Bob', 'phone': '555-987-6543'}]




```python
# Use [key] to get an item; this calls dict.__getitem__
contacts[0]['name']
```




    'Alice'




```python
# Use dict[key] = value to change an item; this calls dict.__setitem__
contacts[0]['name'] = 'Carol'
contacts[0]
```




    {'name': 'Carol', 'phone': '555-123-4567'}




```python
# Trying to use a non-existent key raises an exception
contacts[0]['address']
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-44-0a84b14a0ce5> in <module>()
          1 # Trying to use a non-existent key raises an exception
    ----> 2 contacts[0]['address']
    

    KeyError: 'address'



```python
# You can avoid above and return a default value by using .get()
print(contacts[0].get('name', 'No name'))
print(contacts[0].get('address', 'No address'))
```

    Carol
    No address



```python
# Use 'in' to see if a key exists in a dict; this calls dict.__contains__
print('name' in contacts[0])
print('address' in contacts[0])
```

    True
    False



```python
# Test for equality with '==' and !=; this calls dict.__eq__ and dict.__ne__
print(contacts[0] == contacts[1])
print(contacts[0] == { 'name': 'Carol', 'phone': '555-123-4567'})
```

    False
    True



```python
# Use for-in to iterate over items; this calls dict.__iter__

for x in contacts[0]:
    print(x)
```

    name
    phone



```python
# Use len() to get number of items; this calls dict.__len__

print(len(contacts[0]))
```

    2



```python
# Use 'del' to delete a key from a dict; this calls dict.__delitem__
```


```python
# Use .clear() to empty dict (without changing references)

a = {'name': 'me'}
b = a
a.clear()
b
```




    {}




```python
# Contrast above with assigning empty dict
a = {'name': 'me'}
b = a
a = {}
b
```




    {'name': 'me'}




```python
# Use .keys(), .values() or .items() to get the keys, values, or both
```

There are some alternative implementations in the `collections` module; you won't need these now but they may come in handy in the future, especially the first two:

* `collections.OrderedDict`s remember the order of insertion so this is preserved when iterating over the entries or keys
* `collections.defaultdict`s can specify a type in the constructor whose return vaslue will be used if an entry can't be found
* `collections.ChainMap`s group multiple dictionaries into a single item for lookups; inserts go in the first dictionary

### Sets

A set is a mutable unordered collection that cannot contain duplicates. Sets are used to remove duplicates and test for membership. One use for sets is to quickly see differences. For example, if you have two dicts and want to see what keys are in one but not the other:


```python
a = {'food': 'ham', 'drink': 'soda', 'desert': 'ice cream'}
b = {'food': 'tofu', 'desert': 'cake'}

set(a) - set(b)
```




    {'drink'}



Sets are less commonly used than lists and dicts and we will not discuss them further here. You can read more here: https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset

### Tuples

Tuples are immutable sequences. Typically they are used to store record type data, or to return multiple values from a function. Tuples behave a lot like lists and support many of the same operations with similar behavior, aside from their immutability. We'll consider them briefly here.

The `collections` package defines a variant `namedtuple` which allows each field to be given a name; we won't go into that here other than to point out its existence. `collections` also defines a `deque` class; stacks are easy to implement just with the built-io list type.


```python
('dog', 'canine')  # tuple
```




    ('dog', 'canine')




```python
('dog')  # Not a tuple! This is just a string in parens
```




    'dog'




```python
('dog',)  # For a single-valued tuple, use a trailing comma to avoid above issue
```




    ('dog',)




```python
'dog',  # Parentheses are often optional
```




    ('dog',)




```python
# Indexing can be used to get at elements, much like lists
print(('dog', 'canine')[0])
print(('dog', 'canine')[1])
print(('dog', 'canine')[-2])
print(('dog',)[0])
print(('dog',)[1])
```

    dog
    canine
    dog
    dog



    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    <ipython-input-59-c2e4b522d95a> in <module>()
          4 print(('dog', 'canine')[-2])
          5 print(('dog',)[0])
    ----> 6 print(('dog',)[1])
    

    IndexError: tuple index out of range



```python
# We can unpack a tuple through assignment to multiple variables
a = ('dog', 'bone')
animal, toy = a
print(animal)
print(toy)
```

    dog
    bone



```python
# But need to ensure we use the right number of variables
a = ('dog', 'bone')
animal, toy, place = a
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-61-fee6f9af1778> in <module>()
          1 # But need to ensure we use the right number of variables
          2 a = ('dog', 'bone')
    ----> 3 animal, toy, place = a
    

    ValueError: not enough values to unpack (expected 3, got 2)



```python
a = ('dog', 'bone', 'house')
animal, toy = a
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-62-fff6c985f996> in <module>()
          1 a = ('dog', 'bone', 'house')
    ----> 2 animal, toy = a
    

    ValueError: too many values to unpack (expected 2)



```python
# Tuples allow us to do a neat trick in Python that is harder in many languages - swap two values without using a
# temporary intermediate.
# Note what is going on here: the RHS of the assignment is creating a tuple; the LHS is unpacking the tuple.

a = 1
b = 2
print(a,b)
a, b = b, a
print(a,b)
```

    1 2
    2 1


## Some built-in Functions

See https://docs.python.org/3.6/library/functions.html for a full list and more details.

`abs(num)` - Return absolute value


```python
print(abs(3))
print(abs(-3))
```

    3
    3


`all(iterable)` - returns True if all items in the iterable are True


```python
print(all([True, True, True]))
print(all([True, False, True]))
```

    True
    False


`any(iterable)` - returns True is any item in the iterable is True.


```python
print(any([False, False]))
print(any([False, True]))
```

    False
    True


`filter(fn, iter)` - construct an iterator from the elements of iterable object `iter` for which a function `fn` returns true.


```python
names = ["John Smith", "Alan Alda"]

# Get the names that start and end with same letter
for i in filter(lambda s: s[0].upper() == s[-1].upper(), names):
    print(i)
```

    Alan Alda


`input` - get input from the console


```python
n = input("What is your name?")
print(f'Hello {n}!')
```

    What is your name?Graham
    Hello Graham!


`isinstance` - check if an object has a certain type


```python
s = 'abc'
n = 123
print(isinstance(s, int))
print(isinstance(s, str))
print(isinstance(n, int))
print(isinstance(n, str))
```

    False
    True
    True
    False


`iter` - create an sequential iterable from an object; we will discuss iterables later


```python
x = iter([1, 2, 3, 4])
print(x)
print("Before first next()")
print(next(x))  # returns first item and advances
print("Before second next()")
print(next(x))  # returns second item and advances
print("After second next()")
for v in x:  # iterates through remaining items
    print(v)
```

    <list_iterator object at 0x10ff53978>
    Before first next()
    1
    Before second next()
    2
    After second next()
    3
    4


`len` - calls the object's `__len__` method to get the length.

`map` - similar to `filter` but returns an iterable with the results of applying the function


```python
names = ["John Smith", "Alan Alda"]

# Get a list of bools, one for each name, specifying if the name starts and ends with the same letter.
print(list(map(lambda s: s[0].upper() == s[-1].upper(), names)))
```

    [False, True]


`max(arg1,...)` - returns the largest arg. If a single iterable arg is given it will iterate.

`min(arg1, ...)` - returns the smallest arg


```python
print(max(2, 3, 1))  # Multiple scalar args
print(max([3, 2, 1])) # Single list arg
print(max([3, 2, 1], 4))  # Not allowed
```

    3
    3



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-72-5ebfda590ac7> in <module>()
          1 print(max(2, 3, 1))  # Multiple scalar args
          2 print(max([3, 2, 1])) # Single list arg
    ----> 3 print(max([3, 2, 1], 4))  # Not allowed
    

    TypeError: '>' not supported between instances of 'int' and 'list'


`next` - gets next item from an iterable; see the section on iterables and example for `iter` above.

`repr` - calls the object `__repr__` method to get a string representation. This is the *formal* representation while `__str__` returns the *informal* representation. Another way of thinking about this is that `__str__` returns the value of the object when used as a string, while `__repr__` returns a printable representation of the object's state. In Jupyter, when displaying an object, `__repr__` will be used if possible, with `__str__` used as a fallback. 

`reversed` - makes a copy of the object with items in reversed order (object must support `__len__` and `__getitem__`)

`round` - rounds number to some number of decimal places (default 0)


```python
pi = 3.1415927
print(round(pi))
print(round(pi, 3))
```

    3
    3.142


`sorted(list)` - returns a sorted version of the list.


```python
print(sorted([3, 1, 3]))
```

    [1, 3, 3]


`sum(iterable)` - returns the sum of the iterable


```python
print(sum([1, 2, 3]))
```

    6


`type(obj)` - return the type of an object


```python
print(type('foo'))
```

    <class 'str'>


`zip(list, ...)` - combines multiple lists into a single list of tuples. Note this returns a lazy iterable, not a list


```python
print(zip(['a', 'b', 'c'], [1, 2, 3]))
print(list(zip(['a', 'b', 'c'], [1, 2, 3])))  # instantiates the iterable as a list
```

    <zip object at 0x10fcdc408>
    [('a', 1), ('b', 2), ('c', 3)]


## String Formatting

String formatting has evolved over time with Python. Python 3.6 introduced "format strings" which allow code to be directly embedded in the string. This is an improvement over older approaches and we will use it extensively.
Format strings have an `f` prefix and include code in `{}`. For example:


```python
a = 10
print(f"2 x {a} = {2*a}")
```

    2 x 10 = 20


If you need to use the old approaches, there are a lot of details here: https://pyformat.info/ (this doesn't seem to cover format strings yet though). That site covers things like padding, justification, truncation, leading zeroes, fixing number of decimal places, etc. We won't cover these here except the latter:


```python
a = 1.23456
print(a)
print(f'{a:.2f}')  # Float restricted to two decimal places
print(f'{a:06.2f}')  # Float restricted to two decimal places and padded with leading zeroes if less than 6 chars
```

    1.23456
    1.23
    001.23


When you use `f'{a}'`, Python will look in turn for a `__format__`, a `__repr__` or a `__str__` method to call to get the string representation of `a`. You can force it to use `__repr__` with `f'{a!r}'` or to use `__str__` with `f'{a!s}'`.

## Sorting

We've already seen the `sorted` function, that can create a sorted list from any iterable:


```python
d = [3,5,2,4,1,7]
for i in sorted(d):
    print(i)
```

    1
    2
    3
    4
    5
    7


You can do a descending sort by adding a `reverse=True` argument:


```python
for i in sorted(d, reverse=True):
    print(i)
```

    7
    5
    4
    3
    2
    1


You can sort a list in place with `sort`, but this only applies to lists:


```python
print(d)
d.sort()
print(d)
```

    [3, 5, 2, 4, 1, 7]
    [1, 2, 3, 4, 5, 7]


You can read more about sorting here, including how to sort composite objects like dictionaries, tuples and nested lists, and by multiple keys: https://docs.python.org/3/howto/sorting.html

## Statements

Here we will consider statements. We'll leave some statements to when we get to exceptions, functions and classes.

For more info on statements see https://docs.python.org/3/reference/simple_stmts.html

### pass

The `pass` statement is a no-op. This is needed in Python as the language doesn't use braces, so it is the equivalent of `{}` in Java- or C-like languages.

### del

`del` is used to delete an object; it isn't used much but can be useful if the object uses a lot of memory to allow it to be garbage-collected.

### for, break and continue

You can loop over any iterable with `for...in`. `break` and `continue` are supported, and behave in the expected fashion.


```python
for i in ['green eggs', 'ham']:
    print(i)
```

    green eggs
    ham



```python
for i in 'green eggs':
    print(i)
```

    g
    r
    e
    e
    n
     
    e
    g
    g
    s



```python
for i in {'a': 1, 'b': 2}: # This will loop over keys
    print(i)
```

    a
    b



```python
for i in {'a': 1, 'b': 2}.values(): # This will loop over values
    print(i)
```

    1
    2



```python
for i in {'a': 1, 'b': 2}.items():  # This will loop over key-value pairs as tuples
    print(i)
```

    ('a', 1)
    ('b', 2)



```python
for i in [1, 2, 3]:
    print(i)
```

    1
    2
    3



```python
for i in enumerate([1, 2, 3]):  # Returns (index, value) tuples
    print(i)
```

    (0, 1)
    (1, 2)
    (2, 3)



```python
for index, value in enumerate([1, 2, 3]):  # We can unpack the (index, value) tuples
    print(f'At position {index} we have value {value}')
```

    At position 0 we have value 1
    At position 1 we have value 2
    At position 2 we have value 3



```python
for i in range(1, 10):
    print(i)
```

    1
    2
    3
    4
    5
    6
    7
    8
    9



```python
for i in range(1, 10, 2):
    print(i)
```

    1
    3
    5
    7
    9


Python has an unusual construct: for..else. The else part is executed if there was no early break from the loop.

This is a common construct in other languages:

```python
    # See if the list has an even number and then take an action.
    has_even_number = False
    for elt in [1, 2, 3]:
        if elt % 2 == 0:
            has_even_number = True
            break
    if not has_even_number:
        print "list has no even numbers"
```

but in Python, we can just do:

```python
    for elt in [1, 2, 3]:
        if elt % 2 == 0:
            break
    else:
        print "list has no even numbers"
```

I.e. the `else` statement will be executed if the loop completes normally (does not exit through a `break`).

### while

`while` loops are very straighforward:


```python
i = 0
while i < 10:
    print(i)
    i += 2
```

    0
    2
    4
    6
    8


`while...else` is supported:


```python
i = 0
while i < 10:
    print(i)
    i += 2
else:
    print('Done')
```

    0
    2
    4
    6
    8
    Done



```python
i = 0
while i < 10:
    print(i)
    if i % 2 == 0:
        print('Found an even number!')
        break
    i += 2
else:
    print('No even numbers!')
```

    0
    Found an even number!



```python
i = 1
while i < 10:
    print(i)
    if i % 2 == 0:
        print('Found an even number!')
        break
    i += 2
else:
    print('No even numbers!')
```

    1
    3
    5
    7
    9
    No even numbers!


### if Statement and Boolean Expressions

Python uses `if...elif...else` syntax:


```python
grade = 75
if grade > 90:
    print('A')
elif grade > 80:
    print('B')
elif grade > 70:
    print('C')
else:
    print('D')
```

    C


`and`, `or` and `not` are Boolean operators, while `&`, `|` and `^` are bitwise-operators. Short-circuiting rules apply:


```python
1 and 1/0
```


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-98-d26a3ac7f29d> in <module>()
    ----> 1 1 and 1/0
    

    ZeroDivisionError: division by zero



```python
1 or 1/0
```




    1




```python
0 and 1/0
```




    0




```python
0 or 1/0
```


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-101-a829942d3284> in <module>()
    ----> 1 0 or 1/0
    

    ZeroDivisionError: division by zero


You can combine multiple range comparisons into a single one:


```python
print(0 < 2 < 4)
print(2 < 0 < 4)
```

    True
    False


Note that the Boolean literals are `True` and `False`, with capitalized first letters.


```python
print(0 < 2 < 4 < 6)
```

    True


If an instance of a class is used in a Boolean expression, it is evaluated by calling its `__bool__` method if it has one, else its `__len__` method (where non-zero is `True`), else it is considered `True`.

Python doesn't support conditional expressions like `:?` but does support ternary expressions with `if...else`:


```python
for count in range(0, 3):
    print(f'{count} {"Widget" if count == 1 else "Widgets"}')
```

    0 Widgets
    1 Widget
    2 Widgets


### with

`with` is used for scoped use of classes that need to clean up when they are no longer used (e.g. file objects that need to release underlying file handles). 

The most common place you'll see this is with file reading and writing, which we conver in the next section.

---
> _Under the Hood_
>
> When the “with” statement is executed, Python evaluates the following expression, calls the `__enter__` method on the resulting value (a “context guard”), and assigns whatever `__enter__` returns to the variable given by as. Python will then execute the code body, and no matter what happens in that code, call the guard object’s `__exit__` method.
> 
> As an extra bonus, the `__exit__` method can look at the exception, if any, and suppress it or act on it as necessary (to suppress it, it just needs to return `True`).
> 
> We're getting ahead of ourselves here with classes, but here is an example:


```python
class Wither:
    def __enter__(self):
        return 'green eggs'
    def __exit__(self,  type, value, traceback):
        print('ham')
    
with Wither() as x:
    print(x)
```

    green eggs
    ham


## Reading and Writing Files

Python has a built-in `open` function for opening files for reading and writing: https://docs.python.org/3.6/library/functions.html#open

The simplest for of reading a file is just:

```python
with open('myfile.txt') as f:
    for line in f:
        print(line)
```

and writing a file, assuming we have a list of strings `data`:

```python
with open('myfile.txt', 'w') as f:
    for line in data:
        f.write(line)
```

You can see more detailed examples in the tutorial, section 7.2, here: https://docs.python.org/3/tutorial/inputoutput.html

If you are doing more sophisticated operations with files you may want to look at the `pyfilesystem` package: https://www.pyfilesystem.org/. This provides a richer set of functionality over a variety of different "virtual" file systems, like zipfiles, tarfiles, FTP, SMB, DLNA and WebDAV servers, and services like DropBox.

## Functions and Lambdas

Recall that Python named functions are defined with `def`:


```python
def add(a, b):
    return a + b

add(2, 3)
```




    5



Default arguments are allowed. If a default argument is specified, then all following arguments must have defaults as well:


```python
def add(a, b=1):
    print(f'a={a}, b={b}')
    return a + b

print(add(2, 3))
print(add(2))
print(add())
```

    a=2, b=3
    5
    a=2, b=1
    3



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-107-ad63163207a7> in <module>()
          5 print(add(2, 3))
          6 print(add(2))
    ----> 7 print(add())
    

    TypeError: add() missing 1 required positional argument: 'a'


Arguments with no defaults are "positional" arguments and must be specified in order _except_ if they are named explicitly when calling the function:


```python
print(add(b=2, a=1))
```

    a=1, b=2
    3


Variables referenced in a function are either local or arguments. To access a global variable you must explicitly declare it global (but it is better to avoid using globals):


```python
x = 2

def foo():
    x = 1  # This is local
    
print(x)  # This is the global
foo()
print(x)
```

    2
    2



```python
x = 2

def foo():
    global x
    x = 1
    
print(x)
foo()
print(x)
```

    2
    1


Functions can be nested. In Python 3 you can declare a variable as "nonlocal" to access an outer but non-global scope.


```python
def outside():
    msg = "Outside!"
    def inside():
        msg = "Inside!"  # This is different to the one in outside()
        print(msg)
    inside()
    print(msg)
    
outside()
```

    Inside!
    Outside!



```python
def outside():
    msg = "Outside!"
    def inside():
        nonlocal msg  # This is the same as the one in outside()
        msg = "Inside!"
        print(msg)
    inside()
    print(msg)
    
outside()
```

    Inside!
    Inside!


It is good practice to follow the `def` line with a _docstring_ to document the function. There are different conventions for how this should be formatted; I like the Google style: http://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html


```python
def add(a, b):
    """Adds two objects and returns the result.

    Args:
        a: The first parameter.
        b: The second parameter.

    Returns:
        The result of adding a and b.
    """
    return a + b

# Now we can use help() to get the docstring.
help(add)
```

    Help on function add in module __main__:
    
    add(a, b)
        Adds two objects and returns the result.
        
        Args:
            a: The first parameter.
            b: The second parameter.
        
        Returns:
            The result of adding a and b.
    


You can return multiple values from a function (really just a tuple):


```python
def sum_diff(a, b):
    return a+b, a-b

print(sum_diff(3, 2))
x, y = sum_diff(4, 5)
print(x)
print(y)
```

    (5, 1)
    9
    -1


Python supports continuations with yield (this returns a generator which we will dicuss later):


```python
def get_next_even_number(l):
    for v in l:
        if v % 2 == 0:
            yield v
    
x = [1, 2, 3, 4, 5, 6]
for e in get_next_even_number(x):
    print(e)
```

    2
    4
    6


You can use `*args` for a variable number of non-keyword arguments, which will be available internally as a list:


```python
def multiply(*args):
    z = 1
    for num in args:
        z *= num
    return z
    
print(multiply(1, 2, 3, 4))
```

    24



```python
def foo(*args):
    for i in range(0, len(args)):
        print(f'Argument {i} is {args[i]}')

        
foo(1, 2, 'cat')
```

    Argument 0 is 1
    Argument 1 is 2
    Argument 2 is cat


For keyword arguments, you can use `**kwargs`, which will be available internally as a dictionary:


```python
def foo(*args, **kwargs):
    for i in range(0, len(args)):
        print(f'Positional argument {i} is {args[i]}')
    for k, v in kwargs.items():
        print(f'Keyword argument {k} is {v}')
        
foo('cat', 1, clothing='hat', location='mat')
```

    Positional argument 0 is cat
    Positional argument 1 is 1
    Keyword argument clothing is hat
    Keyword argument location is mat


You can mix all types of arguments but the order is important:
* Formal positional arguments
* `*args`
* Keyword arguments
* `**kwargs`

You can do the opposite as well - pass a list instead of several positional arguments, and a dictionary instead of several keyword arguments, by using `*` and `**`:


```python
def foo(pos1, pos2, named1='a', named2='b'):
    print(f"Positional 1 is {pos1}")
    print(f"Positional 2 is {pos2}")
    print(f"Named1 is {named1}")
    print(f"Named1 is {named2}")    
    
p = [1, 2]
n = {'named1': 'cat', 'named2': 'hat'}
foo(*p, **n)
```

    Positional 1 is 1
    Positional 2 is 2
    Named1 is cat
    Named1 is hat


The above is actually a common pattern in Python when writing wrapper functions that need to support arbitrary arguments that they are just going to pass on to some other function. For example, say we wanted to write a wrapper that timed the execution of a function:


```python
import datetime as dt


def foo(a, b=None, c=None):
    print(f'a={a}, b={b}, c={c}')


def log_time(fn, *args, **kwargs):
    start = dt.datetime.now()
    fn(*args, **kwargs)
    end = dt.datetime.now()
    print(f"{fn} took {(end-start).microseconds} microseconds")
    
log_time(foo, 1, c='hello')
    
```

    a=1, b=None, c=hello
    <function foo at 0x10ff6dae8> took 58 microseconds


Note that `def` statements are executed at their level of indentation, and they create function objects that can be called later, including evaluating the default argument values. This means you should be careful when specifying default values for arguments; stick to scalar variables. In particular avoid using things like empty lists! Look at how this can go wrong:


```python
def beware(a=[]):
    print(a)
    a.append('gotcha!')
    
beware()
beware() # No longer empty list!
```

    []
    ['gotcha!']


What heppened above is that the empty list argument was created at function definition time, and at function call time a is assigned a default value which is a reference to the previously created list object. If the list changes those changes will persist.

Instead, use something like:


```python
def beware(a=None):
    if a is None:
        a=[]
    print(a)
    a.append('gotcha!')
    
beware()
beware() # Now we are safe
```

    []
    []


Finally, you can use `lambda` to define anonymous functions. These will be very useful when we get to using Pandas for data manipulation:


```python
adder = lambda a, b: a + b

adder(1, 2)
```




    3



## Classes


```python
class Widget:  # same as "class Widget(object):"
    """ This is a Widget class. """  # Classes have docstrings too.
    
    def print_my_class(self):  # Instance method as it has a 'self' parameter
        """ Print the instance class. """
        print(self.__class__)  # __class__ is the easy way to get at an object's class
    
    @staticmethod
    def print_class():  # Static method as it has no 'self' parameter
        """ Print the class class. """
        print(Widget)
        
        
x = Widget()  # We don't use 'new' in Python
x.__doc__  # __doc__ has the docstring
```




    ' This is a Widget class. '



In Python, we can declare a class with `class(base)`. If the base class is omitted then `object` is assumed.

As mentioned earlier, instance methods take an explicit `self` first parameter which references the instance. So if `widget` is an instance of a `Widget` class and we call:

```python
widget.foo()
```

internally that gets converted to the equivalent of:

```python
Widget.foo(widget)
```

To declare an instance method, we omit the `self` argument and use a `staticmethod` decorator. The latter prevents the instance being passed as a parameter when we call the method from that instance.


```python
help(x)
```

    Help on Widget in module __main__ object:
    
    class Widget(builtins.object)
     |  This is a Widget class.
     |  
     |  Methods defined here:
     |  
     |  print_my_class(self)
     |      Print the instance class.
     |  
     |  ----------------------------------------------------------------------
     |  Static methods defined here:
     |  
     |  print_class()
     |      Print the class class.
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors defined here:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
    



```python
x.print_my_class()
```

    <class '__main__.Widget'>



```python
x.print_class()
```

    <class '__main__.Widget'>



```python
Widget.print_class()
```

    <class '__main__.Widget'>



```python
Widget.print_my_class()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-129-70eb78ad9fde> in <module>()
    ----> 1 Widget.print_my_class()
    

    TypeError: print_my_class() missing 1 required positional argument: 'self'


Note that if we had:

```python
class Foo():
     def s1():
         print('s1')

     @staticmethod
     def s2():
         print('s2')
```

then we could call `Foo.s1()` or `Foo.s2()` with no issues, but if `foo` was an instance of `Foo`, while we could call `foo.s2()` without a problem, if we called `foo.s1()` we would get an error:

```
TypeError: s1() takes 0 positional arguments but 1 was given
```

because Python would try to pass the instance as a parameter as it is missing @staticdecorator.

We can get the docstring of the class and more with `help`:


```python
help(Widget)
```

    Help on class Widget in module __main__:
    
    class Widget(builtins.object)
     |  This is a Widget class.
     |  
     |  Methods defined here:
     |  
     |  print_my_class(self)
     |      Print the instance class.
     |  
     |  ----------------------------------------------------------------------
     |  Static methods defined here:
     |  
     |  print_class()
     |      Print the class class.
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors defined here:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
    


### Constructors and visibility

A class does not require a constructor, but can have (at most) one. The constructor is an instance method named `__init__`. It can take additional parameters other than `self`.

Python does not support private or protected members. By convention, private members should be named starting with an underscore, but this is an 'honor system'; everything is public. Also by convention, you should avoid double underscores; that should be reerved for dunder-methods.


```python
class Bug:
    """ A class for creepy crawly things. """
    
    heads = 1  # This is a class variable
    
    def __init__(self, legs=6, name='bug'):
        self.legs = legs  # Any variable assigned to with self.var = ... in constructor is an instance variable
        self.name = name
    
    @staticmethod
    def _article(name):  # 'private' class method
        """ Return the English article for the given name. """
        return 'an'if 'aeiouAEIOU'.find(name[0]) >= 0 else 'a'

    def article(self):  # 'public' instance method
        """ Return the English article for the given name. """
        return Bug._article(self.name)
    
    def __repr__(self):  # __repr__ is called to get a printable representation of an object
        return f"I'm {Bug._article(self.name)} {self.name} with {self.legs} legs"

# Notice how help() will show help for article() but not _article().
# It respects the '_' convention for 'privacy'.
help(Bug)
```

    Help on class Bug in module __main__:
    
    class Bug(builtins.object)
     |  A class for creepy crawly things.
     |  
     |  Methods defined here:
     |  
     |  __init__(self, legs=6, name='bug')
     |      Initialize self.  See help(type(self)) for accurate signature.
     |  
     |  __repr__(self)
     |      Return repr(self).
     |  
     |  article(self)
     |      Return the English article for the given name.
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors defined here:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes defined here:
     |  
     |  heads = 1
    



```python
Bug()
```




    I'm a bug with 6 legs




```python
Bug(legs=8)
```




    I'm a bug with 8 legs



It is recommended to always define a `__repr__` method on your classes.

### Inheritance

Python supports both single and multiple inheritance (which we won't discuss). To up-call to a base method with single-inheritance we use `super()`:


```python
class Insect(Bug):
    
    def __init__(self):
        super().__init__(name='insect')
        
Insect()
```




    I'm an insect with 6 legs




```python
class Spider(Bug):
    
    def __init__(self):
        super().__init__(legs=8, name='spider')
        
Spider()
```




    I'm a spider with 8 legs



### Under the Hood

You can skip this section if you're not interested, but it can be useful to have some understanding of how classes work in Python.

Classes and class instances both have a `.__dict__` attribute that holds their methods and variables/attributes. For example:


```python
class Example:
    """ this is a class docopt string. """
    
    class_var = 'this is a class variable'
    
    def __init__(self):
        """ This is an instance docopt string. """
        self.instance_var = 'this is an instance var'
        
    def class_method():
        """ This is a class method docopt string. """
        pass
    
    def instance_method(self):
        return self.instance_var
    
Example.__dict__
```




    mappingproxy({'__dict__': <attribute '__dict__' of 'Example' objects>,
                  '__doc__': ' this is a class docopt string. ',
                  '__init__': <function __main__.Example.__init__>,
                  '__module__': '__main__',
                  '__weakref__': <attribute '__weakref__' of 'Example' objects>,
                  'class_method': <function __main__.Example.class_method>,
                  'class_var': 'this is a class variable',
                  'instance_method': <function __main__.Example.instance_method>})



In the case of classes we really have a special object, a `mappingproxy`; this is a wrapper around a dictionary that makes it read-only and enforces that all keys are strings.


```python
# Similarly for an instance, although this really is a dict, not a mappingproxy.
e = Example()
print(e.__dict__)
print(e.__dict__.__class__)
```

    {'instance_var': 'this is an instance var'}
    <class 'dict'>



```python
# Instances have a .__class__ attribute that points to their class.
e.__class__
```




    __main__.Example




```python
# To change a class variable, qualify with the class name:

e2 = Example()
print(e.class_var)
print(e2.class_var)

Example.class_var = 'Changed class var'

# Note how it is changed for all instances
print(e.class_var)
print(e2.class_var)
```

    this is a class variable
    this is a class variable
    Changed class var
    Changed class var



```python
# If you qualify with an instance instead, you'll end up creating an instance variable instead!
e2.class_var = 'e2 class var is actually an instance var'
print(e.class_var)
print(e2.class_var)
print(e.__dict__)
print(e2.__dict__)
```

    Changed class var
    e2 class var is actually an instance var
    {'instance_var': 'this is an instance var'}
    {'instance_var': 'this is an instance var', 'class_var': 'e2 class var is actually an instance var'}



```python
# When we dereference an instance method, we get a *bound method*; the instance method bound to the instance:
e.instance_method
```




    <bound method Example.instance_method of <__main__.Example object at 0x10fe224e0>>




```python
# We can save a reference to the bound method and call it later and it will use the right instance

f = e.instance_method
e.instance_var = 'e\'s instance var'
f()
```




    "e's instance var"



There's a lot more to it than this, but this should give you some idea of how Python can support monkey-patching at run-time and other flexibility.

## Exceptions

You can raise an exception with the `raise` statememt. You can give an instance of any class that derives from the `BaseException` class. You can catch exceptions using `try: except:`. If you want to get a reference to the exception, use `catch..as..`:


```python
try:
    raise Exception('The dude minds, man!')
except Exception as x:  # Exception is the type of exception to catch, x is the variable to catch it with.
    print(x)
    
# You can catch different types of exceptions, and you can use 'raise' on its own in the exception handling
# block to rethrow the exception.

def average(seq):
    "Compute the average of an iterable. "
    try:
        result = sum(seq) / len(seq)
    except ZeroDivisionError as e:
        return None
    except Exception:
        raise
    return result

print(average([]))
print(average(['cat']))
```

    The dude minds, man!
    None



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-143-d2931b582ed8> in <module>()
         18 
         19 print(average([]))
    ---> 20 print(average(['cat']))
    

    <ipython-input-143-d2931b582ed8> in average(seq)
         10     "Compute the average of an iterable. "
         11     try:
    ---> 12         result = sum(seq) / len(seq)
         13     except ZeroDivisionError as e:
         14         return None


    TypeError: unsupported operand type(s) for +: 'int' and 'str'


## Comprehensions

Comprehensions are a powerful feature in Python, allowing lists, dictionaries and tuples to be constructed from iterative computations with minimal code. These are best illustrated by examples:


```python
# A list of all squares from 1 to 25
[x*x for x in range(1, 6)]
```




    [1, 4, 9, 16, 25]




```python
# A list of all squares from 1 to 1024 except those divisble by 5
[x*x for x in range(1, 33) if (x*x) % 5 != 0]
```




    [1,
     4,
     9,
     16,
     36,
     49,
     64,
     81,
     121,
     144,
     169,
     196,
     256,
     289,
     324,
     361,
     441,
     484,
     529,
     576,
     676,
     729,
     784,
     841,
     961,
     1024]




```python
# Comprehensions can be nested
t = [
    ['1', '2'],
    ['3', '4']
]

# Make a list of lists from t where we convert the strings to floats
[[float(y) for y in x] for x in t]
```




    [[1.0, 2.0], [3.0, 4.0]]




```python
# Dictionary comprehension
{ f'Square of {x}': x*x for x in range(1, 6)}
```




    {'Square of 1': 1,
     'Square of 2': 4,
     'Square of 3': 9,
     'Square of 4': 16,
     'Square of 5': 25}



## Iterators and Generators

A Python iterator is an object with a `__next__` method for sequential access, that raises a StopIteration when done.

A Python iterable is an object that defines a `__getitem__` method that can take sequential integer indices starting from 0 (so not necessarily random access) and raises an IndexError when done, or that has an `__iter__` method which returns an iterator.

See https://docs.python.org/3/tutorial/classes.html#iterators for more; here's an example from that link:


```python
class Reverse:
    """Iterator for looping over a sequence backwards."""
    def __init__(self, data):
        self.data = data
        self.index = len(data)

    def __iter__(self):
        return self

    def __next__(self):
        if self.index == 0:
            raise StopIteration
        self.index = self.index - 1
        return self.data[self.index]
    
for char in Reverse("spam"):
    print(char)
```

    m
    a
    p
    s


A generator is an easier way of creating an iterable, by simply writing a function that uses `yield` instead of `return`. For example, we can write a generator for Fibonacci numbers like this:


```python
def fibonacci():
    x = 1
    y = 0
    while True:
        lasty = y
        y += x
        x = lasty
        yield y
        
#f = fibonacci()
for i in fibonacci():
    print(i)
    if i > 100:
        break
```

    1
    1
    2
    3
    5
    8
    13
    21
    34
    55
    89
    144


It is worth noting that using these is very idiomatic to Python normally (see the *Fluent Python* book for example), but in the data science domain, this idiom is more commonly replaced by vectorising. This web-based book goes deep into this different way of thinking: http://www.labri.fr/perso/nrougier/from-python-to-numpy/

## async/await

Python runs as a single-threaded process. That means things like I/O can slow things down a lot. It is possible to use multiple threads - there are several libaries for that - but even with a single thread big improvements are possible with async code. The details are beyond the scope of the bootcamp, but more info is available here: https://docs.python.org/3/library/asyncio-task.html. Recent changes in Python have made this much more powerful, flexible and easy to use.

## Type Annotations

Python has some mechanisms for doing optional type annotations. These can improve execution speed and there are some packages that can enforce type checking at run-time. It's not a bad idea to start using these but they're out of scope of this bootcamp. 

See https://docs.python.org/3/library/typing.html and http://mypy-lang.org/ for more.


## Logging

See https://opensource.com/article/17/9/python-logging for detals on Python logging.

I recommend looking at Daiquiri, which biulds on top of the standard logging library and make things easy:

https://julien.danjou.info/blog/python-logging-easy-with-daiquiri


```python
import sys
!{sys.executable} -m pip install daiquiri
```

    Collecting daiquiri
      Downloading daiquiri-1.3.0-py2.py3-none-any.whl
    Installing collected packages: daiquiri
    Successfully installed daiquiri-1.3.0
    [33mYou are using pip version 9.0.1, however version 9.0.3 is available.
    You should consider upgrading via the 'pip install --upgrade pip' command.[0m



```python
import logging
import daiquiri

daiquiri.setup(level=logging.INFO)

logger = daiquiri.getLogger("bootcamp")
logger.info("It works and logs to stderr by default with color!")
```

    2018-04-12 19:58:17,065 [13060] INFO     bootcamp: It works and logs to stderr by default with color!


## Debugging in the Notebook

You can use the Python debugger pdf within a notebook. Just add this code at the point you want to break execution and enter the debugger:
    
```python
import pdb; pdb.set_trace()
```

Once you're in the debugger, use the command `h` for help to see the commands available.

## Converting between Lists/Dictionaries and JSON

Non-tabular data can be stored in dictionaries, which may be nested and contain lists. This is similar to JSON data on the web and in Javascript, and Python provides a `json` package for converting between these formats.


```python
import json

my_albums = [
    {
        'title': 'Tales of the Inexpressible',
        'artist': 'Shpongle',
        'year': 2001,
        'tracks': [
            { 'title': 'Dorset Perception', 'time': '8:12' },
            { 'title': 'Star Shpongled Banner', 'time': '8:23' },
            { 'title': 'A New Way to Say Hooray!', 'time': '8:32' },
            { 'title': 'Room 2ॐ', 'time': '5:05' },
            { 'title': 'My Head Feels Like a Frisbee', 'time': '8:52' },
            { 'title': 'Shpongleyes', 'time': '8:56' },
            { 'title': 'Once Upon the Sea of Blissful Awareness', 'time': '7:30' },
            { 'title': 'Around the World in a Tea Daze', 'time': '11:21' },
            { 'title': 'Flute Fruit', 'time': '2:09' },
        ],
    }
]

j = json.dumps(my_albums)  # Convert to JSON string
print(type(j))
j
```

    <class 'str'>





    '[{"title": "Tales of the Inexpressible", "artist": "Shpongle", "year": 2001, "tracks": [{"title": "Dorset Perception", "time": "8:12"}, {"title": "Star Shpongled Banner", "time": "8:23"}, {"title": "A New Way to Say Hooray!", "time": "8:32"}, {"title": "Room 2\\u0950", "time": "5:05"}, {"title": "My Head Feels Like a Frisbee", "time": "8:52"}, {"title": "Shpongleyes", "time": "8:56"}, {"title": "Once Upon the Sea of Blissful Awareness", "time": "7:30"}, {"title": "Around the World in a Tea Daze", "time": "11:21"}, {"title": "Flute Fruit", "time": "2:09"}]}]'




```python
p = json.loads(j)  # Convert from JSON string to Python object
print(type(p))
p
```

    <class 'list'>





    [{'artist': 'Shpongle',
      'title': 'Tales of the Inexpressible',
      'tracks': [{'time': '8:12', 'title': 'Dorset Perception'},
       {'time': '8:23', 'title': 'Star Shpongled Banner'},
       {'time': '8:32', 'title': 'A New Way to Say Hooray!'},
       {'time': '5:05', 'title': 'Room 2ॐ'},
       {'time': '8:52', 'title': 'My Head Feels Like a Frisbee'},
       {'time': '8:56', 'title': 'Shpongleyes'},
       {'time': '7:30', 'title': 'Once Upon the Sea of Blissful Awareness'},
       {'time': '11:21', 'title': 'Around the World in a Tea Daze'},
       {'time': '2:09', 'title': 'Flute Fruit'}],
      'year': 2001}]



## Dates and Times

It's worth briefly discussing Python's support for date and time operations as these are relevant to the exploratory data analysis we will be doing.

The standard library has two modules related to this area:

- `time`, which includes many low-level wrappers around platform C APIs. In particular, routines that convert between epoch time (from Jan 1, 1970) to the various time components found in a C `tm` struct. The most useful functions here are related to getting the system time zone and the `time.sleep()` function which pauses execution;
- `datetime` which provides a more high-level set of functions for dealing with dates, times, and time intervals; this is the module we will focus on here.

In addition to this, there are some good third-party libraries to be aware of, that, amongst other things, provide flexible date parsing operations from different formats. The most commonly used one, that extends the functionality of `datetime`, is `dateutil` (https://dateutil.readthedocs.io/en/stable/) but another that is growing in popularity is `arrow` (http://arrow.readthedocs.io/en/latest/) which provides a completely different approach with a very natural API. 

The `datetime` module (https://docs.python.org/3.6/library/datetime.html) defines five classes:

- `datetime`, combining a date and time
- `date`, a date only with no time component
- `time`, a time of day only, with no date component
- `timedelta`, an interval between two points in time
- `tzinfo`, a class that contains information about a time zone


## Cool Stuff

See https://github.com/tukkek/notablepython

Concise reference: https://github.com/mattharrison/Tiny-Python-3.6-Notebook

The Hitchhikers Guide to Python documents many best practices: http://docs.python-guide.org/en/latest/

Easily progress bars to outer loops (works in Jupyter and console): https://pypi.python.org/pypi/tqdm

For anyone who wants to get really serious about Python, Mark Lutz's and David Beazley's books are good but some are dated, but the best book on the language itself is IMO "Fluent Python" by Luciano Ramalho. There are also many excellent talks at http://pyvideo.org/. 

Blog aggregator for Python: http://planetpython.org/


If you're interested in what the underlying Python byte code looks like for a function or class you can use the `dis` module:


```python
import dis

dis.dis(Widget)
```

    Disassembly of print_class:
     11           0 LOAD_GLOBAL              0 (print)
                  2 LOAD_GLOBAL              1 (Widget)
                  4 CALL_FUNCTION            1
                  6 POP_TOP
                  8 LOAD_CONST               1 (None)
                 10 RETURN_VALUE
    
    Disassembly of print_my_class:
      6           0 LOAD_GLOBAL              0 (print)
                  2 LOAD_FAST                0 (self)
                  4 LOAD_ATTR                1 (__class__)
                  6 CALL_FUNCTION            1
                  8 POP_TOP
                 10 LOAD_CONST               1 (None)
                 12 RETURN_VALUE
    


## Going Deeper

### The sys module

`sys.modules` is a dictionary of the currently imported modules:


```python
import sys

sys.modules
```




    {'builtins': <module 'builtins' (built-in)>,
     'sys': <module 'sys' (built-in)>,
     '_frozen_importlib': <module 'importlib._bootstrap' (frozen)>,
     '_imp': <module '_imp' (built-in)>,
     '_warnings': <module '_warnings' (built-in)>,
     '_thread': <module '_thread' (built-in)>,
     '_weakref': <module '_weakref' (built-in)>,
     '_frozen_importlib_external': <module 'importlib._bootstrap_external' (frozen)>,
     '_io': <module 'io' (built-in)>,
     'marshal': <module 'marshal' (built-in)>,
     'posix': <module 'posix' (built-in)>,
     'zipimport': <module 'zipimport' (built-in)>,
     'encodings': <module 'encodings' from '/Users/gram/anaconda/lib/python3.6/encodings/__init__.py'>,
     'codecs': <module 'codecs' from '/Users/gram/anaconda/lib/python3.6/codecs.py'>,
     '_codecs': <module '_codecs' (built-in)>,
     'encodings.aliases': <module 'encodings.aliases' from '/Users/gram/anaconda/lib/python3.6/encodings/aliases.py'>,
     'encodings.utf_8': <module 'encodings.utf_8' from '/Users/gram/anaconda/lib/python3.6/encodings/utf_8.py'>,
     '_signal': <module '_signal' (built-in)>,
     '__main__': <module '__main__'>,
     'encodings.latin_1': <module 'encodings.latin_1' from '/Users/gram/anaconda/lib/python3.6/encodings/latin_1.py'>,
     'io': <module 'io' from '/Users/gram/anaconda/lib/python3.6/io.py'>,
     'abc': <module 'abc' from '/Users/gram/anaconda/lib/python3.6/abc.py'>,
     '_weakrefset': <module '_weakrefset' from '/Users/gram/anaconda/lib/python3.6/_weakrefset.py'>,
     '_bootlocale': <module '_bootlocale' from '/Users/gram/anaconda/lib/python3.6/_bootlocale.py'>,
     '_locale': <module '_locale' (built-in)>,
     'site': <module 'site' from '/Users/gram/anaconda/lib/python3.6/site.py'>,
     'os': <module 'os' from '/Users/gram/anaconda/lib/python3.6/os.py'>,
     'errno': <module 'errno' (built-in)>,
     'stat': <module 'stat' from '/Users/gram/anaconda/lib/python3.6/stat.py'>,
     '_stat': <module '_stat' (built-in)>,
     'posixpath': <module 'posixpath' from '/Users/gram/anaconda/lib/python3.6/posixpath.py'>,
     'genericpath': <module 'genericpath' from '/Users/gram/anaconda/lib/python3.6/genericpath.py'>,
     'os.path': <module 'posixpath' from '/Users/gram/anaconda/lib/python3.6/posixpath.py'>,
     '_collections_abc': <module '_collections_abc' from '/Users/gram/anaconda/lib/python3.6/_collections_abc.py'>,
     '_sitebuiltins': <module '_sitebuiltins' from '/Users/gram/anaconda/lib/python3.6/_sitebuiltins.py'>,
     'sysconfig': <module 'sysconfig' from '/Users/gram/anaconda/lib/python3.6/sysconfig.py'>,
     '_sysconfigdata_m_darwin_darwin': <module '_sysconfigdata_m_darwin_darwin' from '/Users/gram/anaconda/lib/python3.6/_sysconfigdata_m_darwin_darwin.py'>,
     '_osx_support': <module '_osx_support' from '/Users/gram/anaconda/lib/python3.6/_osx_support.py'>,
     're': <module 're' from '/Users/gram/anaconda/lib/python3.6/re.py'>,
     'enum': <module 'enum' from '/Users/gram/anaconda/lib/python3.6/enum.py'>,
     'types': <module 'types' from '/Users/gram/anaconda/lib/python3.6/types.py'>,
     'functools': <module 'functools' from '/Users/gram/anaconda/lib/python3.6/functools.py'>,
     '_functools': <module '_functools' (built-in)>,
     'collections': <module 'collections' from '/Users/gram/anaconda/lib/python3.6/collections/__init__.py'>,
     'operator': <module 'operator' from '/Users/gram/anaconda/lib/python3.6/operator.py'>,
     '_operator': <module '_operator' (built-in)>,
     'keyword': <module 'keyword' from '/Users/gram/anaconda/lib/python3.6/keyword.py'>,
     'heapq': <module 'heapq' from '/Users/gram/anaconda/lib/python3.6/heapq.py'>,
     '_heapq': <module '_heapq' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_heapq.cpython-36m-darwin.so'>,
     'itertools': <module 'itertools' (built-in)>,
     'reprlib': <module 'reprlib' from '/Users/gram/anaconda/lib/python3.6/reprlib.py'>,
     '_collections': <module '_collections' (built-in)>,
     'weakref': <module 'weakref' from '/Users/gram/anaconda/lib/python3.6/weakref.py'>,
     'collections.abc': <module 'collections.abc' from '/Users/gram/anaconda/lib/python3.6/collections/abc.py'>,
     'sre_compile': <module 'sre_compile' from '/Users/gram/anaconda/lib/python3.6/sre_compile.py'>,
     '_sre': <module '_sre' (built-in)>,
     'sre_parse': <module 'sre_parse' from '/Users/gram/anaconda/lib/python3.6/sre_parse.py'>,
     'sre_constants': <module 'sre_constants' from '/Users/gram/anaconda/lib/python3.6/sre_constants.py'>,
     'copyreg': <module 'copyreg' from '/Users/gram/anaconda/lib/python3.6/copyreg.py'>,
     'importlib': <module 'importlib' from '/Users/gram/anaconda/lib/python3.6/importlib/__init__.py'>,
     'importlib._bootstrap': <module 'importlib._bootstrap' (frozen)>,
     'importlib._bootstrap_external': <module 'importlib._bootstrap_external' (frozen)>,
     'warnings': <module 'warnings' from '/Users/gram/anaconda/lib/python3.6/warnings.py'>,
     'importlib.util': <module 'importlib.util' from '/Users/gram/anaconda/lib/python3.6/importlib/util.py'>,
     'importlib.abc': <module 'importlib.abc' from '/Users/gram/anaconda/lib/python3.6/importlib/abc.py'>,
     'importlib.machinery': <module 'importlib.machinery' from '/Users/gram/anaconda/lib/python3.6/importlib/machinery.py'>,
     'contextlib': <module 'contextlib' from '/Users/gram/anaconda/lib/python3.6/contextlib.py'>,
     'mpl_toolkits': <module 'mpl_toolkits' (namespace)>,
     'sphinxcontrib': <module 'sphinxcontrib' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinxcontrib/__init__.py'>,
     'zope': <module 'zope' from '/Users/gram/anaconda/lib/python3.6/site-packages/zope/__init__.py'>,
     'runpy': <module 'runpy' from '/Users/gram/anaconda/lib/python3.6/runpy.py'>,
     'pkgutil': <module 'pkgutil' from '/Users/gram/anaconda/lib/python3.6/pkgutil.py'>,
     'ipykernel': <module 'ipykernel' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/__init__.py'>,
     'ipykernel._version': <module 'ipykernel._version' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/_version.py'>,
     'ipykernel.connect': <module 'ipykernel.connect' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/connect.py'>,
     '__future__': <module '__future__' from '/Users/gram/anaconda/lib/python3.6/__future__.py'>,
     'json': <module 'json' from '/Users/gram/anaconda/lib/python3.6/json/__init__.py'>,
     'json.decoder': <module 'json.decoder' from '/Users/gram/anaconda/lib/python3.6/json/decoder.py'>,
     'json.scanner': <module 'json.scanner' from '/Users/gram/anaconda/lib/python3.6/json/scanner.py'>,
     '_json': <module '_json' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_json.cpython-36m-darwin.so'>,
     'json.encoder': <module 'json.encoder' from '/Users/gram/anaconda/lib/python3.6/json/encoder.py'>,
     'subprocess': <module 'subprocess' from '/Users/gram/anaconda/lib/python3.6/subprocess.py'>,
     'time': <module 'time' (built-in)>,
     'signal': <module 'signal' from '/Users/gram/anaconda/lib/python3.6/signal.py'>,
     '_posixsubprocess': <module '_posixsubprocess' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_posixsubprocess.cpython-36m-darwin.so'>,
     'select': <module 'select' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/select.cpython-36m-darwin.so'>,
     'selectors': <module 'selectors' from '/Users/gram/anaconda/lib/python3.6/selectors.py'>,
     'math': <module 'math' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/math.cpython-36m-darwin.so'>,
     'threading': <module 'threading' from '/Users/gram/anaconda/lib/python3.6/threading.py'>,
     'traceback': <module 'traceback' from '/Users/gram/anaconda/lib/python3.6/traceback.py'>,
     'linecache': <module 'linecache' from '/Users/gram/anaconda/lib/python3.6/linecache.py'>,
     'tokenize': <module 'tokenize' from '/Users/gram/anaconda/lib/python3.6/tokenize.py'>,
     'token': <module 'token' from '/Users/gram/anaconda/lib/python3.6/token.py'>,
     'IPython': <module 'IPython' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/__init__.py'>,
     'IPython.core': <module 'IPython.core' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/__init__.py'>,
     'IPython.core.getipython': <module 'IPython.core.getipython' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/getipython.py'>,
     'IPython.core.release': <module 'IPython.core.release' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/release.py'>,
     'IPython.core.application': <module 'IPython.core.application' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/application.py'>,
     'atexit': <module 'atexit' (built-in)>,
     'copy': <module 'copy' from '/Users/gram/anaconda/lib/python3.6/copy.py'>,
     'glob': <module 'glob' from '/Users/gram/anaconda/lib/python3.6/glob.py'>,
     'fnmatch': <module 'fnmatch' from '/Users/gram/anaconda/lib/python3.6/fnmatch.py'>,
     'logging': <module 'logging' from '/Users/gram/anaconda/lib/python3.6/logging/__init__.py'>,
     'string': <module 'string' from '/Users/gram/anaconda/lib/python3.6/string.py'>,
     '_string': <module '_string' (built-in)>,
     'shutil': <module 'shutil' from '/Users/gram/anaconda/lib/python3.6/shutil.py'>,
     'zlib': <module 'zlib' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/zlib.cpython-36m-darwin.so'>,
     'bz2': <module 'bz2' from '/Users/gram/anaconda/lib/python3.6/bz2.py'>,
     '_compression': <module '_compression' from '/Users/gram/anaconda/lib/python3.6/_compression.py'>,
     '_bz2': <module '_bz2' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_bz2.cpython-36m-darwin.so'>,
     'lzma': <module 'lzma' from '/Users/gram/anaconda/lib/python3.6/lzma.py'>,
     '_lzma': <module '_lzma' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_lzma.cpython-36m-darwin.so'>,
     'pwd': <module 'pwd' (built-in)>,
     'grp': <module 'grp' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/grp.cpython-36m-darwin.so'>,
     'traitlets': <module 'traitlets' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/__init__.py'>,
     'traitlets.traitlets': <module 'traitlets.traitlets' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/traitlets.py'>,
     'inspect': <module 'inspect' from '/Users/gram/anaconda/lib/python3.6/inspect.py'>,
     'ast': <module 'ast' from '/Users/gram/anaconda/lib/python3.6/ast.py'>,
     '_ast': <module '_ast' (built-in)>,
     'dis': <module 'dis' from '/Users/gram/anaconda/lib/python3.6/dis.py'>,
     'opcode': <module 'opcode' from '/Users/gram/anaconda/lib/python3.6/opcode.py'>,
     '_opcode': <module '_opcode' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_opcode.cpython-36m-darwin.so'>,
     'six': <module 'six' from '/Users/gram/anaconda/lib/python3.6/site-packages/six.py'>,
     'struct': <module 'struct' from '/Users/gram/anaconda/lib/python3.6/struct.py'>,
     '_struct': <module '_struct' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_struct.cpython-36m-darwin.so'>,
     'traitlets.utils': <module 'traitlets.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/utils/__init__.py'>,
     'traitlets.utils.getargspec': <module 'traitlets.utils.getargspec' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/utils/getargspec.py'>,
     'traitlets.utils.importstring': <module 'traitlets.utils.importstring' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/utils/importstring.py'>,
     'ipython_genutils': <module 'ipython_genutils' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipython_genutils/__init__.py'>,
     'ipython_genutils._version': <module 'ipython_genutils._version' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipython_genutils/_version.py'>,
     'ipython_genutils.py3compat': <module 'ipython_genutils.py3compat' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipython_genutils/py3compat.py'>,
     'ipython_genutils.encoding': <module 'ipython_genutils.encoding' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipython_genutils/encoding.py'>,
     'locale': <module 'locale' from '/Users/gram/anaconda/lib/python3.6/locale.py'>,
     'platform': <module 'platform' from '/Users/gram/anaconda/lib/python3.6/platform.py'>,
     'traitlets.utils.sentinel': <module 'traitlets.utils.sentinel' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/utils/sentinel.py'>,
     'traitlets.utils.bunch': <module 'traitlets.utils.bunch' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/utils/bunch.py'>,
     'traitlets._version': <module 'traitlets._version' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/_version.py'>,
     'traitlets.config': <module 'traitlets.config' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/config/__init__.py'>,
     'traitlets.config.application': <module 'traitlets.config.application' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/config/application.py'>,
     'decorator': <module 'decorator' from '/Users/gram/anaconda/lib/python3.6/site-packages/decorator.py'>,
     'traitlets.config.configurable': <module 'traitlets.config.configurable' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/config/configurable.py'>,
     'traitlets.config.loader': <module 'traitlets.config.loader' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/config/loader.py'>,
     'argparse': <module 'argparse' from '/Users/gram/anaconda/lib/python3.6/argparse.py'>,
     'textwrap': <module 'textwrap' from '/Users/gram/anaconda/lib/python3.6/textwrap.py'>,
     'gettext': <module 'gettext' from '/Users/gram/anaconda/lib/python3.6/gettext.py'>,
     'ipython_genutils.path': <module 'ipython_genutils.path' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipython_genutils/path.py'>,
     'random': <module 'random' from '/Users/gram/anaconda/lib/python3.6/random.py'>,
     'hashlib': <module 'hashlib' from '/Users/gram/anaconda/lib/python3.6/hashlib.py'>,
     '_hashlib': <module '_hashlib' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_hashlib.cpython-36m-darwin.so'>,
     '_blake2': <module '_blake2' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_blake2.cpython-36m-darwin.so'>,
     '_sha3': <module '_sha3' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_sha3.cpython-36m-darwin.so'>,
     'bisect': <module 'bisect' from '/Users/gram/anaconda/lib/python3.6/bisect.py'>,
     '_bisect': <module '_bisect' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_bisect.cpython-36m-darwin.so'>,
     '_random': <module '_random' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_random.cpython-36m-darwin.so'>,
     'ipython_genutils.text': <module 'ipython_genutils.text' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipython_genutils/text.py'>,
     'ipython_genutils.importstring': <module 'ipython_genutils.importstring' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipython_genutils/importstring.py'>,
     'IPython.core.crashhandler': <module 'IPython.core.crashhandler' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/crashhandler.py'>,
     'pprint': <module 'pprint' from '/Users/gram/anaconda/lib/python3.6/pprint.py'>,
     'IPython.core.ultratb': <module 'IPython.core.ultratb' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/ultratb.py'>,
     'pydoc': <module 'pydoc' from '/Users/gram/anaconda/lib/python3.6/pydoc.py'>,
     'urllib': <module 'urllib' from '/Users/gram/anaconda/lib/python3.6/urllib/__init__.py'>,
     'urllib.parse': <module 'urllib.parse' from '/Users/gram/anaconda/lib/python3.6/urllib/parse.py'>,
     'IPython.core.debugger': <module 'IPython.core.debugger' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/debugger.py'>,
     'bdb': <module 'bdb' from '/Users/gram/anaconda/lib/python3.6/bdb.py'>,
     'IPython.utils': <module 'IPython.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/__init__.py'>,
     'IPython.utils.PyColorize': <module 'IPython.utils.PyColorize' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/PyColorize.py'>,
     'IPython.utils.coloransi': <module 'IPython.utils.coloransi' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/coloransi.py'>,
     'IPython.utils.ipstruct': <module 'IPython.utils.ipstruct' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/ipstruct.py'>,
     'IPython.utils.colorable': <module 'IPython.utils.colorable' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/colorable.py'>,
     'pygments': <module 'pygments' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/__init__.py'>,
     'pygments.util': <module 'pygments.util' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/util.py'>,
     'IPython.utils.py3compat': <module 'IPython.utils.py3compat' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/py3compat.py'>,
     'IPython.utils.encoding': <module 'IPython.utils.encoding' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/encoding.py'>,
     'IPython.core.excolors': <module 'IPython.core.excolors' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/excolors.py'>,
     'IPython.testing': <module 'IPython.testing' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/testing/__init__.py'>,
     'IPython.testing.skipdoctest': <module 'IPython.testing.skipdoctest' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/testing/skipdoctest.py'>,
     'pdb': <module 'pdb' from '/Users/gram/anaconda/lib/python3.6/pdb.py'>,
     'cmd': <module 'cmd' from '/Users/gram/anaconda/lib/python3.6/cmd.py'>,
     'code': <module 'code' from '/Users/gram/anaconda/lib/python3.6/code.py'>,
     'codeop': <module 'codeop' from '/Users/gram/anaconda/lib/python3.6/codeop.py'>,
     'IPython.core.display_trap': <module 'IPython.core.display_trap' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/display_trap.py'>,
     'IPython.utils.openpy': <module 'IPython.utils.openpy' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/openpy.py'>,
     'IPython.utils.path': <module 'IPython.utils.path' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/path.py'>,
     'IPython.utils.process': <module 'IPython.utils.process' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/process.py'>,
     'IPython.utils._process_posix': <module 'IPython.utils._process_posix' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/_process_posix.py'>,
     'pexpect': <module 'pexpect' from '/Users/gram/anaconda/lib/python3.6/site-packages/pexpect/__init__.py'>,
     'pexpect.exceptions': <module 'pexpect.exceptions' from '/Users/gram/anaconda/lib/python3.6/site-packages/pexpect/exceptions.py'>,
     'pexpect.utils': <module 'pexpect.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/pexpect/utils.py'>,
     'pexpect.expect': <module 'pexpect.expect' from '/Users/gram/anaconda/lib/python3.6/site-packages/pexpect/expect.py'>,
     'pexpect.pty_spawn': <module 'pexpect.pty_spawn' from '/Users/gram/anaconda/lib/python3.6/site-packages/pexpect/pty_spawn.py'>,
     'pty': <module 'pty' from '/Users/gram/anaconda/lib/python3.6/pty.py'>,
     'tty': <module 'tty' from '/Users/gram/anaconda/lib/python3.6/tty.py'>,
     'termios': <module 'termios' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/termios.cpython-36m-darwin.so'>,
     'ptyprocess': <module 'ptyprocess' from '/Users/gram/anaconda/lib/python3.6/site-packages/ptyprocess/__init__.py'>,
     'ptyprocess.ptyprocess': <module 'ptyprocess.ptyprocess' from '/Users/gram/anaconda/lib/python3.6/site-packages/ptyprocess/ptyprocess.py'>,
     'fcntl': <module 'fcntl' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/fcntl.cpython-36m-darwin.so'>,
     'resource': <module 'resource' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/resource.cpython-36m-darwin.so'>,
     'ptyprocess.util': <module 'ptyprocess.util' from '/Users/gram/anaconda/lib/python3.6/site-packages/ptyprocess/util.py'>,
     'pexpect.spawnbase': <module 'pexpect.spawnbase' from '/Users/gram/anaconda/lib/python3.6/site-packages/pexpect/spawnbase.py'>,
     'pexpect.run': <module 'pexpect.run' from '/Users/gram/anaconda/lib/python3.6/site-packages/pexpect/run.py'>,
     'IPython.utils._process_common': <module 'IPython.utils._process_common' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/_process_common.py'>,
     'shlex': <module 'shlex' from '/Users/gram/anaconda/lib/python3.6/shlex.py'>,
     'IPython.utils.decorators': <module 'IPython.utils.decorators' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/decorators.py'>,
     'IPython.utils.data': <module 'IPython.utils.data' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/data.py'>,
     'IPython.utils.terminal': <module 'IPython.utils.terminal' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/terminal.py'>,
     'IPython.utils.sysinfo': <module 'IPython.utils.sysinfo' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/sysinfo.py'>,
     'IPython.utils._sysinfo': <module 'IPython.utils._sysinfo' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/_sysinfo.py'>,
     'IPython.core.profiledir': <module 'IPython.core.profiledir' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/profiledir.py'>,
     'IPython.paths': <module 'IPython.paths' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/paths.py'>,
     'tempfile': <module 'tempfile' from '/Users/gram/anaconda/lib/python3.6/tempfile.py'>,
     'IPython.utils.importstring': <module 'IPython.utils.importstring' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/importstring.py'>,
     'IPython.terminal': <module 'IPython.terminal' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/terminal/__init__.py'>,
     'IPython.terminal.embed': <module 'IPython.terminal.embed' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/terminal/embed.py'>,
     'IPython.core.compilerop': <module 'IPython.core.compilerop' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/compilerop.py'>,
     'IPython.core.magic_arguments': <module 'IPython.core.magic_arguments' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magic_arguments.py'>,
     'IPython.core.error': <module 'IPython.core.error' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/error.py'>,
     'IPython.utils.text': <module 'IPython.utils.text' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/text.py'>,
     'pathlib': <module 'pathlib' from '/Users/gram/anaconda/lib/python3.6/pathlib.py'>,
     'ntpath': <module 'ntpath' from '/Users/gram/anaconda/lib/python3.6/ntpath.py'>,
     'IPython.core.magic': <module 'IPython.core.magic' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magic.py'>,
     'getopt': <module 'getopt' from '/Users/gram/anaconda/lib/python3.6/getopt.py'>,
     'IPython.core.oinspect': <module 'IPython.core.oinspect' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/oinspect.py'>,
     'IPython.core.page': <module 'IPython.core.page' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/page.py'>,
     'IPython.core.display': <module 'IPython.core.display' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/display.py'>,
     'binascii': <module 'binascii' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/binascii.cpython-36m-darwin.so'>,
     'mimetypes': <module 'mimetypes' from '/Users/gram/anaconda/lib/python3.6/mimetypes.py'>,
     'IPython.lib': <module 'IPython.lib' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/lib/__init__.py'>,
     'IPython.lib.security': <module 'IPython.lib.security' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/lib/security.py'>,
     'getpass': <module 'getpass' from '/Users/gram/anaconda/lib/python3.6/getpass.py'>,
     'IPython.lib.pretty': <module 'IPython.lib.pretty' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/lib/pretty.py'>,
     'datetime': <module 'datetime' from '/Users/gram/anaconda/lib/python3.6/datetime.py'>,
     '_datetime': <module '_datetime' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_datetime.cpython-36m-darwin.so'>,
     'IPython.utils.dir2': <module 'IPython.utils.dir2' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/dir2.py'>,
     'IPython.utils.wildcard': <module 'IPython.utils.wildcard' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/wildcard.py'>,
     'pygments.lexers': <module 'pygments.lexers' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/lexers/__init__.py'>,
     'pygments.lexers._mapping': <module 'pygments.lexers._mapping' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/lexers/_mapping.py'>,
     'pygments.modeline': <module 'pygments.modeline' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/modeline.py'>,
     'pygments.plugin': <module 'pygments.plugin' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/plugin.py'>,
     'pygments.lexers.python': <module 'pygments.lexers.python' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/lexers/python.py'>,
     'pygments.lexer': <module 'pygments.lexer' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/lexer.py'>,
     'pygments.filter': <module 'pygments.filter' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/filter.py'>,
     'pygments.filters': <module 'pygments.filters' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/filters/__init__.py'>,
     'pygments.token': <module 'pygments.token' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/token.py'>,
     'pygments.regexopt': <module 'pygments.regexopt' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/regexopt.py'>,
     'pygments.unistring': <module 'pygments.unistring' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/unistring.py'>,
     'pygments.formatters': <module 'pygments.formatters' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/formatters/__init__.py'>,
     'pygments.formatters._mapping': <module 'pygments.formatters._mapping' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/formatters/_mapping.py'>,
     'pygments.formatters.html': <module 'pygments.formatters.html' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/formatters/html.py'>,
     'pygments.formatter': <module 'pygments.formatter' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/formatter.py'>,
     'pygments.styles': <module 'pygments.styles' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/styles/__init__.py'>,
     'IPython.core.inputsplitter': <module 'IPython.core.inputsplitter' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/inputsplitter.py'>,
     'IPython.core.inputtransformer': <module 'IPython.core.inputtransformer' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/inputtransformer.py'>,
     'IPython.core.splitinput': <module 'IPython.core.splitinput' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/splitinput.py'>,
     'IPython.utils.tokenize2': <module 'IPython.utils.tokenize2' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/tokenize2.py'>,
     'IPython.core.interactiveshell': <module 'IPython.core.interactiveshell' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/interactiveshell.py'>,
     'pickleshare': <module 'pickleshare' from '/Users/gram/anaconda/lib/python3.6/site-packages/pickleshare.py'>,
     'pickle': <module 'pickle' from '/Users/gram/anaconda/lib/python3.6/pickle.py'>,
     '_compat_pickle': <module '_compat_pickle' from '/Users/gram/anaconda/lib/python3.6/_compat_pickle.py'>,
     '_pickle': <module '_pickle' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_pickle.cpython-36m-darwin.so'>,
     'IPython.core.prefilter': <module 'IPython.core.prefilter' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/prefilter.py'>,
     'IPython.core.autocall': <module 'IPython.core.autocall' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/autocall.py'>,
     'IPython.core.macro': <module 'IPython.core.macro' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/macro.py'>,
     'IPython.core.alias': <module 'IPython.core.alias' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/alias.py'>,
     'IPython.core.builtin_trap': <module 'IPython.core.builtin_trap' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/builtin_trap.py'>,
     'IPython.core.events': <module 'IPython.core.events' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/events.py'>,
     'IPython.core.displayhook': <module 'IPython.core.displayhook' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/displayhook.py'>,
     'IPython.core.displaypub': <module 'IPython.core.displaypub' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/displaypub.py'>,
     'IPython.core.extensions': <module 'IPython.core.extensions' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/extensions.py'>,
     'IPython.core.formatters': <module 'IPython.core.formatters' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/formatters.py'>,
     'IPython.utils.sentinel': <module 'IPython.utils.sentinel' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/sentinel.py'>,
     'IPython.core.history': <module 'IPython.core.history' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/history.py'>,
     'sqlite3': <module 'sqlite3' from '/Users/gram/anaconda/lib/python3.6/sqlite3/__init__.py'>,
     'sqlite3.dbapi2': <module 'sqlite3.dbapi2' from '/Users/gram/anaconda/lib/python3.6/sqlite3/dbapi2.py'>,
     '_sqlite3': <module '_sqlite3' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_sqlite3.cpython-36m-darwin.so'>,
     'IPython.core.logger': <module 'IPython.core.logger' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/logger.py'>,
     'IPython.core.payload': <module 'IPython.core.payload' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/payload.py'>,
     'IPython.core.usage': <module 'IPython.core.usage' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/usage.py'>,
     'IPython.display': <module 'IPython.display' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/display.py'>,
     'IPython.lib.display': <module 'IPython.lib.display' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/lib/display.py'>,
     'IPython.utils.io': <module 'IPython.utils.io' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/io.py'>,
     'IPython.utils.capture': <module 'IPython.utils.capture' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/capture.py'>,
     'IPython.utils.strdispatch': <module 'IPython.utils.strdispatch' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/strdispatch.py'>,
     'IPython.core.hooks': <module 'IPython.core.hooks' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/hooks.py'>,
     'IPython.utils.syspathcontext': <module 'IPython.utils.syspathcontext' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/syspathcontext.py'>,
     'IPython.utils.tempdir': <module 'IPython.utils.tempdir' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/tempdir.py'>,
     'typing': <module 'typing' from '/Users/gram/anaconda/lib/python3.6/typing.py'>,
     'typing.io': typing.io,
     'typing.re': typing.re,
     'IPython.utils.contexts': <module 'IPython.utils.contexts' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/contexts.py'>,
     'docrepr': <module 'docrepr' from '/Users/gram/anaconda/lib/python3.6/site-packages/docrepr/__init__.py'>,
     'docrepr._version': <module 'docrepr._version' from '/Users/gram/anaconda/lib/python3.6/site-packages/docrepr/_version.py'>,
     'docrepr.sphinxify': <module 'docrepr.sphinxify' from '/Users/gram/anaconda/lib/python3.6/site-packages/docrepr/sphinxify.py'>,
     'xml': <module 'xml' from '/Users/gram/anaconda/lib/python3.6/xml/__init__.py'>,
     'xml.sax': <module 'xml.sax' from '/Users/gram/anaconda/lib/python3.6/xml/sax/__init__.py'>,
     'xml.sax.xmlreader': <module 'xml.sax.xmlreader' from '/Users/gram/anaconda/lib/python3.6/xml/sax/xmlreader.py'>,
     'xml.sax.handler': <module 'xml.sax.handler' from '/Users/gram/anaconda/lib/python3.6/xml/sax/handler.py'>,
     'xml.sax._exceptions': <module 'xml.sax._exceptions' from '/Users/gram/anaconda/lib/python3.6/xml/sax/_exceptions.py'>,
     'xml.sax.saxutils': <module 'xml.sax.saxutils' from '/Users/gram/anaconda/lib/python3.6/xml/sax/saxutils.py'>,
     'urllib.request': <module 'urllib.request' from '/Users/gram/anaconda/lib/python3.6/urllib/request.py'>,
     'base64': <module 'base64' from '/Users/gram/anaconda/lib/python3.6/base64.py'>,
     'email': <module 'email' from '/Users/gram/anaconda/lib/python3.6/email/__init__.py'>,
     'http': <module 'http' from '/Users/gram/anaconda/lib/python3.6/http/__init__.py'>,
     'http.client': <module 'http.client' from '/Users/gram/anaconda/lib/python3.6/http/client.py'>,
     'email.parser': <module 'email.parser' from '/Users/gram/anaconda/lib/python3.6/email/parser.py'>,
     'email.feedparser': <module 'email.feedparser' from '/Users/gram/anaconda/lib/python3.6/email/feedparser.py'>,
     'email.errors': <module 'email.errors' from '/Users/gram/anaconda/lib/python3.6/email/errors.py'>,
     'email._policybase': <module 'email._policybase' from '/Users/gram/anaconda/lib/python3.6/email/_policybase.py'>,
     'email.header': <module 'email.header' from '/Users/gram/anaconda/lib/python3.6/email/header.py'>,
     'email.quoprimime': <module 'email.quoprimime' from '/Users/gram/anaconda/lib/python3.6/email/quoprimime.py'>,
     'email.base64mime': <module 'email.base64mime' from '/Users/gram/anaconda/lib/python3.6/email/base64mime.py'>,
     'email.charset': <module 'email.charset' from '/Users/gram/anaconda/lib/python3.6/email/charset.py'>,
     'email.encoders': <module 'email.encoders' from '/Users/gram/anaconda/lib/python3.6/email/encoders.py'>,
     'quopri': <module 'quopri' from '/Users/gram/anaconda/lib/python3.6/quopri.py'>,
     'email.utils': <module 'email.utils' from '/Users/gram/anaconda/lib/python3.6/email/utils.py'>,
     'socket': <module 'socket' from '/Users/gram/anaconda/lib/python3.6/socket.py'>,
     '_socket': <module '_socket' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_socket.cpython-36m-darwin.so'>,
     'email._parseaddr': <module 'email._parseaddr' from '/Users/gram/anaconda/lib/python3.6/email/_parseaddr.py'>,
     'calendar': <module 'calendar' from '/Users/gram/anaconda/lib/python3.6/calendar.py'>,
     'email.message': <module 'email.message' from '/Users/gram/anaconda/lib/python3.6/email/message.py'>,
     'uu': <module 'uu' from '/Users/gram/anaconda/lib/python3.6/uu.py'>,
     'email._encoded_words': <module 'email._encoded_words' from '/Users/gram/anaconda/lib/python3.6/email/_encoded_words.py'>,
     'email.iterators': <module 'email.iterators' from '/Users/gram/anaconda/lib/python3.6/email/iterators.py'>,
     'ssl': <module 'ssl' from '/Users/gram/anaconda/lib/python3.6/ssl.py'>,
     'ipaddress': <module 'ipaddress' from '/Users/gram/anaconda/lib/python3.6/ipaddress.py'>,
     '_ssl': <module '_ssl' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_ssl.cpython-36m-darwin.so'>,
     'urllib.error': <module 'urllib.error' from '/Users/gram/anaconda/lib/python3.6/urllib/error.py'>,
     'urllib.response': <module 'urllib.response' from '/Users/gram/anaconda/lib/python3.6/urllib/response.py'>,
     '_scproxy': <module '_scproxy' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_scproxy.cpython-36m-darwin.so'>,
     'docutils': <module 'docutils' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/__init__.py'>,
     'docutils.utils': <module 'docutils.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/utils/__init__.py'>,
     'unicodedata': <module 'unicodedata' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/unicodedata.cpython-36m-darwin.so'>,
     'docutils.nodes': <module 'docutils.nodes' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/nodes.py'>,
     'docutils.io': <module 'docutils.io' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/io.py'>,
     'docutils._compat': <module 'docutils._compat' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/_compat.py'>,
     'docutils.utils.error_reporting': <module 'docutils.utils.error_reporting' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/utils/error_reporting.py'>,
     'jinja2': <module 'jinja2' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/__init__.py'>,
     'jinja2.environment': <module 'jinja2.environment' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/environment.py'>,
     'jinja2.nodes': <module 'jinja2.nodes' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/nodes.py'>,
     'jinja2.utils': <module 'jinja2.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/utils.py'>,
     'jinja2._compat': <module 'jinja2._compat' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/_compat.py'>,
     'markupsafe': <module 'markupsafe' from '/Users/gram/anaconda/lib/python3.6/site-packages/markupsafe/__init__.py'>,
     'markupsafe._compat': <module 'markupsafe._compat' from '/Users/gram/anaconda/lib/python3.6/site-packages/markupsafe/_compat.py'>,
     'markupsafe._speedups': <module 'markupsafe._speedups' from '/Users/gram/anaconda/lib/python3.6/site-packages/markupsafe/_speedups.cpython-36m-darwin.so'>,
     'jinja2.defaults': <module 'jinja2.defaults' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/defaults.py'>,
     'jinja2.filters': <module 'jinja2.filters' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/filters.py'>,
     'jinja2.runtime': <module 'jinja2.runtime' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/runtime.py'>,
     'jinja2.exceptions': <module 'jinja2.exceptions' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/exceptions.py'>,
     'jinja2.tests': <module 'jinja2.tests' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/tests.py'>,
     'decimal': <module 'decimal' from '/Users/gram/anaconda/lib/python3.6/decimal.py'>,
     'numbers': <module 'numbers' from '/Users/gram/anaconda/lib/python3.6/numbers.py'>,
     '_decimal': <module '_decimal' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_decimal.cpython-36m-darwin.so'>,
     'jinja2.lexer': <module 'jinja2.lexer' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/lexer.py'>,
     'jinja2.parser': <module 'jinja2.parser' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/parser.py'>,
     'jinja2.compiler': <module 'jinja2.compiler' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/compiler.py'>,
     'jinja2.visitor': <module 'jinja2.visitor' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/visitor.py'>,
     'jinja2.optimizer': <module 'jinja2.optimizer' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/optimizer.py'>,
     'jinja2.idtracking': <module 'jinja2.idtracking' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/idtracking.py'>,
     'jinja2.loaders': <module 'jinja2.loaders' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/loaders.py'>,
     'jinja2.bccache': <module 'jinja2.bccache' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/bccache.py'>,
     'jinja2.asyncsupport': <module 'jinja2.asyncsupport' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/asyncsupport.py'>,
     'asyncio': <module 'asyncio' from '/Users/gram/anaconda/lib/python3.6/asyncio/__init__.py'>,
     'asyncio.base_events': <module 'asyncio.base_events' from '/Users/gram/anaconda/lib/python3.6/asyncio/base_events.py'>,
     'concurrent': <module 'concurrent' from '/Users/gram/anaconda/lib/python3.6/concurrent/__init__.py'>,
     'concurrent.futures': <module 'concurrent.futures' from '/Users/gram/anaconda/lib/python3.6/concurrent/futures/__init__.py'>,
     'concurrent.futures._base': <module 'concurrent.futures._base' from '/Users/gram/anaconda/lib/python3.6/concurrent/futures/_base.py'>,
     'concurrent.futures.process': <module 'concurrent.futures.process' from '/Users/gram/anaconda/lib/python3.6/concurrent/futures/process.py'>,
     'queue': <module 'queue' from '/Users/gram/anaconda/lib/python3.6/queue.py'>,
     'multiprocessing': <module 'multiprocessing' from '/Users/gram/anaconda/lib/python3.6/multiprocessing/__init__.py'>,
     'multiprocessing.context': <module 'multiprocessing.context' from '/Users/gram/anaconda/lib/python3.6/multiprocessing/context.py'>,
     'multiprocessing.process': <module 'multiprocessing.process' from '/Users/gram/anaconda/lib/python3.6/multiprocessing/process.py'>,
     'multiprocessing.reduction': <module 'multiprocessing.reduction' from '/Users/gram/anaconda/lib/python3.6/multiprocessing/reduction.py'>,
     'array': <module 'array' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/array.cpython-36m-darwin.so'>,
     '__mp_main__': <module 'ipykernel_launcher' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel_launcher.py'>,
     'multiprocessing.connection': <module 'multiprocessing.connection' from '/Users/gram/anaconda/lib/python3.6/multiprocessing/connection.py'>,
     '_multiprocessing': <module '_multiprocessing' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_multiprocessing.cpython-36m-darwin.so'>,
     'multiprocessing.util': <module 'multiprocessing.util' from '/Users/gram/anaconda/lib/python3.6/multiprocessing/util.py'>,
     'concurrent.futures.thread': <module 'concurrent.futures.thread' from '/Users/gram/anaconda/lib/python3.6/concurrent/futures/thread.py'>,
     'asyncio.compat': <module 'asyncio.compat' from '/Users/gram/anaconda/lib/python3.6/asyncio/compat.py'>,
     'asyncio.coroutines': <module 'asyncio.coroutines' from '/Users/gram/anaconda/lib/python3.6/asyncio/coroutines.py'>,
     'asyncio.events': <module 'asyncio.events' from '/Users/gram/anaconda/lib/python3.6/asyncio/events.py'>,
     'asyncio.base_futures': <module 'asyncio.base_futures' from '/Users/gram/anaconda/lib/python3.6/asyncio/base_futures.py'>,
     'asyncio.log': <module 'asyncio.log' from '/Users/gram/anaconda/lib/python3.6/asyncio/log.py'>,
     'asyncio.futures': <module 'asyncio.futures' from '/Users/gram/anaconda/lib/python3.6/asyncio/futures.py'>,
     'asyncio.base_tasks': <module 'asyncio.base_tasks' from '/Users/gram/anaconda/lib/python3.6/asyncio/base_tasks.py'>,
     '_asyncio': <module '_asyncio' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_asyncio.cpython-36m-darwin.so'>,
     'asyncio.tasks': <module 'asyncio.tasks' from '/Users/gram/anaconda/lib/python3.6/asyncio/tasks.py'>,
     'asyncio.locks': <module 'asyncio.locks' from '/Users/gram/anaconda/lib/python3.6/asyncio/locks.py'>,
     'asyncio.protocols': <module 'asyncio.protocols' from '/Users/gram/anaconda/lib/python3.6/asyncio/protocols.py'>,
     'asyncio.queues': <module 'asyncio.queues' from '/Users/gram/anaconda/lib/python3.6/asyncio/queues.py'>,
     'asyncio.streams': <module 'asyncio.streams' from '/Users/gram/anaconda/lib/python3.6/asyncio/streams.py'>,
     'asyncio.subprocess': <module 'asyncio.subprocess' from '/Users/gram/anaconda/lib/python3.6/asyncio/subprocess.py'>,
     'asyncio.transports': <module 'asyncio.transports' from '/Users/gram/anaconda/lib/python3.6/asyncio/transports.py'>,
     'asyncio.unix_events': <module 'asyncio.unix_events' from '/Users/gram/anaconda/lib/python3.6/asyncio/unix_events.py'>,
     'asyncio.base_subprocess': <module 'asyncio.base_subprocess' from '/Users/gram/anaconda/lib/python3.6/asyncio/base_subprocess.py'>,
     'asyncio.constants': <module 'asyncio.constants' from '/Users/gram/anaconda/lib/python3.6/asyncio/constants.py'>,
     'asyncio.selector_events': <module 'asyncio.selector_events' from '/Users/gram/anaconda/lib/python3.6/asyncio/selector_events.py'>,
     'asyncio.sslproto': <module 'asyncio.sslproto' from '/Users/gram/anaconda/lib/python3.6/asyncio/sslproto.py'>,
     'jinja2.asyncfilters': <module 'jinja2.asyncfilters' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/asyncfilters.py'>,
     'sphinx': <module 'sphinx' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/__init__.py'>,
     'sphinx.deprecation': <module 'sphinx.deprecation' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/deprecation.py'>,
     'sphinx.application': <module 'sphinx.application' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/application.py'>,
     'six.moves': <module 'six.moves' (<six._SixMetaPathImporter object at 0x10b8d1630>)>,
     'docutils.parsers': <module 'docutils.parsers' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/__init__.py'>,
     'docutils.parsers.rst': <module 'docutils.parsers.rst' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/__init__.py'>,
     'docutils.statemachine': <module 'docutils.statemachine' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/statemachine.py'>,
     'docutils.parsers.rst.states': <module 'docutils.parsers.rst.states' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/states.py'>,
     'docutils.parsers.rst.directives': <module 'docutils.parsers.rst.directives' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/directives/__init__.py'>,
     'docutils.parsers.rst.languages': <module 'docutils.parsers.rst.languages' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/languages/__init__.py'>,
     'docutils.parsers.rst.languages.en': <module 'docutils.parsers.rst.languages.en' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/languages/en.py'>,
     'docutils.parsers.rst.tableparser': <module 'docutils.parsers.rst.tableparser' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/tableparser.py'>,
     'docutils.parsers.rst.roles': <module 'docutils.parsers.rst.roles' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/roles.py'>,
     'docutils.utils.code_analyzer': <module 'docutils.utils.code_analyzer' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/utils/code_analyzer.py'>,
     'docutils.utils.punctuation_chars': <module 'docutils.utils.punctuation_chars' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/utils/punctuation_chars.py'>,
     'docutils.utils.roman': <module 'docutils.utils.roman' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/utils/roman.py'>,
     'docutils.utils.urischemes': <module 'docutils.utils.urischemes' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/utils/urischemes.py'>,
     'docutils.frontend': <module 'docutils.frontend' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/frontend.py'>,
     'configparser': <module 'configparser' from '/Users/gram/anaconda/lib/python3.6/configparser.py'>,
     'optparse': <module 'optparse' from '/Users/gram/anaconda/lib/python3.6/optparse.py'>,
     'docutils.transforms': <module 'docutils.transforms' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/transforms/__init__.py'>,
     'docutils.languages': <module 'docutils.languages' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/languages/__init__.py'>,
     'docutils.transforms.universal': <module 'docutils.transforms.universal' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/transforms/universal.py'>,
     'docutils.utils.smartquotes': <module 'docutils.utils.smartquotes' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/utils/smartquotes.py'>,
     'sphinx.locale': <module 'sphinx.locale' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/locale/__init__.py'>,
     'sphinx.config': <module 'sphinx.config' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/config.py'>,
     'sphinx.errors': <module 'sphinx.errors' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/errors.py'>,
     'sphinx.util': <module 'sphinx.util' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/__init__.py'>,
     'six.moves.urllib': <module 'six.moves.urllib' (<six._SixMetaPathImporter object at 0x10b8d1630>)>,
     'six.moves.urllib.parse': <module 'six.moves.urllib.parse' (<six._SixMetaPathImporter object at 0x10b8d1630>)>,
     'sphinx.util.logging': <module 'sphinx.util.logging' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/logging.py'>,
     'logging.handlers': <module 'logging.handlers' from '/Users/gram/anaconda/lib/python3.6/logging/handlers.py'>,
     'sphinx.util.console': <module 'sphinx.util.console' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/console.py'>,
     'colorama': <module 'colorama' from '/Users/gram/anaconda/lib/python3.6/site-packages/colorama/__init__.py'>,
     'colorama.initialise': <module 'colorama.initialise' from '/Users/gram/anaconda/lib/python3.6/site-packages/colorama/initialise.py'>,
     'colorama.ansitowin32': <module 'colorama.ansitowin32' from '/Users/gram/anaconda/lib/python3.6/site-packages/colorama/ansitowin32.py'>,
     'colorama.ansi': <module 'colorama.ansi' from '/Users/gram/anaconda/lib/python3.6/site-packages/colorama/ansi.py'>,
     'colorama.winterm': <module 'colorama.winterm' from '/Users/gram/anaconda/lib/python3.6/site-packages/colorama/winterm.py'>,
     'colorama.win32': <module 'colorama.win32' from '/Users/gram/anaconda/lib/python3.6/site-packages/colorama/win32.py'>,
     'ctypes': <module 'ctypes' from '/Users/gram/anaconda/lib/python3.6/ctypes/__init__.py'>,
     '_ctypes': <module '_ctypes' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_ctypes.cpython-36m-darwin.so'>,
     'ctypes._endian': <module 'ctypes._endian' from '/Users/gram/anaconda/lib/python3.6/ctypes/_endian.py'>,
     'sphinx.util.fileutil': <module 'sphinx.util.fileutil' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/fileutil.py'>,
     'sphinx.util.osutil': <module 'sphinx.util.osutil' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/osutil.py'>,
     'filecmp': <module 'filecmp' from '/Users/gram/anaconda/lib/python3.6/filecmp.py'>,
     'sphinx.util.smartypants': <module 'sphinx.util.smartypants' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/smartypants.py'>,
     'sphinx.util.docutils': <module 'sphinx.util.docutils' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/docutils.py'>,
     'distutils': <module 'distutils' from '/Users/gram/anaconda/lib/python3.6/distutils/__init__.py'>,
     'distutils.version': <module 'distutils.version' from '/Users/gram/anaconda/lib/python3.6/distutils/version.py'>,
     'sphinx.util.nodes': <module 'sphinx.util.nodes' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/nodes.py'>,
     'sphinx.addnodes': <module 'sphinx.addnodes' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/addnodes.py'>,
     'sphinx.util.matching': <module 'sphinx.util.matching' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/matching.py'>,
     'sphinx.util.i18n': <module 'sphinx.util.i18n' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/i18n.py'>,
     'babel': <module 'babel' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/__init__.py'>,
     'babel.core': <module 'babel.core' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/core.py'>,
     'babel.localedata': <module 'babel.localedata' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/localedata.py'>,
     'babel._compat': <module 'babel._compat' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/_compat.py'>,
     'babel.plural': <module 'babel.plural' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/plural.py'>,
     'babel.dates': <module 'babel.dates' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/dates.py'>,
     'pytz': <module 'pytz' from '/Users/gram/anaconda/lib/python3.6/site-packages/pytz/__init__.py'>,
     'pytz.exceptions': <module 'pytz.exceptions' from '/Users/gram/anaconda/lib/python3.6/site-packages/pytz/exceptions.py'>,
     'pytz.lazy': <module 'pytz.lazy' from '/Users/gram/anaconda/lib/python3.6/site-packages/pytz/lazy.py'>,
     'pytz.tzinfo': <module 'pytz.tzinfo' from '/Users/gram/anaconda/lib/python3.6/site-packages/pytz/tzinfo.py'>,
     'pytz.tzfile': <module 'pytz.tzfile' from '/Users/gram/anaconda/lib/python3.6/site-packages/pytz/tzfile.py'>,
     'babel.util': <module 'babel.util' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/util.py'>,
     'babel.localtime': <module 'babel.localtime' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/localtime/__init__.py'>,
     'babel.localtime._unix': <module 'babel.localtime._unix' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/localtime/_unix.py'>,
     'babel.messages': <module 'babel.messages' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/messages/__init__.py'>,
     'babel.messages.catalog': <module 'babel.messages.catalog' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/messages/catalog.py'>,
     'cgi': <module 'cgi' from '/Users/gram/anaconda/lib/python3.6/cgi.py'>,
     'html': <module 'html' from '/Users/gram/anaconda/lib/python3.6/html/__init__.py'>,
     'html.entities': <module 'html.entities' from '/Users/gram/anaconda/lib/python3.6/html/entities.py'>,
     'difflib': <module 'difflib' from '/Users/gram/anaconda/lib/python3.6/difflib.py'>,
     'babel.messages.plurals': <module 'babel.messages.plurals' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/messages/plurals.py'>,
     'babel.messages.pofile': <module 'babel.messages.pofile' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/messages/pofile.py'>,
     'babel.messages.mofile': <module 'babel.messages.mofile' from '/Users/gram/anaconda/lib/python3.6/site-packages/babel/messages/mofile.py'>,
     'sphinx.util.pycompat': <module 'sphinx.util.pycompat' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/pycompat.py'>,
     'sphinx.environment': <module 'sphinx.environment' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/environment/__init__.py'>,
     'docutils.core': <module 'docutils.core' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/core.py'>,
     'docutils.readers': <module 'docutils.readers' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/readers/__init__.py'>,
     'docutils.writers': <module 'docutils.writers' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/writers/__init__.py'>,
     'docutils.readers.doctree': <module 'docutils.readers.doctree' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/readers/doctree.py'>,
     'sphinx.io': <module 'sphinx.io' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/io.py'>,
     'docutils.readers.standalone': <module 'docutils.readers.standalone' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/readers/standalone.py'>,
     'docutils.transforms.frontmatter': <module 'docutils.transforms.frontmatter' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/transforms/frontmatter.py'>,
     'docutils.transforms.references': <module 'docutils.transforms.references' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/transforms/references.py'>,
     'docutils.transforms.misc': <module 'docutils.transforms.misc' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/transforms/misc.py'>,
     'sphinx.transforms': <module 'sphinx.transforms' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/transforms/__init__.py'>,
     'docutils.transforms.parts': <module 'docutils.transforms.parts' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/transforms/parts.py'>,
     'sphinx.transforms.compact_bullet_list': <module 'sphinx.transforms.compact_bullet_list' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/transforms/compact_bullet_list.py'>,
     'sphinx.transforms.i18n': <module 'sphinx.transforms.i18n' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/transforms/i18n.py'>,
     'sphinx.domains': <module 'sphinx.domains' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/domains/__init__.py'>,
     'sphinx.domains.std': <module 'sphinx.domains.std' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/domains/std.py'>,
     'sphinx.roles': <module 'sphinx.roles' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/roles.py'>,
     'sphinx.directives': <module 'sphinx.directives' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/directives/__init__.py'>,
     'sphinx.util.docfields': <module 'sphinx.util.docfields' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/docfields.py'>,
     'sphinx.directives.code': <module 'sphinx.directives.code' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/directives/code.py'>,
     'sphinx.directives.other': <module 'sphinx.directives.other' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/directives/other.py'>,
     'docutils.parsers.rst.directives.admonitions': <module 'docutils.parsers.rst.directives.admonitions' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/directives/admonitions.py'>,
     'docutils.parsers.rst.directives.misc': <module 'docutils.parsers.rst.directives.misc' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/directives/misc.py'>,
     'docutils.parsers.rst.directives.body': <module 'docutils.parsers.rst.directives.body' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/directives/body.py'>,
     'sphinx.directives.patches': <module 'sphinx.directives.patches' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/directives/patches.py'>,
     'docutils.parsers.rst.directives.images': <module 'docutils.parsers.rst.directives.images' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/directives/images.py'>,
     'PIL': <module 'PIL' from '/Users/gram/anaconda/lib/python3.6/site-packages/PIL/__init__.py'>,
     'PIL.version': <module 'PIL.version' from '/Users/gram/anaconda/lib/python3.6/site-packages/PIL/version.py'>,
     'PIL.Image': <module 'PIL.Image' from '/Users/gram/anaconda/lib/python3.6/site-packages/PIL/Image.py'>,
     'PIL._imaging': <module 'PIL._imaging' from '/Users/gram/anaconda/lib/python3.6/site-packages/PIL/_imaging.cpython-36m-darwin.so'>,
     'PIL.ImageMode': <module 'PIL.ImageMode' from '/Users/gram/anaconda/lib/python3.6/site-packages/PIL/ImageMode.py'>,
     'PIL._binary': <module 'PIL._binary' from '/Users/gram/anaconda/lib/python3.6/site-packages/PIL/_binary.py'>,
     'PIL._util': <module 'PIL._util' from '/Users/gram/anaconda/lib/python3.6/site-packages/PIL/_util.py'>,
     'cffi': <module 'cffi' from '/Users/gram/anaconda/lib/python3.6/site-packages/cffi/__init__.py'>,
     'cffi.api': <module 'cffi.api' from '/Users/gram/anaconda/lib/python3.6/site-packages/cffi/api.py'>,
     'cffi.lock': <module 'cffi.lock' from '/Users/gram/anaconda/lib/python3.6/site-packages/cffi/lock.py'>,
     'cffi.error': <module 'cffi.error' from '/Users/gram/anaconda/lib/python3.6/site-packages/cffi/error.py'>,
     'cffi.model': <module 'cffi.model' from '/Users/gram/anaconda/lib/python3.6/site-packages/cffi/model.py'>,
     'docutils.parsers.rst.directives.html': <module 'docutils.parsers.rst.directives.html' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/directives/html.py'>,
     'docutils.transforms.components': <module 'docutils.transforms.components' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/transforms/components.py'>,
     'docutils.parsers.rst.directives.tables': <module 'docutils.parsers.rst.directives.tables' from '/Users/gram/anaconda/lib/python3.6/site-packages/docutils/parsers/rst/directives/tables.py'>,
     'csv': <module 'csv' from '/Users/gram/anaconda/lib/python3.6/csv.py'>,
     '_csv': <module '_csv' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_csv.cpython-36m-darwin.so'>,
     'sphinx.util.parallel': <module 'sphinx.util.parallel' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/parallel.py'>,
     'sphinx.util.websupport': <module 'sphinx.util.websupport' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/websupport.py'>,
     'sphinxcontrib.websupport': <module 'sphinxcontrib.websupport' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinxcontrib/websupport/__init__.py'>,
     'pkg_resources': <module 'pkg_resources' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/__init__.py'>,
     'zipfile': <module 'zipfile' from '/Users/gram/anaconda/lib/python3.6/zipfile.py'>,
     'plistlib': <module 'plistlib' from '/Users/gram/anaconda/lib/python3.6/plistlib.py'>,
     'xml.parsers': <module 'xml.parsers' from '/Users/gram/anaconda/lib/python3.6/xml/parsers/__init__.py'>,
     'xml.parsers.expat': <module 'xml.parsers.expat' from '/Users/gram/anaconda/lib/python3.6/xml/parsers/expat.py'>,
     'pyexpat.errors': <module 'pyexpat.errors'>,
     'pyexpat.model': <module 'pyexpat.model'>,
     'pyexpat': <module 'pyexpat' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/pyexpat.cpython-36m-darwin.so'>,
     'xml.parsers.expat.model': <module 'pyexpat.model'>,
     'xml.parsers.expat.errors': <module 'pyexpat.errors'>,
     'pkg_resources.extern': <module 'pkg_resources.extern' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/extern/__init__.py'>,
     'pkg_resources._vendor': <module 'pkg_resources._vendor' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/__init__.py'>,
     'pkg_resources.extern.six': <module 'pkg_resources._vendor.six' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/six.py'>,
     'pkg_resources._vendor.six': <module 'pkg_resources._vendor.six' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/six.py'>,
     'pkg_resources.extern.six.moves': <module 'pkg_resources._vendor.six.moves' (<pkg_resources._vendor.six._SixMetaPathImporter object at 0x10cf14a90>)>,
     'pkg_resources._vendor.six.moves': <module 'pkg_resources._vendor.six.moves' (<pkg_resources._vendor.six._SixMetaPathImporter object at 0x10cf14a90>)>,
     'pkg_resources.py31compat': <module 'pkg_resources.py31compat' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/py31compat.py'>,
     'pkg_resources.extern.appdirs': <module 'pkg_resources._vendor.appdirs' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/appdirs.py'>,
     'pkg_resources._vendor.packaging.__about__': <module 'pkg_resources._vendor.packaging.__about__' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/packaging/__about__.py'>,
     'pkg_resources.extern.packaging': <module 'pkg_resources._vendor.packaging' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/packaging/__init__.py'>,
     'pkg_resources.extern.packaging.version': <module 'pkg_resources.extern.packaging.version' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/packaging/version.py'>,
     'pkg_resources.extern.packaging._structures': <module 'pkg_resources.extern.packaging._structures' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/packaging/_structures.py'>,
     'pkg_resources.extern.packaging.specifiers': <module 'pkg_resources.extern.packaging.specifiers' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/packaging/specifiers.py'>,
     'pkg_resources.extern.packaging._compat': <module 'pkg_resources.extern.packaging._compat' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/packaging/_compat.py'>,
     'pkg_resources.extern.packaging.requirements': <module 'pkg_resources.extern.packaging.requirements' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/packaging/requirements.py'>,
     'pkg_resources.extern.pyparsing': <module 'pkg_resources._vendor.pyparsing' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/pyparsing.py'>,
     'pkg_resources.extern.six.moves.urllib': <module 'pkg_resources._vendor.six.moves.urllib' (<pkg_resources._vendor.six._SixMetaPathImporter object at 0x10cf14a90>)>,
     'pkg_resources.extern.packaging.markers': <module 'pkg_resources.extern.packaging.markers' from '/Users/gram/anaconda/lib/python3.6/site-packages/pkg_resources/_vendor/packaging/markers.py'>,
     'sphinxcontrib.websupport.core': <module 'sphinxcontrib.websupport.core' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinxcontrib/websupport/core.py'>,
     'sphinx.util.jsonimpl': <module 'sphinx.util.jsonimpl' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/jsonimpl.py'>,
     'sphinxcontrib.websupport.errors': <module 'sphinxcontrib.websupport.errors' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinxcontrib/websupport/errors.py'>,
     'sphinxcontrib.websupport.search': <module 'sphinxcontrib.websupport.search' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinxcontrib/websupport/search/__init__.py'>,
     'sphinxcontrib.websupport.storage': <module 'sphinxcontrib.websupport.storage' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinxcontrib/websupport/storage/__init__.py'>,
     'sphinxcontrib.websupport.version': <module 'sphinxcontrib.websupport.version' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinxcontrib/websupport/version.py'>,
     'sphinxcontrib.websupport.utils': <module 'sphinxcontrib.websupport.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinxcontrib/websupport/utils.py'>,
     'sphinx.versioning': <module 'sphinx.versioning' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/versioning.py'>,
     'uuid': <module 'uuid' from '/Users/gram/anaconda/lib/python3.6/uuid.py'>,
     'ctypes.util': <module 'ctypes.util' from '/Users/gram/anaconda/lib/python3.6/ctypes/util.py'>,
     'ctypes.macholib': <module 'ctypes.macholib' from '/Users/gram/anaconda/lib/python3.6/ctypes/macholib/__init__.py'>,
     'ctypes.macholib.dyld': <module 'ctypes.macholib.dyld' from '/Users/gram/anaconda/lib/python3.6/ctypes/macholib/dyld.py'>,
     'ctypes.macholib.framework': <module 'ctypes.macholib.framework' from '/Users/gram/anaconda/lib/python3.6/ctypes/macholib/framework.py'>,
     'ctypes.macholib.dylib': <module 'ctypes.macholib.dylib' from '/Users/gram/anaconda/lib/python3.6/ctypes/macholib/dylib.py'>,
     'sphinx.environment.adapters': <module 'sphinx.environment.adapters' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/environment/adapters/__init__.py'>,
     'sphinx.environment.adapters.indexentries': <module 'sphinx.environment.adapters.indexentries' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/environment/adapters/indexentries.py'>,
     'sphinx.environment.adapters.toctree': <module 'sphinx.environment.adapters.toctree' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/environment/adapters/toctree.py'>,
     'sphinx.events': <module 'sphinx.events' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/events.py'>,
     'sphinx.extension': <module 'sphinx.extension' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/extension.py'>,
     'sphinx.registry': <module 'sphinx.registry' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/registry.py'>,
     'sphinx.util.tags': <module 'sphinx.util.tags' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/util/tags.py'>,
     'docrepr.utils': <module 'docrepr.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/docrepr/utils.py'>,
     'IPython.terminal.interactiveshell': <module 'IPython.terminal.interactiveshell' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/terminal/interactiveshell.py'>,
     'prompt_toolkit': <module 'prompt_toolkit' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/__init__.py'>,
     'prompt_toolkit.interface': <module 'prompt_toolkit.interface' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/interface.py'>,
     'prompt_toolkit.application': <module 'prompt_toolkit.application' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/application.py'>,
     'prompt_toolkit.buffer': <module 'prompt_toolkit.buffer' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/buffer.py'>,
     'prompt_toolkit.auto_suggest': <module 'prompt_toolkit.auto_suggest' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/auto_suggest.py'>,
     'prompt_toolkit.filters': <module 'prompt_toolkit.filters' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/filters/__init__.py'>,
     'prompt_toolkit.filters.base': <module 'prompt_toolkit.filters.base' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/filters/base.py'>,
     'prompt_toolkit.utils': <module 'prompt_toolkit.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/utils.py'>,
     'wcwidth': <module 'wcwidth' from '/Users/gram/anaconda/lib/python3.6/site-packages/wcwidth/__init__.py'>,
     'wcwidth.wcwidth': <module 'wcwidth.wcwidth' from '/Users/gram/anaconda/lib/python3.6/site-packages/wcwidth/wcwidth.py'>,
     'wcwidth.table_wide': <module 'wcwidth.table_wide' from '/Users/gram/anaconda/lib/python3.6/site-packages/wcwidth/table_wide.py'>,
     'wcwidth.table_zero': <module 'wcwidth.table_zero' from '/Users/gram/anaconda/lib/python3.6/site-packages/wcwidth/table_zero.py'>,
     'prompt_toolkit.filters.cli': <module 'prompt_toolkit.filters.cli' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/filters/cli.py'>,
     'prompt_toolkit.enums': <module 'prompt_toolkit.enums' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/enums.py'>,
     'prompt_toolkit.key_binding': <module 'prompt_toolkit.key_binding' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/__init__.py'>,
     'prompt_toolkit.key_binding.vi_state': <module 'prompt_toolkit.key_binding.vi_state' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/vi_state.py'>,
     'prompt_toolkit.cache': <module 'prompt_toolkit.cache' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/cache.py'>,
     'prompt_toolkit.filters.types': <module 'prompt_toolkit.filters.types' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/filters/types.py'>,
     'prompt_toolkit.filters.utils': <module 'prompt_toolkit.filters.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/filters/utils.py'>,
     'prompt_toolkit.clipboard': <module 'prompt_toolkit.clipboard' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/clipboard/__init__.py'>,
     'prompt_toolkit.clipboard.base': <module 'prompt_toolkit.clipboard.base' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/clipboard/base.py'>,
     'prompt_toolkit.selection': <module 'prompt_toolkit.selection' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/selection.py'>,
     'prompt_toolkit.clipboard.in_memory': <module 'prompt_toolkit.clipboard.in_memory' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/clipboard/in_memory.py'>,
     'prompt_toolkit.completion': <module 'prompt_toolkit.completion' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/completion.py'>,
     'prompt_toolkit.document': <module 'prompt_toolkit.document' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/document.py'>,
     'prompt_toolkit.history': <module 'prompt_toolkit.history' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/history.py'>,
     'prompt_toolkit.search_state': <module 'prompt_toolkit.search_state' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/search_state.py'>,
     'prompt_toolkit.validation': <module 'prompt_toolkit.validation' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/validation.py'>,
     'prompt_toolkit.buffer_mapping': <module 'prompt_toolkit.buffer_mapping' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/buffer_mapping.py'>,
     'prompt_toolkit.key_binding.bindings': <module 'prompt_toolkit.key_binding.bindings' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/bindings/__init__.py'>,
     'prompt_toolkit.key_binding.bindings.basic': <module 'prompt_toolkit.key_binding.bindings.basic' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/bindings/basic.py'>,
     'prompt_toolkit.keys': <module 'prompt_toolkit.keys' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/keys.py'>,
     'prompt_toolkit.layout': <module 'prompt_toolkit.layout' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/__init__.py'>,
     'prompt_toolkit.layout.containers': <module 'prompt_toolkit.layout.containers' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/containers.py'>,
     'prompt_toolkit.layout.controls': <module 'prompt_toolkit.layout.controls' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/controls.py'>,
     'prompt_toolkit.mouse_events': <module 'prompt_toolkit.mouse_events' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/mouse_events.py'>,
     'prompt_toolkit.token': <module 'prompt_toolkit.token' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/token.py'>,
     'prompt_toolkit.layout.lexers': <module 'prompt_toolkit.layout.lexers' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/lexers.py'>,
     'prompt_toolkit.layout.utils': <module 'prompt_toolkit.layout.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/utils.py'>,
     'prompt_toolkit.layout.processors': <module 'prompt_toolkit.layout.processors' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/processors.py'>,
     'prompt_toolkit.reactive': <module 'prompt_toolkit.reactive' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/reactive.py'>,
     'prompt_toolkit.layout.screen': <module 'prompt_toolkit.layout.screen' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/screen.py'>,
     'prompt_toolkit.layout.dimension': <module 'prompt_toolkit.layout.dimension' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/dimension.py'>,
     'prompt_toolkit.layout.margins': <module 'prompt_toolkit.layout.margins' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/margins.py'>,
     'prompt_toolkit.renderer': <module 'prompt_toolkit.renderer' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/renderer.py'>,
     'prompt_toolkit.layout.mouse_handlers': <module 'prompt_toolkit.layout.mouse_handlers' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/mouse_handlers.py'>,
     'prompt_toolkit.output': <module 'prompt_toolkit.output' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/output.py'>,
     'prompt_toolkit.styles': <module 'prompt_toolkit.styles' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/styles/__init__.py'>,
     'prompt_toolkit.styles.base': <module 'prompt_toolkit.styles.base' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/styles/base.py'>,
     'prompt_toolkit.styles.defaults': <module 'prompt_toolkit.styles.defaults' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/styles/defaults.py'>,
     'prompt_toolkit.styles.from_dict': <module 'prompt_toolkit.styles.from_dict' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/styles/from_dict.py'>,
     'prompt_toolkit.styles.utils': <module 'prompt_toolkit.styles.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/styles/utils.py'>,
     'prompt_toolkit.styles.from_pygments': <module 'prompt_toolkit.styles.from_pygments' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/styles/from_pygments.py'>,
     'pygments.style': <module 'pygments.style' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/style.py'>,
     'pygments.styles.default': <module 'pygments.styles.default' from '/Users/gram/anaconda/lib/python3.6/site-packages/pygments/styles/default.py'>,
     'prompt_toolkit.key_binding.bindings.named_commands': <module 'prompt_toolkit.key_binding.bindings.named_commands' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/bindings/named_commands.py'>,
     'prompt_toolkit.key_binding.bindings.completion': <module 'prompt_toolkit.key_binding.bindings.completion' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/bindings/completion.py'>,
     'prompt_toolkit.key_binding.registry': <module 'prompt_toolkit.key_binding.registry' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/registry.py'>,
     'prompt_toolkit.key_binding.input_processor': <module 'prompt_toolkit.key_binding.input_processor' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/input_processor.py'>,
     'prompt_toolkit.key_binding.bindings.emacs': <module 'prompt_toolkit.key_binding.bindings.emacs' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/bindings/emacs.py'>,
     'prompt_toolkit.key_binding.bindings.scroll': <module 'prompt_toolkit.key_binding.bindings.scroll' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/bindings/scroll.py'>,
     'prompt_toolkit.key_binding.bindings.vi': <module 'prompt_toolkit.key_binding.bindings.vi' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/bindings/vi.py'>,
     'prompt_toolkit.key_binding.digraphs': <module 'prompt_toolkit.key_binding.digraphs' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/digraphs.py'>,
     'prompt_toolkit.key_binding.defaults': <module 'prompt_toolkit.key_binding.defaults' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/defaults.py'>,
     'prompt_toolkit.eventloop': <module 'prompt_toolkit.eventloop' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/eventloop/__init__.py'>,
     'prompt_toolkit.eventloop.base': <module 'prompt_toolkit.eventloop.base' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/eventloop/base.py'>,
     'prompt_toolkit.eventloop.callbacks': <module 'prompt_toolkit.eventloop.callbacks' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/eventloop/callbacks.py'>,
     'prompt_toolkit.input': <module 'prompt_toolkit.input' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/input.py'>,
     'prompt_toolkit.terminal': <module 'prompt_toolkit.terminal' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/terminal/__init__.py'>,
     'prompt_toolkit.terminal.vt100_input': <module 'prompt_toolkit.terminal.vt100_input' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/terminal/vt100_input.py'>,
     'prompt_toolkit.shortcuts': <module 'prompt_toolkit.shortcuts' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/shortcuts.py'>,
     'prompt_toolkit.layout.menus': <module 'prompt_toolkit.layout.menus' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/menus.py'>,
     'prompt_toolkit.layout.prompt': <module 'prompt_toolkit.layout.prompt' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/prompt.py'>,
     'prompt_toolkit.layout.toolbars': <module 'prompt_toolkit.layout.toolbars' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/layout/toolbars.py'>,
     'prompt_toolkit.terminal.vt100_output': <module 'prompt_toolkit.terminal.vt100_output' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/terminal/vt100_output.py'>,
     'prompt_toolkit.key_binding.manager': <module 'prompt_toolkit.key_binding.manager' from '/Users/gram/anaconda/lib/python3.6/site-packages/prompt_toolkit/key_binding/manager.py'>,
     'IPython.terminal.debugger': <module 'IPython.terminal.debugger' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/terminal/debugger.py'>,
     'IPython.core.completer': <module 'IPython.core.completer' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/completer.py'>,
     'IPython.core.latex_symbols': <module 'IPython.core.latex_symbols' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/latex_symbols.py'>,
     'IPython.utils.generics': <module 'IPython.utils.generics' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/generics.py'>,
     'simplegeneric': <module 'simplegeneric' from '/Users/gram/anaconda/lib/python3.6/site-packages/simplegeneric.py'>,
     'jedi': <module 'jedi' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/__init__.py'>,
     'jedi.api': <module 'jedi.api' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/api/__init__.py'>,
     'parso': <module 'parso' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/__init__.py'>,
     'parso.parser': <module 'parso.parser' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/parser.py'>,
     'parso.tree': <module 'parso.tree' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/tree.py'>,
     'parso._compatibility': <module 'parso._compatibility' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/_compatibility.py'>,
     'parso.pgen2': <module 'parso.pgen2' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/pgen2/__init__.py'>,
     'parso.pgen2.parse': <module 'parso.pgen2.parse' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/pgen2/parse.py'>,
     'parso.python': <module 'parso.python' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/python/__init__.py'>,
     'parso.python.tokenize': <module 'parso.python.tokenize' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/python/tokenize.py'>,
     'parso.python.token': <module 'parso.python.token' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/python/token.py'>,
     'parso.utils': <module 'parso.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/utils.py'>,
     'parso.grammar': <module 'parso.grammar' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/grammar.py'>,
     'parso.pgen2.pgen': <module 'parso.pgen2.pgen' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/pgen2/pgen.py'>,
     'parso.pgen2.grammar': <module 'parso.pgen2.grammar' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/pgen2/grammar.py'>,
     'parso.python.diff': <module 'parso.python.diff' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/python/diff.py'>,
     'parso.python.parser': <module 'parso.python.parser' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/python/parser.py'>,
     'parso.python.tree': <module 'parso.python.tree' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/python/tree.py'>,
     'parso.python.prefix': <module 'parso.python.prefix' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/python/prefix.py'>,
     'parso.cache': <module 'parso.cache' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/cache.py'>,
     'gc': <module 'gc' (built-in)>,
     'parso.python.errors': <module 'parso.python.errors' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/python/errors.py'>,
     'parso.normalizer': <module 'parso.normalizer' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/normalizer.py'>,
     'parso.python.pep8': <module 'parso.python.pep8' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/python/pep8.py'>,
     'parso.python.fstring': <module 'parso.python.fstring' from '/Users/gram/anaconda/lib/python3.6/site-packages/parso/python/fstring.py'>,
     'jedi.parser_utils': <module 'jedi.parser_utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/parser_utils.py'>,
     'jedi._compatibility': <module 'jedi._compatibility' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/_compatibility.py'>,
     'imp': <module 'imp' from '/Users/gram/anaconda/lib/python3.6/imp.py'>,
     'jedi.debug': <module 'jedi.debug' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/debug.py'>,
     'jedi.settings': <module 'jedi.settings' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/settings.py'>,
     'jedi.cache': <module 'jedi.cache' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/cache.py'>,
     'jedi.api.classes': <module 'jedi.api.classes' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/api/classes.py'>,
     'jedi.common': <module 'jedi.common' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/common.py'>,
     'jedi.evaluate': <module 'jedi.evaluate' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/__init__.py'>,
     'jedi.evaluate.representation': <module 'jedi.evaluate.representation' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/representation.py'>,
     'jedi.evaluate.cache': <module 'jedi.evaluate.cache' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/cache.py'>,
     'jedi.evaluate.compiled': <module 'jedi.evaluate.compiled' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/compiled/__init__.py'>,
     'jedi.evaluate.filters': <module 'jedi.evaluate.filters' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/filters.py'>,
     'jedi.evaluate.flow_analysis': <module 'jedi.evaluate.flow_analysis' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/flow_analysis.py'>,
     'jedi.evaluate.context': <module 'jedi.evaluate.context' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/context.py'>,
     'jedi.evaluate.compiled.getattr_static': <module 'jedi.evaluate.compiled.getattr_static' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/compiled/getattr_static.py'>,
     'jedi.evaluate.compiled.fake': <module 'jedi.evaluate.compiled.fake' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/compiled/fake.py'>,
     'jedi.evaluate.recursion': <module 'jedi.evaluate.recursion' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/recursion.py'>,
     'jedi.evaluate.iterable': <module 'jedi.evaluate.iterable' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/iterable.py'>,
     'jedi.evaluate.helpers': <module 'jedi.evaluate.helpers' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/helpers.py'>,
     'jedi.evaluate.analysis': <module 'jedi.evaluate.analysis' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/analysis.py'>,
     'jedi.evaluate.pep0484': <module 'jedi.evaluate.pep0484' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/pep0484.py'>,
     'jedi.evaluate.precedence': <module 'jedi.evaluate.precedence' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/precedence.py'>,
     'jedi.evaluate.docstrings': <module 'jedi.evaluate.docstrings' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/docstrings.py'>,
     'numpydoc': <module 'numpydoc' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpydoc/__init__.py'>,
     'numpydoc.numpydoc': <module 'numpydoc.numpydoc' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpydoc/numpydoc.py'>,
     'numpydoc.docscrape_sphinx': <module 'numpydoc.docscrape_sphinx' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpydoc/docscrape_sphinx.py'>,
     'jinja2.sandbox': <module 'jinja2.sandbox' from '/Users/gram/anaconda/lib/python3.6/site-packages/jinja2/sandbox.py'>,
     'sphinx.jinja2glue': <module 'sphinx.jinja2glue' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/jinja2glue.py'>,
     'numpydoc.docscrape': <module 'numpydoc.docscrape' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpydoc/docscrape.py'>,
     'sphinx.domains.c': <module 'sphinx.domains.c' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/domains/c.py'>,
     'sphinx.domains.python': <module 'sphinx.domains.python' from '/Users/gram/anaconda/lib/python3.6/site-packages/sphinx/domains/python.py'>,
     'jedi.evaluate.param': <module 'jedi.evaluate.param' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/param.py'>,
     'jedi.evaluate.imports': <module 'jedi.evaluate.imports' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/imports.py'>,
     'jedi.evaluate.sys_path': <module 'jedi.evaluate.sys_path' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/sys_path.py'>,
     'jedi.evaluate.site': <module 'jedi.evaluate.site' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/site.py'>,
     'jedi.evaluate.parser_cache': <module 'jedi.evaluate.parser_cache' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/parser_cache.py'>,
     'jedi.evaluate.stdlib': <module 'jedi.evaluate.stdlib' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/stdlib.py'>,
     'jedi.evaluate.instance': <module 'jedi.evaluate.instance' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/instance.py'>,
     'jedi.evaluate.finder': <module 'jedi.evaluate.finder' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/finder.py'>,
     'jedi.api.keywords': <module 'jedi.api.keywords' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/api/keywords.py'>,
     'pydoc_data': <module 'pydoc_data' from '/Users/gram/anaconda/lib/python3.6/pydoc_data/__init__.py'>,
     'pydoc_data.topics': <module 'pydoc_data.topics' from '/Users/gram/anaconda/lib/python3.6/pydoc_data/topics.py'>,
     'jedi.api.interpreter': <module 'jedi.api.interpreter' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/api/interpreter.py'>,
     'jedi.evaluate.compiled.mixed': <module 'jedi.evaluate.compiled.mixed' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/evaluate/compiled/mixed.py'>,
     'jedi.api.usages': <module 'jedi.api.usages' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/api/usages.py'>,
     'jedi.api.helpers': <module 'jedi.api.helpers' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/api/helpers.py'>,
     'jedi.api.completion': <module 'jedi.api.completion' from '/Users/gram/anaconda/lib/python3.6/site-packages/jedi/api/completion.py'>,
     'IPython.terminal.ptutils': <module 'IPython.terminal.ptutils' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/terminal/ptutils.py'>,
     'IPython.terminal.shortcuts': <module 'IPython.terminal.shortcuts' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/terminal/shortcuts.py'>,
     'IPython.terminal.magics': <module 'IPython.terminal.magics' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/terminal/magics.py'>,
     'IPython.lib.clipboard': <module 'IPython.lib.clipboard' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/lib/clipboard.py'>,
     'IPython.terminal.pt_inputhooks': <module 'IPython.terminal.pt_inputhooks' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/terminal/pt_inputhooks/__init__.py'>,
     'IPython.terminal.prompts': <module 'IPython.terminal.prompts' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/terminal/prompts.py'>,
     'IPython.terminal.ipapp': <module 'IPython.terminal.ipapp' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/terminal/ipapp.py'>,
     'IPython.core.magics': <module 'IPython.core.magics' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/__init__.py'>,
     'IPython.core.magics.auto': <module 'IPython.core.magics.auto' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/auto.py'>,
     'IPython.core.magics.basic': <module 'IPython.core.magics.basic' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/basic.py'>,
     'IPython.core.magics.code': <module 'IPython.core.magics.code' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/code.py'>,
     'IPython.core.magics.config': <module 'IPython.core.magics.config' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/config.py'>,
     'IPython.core.magics.display': <module 'IPython.core.magics.display' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/display.py'>,
     'IPython.core.magics.execution': <module 'IPython.core.magics.execution' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/execution.py'>,
     'timeit': <module 'timeit' from '/Users/gram/anaconda/lib/python3.6/timeit.py'>,
     'cProfile': <module 'cProfile' from '/Users/gram/anaconda/lib/python3.6/cProfile.py'>,
     '_lsprof': <module '_lsprof' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_lsprof.cpython-36m-darwin.so'>,
     'profile': <module 'profile' from '/Users/gram/anaconda/lib/python3.6/profile.py'>,
     'pstats': <module 'pstats' from '/Users/gram/anaconda/lib/python3.6/pstats.py'>,
     'IPython.utils.module_paths': <module 'IPython.utils.module_paths' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/module_paths.py'>,
     'IPython.utils.timing': <module 'IPython.utils.timing' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/timing.py'>,
     'IPython.core.magics.extension': <module 'IPython.core.magics.extension' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/extension.py'>,
     'IPython.core.magics.history': <module 'IPython.core.magics.history' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/history.py'>,
     'IPython.core.magics.logging': <module 'IPython.core.magics.logging' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/logging.py'>,
     'IPython.core.magics.namespace': <module 'IPython.core.magics.namespace' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/namespace.py'>,
     'IPython.core.magics.osm': <module 'IPython.core.magics.osm' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/osm.py'>,
     'IPython.core.magics.pylab': <module 'IPython.core.magics.pylab' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/pylab.py'>,
     'IPython.core.pylabtools': <module 'IPython.core.pylabtools' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/pylabtools.py'>,
     'IPython.core.magics.script': <module 'IPython.core.magics.script' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/magics/script.py'>,
     'IPython.lib.backgroundjobs': <module 'IPython.lib.backgroundjobs' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/lib/backgroundjobs.py'>,
     'IPython.core.shellapp': <module 'IPython.core.shellapp' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/shellapp.py'>,
     'IPython.extensions': <module 'IPython.extensions' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/extensions/__init__.py'>,
     'IPython.extensions.storemagic': <module 'IPython.extensions.storemagic' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/extensions/storemagic.py'>,
     'IPython.utils.frame': <module 'IPython.utils.frame' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/frame.py'>,
     'jupyter_client': <module 'jupyter_client' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/__init__.py'>,
     'jupyter_client._version': <module 'jupyter_client._version' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/_version.py'>,
     'jupyter_client.connect': <module 'jupyter_client.connect' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/connect.py'>,
     'zmq': <module 'zmq' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/__init__.py'>,
     'zmq.backend': <module 'zmq.backend' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/__init__.py'>,
     'zmq.backend.select': <module 'zmq.backend.select' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/select.py'>,
     'zmq.backend.cython': <module 'zmq.backend.cython' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/cython/__init__.py'>,
     'cython_runtime': <module 'cython_runtime'>,
     'zmq.backend.cython.constants': <module 'zmq.backend.cython.constants' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/cython/constants.cpython-36m-darwin.so'>,
     '_cython_0_27_2': <module '_cython_0_27_2'>,
     'zmq.backend.cython.error': <module 'zmq.backend.cython.error' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/cython/error.cpython-36m-darwin.so'>,
     'zmq.utils': <module 'zmq.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/utils/__init__.py'>,
     'zmq.utils.strtypes': <module 'zmq.utils.strtypes' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/utils/strtypes.py'>,
     'zmq.backend.cython.message': <module 'zmq.backend.cython.message' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/cython/message.cpython-36m-darwin.so'>,
     'zmq.error': <module 'zmq.error' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/error.py'>,
     'zmq.backend.cython.context': <module 'zmq.backend.cython.context' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/cython/context.cpython-36m-darwin.so'>,
     'zmq.backend.cython.socket': <module 'zmq.backend.cython.socket' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/cython/socket.cpython-36m-darwin.so'>,
     'zmq.backend.cython.utils': <module 'zmq.backend.cython.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/cython/utils.cpython-36m-darwin.so'>,
     'zmq.backend.cython._poll': <module 'zmq.backend.cython._poll' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/cython/_poll.cpython-36m-darwin.so'>,
     'zmq.backend.cython._version': <module 'zmq.backend.cython._version' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/cython/_version.cpython-36m-darwin.so'>,
     'zmq.backend.cython._device': <module 'zmq.backend.cython._device' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/backend/cython/_device.cpython-36m-darwin.so'>,
     'zmq.sugar': <module 'zmq.sugar' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/sugar/__init__.py'>,
     'zmq.sugar.constants': <module 'zmq.sugar.constants' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/sugar/constants.py'>,
     'zmq.utils.constant_names': <module 'zmq.utils.constant_names' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/utils/constant_names.py'>,
     'zmq.sugar.context': <module 'zmq.sugar.context' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/sugar/context.py'>,
     'zmq.sugar.attrsettr': <module 'zmq.sugar.attrsettr' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/sugar/attrsettr.py'>,
     'zmq.sugar.socket': <module 'zmq.sugar.socket' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/sugar/socket.py'>,
     'zmq.sugar.poll': <module 'zmq.sugar.poll' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/sugar/poll.py'>,
     'zmq.sugar.frame': <module 'zmq.sugar.frame' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/sugar/frame.py'>,
     'zmq.sugar.tracker': <module 'zmq.sugar.tracker' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/sugar/tracker.py'>,
     'zmq.sugar.version': <module 'zmq.sugar.version' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/sugar/version.py'>,
     'zmq.sugar.stopwatch': <module 'zmq.sugar.stopwatch' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/sugar/stopwatch.py'>,
     'jupyter_client.localinterfaces': <module 'jupyter_client.localinterfaces' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/localinterfaces.py'>,
     'jupyter_core': <module 'jupyter_core' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_core/__init__.py'>,
     'jupyter_core.version': <module 'jupyter_core.version' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_core/version.py'>,
     'jupyter_core.paths': <module 'jupyter_core.paths' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_core/paths.py'>,
     'jupyter_client.launcher': <module 'jupyter_client.launcher' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/launcher.py'>,
     'traitlets.log': <module 'traitlets.log' from '/Users/gram/anaconda/lib/python3.6/site-packages/traitlets/log.py'>,
     'jupyter_client.client': <module 'jupyter_client.client' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/client.py'>,
     'jupyter_client.channels': <module 'jupyter_client.channels' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/channels.py'>,
     'jupyter_client.channelsabc': <module 'jupyter_client.channelsabc' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/channelsabc.py'>,
     'jupyter_client.clientabc': <module 'jupyter_client.clientabc' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/clientabc.py'>,
     'jupyter_client.manager': <module 'jupyter_client.manager' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/manager.py'>,
     'jupyter_client.kernelspec': <module 'jupyter_client.kernelspec' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/kernelspec.py'>,
     'jupyter_client.session': <module 'jupyter_client.session' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/session.py'>,
     'hmac': <module 'hmac' from '/Users/gram/anaconda/lib/python3.6/hmac.py'>,
     'zmq.utils.jsonapi': <module 'zmq.utils.jsonapi' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/utils/jsonapi.py'>,
     'zmq.eventloop': <module 'zmq.eventloop' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/eventloop/__init__.py'>,
     'zmq.eventloop.ioloop': <module 'zmq.eventloop.ioloop' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/eventloop/ioloop.py'>,
     'tornado': <module 'tornado' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/__init__.py'>,
     'tornado.ioloop': <module 'tornado.ioloop' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/ioloop.py'>,
     'tornado.concurrent': <module 'tornado.concurrent' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/concurrent.py'>,
     'tornado.log': <module 'tornado.log' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/log.py'>,
     'tornado.escape': <module 'tornado.escape' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/escape.py'>,
     'tornado.util': <module 'tornado.util' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/util.py'>,
     'curses': <module 'curses' from '/Users/gram/anaconda/lib/python3.6/curses/__init__.py'>,
     '_curses': <module '_curses' from '/Users/gram/anaconda/lib/python3.6/lib-dynload/_curses.cpython-36m-darwin.so'>,
     'tornado.stack_context': <module 'tornado.stack_context' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/stack_context.py'>,
     'tornado.platform': <module 'tornado.platform' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/platform/__init__.py'>,
     'tornado.platform.auto': <module 'tornado.platform.auto' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/platform/auto.py'>,
     'tornado.platform.posix': <module 'tornado.platform.posix' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/platform/posix.py'>,
     'tornado.platform.common': <module 'tornado.platform.common' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/platform/common.py'>,
     'tornado.platform.interface': <module 'tornado.platform.interface' from '/Users/gram/anaconda/lib/python3.6/site-packages/tornado/platform/interface.py'>,
     'zmq.eventloop.zmqstream': <module 'zmq.eventloop.zmqstream' from '/Users/gram/anaconda/lib/python3.6/site-packages/zmq/eventloop/zmqstream.py'>,
     'jupyter_client.jsonutil': <module 'jupyter_client.jsonutil' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/jsonutil.py'>,
     'dateutil': <module 'dateutil' from '/Users/gram/anaconda/lib/python3.6/site-packages/dateutil/__init__.py'>,
     'dateutil._version': <module 'dateutil._version' from '/Users/gram/anaconda/lib/python3.6/site-packages/dateutil/_version.py'>,
     'dateutil.parser': <module 'dateutil.parser' from '/Users/gram/anaconda/lib/python3.6/site-packages/dateutil/parser.py'>,
     'dateutil.relativedelta': <module 'dateutil.relativedelta' from '/Users/gram/anaconda/lib/python3.6/site-packages/dateutil/relativedelta.py'>,
     'dateutil._common': <module 'dateutil._common' from '/Users/gram/anaconda/lib/python3.6/site-packages/dateutil/_common.py'>,
     'dateutil.tz': <module 'dateutil.tz' from '/Users/gram/anaconda/lib/python3.6/site-packages/dateutil/tz/__init__.py'>,
     'dateutil.tz.tz': <module 'dateutil.tz.tz' from '/Users/gram/anaconda/lib/python3.6/site-packages/dateutil/tz/tz.py'>,
     'dateutil.tz._common': <module 'dateutil.tz._common' from '/Users/gram/anaconda/lib/python3.6/site-packages/dateutil/tz/_common.py'>,
     '_strptime': <module '_strptime' from '/Users/gram/anaconda/lib/python3.6/_strptime.py'>,
     'jupyter_client.adapter': <module 'jupyter_client.adapter' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/adapter.py'>,
     'jupyter_client.managerabc': <module 'jupyter_client.managerabc' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/managerabc.py'>,
     'jupyter_client.blocking': <module 'jupyter_client.blocking' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/blocking/__init__.py'>,
     'jupyter_client.blocking.client': <module 'jupyter_client.blocking.client' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/blocking/client.py'>,
     'jupyter_client.blocking.channels': <module 'jupyter_client.blocking.channels' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/blocking/channels.py'>,
     'jupyter_client.multikernelmanager': <module 'jupyter_client.multikernelmanager' from '/Users/gram/anaconda/lib/python3.6/site-packages/jupyter_client/multikernelmanager.py'>,
     'ipykernel.kernelapp': <module 'ipykernel.kernelapp' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/kernelapp.py'>,
     'ipykernel.iostream': <module 'ipykernel.iostream' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/iostream.py'>,
     'ipykernel.heartbeat': <module 'ipykernel.heartbeat' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/heartbeat.py'>,
     'ipykernel.ipkernel': <module 'ipykernel.ipkernel' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/ipkernel.py'>,
     'IPython.utils.tokenutil': <module 'IPython.utils.tokenutil' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/utils/tokenutil.py'>,
     'ipykernel.comm': <module 'ipykernel.comm' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/comm/__init__.py'>,
     'ipykernel.comm.manager': <module 'ipykernel.comm.manager' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/comm/manager.py'>,
     'ipykernel.comm.comm': <module 'ipykernel.comm.comm' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/comm/comm.py'>,
     'ipykernel.kernelbase': <module 'ipykernel.kernelbase' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/kernelbase.py'>,
     'ipykernel.jsonutil': <module 'ipykernel.jsonutil' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/jsonutil.py'>,
     'ipykernel.zmqshell': <module 'ipykernel.zmqshell' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/zmqshell.py'>,
     'IPython.core.payloadpage': <module 'IPython.core.payloadpage' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/payloadpage.py'>,
     'ipykernel.displayhook': <module 'ipykernel.displayhook' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/displayhook.py'>,
     'ipykernel.parentpoller': <module 'ipykernel.parentpoller' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/parentpoller.py'>,
     'faulthandler': <module 'faulthandler' (built-in)>,
     'ipykernel.datapub': <module 'ipykernel.datapub' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/datapub.py'>,
     'ipykernel.serialize': <module 'ipykernel.serialize' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/serialize.py'>,
     'ipykernel.pickleutil': <module 'ipykernel.pickleutil' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/pickleutil.py'>,
     'ipykernel.codeutil': <module 'ipykernel.codeutil' from '/Users/gram/anaconda/lib/python3.6/site-packages/ipykernel/codeutil.py'>,
     'IPython.core.completerlib': <module 'IPython.core.completerlib' from '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/core/completerlib.py'>,
     'matplotlib': <module 'matplotlib' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/__init__.py'>,
     'distutils.sysconfig': <module 'distutils.sysconfig' from '/Users/gram/anaconda/lib/python3.6/distutils/sysconfig.py'>,
     'distutils.errors': <module 'distutils.errors' from '/Users/gram/anaconda/lib/python3.6/distutils/errors.py'>,
     'matplotlib.cbook': <module 'matplotlib.cbook' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/cbook/__init__.py'>,
     'gzip': <module 'gzip' from '/Users/gram/anaconda/lib/python3.6/gzip.py'>,
     'matplotlib.cbook.deprecation': <module 'matplotlib.cbook.deprecation' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/cbook/deprecation.py'>,
     'numpy': <module 'numpy' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/__init__.py'>,
     'numpy._globals': <module 'numpy._globals' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/_globals.py'>,
     'numpy.__config__': <module 'numpy.__config__' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/__config__.py'>,
     'numpy.version': <module 'numpy.version' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/version.py'>,
     'numpy._import_tools': <module 'numpy._import_tools' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/_import_tools.py'>,
     'numpy.add_newdocs': <module 'numpy.add_newdocs' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/add_newdocs.py'>,
     'numpy.lib': <module 'numpy.lib' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/__init__.py'>,
     'numpy.lib.info': <module 'numpy.lib.info' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/info.py'>,
     'numpy.lib.type_check': <module 'numpy.lib.type_check' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/type_check.py'>,
     'numpy.core': <module 'numpy.core' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/__init__.py'>,
     'numpy.core.info': <module 'numpy.core.info' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/info.py'>,
     'numpy.core.multiarray': <module 'numpy.core.multiarray' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/multiarray.cpython-36m-darwin.so'>,
     'numpy.core.umath': <module 'numpy.core.umath' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/umath.cpython-36m-darwin.so'>,
     'numpy.core._internal': <module 'numpy.core._internal' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/_internal.py'>,
     'numpy.compat': <module 'numpy.compat' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/compat/__init__.py'>,
     'numpy.compat._inspect': <module 'numpy.compat._inspect' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/compat/_inspect.py'>,
     'numpy.compat.py3k': <module 'numpy.compat.py3k' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/compat/py3k.py'>,
     'numpy.core.numerictypes': <module 'numpy.core.numerictypes' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/numerictypes.py'>,
     'numpy.core.numeric': <module 'numpy.core.numeric' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/numeric.py'>,
     'numpy.core.arrayprint': <module 'numpy.core.arrayprint' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/arrayprint.py'>,
     'numpy.core.fromnumeric': <module 'numpy.core.fromnumeric' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/fromnumeric.py'>,
     'numpy.core._methods': <module 'numpy.core._methods' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/_methods.py'>,
     'numpy.core.defchararray': <module 'numpy.core.defchararray' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/defchararray.py'>,
     'numpy.core.records': <module 'numpy.core.records' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/records.py'>,
     'numpy.core.memmap': <module 'numpy.core.memmap' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/memmap.py'>,
     'numpy.core.function_base': <module 'numpy.core.function_base' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/function_base.py'>,
     'numpy.core.machar': <module 'numpy.core.machar' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/machar.py'>,
     'numpy.core.getlimits': <module 'numpy.core.getlimits' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/getlimits.py'>,
     'numpy.core.shape_base': <module 'numpy.core.shape_base' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/shape_base.py'>,
     'numpy.core.einsumfunc': <module 'numpy.core.einsumfunc' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/core/einsumfunc.py'>,
     'numpy.testing': <module 'numpy.testing' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/testing/__init__.py'>,
     'unittest': <module 'unittest' from '/Users/gram/anaconda/lib/python3.6/unittest/__init__.py'>,
     'unittest.result': <module 'unittest.result' from '/Users/gram/anaconda/lib/python3.6/unittest/result.py'>,
     'unittest.util': <module 'unittest.util' from '/Users/gram/anaconda/lib/python3.6/unittest/util.py'>,
     'unittest.case': <module 'unittest.case' from '/Users/gram/anaconda/lib/python3.6/unittest/case.py'>,
     'unittest.suite': <module 'unittest.suite' from '/Users/gram/anaconda/lib/python3.6/unittest/suite.py'>,
     'unittest.loader': <module 'unittest.loader' from '/Users/gram/anaconda/lib/python3.6/unittest/loader.py'>,
     'unittest.main': <module 'unittest.main' from '/Users/gram/anaconda/lib/python3.6/unittest/main.py'>,
     'unittest.runner': <module 'unittest.runner' from '/Users/gram/anaconda/lib/python3.6/unittest/runner.py'>,
     'unittest.signals': <module 'unittest.signals' from '/Users/gram/anaconda/lib/python3.6/unittest/signals.py'>,
     'numpy.testing.decorators': <module 'numpy.testing.decorators' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/testing/decorators.py'>,
     'numpy.testing.utils': <module 'numpy.testing.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/testing/utils.py'>,
     'numpy.lib.utils': <module 'numpy.lib.utils' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/utils.py'>,
     'numpy.testing.nosetester': <module 'numpy.testing.nosetester' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/testing/nosetester.py'>,
     'numpy.lib.ufunclike': <module 'numpy.lib.ufunclike' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/ufunclike.py'>,
     'numpy.lib.index_tricks': <module 'numpy.lib.index_tricks' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/index_tricks.py'>,
     'numpy.lib.function_base': <module 'numpy.lib.function_base' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/function_base.py'>,
     'numpy.lib.twodim_base': <module 'numpy.lib.twodim_base' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/twodim_base.py'>,
     'numpy.matrixlib': <module 'numpy.matrixlib' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/matrixlib/__init__.py'>,
     'numpy.matrixlib.defmatrix': <module 'numpy.matrixlib.defmatrix' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/matrixlib/defmatrix.py'>,
     'numpy.lib.stride_tricks': <module 'numpy.lib.stride_tricks' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/stride_tricks.py'>,
     'numpy.lib.mixins': <module 'numpy.lib.mixins' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/mixins.py'>,
     'numpy.lib.nanfunctions': <module 'numpy.lib.nanfunctions' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/nanfunctions.py'>,
     'numpy.lib.shape_base': <module 'numpy.lib.shape_base' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/shape_base.py'>,
     'numpy.lib.scimath': <module 'numpy.lib.scimath' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/scimath.py'>,
     'numpy.lib.polynomial': <module 'numpy.lib.polynomial' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/polynomial.py'>,
     'numpy.linalg': <module 'numpy.linalg' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/linalg/__init__.py'>,
     'numpy.linalg.info': <module 'numpy.linalg.info' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/linalg/info.py'>,
     'numpy.linalg.linalg': <module 'numpy.linalg.linalg' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/linalg/linalg.py'>,
     'numpy.linalg.lapack_lite': <module 'numpy.linalg.lapack_lite' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/linalg/lapack_lite.cpython-36m-darwin.so'>,
     'numpy.linalg._umath_linalg': <module 'numpy.linalg._umath_linalg' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/linalg/_umath_linalg.cpython-36m-darwin.so'>,
     'numpy.lib.arraysetops': <module 'numpy.lib.arraysetops' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/arraysetops.py'>,
     'numpy.lib.npyio': <module 'numpy.lib.npyio' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/npyio.py'>,
     'numpy.lib.format': <module 'numpy.lib.format' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/format.py'>,
     'numpy.lib._datasource': <module 'numpy.lib._datasource' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/_datasource.py'>,
     'numpy.lib._iotools': <module 'numpy.lib._iotools' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/_iotools.py'>,
     'numpy.lib.financial': <module 'numpy.lib.financial' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/financial.py'>,
     'numpy.lib.arrayterator': <module 'numpy.lib.arrayterator' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/arrayterator.py'>,
     'numpy.lib.arraypad': <module 'numpy.lib.arraypad' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/arraypad.py'>,
     'numpy.lib._version': <module 'numpy.lib._version' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/lib/_version.py'>,
     'numpy._distributor_init': <module 'numpy._distributor_init' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/_distributor_init.py'>,
     'numpy.fft': <module 'numpy.fft' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/fft/__init__.py'>,
     'numpy.fft.info': <module 'numpy.fft.info' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/fft/info.py'>,
     'numpy.fft.fftpack': <module 'numpy.fft.fftpack' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/fft/fftpack.py'>,
     'numpy.fft.fftpack_lite': <module 'numpy.fft.fftpack_lite' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/fft/fftpack_lite.cpython-36m-darwin.so'>,
     'numpy.fft.helper': <module 'numpy.fft.helper' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/fft/helper.py'>,
     'numpy.polynomial': <module 'numpy.polynomial' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/polynomial/__init__.py'>,
     'numpy.polynomial.polynomial': <module 'numpy.polynomial.polynomial' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/polynomial/polynomial.py'>,
     'numpy.polynomial.polyutils': <module 'numpy.polynomial.polyutils' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/polynomial/polyutils.py'>,
     'numpy.polynomial._polybase': <module 'numpy.polynomial._polybase' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/polynomial/_polybase.py'>,
     'numpy.polynomial.chebyshev': <module 'numpy.polynomial.chebyshev' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/polynomial/chebyshev.py'>,
     'numpy.polynomial.legendre': <module 'numpy.polynomial.legendre' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/polynomial/legendre.py'>,
     'numpy.polynomial.hermite': <module 'numpy.polynomial.hermite' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/polynomial/hermite.py'>,
     'numpy.polynomial.hermite_e': <module 'numpy.polynomial.hermite_e' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/polynomial/hermite_e.py'>,
     'numpy.polynomial.laguerre': <module 'numpy.polynomial.laguerre' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/polynomial/laguerre.py'>,
     'numpy.random': <module 'numpy.random' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/random/__init__.py'>,
     'numpy.random.info': <module 'numpy.random.info' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/random/info.py'>,
     'mtrand': <module 'numpy.random.mtrand' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/random/mtrand.cpython-36m-darwin.so'>,
     'numpy.random.mtrand': <module 'numpy.random.mtrand' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/random/mtrand.cpython-36m-darwin.so'>,
     'numpy.ctypeslib': <module 'numpy.ctypeslib' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/ctypeslib.py'>,
     'numpy.ma': <module 'numpy.ma' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/ma/__init__.py'>,
     'numpy.ma.core': <module 'numpy.ma.core' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/ma/core.py'>,
     'numpy.ma.extras': <module 'numpy.ma.extras' from '/Users/gram/anaconda/lib/python3.6/site-packages/numpy/ma/extras.py'>,
     'matplotlib.cbook._backports': <module 'matplotlib.cbook._backports' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/cbook/_backports.py'>,
     'matplotlib.compat': <module 'matplotlib.compat' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/compat/__init__.py'>,
     'matplotlib.compat.subprocess': <module 'matplotlib.compat.subprocess' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/compat/subprocess.py'>,
     'matplotlib.rcsetup': <module 'matplotlib.rcsetup' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/rcsetup.py'>,
     'matplotlib.fontconfig_pattern': <module 'matplotlib.fontconfig_pattern' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/fontconfig_pattern.py'>,
     'pyparsing': <module 'pyparsing' from '/Users/gram/anaconda/lib/python3.6/site-packages/pyparsing.py'>,
     'matplotlib.colors': <module 'matplotlib.colors' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/colors.py'>,
     'matplotlib._color_data': <module 'matplotlib._color_data' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/_color_data.py'>,
     'cycler': <module 'cycler' from '/Users/gram/anaconda/lib/python3.6/site-packages/cycler.py'>,
     'six.moves.urllib.request': <module 'six.moves.urllib.request' (<six._SixMetaPathImporter object at 0x10b8d1630>)>,
     'matplotlib._version': <module 'matplotlib._version' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/_version.py'>,
     'matplotlib.pyplot': <module 'matplotlib.pyplot' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/pyplot.py'>,
     'matplotlib.colorbar': <module 'matplotlib.colorbar' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/colorbar.py'>,
     'matplotlib.artist': <module 'matplotlib.artist' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/artist.py'>,
     'matplotlib.docstring': <module 'matplotlib.docstring' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/docstring.py'>,
     'matplotlib.path': <module 'matplotlib.path' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/path.py'>,
     'matplotlib._path': <module 'matplotlib._path' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/_path.cpython-36m-darwin.so'>,
     'matplotlib.transforms': <module 'matplotlib.transforms' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/transforms.py'>,
     'matplotlib.collections': <module 'matplotlib.collections' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/collections.py'>,
     'matplotlib.cm': <module 'matplotlib.cm' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/cm.py'>,
     'matplotlib._cm': <module 'matplotlib._cm' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/_cm.py'>,
     'matplotlib._cm_listed': <module 'matplotlib._cm_listed' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/_cm_listed.py'>,
     'matplotlib.mlab': <module 'matplotlib.mlab' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/mlab.py'>,
     'matplotlib.lines': <module 'matplotlib.lines' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/lines.py'>,
     'matplotlib.markers': <module 'matplotlib.markers' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/markers.py'>,
     'matplotlib.contour': <module 'matplotlib.contour' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/contour.py'>,
     'matplotlib._cntr': <module 'matplotlib._cntr' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/_cntr.cpython-36m-darwin.so'>,
     'matplotlib._contour': <module 'matplotlib._contour' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/_contour.cpython-36m-darwin.so'>,
     'matplotlib.ticker': <module 'matplotlib.ticker' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/ticker.py'>,
     'matplotlib.font_manager': <module 'matplotlib.font_manager' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/font_manager.py'>,
     'matplotlib.afm': <module 'matplotlib.afm' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/afm.py'>,
     'matplotlib._mathtext_data': <module 'matplotlib._mathtext_data' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/_mathtext_data.py'>,
     'matplotlib.ft2font': <module 'matplotlib.ft2font' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/ft2font.cpython-36m-darwin.so'>,
     'matplotlib.text': <module 'matplotlib.text' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/text.py'>,
     'matplotlib.patches': <module 'matplotlib.patches' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/patches.py'>,
     'matplotlib.bezier': <module 'matplotlib.bezier' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/bezier.py'>,
     'matplotlib.textpath': <module 'matplotlib.textpath' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/textpath.py'>,
     'matplotlib.mathtext': <module 'matplotlib.mathtext' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/mathtext.py'>,
     'matplotlib._png': <module 'matplotlib._png' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/_png.cpython-36m-darwin.so'>,
     'matplotlib.dviread': <module 'matplotlib.dviread' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/dviread.py'>,
     'matplotlib.texmanager': <module 'matplotlib.texmanager' from '/Users/gram/anaconda/lib/python3.6/site-packages/matplotlib/texmanager.py'>,
     ...}



`sys.path` is the path to look for imports:


```python
sys.path
```




    ['',
     '/Users/gram/anaconda/lib/python36.zip',
     '/Users/gram/anaconda/lib/python3.6',
     '/Users/gram/anaconda/lib/python3.6/lib-dynload',
     '/Users/gram/.local/lib/python3.6/site-packages',
     '/Users/gram/anaconda/lib/python3.6/site-packages',
     '/Users/gram/anaconda/lib/python3.6/site-packages/aeosa',
     '/Users/gram/anaconda/lib/python3.6/site-packages/IPython/extensions',
     '/Users/gram/.ipython']



### Using Threads and Processes

See https://medium.com/@bfortuner/python-multithreading-vs-multiprocessing-73072ce5600b

### Extending Python with C code

See https://dbader.org/blog/python-ctypes-tutorial#.

### Functional Programming in Python

See https://docs.python.org/dev/howto/functional.html#iterators and http://coconut-lang.org/

### Making HTTP Requests and Parsing Responses

There are numerous ways to do this in Python, but the most commonly used libraries for these are `requests` (http://docs.python-requests.org/en/master/) and Beautiful Soup (https://www.crummy.com/software/BeautifulSoup/); look at those first before considering anything else as they are powerful, stable, mature and easy to use. Kenneth Reitz, who wrote `requests`, has recently implemented a new library on top of both `requests` and Beautiful Soup: https://github.com/kennethreitz/requests-html


