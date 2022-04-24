# Control Flow

## If Expressions

If expressions are of the form `if -condition- { -block- }` where condition
must be of type `bool`.

```rust
if number > 5 {
    // ...
} else {
    // ...
}
```
If expressions are used anywhere an expression can be used and returns the
final statement of the executed block.

## Loop Expressions

`break` optionally takes an expression which is the return value of the
enclosing loop

### Loop

Control `loop`s using `continue`, `break`

```rust
loop {
    println!("Forever!")
}
```

### While

```rust
while number != 0 {
    number = number - 1;
    println!("Not yet!");
}
```

### For

For loops are more optimized by the Rust compiler than while loops.

```rust
let a = [1, 2, 3];
for num in a.iter() {
    println!("Number was {}", num);
}
```

Use ranges with for loops
```rust
for num in (1..4).rev() {
    println!("Number was {}", num);
}
```


