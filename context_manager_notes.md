# **Context Manager**
---

- An application is just an infinite loop that listens to external events and responses accordingly. This could be example of a server listening to http requests, a game loop listening to user events, a display loop that draws objects on screen at constant rate, it could be anything that is "alive" and solves multiple copies of a problem. While in contrast an algorithm is not infinite. The program is the specification of how everything will work for given system. The program is written with code which is an interface between human language and how computer understands.

- When inside this application loop needs to load objects in memory, then it also needs to be destroyed at some point other wise infinite loop will keep loading objects but no destruction of tham will lead to full main memory. This concept is called memory leak. This can also occure when an exception occures before closing the resource, and while handling the exception the application skips the closing part, but that can be prevented in finally statement.

- To prevent momory leak and setup needs to done before loading objects, the python introduces Context manager. As name suggests they manages Contexts(resources/objects that needs to be loaded in memory). With context manager we can specify the setup and teardown of resouces which will happen when ever a "with block" starts and ends respectively.

- General syntax is:
```python
with expression as target_var:
    do_something(target_var)
```

- The expression will return the object which is our context and its bind to target_var.

- for custome decorators we define two magic methods: `__enter__(self)` and `__exit__(self,exc_type, exc_value, exc_tb)`. the exit method will called when with block completes, if the expection has occured in with block then its type, value and traceback will be captured in exit method. The enter method returns the object to work with inisde the with block and can be bind to target var.

- example:
```python
class WritableFile:
    def __init__(self, file_path):
        self.file_path = file_path

    def __enter__(self):
        self.file_obj = open(self.file_path, mode="w")
        return self.file_obj

    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file_obj:
            self.file_obj.close()

with WritableFile("hello.txt") as file:
	file.write("Hello, World!")

```

## **yield keyword**

- yield does not end the function it returns the generator which will return yield value and the code after yield will be executed as well.

- For example take a look at the code below:
```python
def myFunc1():
    print('before yield')
    yield 5
    print('after yield')

def myFunc2():
    print('before yield')
    yield 5
    print('after first yield')
    yield 8
    print('after both yield')

def myFunc3():
    print('before yield')
    for i in range(10):
        yield i+1
        print('after each yield')
    
    print('after all yield')

gen1 = myFunc1()

print(next(gen1))
print(next(gen1, -1))

print('--------------------------------------------------')

gen2 = myFunc2()
print(next(gen2))
print(next(gen2))
print(next(gen2,-2))

print('--------------------------------------------------')

gen3 = myFunc3()
while (val:=next(gen3,-3)) != -3:
    print(val)
    
print('--------------------------------------------------')    

gen33 = myFunc3()
print([*gen33])
```
