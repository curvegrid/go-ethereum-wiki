HLL [DRAFT]
===

Contract development can be done in a higher level language so you don't have to muck around with the assembler language (of course you're allowed to do so). The higher level language, draft name **mutan**, can be found [here](https://github.com/obscuren/mutan). mutan is a statically typed language.

## Notation

The syntax is specified using Extended Backus-Naur Form (EBNF):
```
digit excluding zero = "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;
digit                = "0" | digit excluding zero ;
```
### Keywords

The following keywords are reserved and may not be used at identifiers
```
this if else return exit for asm store
```

### Operators and delimiters

Mutan contains the following operators and delimiters (some are wip)
```
+       =       |       (   )
-       ==      &       {   }
*       >=      ++      ;
/       <=      --
```

### Numeric types

Numeric types in Mutan can only be represented by integer values and are architecture-independent. They are all represented in big endian byte order. Signed integers aren't supported (yet?)
```
int8        the set of all _unsigned_ integers (0 to 255)
int16       the set of all _unsigned_ integers (0 to 65535)
int32       the set of all _unsigned_ integers (0 to 4294967295)
int64       the set of all _unsigned_ integers (0 to 18446744073709551615)
int256      the set of all _unsigned_ integers (0 to 1.1579209e+77)
big         same as int256
```

### Arrays

Arrays are of theoretical unlimited length
```
ArrayType   = ElementType "[" Number "]" .
ElementType = Type .
```

The following are all valid arrays:
```
int16[10] a
int32[10] b
big[10]   c
```

## Declarations

A declaration binds a identifier to a type. Every identifier must be declared. No identifier may be declared twice. All declarations are global (for now, TODO/FIXME).

```
Declaration = TypeDecl .
```

Mutan, in it's current state, is globally scoped:
    1. No identifier may be used twice
    2. All identifiers must be declared using a Type

```
Types:
    int int8 int16 int32 int64 int256 big
    (todo)
    bool string
```

## Statements

```
Statement = Declaration | Block | IfStmt | ForStmt
```

## Blocks

Blocks contain, but not necessarily, contain lists of Statements within matching brackets.

```
Block = "{" StatementList "}" .
StatementList = { Statement ";" } .
```

## If statements

If statements specify the conditional execution of two branches according to the value of an expression. If the expression evaluated to true, the "if" branch is executed, else, if present, the else branch is executed.

```
IfStmt = "if" [ SimpleStmt ";" ] Expression Block [ "else" Block ] .
```

```go
if x < 10 {
   x = maximum
}
```

The expression may be preceded by a simple statement which executes before the boolean expression

```go
if int8 x = this.Value(); x < 10 {
    x = maximum
} else {
    y = 10
}
```

## For statement

A "for" statements specifies repeated execution of a block, the iteration is controlled by a conditional block.

```
ForStmt = "for" [ InitStmt ] ";" [ Condition ] ";" [ PostStmt ] .
InitStmt = SimpleStmt .
PostStmt = SimpleStmt .
```

A "for" in it's simplest form is a C-Like "while" statement (therefor Mutan doesn't have a "while")

```go
for a < b {
    a = a * 2
}
```

A "for" statement in it's purest form is controlled my a initialiser, condition and a post statement which will be executed at the end of the Block

```go
for int8 = 0; a < b; a++ {
    b = b - 1
}
```

```
for cond { T() }         is the same as    for ; cond ; { T() }
for cond; post { T() }   is the same as    for ; cond; post { T() }
```

## Build in functions

mutan comes with a couple build in functions

```go
Exit()                                             Stops the execution of the current call
Call(address, value, gas, calldata, returndata)    Calls another contract (e.g. closure) and executes
```

The following build in functions are related to the current executing context (i.e. the closure) and are called in combination with `this`
```
Method     = "this" Dot MethodName Parentheses .
Dot        = "." .
MethodName = "DataLoad" | "DataSize" | "Origin" | "Caller" | "GasPrice" | "Value" |
             "Diff" | "PrevHash" | "Time" | "GasPrice" | "Number" | "Coinbase" | "Gas" .
```

```
DataLoad      Returns the data attached to this call
DataSize      Returns the size of the data attached to this call
Origin        Returns the origin address of this execution
Caller        Returns the current caller of the closure
GasPrice      Returns the gas price attached to this call
Value         Returns the value attached to this call
Diff          Returns the current difficulty
PrevHash      Returns the previous block's hash
Time          Returns the current block's timestamp
GasPrice      Returns the attached call's gas price
Number        Returns the current block's number
Coinbase      Returns the current block's coinbase
Gas           Returns the current call's attached amount of gas
```

## Assembler

Inline assembler is allowed through the `asm` keyword

```
InlineAssembler = "asm" "(" Code ")" .
Code            = "abcdefghijklmnopqrstuwvxyz" | "1234567890" .
```

## Basic syntax

```go
int32 a = 20
int32 b = 10
if a < b {
    Exit()
}

store[a] = 10000
store[b] = this.Origin()

for int8 i = 0; i < 10; i++ {
    int32[10] out
    Call(1234567890, 0, 10000, i, out)
}

asm (
    PUSH 10
    PUSH 0
    MSTORE
)

```