# The PointOfNoReturn Project

This is a hobby project and the focus is on the process not the result. I want to try out new ways of thinking which should lead to a new programming language and in the end create a point of no return to the existing programming languages.

## List of ideas for the new programming language

* Forward calls only - no return stack
* Dispatch queue for forward calls - to simulate threads
* Waiting queue - merge simulated threads
* Streaming source code - running on a virtual machine
* Hot code update - through streaming

## List of ideas for the virtual machine

### VM state
* Registers (Vector)
* Functions (HashMap)
* Next (Queue)
* Wait (HashMap)

### VM interfaces
* Console (read/write)
* Program (read/write)
* Registers (readonly)
* Instructions (readonly)
* Functions (readonly)
* Next (readonly)
* Wait (readonly)
* Status (readonly)

## Built in functions
```
Input(Function)
Output(String)
OutputLine(String)
Wait(Key, Lifetime, Name, Params)

CallExt(Node, Name, Params)
SendExt(Node, Name, Function)
WaitExt(Node, Key, Lifetime, Name, Params)
```
## Syntax examples

### Hello world example
```
fun Hello() do
  OutputLine("Hello World!")
endfun

Hello()
```
### Hot code update example
```
fun Foo() do
  next Bar()
endfun
    
fun Bar() do
  Foo()
endfun

Foo()

fun Bar() do
  nothing
endfun
```
### Fibonacci example
```
fun Fib
  (0) do
    next Fib(0, 0, 1);
  (X) when is_int(X), X > 0 do
    next Fib(X, X, 0);
  (X0, 0, Acc) do
    OutputLine("Fib(" + to_str(X0) + ") is " + to_str(Acc));
  (X0, X, Acc) do
    Xn = X - 1,
    Fib(X0, Xn, Acc+X)
endfun

Fib(0)
Fib(1)
Fib(2)
Fib(3)
```
### Echo example
```
Input(Output)
``` 
### Typewriter example
```
fun Init() do
  Input(Keystroke),
  next Keystroke("", "")
endfun

fun Keystroke
  (I) do
    Wait(key, 1, Keystroke, (nil, I));
  (S, I) when I == "\r" do
    OutputLine(S),
    Wait(key, 1, Keystroke, ("", nil));
  (S, I) do
    Wait(key, 1, Keystroke, (S+I, nil))
endfun

Init()
```
