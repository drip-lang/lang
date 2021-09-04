# Planned

## Types

### Numeric
| Integer | Unsigned | Float |
|:-------:|:--------:|:-----:|
|   i8    |    u8    |       |
|   i16   |    u16   |  f16  |
|   i32   |    u32   |  f32  |
|   i64   |    u64   |  f64  |
|   i128  |    u128  |  f128 |
|   isize |    usize |       |

### Boolean
`bool => true / false (1 byte)`

### String & Char
`string => "<value>"` 
`char   => '<value>'  `

### Arrays & Slices
Array: `[usize]T`
Slice: `[]T`

#### Multi Dimensional Arrays
`[usize][usize][usize]T  // 3-dim array`

#### Array & Slice Ranges
```
[..]                full range
[<from>..]          sub-range from <from> until end of range
[<from>..<end>]     sub-range from <from> until <end>

example
x := [4, 6, 8, 11]
y := x[1..]
y = [6, 8, 11]
```

### References and Pointer
| Reference | Pointer  |
|:---------:|:--------:|
|   &T      |  ^T      |
|   &mut T  |  ^mut T  |

### Optional
`?T => None / <value: T>`

### Void
`void`

### Math
TODO: std: Complex, Quaternion, Matrix

## Punctuation & Keywords
### Operators
| Normal | Wrapping | Assign | WrapAssign | Description     |
|:------:|:--------:|:------:|:----------:|:----------------:
|   +    |    +%    |   +=   |     +%=    | Addition        |
|   -    |    -%    |   -=   |     -%=    | Subtraction     |
|   *    |    *%    |   *=   |     *%=    | Multiplication  |
|   ^    |    ^%    |   ^=   |     ^%=    | Exponentiation  |
|   ^^   |    ^^%   |   ^^=  |     ^^%=   | Tetration       |
|   /    |          |   /=   |            | Division        |
|   %    |          |   %=   |            | Modulo          |

### Bit
| LShift | RShift | And  | Or  | Not | Nand | Nor  |  Xor  |  Xnor  | Description  |
|:------:|:------:|:----:|:---:|:---:|:----:|:----:|:-----:|:------:|:-------------:
|   <<   |   >>   |  &   | \|  |  ~  |  ~&  | ~\|  |  \|>  |  ~\|>  |   Normal     |
|   <<=  |   >>=  |  &=  | \|= |  ~= |  ~&= | ~\|= |  \|>= |  ~\|>= |   Assign     |

### Logical
| And |  Or   | Not (unary) | Nand |  Nor  |  Xor  |  Xnor  |
|:---:|:-----:|:-----------:|:----:|:-----:|:-----:|:------:|
| &&  | \|\|  |      !      | !&&  | !\|\| | \|\|> |  !\|>  |

### Comparison
```
>   greater
<   less
==  equal
>=  greater or equal
<=  less or equal
```

### Ranges
```
(..)                full range
(<from>..)          sub-range from <from> (inclusive) until end of range
(..<end>)           sub-range from start until <end> (exclusive) until end of range
(<from>..<end>)     sub-range from <from> until <end> (exlusive)
```
ranges can also have a custom step expression.
this step expression will firstly be implemented as constants e.g. i32
but something like (2..6= n => n + 2 ) => 4 -> 5 -> 6 -> 7
```
(..=<expr>)
```

### Other
```
.                   Field Acess
..                  Range
..=(value)          Range with Step
( )                 Block (functions)
{ }                 Block (scope)
[ ]                 Block (slices, access)
,                   Separator
:                   Separator (<id>: T)
::                  Statement (const)
:=                  Statement (variable)
->                  Return Type of Function
=>                  Match Arm
#                   Compiler Annotations
#[<comp_expr>]      Compiler Annotations
// todo
```

### Keywords
```
struct
trait
use
if
else
match
for
in
while
loop
dyn
return
true
false
None
Some
```

## Control FLow
### If
```
if <expr> { }

if <expr> { } else { }

if <expr> { } else <expr> { }

if <expr> { } else <expr> { } else { }
```

### Match
```
match <expr> {
    <match_expr> => <expr>,
    ...
}

<match_expr> :=
    value               value
    variance            e.g Enum
    <from>..<to>        range expression
    _                   No Match / matches all
```

### For
```
//<expr> must return a Sequence of Values

// id = value
for <id> in <expr> { }

// id = value, usize = counter / index
for <id>, <usize> in <expr> { }

```

### While
```
// running as long as <cond> == true
while <cond> { }
```


## User Definable
### Functions
#### Declaration
are scoped and can be created almost everywhere but not accessed.
```
<id> :: () {}                   function (void)
<id> :: () -> T {}              function (returns T)
<id> :: () -> (T, U) {}         function (returns T, U)
```

Functions return the last expression automatically.
To return earlier, `return <expr>` may be used

### Structs
#### Declaration
are scoped and can be created almost everywhere but not accessed.
Fields can optionally be separated via `,` but whitespaces of any type would be sufficient
```
<id> :: struct { }              struct

// example
<id> :: stuct {
    x, y, z: i32
}

<id> :: struct {
    x: i32
    y: i32
    z: {
        s: i32
        u: i32
    }
}
```

#### Declaration of "methods"
```
<id> :: impl {
    <id> :: () {} 
}

<id struct> <| <id trait> :: impl { 
    <trait fn id> :: () {} 
}
```

### Consts and Variables
#### Declaration
```
// unmutable
<id> :: <expr>              const with "infered" Type
<id>: T :: <expr>           const with Type T
// mutable
<id> := <expr>               variable with infered Type
<id>: T := <expr>            variable with given Type

// Todo: statics?
```

## Traits
- Traits are a set of functionality that can be implemented for structs
- Traits can depend on each other
- Traits can be generic, although this feature is not a priority
```
<id> :: trait { 
    <id> :: () -> T;
}

<id> :: trait <| <id of other trait> + ... { }
```

## Generics
Todo