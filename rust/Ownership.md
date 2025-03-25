## Ownership Basics
Rust's ownership rules help you manage memory safely and efficiently. Every value has an "owner" and there can only ever be a single owner of a value. When the owner goes out of scope the value is removed.
When dealing with simple values stored on the stack such as integers calling something like this:
```rust
let x = 5;
let y = x;
```
will copy the value of x to y, both variables are valid and can be used. When dealing with data stored on the heap however such as strings:
```rust
let s = String::from("this is a string");
let s2 = s;
```
`s2` becomes the owner of the data and `s` is no longer a valid variable. This code "moves" the value of `s` to `s2`. Trying to use `s` after moving the value will result in a compile error. This means when the data is deallocated from the heap it will only happen once. 
Further note, anytime an automatic copy happens in Rust you can assume it is a "shallow copy/move" Rust will never automatically create a deep copy of data as this is a slow and compute intensive process.
## Deep Copies
If you do want to make a deep copy, meaning copy the data from the heap to another address, you can use the `clone()` function. This function does not invalidate the first pointer.
```rust 
let s = String::from("This is a string!");
let s2 = s.clone();
```
## Passing Data to Functions
[Book Code](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html#ownership-and-functions) 
When a function takes an argument and you pass it a variable
```rust
let s = String::from("this is a string");
a_func(s); // a_func now has ownership and s is invalid
```
a move happens and the variable is no longer valid. The function will call `drop` when it is done with the value.
## References
Instead of passing the value into the function like this `a_func(s)` you can instead pass a "reference" to the value. A reference is like a pointer but it is guaranteed to point to a valid address. To use a reference use `&` so our call becomes `a_func(&s)`. You also need to tell the function it should be expecting a reference and not a value.
```rust
fn main() {
	let s = String::from("This is a string");
	a_func(&s);
}

fn a_func(string_var: &String) {
	// do something with the reference 
}
```
Note that when using a reference inside a function like this Rust will not allow you to modify the value in any way. If you need a function to modify a value passed into it you need to give it ownership of the value or use a mutable reference.
## Mutable References
Example from book: 
```rust
fn main() {
	let mut s = String::from("hello");
	change(&mut s);
}

fn change(some_string: &mut String) {
	some_string.push(", world");
}
```
To make a change like this you need to declare a mutable variable `let mut s` and the function needs to accept a reference to a mutable variable `fn a_func(variable: &mut String)` 
### Side Note On Mutable References
This is how you declare a mutable reference to a value. 
```rust
let mut s = String::from("A String");
let s2 = &mut s;
```
In this code  s2 is now borrowing the value and it knows it is mutable. You can change or mutate the value in s2 but you cannot use `s`. Once `s2` goes out of scope the value returns to `s`. 
This code: 
```rust 
let mut s = String::from("A String");
let s2 = s;
```
Means that the value of `s` has been moved to `s2` and is no longer mutable.
```rust
let mut s = String::from("A String");
let mut s2 = s;
```
The above code moves the value of `s` to `s2` but s2 is also mutable so the value of `s2` can be changed.
```rust
let mut s = String::from("A String");
let s2 = &s;
```
In the above code `s2` is now a reference to s but NOT a mutable reference, `s2` can access the value but cannot change it in any way.

It is also important to remember that you may not have two mutable references to the same value at the same time. This prevents "data races" where one reference is attempting to write and one is attempting to read which can cause unexpected results and bugs. You can however have any number of immutable references to the same value at one time. 