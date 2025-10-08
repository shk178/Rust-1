```rust
// trait = 초능력
// This type implements (trait name)
// From
// Into
fn main() {
    let my_name = String::from("Dave");
    let my_city: String = "Seoul".into(); // &str -> String
    // from이 있으면 into가 있다.
    // into가 있다고 from이 있지는 않다.
    println!("{}", my_city); // Seoul
    //my_city.ethwhtwr(); // error[E0599]: no method named `ethwhtwr` found for struct `String` in the current scope
}
```
- Rust에서 trait는 타입이 어떤 기능(행동)을 할 수 있는지를 정의
```rust
trait Fly {
    fn fly(&self);
}
// 이걸 구현하면 "이 타입은 날 수 있다"는 초능력을 갖는다.
```
# From과 Into: 타입 변환 트레이트
- (1) From<T> 트레이트
- T → Self로 변환하는 능력
- String::from("hello")
- (2) Into<T> 트레이트
- Self → T로 변환하는 능력
- "hello".into()
- 관계: From이 있으면 Into도 자동으로 생긴다.
```rust
// From<&str> for String 구현
impl From<&str> for String {
    fn from(s: &str) -> Self {
        String::new() + s
    }
}
// 자동으로 Into<String> for &str도 만든다.
let my_city: String = "Seoul".into(); // &str → String
// "Seoul"은 &str 타입
// String이 From<&str>을 구현했기 때문에
// &str은 자동으로 Into<String>도 갖게 됨
```
- Into<T>를 직접 구현하면 자동으로 From을 만들지 않는다.
```rust
// Into<T>를 구현
impl Into<String> for &str {
    fn into(self) -> String {
        String::from(self)
    }
}
// String::from("hello")는 작동하지 않을 수 있다.
```
# String From
- https://doc.rust-lang.org/std/string/struct.String.html
- https://doc.rust-lang.org/std/string/struct.String.html#impl-Add%3C%26str%3E-for-String
# arr -> vec
- https://doc.rust-lang.org/std/vec/struct.Vec.html#impl-From%3C%5BT;+N%5D%3E-for-Vec%3CT%3E
```rust
fn main() {
    let my_vec = Vec::from([8, 9, 10]);
    // [i32; 3] -> Vec<{i32}>
    my_vec.qhoihfoq(); // error[E0599]: no method named `qhoihfoq` found for struct `Vec<{integer}>` in the current scope
}
```