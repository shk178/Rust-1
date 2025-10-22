```rust
#[derive(Debug)]
enum Option<T: std::fmt::Debug> {
    None,
    Some(T),
}
fn take_fifth(input: Vec<i32>) -> i32 {
    input[4]
}
fn safe_take_fifth(input: Vec<i32>) -> Option<i32> {
    if input.len() < 5 {
        Option::None
    } else {
        Option::Some(input[4])
    }
}
fn main() {
    let new_vec = vec![1, 2];
    //let value = take_fifth(new_vec);
    //thread 'main' panicked at src/main.rs:6:10:
    //index out of bounds: the len is 2 but the index is 4
    let value_1 = safe_take_fifth(new_vec);
    let other_vec = vec![1, 2, 3, 4, 5];
    let value_2 = safe_take_fifth(other_vec);
    println!("{:?} {:?}", value_1, value_2); // None Some(5)
}
```