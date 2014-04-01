HLL
===

Contract development can be done in a higher level language so you don't have to muck around with the assembler language (of course you're allowed to do so). The higher level language, draft name **mutan**, can be found [here](https://github.com/obscuren/mutan).

Mutan is a statically typed language and consists of the following types
```
int8      int16   int32   int64    int256
big
string
int8[10]  int16[10]   ...
```

### Keywords

The following keywords are reserved and may not be used at identifiers
```
this if else return exit for
```

### Operators and delimiters
```
+   =   |   (   )
-   ==  &   {   }
*   >=  ++  ;
/   <=  --
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
```