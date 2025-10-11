```rust
fn match_colours(rbg: (u32, u32, u32)) {
    match rbg {
        (r, _, _) if r <10 => println!("Not much red"),
        (_, b, _) if b <10 => println!("Not much blue"),
        (_, _, g) if g <10 => println!("Not much green"),
        _ => println!("Every colour has at least 10")
    }
}
fn main() {
    match_colours((5, 10, 10)); //Not much red
    match_colours((10, 5, 10)); //Not much blue
    match_colours((10, 10, 5)); //Not much green
    match_colours((5, 5, 5)); //Not much red
    match_colours((10, 10, 10)); //Every colour has at least 10
}
```
```java
fn main() {
    let my_number = 10;
    let some_variable = match my_number {
        10 => 8,
        _ => "Not ten" //error[E0308]: `match` arms have incompatible types
    };
}
fn main() {
    let my_number = 10;
    let some_variable = if my_number==10 {8} else {"Not ten"};
    //error[E0308]: `if` and `else` have incompatible types
}
```
```rust
fn match_number(input: i32) {
    match input {
        0..=10 => println!("0~10"),
        _ => println!(">10")
    }
}
fn main() {
    match_number(10); //0~10
    match_number(100); //>10
}
```
```rust
fn match_number(input: i32) {
    match input {
        0..=10 => println!("{}", input),
        _ => println!(">10")
    }
}
fn main() {
    match_number(10); //10
    match_number(100); //>10
}
```
```rust
fn match_number(input: i32) {
    match input {
        number @ 0..=10 => println!("{}", number),
        _ => println!(">10")
    }
}
fn main() {
    match_number(10); //10
    match_number(100); //>10
}
```
```rust
fn match_number(input: i32) {
    match input {
        number => println!("{}", number),
        _ => println!(">10") //warning: unreachable pattern
    }
}
fn main() {
    match_number(10); //10
    match_number(100); //100
}
```