```rust
enum Star {
    BrownDwarf = 10, // 0 대신 10
    RedDwarf = 50, // 1 대신 50
    Test = 1000, // 1000
    YellowStar = 100, // 100
    RedGiant, // ?
}

fn main() {
    use Star::*;
    let svec = vec![BrownDwarf, RedDwarf, Test, YellowStar, RedGiant];
    for star in svec {
        println!("{}", star as u32);
    }
}
/*
10
50
1000
100
101
 */
```
```rust
#[derive(Debug)]
enum Number {
    Positive(u32),
    CouldBeNeg(i32),
}

fn get_number(input: i32) -> Number {
    match input.is_positive() {
        true => Number::Positive(input as u32),
        false => Number::CouldBeNeg(input as i32)
    }
}

fn main() {
    let my_vec = vec![get_number(-800), get_number(8)];
    for item in my_vec {
        println!("{:?}", item);
    }
}
/*
CouldBeNeg(-800)
Positive(8)
 */
```