## Dart Notes:
---

- A humble beginning to application development.

### **Syntax Basics:**

#### **Variables**:

- Dart does **not lets you observe(print/use) non-nullable uninitialized variables**.

- Dart does not allow to access properties/methods on nullable type unless they are supported by null like toString() and hashCode().

- Every data type in dart are objects.

- **Top-level (file-level/global) and class variables(static variables) are lazily initialized**; the initialization code runs the first time the variable is used.

- If you're sure that a variable is set before it's used, but Dart disagrees, you can fix the error by marking the variable as late, if you fail to initialize late variable then dart will throw runtime error.

- For example:
```dart
	late String description;

	void main() {
	  description = 'Feijoada!';
	  print(description);
	}
	// here dart will throw compile time error if description is not late, because dart can't figure out that description will be initialize later. 
```

```dart
	String f = "lol"; // top level variable
	void main() {
	  late String x = f;

	  f = "hello";

	  print(x);
	}
	// output is "hello", because late variables initialized when its used, if the x wasn't late then output would be "lol"
```

- A final variable can only be set once, it can be set lazily.

- A const variable must be initialized because its compile time constant.

- For example:
```dart
	void main() {
	  final int x;
	  x = 5;
	  print(x);
	}
	// output is 5, its valid to set final variables lazily, if x was const then dart would throw error that x must be initalized.
```

- **Instance variable can be final but not const**, because instances(objects) of a class always created at runtime.

- By default class variables (static variables) are not initialized until they'r used, but by making tham static const, the variable is initialized at compile time.

- final variables can change its fields, but cannot be modified.

- const variables can be used as values as well.

- For example:
```dart
	var foo = const [];
	final bar = const [];
	const baz = [];
	// here the foo can be modified later like foo = [1,2,3], but you cannot modify foo's value, for example appending something to foo.
	// the baz however cannot be modified, as well as its value cannot be modified as well
``` 
```dart
	void main() {
	  final baz = [];
	  baz.add(5);
	  print(baz[0]);
	}
	// here this is valid since final variable is runtime thing thus the baz is not really modified while we adding 5 to list, if the baze were const than, it would throw error while adding 5 because const variable's values are determined at compiletime and cannot be changed.
```
```dart
	int x = 6;
	int f = 7;
	void main() {
	  late final int baz = x;
	  x = f;
	  print(baz);
	}
 	// output is 7, its suggests that if finals are late then it will be lazily initialized only once. 
```
```dart
	var x = const [1, 2, 3];
	var f = [6, 7, 8];
	void main() {
	  late final baz = x;
	  x = f;
	  print(baz);
	}
	// the output will be [6,7,8]
```
- type checking, type casting, collection if and spreading are determined at compile time thus they can be const.

#### **Summary**:
- const : **const vaiables are determined at compile time means its set before even running the code, and cannot be changed, that's why instance variables cannot be const and const variables cannot change its fields, the values can be const.**

- final : **finals are determined at runtime, final variables cannot be reassinged, finals can be late thus can be initialized lazily(reassigning and lazily initializing are both different thing).**

---

#### **Operators**:

- When you use operators, you create expressions.

- For operators that take two operands, the leftmost operand determines which method is used. For example, if you have a Vector object and a Point object, then aVector + aPoint uses Vector addition (+).

- In the rare case where you need to know whether two objects are the exact same object, use the identical() function instead.

- The result of obj is T is true if obj implements the interface specified by T. For example, obj is Object? is always true.

```dart
var paint = Paint()
  ..color = Colors.black
  ..strokeCap = StrokeCap.round
  ..strokeWidth = 5.0;
 // the above is equvivalent to:
 
var paint = Paint();
paint.color = Colors.black;
paint.strokeCap = StrokeCap.round;
paint.strokeWidth = 5.0;

// in the code below if the object for which we are accessing properties/methods can be null then use this:

querySelector('#confirm') // Get an object.
  ?..text = 'Confirm' // Use its members.
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'))
  ..scrollIntoView();
```
