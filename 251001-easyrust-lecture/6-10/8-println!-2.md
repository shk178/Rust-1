# String interpolation
- 문자열 안에 변수나 표현식을 직접 삽입하는 것을 말한다.
```python
name = "Alice"
age = 30
print(f"My name is {name} and I'm {age} years old")
```
```javascript
const name = "Alice";
const age = 30;
console.log(`My name is ${name} and I'm ${age} years old`);
```
- Rust는 직접적인 interpolation 문법이 없다.
- 대신 format!, println! 매크로를 (Format macros) 사용한다.
```rust
fn main() {
    let name = "Alice";
    let age = 30;
    println!("{} is {} years old", name, age); // Rust 방식 (Positional)
    println!("{name} is {age} years old", name=name, age=age); // Named arguments
    println!("{name} is {age} years old"); // 변수명과 같으면 생략 가능
    let s = format!("Hello, {}!", name); // format! - String 반환
    eprintln!("Error in {}!", name); // eprintln! - 에러 출력
    // Standard Error:
    // warning: unused variable: `s`
    // Error in Alice!
    // Standard Output:
    // Alice is 30 years old
    // Alice is 30 years old
    // Alice is 30 years old
}
```