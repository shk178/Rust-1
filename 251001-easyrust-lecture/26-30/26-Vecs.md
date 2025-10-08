# 벡터
- 벡터: 크기를 바꿀 수 있는 유연한 배열
- 배열보다 느리지만 더 많은 기능
- 벡터 만드는 법
- (1) Vec::new()로 빈 벡터 만들기
```rust
fn main() {
    let mut my_vec = Vec::new(); // 빈 벡터 생성
    my_vec.push(10); // 원소 추가
    my_vec.push(20);
// push()로 원소를 추가하면 Rust가 자동으로 타입을 추론
// 예: Vec<i32> 또는 Vec<String> 등
}
```
- (2) 타입을 명시적 선언
```rust
let mut my_vec: Vec<String> = Vec::new();
// 타입을 미리 알려주면 .push() 하기 전에도 오류가 안 남
```
- (3) vec![] 매크로 사용
```rust
let my_vec = vec![8, 10, 10];
// Vec을 생성
// Vec<i32> 타입
```
- 벡터의 타입: Vec<type> 형식
```
- Vec<String> → 문자열들의 벡터
- Vec<(i32, i32)> → (i32, i32) 튜플을 담는 벡터
- Vec<Vec<String>> → 문자열 벡터를 담는 벡터 (2차원 느낌)
// 책 한 권을 Vec<String> (각 줄이 문자열)로 저장
// 그걸 여러 권 모으면 Vec<Vec<String>>이 된다.
```
- 벡터 슬라이싱: 배열처럼 범위로 일부를 가져올 수 있다. (&vec[2..5] 등)
```rust
fn main() {
    let vec_of_ten = vec![1,2,3,4,5,6,7,8,9,10];
    let three_to_five = &vec_of_ten[2..5]; // 인덱스 2~4
    let start_at_two = &vec_of_ten[1..];   // 인덱스 1부터 끝까지
    let end_at_five = &vec_of_ten[..5];    // 처음부터 4까지
    let everything = &vec_of_ten[..];      // 전체
}
```
- 벡터의 용량(Capacity)
- 벡터에는 실제 원소 개수(len) 외에도 capacity(미리 확보된 공간)이 있다.
- 용량이 꽉 차면 2배로 늘려서 새 공간을 만들고 복사한다. (reallocation)
- 이는 성능을 떨어뜨린다.
```rust
fn main() {
    let mut num_vec = Vec::new();
    println!("{}", num_vec.capacity()); // 0
    num_vec.push('a');
    println!("{}", num_vec.capacity()); // 4 (처음엔 4로 시작)
    num_vec.push('a');
    num_vec.push('a');
    num_vec.push('a');
    println!("{}", num_vec.capacity()); // 여전히 4
    num_vec.push('a');
    println!("{}", num_vec.capacity()); // 8로 증가 (재할당 발생)
}
```
- 성능 최적화: Vec::with_capacity()
```rust
// 미리 추가할 용량 포함해 할당한다.
fn main() {
    let mut num_vec = Vec::with_capacity(8);
    num_vec.push('a');
    num_vec.push('a');
    num_vec.push('a');
    println!("{}", num_vec.capacity()); // 8 그대로 유지
}
```
- 배열을 벡터로 바꾸기 (.into())
```rust
fn main() {
    let my_vec: Vec<u8> = [1, 2, 3].into();  // 타입 지정
    let my_vec2: Vec<_> = [9, 0, 10].into(); // 타입 추론(Vec<i32>)
// [1, 2, 3] → Vec로 변환할 때 .into()를 사용
// Vec<_>는 타입을 알아서 결정해달라는 뜻
}
```
# 배열[T; N]과 벡터Vec<T>
- 배열은 크기 고정, 벡터는 가변 (원소 추가/삭제)
- 속도 배열은 빠름, 벡터는 조금 느림 (동적 크기 조정)
- 사용법 배열 [1, 2, 3], 벡터 vec![1, 2, 3]
- 메모리 배열은 스택에 저장, 벡터는 힙에 저장
- 벡터 요소를 추가/제거해도 타입이 바뀌지 않는다.
- 벡터는 크기가 가변적이어서, 타입에 크기 정보를 포함할 수 없다.
- 벡터는 런타임에 크기를 관리한다.
# 벡터의 capacity
- len: 실제 벡터에 들어있는 원소 개수
- capacity: 벡터가 현재 재할당 없이 수용할 수 있는 최대 원소 개수
- Vec::new() → len 0, capacity 0 → push하면 자동으로 증가
- Vec::with_capacity(n) → len 0, capacity n → 미리 공간 확보
- (1) Vec::new()로 생성한 경우
```rust
let mut v: Vec<i32> = Vec::new();
println!("len: {}, capacity: {}", v.len(), v.capacity());
// 출력: len: 0, capacity: 0
// 처음에는 용량도 0이라서 push를 하면 자동으로 최소 4 정도로 늘어남
v.push(1);
println!("len: {}, capacity: {}", v.len(), v.capacity());
// 출력: len: 1, capacity: 4
```
- (2) Vec::with_capacity(n) 사용 시
```rust
let mut v: Vec<i32> = Vec::with_capacity(8);
println!("len: {}, capacity: {}", v.len(), v.capacity());
// 출력: len: 0, capacity: 8
// len은 0이지만, capacity는 미리 8 확보됨
// push하면 len 증가하지만, capacity는 재할당이 필요할 때까지 그대로
```
# 벡터 타입 에러
```rust
fn main() {
    //let my_vec = Vec::new(); // error[E0282]: type annotations needed for `Vec<_>`
    //let mut my_vec = Vec::new(); // error[E0282]: type annotations needed for `Vec<_>`
}
```
- Rust 컴파일러가 벡터의 타입을 알 수 없어서 나는 에러
- 방법 (1) 타입을 명시적으로 선언
```rust
let my_vec: Vec<String> = Vec::new(); // String을 담는 벡터
let mut my_vec: Vec<i32> = Vec::new(); // i32를 담는 벡터
```
- 방법 (2) .push()로 요소를 추가해서 타입 추론
```rust
let mut my_vec = Vec::new();
my_vec.push(5); // Vec<i32>라고 알 수 있음
```
- 방법 (3) vec! 매크로 사용
```rust
let my_vec = vec![1, 2, 3]; // Vec<i32>로 자동 추론
```
# 벡터 mut
- mut 없을 때 (불변): 변수를 만든 후에 내용을 바꿀 수 없다.
```rust
let my_vec = Vec::new();
my_vec.push(5); // 변경할 수 없음
```
- mut 있을 때 (가변): 변수를 만든 후에 내용을 바꿀 수 있다.
```rust
let mut my_vec = Vec::new();
my_vec.push(5); // 가능
my_vec.push(10); // 가능
```
- 스택(Stack): 빠르고 고정된 크기의 데이터 저장소. 변수 자체(예: 벡터의 포인터, 길이, 용량 정보)가 여기에 저장
- 힙(Heap): 동적으로 할당된 큰 데이터 저장소. 벡터 안의 실제 값들(예: [1, 2, 3])이 여기에 저장
```rust
fn main() {
    let mut numbers = vec![1, 2, 3];
    numbers.push(4);
}
```
- (1) 스택
- 스택에는 numbers라는 변수 자체가 저장된다.
- 이 변수는 벡터의 메타데이터를 담고 있다.
- 포인터: 힙에 있는 실제 데이터 [1, 2, 3]을 가리킴 (0xABC)
- 길이: 현재 요소 개수 (처음엔 3, .push(4) 후엔 4)
- 용량: 힙에 미리 확보된 공간 (예: 4개까지 저장 가능)
- (2) 힙
- 힙에는 실제 벡터의 데이터가 저장된다.
- numbers가 가리키는 메모리 주소 (0xABC)에 [1, 2, 3, 4]가 들어 있다.
- .push(4)를 하려면 왜 mut가 필요할까
- 힙에 [4]를 추가해야 하고
- 스택의 len 값도 3 → 4로 바뀌어야 한다.
- 즉, 스택의 변수 자체를 수정해야 하므로 mut가 필요하다.
# String 타입도 Vec<u8> 기반
```rust
fn main() {
    let mut greeting = String::from("Hello");
    greeting.push_str(", world!");
}
```
- (1) 스택(Stack)
- greeting이라는 변수 자체는 스택에 저장된다.
- 포인터: 힙에 있는 "Hello" 문자열을 가리킴 (0xDEF)
- 길이: 현재 문자열 길이 (처음엔 5, 나중엔 13)
- 용량: 힙에 미리 확보된 공간 (예: 16바이트)
- (2) 힙(Heap)
- 힙에는 실제 문자열 데이터가 저장된다.
- greeting이 가리키는 주소 (0xDEF)에 "Hello, world!"가 들어 있다.
- .push_str(", world!")를 하면
- 힙에 문자열을 추가해야 하고
- 스택의 len 값도 5 → 13으로 바뀌어야 한다.
- 스택의 변수 자체를 수정해야 하므로 mut가 필요하다.
# 벡터 가변 참조
```rut
let mut v = vec![1, 2, 3];
let r = &v;       // 불변 참조
let r_mut = &mut v; // 가변 참조
```
- &v: 불변 참조 — 읽을 수만 있고, 수정은 못 한다.
- &mut v: 가변 참조 — 읽고 쓸 수 있다. 단, 동시에 하나만 존재할 수 있다.
```rust
fn add_number(v: &mut Vec<i32>) {
    v.push(4);
}
fn main() {
    let mut numbers = vec![1, 2, 3];
    add_number(&mut numbers);
    println!("{:?}", numbers); // [1, 2, 3, 4]
}
```
- add_number 함수는 &mut Vec<i32>를 받음
- 벡터를 가변 참조로 받았기 때문에 .push(4)가 가능
- numbers 자체는 mut이니까 가변 참조를 만들 수 있다.
- 함수 안에서는 스택의 메타데이터와 힙의 데이터 모두 접근 가능하다.
- 가변 참조는 왜 안전할까
- 빌림 검사기(Borrow Checker)가 다음을 보장한다.
- 가변 참조는 동시에 하나만 존재해야 함
- 불변 참조와 가변 참조는 동시에 존재할 수 없다.
```rust
fn main() {
    let mut x = 10;
    let r1 = &x;       // 불변 참조
    let r2 = &mut x;   // 오류: 불변 참조가 아직 살아있음
    println!("{}", r1);
    println!("{}", r2);
}
```
```rust
fn main() {
    let mut x = 10;
    {
        let r1 = &x;
        println!("{}", r1); // r1은 여기서 끝남
    }
    let r2 = &mut x; // 가변 참조 가능
    *r2 += 1;
    println!("{}", r2); // 11
}
```
- 이 덕분에 데이터 경쟁 없이 안전하게 메모리를 다룰 수 있다.
# 소유권이 어떻게 이동
```rust
fn main() {
    let name1 = String::from("Windy");
    let name2 = String::from("Gomesy");
    let my_vec = vec![name1, name2];
    println!("{:?}", my_vec); // ["Windy", "Gomesy"]
}
```
- String::from("Windy")와 String::from("Gomesy")는
- 각각 힙에 문자열 데이터를 저장하고
- 스택에는 포인터 + 길이 + 용량 정보를 가진 String 구조체가 있다.
- vec![name1, name2]를 호출하면
- name1과 name2의 소유권이 벡터로 이동한다.
- 즉, my_vec은 name1, name2를 소유하는 벡터가 된다.
- name1, name2는 더 이상 유효하지 않게 된다.
- String이나 Vec 같은 소유권 있는 타입은 복사되지 않고 이동(move)된다.
- 이동이란
- 스택의 메타데이터(포인터, 길이, 용량)를 my_vec에 넘기고
- 원래 변수(name1, name2)는 무효화된다를 의미한다.
- 원래 변수를 또 사용하면, Rust는 컴파일 시점에 오류를 낸다.
- 소유권 유지 (1) .clone()으로 복제
```rust
let my_vec = vec![name1.clone(), name2.clone()];
println!("{}", name1); // 가능
// .clone()은 힙의 데이터를 복제해서 새로운 String을 만들어줌
```
- 소유권 유지 (2) 참조를 넘기기
```rust
let my_vec = vec![&name1, &name2];
// Vec<&String>이 됨
// 소유권은 넘기지 않음
// 참조의 수명(lifetime)을 잘 관리해야 한다.
```