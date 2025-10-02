- 매크로는 복잡한 코드를 감추고 간단한 문법을 제공
- println! 같은 간단한 호출이 실제로는 여러 단계의 함수 호출로 확장됨
```rust
fn main() {
    println!("{}", 1u8); // 1
    print!("{}", 1.1f32); // 1.1
}
// fn = function
// main = the function that starts the program
// () = main 함수에 인자 넘기지 않음
// 함수/제어문 뒤의 {} = 코드 블록
// "...{}..." = format string
// 포맷 문자열 안의 {} = format specifier/placeholder
```
- Macro expansion
```rust
#![feature(prelude_import)]
#[macro_use]
extern crate std;
#[prelude_import]
use std::prelude::rust_2024::*;
fn main() { //1
    //1.1
    { ::std::io::_print(format_args!("{0}\n", 1u8)); };
    { ::std::io::_print(format_args!("{0}", 1.1f32)); };
}
```
- println!, print!처럼 이름 뒤에 !가 붙으면 매크로
- 함수가 아니라 컴파일 타임에 코드를 생성하는 도구
- 컴파일 타임에 처리해서 런타임 오버헤드 없음
- println! 다양한 포맷 옵션
```rust
println!("{}", 10);      // 기본: 10
println!("{:?}", 10);    // 디버그: 10
println!("{:b}", 10);    // 이진수: 1010
println!("{:x}", 10);    // 16진수: a
println!("{:.2}", 1.5);  // 소수점 2자리: 1.50
println!("{0} {1} {0}", "A", "B"); // 위치 지정: A B A
```
- Arguments
```rust
// Positional arguments
println!("{} {}", "first", "second");
println!("{0} {1}", "first", "second");
// Named arguments
println!("{name} is {age} years old", name="Alice", age=30);
// 혼합 사용
println!("{0} {1} {name}", "A", "B", name="C");
```
# function
```rust
fn number() -> i32 {
    8
}
fn main() {
    println!("Hello, world number {}!", number());
    // Hello, world number 8!
}
// -> = skinny arrow
// -> i32 = 반환 타입
// 8에 세미콜론 없음 = expression, 8을 반환
```
- 8에 세미콜론이 있으면 () 반환하는데, 컴파일 에러 난다.
- i32 가리키며 expected `i32`, found `()`라고 한다.
- Expression (표현식) = 값을 반환
```rust
// 모두 expression (값을 만듦)
5 + 3          // 8
x * 2          // 어떤 값
true && false  // false
if x > 0 { 1 } else { 0 }  // 1 또는 0
// 함수도 expression이 될 수 있음
fn add(a: i32, b: i32) -> i32 {
    a + b  // <- 이것도 expression (세미콜론 없음)
}
```
- Statement (구문) = 동작만 수행, 값 반환 안 함
```rust
// 모두 statement (값 없음)
let x = 5;     // 변수 선언
x = 10;        // 값 할당
println!("hi"); // 출력
// 세미콜론이 있으면 statement가 됨
8;             // 8을 계산하고 버림 → () 반환
```
- 명시적 반환: 함수 중간에서 빠져나올 때, 조기 반환 등
```rust
fn number() -> i32 {
    return 8;
}
// return 사용 시 세미콜론 권장한다.
// 조기 반환은 return, 마지막 값은 return 없이 쓰자.
```
- to give variables to a function
```rust
fn multiply(number_one: i32, number_two: i32) {
    let result = number_one * number_two;
    println!("{} times {} is {}", number_one, number_two, result);
}
fn main() {
    multiply(8, 9); // 8 times 9 is 72
}
```
- Variables start and end inside a code block {}.
```rust
fn main() {
    {
        let my_number = 8;
    }
    println!("Hello, number {}", my_number);
    // error: cannot find value `my_number` in this scope
}
```
- a code block to return a value
```rust
fn main() {
    let my_number = {
        let second_number = 8;
        second_number + 9
    }; // 코드 블럭이 함수처럼 작동한다.
    println!("My number is: {}", my_number); // My number is: 17
}
```
- 리턴 안 해도 컴파일 오류 안 난다.
```rust
fn main() {
    let my_number = {
        let second_number = 8;
        second_number + 9;
        // warning: unused arithmetic operation that must be used
        // help: use `let _ = ...` to ignore the resulting value
        // let _ = second_number + 9;
    };
    println!("My number is: {:?}", my_number); // My number is: ()
}
```
- 디버그 포맷(Debug format)
```rust
// {} - Display (읽기 좋게)
println!("{}", 42);        // 42
println!("{}", "Hello");   // Hello
// {:?} - Debug (자세하게)
println!("{:?}", 42);      // 42
println!("{:?}", "Hello"); // "Hello" - 따옴표 포함
println!("{:?}", ());      // () - unit type
```
- ()는 Display를 구현하지 않아서 {}로 출력 안 됨
```rust
fn main() {
    println!("My number is: {}", ());
    // error: `()` doesn't implement `std::fmt::Display`
}
```