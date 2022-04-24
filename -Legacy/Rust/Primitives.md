# Primitive Types

## Scalar Types

### Integer

`let x: 132 = 12`
Defaults to `i32`

| length  | signed | unsigned |
| ---     | ---    | ---      |
| 8-bit   | i8     | u8       |
| 16-bit  | i16    | u16      |
| 32-bit  | i32    | u32      |
| 64-bit  | i64    | u64      |
| 128-bit | i128   | u128     |
| arch    | isize  | usize    |
Signed:
`-(2^n -1) -> 2^n -1`
Unsigned:
`0 -> 2^n -1`
Arch:
`isize`/`usize` use 32 for 32x architecture and 64 for 64x architecture

Number literals allow using `_` as a visual separator: `1_000`

If Rust detects that there could be an integer overflow (EG: using an i8 and
setting it to 256) it will issue a panic in development. You can disregard
this instead which will allow integer overflows to wrap (257 -> 1, 256 -> 0).

### Floating-Point Numbers

`let x: f64 = 1.1`
Defaults to `f64`

| type  |
| ---   |
| `f32` |
| `f64` |

### Booleans

`let f: bool = true`
| type    |
| ---     |
| `true`  |
| `false` |

### Characters

```
let c = 'z';
let z = 'ℤ';
let heart_eyed_cat = '😻'
```

Characters can be any Unicode scalar value. You can create a literal char by
surrounding it in single quotes.

## Compound Types

### Tuples

Defaults to the contained values default types.
`let my_tuple: ( i32, f64, bool ) = (12, 1.2, true)`

Pattern matching extracts values.
`let (x, y, z) = my_tuple`

Dot syntax can extract values.
`let my_value = my_tuple.0`

### Arrays

`let ar: [i32, 3] = [ 1, 2, 3 ]`
Arrays have fixed length. Once initialized they cannot expand or contract.
Vectors are more alike the traditional array found in other languages.

Bracket syntax can access individual items. Attempting to access out of bounds
array items will cause a panic at runtime.
`let x = ar[0]`

## Complex Types

### String


### String Slice

