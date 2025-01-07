### **Date-time notes:**
---

- There are two most useful functions when working with date time:
	- **strftime** : "string from time" converts the datetime/date/time objects to strings with given format, its an instance method.
	- **strptime** : "string parse time" converts the given string with specified format to datetime object.
	- type **`man	strftime`** or **`man strptime`** to get all format codes.



### **sqlite3 notes:**
---

- when using the with statement for sqlite3.connect() function, the connection is auto commited on the end of with statement.

- a really cool feature of slqlite is that you can create experiment with it without creating any files by just typing the command sqlite3, it will delete the database after exiting the program.

- **The best way to learn sqlite3 library or sqlite database program is to fast experiment with in memory database by specifying the file name as :memory:**

### **Notes on Exception**:

- The except block will catch the exception if the occured exception is matched or is subclass of targeted error.

- One except block can catch multiple exceptions.

- BaseException is the common base class of the exceptions, The Exception class is one the subclass of BaseException.

- Exceptions which are not subclasses of class Exception cannot be handled because they are not ment to be handled like KeyboardInterrupt and SystemExit, they are ment to terminate the program.

- Examples:
	- If there is some unknown error occured and if you want to discontinue the flow of program from there then reraise the error:
```python3
import sys

try:
    f = open('myfile.txt')
    s = f.readline()
    i = int(s.strip())
except OSError as err:
    print("OS error:", err)
except ValueError:
    print("Could not convert data to an integer.")
except Exception as err:
    print(f"Unexpected {err=}, {type(err)=}")
    raise
```

- The above example also allows caller to handle the unexpected exception as well.

- The single raise keyword will re-raise the current active exception.


- We can use raise Exception from exc clause to properly chain the exceptions:
```python3
def func():
    raise ConnectionError

try:
    func()
except ConnectionError as exc:
    raise RuntimeError('Failed to open database') from exc
```

- We can even disable the chaining by using None:
```python3
try:
    open('database.sqlite')
except OSError:
    raise RuntimeError from None
    
Traceback (most recent call last):
  File "<stdin>", line 4, in <module>
RuntimeError
```

```python3
def bool_return():
    try:
        return True
    finally:
        return False

bool_return()
False
```



### Detect if a callable is method or function or class:

- use builtin `inspect` module's `is*` functions, for example: `inspect.isclass()`, `inspect.ismethod()` or `inspect.isfunction()`
