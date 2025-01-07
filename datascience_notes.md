# Notes for Jupyter Notebook:

---

- `ctrl + shift + h` to get shortcuts or go to help -> show keyboard shortcuts
- `ctrl + a` select all cells
- `shift` + `up`/`down` arrow to select mutiple cells above or below 	
- `ctrl + enter` to execute selected cells
- `shift + enter` to execute current cell and select below cell, if not exist create one
- `alt + enter` to execute current cell and add new cell below
- `tab` after writing something to get completion suggestions
- `?` to get general object interospection
- `y`, `m` and `r` for converting a cell to code, markdown or raw
-  arrow keys to move up and down
- `a`, `b` and `dd` to insert cell above, below and delete cell

# Notes for Numpy

---

NumPy internally stores data in a contiguous block of memory,  independent of other built-in Python objects. NumPy's library of  algorithms written in the C language can operate on this memory without  any type checking or other overhead. NumPy arrays also use much less  memory than built-in Python sequences.

NumPy operations perform complex computations on entire arrays without the need for Python `for` loops, which can be slow for large sequences. NumPy is faster than  regular Python code because its C-based algorithms avoid overhead  present with regular interpreted Python code.

### The NumPy ndarray: A Multidimensional Array Object

An ndarray is a generic multidimensional container for homogeneous data; that is, all of the elements must be the same type. Every array has a `shape`, a tuple indicating the size of each dimension, and a `dtype`, an object describing the *data type* of the array.

The `dtypes` module contains all datatypes as python classes, a datatype of an element is represented by `{type}{bits per element}`, for example `float64` represents a datatype that takes 64 bits per element and its a float value. Get name of a datatype from `name` attribute  and Get size in bytes from `itemsize` attribute of instance of `dtypes.*` class, other attributes are also useful. Use numpy's `dtype` to construct a datatype from its name. Casting can be done through `ndarray.astype` method.

The `shape` property returns sequence of dimensions **from outer to inner** meaning last dimension represents length of array that has non-sequence datatype. For example:

```python 
data = [[1, 2, 3, 4], [5, 6, 7, 8]]
array = np.array(data)
print(array.shape)
# (2, 4)
print(array.dtype)
# dtype('int64')
```

In above code, the last dimension represents length of array that has datatype of `int64`. Also, the datatype of last dimension array is the data type of whole `ndarray`.

The ndarrya object can be created using some below listed useful functions:

- `array`, `zeros`, `ones`, `empty`, `arange`, `full`, `identity`
- note: `empty` construncts `ndarray` without **initializing** its values to any perticular value, its shows garbadge or zeros when printed, **but the underlaying memory is uninitialized**. 

#### Vectorization:

Its process of "element wise operation(s)" or "operation applied on each element".

A universal function (or `ufunc` for short) is a function that operates on `ndarrays` in an element-by-element fashion. That is, a ufunc is a “vectorized” wrapper for a function that takes a fixed number of specific inputs and produces a fixed number of specific outputs.

Universal functions are instances of `ufunc` class. Custom `ufunc` instances can be created using `frompyfunc` factory function. `ufunc` **can be unary or binary**, for example `sqrt` function is unary and `add` function is binary. `ufunc` can return one or more ndarray as result.

`ndarray` can be vectorized with either a scaler value or another `ndarray`. All default ufuncs can be found [here](https://numpy.org/doc/stable/reference/ufuncs.html#available-ufuncs). For simple useage =, +, -, *, /, **, <, >, ==, !=, @(for matrix multiplication) operators are supported as well.

#### Indexing and slicing:

