# Decorators in python
---


- A decorator function has 2 types:
	- without parameters (not even zero parameters)
	- parameterized (can have zero parameters)


### **Without parameters:**

Example:

```python
def my_decorator(func):
    def wrapper(x):
        print("Before calling the function")
        result = func(x,10)
        print("After calling the function")
        return result
    return wrapper

@my_decorator
def my_func(x, y):
    return x + y

result = my_func(3)
print("Result:", result)
```

- here the decorator function is defined as a function which takes another function as input and returns a new function as output often called wrapper(because it may wrap original function).

- **The my_decorator function is called when there is @ sign is used to decorate function. After that the output wrapper function is assigned to the original function's name.**

Here is the example highlighting the above point:

```python
from time import sleep

def my_decorator(func):
    print('inside decorator', flush=True)
    def wrapper(x):
        print("Before calling the function",flush=True)
        result = func(x,10)
        print("After calling the function",flush=True)
        return result

    print('inside decorator', flush=True)

    return wrapper

@my_decorator
def my_func(x, y):
    return x + y

print('before sleep',flush=True)
sleep(3)

result = my_func(3)
print("Result:", result,flush=True)
```

the output before sleep is:
```txt
inside decorator
inside decorator
before sleep
```
and after sleep is:
```
Before calling the function
After calling the function
Result: 13
```

## **Parameterized:**

Example:

```python
def my_multi_parameter_decorator():
    def decorator(func):
        def wrapper(*args, **kwargs):
            print(f": Before calling the function")
            result = func(*args, **kwargs)
            print(f": After calling the function")
            return result
        return wrapper
    return decorator


@my_multi_parameter_decorator()
def my_func(x, y):
    return x + y

result = my_func(3, 5)
print("Result:", result)
```
- As we can see in above code that parameterized decorators are functions with parameters that returns decorator functions. this decorator functions then returns wrapper function as we saw in without parametized section.

```python
def my_multi_parameter_decorator():
    print('Inside higher order', flush=True)
    def decorator(func):
        print('inside decorator',flush=True)
        def wrapper(*args, **kwargs):
            print(f": Before calling the function",flush=True)
            result = func(*args, **kwargs)
            print(f": After calling the function",flush=True)
            return result
        return wrapper
    return decorator

@my_multi_parameter_decorator()
def my_func(x, y):
    return x + y

print('Before sleeping',flush=True)
sleep(3)

result = my_func(3, 5)
print("Result:", result,flush=True)
```

- the outpur before sleeping is:
```txt
Inside higher order
inside decorator
Before sleeping
``` 
- after sleeping:
```txt
: Before calling the function
: After calling the function
Result: 8
```

--- 

## **Using functools.wraps to create decorators:**

- The functools.wraps is a parameterized decorator function which takes the original function as argument and decorated on wrapper function. It is used to preserve the metadata(like docstring) of the original function.

```python
import functools

# Define a parameterized decorator function
def my_parameterized_decorator(prefix):
    def decorator(func):
        @functools.wraps(func)  # Use functools.wraps to preserve metadata
        def wrapper(*args, **kwargs):
            print(f"{prefix}: Before calling the function")
            result = func(*args, **kwargs)
            print(f"{prefix}: After calling the function")
            return result
        return wrapper
    return decorator

# Decorate a function with the parameterized decorator
@my_parameterized_decorator("DEBUG")
def my_func(x, y):
    """This is a function that adds two numbers."""
    return x + y

# Call the decorated function
result = my_func(3, 5)
print("Result:", result)

# Access metadata of the decorated function
print("Function name:", my_func.__name__)
print("Docstring:", my_func.__doc__)
```

