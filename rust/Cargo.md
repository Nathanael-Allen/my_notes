## Creating a Cargo project
To create a cargo project use `cargo new name-of-project`
Compile a project with `cargo build -flags` using the `-r` or `--release` flag will build the project with optimizations. This makes the compile time longer but will speed up the code for production.
You can run the project with `rustc` or `cargo` or use `cargo run` to build and run the project all in one command.

### Cargo.yaml
Add packages to your project with `cargo add package-name` or by adding them directly to the Cargo.yaml file that is created when you use `cargo new` 
