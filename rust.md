---
title: Rust
date: 2020-04-10 08:13:35+01:00
author: Gaxpert
---

# Rust
## Basics
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

## Cargo
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

## Getting Started
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

### Data Types
Every value as a certain data type, divided in two subsets: **scalar** and **compound**

#### Scalar types
Represent a single value. It has 4 types: Integers, floating-point numbers, booleans and chars

##### Integer Types
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

##### Conversion
Int to float
`let len = len as f32`

#### Floating point
Primitive types: f32 and f64

#### Char Type
Chars use single quotes and strings use double quotes

### Compound Types
Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays
#### Tuple
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
#### Array
All elements must have same type. Fixed lenght, can't grow or shrink
Arrays are useful when you want your data allocated on the stack rather than the heap, or ensure a fixed number of elements
```rust
let arr = [1,2,3];
let first = arr[0];
```

#### Print
Print memory address
```rust
println!("{:p}", &x);
```



### Functions
Uses **snake case** convetion. All letters are lowercase and underscores separators
We must declare the type of each parameter
```rust
fn another_function(x: i32, y: i32) {
		println!("Another func {}  {} ",x,y);
	}
```
#### Statements and expressions
Statements -> instructions to perform some action and do **not** return a value
Expressions -> Evaluate to a resulting value. Expressions do not end with a semicolon

#### Return values
We don't name return values, but we declare their type after an arrow
The return value is synonymous with the value of the final expression of the block of the function. If we want to return early we use the **return** keyword
```rust
fn five() -> i32{
    5
}
```

### Control flow
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

### Loops
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

## Ownership
### Rules
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

#### Return values and scope
Return values can also transfer ownership. If we return a string we are giving ownership

What if we want to let a function use a value and not take ownership? We can pass it as a parameter and then return it, but there is a better way

#### References and borrowing
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

#### Rules of reference
* At any given time, we can have either but not both of: one mutable reference or any number of immutable references
* References must always be valid

### Slice Type
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

### Stuctures
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

#### Struct update
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

#### Tupple Structs
```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);
let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```
#### Derived traits
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

#### Struct Methods
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

#### Associated functions
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

###Enum
Enums allow to define a type by enumerating its possible values.
```rust
enum IpAddrKind {
    V4,
    V6,
}
fn main() {
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;
}
```
The variants of enum are namespaced under its identifier, and we use double colon to separate them.
We can also associate values with enums, any kind of data
```rust
enum IpAddrKind {
    V4(String),
    V6(String),
}
//or
enum IpAddr {
	V4(u8, u8, u8, u8),
	V6(String),
}
fn main() {
    let home = IpAddrKind::V4(String::from("127.0.0.1"));
    let loopback = IpAddrKind::V6(String::from("::1"));
}
```

We can also define impl with enum
```rust
enum Message {
	Quit,
	Move { x: i32, y: i32 },
	Write(String),
	ChangeColor(i32, i32, i32),
}
impl Message {
	fn call(&self) {
	// method body would be defined here
	}
}
let m = Message::Write(String::from("hello"));
m.call();
```

#### Option enum
Rust doesnt have null values, but has an enum to encode the concept of null(of a value being present or absent)
```rust
let some_string = Some("a string");
let absent_number: Option<i32> = None;
```
Option<T> (where T can be any type) its a generic type parameter

#### Match 
Match compares a value against a series of patterns and the executes the corresponding arm.
Match stops at the first matched value
```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}
fn value_in_cents(coin: Coin) ->  u32 {
    match coin {
        Coin::Penny => 1{
            println!("Lucky penny");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime  => 10,
        Coin::Quarter => 25,
    }
}
```

