---
title: Rust
date: 2020-04-10 08:13:35+01:00
author: Gaxpert
---

# Rust
##Basics
Install
`curl https://sh.rustup.rs -sSf | sh`

Update
`rustup`

Compile
`rustc main.rs`

Version
`rustc --version`

Documentation
`rustup doc`

##Cargo
Create new project
`cargo new hello_cargo --bin`

crates --> packages of code

Build
`cargo build`
Run
`cargo run`
Check it compiles
`cargo check`

Building for release, with optimizations
`cargo build --release`

Add dependency to 'Cargo.toml'. Cargo will fetch the dependency.
`rand = "0.3.14"`

Check documentation on dependencies 
`cargo doc --open`

##Getting Started
Import standard lib, IO
`use std::io;`

Create mutable variable bound to a new, empty instance of string
`let mut guess = String::new();`
mut --> mutable, variables and references are immutable by default
String::new --> function that returns a new instance of a String.String
'::' --> Indicates that new is and associated function

Read user input
```rust
io::stdin().read_line(&mut guess)
.expect("Failed to read line");
```
The stdin function returns an instance of std::io::stdin, which is a type that represents a handle to the standard input. .read_line calls the method on the standard input handle to get input.
Note: String argument must be mutable

The read_line also returns a value, 'io::Result'

Result types are enumerations, or enums. An enumeration is a type that can have a fixed set of values, called variants.
For Result types the variants are 'Ok' or 'Err'

Add external dependency
`extern crate rand;`

Error handling
expect --> crash
match --> handle error
```rust
let guess: u32 = match guess.trim().parse() {
	Ok(num) => num,
	Err(_) => continue,
};
```

###Variables
Variables are immutable by default
Constants are always immutable. To declare a const the type of value must be annotated
`const MAX_POINTS: u32 = 100_000;`
Shadowing a variable -> replace a value
```rust
let x = 5;
let mut y = 5;
x = 6; //Error
y = 6; //Ok
let x = 6; //Ok
```

The difference between **shadowing** a variable with **let** and making it **mut** is that **let** creates a new variable(which means we can also change the data type)

###Data Types
Every value as a certain data type, divided in two subsets: **scalar** and **compound**

####Scalar types
Represent a single value. It has 4 types: Integers, floating-point numbers, booleans and chars

#####Integer Types
Signed -> i
Unsigned -> u
Length -> 8, 16, 32, 64
ex: i8, u16, u32, i64

|  Number literals      |      Example     |     	
| ------------- |-------------  |
|    Decimal    |    98_222          |        
|    Hex |    0xff          |        
|    Octal    |    0o77          |        
|    Binary    |    0b1111_0000 |        
|    Byte(u8 only)    |    b'A'          |        

####Floating point
Primitive types: f32 and f64

####Char Type
Chars use single quotes and strings use double quotes

###Compound Types
Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays
####Tuple
Can have elements of different types
```rust
fn main() {
	let tup = (500, 6.4, 1); //Declare tupple
	let tup: (i32, f64, u8) = (500, 6.4, 1); //Or with type notations
	println!("The value of y is: {}", y);
}
```
To access a tupple values we can **destructure** a tupple or access a tupple element
```rust
fn main() {
	let tup = (500, 6.4, 1); //Declare tupple
	let (x, y, z) = tup;  //Pattern matching to destructure tupple
	println!("The value of y is: {}", y);
	let five = x.0;
}
```
####Array
All elements must have same type. Fixed lenght, can't grow or shrink
Arrays are useful when you want your data allocated on the stack rather than the heap, or ensure a fixed number of elements
```rust
let arr = [1,2,3];
let first = arr[0];
```

###Functions
Uses **snake case** convetion. All letters are lowercase and underscores separators
We must declare the type of each parameter
```rust
fn another_function(x: i32, y: i32) {
		println!("Another func {}  {} ",x,y);
	}
```
####Statements and expressions
Statements -> instructions to perform some action and do **not** return a value
Expressions -> Evaluate to a resulting value. Expressions do not end with a semicolon

####Return values
We don't name return values, but we declare their type after an arrow
The return value is synonymous with the value of the final expression of the block of the function. If we want to return early we use the **return** keyword
```rust
fn five() -> i32{
    5
}
```

###Control flow
Note:Rust only checks for the first true condition.
```rust
fn main() {
    let num = 3;

    if num %4 ==0{
        println!("True");
    } else if num %3==0{
        println!("False");
    } else{
    println!("");
		}
}
```
Since **let** is an expression, we can use a statement on the right side 
```rust
fn main() {
    let condition = true;
    let number = if condition {
        5
    } else{
        6
    };
    println!("Value of {}", number);
	}
}
```

###Loops
Three types of loops: loop, while and for
```rust
loop{
		println!("Hello, world!");
}
```
while condition{}
```rust
let mut number = 3;
while number != 0{
		println!("{}!", number);
		number = number -1;
}
```
loop through collection with **for**




















