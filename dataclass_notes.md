# Python Dataclasses notes:
---

- **Important PEP:** PEP-557, PEP-526

---

### **Glossary**:

- fields : Syntax for variable annotions

---

- Dataclasses can be thought of as "mutable namedtuple with defaults".

- The dataclass decorator will add various “dunder” methods to the class, described below. If any of the added methods already exist on the class, a TypeError will be raised.

- the dataclass decorator can be applied without any parameters and parantheses, it also supports the below signature:
```
def dataclass(*, init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False)
```
if there is no parantheses(no signature) then it acts as it has default values in above signature.

- the below all three signatures are equvivalent:
```
@dataclass
class C:
    ...

@dataclass()
class C:
    ...

@dataclass(init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False)
class C:
    ...
```

- If order is true and eq is false, a ValueError is raised.

- The comparision methods like equality, greater than or less than are compared as if classes were a tuple of its fields

- frozen: If true (the default is False), assigning to fields will generate an exception. This emulates read-only frozen instances. If either __getattr__ or __setattr__ is defined in the class, then ValueError is raised. See the discussion below.

- if eq and frozen are false then dataclass will generate `__hash__` for you, if eq is true and frozen is false then class is unhashable, if eq is false the superclass's `__hash__` is used, if the superclass is object then will do id-based hashing.

- **TypeError will be raised if a field without a default value follows a field with a default value.**, for example:
```
@dataclass
class C:
    a: int       
    b: int = 0
# generated repr:
# def __init__(self, a: int, b: int = 0):
OK

```
```
@dataclass
class C:
    a: int = 0       
    b: int
# generated repr:
# def __init__(self, a: int = 0, b: int):
BAD
```

- To configure fields of dataclass, use field() function, signature is below:
```
def field(*, default=MISSING, default_factory=MISSING, repr=True,
          hash=None, init=True, compare=True, metadata=None)
```
It is an error to specify both default and default_factory.

- **the field call itself replaces the normal position of the default value.**

- A field should be considered in the hash if it’s used for comparisons, Setting hash's value to anything other than None is discouraged.

- **metadata:** This can be a mapping or None. None is treated as an empty dict. This value is wrapped in types.MappingProxyType to make it read-only, and exposed on the Field object. It is not used at all by Data Classes, and is provided as a third-party extension mechanism. Multiple third-parties can each have their own key, to use as a namespace in the metadata.
