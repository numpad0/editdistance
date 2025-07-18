# editdistance compilation workaround fork

## usage

> git clone <this>  
> cd <this>  
> pip install .

dot means ./

## motivation & changes from original  

`C:\> pip install editdistance` currently doesn't work on CJK Windows because native programming language for Windows is C/C++ and string manipulation in C/C++ is done through directly manipulating C arrays hence legacy programs that manipulate string any shape or form needs to have compiling source rewritten and recompiled if they were to hypothetically support UTF-8 and therefore CJK systems are stuck with pre-UTF8 language-specific encoding for string formats which are at least backwards compatible with ASCII but anything outside ASCII && outside codepages of specific CJK languages the system is configured for will break it and make even MSVC compiler crash and burn if specific CJK languages support was correctly enabled at application side which is still a dice roll.

The only changes made was to delete non-ASCII comments. Not tested beyond compiling and installing once.

----------------------

# editdistance

Fast implementation of the edit distance (Levenshtein distance).

This library simply implements [Levenshtein distance](http://en.wikipedia.org/wiki/Levenshtein_distance) with C++ and Cython.

The algorithm used in this library is proposed by
[Heikki Hyyrö, "Explaining and extending the bit-parallel approximate string matching algorithm of Myers", (2001)](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.19.7158&rep=rep1&type=pdf)

## Binary wheels

Thanks to [pypa/cibuildwheel](https://github.com/pypa/cibuildwheel)
There are binary wheels on Linux, Mac OS, and Windows.

## Install

You can install via pip:

```bash
pip install editdistance
```


## Usage

It's quite simple:

```python
import editdistance
editdistance.eval('banana', 'bahama')
# 2L
```


# Simple Benchmark

With IPython, I tried several libraries:

* [pyxDamerauLevenshtein](https://pypi.python.org/pypi/pyxDamerauLevenshtein)
* [pylev](https://pypi.python.org/pypi/pylev)
* [python-Levenshtein](https://pypi.python.org/pypi/python-Levenshtein)

On Python 2.7.5:

```python
a = 'fsffvfdsbbdfvvdavavavavavava'
b = 'fvdaabavvvvvadvdvavavadfsfsdafvvav'
import pylev
timeit pylev.levenshtein(a, b)
# 100 loops, best of 3: 7.48 ms per loop

from pyxdameraulevenshtein import damerau_levenshtein_distance
timeit damerau_levenshtein_distance(a, b)
# 100000 loops, best of 3: 11.4 µs per loop

timeit editdistance.eval(a, b)  # my library
# 100000 loops, best of 3: 3.5 µs per loop

import Levenshtein

timeit Levenshtein.distance(a, b)
# 100000 loops, best of 3: 3.21 µs per loop
```

## Distance with Any Object

Above libraries only support strings.
But Sometimes other type of objects such as list of strings(words).
I support any iterable, only requires hashable object of it:

```python
Levenshtein.distance(['spam', 'egg'], ['spam', 'ham'])
# ---------------------------------------------------------------------------
# TypeError                                 Traceback (most recent call last)
# <ipython-input-22-3e0b30d145ac> in <module>()
# ----> 1 Levenshtein.distance(['spam', 'egg'], ['spam', 'ham'])
#
# TypeError: distance expected two Strings or two Unicodes

editdistance.eval(['spam', 'egg'], ['spam', 'ham'])
# 1L
```

So if object's hash is same, it's same.
You can provide `__hash__` method to your object instances.

Enjoy!

## License

It is released under the MIT license.

```
Copyright (c) 2013 Hiroyuki Tanaka

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```
