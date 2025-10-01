- Rust의 문자와 문자열
```rust
fn main() {
    // 문자열(String): 큰따옴표 ""
    let greeting = "Hello, world!";// 문자(char): 작은따옴표 ''
    let first_letter = 'A';
    let space = ' ';
    let korean_char = '가';
    let cat_face = '🐱';
}
```
- char 타입 크기: 4바이트 (32비트), 모든 언어의 문자를 표현하기 위해
- Unicode: 모든 문자에 고유한 숫자를 할당한 국제 표준
```rust
fn main() {
    let a = 'A';        // Unicode: 65
    let friend = '友';   // Unicode: 21451
    let emoji = '😀';    // Unicode: 128512
    // 영문 대문자 A-Z: 65-90
    // 영문 소문자 a-z: 97-122
    // 숫자 0-9: 48-57
    // 공백(space): 32
}
```
- u8 → char 변환 (항상 안전)
```rust
fn main() {
    let my_number: u8 = 65;
    let my_char = my_number as char;
    println!("{}", my_char); // 출력: A
    // u8 범위 (0-255)의 문자들
    let space = 32u8 as char;     // ' '
    let zero = 48u8 as char;      // '0'
    let capital_a = 65u8 as char; // 'A'
    let small_a = 97u8 as char;   // 'a'
}
```
- u8: 0 ~ 255 (256개), 가장 자주 쓰이는 문자들이 0-255 범위에 배치됨
- 모든 u8 값은 유효한 Unicode 문자에 대응됨
```
(1) ASCII (American Standard Code for Information Interchange)
- 0 ~ 127 범위 (7비트, 총 128개 문자)
- 영어 알파벳, 숫자, 기본 기호, 제어 문자 포함
(2) Unicode
- 0 ~ 127 범위는 ASCII와 완전히 동일 (하위 호환성)
- 128 ~ 255 범위는 Latin-1 (ISO 8859-1)과 동일
```
- casting = simple type change using 'as'
- u8 as char: u8 값을 char로 안전하게 변환
```rust
fn main() {
    // 0-127: ASCII 영역 (Unicode와 동일)
    println!("{}", 65 as char);   // A
    println!("{}", 97 as char);   // a
    println!("{}", 48 as char);   // 0
    println!("{}", 32 as char);   // 공백
    // 128-255: Latin-1 확장 영역 (특수문자, 악센트 등)
    println!("{}", 169 as char);  // © (저작권 기호)
    println!("{}", 241 as char);  // ñ (스페인어)
    // 256 이상: Unicode 고유 영역
    println!("{}", '가' as u32);  // 44032 (한글)
    println!("{}", '😀' as u32);  // 128512 (이모지)
}
```
- 안전하게 타입 바꿀 수 있으면 as, 아니면 From을 쓴다.
- function: 인자 개수 고정, 타입 고정 등..
```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}
```
- macro = super function: Macro는 함수보다 강력하다.
```rust
fn main() {
    // 가변 인자 개수
    println!("One: {}", 1);
    println!("Two: {} {}", 1, 2);
    println!("Three: {} {} {}", 1, 2, 3);
    // 다양한 타입
    println!("{}", 42);      // 숫자
    println!("{}", "text");  // 문자열
    println!("{}", true);    // 불린
    // 컴파일 타임에 코드 생성
    vec![1, 2, 3];           // Vec 생성
    vec![0; 100];            // 0을 100개
}
```
```rust
fn main() {
    // 느낌표(!)가 있으면 매크로
    println!();     // 매크로
    vec![];         // 매크로
    format!();      // 매크로
    panic!();       // 매크로
    // 느낌표 없으면 함수
    String::new();  // 함수
    Vec::new();     // 함수
    i32::max();     // 함수
}
```