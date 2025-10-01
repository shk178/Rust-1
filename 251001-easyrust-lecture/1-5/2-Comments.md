```rust
/// documentation
struct Book;

fn main() {
    // 주석
    let x: i16 = 10;
    // : i16을 컴파일러가 안 봤으면 할 때
    let x/*: i16*/ = 10;
}
```
- /*: i16*/을 하면 컴파일러는 let x = 10;만 본다.
- 컴파일러가 warning: unused variable: `x`라고 말한다.
- 주석을 달아서 그런 게 아니라, x를 사용하지 않아서 그런 것이다.
```rust
fn main() {
    let _x = 10;
}
```
- 변수명을 _(언더스코어)로 시작하게 하면, 컴파일러에게 일단 무시하라고 말한 것이다.
- warning이 많으면 번잡해서 그렇게 한다.
```rust
fn main() {
    let 내숫자 = 10;
}
- 한글 변수명도 가능하다.