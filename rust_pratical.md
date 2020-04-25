---
title: Rust_pratical
date: 2020-04-10 08:13:35+01:00
author: Gaxpert
---

# Rust Pratical

## Command line args
### Standard argument get
Get args / argv. The function in `std::env::args` returns an iterator with the args
```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();
    println!("{:?}", args);
    
}
```
### Arguments, clean config
On main file
```rust
use std::env;
use std::process;

// -- snip -- //

    let config = Config::new(env::args()).unwrap_or_else(|err|{
        eprintln!("Problem passing arguments: {}", err);
        process::exit(1);
    });
```
On lib file:
```rust
pub struct Config {
    pub query: String,
    pub filename: String,
    pub case_sensitive: bool,
}

impl Config {
    pub fn new(mut args: std::env::Args) ->  Result<Config, &'static str> {

        args.next(); //First arg is filename
        let query = match args.next(){
            Some(arg) => arg,
            None => return Err("Didn't get a query string"),
        };

        let filename = match args.next(){
            Some(arg) => arg,
            None => return Err("Didn't get a filename"),
        };

        let mut case_sensitive = env::var("CASE_INSENSITIVE").is_err();

        Ok(Config { query, filename, case_sensitive })
    }
}
```

### Arguments with clap library
```rust
// (Full example with detailed comments in examples/01b_quick_example.rs)
//
// This example demonstrates clap's full 'builder pattern' style of creating arguments which is
// more verbose, but allows easier editing, and at times more advanced options, or the possibility
// to generate arguments dynamically.
extern crate clap;
use clap::{Arg, App, SubCommand};

fn main() {
    let matches = App::new("My Super Program")
                          .version("1.0")
                          .author("Kevin K. <kbknapp@gmail.com>")
                          .about("Does awesome things")
                          .arg(Arg::with_name("config")
                               .short("c")
                               .long("config")
                               .value_name("FILE")
                               .help("Sets a custom config file")
                               .takes_value(true))
                          .arg(Arg::with_name("INPUT")
                               .help("Sets the input file to use")
                               .required(true)
                               .index(1))
                          .arg(Arg::with_name("v")
                               .short("v")
                               .multiple(true)
                               .help("Sets the level of verbosity"))
                          .subcommand(SubCommand::with_name("test")
                                      .about("controls testing features")
                                      .version("1.3")
                                      .author("Someone E. <someone_else@other.com>")
                                      .arg(Arg::with_name("debug")
                                          .short("d")
                                          .help("print debug information verbosely")))
                          .get_matches();

    // Gets a value for config if supplied by user, or defaults to "default.conf"
    let config = matches.value_of("config").unwrap_or("default.conf");
    println!("Value for config: {}", config);

    // Calling .unwrap() is safe here because "INPUT" is required (if "INPUT" wasn't
    // required we could have used an 'if let' to conditionally get the value)
    println!("Using input file: {}", matches.value_of("INPUT").unwrap());

    // Vary the output based on how many times the user used the "verbose" flag
    // (i.e. 'myprog -v -v -v' or 'myprog -vvv' vs 'myprog -v'
    match matches.occurrences_of("v") {
        0 => println!("No verbose info"),
        1 => println!("Some verbose info"),
        2 => println!("Tons of verbose info"),
        3 | _ => println!("Don't be crazy"),
    }

    // You can handle information about subcommands by requesting their matches by name
    // (as below), requesting just the name used, or both at the same time
    if let Some(matches) = matches.subcommand_matches("test") {
        if matches.is_present("debug") {
            println!("Printing debug info...");
        } else {
            println!("Printing normally...");
        }
    }

    // more program logic goes here...
}
```


## Log
Using pretty_env_looger:
Add dependencies
```
log = "0.4"
pretty_env_logger = "0.4"
```
Main
```rust
extern crate pretty_env_logger;
#[macro_use] extern crate log;

fn main() {
    pretty_env_logger::init();
    info!("such information");
    warn!("o_O");
    error!("much error");
}
```
Normal run will only show error
use env variable to show full
`RUST_LOG=trace cargo run`


## File I/O

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

### Image metadata
```rust
extern crate rexiv2
// --snip-- //
let file = "myimage.jpg";
let tag = "Iptc.Application2.Keywords";
let meta = rexiv2::Metadata::new_from_path(&file).unwrap();
println!("{:?}", meta.get_tag_multiple_strings(tag));
```


## Time
### Time sleep
```rust
use std::{thread, time};
// --snip-- //

thread::sleep(time::Duration::from_secs(1));
```

### Progress bar
Simple progress bar on iterator. 
Check [indicatif](https://github.com/mitsuhiko/indicatif) for more info
```rust
extern crate indicatif;

use indicatif::{ProgressBar, ProgressIterator};
use std::{thread, time};


fn main() {
    for _ in (0..1000).progress(){
		// do stuff
    }
}
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


### Check env variable
We are using **is_err()** because we only care if the variable is set or unset. If its an error it will set case_sensitive at false, if it isnt, and the env var exists, it will be set to true
```rust
use std::env;

            let case_sensitive = env::var("CASE_INSENSITIVE").is_err();
```

#### Filter iterators
We take ownership of a vector of shoes and shoe size. Next we call **into_iter** to create an iterator that takes ownership of the vector. Then we call filter to adapt that iterator into a new iterator that only contains elements for which the closure returns true. The closure captures the show_size env parameter and compares the value, keeping the shoe size we specified. Finally we call **collect** to gather the result
```rust
fn shoes_in_my_size(shoes: Vec<Shoe>, shoe_size: u32) ->  Vec<Shoe> {
    shoes.into_iter()
        .filter(|s| s.size == shoe_size)
        .collect()
}
```
Ex2: minigrep. We pass a query and the contents to search. Return the lines in contents that match query
```rust
pub fn search<'a>(query: &str, contents: &'a str) ->  Vec<&'a str> {
    contents.lines()
        .filter(|line| line.contains(query))
            .collect()
}
```

#### Iterate through lines string
```rust
for line in contents.lines(){
        if line.contains(query){

        }
}
```

#### Iterators vs Loops
Iterators are slightly faster than loops since, although a high level abstraction, gets compiled down to roughly the same code as if you wrote the low-level code yourself. Iterators are one of Rust's **zero-cost abstractions**.

### Print to standard error
```rust
eprintln!("Problem parsing arguments: {}", err);
```


Exit process cleanly (Without rust errors)
```rust
use std::process;
// --snip---
process::exit(1);
```
