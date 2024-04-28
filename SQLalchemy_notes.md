# SQLAlchemy Notes:
---

- It is persisted of two distinct API: Core and ORM
	- The <u>Core API</u> is foundetional architecture for <mark>`Database toolkit`</mark>, which is used to **manage database connectivity, programatic construction of SQL and intraction with queries and results.**it is imported from <u>sqlalchemy</u> namespace.
	
	- The <u>ORM API</u> builds upon core and provides **Object Relational Mapping** functionalities. it has object persistence machenism known as <mark>`Session`</mark>.it also allows custom classes to be mapped with tables.it is imported from <u>sqlalchemy.orm</u> namespace.

## Getting Started:
---

### Engine:-
- It is typically a global object that is <u>created once for a database server</u>, it provides **connection pool** and **factory**.
```python
>>> from sqlalchemy import create_engine
>>> engine = create_engine("sqlite+pysqlite:///:memory:", echo=True)
```

#### Anatomy of connection string:
- it consists of 3 information to create database.
	- which type of database (here sqlite)
	- which DBAPI (here pysqlite)
	- how we want to create/store database (here in memory)

- The echo parameter is used to log the output query to stdout.

---

### Transactions and DBAPI:-
- In this section we mainly discuss about three objects **Connection, Session and Result**.


#### **Connection**:
```python3
>>> from sqlalchemy import text

>>> with engine.connect() as conn:
...     result = conn.execute(text("select 'hello world'"))
...     print(result.all())
```

- It is a unit of connectivity with database, the `text()` construct is used to execute SQL statements.

- By default a transaction is always in progress, it is rollbacked when context manager(with statement) is closed, use `Connection.commit()` to commit the transaction.

- the result of SELECT in execute method returns the Result object, <mark>It is best to use the Result object inside context manager and not passed outside.</mark>

- The above style is known as "Commit as you go".

- There is also another style "Begin once", where the context manager is works both as connection and single transactional block where commit is emmited when context is closed.
```python
>>> with engine.begin() as conn:
...     result = conn.execute(text("SELECT x, y FROM some_table"))
...     for row in result:
...         print(f"x: {row.x}  y: {row.y}")
```

#### **Result**:

- The Result object which is returned upon execution of SELECT statement.

- <u>The Result is orderd collection of Row objects and Row objects are made to look like namedtupel. we can also use Result.mappings() to get RowMapping objects which are like python's mapping (read-only version of dict)</u>

#### **Session**:

- The <u>fundamental transactional / database </u> interactive object when using the ORM is called the Session.

```python
>>> from sqlalchemy.orm import Session

>>> stmt = text("SELECT x, y FROM some_table WHERE y > :y ORDER BY x, y")
>>> with Session(engine) as session:
...     result = session.execute(stmt, {"y": 6})
...     for row in result:
...         print(f"x: {row.x}  y: {row.y}")
```

- Here we directly replace the call to with engine.connect() as conn with with Session(engine) as session.

- The Session doesn’t actually hold onto the Connection object after it ends the transaction. It gets a new Connection from the Engine the next time it needs to execute SQL against the database.

- Session is a place where mapped object can be added but they are not actually persisted on database until you commit/flush the session.

---

### MetaData, Table and Column:-

- Here we discuss about **SQL Expression Language** which allows for fluent, composable construction of SQL queries.

- In order to describe database objects in terms of python classes we have three main objects written in title for this section.

#### **Metadata**:
- It is a **facade around python dictionary**, it is used to store Tables with key being their name.

- It is used to **group all tables**. 
```python
>>> from sqlalchemy import MetaData
>>> metadata_obj = MetaData()

>>> from sqlalchemy import Table, Column, Integer, String
>>> user_table = Table(
...    "user_account",
...    metadata_obj,
...    Column("id", Integer, primary_key=True),
...    Column("name", String(30)),
...    Column("fullname", String),
	 )

>>> user_table.c.name

>>> user_table.c.keys()

>>> from sqlalchemy import ForeignKey
>>> address_table = Table(
...     "address",
...     metadata_obj,
...     Column("id", Integer, primary_key=True),
...     Column("user_id", ForeignKey("user_account.id"), nullable=False),
...     Column("email_address", String, nullable=False),
... )

>>> metadata_obj.create_all(engine)
```
- <u>When using the ForeignKey object within a Column definition, we can omit the datatype for that Column;</u> it is automatically inferred from that of the related column.

