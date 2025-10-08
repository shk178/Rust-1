# uninitialized variables
```rust
let my_variable;
```
- 초기화되지 않은 변수: 사용 불가 (컴파일 에러)
- 하지만 초기화 늦어질 때 있다.
```rust
fn loop_then_return(mut counter: i32) -> i32 {
    loop {
        counter += 1;
        if counter % 50 == 0 {
            break;
        }
    }
    counter
}
fn main() {
    let my_number; // 값 없음
    {
        let number = {
            // ...
            57
        };
        // my_number 초기화
        my_number = loop_then_return(number);
    }
    println!("{}", my_number); // 100
}
```
- my_number는 main() 함수 스코프에 선언되었다.
- number는 { ... } 블록 안에서만 유효하다.
- my_number는 main() 전체에서 유효해야 한다.
- 그래서 바깥에서 미리 선언해둔다.
```rust
// 단순화
fn main() {
    let my_number;
    {
        my_number = 100;
    }
    println!("{}", my_number);
}
```
- 즉, Rust에서는 변수를 한 번만 값으로 초기화할 수 있다.
- 나중에 초기화는 되고, 두 번 초기화는 안 된다.
- mut를 쓰면 한 번 초기화 + 여러 번 값 재할당 가능하다.
# rustic code
- mut 매개변수를 사용하는 것 rustic하지 않다.
- rust는 함수 인자에 mut를 거의 쓰지 않는다.
- 함수 매개변수는 값이 복사되거나 이동되어 들어온다.
- mut를 붙이면 이 함수 안에서만 변경 가능한 로컬 복사본이 된다.
- 입력은 불변 값으로 두고, 새로운 가변 값을 내부 변수로 선언해서 리턴하게 한다.
- 무한 루프 + break도 괜찮지만, 명시적으로 종료 조건이 있는 루프를 선호한다.
- while이나 Iterator 기반의 루프가 rustic하다.
- 함수 이름을 동사형보다 의미형으로 짓는 걸 선호한다.
- do_this(과정)보다 processed_value(의도)가 낫다.
- 함수가 무엇을 반환하는지 알 수 있도록 짓는다.
```rust
fn next_multiple_of_50(n: i32) -> i32 {
    let mut result = n;
    while result % 50 != 0 {
        result += 1;
    }
    result
}
fn main() {
    let my_number = {
        let number = 57;
        next_multiple_of_50(number)
    };
    println!("{}", my_number);
}
```
- rust에서 모든 블록이 표현식이다.
- { ... }가 값을 반환하는 식(expression)임을 활용해 작성한다.
- 불필요한 mut 변수는 줄이고, 무엇을 계산하는가에 집중한다.
- rustic code = 기계 입장에서도 빠르고 안전하게 바꿀 수 있는 코드다.
```
// 컴파일러(LLVM) 입장
- mut counter (가변 매개변수)를 쓰면
- 혹시 다른 코드가 이 값의 주소를 쓸지도 몰라
- 메모리에 남겨두고 접근한다. (alloca, load, store)
- let mut result = n; (로컬 가변 변수)는
- 이 값이 함수 안에서만 쓰이고, 외부에 노출 안 되니까
- 레지스터에만 저장한다.
- 불변 변수 (n)은 안 바뀌니까 상수처럼 최적화한다.
```