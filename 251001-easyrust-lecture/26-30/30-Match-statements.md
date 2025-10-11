```rust
fn main() {
    let sky = "cloudy"; // &str
    let temperature = "warm";
    match (sky, temperature) {
        ("cloudy", "cold") => println!("1"),
        ("clear", "warm") => println!("2"),
        _ => println!("3")
    }; // 3
}
```
```rust
fn main() {
    let children = 5;
    let married = true;
    match (children, married) {
        (c, m) if m == false => println!("not married with {} children", c),
        (c2, m2) if c2 >= 0 && m2 => println!("married with {} children", c2),
        _ => println!("Some other type of marriage and children combination")
    }; // married with 5 children
}
```