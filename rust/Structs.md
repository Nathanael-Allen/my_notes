## Defining Structs

Structs are a way to group data of different types, similar to a Typescript object. To instantiate a struct use the following syntax:
```rust
struct Person {
	employed: bool,
	age: u8,
	name: String,
}
```
To create an instance of a struct use the following syntax, note you do not need to assign values in the order they are declared in the struct.
```rust
let new_person = Person {
	employed: true,
	name: String::from("Natty"),
	age: 28,
}
```
You can declare an instance of a struct as mutable `let mut new_person = Person{}`. When an instance is mutable you can change it's fields with dot notations `new_person.employed=false`.
## Factory Function
You can use a factory function to create structs easily like this:
```rust
fn create_person(employed: bool, name: String, age: u8) -> Person {
	employed,
	name,
	age,
};

let new_person = create_person(true, "Natty", 28);
```
You can also copy the fields of one struct instance into another like so:
```rust
let p1 = Person {..p2};
```
Note that when you copy an instance like this you are "moving" the values, so for instance the name field would be moved from `p2` to `p1`  making p1 invalid since the string value is stored on the heap.
## Tuple Structs
Rust also has "tuple structs" they behave like structs but do not have named fields. Instead you declare them just with types. `struct Point(i32, i32)` 
### Destructuring Tuple Structs
You can destructure tuple structs using this syntax
```rust
let Point(x, y) = p1;
```
You do have to declare the type when doing this unlike regular tuples.