A useful feature of match arms is that they can bind to the parts of the values.
```rust
fn value_in_cents(coin: Coin) -> u32 {
match coin {
			Coin::Penny => 1,
			Coin::Nickel => 5,
			Coin::Dime => 10,
			Coin::Quarter(state) => {
				println!("State quarter from {:?}!", state);
				25
		},
	}
}
```
Matching with option
```rust
fn plus_one(x: Option<i32>) ->  Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i+1),
    }
}
fn main() {
    let five = Some(5);
    let six = plus_one(five);
    let none = plus_one(None);
}
```
The _ placeholder. It can match any value, while the () is just the unit, so nothing will happen in the _ case
```rust
match some_u8_value {
	1 => println!("one"),
	3 => println!("three"),
	5 => println!("five"),
	7 => println!("seven"),
	_ => (),
}
```
If we want to match only one case, and use a placeholder for all the other cases. Instead of using match with one arm and a placeholder we can use if let
Its just a syntax sugar in cases we want to match only one case and ignore all the other
```rust
if let Some(3) = some_u8_value{
	println!("three");
}
```

### Modules
A module is a namespace that contains definitions of function or types (like structs and enums), which can be public or private
* **mod** declares a new module
* By default is private, the **pub** keyword makes it visible outside its namespace
* **use** keyword brings modules, or the definitions inside modules, into scope
Create library
```rust
cargo new LIB --lib
```
#### Rules of modules
* If a module X has no submodules, you should put the declarations for X in a file named X.rs
* If a module named X has submodules, you should put the declaration for X in X/mod.rs

### Public
Making a module public. Module must be public and function too
```rust
pub mod client;
mod network;
pub fn connect() {
}
```
#### Rules of privacy
* If an item is public, it can be accessed though any of its parent modules
* If an item is private, it can be accessed only by its immediate parent module and any of the parent's child modules
```rust
mod outermost {
    pub fn middle_function() {}
    fn middle_secret_function() {}
    mod inside {
        pub fn inner_function() {}
        fn secret_function() {}
    }
}
fn try_me() {
    outermost::middle_function(); //Works, although outermost is not public. The rule 
                                  //states that the root module can be accessed
    outermost::middle_secret_function(); //Error, is private
    outermost::inside::inner_function(); //Error, inside is private
    outermost::inside::secret_function();//Error, is private
}
```

### Refering to names
Instead of using the full name `a::series::of::nested_modules();`, we can use the **use** keyword
```rust
pub mod a {
		pub mod series {
			pub mod of {
				pub fn nested_modules() {}
		}
	}
}
use a::series::of;
fn main() {
	of::nested_modules();
}
```
The **use** keyword brings only what we have specified into scope
```rust
use a::series::of::nested_modules;
fn main() {
	nested_modules();
}
```
We can specify exactly what to bring into scope
```rust
enum TrafficLight {
	Red,
	Yellow,
	Green,
}
use TrafficLight::{Red, Yellow};
```
Or bring all 
```rust
use TrafficLight::*;
```
### Super
We can specify the root with
```rust
::client::connect();
```
Or go up one dir with
```rust
codesuper::client::connect();
```

### Common Collections
Most other data structures represent on specific value, while collections can contain multiple values. Unlike array and tupple types, collections are stored in the heap, which means the amount of data does not need to be known at compile time and can grow or shrink.
Each kind of collection has different costs and capabilities
#### Vector
Vector or Vec<T> allows to store more than one value in a single data structure that puts all values next to each other in memory. 
Vector can only store values of the same type

Creating a vector
```rust
let v: Vec<i32> = Vec::new();
```
Rust can infer the type
```rust
let v = vec![1, 2, 3];
```
Updating elements
```rust
let mut v = Vec::new();
v.push(5);
v.push(6);
```
Out of scope
```rust
{
	let v = vec![1, 2, 3, 4];
	// do stuff with v
} // <- v goes out of scope and is freed here
```
Reading from a vector
```rust
let v = vec![1, 2, 3, 4, 5];
let third: &i32 = &v[2];
let third: Option<&i32> = v.get(2);
```
Iterating over a vector
```rust
let v = vec![100, 32, 57];
for i in &v {
	println!("{}", i);
}
```
Iterate over mutable references
```rust
let mut v = vec![100, 32, 57];
for i in &mut v {
	*i += 50;
}
```
If we need to store elements of different types in a vector, we can use enum!
```rust
enum SpreadsheetCell {
	Int(i32),
	Float(f64),
	Text(String),
}
let row = vec![
	SpreadsheetCell::Int(3),
	SpreadsheetCell::Text(String::from("blue")),
	SpreadsheetCell::Float(10.12),
];
```
Note: If we don't know the exhaustive set of types the program will get at runtime t store in a vector, the enum technique won't work. For that we will use traits