- **Summary**: A Metadata object is collection of Table objects, Table object is a collection of Column and Constraint objects.

---

### Using ORM Declarative Forms to Define Table Metadata:-

- This section discusses the ORM pattern to create our database schema.

#### **DeclarativeBase class**:

- The **Declarative Table** process achives same goals as buliding Table objects but it also gives us an **ORM mapped class** also known as mapped class.

- When using the ORM, the **MetaData** collection remains present, however it itself is associated with an **ORM-only construct commonly referred towards as the Declarative Base**.
```python
>>> from sqlalchemy.orm import DeclarativeBase
>>> class Base(DeclarativeBase):
...     pass

>>> Base.metadata
OUT: MetaData()
```

#### **Mapped classes**:

- We can now use our Base class as super class for ORM mapped classes for user and address tables.

```python
from typing import List
from typing import Optional
from sqlalchemy.orm import Mapped
from sqlalchemy.orm import mapped_column
from sqlalchemy.orm import relationship

class User(Base):
    __tablename__ = "user_account"
    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(30))
    fullname: Mapped[Optional[str]]
    addresses: Mapped[List["Address"]] = relationship(back_populates="user")
    def __repr__(self) -> str:
        return f"User(id={self.id!r}, name={self.name!r}, fullname={self.fullname!r})"

class Address(Base):
    __tablename__ = "address"
    id: Mapped[int] = mapped_column(primary_key=True)
    email_address: Mapped[str]
    user_id = mapped_column(ForeignKey("user_account.id"))
    user: Mapped[User] = relationship(back_populates="addresses")
    def __repr__(self) -> str:
        return f"Address(id={self.id!r}, email_address={self.email_address!r})"
```

The two classes above, **User and Address, are now called as ORM Mapped Classes**, and are available for use in ORM persistence and query operations,

- Use of explicit typing annotations for mapped_column is **completely optional**.

- Visit <a href"https://docs.sqlalchemy.org/en/20/tutorial/metadata.html#declaring-mapped-classes">here</a> to read details about mapped classes.

Must read : https://docs.sqlalchemy.org/en/20/tutorial/metadata.html#using-orm-declarative-forms-to-define-table-metadata

#### **Relationship construct**:

- In a declarative mapped class, we can specify another type of property which can be obtained by "relationship()" construct. lets call this class "host" which has a relationship field.

- This new type of field is will not become a column in the table.

- The datatype for this field has to be the class name of declarative mapped class or collection of it i.e. `list["User"]`, here lets call it "client" class. the client must have a foreighn key of host class.

- So whenever an object will be assigned/appended to that field and commited, all the client objects will also be commited in database.

```python
class Parent(Base):
    __tablename__ = "parent_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    children: Mapped[list["Child"]] = relationship()


class Child(Base):
    __tablename__ = "child_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    parent_id: Mapped[int] = mapped_column(ForeignKey("parent_table.id"))
    


if __name__ == '__main__':
    engine = create_engine('sqlite:///example.db')
    session = Session(engine)
    Base.metadata.create_all(engine)
    
    
    parent1 = Parent()
    child1 = Child()
    child2 = Child()
    
    parent1.children += [child1,child2]
    
    print(parent1.children)
    
    # child1 and child2 will be automatically commited
    session.add(parent1)
    session.commit()
``` 

- simmilarly, we can use relationship to client class as well:

```python
class Parent(Base):
    __tablename__ = "parent_table"
    id: Mapped[int] = mapped_column(primary_key=True)


class Child(Base):
    __tablename__ = "child_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    parent_id: Mapped[int] = mapped_column(ForeignKey("parent_table.id"))
    parent: Mapped["Parent"] = relationship()
    


if __name__ == '__main__':
    engine = create_engine('sqlite:///example.db')
    session = Session(engine)
    Base.metadata.create_all(engine)
    
    parent1 = Parent()
    child1 = Child()
    child2 = Child()
    
    child1.parent = parent1
    child2.parent = parent1
    
# the parent1 will be automatically commited only once 
    session.add_all([child1,child2])
    session.commit()
```

