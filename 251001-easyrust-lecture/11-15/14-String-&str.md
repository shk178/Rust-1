- Rust 문자열 2가지 타입 있다.
# &str: 문자열 슬라이스 (string slice)
- let greeting = "Hello, world!";
- 고정된 크기: 컴파일 시점에 크기가 정해져 있다.
- 빠름: 메모리 할당이 없기 때문에 성능이 좋다.
- 참조 타입: &str은 str에 대한 참조(reference)다. 앞에 &가 붙는다.
- 소유권 없음: &str은 데이터를 빌려오는 형태라서 직접 수정하거나 소유할 수 없다.
# String: 힙에 저장된 문자열
- let mut name = String::from("shk178");
- 가변 크기: 문자열을 추가하거나 수정할 수 있다.
- 힙에 저장됨: 데이터가 힙(heap)에 저장되므로 런타임에 크기를 바꿀 수 있다.
- 소유권 있음: String은 데이터를 소유한다. 자유롭게 수정하거나 이동할 수 있다.
- 더 많은 기능 제공: push, replace, split 같은 다양한 메서드를 사용할 수 있다.
# 왜 &str에는 &가 붙을까
- &str은 참조(reference) 타입이다.
- 어떤 문자열 데이터가 있는 메모리 주소를 가리키는 포인터다.
- 이 참조는 고정된 크기를 가지는데, 64비트 시스템에서는 8바이트다.
- Rust는 컴파일 시점에 스택에 저장될 데이터의 크기를 반드시 알아야 한다.
- str 자체는 크기가 고정되어 있지 않아서 컴파일 타임에 크기를 알 수 없다.
- &str처럼 참조 형태로 사용하면 크기를 알 수 있어서 스택에 저장할 수 있다.
# str은 런타임에 힙에 할당된 데이터일까
- str은 크기가 고정되지 않은 문자열 타입이다.
- 직접 변수로 사용할 수 없고, 항상 참조(&str)로 사용한다.
- str 데이터는 보통 힙(heap)에 저장된다.
```rust
let s: String = String::from("Hello");
let slice: &str = &s; // String의 힙 데이터를 참조
// String은 힙에 "Hello"라는 데이터를 저장
// &str은 그 데이터를 참조
```
- 하지만 "Hello"처럼 리터럴 문자열은
- 프로그램의 바이너리 안에 고정된 위치에 저장되기도 한다.
- 이 경우에도 &str은 그 위치를 참조한다.
- 힙에 할당된다고 단정할 수는 없다.
- 런타임에 접근 가능한 메모리 어딘가에 존재한다고 보면 된다.
# 리터럴 문자열 저장 장소
- 프로그램의 바이너리 안에 있는 읽기 전용 메모리 영역에 저장된다.
- "static memory" 또는 "read-only data segment (.rodata)"라고 부른다.
- let greeting = "Hello, world!"; // 아래와 같은 문장
- "Hello, world!"는 컴파일 시점에 고정된 위치에 저장된다.
- 런타임에 &str 타입으로 그 위치를 참조한다.
- 힙도, 스택도 아니고 프로그램이 시작될 때부터 존재하는 고정된 메모리다.
- let greeting: &str = "Hello, world!"; // 위와 같은 문장
- greeting은 "Hello, world!"라는 문자열이 저장된 위치를 가리키는 참조다.
- 참조는 스택에 저장되지만, 문자열 데이터는 바이너리의 읽기 전용 영역에 있다.
# 다른 리터럴은 스택에 저장될까?
- 숫자 리터럴, 불리언, 문자(char) 같은 값은 스택에 직접 저장된다.
```rust
let x = 42;      // 정수 리터럴 → 스택에 저장
let flag = true; // 불리언 리터럴 → 스택에 저장
let ch = 'A';    // 문자 리터럴 → 스택에 저장
```
- 크기가 고정되어 있고, 값 자체가 작기 때문에 스택에 들어간다.
- 문자열은 길이가 가변적일 수 있고, str 타입은 크기를 알 수 없는 unsized 타입이다.
- 그래서 직접 스택에 저장할 수 없고, 참조(&str) 형태로 다룬다.
- str = a dynamically sized type
```rust
fn main() {
    println!("A String is always {:?} bytes. It is Sized.", std::mem::size_of::<String>());
    // std::mem::size_of::<Type>() gives you the size in bytes of a type
    println!("An i8 is always {:?} bytes. It is Sized.", std::mem::size_of::<i8>());
    println!("An f64 is always {:?} bytes. It is Sized.", std::mem::size_of::<f64>());
    println!("A &str? It can be anything. '서태지' is {:?} bytes. It is not Sized.", std::mem::size_of_val("서태지"));
    // std::mem::size_of_val() gives you the size in bytes of a variable
// A String is always 24 bytes. It is Sized.
// An i8 is always 1 bytes. It is Sized.
// An f64 is always 8 bytes. It is Sized.
// A &str? It can be anything. '서태지' is 9 bytes. It is not Sized.
}
```
# 문자열 리터럴을 String으로 변환 과정
```rust
let s: &str = "Hello, world!";
let owned: String = s.to_string(); // 문자열 리터럴 -> String
```
- (1) 힙 메모리 할당
- "Hello, world!"의 길이를 계산하고, 그만큼의 공간을 힙에 할당
- "Hello, world!"는 13바이트 → 힙에 13바이트 공간 확보
- (2) 데이터 복사
- 읽기 전용 메모리에 있는 "Hello, world!"의 내용을 힙에 복사
- 복사된 데이터는 이제 String이 소유
- (3) 포인터 + 길이 + 용량 저장
- String은 내부적으로 세 가지 정보를 저장
- 포인터: 힙에 복사된 문자열의 시작 주소 (런타임에 할당된 힙 주소)
- 길이: 현재 문자열의 길이
- 용량: 할당된 힙 공간의 크기 (길이와 같거나 더 큼)
```rust
struct String {
    ptr: *const u8, // 힙에 있는 문자열 데이터 주소
    len: usize,     // 문자열 길이
    capacity: usize // 힙에 할당된 총 크기
    // 구조 덕분에 String은 문자열을 수정하거나 확장할 수 있다.
}
```
```rust
let s2 = String::from("Hello");
// 문자열 리터럴 "Hello"를 &str로 해석한 다음, 그것을 String으로 변환
```
# The format! macro.
- This is like println! except it creates a String instead of printing.
```rust
fn main() {
    let my_name = "Billybrobby";
    let my_country = "USA";
    let my_home = "Korea";
    let together = format!(
        "I am {} and I come from {} but I live in {}.",
        my_name, my_country, my_home
    );
    println!("{}", together);
    // I am Billybrobby and I come from USA but I live in Korea.
}
```
# .into()
- 한 타입을 다른 타입으로 변환할 때 사용하는 범용 메서드
- `From 트레이트`가 구현되어 있으면 자동으로 사용할 수 있다.
```rust
let s: String = "Hello".into();
// "Hello"라는 &str을 String으로 변환하는 코드
// String::from(&str)을 내부적으로
// From<&str> for String으로 구현해놨기 때문
```
- .into()는 From<&str>이 구현된 모든 타입으로 변환할 수 있다.
- 변환할 타입을 명시 안 하면, 컴파일 에러 난다.
```rust
fn main() {
    let s = "Hello".into();
    // error[E0283]: type annotations needed
    // note: cannot satisfy `_: From<&str>`
    // note: required for `&str` to implement `Into<_>`
    // let s: /* Type */ = "Hello".into();
}
```
# From<&str> 트레이트를 구현한 타입들
- String: 가장 흔한 변환 대상. 힙에 복사해서 소유권을 가짐
- Cow<'a, str>: "Clone-on-write" 타입. &str을 그대로 쓰거나 필요 시 String으로 복사
- PathBuf: 파일 경로를 나타내는 타입. 문자열을 경로로 변환
- OsString: 운영체제 문자열 타입. 플랫폼에 따라 다르게 처리됨
- Box<str>: 힙에 저장된 str. &str을 힙에 복사해서 박싱함
- Rc<str>, Arc<str>: 참조 카운팅 기반의 힙 문자열. &str을 공유 가능한 형태로 변환
- 커스텀 타입들도 있다.
# Rust의 주요 문자열 타입 8가지
- (1) &str
- 문자열 슬라이스. 읽기 전용이며, 바이너리나 힙의 문자열을 참조함
- 불변, 소유x, 일반 문자열 참조
- (2) String
- 힙에 저장된 가변 문자열. 소유권을 가지며 수정 가능
- 가변, 소유o, 일반 문자열 소유
- (3) Box<str>
- 힙에 저장된 문자열 슬라이스. String보다 메모리 절약 가능하지만 불변
- 불변, 소유o, 힙에 저장된 불변 문자열
- (4) Cow<'a, str>
- "Clone-on-write" 문자열. &str로 시작해서 필요 시 String으로 복사
- 가변, 소유x or o, 효율적인 문자열 복사 제어
- (5) OsString
- 운영체제 문자열. Windows와 Unix에서 다르게 동작함
- (6) &OsStr
- OsString의 참조 버전. 시스템 API와 상호작용할 때 사용
- (7) PathBuf
- 파일 경로를 나타내는 문자열. 내부적으로 OsString 기반
- (8) &Path
- PathBuf의 참조 버전. 경로를 읽기 전용으로 다룰 때 사용
# String은 Sized Type
- String은 힙에 저장되는 가변 문자열
- 크기가 고정되어 있음을 의미하는 Sized trait를 구현한다.
```
- String 타입은 크기가 고정된 3개의 필드를 가지는 구조체다.
- ptr: 힙에 있는 문자열 데이터를 가리키는 포인터
- len: 문자열 길이
- capacity: 할당된 힙 메모리 용량
- 이 세 필드는 모두 고정된 크기를 가지므로, String 타입 자체는 Sized다.
- 즉, String 변수는 스택에 24바이트(64비트 시스템 기준)로 저장된다.
```
- 컴파일 타임에 String의 포인터 크기와 구조가 명확하게 정의되어 있어서
- 스택에 포인터를 저장하고, 실제 문자열은 힙에 저장된다.
```
- "hello"라는 문자열은 5바이트이고, 이는 힙에 저장된다.
- String의 ptr 필드가 그 위치를 가리킨다.
```
- let s: String = String::from("hello");
```
- s는 String 타입의 인스턴스를 직접 담고 있는 변수
- s는 String 객체 자체를 소유하고 있고, 그 객체는 스택에 24바이트 크기로 존재한다.
```
- String은 소유권을 가지며, push, pop 등으로 내용 변경할 수 있다.
- 크기가 명확하므로 함수 인자나 구조체 필드로 직접 사용할 수 있다.
# str은 Dynamically Sized Type
- 크기가 컴파일 타임에 결정되지 않는 타입
- str 자체는 크기를 알 수 없어 직접 사용 불가
- let s: &str = "hello";
- "hello"는 리터럴로 프로그램 바이너리에 저장되고,
- 문자열 리터럴은 프로그램 바이너리에 저장되며 불변이다.
- &str은 그 위치를 가리키는 불변 참조다.
- 다른 문자열을 가리키도록 가변 참조를 할 수는 있다.
```rust
let mut s: &str = "hello";
s = "world"; // 가능
// s.push_str("world") 같은 문자열 수정은 불가능
```