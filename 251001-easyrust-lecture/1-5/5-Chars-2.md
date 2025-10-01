- .len() gives the size of the string in bytes.
```rust
use std::mem::size_of;
fn main() {
    println!("{}", size_of::<char>()); // 4
    println!("{}", "a".len()); // 1
    println!("{}", "ß".len()); // 2
    println!("{}", "ßßß".len()); // 6
    println!("{}", "国".len()); // 3
    println!("{}", "𓅱".len()); // 4
}
```
- .chars().count() is about the size in characters.
```rust
fn main() {
    let slice = "Hello!";
    println!("Slice is {} bytes and also {} characters.", slice.len(), slice.chars().count()); // 6, 6
    let slice2 = "안녕!";
    println!("Slice2 is {} bytes but only {} characters.", slice2.len(), slice2.chars().count()); // 7, 3
}
```