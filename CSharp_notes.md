# C# Notes:

---

## Entry Point:

A C# Project can have either `Main` functions as entry point or a file at root of the project which contains `Top-Level Statements` can be the entry point.

- There can be only one file which can contain top-level statements.
- There can be my files with Main function but only one of those executed. In order to specify which class's Main to use, add a `StartupObject` to \<proj\>.csproj file inside `PropertyGroup` like this:  \<StartupObject\>MyNamespace.MyClass\</StartupObject\>
- If the class is inside the global namespace then just using class name in `StartupObject` will be enough.



## Java Pakcages (For Comparision with namespaces):

In a java source file, only one type (classes, interfaces, enumerations, and annotation types) can be public which must have same name as source file.

All types in a package can access other types with their fully qualified name. Or import tham to use simple names of types.

The name of the package at the root direcotry is "" (empty). This is known as default package. **You can't import a type from unnamed package.**

If source files are inside a directory than all source files must use `package <directoryname>;` as first line of code to organize those public types inside package, now it can be access via `directoryname.typename`, or import it to just use `typename`.   



## Namespace:

Classes that inside of source files at the root of project directory are inside `global` namesapace. Thus access tham using `global::<classname>` from anywhere inside project.

If there are classes that are inside a directory than it must be included inside the namespace of `<foldername>` just like packages.

Otherwise, we can create any amount of nested namespaces and and anywhere in the project, just use fully qualified name or use `using` keyword to use simple type names. 

But its good to organize namespaces aligned with direcotry structure.



## Type System:

Every method declaration specicfies a name, type and kind (value, reference or output) for each input parameter and for return value.

The compiler embeds the type information into the executable file as  metadata. The common language runtime (CLR) uses that metadata at run  time to further guarantee type safety when it allocates and reclaims  memory.

A type stores this information:

- The storage space that a variable of the type requires.
- The maximum and minimum values that it can represent.
- The members (methods, fields, events, and so on) that it contains.
- The base type it inherits from.
- The interface(s) it implements.
- The kinds of operations that are permitted.

When you declare a variable or constant in a program, you must either specify its type or use the `var` keyword to let the compiler infer the type.

There are value types and reference types. See all kinds of types from  [here](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/built-in-types).

A *type conversion* that doesn't cause data loss is performed automatically by the compiler. A conversion that might cause data loss requires a *cast* in the source code.

Custom types can be crated by using one of these: `struct`, `class`, `interface`, `enum` and `record`. By default, the most frequently used types in the class library are available in any C# program. The [.NET class library](https://learn.microsoft.com/en-us/dotnet/standard/class-library-overview) itself is a collection of custom types.

