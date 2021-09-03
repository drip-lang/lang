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
Array: `[T; usize]`
Slice: `[T]`

#### Array & Slice Ranges
```
[..]                full range
[<from>..]          sub-range from <from> until end of range
[<from>..<end>]     sub-range from <from> until <end>

example
let x = [4, 6, 8, 11]
let y = x[1..]
y == [6, 8, 11]
```

### References and Pointer
| Reference | Pointer  |
|:---------:|:--------:|
|   &T      |  *T      |
|   &mut T  |  *mut T  |

### Optional
`?T => None / <value: T>`

### Ranges
```
(..)                full range
(<from>..)          sub-range from <from> (inclusive) until end of range
(<from>..<end>)     sub-range from <from> until <end> (exlusive)
```
ranges can also have a custom step expression.
this step expression will firstly be implemented as constants e.g. i32
but something like (2..6:= n => n + 2 ) => 4 -> 5 -> 6 -> 7 
```
(..:=<expr>)
```

### Void
`void`

### Math
TODO: std: Complex, Quaternion, Matrix

## Punctuation
### Operators
| Normal | Wrapping | Assign | WrapAssign | Description     |
|:------:|:--------:|:------:|:----------:|:----------------:
|    +   |    +%    |   +=   |     +%=    | Addition        |
|    -   |    -%    |   -=   |     -%=    | Subtraction     |
|    *   |    *%    |   *=   |     *%=    | Multiplication  |
|    ^   |    ^%    |   ^=   |     ^%=    | Exponentiation  |
|    ^^  |    ^^%   |   ^^=  |     ^^%=   | Tetration       |
|    /   |          |   /=   |            | Division        |
|    %   |          |   %=   |            | Modulo          |

### Bitwise
| LShift | RShift | And  | Or  | Not | Nand | Nor  |  Xor  |  Xnor  | Description  |
|:------:|:------:|:----:|:---:|:---:|:----:|:----:|:-----:|:------:|:-------------:
|   <<   |   >>   |  &   | \|  |  ~  |  ~&  | ~\|  |   #   |   ~#   | Normal       |
|   <<=  |   >>=  |  &=  | \|= |  ~= |  ~&= | ~\|= |   #=  |   ~#=  | Assign       |

### Logical
| And |  Or   | Not (unary) | Nand |  Nor  |  Xor  |  Xnor  |
|:---:|:-----:|:-----------:|:----:|:-----:|:-----:|:------:|
| &&  | \|\|  |      !      | !&&  | !\|\| |  ##   |  !##   |

### Comparison
```
>   greater
<   less
==  equal
>=  greater or equal
<=  less or equal

Arrays:
|>  contains (all)            [3, 2] |> [4, 3, 2, 6]    => true
!|> not contains (all)        [5, 3, 2] !|> [2, 8, 4, 11, 7] => false

@> contains (at least 1)      [5, 3, 2] @> [2, 8, 4, 11, 7] => true
!@> contains (> 1)            [5, 3, 2] !@> [2, 8, 4, 11, 7] => false
```

### Other
```
.                   Field Acess
..                  Range
..(value):=         Range with Step
,                   Separator
:                   Separator (<id>: T)
->                  Return Type of Function
=>                  Match

```


## Control FLow
### If
```
if <expr> { }

if <expr> { } else { }

if <expr> { } else if <expr> { }

if <expr> { } else if <expr> { } else { }
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
    ..                  full range match
    <from>..            from range
    ..<to>              to range
    <from>..<to>        from - to range
    <cond>              condition (low priority)
    _   
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

// starts when <cond1> == true, 
// continues as long as <cond2> == true
while <cond1>, <cond2> { }

// starts when <cond1> == true
// continues as long as <cond2> == true
// terminates when <cond3> == true
while <cond1>, <cond2>, <cond3> { }
```


## User Definable
### Functions
#### Declaration
are scoped and can be created almost everywhere but not accessed.
pub keyword does not work inside functions
```
fn <id>() {}                function (void)
fn <id>() -> T {}           function (returns T)
```

### Structs
#### Declaration
are scoped and can be created almost everywhere but not accessed.
pub keyword does not work inside functions
```
struct <id> { }             struct
```

#### Declaration of "methods"
```
impl <id of struct> {
    (pub) fn <id>() { }
    ...
}
```

### Consts and Variables
#### Declaration
```
// unmutable
let <id> = <expr>              const with infered Type
let <id>: T = <expr>           const with given Type
// mutable
var <id> = <expr>              variable with infered Type
var <id>: T = <expr>           variable with given Type

// Todo: statics?
```

## Traits
- Traits are a set of functionality that can be implemented for structs
- Traits can depend on each other
- Traits can be generic, although this feature is not a priority
```
trait<T> <id> { 
    fn <id>() -> T;
}

trait <id>: <id of other trait>, ... { }
```

## Generics
Generics are not a high priority, but they're planned
```
struct<T, U, ..> <id> { }

fn<T, U, ..> <id>() -> T { }

impl<T, U> <id of struct> {
    fn(x: T, y: U) -> T { }
    fn<V>(x: V) -> U { }
}
```

Generics are restricted to only what Traits are in the Scope
```
// does not work
fn<T> add(x: T, y: T) -> T {
    x + y
}

// works
fn<T: Add> add(x: T, y: T) -> T {
    x + y
}
```