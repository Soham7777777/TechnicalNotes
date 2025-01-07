## **Basic Java notes**:
---

---

### [Autoboxing and Unboxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html):

- Autoboxing is the automatic conversion that the Java compiler makes between the primitive types and their corresponding object wrapper classes. For example, converting an int to an Integer, a double to a Double, and so on. If the conversion goes the other way, this is called unboxing.

- The Java compiler applies autoboxing when a primitive value is:
	- Passed as a parameter to a method that expects an object of the corresponding wrapper class.
	- Assigned to a variable of the corresponding wrapper class.


- The Java compiler applies unboxing when an object of a wrapper class is:
	- Passed as a parameter to a method that expects a value of the corresponding primitive type.
	- Assigned to a variable of the corresponding primitive type.
---
### Generics:

- [Official java tutorial on generics](https://docs.oracle.com/javase/tutorial/java/generics/index.html)

- [Generics tutorial by Gilad Bracha](https://docs.oracle.com/javase/tutorial/extra/generics/index.html)

- A type in java is **either a class or an interface**.

- A Generic type in java is **either a class or an interface** which is parameterized over types.

- Purpose of generics is to **take types as parameters when defining classes, <u>methods</u> or interface**.

- Java applies **strong type checking at compile time** for generic code.

- The type parameter maybe any non-primitive type.

- We can reference generic type by generic type invocation, its like oridanry method invocation but the arguments are types. An **invocation of a generic type is generally known as a parameterized type**.
	- `Box<Integer> integerBox;`
	
- **Many developers use the terms "type parameter" and "type argument" interchangeably, but these terms are not the same. When coding, one provides type arguments in order to create a parameterized type. Therefore, the T in Foo<T> is a type parameter and the String in Foo<String> f is a type argument. This lesson observes this definition when using these terms.**

- When type arguments are ommited for generic types then it is called **raw type for that generic type.** <u>However, non-generic type is not a raw type.</u>

- A parameterized type can be assigned to its raw type but a raw type cannot be assigned to its parameterized type.

- The term "unchecked" means that the compiler does not have enough type information to perform all type checks necessary to ensure type safety. The "unchecked" warning is disabled, by default, though the compiler gives a hint. To see all "unchecked" warnings, recompile with -Xlint:unchecked.

- The type parameters can be bounded via extends keyword (works for both class and interface)
	- `public <U extends Number> void inspect(U u) {...}`

- bounded type parameters allow you to invoke methods defined in the bounds.

- for example here `n.intValue()` can be invoced because type is bounded to Integer.
```
public class NaturalNumber<T extends Integer> {

    private T n;

    public NaturalNumber(T n)  { this.n = n; }

    public boolean isEven() {
        return n.intValue() % 2 == 0;
    }

    // ...
}
```
- We can also have type parameter with multiple bounds:
```
class D <T extends A & B & C> { /* ... */ }
```
- if there is class as bound then it must be first.

#### Type Inference:
---

- Type inference is a Java compiler's ability to look at each method invocation and corresponding declaration to determine the type argument (or arguments) that make the invocation applicable. The inference algorithm determines the types of the arguments and, if available, the type that the result is being assigned, or returned. Finally, the inference algorithm tries to find the most specific type that works with all of the arguments.
