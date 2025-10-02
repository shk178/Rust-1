```rust
fn main() {
    let doesnt_print = ();
    println!("This will not print: {}", doesnt_print);
    // error[E0277]: `()` doesn't implement `std::fmt::Display`
    // `()` cannot be formatted with the default formatter
    // note: in format strings you may be able to use `{:?}` (or {:#?} for pretty-print) instead
}
```
# () - Unit Type
- 아무 값도 없음을 나타내는 타입
```rust
// Rust (일반적 - 생략)
fn do_something() {  // 반환 타입 없음 = () 반환
    println!("Hello");
}
// Rust (명시적)
fn do_something() -> () {  // ()를 반환
    println!("Hello");
}
```
- 다른 언어의 void와 비교
```c
// C/Java
void doSomething() {  // "아무것도 반환 안 함"
    printf("Hello");
}
```
- Empty Tuple이라고도 부른다.
```rust
// 일반 tuple
let tuple1 = (1,);           // 원소 1개
let tuple2 = (1, 2);         // 원소 2개
let tuple3 = (1, 2, 3);      // 원소 3개
// Empty tuple
let tuple0 = ();             // 원소 0개 = unit type
```
# Expression-Based Language
- 거의 모든 것이 값을 반환한다는 의미
- 대부분의 언어는 statement-based지만, Rust는 expression-based다.
- rust는 더 간결하고 함수형 스타일의 코드 작성 가능하다.
- statement-based는 if/else와 블록이 값을 반환하지 않는다.
```c
// C - if는 statement (값 없음)
int x;
if (condition) {
    x = 5;
} else {
    x = 10;
}
```
- expression-based는 반환한다.
```rust
// Rust - if는 expression (값 반환)
let x = if condition { 5 } else { 10 };
```
- match 값 반환
```rust
let number = 3;
let description = match number {
    1 => "one",
    2 => "two",
    3 => "three",
    _ => "other",
};  // description = "three"
```
- block 값 반환
```rust
let result = {
    let a = 10;
    let b = 20;
    a + b  // 세미콜론 없음 → 30 반환
};  // result = 30
```
- loop 값 반환
```rust
let result = loop {
    break 42;  // loop에서 값 반환
};  // result = 42
```
- `base * (블록 반환값)`을 반환하는 함수
```rust
fn calculate_price(base: f64, age: i32) -> f64 {
    base * {
        let discount = get_discount(age);
        1.0 - discount
    }
}
// 같은 동작
fn calculate_price(base: f64, age: i32) -> f64 {
    let multiplier = {
        let discount = get_discount(age);
        1.0 - discount
    };
    base * multiplier
}
```