### Strings
Create string
```rust
let mut s = String::new();
```
```rust
let data = "initial contents";
let s = data.to_string();
//or
let s = String::from("initial contents");

// the method also works on a literal directly:
let s = "initial contents".to_string();
```
Append
```rust
let mut s = String::from("lo");
s.push('l');
```
Concatenate
```rust
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // note s1 has been moved here and can no longer be used
```
Note: the **+** uses the add method
Better to use format
```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");
let s = format!("{}-{}-{}", s1, s2, s3);
```
In rust we can't access index chars in strings directly due to possible different byte sizes.
But we can iterate over chars
```rust
for c in "नमस्ते".chars() {
	println!("{}", c);
}
```
### Hash Maps
The type HashMap<K, V> stores a mapping of keys of type K to values of Type V
All of the Keys and all of the values must have the same type
Create and add elements
```rust
use std::collections::HashMap;
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```
Another way to construct hash map is using the collect method on a vector of tupples
We use **zip** to create a vector of tupples were Blue is paired with 10, then collect the result
```rust
use std::collections::HashMap;
let teams = vec![String::from("Blue"), String::from("Yellow")];
let initial_scores = vec![10, 50];
let scores: HashMap<_, _> = teams.iter().zip(initial_scores.iter()).collect();
```
Note: the _ will make rust infer the type

#### Ownership
For types that implement the **Copy** trait, such as i32, the values are copied into the hash map. For owned values, such as **String**, the values will be moved and the hash map will be the owner of these values.
```rust
use std::collections::HashMap;
let field_name = String::from("Favorite color");
let field_value = String::from("Blue");
let mut map = HashMap::new();
map.insert(field_name, field_value);
// field_name and field_value are invalid at this point, try using them and
// see what compiler error you get!
```
Note: If we insert references to values into the hash map, the values won't be moved into the hash map. But the references point to must be valid as long the hash map is valid

Accessing values
```rust
use std::collections::HashMap;
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
let team_name = String::from("Blue");
let score = scores.get(&team_name);
```
Iterate
```rust
for (key, value) in &scores{
		println!("{}: {} ", key, value);
	}
```
Overwritting a value
```rust
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25);
```
Only inserting a value if the key has no value
```rust
scores.insert(String::from("Blue"), 10);
scores.entry(String::from("Blue")).or_insert(50);
```
The **or_insert** method returns a mutable reference to the value if it exists, if doesn't exist inserts the parameter as the new value for this key and returns a mutable reference to the new value.

Updating based on old value
```rust
for word in text.split_whitespace(){
		let count = map.entry(word).or_insert(0);
		*count += 1;
} // mutable reference goes out of scope after the for loop
```

## Errors
Rust groups errors in two types:
* Recoverable. Ex: File not found
* Unrecoverable. Ex: location beyond array

Rust doesn't use exceptions. Instead it has the type **Result<T, E>** for recoverable errors and the macro **panic!** for unrecoverable ones.o

### Unrecoverable errors
The panic! macro will print a failure message, unwind and clean up the stack, and then quit.
By default, when a panic occurs, the program starts unwinding (cleaning up the data from each function). But this process is a lot of work, the alternative is an **abort** which ends the program without cleaning up (leaving the cleaning for the OS). 
Note: If we need to make the binary as small as possible we can switch from unwinding to aborting by adding "panic = 'abort' " in **Cargo.toml**

Sometimes a panic can come from a function in the library. To perform a backtrace in our code:
```sh
RUST_BACKTRACE=1 cargo run
```
In order to get backtrace, debug symbols must be enabled. Debug symbols are enabled by default when using `cargo build` or `cargo run` without the `--release` flag

