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
```rust
	let a = [1,2,3,4,5];

	for element in a.iter() {
			println!("The value is {}", element);
		}
```
range and rev in for loops
```rust
for n in(1..9).rev(){
		println!("{}!", n);
}
```

##Ownership
###Rules
* Each value in Rust has a variable called its **owner**
* There can be only **one** owner at a time
* When the owner goes out of scope, the value will be dropped

Instead of using garbage collection, Rust takes a different approach. The memory is automatically returned once that variable that owns it goes out of scope
When a variable goes out of scope, rust calls **drop** for us which deallocates the resources (similar to Resourse Acquisition Is Initialization in C++)

In rust **shallow copy** (Copying only the reference to memory) also invalidates the first variable
```rust
let s1 = String::from("hello");
let s2 = s1;  
println!("{}, world!", s1);
//Compile error. Value moved here (to s2)
```
Instead of a shallow copy, its known as a **move**. s1 was moved into s2 (Which will prevent double free bugs)
Note: Rust will never create "deep" copies of data

If we want to deeply copy the heap data, not just the stack, we can call **clone**
```rust
let s1 = String::from("hello");
let s2 = s1.clone();
println!("s1 = {} s2 = {}",s1,s2 );
```

Types that have a known size at compile time (integers for ex) are stored entirely on the stack, so copies are quick to make. In this case there isn't a difference between shallow and deep copy.
There is a special notation called **Copy** trait which we can place on types like integers that are stored on the stack
```rust
fn main() {
    let s = String::from("hello"); // s comes into scope
    
    takes_ownership(s); // s's value moves into the function...
                        // ... and so is no longer valid here
                        
    let x = 5;          // x comes into scope

    makes_copy(x);      // x would move into the function,
                        // but i32 is Copy, so it's okay to
                        // still use x afterward
}// Here, x goes out of scope, then s. But because s's value was moved,
// nothing special happens.

fn takes_ownership(some_string: String ){
    println!("{}!",some_string);
} //Here, some_string goes out of scope and `drop` is called. The backing
// memory is freed.

fn makes_copy(some_integer: i32){  // some_integer comes into scope
    println!("{}", some_integer);
}//Here, some_integer goes out of scope. Nothing special happens.
```

####Return values and scope
Return values can also transfer ownership. If we return a string we are giving ownership

What if we want to let a function use a value and not take ownership? We can pass it as a parameter and then return it, but there is a better way

####References and borrowing
With references we allow to refer to some value without taking ownership of it
```rust
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);

    println!("length of {} is {} ", s1, len);
}
fn calculate_length(s: &String) -> usize {  // s is a reference to a String
    s.len()
}// Here, s goes out of scope. But because it does not have ownership of
// what it refers to, nothing happens.
```
Having references as function parameters is called **borrowing**.
References are also immutable by default. If we try to modify it, we get an error

We can create a mutable reference:
```rust
fn main(){
    let mut s = String::from("hello");
    change(&mut s);
    println!("{} ", s);
}
fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```
The variable must be mutable, the reference must be mutable and the function must accept a mutable reference.
Note: We can only have one mutable reference to a particular piece of data

####Rules of reference
* At any given time, we can have either but not both of: one mutable reference or any number of immutable references
* References must always be valid

###Slice Type
```rust
fn main() {
    let s = String::from("hello world");

    let slice= &s[..5];
    println!("slice1: {}", slice); // hello
    let slice = &s[6..9];
    println!("slice1: {}", slice); // wor
    let slice = &s[6..];
    println!("slice1: {}", slice); //world
    //Full string
    println!("{}", &s[..]);
    println!("First word: {}", first_word(&s));
}

fn first_word(s: &String) -> &str {
   let bytes = s.as_bytes();

   for (i, &item) in bytes.iter().enumerate(){
       if item == b' ' {
           return &s[0..i];
       }
   }
   &s[..]
}
```
Note: Strings are a type of slices

###Stuctures
Similar to tupples, can group different data types but we name each piece of data
```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

fn main() {
    let mut user1 = User {
        email: String::from("some@example.com"),
        username: String::from("user123"),
        active: true,
        sign_in_count: 1,
    }; 
    //changing value
    user1.email = String::from("someone@example.com");
}

fn build_ser(email: String, username: String) -> User {
    User {//We can use field init shorthand
        email, //If the struct value and parameter have same name
        username,//we can use it like this 
        active: true,
        sign_in_count: 1,
    }
}
```

####Struct update
To update only some values of a struct, we can:
```rust
let user2 = User {
	email: String::from("another@example.com"),
	username: String::from("anotherusername567"),
	active: user1.active,
	sign_in_count: user1.sign_in_count,
};
```
Or we can update the value like this
```rust
let user2 = User {
	email: String::from("another@example.com"),
	username: String::from("anotherusername567"),
	..user1
};
```

####Tupple Structs
```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);
let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```
####Derived traits
If we try to just print the rect, it will error. println! uses formatting known as **Display**. We can also add a derive debug and print with **{:?}** or **{:#?}**
```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    heigh: u32,
}
fn main() {
    let rect1 = Rectangle { width: 30, heigh: 50 };

    println!("Rectangle area {:?}", rect1);
}
```

####Struct Methods
We can define a method, a function for the struct, which first parameter is always self
```rust
impl Rectangle{
    fn area(&self) ->  u32 {
        self.width * self.heigh
    }
}
println!("Rectangle area {}", rect1.area());
```
impl -> implementation
In this example we borrow the Rectangle since we only want to read it, not modify if (would have to be mutable)
 Given the receiver and name of a method, Rust can figure out definitively whether the method is
reading (&self), mutating (&mut self), or consuming ( self).

####Associated functions
We can define blocks of **impl** that don't take self as a parameter, called associated functions
To call associated functions we use ::
```rust
impl Rectangle{
    fn square(size: u32) ->  Rectangle {
       Rectangle {width: size, heigh: size} 
    }
}
fn main(){
    let sq = Rectangle::square(3);
    println!("Square {:#?}", sq);
}
```








