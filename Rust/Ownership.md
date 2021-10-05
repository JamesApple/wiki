## Stack vs Heap

Storing data on the stack is fast as the data always goes to the same location
and it's of a fixed size.

Storing data on the heap requires a pointer as it is not in a fixed location
and could be of any size.

When you store something on the heap the OS finds an empty location in the heap
large enough for the data, marks it as being in use and then returns a
_pointer_ which is the memory address. This is called _allocating_ or
_allocating on the heap_. 

Accessing the heap will always be slower than the stack. Searching for a
specified location in memory is not cheap.

A function call pushes its arguments and local variables(potentially pointers)
onto the stack. Completing the function pops it off the stack.

## Ownership Rules

1. Each value has a variable that's called its _owner_
1. There can be one owner at a time
1. When the owner goes out of scope, the value is dropped.

## Memory and Allocation

In the case of string literals we encode the value directly into the final
executable. They cannot be mutated. Mutating a string requires creating a
mutable reference to a string `let mut s = String::from("My String")`.

Rust does not require paring a `malloc` with an associated `free`. Any time
Rust reaches a closing `}` it calls the internal function `drop` to remove
unused references inside.

Apparently this is similar to a pattern called _RAII_ or Resource Acquisition
Is Initialization.

## Variables and Data Interactions

This sets `x` to the simple value `5` and `y` to the simple value `5`.

```rust
let x = 5;
let y = x;
```

*This fails* as `y` is borrowing the pointer that `x` is trying to mutate.

```rust
let mut x = String::from("Hello");
let mut y = x;
x.push_str(" World!"); // Woops! y is borrowing the string at the moment!
```

`x =`
| name     | value |
| ---      | ---   |
| ptr      | ABC   |
| len      | 5     |
| capacity | 5     |

`y =`
| name     | value |
| ---      | ---   |
| ptr      | ABC   |
| len      | 5     |
| capacity | 5     |

`HEAP #ABC =`
| index | value |
| 0     | H     |
| 1     | e     |
| 2     | l     |
| 3     | l     |
| 4     | o     |

---

When `y` was assigned to the value of `x` the string itself was not copied on
the heap. Instead only the pointer is copied onto the stack.

**Move:** 
This is similar to the concept of a shallow copy. Instead of merely
shallow copying Rust also invalidates the previous reference `x`. This is
referred to as a __move__

We could alternatively return a __clone__ which is a true deep copy. `let mut y
= x.copy();`

Rust has an annotation called `Copy` that are placed on integers stored on the
stack. If a type has the copy trait an older variable is still usable after
assignment. Any type with the `Copy` trait will not be able to implement
`Drop`.

These types do not invalidate previously declared variables. AKA implement `Copy`
1. All integers.
1. `bool`
1. `char`
1. Tuples if they only contain types that are also `Copy`

## References

Using `Move` to transfer ownership in and out of scope is tedious. Using `&`
references is much easier to manage.

Reference types are prefixed with `&` such as `&String`.

A reference is actually a pointer to another pointer.

## Mutable References

Mutable Reference: 
```rust
fn my_func(x: &mut String) {
}
my_func(&mut my_string)
```

Mutable references cannot be used twice within the same scope to prevent data
races. This is fixed by wrapping multiple mutations in separate scopes.