The .NET type system contains a CTS (Common Type System) which supports principle of inheritance. All types are ultimately derived from System.Object (c# `object` keyword).

**Each type in the CTS is defined as either a *value type* or a *reference type*. These types include all custom types in the .NET class library and also your own user-defined types. Types that you define by using the `struct` keyword are value types; all the built-in numeric types are `structs`. Types that you define by using the `class` or `record` keyword are reference types. Reference types and value types have  different compile-time rules, and different run-time behavior.**

![type_chart](/home/soham/TechnicalNotes/images/CSharp_notes/type_chart.png)

A struct is a value type. When a struct is created, the variable to  which the struct is assigned holds the struct's actual data. When the  struct is assigned to a new variable, it's copied. The new variable and  the original variable therefore contain two separate copies of the same  data. Changes made to one copy don't affect the other copy.

Record types may be either reference types (`record class`) or value types (`record struct`). Record types are data structures with additional compiler synthesized  members. Records typically store data that isn't intended to be modified after the object is created.

The memory for a struct is allocated inline in whatever context the  variable is declared. There's no separate heap allocation or garbage  collection overhead for value-type variables.

Value types are *sealed*. You can't derive a type from any value type.

Two categories of value type:

	1. struct:
	- Built-in numeric types are structs.
	- You use the `struct` keyword to create your own custom value types.
	- A struct can implement one or more interfaces. You can cast a struct type to any interface type that it implements.
	2. enum:
	- An enum defines a set of named integral constants.
	- You cannot define a method inside the definition of an enumeration type. To add functionality to an enumeration type, create an [extension method](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods).
	- The default value of an enumeration type `E` is the value produced by expression `(E)0`, even if zero doesn't have the corresponding enum member.
	- You use an enumeration type to represent a choice from a set of mutually exclusive values or a combination of choices.

Two Important funcionalites:

- `typeof()`: The `typeof` operator obtains the [System.Type](https://learn.microsoft.com/en-us/dotnet/api/system.type) instance for a type. The argument to the `typeof` operator must be the name of a type or a type parameter.
- `sizeof():` The `sizeof` operator returns the number of bytes occupied by a variable of a given type.

---

### Boxing and Unboxing:

Boxing is the process of converting a value type to the type `object` or to any interface type implemented by this value type. Unboxing extracts the value type from the object. Boxing is implicit;  unboxing is explicit. The concept of boxing and unboxing underlies the  C# unified view of the type system in which a value of any type can be  treated as an object.

Boxing example:

```C#
int i = 123;
// The following line boxes i.
object o = i;
```

Unboxing Example:

```C#
o = 123;
i = (int)o;  // unboxing
```

In relation to simple assignments, boxing and unboxing are  computationally expensive processes. When a value type is boxed, a new  object must be allocated and constructed. To a lesser degree, the cast  required for unboxing is also expensive computationally.

Boxing is used to store value types in the garbage-collected heap. Boxing is an implicit conversion of a `value type` to the type `object` or to any interface type implemented by this value type. Boxing a value type allocates an object instance on the heap and copies the value into the new object.

![](/home/soham/TechnicalNotes/images/CSharp_notes/boxing-operation-i-o-variables.gif)

Unboxing operation consist of:

- Checking the object instance to make sure that it is a boxed value of the given value type.
- Copying the value from the instance into the value-type variable.

---

### Referance Types:

When declaring a variable of a `reference type`, it contains the value `null` until you assign it with an instance of that type or create one using the `new` operator.

**When the object is created, the memory is allocated on the managed heap. The variable holds only a reference to the location of the object.  Types on the managed heap require overhead both when they're allocated  and when they're reclaimed. *Garbage collection* is the automatic memory management functionality of the CLR, which performs the  reclamation. However, garbage collection is also highly optimized, and  in most scenarios it doesn't create a performance issue. **

All arrays are reference types, even if their elements are value types. Arrays implicitly derive from the `System.Array` class.

Because literals are typed, and all types derive ultimately from `System.Object`, you can write and compile code such as the following code:

```C#
string s = "The answer is " + 5.ToString();
// Outputs: "The answer is 5"
Console.WriteLine(s);

Type type = 12345.GetType();
// Outputs: "System.Int32"
Console.WriteLine(type);
```

---

### Generic Types:

A type can be declared with one or more *type parameters* that serve as a placeholder for the actual type (the *concrete type*). Client code provides the concrete type when it creates an instance of the type. Such types are called *generic types*.

Generic collection classes are called *strongly typed collections* because the compiler knows the specific type of the collection's elements and can raise an error at compile time. Generic type parameters are replaced with the type arguments during compilation.

---

### Implicitly typed local variables

You can implicitly type a local variable (but not class members) by using the `var` keyword. The variable still receives a type at compile time, but the type is provided by the compiler.

The `var` keyword instructs the compiler to infer the type of the variable from the expression on the right side of the  initialization statement. The inferred type may be a built-in type, an  anonymous type, a user-defined type, or a type defined in the .NET class library.

The `var` keyword may be used in the following contexts:

- On local variables (variables declared at method scope) as shown in the previous example.
- In a `for` initialization statement.
- In a `foreach` initialization statement.
- In a `using` statement.

In many cases the use of `var` is optional and is just a  syntactic convenience. However, when a variable is initialized with an  anonymous type you must declare the variable as `var` if you need to access the properties of the object at a later point. 

[Read This](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/implicitly-typed-local-variables#remarks)

---

### Anonymous types:

Anonymous types provide a convenient way to encapsulate a set of  read-only properties into a single object without having to explicitly  define a type first. The type name is generated by the compiler and is  not available at the source code level. The type of each property is  inferred by the compiler.

Example:

```c#
var v = new { Amount = 108, Message = "Hello" };

// Rest the mouse pointer over v.Amount and v.Message in the following
// statement to verify that their inferred types are int and string.
Console.WriteLine(v.Amount + v.Message);
```

Anonymous types contain one or more public read-only properties. No  other kinds of class members, such as methods or events, are valid. The  expression that is used to initialize a property cannot be `null`, an anonymous function, or a pointer type.

[Read This](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/anonymous-types)

---

### Nullable value types:

Ordinary value types can't have a value of `null`. However, you can create *nullable value types* by appending a `?` after the type. For example, `int?` is an `int` type that can also have the value `null`. Nullable value types are instances of the generic struct type `System.Nullable`. Nullable value types are especially useful when you're passing data to and from databases in which numeric values might be `null`.

---

### Runtime types and Compile time types:

The *compile-time type* is the declared or inferred type of the variable in the source code. The *run-time type* is the type of the instance referred to by that variable.

For example:

```c#
string message = "This is a string of characters";
```

Above, both compile time and run time types are string.

```c#
object anotherMessage = "This is another string of characters";
IEnumerable<char> someCharacters = "abcdefghijklmnopqrstuvwxyz";
```

Above, both variables have run-time type as `string`. However, `anotherMessage` has compile time type as `object` and `someCharacters` have compile time type as `IEnumerable<char>`.

If the two types are different for a variable, it's important to  understand when the compile-time type and the run-time type apply. The  compile-time type determines all the actions taken by the compiler.  These compiler actions include method call resolution, overload  resolution, and available implicit and explicit casts. The run-time type determines all actions that are resolved at run time. These run-time  actions include dispatching virtual method calls, evaluating `is` and `switch` expressions, and other type testing APIs. To better understand how your code interacts with types, recognize which action applies to which  type.

---

