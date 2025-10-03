- The pointer you usually see in Rust is called a reference.
- A reference means you borrow the value, but you don't own it.
- 포인터는 책의 목차와 같다.
- The table of contents doesn't own the information.
- It's the chapters that own the information.
- In Rust, references have a & in front of them.
# 다중 참조(multiple references)
```rust
fn main() {
    let my_number = 15; // i32
    println!("{}", my_number); // 15
    let single_reference = &my_number; // &i32
    println!("{}", single_reference); // 15
    let double_reference = &single_reference; // &&i32
    println!("{}", double_reference); // 15
    let five_references = &&&&&my_number; // &&&&&i32
    println!("{}", five_references); // 15
}
[스택]
주소     변수              값
0x1000  my_number         15
0x1008  single_reference  0x1000  ─→ my_number
0x1010  double_reference  0x1008  ─→ single_reference ─→ my_number
0x1018  five_references   0x????  ─→ ... ─→ ... ─→ ... ─→ ... ─→ my_number
```
- Rust가 자동으로 역참조해준 것이었다.
```rust
fn main() {
    let my_number = 15;
    let five_references = &&&&&my_number; // &&&&&i32 타입
    // 원래는 값을 얻으려면 * 를 5번
    println!("{}", *****five_references);  // 15
}
```
# 타입 변환과 가변 참조
- 불변 -> 가변으로 불가능하다.
- 가변 -> 불변으로 가능하다.
- 타입과 값 모두 가변으로
```rust
fn main() {
    let mut x = 5;
    let r2: &mut i32 = &mut x;  // OK
    *r2 = 10;
}
```
- 타입 추론 사용
```rust
fn main() {
    let mut x = 5;
    let r2 = &mut x;  // 타입 자동 추론: &mut i32
    *r2 = 10;
}
```
- 불변 참조로 통일
```rust
fn main() {
    let mut x = 5;
    let r2: &i32 = &x;  // OK (불변 참조)
    println!("{}", r2);
    // *r2 = 10;  // 쓰기 불가
}
```
- 불변 타입은 불변 참조
```rust
fn main() {
    let mut x = 5;
    // 가변 참조를 불변 참조로 "다운그레이드"
    let r: &i32 = &mut x;  // 가능
    println!("{}", r);
    // *r = 10;  // r은 &i32라서 쓰기 불가
}
```
# 다중 참조가 필요한 예
```rust
fn modify_reference(ptr: &mut &i32, new_target: &i32) {
    *ptr = new_target;  // 포인터가 가리키는 대상 변경
}
fn main() {
    let x = 5;
    let y = 10;
    let mut ref_x = &x;
    println!("{}", ref_x);  // 5
    modify_reference(&mut ref_x, &y);  // &&i32 (또는 &mut &i32)
    println!("{}", ref_x);  // 10
}
```
- ref_x는 가변 변수 (let mut)
- &mut ref_x는 가변 변수의 가변 참조
- ref_x 자체를 수정하는 것 (무엇을 가리킬지 변경)
- x, y 값은 변경 불가
# 소유권 다시
```rust
fn main() {
    let mut x = 5;
    let mut ref_x = &x;
    // ref_x를 함수에 넘기기
    some_function(ref_x);      // 가능 (&i32 전달)
    some_function(&mut ref_x); // 가능 (&mut &i32 전달)
    // x를 가변으로 넘기기
    other_function(&mut x);    // 불가능: ref_x가 이미 x를 불변으로 빌리고 있어서
}
```
- 소유권 넘어간 후에는, 다른 변수에 &x나 &mut x를 할당할 수 있다.
- x가 가변 변수여서 그렇고, 불변 변수였다면 &x만 넘길 수 있다.
# Stack
- 빠르다: 책을 쌓듯이 위에서부터 차곡차곡 쌓고, 위에서부터 빼냄
- 크기가 고정: 컴파일 타임에 크기를 알아야 함
- 자동 관리: 함수가 끝나면 자동으로 정리됨
- 다음 위치가 정해져 있음 (맨 위)
- 주소 찾을 필요 없이 그냥 쌓고 빼기만 하면 됨
- 나중에 쌓인 것부터 제거 (LIFO)
```rust
fn main() {
    let x = 5;        // 스택에 i32 (4바이트) 저장
    let y = 10;       // 스택에 i32 추가
    let z = x + y;    // 스택에 i32 추가
}   // 함수 끝 → 스택에서 자동으로 x, y, z 제거
```
- 함수마다 스택의 일부분(스텍 프레임)을 사용
```
전체 스택 (하나의 메모리 공간)
│
├─ main 함수의 스택 프레임
├─ function_a의 스택 프레임  
├─ function_b의 스택 프레임
└─ ...
```
- 스택 오버플로우
```rust
fn recursive(n: i32) {
    let x = n;
    recursive(n + 1);  // 끝없이 호출
}
// 스택이 이렇게 됨:
// [recursive: x=9999]
// [recursive: x=9998]
// [recursive: x=9997]
// ...
// [recursive: x=2]
// [recursive: x=1]
// [main]
// Stack Overflow 스택 공간 부족
```
# Heap
- 느리다: 빈 공간을 찾아야 한다.
- 크기가 가변: 런타임에 크기 결정
- 수동 관리: 사용 후 메모리 해제 필요 (Rust는 자동으로 해줌)
- 힙은 창고처럼, 어디에 빈 공간이 있는지 찾고, 데이터를 저장하고, 주소를 기억한다.
- 힙의 데이터에 접근할 때 주소로 찾아가는 과정 추가된다.
```rust
fn main() {
    let s = String::from("hello"); // 힙에 문자열 데이터 저장
    // s 자체(포인터, 길이, 용량)는 스택에
    // 실제 "hello" 데이터는 힙에 저장
}   // s가 drop되면서 힙 메모리도 자동 해제
```
# 스택과 힙
- 스택은 빠르고 힙은 느리다.
- 스택은 크기 고정 (컴파일 타임 결정), 힙은 가변 (런타임 결정)
- 스택 관리는 자동 (함수 끝나면 정리), 힙은 Rust가 자동으로 해줌
- 스택은 i32, f64, bool 등에 사용
- 힙은 String, Vec, Box 등에 사용
```rust
fn example() {
    // 스택: 크기가 고정된 것들
    let num = 42;           // i32: 4바이트
    let flag = true;        // bool: 1바이트
    // 힙: 크기가 가변적인 것들
    let text = String::from("hello");  // 길이가 변할 수 있음
    let list = vec![1, 2, 3];          // 요소 추가 가능
}
```
# 스택에 담을 수 없는 경우
- 컴파일 타임에 크기를 모를 때
```rust
fn main() {
    // 스택에 담을 수 없음: 크기를 미리 알 수 없음
    let user_input = String::new();  // 사용자가 뭘 입력할지 모름
    // 스택에 담을 수 있음: 항상 4바이트
    let x: i32 = 5;  // 무조건 4바이트
}
```
- 크기가 너무 클 때
```rust
fn main() {
    // 스택 오버플로우 위험
    // 스택은 보통 수 MB로 제한됨
    let huge_array = [0; 10_000_000];  // 40MB
    // 힙 사용: 크기 제한이 훨씬 큼
    let huge_vec = vec![0; 10_000_000];  // 가능
}
```
- 크기가 런타임에 변할 때
```rust
fn main() {
    // 배열: 크기 고정 (스택)
    let mut arr = [1, 2, 3];
    // arr.push(4);  // 불가: 크기 고정됨
    // Vec: 크기 변경 가능 (힙)
    let mut vec = vec![1, 2, 3];
    vec.push(4);     // 가능
    vec.push(5);     // 가능
}
```
# Pointer
- 포인터는 주소를 가리키는 화살표
```rust
fn main() {
    // 스택에만 저장
    let x = 5;              // 스택: [5]
    let y = x;              // 스택: [5, 5] (복사)    
    // 힙 사용 (포인터 필요)
    let s1 = String::from("hello");  
    // 스택: [포인터→힙주소, len:5, cap:5]
    // 힙: ['h','e','l','l','o']
    let s2 = s1;            
    // s1의 포인터를 s2로 이동 (복사 아님)
    // s1은 더 이상 사용 불가 (ownership 이동)
    // println!("{}", s1);  // 에러: s1은 이미 무효화됨
    println!("{}", s2);     // OK
}
```
# 소유권(Ownership)
```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;  // s1의 소유권이 s2로 "이동"
    println!("{}", s2);  // 가능
    // println!("{}", s1);  // 컴파일 에러
}
error[E0382]: borrow of moved value: `s1`
  --> src/main.rs:5:20
   |
2  |     let s1 = String::from("hello");
   |         -- move occurs because `s1` has type `String`
3  |     let s2 = s1;
   |              -- value moved here
4  |     
5  |     println!("{}", s1);
   |                    ^^ value borrowed here after move
```
- 소유권 없을 때 문제점
```cpp
// C++ 같은 언어에서
String s1 = "hello";
String s2 = s1;  // s1과 s2가 같은 힙 메모리를 가리킴
delete s1;  // 힙 메모리 해제
// s2는 이제 잘못된 메모리를 가리킴 (dangling pointer)
// s2 사용하면 크래시
```
- 소유권 이동 시 메모리
```rust
let s1 = String::from("hello");
// 메모리 상태:
// [스택]                [힙]
// s1 {
//   ptr: 0x1234  ────→  "hello"
//   len: 5
//   cap: 5
// }
let s2 = s1;  // 소유권 이동
// 메모리 상태:
// [스택]                [힙]
// s1 { 무효화됨 }
// s2 {
//   ptr: 0x1234  ────→  "hello"
//   len: 5
//   cap: 5
// }
```
- 힙 데이터는 복사되지 않음 (비용이 크니까)
- 포인터만 s2로 옮겨짐
- s1은 무효화되어서 접근 불가
- 이렇게 해야 메모리 해제가 한 번만 일어남
# s1 계속 쓰려면
- 방법 1: Clone (복사)
```rust
let s1 = String::from("hello");
let s2 = s1.clone();  // 힙 데이터를 실제로 복사
println!("{}", s1);  // 가능
println!("{}", s2);  // 가능
// 메모리 상태:
// [스택]                [힙]
// s1 { ptr: 0x1000 } ─→ "hello" (원본)
// s2 { ptr: 0x2000 } ─→ "hello" (복사본)
```
- 방법 2: 참조 (빌림, Borrow)
```rust
let s1 = String::from("hello");
let s2 = &s1;  // s1을 "빌림" (소유권 이동 없음)
println!("{}", s1);  // 가능
println!("{}", s2);  // 가능
// s1이 여전히 소유자이고, s2는 그냥 "보기만" 함
```
- 스택은 참조와 다르게 동작
```rust
let x = 5;        // i32는 스택에만 있음
let y = x;        // 복사됨 (Copy trait)
println!("{}", x);  // 5
println!("{}", y);  // 5
// i32 같은 타입은 크기가 작아서 복사가 저렴함
// 스택에만 있어서 포인터 없음
// 자동으로 복사됨 (Move가 아닌 Copy)
```
# 컴파일러가 하는 일
- 힙 메모리 할당/해제는 런타임에 일어난다.
- Rust 컴파일러는 컴파일 타임에 코드를 분석해서
- "이 코드는 런타임에 문제를 일으킬 것"이라고 미리 막는다.
```rust
// 컴파일러의 분석 (컴파일 타임):
fn main() {
    let s1 = String::from("hello");  // 1. s1이 소유자라고 기록
    let s2 = s1;                      // 2. 소유권이 s2로 이동, s1 무효화 표시
    // println!("{}", s1);               // 3. "s1은 이미 무효" → 컴파일 에러
}
```
- 소유권 없는 컴파일러는 문법 검사만 해서 런타임에 use-after-free 크래시 난다.
- rust는 컴파일 타임에 막는다.
```rust
let s1 = String::from("hello");
let s2 = s1;
// drop(s1);  // 메모리 해제 시도
// 컴파일 error: use of moved value: `s1`
// 힙 할당은 런타임에 일어나지만, 그 메모리를 안전하게 사용하는지는 컴파일 타임에 검증
```
# 포인터가 가리키는 것
- 스택을 가리키는 포인터 (참조, reference)
```rust
fn main() {
    let x = 5;           // 스택에 i32 저장
    let ptr = &x;        // x의 스택 주소를 가리키는 포인터
    println!("x = {}", x);      // 5
    println!("ptr = {}", ptr);  // 5
    // 메모리 구조:
    // [스택]
    // x:   5       (주소: 0x1000)
    // ptr: 0x1000  (x의 주소를 가리킴)
}
```
- 힙을 가리키는 포인터
```rust
fn main() {
    let s = String::from("hello");
    // 메모리 구조:
    // [스택]                    [힙]
    // s {
    //   ptr: 0x5000  ─────────→ "hello"
    //   len: 5
    //   cap: 5
    // }
    let ptr = &s;  // s의 스택 주소를 가리킴 (힙이 아니라)
    // ptr이 가리키는 것은? 스택의 s 구조체 (0x1000)
    // s.ptr이 가리키는 것은? 힙의 "hello" 데이터 (0x5000)
}
```
- & 참조: 스택이든 힙이든 뭐든 가리킬 수 있음
- Box, String, Vec 같은 타입: 내부적으로 힙을 가리키는 포인터를 가지고 있음
- 대부분의 경우 &x 같은 참조는 스택을 가리킨다.
# 혼동하기 쉬운 것
```rust
let x = 5;              
let ptr1 = &x;          
let s = String::from("hi");  
let ptr2 = &s;
[스택]
주소      변수    값
0x1000    x       5
0x1008    ptr1    0x1000      (x의 주소를 저장)
0x1010    s       {
                    ptr: 0x5000,  (힙 주소)
                    len: 2,
                    cap: 2
                  }
0x1028    ptr2    0x1010      (s의 주소를 저장)
[힙]
주소      값
0x5000    'h', 'i'
```
- x는 그냥 5라는 값만 가지고 있음
- x가 0x1000에 "위치한" 것이지, x가 주소를 "저장"하는 건 아님
- ptr1, ptr2는 다른 변수의 주소를 값으로 가짐
- s는 3가지 정보를 담은 구조체 (ptr 필드: 힙 주소, len 필드: 길이, cap 필드: 용량)
- 모든 변수는 자신의 메모리 주소에 "위치"한다.
- 포인터 변수만이 다른 변수의 주소를 "값으로 저장"한다.
- x, ptr1, ptr2, s 전부 변수
- 일반 변수(a regular variable): x, 포인터 변수(a reference): ptr1, ptr2
- s는 "포인터를 포함한 구조체"
- x는 i32, ptr1은 &i32, ptr2는 String, s는 &String 타입
# 혼동하기 쉬운 것2
- let mut로 선언하면 가변 변수(mutable variable)
- let mut: 모든 (스택, 힙) 타입에 적용 가능, 포인터도 가능
- 변수의 가변성
```rust
let mut x = 5;
x = 10;      // 변수 x에 다른 값 할당
```
- 데이터의 가변성 (가변 참조)
```rust
let mut x = 5;
let ptr = &mut x;  // 가변 참조 (mutable reference)
*ptr = 10;         // x의 값을 포인터를 통해 변경 (쓰기 가능)
```
- 불변 참조
```rust
let x = 5;
let ptr = &x;      // 불변 참조
// *ptr = 10;      // 에러
println!("{}", ptr); // 읽기만 가능
```
- 가변 변수, 참조 둘 다 사용
```rust
let mut x = 5;
let mut ptr = &mut x;
// ptr은 가변 변수 (다른 걸 가리킬 수 있음)
// &mut x는 가변 참조 (x를 수정할 수 있음)
*ptr = 10;      // x를 10으로 수정
let mut y = 20;
ptr = &mut y;   // 이제 y를 가리킴
*ptr = 30;      // y를 30으로 수정
```