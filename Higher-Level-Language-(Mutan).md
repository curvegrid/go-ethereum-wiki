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
this if else return exit for asm
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

### String type

Strings are not yet supported.

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
MethodName = "DataLoad" | "DataSize" | "Origin" | "Caller" | "GasPrice" | "Value" | "Diff" | "PrevHash" | "Time" | "GasPrice" | "Number" | "Coinbase" | "Gas" .

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
InlineAssembler = Asm "(" Code ")" .
Asm             = "asm" .
Code            = "abcdefghijklmnopqrstuwvxyz" | "1234567890" .
```

## Basic syntax

```go
int32 a = 20
int32 b = 10
if a < b {
    exit()
}

store[a] = 10000
store[b] = this.origin()

int32[2] in
int32[10] out
call(1234567890, 0, 10000, in, out)

asm (
    PUSH 10
    PUSH 0
    MSTORE
)

```