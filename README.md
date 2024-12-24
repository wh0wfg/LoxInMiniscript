# Mlox Extended

## Extended Features

### intrinsic functions

abs, acos, asin, atan, bitAnd, bitOr, bitXor, ceil, char, cos, oor, log, pi, range, round, rnd, sign, sin, sqrt, str, tan

Most of the intrinsic functions are simply wrappers around the [miniscript intrinsic function](https://miniscript.org/files/MiniScript-Manual.pdf#page=26&zoom=100,80,80)

However, it's important to note the distinction in Lox:
- a.len accesses the property of a.
- a.len() calls the method of a.
  
**NOTE:** Many intrinsic functions are
 not been tested and may contain errors or unexpected behaviors.

### unary operation
```c#
var a=1;
print a++; // 1
// --a, a--
print ++a; // 3
a -= 1; // +=, *=, /=, %=
print a; // 2
print a%2; // 0
```

### ternary conditional operation
```c#
var x = 3;
print x == 3 ? "x is 3" : "x is not 3";
```

### compound assignment
```c#
var x=3;
x *= 2; // +=, -=, /=, %= 
print x; // 6
```

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
original: **0.971s**

tail call optimized: **0.521s**

### try-catch
```C#
try {
  throw "error"; // you can throw any thing
} catch (var e) {
  print e;
}

try {
    a=a+1;
} catch (var e) {
    print e;
    print "catch it!";
} finally {
    var a="this is finally";
    print a;
}
```

### switch-case
```C#
var x = 1;
switch (x) {
    case 0: 
        print "x is 0";
    case 1: 
        print "x is 1";
        break;
    default: 
        print "x is not 0 or 1";
}
```

### lambda
```C#
var f = [lambda (x) {
    return x + 1;
}];
print f[0](1);
```

### else-if
```C#
var x = 3;
if (x == 0) {,
    print "x is 0";
} else if (x == 1) {
    print "x is 1";
} else {,
    print "x is not 0 or 1";
}
```

### foreach statement
```C#
var l = [1,2,3,4];
foreach (var i in l) {
    print i;
}
```

### do-while statement
```c#
var i = 0;
do {
    print i;
    i = i + 1;
} while (i <= 0);
```

### break statement
```c#
var l = [1,2,3,4];
foreach (var i in l) {
    print i;
    break;
}
```

### continue statement
```c#
var l = [1,2,3,4];
foreach (var i in l) {
    print i;
    continue;
    print -1;
}
```

### string intrinsics
| Function    | Description                                  |
|-------------|----------------------------------------------|
| len         | Gets the length of the string (property).    |
| upper()     | Converts the string to uppercase.           |
| lower()     | Converts the string to lowercase.           |
| trim()      | Removes leading and trailing whitespace.     |
| indexOf(value) | Finds the index of the first occurrence of a value. |
| replace(oldValue, newValue) | Replaces occurrences of a substring.  |
| split(separator) | Splits the string into parts based on a separator. |

### list
#### expression
```js
var l = [1,2,3,4];
```
#### operation
```c#
var l = [1, 2];
print l * 2; // [1, 2, 1, 2]
print l + [3, 4]; // [1, 2, 3, 4]
```
#### intrinsics
| Function    | Description                                  |
|-------------|----------------------------------------------|
| push(item)  | Adds an item to the list.                   |
| remove(item)| Removes the first occurrence of an item.    |
| hasIndex(index) | Checks if a specific index exists in the list. |
| sort()      | Sorts the list in ascending order.          |
| len         | Gets the number of elements in the list (property). |

### map 
#### expression
```js
var m = {1:1, "key":"value", l:"object"};
```
#### operation
```c#
var a = {"a": 1, "b": 2};
var b = {"a": 2, "c": 3, "d": 4};
print a + b; // {"a": 2, "b": 2, "c": 3, "d": 4}
```
#### intrinsics
| Function       | Description                             |
|----------------|-----------------------------------------|
| remove(key)    | Removes an entry by its key.            |
| hasIndex(key)  | Checks if a specific key exists in the map. |
| len            | Gets the number of key-value pairs in the map (property). |

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

**intrp**: Lox Interpreter (usually you will not use it)

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

| Extended Type     | Type in Miniscript        |
| -----------       | -------                   |
| Intrinsic         | list from AddIntrinsic    |
| List              | list from NewLoxList      |
| Map               | list from NewLoxMap       |


RuntimeException and Throw types are only availalbe in catch block, others it will be automatically handled.

All technical types are subclass of RuntimeException, besides RuntimeException itself.

| Technical Type    | Type in Miniscript        |
| -----------       | -------                   |
| RuntimeException  | map from NewRuntimeException|
| Return       | map from NewReturn |
| Throw        | map from NewThrow  |
| Break        | map from NewBreak  |
| Continue     | map from NewContinue  |
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