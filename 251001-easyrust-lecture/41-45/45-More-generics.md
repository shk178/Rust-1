```rust
// 컴파일러에게 프린트, 비교 약속
fn compare_and_display<T: std::fmt::Display, G: std::fmt::Display + std::cmp::PartialOrd>(statement: T, num_1: G, num_2: G) {
    println!(
        "{}! Is {} greater than {}? {}",
        statement, num_1, num_2, num_1 > num_2
    );
}
fn main() {
    compare_and_display("Listen up!", 9, 8); // Listen up!! Is 9 greater than 8? true
}
```
```rust
fn compare_and_display<T, G>(statement: T, num_1: G, num_2: G)
where T: std::fmt::Display, G: std::fmt::Display + std::cmp::PartialOrd,
{
    println!(
        "{}! Is {} greater than {}? {}",
        statement, num_1, num_2, num_1 > num_2
    );
}
fn main() {
    compare_and_display("Listen up!", 9, 8); // Listen up!! Is 9 greater than 8? true
}
```