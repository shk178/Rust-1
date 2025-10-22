```rust
fn print_and_give_item() -> i32 {
    let number = 9;
    println!("{}", number);
    9
}
//fn give_thing(input: T) -> T {
//    input
//}
//error[E0412]: cannot find type `T` in this scope
fn give_thing<T>(input: T) -> T {
    input
}
fn main() {
    let x = print_and_give_item();
    let y = give_thing(String::from("take this thing"));
    println!("{} {}", x, y);
    //9
    //9 take this thing
}
```
```rust
fn print_thing<T>(input: T) {
    //println!("{}", input);
    //error[E0277]: `T` doesn't implement `std::fmt::Display`
}
fn print_thing_2<T: std::fmt::Display>(input: T) {
    println!("{}", input);
}
fn main() {
    //print_thing(String::from("take this thing"));
    print_thing_2(String::from("take this thing")); // take this thing
}
```
```rust
struct Book;
fn print_thing<T>(input: T) {
    //println!("{}", input);
    //error[E0277]: `T` doesn't implement `std::fmt::Display`
}
fn print_thing_2<T: std::fmt::Display>(input: T) {
    println!("{}", input);
}
fn main() {
    //print_thing(String::from("take this thing"));
    print_thing_2(String::from("take this thing")); // take this thing
    //print_thing_2(Book);
    //error[E0277]: `Book` doesn't implement `std::fmt::Display`
}
```