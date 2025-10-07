# It's trivial to copy the bytes.
- 기본 타입은 카피를 안 할 이유가 없다.
- 카피에 비용이 덜 든다.
- 카피하면 소유권x - 코드 작성 쉽다.
# Copy 타입
- i32, f64, bool, char 등은 rust에서 자동으로 복사된다.
- 스택에 저장되고, 복사 비용이 거의 없어서 함수에 넘길 때 소유권 이동x 복사한다.
```rust
fn prints_number(number: i32) {
    println!("{}", number);
}
fn main() {
    let my_number = 8;
    prints_number(my_number); // 복사본이 들어감
    prints_number(my_number); // 원본 그대로 또 사용 가능
}
```
# Copy가 아닌 타입: String
- String은 힙(heap)에 데이터를 저장
- 복사하지 않고 소유권을 이동(move)시킨다.
```rust
fn prints_country(country_name: String) {
    println!("{}", country_name);
}
fn main() {
    let country = String::from("Kiribati");
    prints_country(country);      // country의 소유권이 넘어감
    prints_country(country);      // 오류: country는 이미 move됨
}
```
# .clone()으로 복사
- String은 Clone 트레잇을 구현하므로 .clone()으로 복사 가능
- 하지만 메모리 비용이 크다.
```rust
prints_country(country.clone()); // 복사본을 넘김
prints_country(country);         // 원본은 그대로 사용 가능
```
# 복사하지 않고 참조(&)
- 복사하지 않고 참조만 넘기면 메모리 낭비 없이 사용
- 반복적으로 사용 시 .clone()보다 효율적
```rust
fn get_length(input: &String) {
    println!("It's {} words long.", input.split_whitespace().count());
}
fn main() {
    let mut my_string = String::new();
    for _ in 0..50 {
        my_string.push_str("Here are some more words ");
        get_length(&my_string); // 참조만 넘김
    }
}
```