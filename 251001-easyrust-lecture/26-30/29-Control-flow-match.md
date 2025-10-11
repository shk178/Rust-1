```rust
fn main() {
    let my_number = 5;
    if my_number == 7 {
        println!("It's seven");
    } else {
        println!("{}", my_number);
    }
    //5
}
```
- 조건문에 괄호 안 써도 된다.
- `if`, `else if`, `else`
- `&&` and, `||` or
```rust
fn main() {
    //match
    let my_number: u8 = 5;
    /* match my_number {
        0 => println!("zero"),
        1 => println!("one"),
    }
    error[E0004]: non-exhaustive patterns: `2_u8..=u8::MAX` not covered
     --> src/main.rs:4:11
     */
}
```
- rust에서는 match 많이 쓴다.
- match 표현식은 반드시 모든 가능한 경우를 다 처리해야한다.
- u8 타입은 0부터 255까지의 값을 가질 수 있는데
- 0~255 다 안 다루면 컴파일 에러 난다.
```rust
//모든 값을 명시적으로 처리
match my_number {
    0 => println!("zero"),
    1 => println!("one"),
    2..=255 => println!("other"),
}
//_를 사용해서 나머지 모든 경우 처리
match my_number {
    0 => println!("zero"),
    1 => println!("one"),
    _ => println!("other"),
}
//_는 "그 외의 모든 경우"를 의미하는 wildcard 패턴
```
```rust
fn main() {
    //expression-based language
    let my_number: u8 = 5;
    let second_number = match my_number {
        0 => 23,
        1 => 25,
        _ => 27
    };
    println!("{}", second_number); //27
}
```