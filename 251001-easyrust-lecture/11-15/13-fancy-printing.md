# print "~"에서 엔터
- 여러 줄로 코드를 쓸 수 있다.
- println!은 엔터를 엔터로, print!는 엔터를 스페이스 바로 처리한다.
- 엔터 치고 앞에 들여쓰기가 있으면 문자열에 포함되니까 유의한다.
# escape characters
- `\n` = a new line
- `\t` = a tab
- `\이스케이프 문자` 또는 `r#"..."#`를 쓰면 그대로 출력한다.
```rust
fn main() {
    println!("He said, \"You can find the file at c:\\files\\my_documents\\file.txt.\" Then I found the file.");
    println!(r#"He said, "You can find the file at c:\files\my_documents\file.txt." Then I found the file."#);
    // 같은 결과
    // #를 출력하려면, `r##"..."##`를 쓴다.
    // ##를 출력하려면, `r###"..."###`를 쓴다.
}
```
# r# has another use
- Rust에는 match, type, async, await 같은 예약어들이 있다.
- r#을 앞에 붙이면 Rust가 "이건 예약어가 아니라 그냥 식별자야"라고 인식한다.
- r#은 정의할 때와 사용할 때 모두 붙여야 한다.
```rust
fn r#type() {
    println!("This is a function named 'type'");
}
fn main() {
    r#type(); // 가능
    let r#match = "reserved word as variable";
    println!("{}", r#match); // 가능
}
```
- Rust에서 FFI(Foreign Function Interface)를 사용할 때
- r# raw identifier는 C 언어 등 외부 언어와의 이름 충돌을 피하거나
- 그대로 유지할 때 유용하게 쓰인다.
- 구체적으로 C의 예약어를 그대로 함수나 변수 이름으로 가져오고 싶을 때 등
# to print the bytes of a &str of a char
```rust
fn main() { println!("{:?}", b"numbers"); }
//[110, 117, 109, 98, 101, 114, 115]
```
- Rust에서 b"..."는 바이트 문자열 리터럴을 의미
- 문자열을 UTF-8 인코딩된 바이트 배열로 표현한다.
```rust
fn main() {
    println!("{}", r##"write "#""##); // write "#"
    println!("{:?}", br##"write "#""##);
    // [119, 114, 105, 116, 101, 32, 34, 35, 34]
    println!("{}", r##""#""##); // "#"
    println!("{:?}", br##""#""##);
    // [34, 35, 34]
}
```
# Unicode escape
- 유니코드 문자를 특수한 형식으로 표현하는 방법
- 코드 안에서 직접 입력하기 어려운 문자나 특수 문자를 사용할 때
- 대부분의 언어에서 Unicode escape는 다음과 같은 형식
```rust
// \uXXXX → 여기서 XXXX는 4자리 16진수
// \UXXXXXXXX → 8자리 16진수 (일부 언어에서 사용)
let heart = '\u{2764}'; // '❤' 문자
// 2764는 하트의 유니코드 코드 포인트
```
- `\u{hexadecimal number}`로 출력한다.
```rust
fn main() {
    println!("{:X}", '행' as u32); // D589
    // Cast char as u32 to get the hexadecimal value
    println!("{:X}", 'H' as u32); // 48
    println!("{:X}", '居' as u32); // 5C45
    println!("{:X}", 'い' as u32); // 3044
    println!("\u{D589}, \u{48}, \u{5C45}, \u{3044}"); // 행, H, 居, い
    // Try printing them with unicode escape \u
}
```
# printing
- {} for Display
- {:?} for Debug
- {:#?} for pretty printing
- {:p} to print the pointer address
```rust
fn main() {
    let number = 9;
    let number_ref = &number;
    println!("{:p}", number_ref); // 0x7ffd5789916c
}
```
- to print binary, hexadecimal and octal
```rust
fn main() {
    let number = 50;
    println!("{:b}, {:x}, {:o}", number, number, number);
    // 110010, 32, 62
}
```
- numbers to change the order
```rust
fn main() {
    let father_name = "Vlad";
    let son_name = "Adrian Fahrenheit";
    let family_name = "Țepeș";
    println!("This is {1} {2}, son of {0} {2}.", father_name, son_name, family_name);
    // This is Adrian Fahrenheit Țepeș, son of Vlad Țepeș.
}
```
- 변수를 1번 이상, 또는 여러 개 변수를 출력하려면 add names to the {}
```rust
fn main() {
    println!(
        "{city1} is in {country} and {city2} is also in {country},
but {city3} is not in {country}.",
        city1 = "Seoul",
        city2 = "Busan",
        city3 = "Tokyo",
        country = "Korea"
    );
    // Seoul is in Korea and Busan is also in Korea,
    // but Tokyo is not in Korea.
}
```
# {variable:padding alignment minimum.maximum}
- format! 매크로나 println!에서 쓰이는 포맷 문자열의 기본 구조
- 출력되는 문자열의 모양, 길이, 정렬 방식, 채우기 문자 등을 세밀하게 조절
- (1) `variable`
- 출력할 변수 이름 또는 값
- {country}, {name}, {value} 등
- (2) `:`
- 포맷 옵션을 시작할 때 씀
- (3) `padding`
- 빈 공간을 채울 문자
- 0, ' ', * 등
- (4) `alignment`
- 정렬 방식
- < 왼쪽, ^ 가운데, > 오른쪽
- (5) `minimum`
- 최소 너비
- `5` → 최소 5칸 확보
- (6) `.maximum`
- 최대 너비 또는 소수점 자리수
- 문자열은 최대 길이, 숫자는 소수점 자리
- 예시
```rust
fn main() {
    let country = "Korea";
    print!("\\");
    print!("{country:*>10.3}");
    print!("\\");
    // \*******Kor\
// country → 변수 이름
// * → 빈 공간을 *로 채움
// > → 오른쪽 정렬
// 10 → 최소 너비 10칸
// .3 → 최대 3글자만 출력 (Kor)
// 나머지 7칸은 *로 채워짐
}
```
```rust
fn main() {
    let letter = "a";
    println!("{:ㅎ^11}", letter);
    // ㅎㅎㅎㅎㅎaㅎㅎㅎㅎㅎ
    println!("{:ㅎ^12}", letter);
    // ㅎㅎㅎㅎㅎaㅎㅎㅎㅎㅎㅎ
    println!("{:ㅎ^13}", letter);
    // ㅎㅎㅎㅎㅎㅎaㅎㅎㅎㅎㅎㅎ
}
```
```rust
fn main() {
    let title = "TODAY'S NEWS";
    println!("{:-^30}", title);
    // no variable name, pad with -
    // put in centre, 30 characters long
    let bar = "|";
    println!("{: <15}{: >15}", bar, bar);
    // no variable name, pad with space
    // 15 characters each, one to the left, one to the right
    let a = "SEOUL";
    let b = "TOKYO";
    println!("{city1:-<15}{city2:->15}", city1 = a, city2 = b);
    // variable names city1 and city2, pad with -
    // one to the left, one to the right
}
/*
---------TODAY'S NEWS---------
|                            |
SEOUL--------------------TOKYO
*/
```
# https://doc.rust-lang.org/stable/std/fmt/