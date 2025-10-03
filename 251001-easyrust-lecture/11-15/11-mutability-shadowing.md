# Mutability (changing)
- `let`으로 선언하면, 값을 변경 불가
- error[E0384]: cannot assign twice to immutable variable
- cannot assign twice to immutable variable
- mutable한 변수 선언은 `let mut`로 한다.
- 값은 변경 가능하나, 타입은 변경 불가하다.
- error[E0308]: mismatched types
- expected integer, found `&str`
# Shadowing
```rust
fn main() {
    let my_number = 8; //i32
    println!("{}", my_number); //8
    let my_number = 9.2; //f64 with the same name.
    //But it's not the first my_number - it is completely different!
    println!("{}", my_number) //9.2
}
```
- 같은 블럭에서 섀도잉 해서, let my_number = 9.2; 이후에는
- let my_number = 8;에 접근할 수 없다.
- When you shadow a variable, you don't destroy it. You block it.
- 다른 블럭에서 섀도잉 하면, 접근할 수 있다.
- 섀도잉: 변수 값 많이 바꿀 때 쓴다. (변수 이름 개수 줄임)
```rust
fn times_two(number: i32) -> i32 {
    number * 2
}
fn main() {
    let final_number = {
        let y = 10;
        let x = 9; //x starts at 9, i32 (정수 리터럴의 기본 타입)
        let x = times_two(x); //shadow with new x: 18, i32 (함수 반환 타입이 i32)
        let x = x + y; //shadow with new x: 28, i32 (i32 + i32 = i32)
        x //return x: final_number is now the value of x
    }; //i32 (블록이 i32를 반환)
    println!("The number is now: {}", final_number) //28
}
```
# Shadowing의 메모리 동작
```rust
fn main() {
    let my_number = 8;     // 주소 A에 i32 (스택에 4바이트 할당)
    println!("{}", my_number); // 첫 번째 my_number 사용
    let my_number = 9.2;   // 주소 B에 f64 (스택에 8바이트 할당)
    // 이 시점에서 주소 A의 값은 여전히 존재하지만 blocked
    // "my_number"라는 이름은 주소 B를 가리킴
}
// 함수 종료 시 두 변수 모두 메모리에서 해제
```
```rust
fn main() {
    let x = 5;           // 외부 x: 주소 A
    {
        let x = 10;      // 내부 x: 주소 B (새로운 메모리)
        println!("{}", x); // 10 (주소 B)
    }                    // 주소 B 메모리 해제
    println!("{}", x);   // 5 (주소 A, 다시 접근 가능)
}
```
# let mut는 같은 메모리 위치
```rust
let mut x = 5;    // 주소 A
x = 10;           // 같은 주소 A의 값만 변경
```