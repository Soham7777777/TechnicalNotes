## Notes on Lua:
--- 

#### **Chunk**:

- A chunk in lua is sequance of statements that are executed by lua.

- Chuck can be a statement in interactive mode, or a lua file. 

- lua is **data-description** language, thus chunks with several megabytes are not uncommon.

- In interactive mode, lua interprets each line as a complete chunk. However in some case a line cannot form a chunk (i.e. if-else ladders, function definations), then lua waits for chunk to complete in next line and thus changes its its prompt (i.e. >>) in interactive mode.

- We can execute sequance of chunks with `-l` option, for example: `lua -ltemp1 -ltemp2` it will first execute temp1.lua then execute temp2.lua and then **opens interactive mode**. The `-l` option actually calls `require` function.

- the `dofile` function will execute the file given in argument. For example: `dofile("temp1.lua")`

- lua creates the table `arg` with all the command line arguments.

#### **Global Variables**:

- Non-initialized variables are `nil` by default, assigning any global variable to `nil` will delete it. Global variables do not need declarations, you just assign tham values.

#### **Types and Values**:

- Lua is a dynamically typed language. There are no type definitions in the language; each value carries its own type.

- There are eight basic types in Lua: nil, boolean, number, string, userdata, function, thread, and table. The type function gives the type name of a given value.

- Both `false` and `nil` are considered as falsy conditions and all others are truthy conditions (even 0 and '').

- Strings are immutable in lua. Strings can be written in `''` or `""` or `[[]]`, the last one represents multiline strings and do not interpret escape sequance.

- If Numeric operations are applied to string then lua will try to convert string to number and opposite is true as well:
```lua
print("-5.3e-10"*"2")    --> -1.06e-09
print(10 .. 20)        --> 1020
```

- lua uses tables to represent packages. Table is the only data-structure in lua. Table members can be accessed either via index notation (a["x"]) or dotted notation (a.x).

- Functions are first-class values in Lua. That means that functions can be stored in variables, passed as arguments to other functions, and returned as results. Lua can call functions written in Lua and functions written in C. All the standard library in Lua is written in C.

- The userdata type allows arbitrary C data to be stored in Lua variables. It has no predefined operations in Lua, except assignment and equality test.

#### **Expressions**:

- Expressions denote values. Expressions in Lua include the numeric constants and string literals, variables, unary and binary operations, and function calls. Expressions can be also the unconventional function definitions and table constructors.

- Lua compares tables, userdata, and functions by reference, that is, two such values are considered equal only if they are the very same object.