### Recoverable errors
The Result enum is defined with two variants: Ok(T) and Err(E)
The T and E are generic type parameters
```rust
use std::fs::File; 

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(error) => {
            panic!("Problem openning file: {:?}", error)
        },
    } ;
}
```
Matching on different errors
```rust
let f = match f {
		Ok(file) => file,
		Err(ref error) if error.kind() == ErrorKind::NotFound =>  {
				match File::create("hello.txt") {
						Ok(fc) => fc,
						Err(e) => {
								panic!("Tried to create a file but {:?}", e)
						},
				}
		},
		Err(error) =>{
				panic!(
						"Problem {:?}", error
				) 
		},
} ;
```
The condition `if error.kind() == ErrorKind::NotFound` is called a **match guard**. An extra condition on a match arm that further refines the arm's pattern

#### Shortcuts for panic on error
If result is Ok, will return value. If its and error calls panic
```rust
use std::fs::File;
fn main() {
	let f = File::open("hello.txt").unwrap();
}
```
We can also use expect, similar to unwrap but let us provide an error message
```rust
let f = File::open("hello.txt").expect("Failed to open hello.txt");
```

#### Propagating Errors
Instead of handling the error inside the function, we can return the error and let the calling code decide
```rust
fn read_username_from_file() ->  Result<String, io::Error> {
    let f = File::open("hello.txt");

    let mut f = match f {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut s = String::new();
    match f.read_to_string(&mut s) {
        Ok(_) => Ok(s),
        Err(e) => Err(e),
    }
}
```
Shortcut for propagating errors
```rust
fn read_username_from_file() -> Result<String, io::Error> {
	let mut f = File::open("hello.txt")?;
	let mut s = String::new();
	f.read_to_string(&mut s)?;
	Ok(s)
	}
```
The only difference between using **match** and the shorcut **?** is that the shortcut goes through the **from** function, defined in the From trait in the std, which is used to convert errors. When the ? operator calls the from function, the error type received is converted into the error type defined in the return type of the current function.
Note: the **?** operator can only be used in functions that have a return type of **Result** 

### Command line minigrep
Get args / argv. The function in `std::env::args` returns an iterator with the args
```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();
    println!("{:?}", args);
    
}
```
Opening and reading file
```rust
use std::env;
use std::fs::File;
use std::io::prelude::*;

//main blabla
let mut f = File::open(filename).expect("file not found");

let mut contents = String::new();
f.read_to_string(&mut contents)
	.expect("Something wrong reading the file");
```



## Pratical examples / Cool crates
### Regex
Check if pattern is found
```rust
extern crate regex; // add   regex = "1.3.7" to cargo.toml
use regex::Regex;

fn main() {
    let re = Regex::new(r"\w{5}").unwrap(); // get raw string. If we don't use 
                                   // raw we need to double escape "\\w"
    let text = "long text to check";

    println!("Found match? {}",re.is_match(text) );
}
```
Find the match
```rust
fn main() {
    let re = Regex::new(r"(\w{5})").unwrap(); // the () in regex is to return the 
                                              // match instead of true/false
    let text = "long text to check";

    // println!("Found match? {}",re.is_match(text) );
    match re.captures(text) {
        Some(caps) => println!("Found match: {}", caps.get(0).unwrap().as_str()),
        None => println!("Couldn't find a match")
    }
}
```
### term
Terminal colors
```rust
extern crate term;

fn main() {
    let mut t = term::stdout().unwrap();
    t.fg(term::color::GREEN).unwrap();
    write!(t, "hello, ").unwrap();
    
    t.fg(term::color::RED).unwrap();
    writeln!(t, "WORLD ").unwrap();

    t.reset().unwrap();
}
```
### Run command
```rust
use std::process::Command;

fn main() {
		//Build command py
    let mut cmd = Command::new("python");
    cmd.arg("test.py");
		//Build command bash
    //let mut cmd = Command::new("ls");

    //execute command
    match cmd.output(){
        Ok(o) => {
            // o.stdout --> gives vector of bytes
            unsafe {
              // String::from_utf8_unchecked(o.stdout); Doesn't check if output is utf8
                println!("Output: {}", String::from_utf8_unchecked(o.stdout));
            } 
        } 
        Err(e) => println!("Err: {}", e)
    }
}
```


