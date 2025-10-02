- Type inference: 타입을 명시하지 않으면 컴파일러가 추론한다.
- integers: i32로 추론한다.
- 타입 명시 방법
```rust
fn main() {
    let var1: u8 = 10;
    let var2 = 10u8;
    let var3 = 10_u8;
    let var4 = 100_000_000_i32;
}
```
- `_`는 읽기 편하도록 덧붙인다. 몇 개씩 써도 된다.
```rust
fn main() {
    println!("{}", 1_2_3 == 123); // true
}
```
# Floats
- float라고 안 불리고 f32, f64라고 불린다. (32 bits, 64 bits)
- let fvar1: f64 = 9.;은 되지만, let fvar2 = 9.f64;는 안 된다.
- let fvar2 = 9.0f64;라고 써야 된다.
- 기본 추론 타입은 f64다.
```rust
fn main() {
    let my_float: f32 = 5.0;
    let my_other_float = 8.5;
    let third_float = my_float + my_other_float;
    // 이때는 my_other_float를 f32로 추론한다.
}
```
- = 1.0f64 + 2.0f32; 하면 컴파일 오류 난다. (다른 타입 간 연산x)
- 컴파일러가 2.0f32 가리키며 expected `f64`, found `f32`라고 한다.
- = 1.0f64 + 2.0f32 as f64; 하면 컴파일 오류 안 난다.