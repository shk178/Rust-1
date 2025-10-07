# 소유권, 참조를 함수에 적용 1
- (1) 소유권
- rust에서 값은 하나의 소유자만 가질 수 있다.
- 어떤 함수가 String을 인자로 받으면 그 함수가 그 값을 소유하게 된다.
```rust
fn print_country(country_name: String) {
    println!("{}", country_name);
}
fn main() {
    let country = String::from("Austria");
    print_country(country); // OK
    print_country(country); // error
}
```
- country는 print_country로 소유권을 넘긴다.
```
함수로 소유권을 넘긴다는 의미
- 그 함수의 매개변수가 그 값을 소유하게 된다는 뜻이다.
- country_name이 소유권을 가진다.
```
- 이 순간 country는 유효하지 않게 된다.
```
유효하지 않다는 의미
- stack에 있는 country의 메타데이터는 scope가 끝날 때까지 남아 있다.
- Rust는 그걸 다시 사용하는 걸 컴파일러 수준에서 금지한다.
- stack: country라는 변수 자체 (포인터, 길이, 용량 등 String의 메타데이터)
- heap: String이 가리키는 실제 문자열 데이터 ("Austria")
```
- 함수가 끝나면 drop이 발생한다.
```
- country_name이 scope를 벗어나므로 그렇다.
- drop: heap에 있는 문자열 데이터가 해제된다.
해제된다는 말은, heap에 저장된 메모리를 Rust가 자동으로 반환(free)한다는 뜻
- Rust는 Drop 트레이트를 통해, 값이 더 이상 필요 없을 때 자동으로 정리
- String이 가리키던 heap 메모리(문자열 데이터)가 운영체제에 반환됨
- 메모리 누수(memory leak) 없이 안전하게 정리됨
```
- (2) 소유권 반환 방식
- 함수가 값을 반환하면 다시 사용할 수 있다.
```rust
fn print_country(country_name: String) -> String {
    println!("{}", country_name);
    country_name
}
fn main() {
    let country = String::from("Austria");
    let country = print_country(country); // OK
    print!("{}", country); // OK
}
```
- 소유권이 함수로 넘어가서 기존 변수는 유효하지 않게 된다.
- 두 번째 let country = ...는 새로운 변수 선언이다.
- 새로운 변수로 섀도잉이 일어난다. (같은 이름이면 섀도잉)
- 함수가 새로운 변수에 소유권을 넘긴다.
```
소유권을 넘긴다는 의미
- heap에 있는 데이터는 그대로 유지된다. (drop 안 됨)
- stack에 있는 메타데이터만 새로운 변수로 옮겨진다.
```
# 소유권, 참조를 함수에 적용 2
- (3) 불변 참조(&T)
- 값을 빌려주는(borrow) 방식이다.
- 소유권을 넘기지 않고, 함수는 값을 읽기만 할 수 있다.
```rust
fn print_country(country_name: &String) {
    println!("{}", country_name);
}
fn main() {
    let country = String::from("Austria");
    print_country(&country); // OK
    print_country(&country); // OK
}
```
- (4) 가변 참조(&mut T)
- 값을 빌려주되, 수정할 수 있는 권한도 준다.
```rust
fn add_hungary(country_name: &mut String) {
    country_name.push_str("-Hungary");
    println!("Now it says: {}", country_name);
}
fn main() {
    let mut country = String::from("Austria");
    add_hungary(&mut country); // OK
}
```
- (5) 함수 내부에서 mut로 선언
- 함수가 값을 소유하면서 내부에서 mut로 선언
- 외부에서는 mut가 아니어도 값을 수정할 수 있다.
- 소유권을 넘겨받았기에 (소유권 독점 상태) 안전하게 수정할 수 있다.
- 안전하다 = 데이터 경쟁이나 use-after-free 같은 문제가 없다는 의미
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
# Move Semantics
- 값의 소유권을 다른 변수나 함수로 이동(move)시키는 방식을 말한다.
- Rust는 기본적으로 값을 복사하지 않고, 소유권을 이동시킨다.
- 메모리 안전성 확보: 중복 해제나 use-after-free가 발생하지 않음
- 성능 최적화: 불필요한 메모리 복사 비용이 없음
- 명확한 생명주기 관리: 컴파일러가 추적 가능
- 어떤 타입은 move, 어떤 타입은 copy
```
- String, Vec, Box는 move - heap 사용, 소유권 이동
- i32, bool, char는 copy - stack에 저장, 값 복사 가능
```
# rust스럽지 않은 코드
```rust
fn print_country(country_name: String) -> String {
    println!("{}", country_name);
    country_name
}
fn main() {
    let mut country = String::from("Austria");
    country = print_country(country);
    country = print_country(country);
} // 실행된다.
```
- (1) 불필요한 소유권 이동
```rust
fn print_country(country_name: String) -> String
```
- 함수는 String을 소유권을 넘겨받아 사용하고, 다시 반환
- println!은 &str 또는 &String을 빌려서 출력할 수 있다.
- 이 경우 함수에 소유권을 넘길 필요가 없다.
- (2) 빌림(Borrowing)을 활용하지 않음
- 매번 String의 소유권을 넘겼다가 다시 받아오는 방식이다.
- 소유권을 넘기지 않고도 읽을 수 있도록 참조(&)를 사용하면 된다.
- (3) 불필요한 복잡성
```rust
country = print_country(country);
```
- 함수가 값을 변경하거나 새로운 값을 반환할 것처럼 보인다.
- 실제로는 단순히 출력만 하고 원래 값을 그대로 반환한다.
- 가독성과 의도 전달 측면에서 좋지 않다.
- (4) rust다운 코드
```rust
fn print_country(country_name: &str) {
    println!("{}", country_name);
}
fn main() {
    let country = String::from("Austria");
    print_country(&country);
    print_country(&country);
}
```
- print_country는 값을 빌려서 읽기만 하므로 소유권을 넘기지 않는다.
- country는 그대로 유지되며, 여러 번 사용할 수 있다.