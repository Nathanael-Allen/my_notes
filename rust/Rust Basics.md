## Variables and Mutability
All variables in Rust are immutable by default example:
```rust
let x = 5; // Immutable
let mut y = 5; // Mutable
```
### Constants
The name convention for constants in Rust is `ALL_UPPERCASE` with underscores in between words.
Constants cannot be assigned to a value that is computed at runtime, i.e. the result of adding two variables together. Constants types must also be explicitly declared 
```rust
const SOME_VAR: u32 = 60100
```
Use constants when you need to store a value that is important to multiple parts of your program and will never change during runtime. You can reference it in many places and if it ever needs to change you only have to update the value in one place.
## Casting and Overshadowing
When using `let` to declare variables you may reassign the value of the variable by calling `let` again, example:
```rust
let x = 5;
let x = 10;
```
Now the value x is assigned to the value 10. This is also how you type cast Rust.
```rust
let x: i32 = 5;
let x: f64 = 5.6;
```
These declarations are scoped to the code block they are in so you can nest code blocks and the variable will be reassigned back to  the most recent assignment in the call stack.
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
The first print statement will show the value as 12 while the second print statement will show the value as being 6 again. Declaring variables in this way is different from using `mut` because it requires you to deliberately use `let` again to change the value ensuring you don't accidentally reassign a mutable variable.
## Data Types
Most types function as expected and mimic the types I am familiar with. Rust has Tuples, a tuple is a group of values that do not need to share a type. Tuples are a fixed size and cannot be changed. Access items in a tuple with normal `[index]` syntax. Declare a tuple like this: 
```rust
let my_tuple: (u32, f64, bool) = (40, 8.99, true);
```
Arrays work much the same as in C# or C. They are fixed length and you can declare the length and type in the variable declaration `let my_array: [i32, 3] = [4, 5, 90];`  <-- this declares a list of i32 values that is 3 values in size.
### Tuples
Tuples are a very important type in Rust as they are the easiest way to group related data. You can pass tuples to functions like this:
```rust
fn calculate_area(dimensions: (u32, u32)) -> u32 {
	dimensions.0 * dimensions.1
}
```
## Statements vs. Expressions
A statement in Rust is something that is just a declaration and does not evaluate to anything. For instance assigning a variable name to a number is a statement but adding two variables together is an expression as it returns a value. 
You can write a statement that assigns something to the value of an expression. Example:
```rust
fn main() {
	let y = {
		let x = 3;
		x + 1
	};
}
```
By omitting the semicolon after `x + 1` rust will treat this as a return statement and the value assigned to y will be 4. Code inside brackets like this is an expression. Rust treats the last expression of a code block as the return value implicitly. You can use the `return` keyword to return early.
## Return Values
In rust you must type annotate your return value explicitly using arrow notation.
```rust
fn add(x: i32, y: i32) -> i32 {
	x + y
}
```
Note there is no semicolon at the end of the expression because we want to return the result of `x + y` 
## if Statements
`if` statements in Rust look a bit like Python in the sense that you don't need to put the evaluation inside parentheses. But you do need to put the code block inside curly braces
Example:
```rust
if x < 5 {
	// do something
} else {
	// do something else
}
```
Rust will **not** automatically convert an expression to a bool like JavaScript will so you must explicitly tell it what you want. You can use inline `if` statements similar to the ternary operator in JavaScript. The syntax looks like: 
```rust
let x = if condition { value } else { different_value };
```
NOTE The values returned MUST be the same type.
## Loops
There are three types of loops in Rust.
- loop (Loops until it is explicitly told to stop with a return or break)	
- while (Loops while a condition is satisfied)
- for (Loops )
`while` and `for` loops look very similar to Python:
```rust
while condition == met {
	// do thing
}

for element in array {
	// do a thing
}
```
### Labeling loops
If you have multiple loops nested inside one another calling `break` or `continue` will apply to the innermost loop that the call happens in. However Rust allows you to label loops like so: `'loop_label: loop` You must prepend the label with a single quote. You can then call `break` or `continue` on a specific label like so: `break 'loop_label` 
