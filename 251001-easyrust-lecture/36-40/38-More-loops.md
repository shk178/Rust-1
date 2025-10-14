```rust
fn main() {
    let mut counter = 0;
    while counter != 5 {
        counter += 1;
        println!("{}", counter);
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
    for number in 0..3 {
        println!("{}", number);
    }
}
/*
0
1
2
 */
```
- 0..3: 3을 제외해서 exclusive range
- 0..=3: inclusive range
```rust
fn main() {
    for _ in 0..=3 {
        //Rust에서 _는 “이 변수는 무시하겠다”는 의미
        println!("1");
    }
    for _number in 0..=3 {
        //_number는 “이 변수는 이름은 있지만, 사용하지 않아도 경고를 내지 말라”는 의미
        println!("1");
        println!("현재 숫자: {_number}"); //_number로 사용
    }
}
```
```rust
fn main() {
    let mut counter = 5;
    let my_number = loop {
        counter += 1;
        if counter % 53 == 3 {
            break counter; //리턴
        }
    };
    println!("{}", my_number); //56
}
```