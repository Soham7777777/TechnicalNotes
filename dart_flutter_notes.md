# Flutter Notes
---

- learn about bang operator from here : [Stackoverflow explains bang in dart](https://stackoverflow.com/questions/67667071/understanding-bang-operator-in-dart)

### Some Basic commands in vs code:
---
- ctrl + shift + p then type "Flutter" to get full list of available commands/actions

## Basics:
---

- The runApp function take a widget as argument and makes it root widget of the widget tree.

- flutter forces root widget to cover the screen.

- All widgets are subclasses of either StatelessWidget or StatefulWidget.

- Every Widget impliments a build() function which returns other widgets (their children).

- The most basic and powerful widget are Text, Row, Column, Stack and Container.

### ->  Row, Column, Stack and Container:

- The design of these objects is based on the web’s flexbox layout model.

- Stack is like z-axis, they are based on web's absolute positoning layout model.

- The Container widget lets you create a rectangular visual element. A container can be decorated with a BoxDecoration, such as a background, a border, or a shadow. A Container can also have margins, padding, and constraints applied to its size. In addition, a Container can be transformed in three-dimensional space using a matrix.

### -> MaterialApp:
---


- Its a widget that has some predefined customizable theme inside it.

- a theme is just a key-value pairs that dictate the appearance in app.

- the **ThemeData** is the class that holds these properties.

- learn about it from [here](https://www.christianfindlay.com/blog/flutter-mastering-material-design3).


## Layouting:
---

- Constrains is a set of 4 variables: min and max height, min and max width
- Constarains go down, sizes go up, parent sets positions.

### Container:

- The container will become as big as possible when there is **no child and no size specifed**.
- It becoms its child size when it has child, but no sizes.
- If Container has paddings than it will exapand from its original size to provide padding to its child, if there is no room to expand then it will decrese its child's constrains.
- If a container has a child as container, then child will expand to its parent size, even if sizes are given, for example:
```dart 
    Center(
      child: Container(
        color: Colors.amber,
        width: 300,
        height: 300,
        child: Container(
          color: Colors.red,
          height: 200,
          width: 200,
        ),
      ),
    )
```

### ConstrainedBox: 
- The ConstrainedBox imposes **additional** constraints from its constraints parameter onto its child.

### UnconstrainedBox:
- The UnconstrainedBox lets its child be any size it wants. The difference between center and unconstrained_box is that center will pass constrains of 0 to its maximum size however Unconstrainedbox will allow any size even infinity in which case error will occure.

### OverflowBox:
- OverflowBox lets its child be any size it wants.
- OverflowBox is similar to UnconstrainedBox; the difference is that it won't display any warnings if the child doesn't fit the space.

### LimitedBox:
- When the LimitedBox is given an infinite size by the UnconstrainedBox; it passes a maximum width down to its child. its limit is only applied when it gets infinite constraints.

### FittedBox:
- The FittedBox lets the Child be any size it wants, but after the Child tells its size to the FittedBox, the FittedBox scales the Child until it fills all of the available width.
