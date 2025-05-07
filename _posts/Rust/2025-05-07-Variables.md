---
layout: post
title : Variables and mutability 
author: Mégane Vilain
category: Rust
---


In rust, variables are immutable by default.

```rust
let x = 5;
x = 6;
```
This code will not compile because we updated the value of x 

### Mutable variables
In order to update the value of a variable, we need to use the **mut** keyword

```rust
let mut x = 5;
x = 6;
println!("The value of x is {x}")
```
### Constants

Constants are values that are bound to a name and are not allowed to change. 
You can't use the mut keyword with a constant. They can be declared in any scope, including global scope.

More importanly, a constant can be set only to a constant expression not the result of value that could only be computed at runtime

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

### Shadowing 

In rust it's possible to create a new variable with the same name as a previous variable.

In effect, the second variable overshadows the first, taking any uses of the variable name to itself until either it itself is shadowed or the scope ends. 
Since it's a new variable, it's even possible to chage it's type.

#### Example

```rust
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}
```

```sh
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.31s
     Running `target/debug/variables`
The value of x in the inner scope is: 12
The value of x is: 6
```
