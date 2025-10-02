```rust
fn give_num(one: i32, two: i32) -> i32 {
    one * two
}
fn print_g_num(one: i32, two: i32) {
    println!("{}", one * two);
}
fn main() {
    let my_num = give_num(9, 8);
    println!("{}", my_num); // 72
    print_g_num(10, 7); // 70
}
```
```rust
fn g_num(one: i16, two: i16) -> i16 {
    let mul_ten = {
        10 * one * two
    };
    mul_ten
}
fn main() {
    println!("{}", g_num(9, 1)); // 90
}
```