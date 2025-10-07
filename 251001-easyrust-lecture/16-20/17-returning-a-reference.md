- 메모리 안정성 - 소유권, 참조 관련
- ownership: 데이터를 누가, 언제까지 가지는지
# String과 &String의 차이
```rust
let country = String::from("Austria");
let ref_one = &country;
```
- country: 실제 데이터를 소유(own)하는 변수
- 데이터 주인은 데이터가 언제 메모리에서 사라질지(drop) 결정
- ref_one: country를 참조(reference)하는 변수
- 참조 변수는 데이터 주인이 아니고 빌려쓰는 것
# 참조(reference)
```rust
let ref_one = &country;
let ref_two = &country;
```
- 읽기 전용 참조는 여러 개 가능
# 함수 안에서 만든 값을 참조해서 반환 불가
```rust
// 컴파일 오류 난다.
fn return_str() -> &str {
    let country = String::from("Austria"); // country라는 String을 새로 만듦
    let country_ref = &country; // country_ref는 country를 참조
    country_ref // 문제 발생 (참조 수명 문제)
    // 함수가 끝나면, 지역 변수 country는 메모리에서 사라진다.
    // 그런데 country_ref는 그 사라진 메모리를 가리키고 있다.
}
```
- rust 컴파일러가 막는다.
# 데이터를 함수 밖으로 전달하려면
- 방법 (1) 소유권을 넘기기 (String 반환)
```rust
fn return_str() -> String {
    let country = String::from("Austria");
    country // 소유권을 밖으로 넘김
}
```
- 이렇게 하면 country의 소유권이 함수 바깥으로 이동(move)
- 함수가 끝나도 데이터가 유효하게 남는다.
- 방법 (2) 수명(Lifetime)을 지정해서 참조를 반환
- (1)과 달리, 빌려온 데이터를 그대로 반환할 때 쓸 수 있는 방법
- 함수에서 만든 게 아니라 밖에서 들어온 참조를 그대로 내보내는 경우에만 가능
```rust
fn return_ref<'a>(input: &'a str) -> &'a str {
    input // return_ref는 밖에서 빌린 참조(&s)를 그대로 반환
} // Rust는 'a라는 수명 표식을 보고, 반환값은 입력 참조만큼만 유효하다는 걸 안다.
fn main() {
    let s = String::from("Austria");
    let s_ref = return_ref(&s);
    println!("{}", s_ref);
}
```
- 방법 (3) 'static 수명을 가지는 상수 데이터 사용
- 데이터가 프로그램 전체 생명주기 동안 유지된다면, 참조를 반환해도 안전
```rust
fn return_str() -> &'static str {
    "Austria" // 문자열 리터럴은 프로그램 끝날 때까지 살아있음
}
fn main() {
    let s = return_str();
    println!("{}", s);
}
```
- 방법 (4) Rc / Arc / RefCell 등 스마트 포인터 사용
- Rc<T>: 참조 카운트를 세어서 여러 곳이 같은 데이터를 소유 가능
- 멀티스레드 환경에서는 Arc(atomic reference count) 사용
# &'static str 반환 시
- (1) "Austria"는 문자열 리터럴
```rust
fn return_str() -> &'static str {
    "Austria"
}
```
- 리터럴 문자열은 프로그램 실행 파일 내부의 상수 영역(static memory)에 저장
- 함수를 실행하지 않아도 "Austria"는 이미 실행 파일 안에 포함되어 있다.
- 프로그램이 메모리에 로드될 때 함수 코드와 함께 적재된다.
- 즉, 문자열 리터럴은 프로그램이 시작할 때 이미 메모리에 올라가 있다.
- (2) 메모리 구조
```
+-----------------------------+
| Stack                      | ← 함수 호출 시 지역 변수들
+-----------------------------+
| Heap                       | ← 런타임 중에 동적 할당 (String, Vec 등)
+-----------------------------+
| Static / Global Data        | ← 전역 변수, static 변수, 문자열 리터럴 등
|    "Austria"                | ← 여기에 저장됨
+-----------------------------+
| Code (Text Segment)         | ← 함수 코드
+-----------------------------+
```
- "Austria"는 읽기 전용(static) 데이터 영역에 저장되어 있다.
- 프로그램이 실행될 때 OS가 그 영역 전체를 한 번에 메모리에 올린다.
```rust
fn main() {
    let s1: &str = "Austria";
    let s2: &str = "Austria";
    println!("s1 addr = {:p}", s1);
    println!("s2 addr = {:p}", s2);
// 두 변수가 같은 주소를 가리킨다.
}
// 프로그램 빌드 시 —
// "Austria" 리터럴이 바이너리의 데이터 섹션에 포함된다.
// .rodata 영역이라고 부른다. (read-only data)
// 프로그램 실행 시 —
// 함수 호출 전, main() 실행 전
// 운영체제가 이 .rodata 영역을 메모리에 올린다.
// 어떤 함수에서든 "Austria"를 참조할 때
// 이미 올라와 있는 메모리 주소를 참조한다.
```
- (3) String::from("Austria")
```rust
fn return_str() -> &'static str {
    let country = String::from("Austria");
    &country
}
```
- country는 지역 변수
- String::from("Austria")는 힙(heap)에 문자열 데이터를 저장
- 그 힙 데이터의 소유권(String 구조체)은 스택(stack)에 위치
- 함수가 끝나면 country는 스코프를 벗어나므로
- Rust가 자동으로 drop()을 호출해서 메모리를 해제
- 그런데 &country는 그 해제된 메모리를 가리키게 되므로 컴파일 오류
- (4) &country는 'a라는 수명
```rust
// (3)을 컴파일러가 이런 식으로 해석
fn return_str<'a>() -> &'a str {
    let country = String::from("Austria");
    &country
}
```
- 'a를 'static으로 확장시킬 수 없다. (속일 수 없다.)
- error[E0515]: cannot return reference to local variable `country`
- (5) String::from("Austria") // 값을 생성하는 표현식(expression)
- 런타임에 동작한다.
- 먼저 문자열 리터럴(static 메모리의 &'static str) "Austria"를 인자로 넘긴다.
- String::from() 함수가 내부적으로 힙에 새 버퍼를 만들고
- "Austria"의 내용을 그 안에 복사한다.
- 결과로 String 타입 값이 생성되지만, 그 값이 저장되지 않아 즉시 drop이 호출된다.
```rust
[static memory]  "Austria"
       ↓
[String::from()]  힙에 새 버퍼 생성, 내용 복사
       ↓
[String 객체 생성] → 스택에 임시로 위치
       ↓
함수 끝 → drop(String) 호출 → 힙 메모리 해제
```
- (6) let s = String::from("Austria");
- 생성된 값을 변수 s에 바인딩한다.
- String 객체(포인터, 길이, 용량 구조체)가 스택에 생성될 때,
- 이 스택 변수의 이름이 s다.
- 함수가 끝날 때 s가 스코프를 벗어나면 drop이 호출된다.
```rust
[Static memory]
    "Austria"  (읽기 전용 리터럴)
       ↓
[String::from()]
       ↓
[Heap]   "Austria" 복사본 저장
[Stack]  s = { ptr -> heap주소, len = 7, capacity = 7 }
```
- (7) 컴파일러 입장에서 보면
- Rust는 모든 표현식이 값을 가진다는 언어
- String::from("Austria");도 값을 반환하는 표현식
```rust
// 두 줄은 사실상 같은 의미
String::from("Austria");  // 값을 생성하고 즉시 drop
let _ = String::from("Austria");  // 생성 후 '_'에 바인딩 (즉시 drop)
```
# &String과 &str
- String 자체에 대한 참조를 반환
- 오류 이유: 참조가 해제된 데이터를 가리킴
- 지역 변수에 대한 참조를 반환할 수 없음
```rust
fn return_it() -> &String {
    let country = String::from("Austria");
    &country // country 타입: &String
} // 반환 타입: &String
fn main() {
    return_it();
}
// error[E0106]: missing lifetime specifier --> src/main.rs:1:19
```
- 문자열 데이터에 대한 &str 슬라이스 참조를 반환
- 오류 이유: 슬라이스가 해제된 데이터를 가리킴
- 지역 변수에 대한 참조를 반환할 수 없음
```rust
fn return_str() -> &str {
    let country = String::from("Austria");
    let country_ref = &country;
    country_ref // country_ref 타입: &String
} // 반환 타입: &str
fn main() {
    return_str();
}
// error[E0106]: missing lifetime specifier --> src/main.rs:1:20
```
- &country는 &str로 자동 변환(Deref)될 수 있다.
```rust
let country = String::from("Austria"); // country는 String 타입
let s: &str = &country; // s는 &str 타입으로 선언
// 허용됨 — String이 Deref<Target = str>을 구현했기 때문
```
- Deref 트레이트
```rust
trait Deref {
    type Target: ?Sized;
    fn deref(&self) -> &Self::Target;
}
// * 연산자(*x)나 참조 변환을 통해
// &T → &U로 바꾸는 방법을 정의할 수 있다.
```
- String의 Deref 구현
```rust
impl Deref for String {
    type Target = str;
    fn deref(&self) -> &str {
        &self[..] // 내부적으로 String의 버퍼를 슬라이스로 반환
    }
// String은 Deref<Target = str>을 구현하므로
// *country (혹은 &*country) 하면
// 자동으로 &str로 바뀔 수 있다.
}
- Deref 강제 변환 (Deref coercion)
```rust
// Rust는 함수 인자나 반환값을 맞출 때
// 자동으로 Deref를 호출해서 타입을 변환
let s1: &str = &*country; // 명시적으로 Deref 호출
let s2: &str = &country;  // 자동 변환 (Deref coercion)
// 컴파일러가 &String → &str로 자동 변환을 삽입
// 두 줄은 동일하게 동작
```
# Deref coercion이 작동하는 상황
- (1) 함수 인자 전달 시
```rust
fn print_str(s: &str) {
    println!("{}", s);
}
let country = String::from("Austria");
print_str(&country); // &String → &str 자동 변환
```
- (2) 함수 반환 시
```rust
fn get_str() -> &str {
    let s = String::from("Hi");
    &s  // 여기서 &String → &str 시도됨 (하지만 수명 문제로 오류)
}
```
- (3) 메서드 호출 시
```rust
let s = String::from("hello");
println!("{}", s.len()); // s.len()은 실제로 (&*s).len() 호출
```
# &str -> &String은 불가
- &str -> &String은 자동 변환되지 않는다.
- Deref는 소유자가 내부 데이터를 빌려주는 방향으로만 동작
- (1) &String -> &str는 가능
- String이 내부에 str 데이터를 포함하고 있다.
- (2) &str -> &String는 불가
- &str은 단순한 뷰(slice)일 뿐, String의 구조를 복원할 수 없다.
- (3) String은 구조체이고, &str은 포인터+길이 쌍이다.
- String = 건물 (소유권 + 구조물 + 내부 데이터)
- &str = 그 건물 안의 방 하나를 가리키는 지도
- (4) 참조가 아니라 새 String을 만들어서 변환할 수 있다.
```rust
let slice: &str = "hello";
let s: String = slice.to_string();     // 새 String 소유권 생성
let s2: String = String::from(slice);  // 같은 의미
```
- (5) &str -> &String 할 일이 별로 없다.
- &str 그대로 반환해도 참조 유지된다.
- (6) 정리
- String -> &str: 자동 (Deref)
- &String -> &str: 자동 (Deref)
- &str -> String: 명시적 (to_string()/String::from)
- &str -> &String: 불가