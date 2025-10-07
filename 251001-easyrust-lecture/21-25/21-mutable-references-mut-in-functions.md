# 값을 반환하는 것보다 참조 사용
- 소유권을 다시 받아 값을 재사용
```rust
fn print_country(country_name: String) -> String {
    println!("{}", country_name);
    country_name // 다시 반환
}
fn main() {
    let country = String::from("Austria");
    let country = print_country(country); // 반환된 값을 다시 받음
    print_country(country); // OK
}
```
- 읽기 전용으로 빌려주기
```rust
fn print_country(country_name: &String) { // &String은 불변 참조
    println!("{}", country_name);
}
fn main() {
    let country = String::from("Austria");
    print_country(&country); // 참조를 넘김
    print_country(&country); // OK
}
```
- 가변 참조로 빌려주기
```rust
fn add_hungary(country_name: &mut String) { // &mut String은 가변 참조
    country_name.push_str("-Hungary");
    println!("Now it says: {}", country_name);
}
fn main() {
    let mut country = String::from("Austria");
    add_hungary(&mut country); // 가변 참조 넘김
}
```
- 헷갈릴 수 있는 예시
- 소유권을 넘기고 함수 내부에서 가변으로 선언
```rust
fn adds_hungary(mut country: String) {
    country.push_str("-Hungary");
    println!("{}", country);
}
fn main() {
    let country = String::from("Austria");
    adds_hungary(country); // OK
}
// 함수에 소유권을 넘겼기 때문에, 함수 내부에서 mut로 선언해도 괜찮다.
// 원래의 country는 더 이상 사용할 수 없다.
```
# &mut String
- String을 직접 넘김 → &mut String 필요
```rust
fn add(name: &mut String) {
    name.push_str(" is great!");
    println!("{}", name);
}
fn main() {
    let country = "캐나다".to_string();
    add(country); // error[E0308]: mismatched types
    add(country); // error[E0308]: mismatched types
    // expected `&mut String`, found `String`
}
```
- 가변 참조를 넘기려 했지만 country는 불변
```rust
fn add(name: &mut String) {
    name.push_str(" is great!");
    println!("{}", name);
}
fn main() {
    let country = "캐나다".to_string();
    add(&mut country);
    add(&mut country);
    // error[E0596]: cannot borrow `country` as mutable, as it is not declared as mutable
}
```
- String을 직접 넘김 → &mut String 필요
```rust
fn add(name: &mut String) {
    name.push_str(" is great!");
    println!("{}", name);
}
fn main() {
    let mut country = "캐나다".to_string();
    add(country); // error[E0308]: mismatched types
    add(country); // error[E0308]: mismatched types
    // expected `&mut String`, found `String`
}
```
- 성공: 가변 변수에 가변 참조를 넘김
```rust
fn add(name: &mut String) {
    name.push_str(" is great!");
    println!("{}", name);
}
fn main() {
    let mut country = "캐나다".to_string();
    add(&mut country); // 캐나다 is great!
    add(&mut country); // 캐나다 is great! is great!
}
```
- 불변 참조를 넘김
```rust
fn add(name: &mut String) {
    name.push_str(" is great!");
    println!("{}", name);
}
fn main() {
    let country = "캐나다".to_string();
    add(&country); // error[E0308]: mismatched types
    add(&country); // error[E0308]: mismatched types
    // expected mutable reference `&mut String`
    // found reference `&String`
}
```
- 불변 참조를 넘김
```rust
fn add(name: &mut String) {
    name.push_str(" is great!");
    println!("{}", name);
}
fn main() {
    let mut country = "캐나다".to_string();
    add(&country); // error[E0308]: mismatched types
    add(&country); // error[E0308]: mismatched types
    // expected mutable reference `&mut String`
    // found reference `&String`
}
```
# 자동 참조 변환(deref coercion)
- 함수 호출에서 참조 타입을 기대할 때
- 명시적으로 &나 &mut를 쓰지 않아도 자동으로 참조를 만들어준다.
- 불변 참조(&T)에 대해서만 작동한다.
```rust
fn print_name(name: &String) {
    println!("{}", name);
}
fn main() {
    let name = String::from("Rust");
    print_name(&name); // 명시적 참조
    print_name(name);  // 자동 참조 변환: String → &String
}
// print_name(name)에서 Rust는 자동으로 &name으로 변환해준다.
```
- 가변 참조는 반드시 명시적으로 &mut를 써야 한다.
```rust
fn modify(name: &mut String) {
    name.push_str(" is awesome!");
}
fn main() {
    let mut name = String::from("Rust");
    modify(name); // 오류: expected `&mut String`, found `String`
}
```
- 왜 가변 참조는 자동 변환이 안 될까
- 데이터 경쟁과 동시성 문제를 방지하기 위해
- 가변 참조는 단 하나만 존재해야 한다는 규칙 때문이다.
# mut in functions
```rust
fn adds_hungary(mut country: String) { // &String이나 &mut String이 아님
    country.push_str("-Hungary");
    println!("{}", country);
}
fn main() {
    let country = String::from("Austria"); // country는 mut 아님
    adds_hungary(country); // OK
}
```
- take by value, declare as mutable