# Mlox Extended

## Extended Features
### tail call optimization
```js
tailCall = [
    "fun f(n, acc) {",
    "  if (n <= 1) return acc;",
    "  return f(n - 1, acc + 1);",
    "}",
    "var s = clock();",
    "print f(1000, 0);",
    "print clock() - s;",
];
runSource(tailCall.join(char(10)))
```
Original: **0.971s**

TailCall Optimized: **0.521s**
### list expression
```js
var l = [1,2,3,4];
```
### map expression
```js
var m = {1:1, "key":"value", l:"object"};
```
### subscript
```js
l = l[1];
```
### slice

```js
l = l[1:2];
```

## Tips
If the function name is XXXDoSomething, you will most likely need to pass in an instance of XXX.

eg: 
```lua
IntrpResolve(intrp, xxx, xxx)
```

Since most of objects are list, you have to use index to access their member.

The index of each memeber is defined at NewXXXX,

eg: 
```
a = NewInterpreter
a[0] // namely a.globals
```

## Add Custom Intrinsic
```lua
clockIntrinsic = function(intrp, arguments)
    return time
end function
AddIntrinsic("clock", 0, @clockIntrinsic)
```
AddIntrinsic takes three arguments
- name: intrinsic function name 
- arity: number of arguments
- callable: function to call, must accept 2 parameters.

**intrp**: Lox Interpreter
**argumments**: List of arguments

Here are possible types of each arugment:

| Type in Lox | Type in Miniscript |
| ----------- | -------            |
| string      | string             |
| number      | number             |
| nil         | null               |
| bool        | number             |
| Class       | list from NewLoxClass   |
| Instance    | list from NewLoxInstance|
| Function    | list from NewLoxFunction|

| Extended Type | Type in Miniscript |
| -----------   | -------            |
| Intrinsic     | list from  AddIntrinsic|
| List          | work in progress       |
| Map           | work in progress       |

For simple lox type variables (string, number ....), you can simply treat them as normal miniscript type.

Under most situations, you will not use intrp.

Define intrinsic function below will not work properly.

```lua
class = [
    "class Animal {",
    "  init(name) {",
    "    this.name = name;",
    "  }",
    "  speak() {",
    "    print this.name + "" makes a noise."";",
    "  }",
    "}",
    "var myAnimal = Animal(""Generic Animal"");",
    "myAnimal.speak();",
    "rename(myAnimal, ""dog"")",
]
renameIntrinsic = function(intrp, arguments)
    // Invalid!
    arguments[0].name = arguments[1]

    // arguments[0] looks like  "[""LoxInstance"", [""Animal"", ..., ...] , ....]"
    // but arguments[1] is indeed a miniscript string: "dog"
end function
AddIntrinsic("rename", 2, @feedIntrinsic)
runSource(fib.join(char(10)))
```
In order to properly handle it, you can use LoxInstanceSet(intrp, "name", arguments[1]).

Here are some more functions you might want to use:

- LoxInstanceGet
- LoxClassFindMethod
- LoxFunctionBind
- LoxCallableCall

## REPL
Call runPrompt()

## Run Source
Call runSource(source)

source is the string representation of the code.

NOTE: you can not use "\n", because it is actually 2 chars in miniscript.

You have to use char(10) instead.
```lua
fib = [
    "fun fib(n) {",
    "  if (n <= 1) return n;",
    "  return fib(n - 2) + fib(n - 1);",
    "}",
    " ",
    "var start = clock();",
    "print fib(15) == 610;",
    "print clock() - start;",
]
runSource(fib.join(char(10)))
```

## Create a Lox Interpreter
NOTE: interpreter is already exists in the global.
```lua
interpreter2 = NewInterpreter()
```