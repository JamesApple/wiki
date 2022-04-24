# Functions

The following defines a function that takes one `i32` and returns an `i32`
```rust
fn my_func(x: i32) -> i32 {
    x
}
my_func(1);
```

## Blocks

Functions and other blocks implicitly return the final expression in the
block. This is indicated by the lack of a final semicolon.
```rust
let x = {
    let y = 2;
    y
};
```

## Void Returns

The type of operations with no return value is `-> ()`.
