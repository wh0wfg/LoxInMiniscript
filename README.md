# Mlox Extended

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

**intrp**: Lox Interpreter (usually you don't need to use this)
**argumments**: List of arguments

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

## Create Lox Interpreter
NOTE: interpreter is already exists in the global.
```lua
interpreter = NewInterpreter()
```