- If we want to use relationship on both client and host class then if we update the relationship field for one of them then other class's field should also be automatically updated, for this "syncronized" behaviour we use "back_populates" property in relationship() construct.

- the value of back-populates must be a field of class that is on the other side of relationship.

- for example:

```python
class Parent(Base):
    __tablename__ = "parent_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    children: Mapped[list["Child"]] = relationship()


class Child(Base):
    __tablename__ = "child_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    parent_id: Mapped[int] = mapped_column(ForeignKey("parent_table.id"))
    parent: Mapped["Parent"] = relationship()
    

if __name__ == '__main__':
    engine = create_engine('sqlite:///example.db')
    session = Session(engine)
    Base.metadata.create_all(engine)
    
    parent1 = Parent()
    child1 = Child()
    child2 = Child()
    
    # this will not update parent1.children list, however logically it should
    child1.parent = parent1
    child2.parent = parent1

    print(parent1.children) # prints [] but expects a list with two child objects
    
# this will not commit both child objects since they are not part of children list
    session.add(parent1) 
    session.commit()
```

- example which uses "back-populates" :

```python
class Parent(Base):
    __tablename__ = "parent_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    children: Mapped[list["Child"]] = relationship()


class Child(Base):
    __tablename__ = "child_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    parent_id: Mapped[int] = mapped_column(ForeignKey("parent_table.id"))
    parent: Mapped["Parent"] = relationship(back_populates='children')
    

if __name__ == '__main__':
    engine = create_engine('sqlite:///example.db')
    session = Session(engine)
    Base.metadata.create_all(engine)
    
    parent1 = Parent()
    child1 = Child()
    child2 = Child()
    
    # this will now add both child to parent1.children list
    child1.parent = parent1
    child2.parent = parent1

    print(parent1.children) 
    
    # all 3 objects will be commited
    session.add(parent1) 
    session.commit()
    
    # but since back_populates isn't defined for parent class therefore it will not reflect the changes
    parent2 = Parent()
    child3 = Child()
    parent2.children.append(child3)
    
    # prints None
    print(child3.parent)
    
    # it will cause error since child3 has no parent associeted with it
    session.add(child3)
    session.commit()
```

- Thus we need to specify back-populates for both sides of relationship:


```python
class Parent(Base):
    __tablename__ = "parent_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    children: Mapped[list["Child"]] = relationship(back_populates="parent")


class Child(Base):
    __tablename__ = "child_table"
    id: Mapped[int] = mapped_column(primary_key=True)
    parent_id: Mapped[int] = mapped_column(ForeignKey("parent_table.id"))
    parent: Mapped["Parent"] = relationship(back_populates='children')
    

if __name__ == '__main__':
    engine = create_engine('sqlite:///example.db')
    session = Session(engine)
    Base.metadata.create_all(engine)
    
    parent1 = Parent()
    child1 = Child()
    child2 = Child()
    
    child1.parent = parent1
    child2.parent = parent1

    print(parent1.children) 
    
    session.add(parent1) 
    session.commit()
    
    parent2 = Parent()
    child3 = Child()
    parent2.children.append(child3)
    
    # prints parent2 object 
    print(child3.parent)
    
    # both child3 and parent2 are commited    
    session.add(child3)
    session.commit()
```
---

##### **Important terminology while using relationship:**

- In the above example we can say that "Parent class has one-to-many relationship defined on children field" and simmilarly "Child class has many-to-one relationship defined on parent field".

---

- When deleting the parent object we also want to automatically delete all children specified in one-to-many relationship, for that we have another property "cascade" which should be give value "all, delete-orphan".

- If we want to allow null then allow it for both foreign key column and relationship field.

- Other things can be found [here](https://docs.sqlalchemy.org/en/20/orm/basic_relationships.html#one-to-one)

- A sqlite row can have maximum size upto 2 GB while a mysql row can only have maximum size upto 65535 bytes, but mysql longblob type can strore upto 4GB of data.


 
