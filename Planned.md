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

usize and isize are 32bit or 64bit depending on the host system architecture

additionally for integers every possible bit size may be used [planned]

### Boolean
Boolean is implemented is an enum with variants true and false.
If else expressions will be desugared to a match

### String & Rune
full utf8 encoding for both, rune represents a single character
`string => "<value>"` 
`rune   => '<value>'  `

### Arrays & Slices
Everything that is an Ordinal can be used to index an array
Array: `[N]T`
Slice: `[]T`

#### Multi Dimensional Arrays
`[N][N][N]T  // 3-dim array`

#### Special Case
using the [dyn]T creates a dynamically sized Array with usize indexing

#### Array & Slice Rangs
Arrays can be indexed with Ranges

### References and Pointer
| Reference | Pointer  |
|:---------:|:--------:|
|   &T      |  ^T      |
|   &mut T  |  ^mut T  |

### Optional
the language has no null (except for pointer types and interfacing with C), a enum called Option is used for this
```
Option :: enum [T] {
    Some(T)
    None
}
```

### Declarations
constants (immutable variables) and `structs`, `enums`, `traits`, `functions` are created with the same syntax
```
some_number :: 23423
text :: "hello"
a_character :: 'H'

struct_def :: struct { }
trait_def :: trait { }
enum_def :: enum { }
function_def :: ( ) { }
```
for constants the type is automatically infered when they are used, a constant has no strict defined value.
the type for constans can be explicitly set with: `<id>: <type> : <value>
```
x: int : 5
```

`mutable` variables are defined and modified like this:
```
x := 5
y: int = 8

set x = 5 // x == 8
```

## Punctuation & Keywords
### Operators
These operators will be implemented on language level, except for some exceptions.
Operators and their precedence are fully defininable within the language.
Some of these, especially the atypical operators are in strong subject to change once the language reaches it's first stage!

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
ranges are implemented on language level as a Type and an operator `..`.
ranges are [a..b], to get exclusive ranges a > or < can be added to either side.
in case of b > a the range will count downwards.
everything that implements "Ordinal" can be used for ranges
```
from..to => [a..b]
from<..<to (a..b)
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
enum
trait
use
using
set
as
when
for
if
else
defer
match
break
continue
fallthrough
in
return
context
extern
```

## Control FLow
### If
will be desugared to match
```
if <expr> { }

if <expr> { } else { }

if <expr> { } else <expr> { }

if <expr> { } else <expr> { } else { }
```

### match
- enums and some other types can be matched.
- Matches must be exhaustive or contain a "catch all" `_ =>`.
- The order of branches matters.
```
Event :: enum {
  New(i32)
  Update { idx: usize, with: i32 }
  Delete { idx: usize }
  Error
}

new_event :: Event::Change { idx: 5, with 10 }
match new_event {
  New(val) => ... ,
  Update { idx, with } =>  ... , 
  _ => { }
}

is_sussy :: true
x :: match is_sussy {
  true => 420,
  false => 69
}
// x == 420

match x {
  0 => ..,
  3 => ..,
  420 => ..,
  _ => { }
}
```

matches can also have additional if check or match multiple.
```
hello :: "ok"
x :: 9000
match x {
  0 if hello == "ok" => ..,
  3 => ..,
  7..10 =>
  69, 9000 if hello == "bye" => ...,
  420, 9000 if hello == "ok" => ..., // this branch is chosen
  9000 => ..,
  _ => { }
}
```

### For
for loops might look a bit sus at first but they can do a lot!
```
for { } // infinite loop, all other for loops will desugar to this

for <id> in <expr> { }

for <id>, <usize> in <expr> { } 

for <expr> { } // run as long as condition holds true

for <expr>, <expr> { } // the former with an additional variable (for x := 0, x < 5 { })

for <expr>, <expr>, <expr> { } // the former with an additional after each run expression (for x := 0, x < 5, x += 1}
```

## Functions
### Declaration
are scoped and can be created almost everywhere but not accessed.
```
<id> :: () {}                   function (void), can also be written as -> _
<id> :: () -> T {}              function (returns T)
<id> :: () -> (T, U) {}         function (returns T, U)
<id> :: (x, y: T, z: U) -> T    function (takes x, y: T, z: U) and returns T)
```

Functions return the last expression automatically.
To return earlier, `return <expr>` may be used

## Enums (may be replaced with Sum Types)
Enums are "variadic objects" similar to integers,
they are special in a sense that each of their subtypes represents not just a number but also an entire ("sub-") type.
Enums may also be tagged unions

```
bool :: enum {
  false // implicit 0
  true // implicit 1
}

option :: enum [T] {
  some (T)
  none
}

Event :: enum {
    New(i32)
    Change { idx: usize, with: i32 }
    Error
}
```

## Structs
### Declaration
are scoped and can be created almost everywhere but not accessed.
Fields can optionally be separated via `,` but whitespaces of any type would be sufficient.
Fields can have default values, but the type must be provided!

```
<id> :: struct { }

// example
<id> :: stuct {
  x, y, z: i32
}

<id> :: struct {
  x: i32 = 1
  y: i32
  z: {
      s: i32
      u: i32
  }
}
```

### Declaration of member functions, to use itself, self may be used
```
impl <id> {
  <id> :: () {}
  <id> :: (self) -> T {}  // Takes self and returns T, self or self: Self is equivalent
}
```

### "Inheritance"
a struct can inherhit other types in a compositional way via the `using` keyword
default values of the "inherited" field type can be set inside `{ }`
```
Position :: struct { x, y: f32 = 0 }
Life :: struct { left: i32 }
Entity :: struct { 
  pos: using Position
  life: using Life { left = 3 }
  name: string
}
Cat :: struct {
    entity: using Entity { life: { life = 7 }, name: "Cat" }
    owner: option[string]
}

impl Position {
  move_right :: (self) { self.x += 1 }
}

impl Life {
  damage :: (self) { self.left -= 1 }
}

impl Cat {
  new :: (owner: option[string]) -> { Self { Self(owner) } }
}

cat := Cat::new(some("frog"))
cat.move_right()
cat.move_right()
cat.y = 3
cat.damage()
// cat now is { entity: {pos: {x: 2, y: 3}, life: { left: 6 }, name: "Cat" } owner: some("frog") }
```

## Generics
Generics are optionally defined after the `struct`, `trait` or `enum` keyword insde `[]`.
They are defined like normal function arguments for example.
generics may be automatically infered.

Generics are compile time generated for the types that were found
```
List = struct [T] { ... }

impl List[T] { 
  new :: () -> Self { ... }
}

test := List[i32]::new()
```
also ordinals my be used as generics
```
NewArray :: struct [N: i32, T] { ... }

test := NewArray[5, i32] { .. }
```

## Traits
- Traits are a set of functionality that can be implemented for structs
- Traits can depend on each other
- Trait functions can have a default implementation, that may be overridden

```
<id_struct> :: struct { }

<id> :: trait { 
    <id> :: () -> T
}

impl <id_strut> <| <id> { }
```

### Operators
Operators are implemented as traits with optional precedence
```
Add :: trait[L, R, T = Self] #[op: 5] {
  + :: (lhs: L, rhs: R) -> T
}
```

