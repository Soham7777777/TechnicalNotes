## Typing in python:
---

- Material : https://realpython.com/python-type-checking/

- Python is dynamically typed lanugae. The word **Dynamic** means "while running the code".

- It means that types are checked during code is running. python also allows dynamic type changing means its allow to change type of an object during runtime.

- opposite of this is static typing where types are checked during compile time and types are not allowed to change during runtime.

- Python can have **Duck Typing** means a type that looks like List must be list, the term comes from "if it walks like a duck and it quacks like a duck, then it must be a duck".

- isinstance() and issubclass() are two important builtin functions.

- Python will always remain dynamicly typed language, but it can have "type hints" which makes it possible to do static type cheking, although they don't enforce python to have types, for that you need to use static type checker.

- the static type checker like "mypy" needs to check the the code before its running.

- PEP-275 is important proposal for documenting the code and docstring in general.

- PEP-483 is proposal for gradual typing.

- always remember that adding type hints don't effect the working python code, its only there for developers.

- Type annotions are just regular python expressions.


- **Union types** : Employee | Sequence[Employee]. A type factored by T1 | T2 | ... is a supertype of all types T1, T2, etc., so that a value that is a member of one of these types is acceptable for an argument annotated by T1 | T2 | ....

### Type checker support for Singleton Types:

- In application, we often need to define some special cases like, when there is a variable that is "red" color, if current state is "critical", or if process returned "timeout", these types of words can be described as some kind of special number or string or flag to diffrentiate between those "states".

- so we define some special values for tham, like ciritical = 0, red = 1, timeout = 2, etc..., **These special variables are singleton types.**

- but the problem here is that the integer value defined for those special cases can be overlapped for other cases for example, we want to multiply with the given argument, if the argument is 2 then how can we diffrentialte that its represnting "timeout" or actual integer value.

- to solve that problem python introduces EnumType. (C enum and python enum are not same, they might look like they works in same way.) 

- Example: lets say we have a function which takes an argument x, this can be int or None can be empty when calling. we want to handle all cases, so if we put None as default type then the case for empty and None are merged to gather.

- solution: Use union types in conjuction with custom Enum.
```python
from enum import Enum

class Empty(Enum):
    token = 0
_empty = Empty.token

def func(x: int | None | Empty = _empty) -> int:

    boom = x * 42  # This fails type check

    if x is _empty:
        return 0
    elif x is None:
        return 1
    else:  # At this point typechecker knows that x can only have type int
        return x * 2
        
# now function is handeled saperately in all 3 ways.
# case1: func()
# case2: func(None)
# case3: func(5)
```
- **The above code is an example of precise type checking with singleton types**

- In the above code, the 0 is not equal to Enum.token or _empty.token, but 0 is equal to Enum.token.value.

- The benifit of above code is that type checker is able to detect programmer's error for boom variable, and we can handle all cases saperetly.

- **Since the subclasses of Enum cannot be further subclassed, the type of variable x can be statically inferred in all branches of the above example.**

- other example:
```python
class Reason(Enum):
    timeout = 1
    error = 2

def process(response: str | Reason = '') -> str:
    if response is Reason.timeout:
        return 'TIMEOUT'
    elif response is Reason.error:
        return 'ERROR'
    else:
        # response can be only str, all other possible values exhausted
        return 'PROCESSED: ' + response
```
