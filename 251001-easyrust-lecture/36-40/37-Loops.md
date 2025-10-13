```rust
fn main() {
    let mut counter = 0;
    loop {
        counter += 1;
        println!("{}", counter);
        if counter == 5 {
            break;
        }
    }
}
/*
1
2
3
4
5
 */
```
```rust
fn main() {
    let mut counter = 0;
    let mut counter2 = 0;
    'first_loop: loop {
        counter += 1;
        print!("{}", counter);
        if counter >= 3 {
            println!();
            'second_loop: loop { // warning: unused label
                counter2 += 1;
                print!("{}", counter2);
                if counter2 >=3 {
                    break 'first_loop;
                }
            }
        }
    }
}
/*
123
123
 */
```