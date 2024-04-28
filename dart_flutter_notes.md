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

- Its really about writing more code, there isn't really much to record, just code.
