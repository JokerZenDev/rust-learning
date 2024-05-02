# Rust learning

## Introduction

I start learning Rust on the official Rust book https://doc.rust-lang.org/book/. I use this repo to document my progress.

## Cargo commands

Most useful cargo commands are:
- cargo new <project_name>: to create a new project
- cargo build: to build your project and produce an executable
- cargo run: to build and run your project and produce an executable
- cargo check: to quickly checks your code to make sure it compiles but doesn’t produce an executable
- cargo build --release: to compile your project with optimizations
- cargo add <crate_name>: to add a library crate as a dependency to your project
- cargo doc --open: to build documentation provided by all your dependencies locally and open it in your browser
- cargo update: to update the crates in your dependencies

## Variables

There are some different way to declare a variables:
- let x = 5: declare a new immutable variable, it can be redeclare multiple times using different types, each declare is valid for that scope
- let mut x = 5: declare a new mutable variable, it can change value but cannot change type
- const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3: declare a constant and the type of the value must be annotated

## Functions

There are some costraints:
- to declare a functions must use fn keyword
- you must declare the type of each parameter of a function
- statements are instructions that perform some action and do not return a value (ex: let x = 5)
- expressions evaluate to a resultant value (ex: {let x = 5; x + 1})
- functions with return values must declare their type after an arrow (->), you can return early by using the return or at the last expression implicitly without semicolon

## Control flow

### if

There are some constraints:
- the condition must be boolean
- the if and else arms must have the same value type in variable based on the outcome of the if expression
```rust
fn main() {
    let number = if condition { 5 } else { 6 };
    let number = if condition { 5 } else { "six" }; // This is an error

	if number { // This is an error
        println!("number is divisible by 4");
	}

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

### loop

The loop keyword tells Rust to execute a block of code over and over again forever or until you explicitly tell it to stop.

There are some important keywords:
- break: to stop executing the loop
- break some_value: to stop the loop and return a value
- continue: to skip over any remaining code in this iteration of the loop and go to the next iteration
- 'name_loop: loop: to specify a loop label on a loop that you can then use with break or continue

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break; // Stop the second loop
            }
            if count == 2 {
                break 'counting_up; // Stop the first loop
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```

### while

There is a constraint:
- the condition must be boolean

```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```

### for

You can use a for loop and execute some code for each item in a collection and you can use rev to reverse the range.

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }

    for number in (1..4).rev() {
        println!("{number}!");
    }
}
```

## Ownership

Ownership is a set of rules that govern how a Rust program manages memory.
Rules:
- Each value in Rust has an owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

There are 3 methods to assign a variable to another variable:
- copy: bind the value stored on the stack, after bind the first and second variables are valid
- move: bind the value stored on the heap to another variable, after bind the first variable is invalid
- clone: deeply copy the heap data

## References and borrowing

A reference is like a pointer in that it’s an address we can follow to access the data stored at that address; that data is owned by some other variable.
Unlike a pointer, a reference is guaranteed to point to a valid value of a particular type for the life of that reference.
Must use & to pass a reference and indicate that the type of the parameter is a reference.
A reference by default is immutable like a variable.
Borrowing: to make a reference mutable must use &mut to pass a mutable reference and indicate that the type of the parameter is a mutable reference.
To prevent dangling at any given time, you can have either one mutable reference or any number of immutable references.

## Slice type

A slice is a reference to part of a String, string litreal or array.
A slice contains a pointer to the byte at the start index of a variable with a length value of the slice.

```rust
let slice = &var_name[start_index..end_index]
let slice_first_part = &var_name[..end_index]
let slice_last_part = &var_name[start_index..]
let slice_all_part = &var_name[..]
```