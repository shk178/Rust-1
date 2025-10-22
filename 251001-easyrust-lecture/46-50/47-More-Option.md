```rust
fn safe_take_fifth(input: Vec<i32>) -> Option<i32> {
    if input.len() < 5 {
        None
    } else {
        Some(input[4])
    }
}
fn main() {
    let new_vec = vec![1, 2];
    let value_1 = safe_take_fifth(new_vec);
    let other_vec = vec![1, 2, 3, 4, 5];
    let value_2 = safe_take_fifth(other_vec);
    //println!("{} {}", value_1.unwrap(), value_2.unwrap());
    //thread 'main' panicked at src/main.rs:13:31:
    //called `Option::unwrap()` on a `None` value
    match value_1 {
        Some(number) => println!("I got a number: {}", number),
        None => println!("There was nothing inside"),
    } // There was nothing inside
    match value_2 {
        Some(number) => println!("I got a number: {}", number),
        None => println!("There was nothing inside"),
    } // I got a number: 5
    if !value_1.is_some() {
        println!("There was nothing inside");
    } // There was nothing inside
    if value_2.is_some() {
        println!("I got a number: {}", value_2.unwrap());
    } // I got a number: 5
    value_1.expect("Needed at least five items - make sure Vec has at least five");
    //thread 'main' panicked at src/main.rs:30:13:
    //Needed at least five items - make sure Vec has at least five
    value_2.expect("Needed at least five items - make sure Vec has at least five");
}
```