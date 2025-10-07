- unsafe를 쓰지 말자 (static mut)
# 'static lifetime
- lifetime = 참조가 얼마나 오래 유효한가를 컴파일러가 추적하기 위한 개념
- 'static lifetime = rust에서 가장 긴 생명주기
- 프로그램 실행 내내 유효한 데이터 (값 또는 참조)를 말한다.
- (1) 문자열 리터럴
```rust
let s: &'static str = "hello world";
// 문자열 리터럴 "hello world"는 바이너리에 하드코딩되어 프로그램이 끝날 때까지 존재
```
- (2) static 변수
```rust
static NAME: &str = "Rust";
fn main() {
    println!("{}", NAME);
}
```
- (3) 'static 제약이 있는 제네릭 타입
```rust
// 스레드에서 사용되는 클로저는 'static 제약이 필요할 때가 많다.
use std::thread;
fn main() {
    let s = String::from("hello");
    // 아래 코드는 컴파일 에러
    // thread::spawn의 클로저는 'static lifetime을 요구
    // 스레드가 부모 함수(main)보다 오래 살아있을 수 있기 때문
    thread::spawn(|| {
        println!("{}", s);
    });
    // 해결 방법: s를 move로 소유권 이전
    thread::spawn(move || {
        println!("{}", s);
    });
}
```
- (4) 'static을 사용하는 이유
- 데이터가 프로그램 전체에서 살아남아야 할 때
- 스레드나 비동기 작업 등에서, 부모 스코프보다 오래 존재할 가능성이 있을 때
- 제너릭 제약 조건으로 lifetime을 강제하고 싶을 때
```rust
fn use_static<T: 'static>(val: T) {
    // val은 프로그램 전체 동안 유효해야 함
}
```
# 어트리뷰트(attribute)
- 컴파일러에게 특정한 지시나 메타데이터를 전달하는 주석
- `#[derive(...)]` - 자동 trait 구현
- `#[cfg(...)]` - 조건부 컴파일
- `#[allow(...)]`, `#[warn(...)]`, `#[deny(...)]` - 컴파일러 경고/에러 제어
- `#[test]` - 테스트 함수 지정
- `#[inline]` - 인라인 최적화 요청
# const, static
- 값 선언: let, static, const
- const와 static: 변하지 않는 값, global scope
- (1) const
- 값이 바뀌지 않는 상수
- 사용될 때마다 그 값 자체로 대체된다.
- 메모리 상 고정된 위치가 없다.
- (2) static
- 메모리에 고정된 위치 있음
- 전역 변수처럼 동작
- 실무에서는 const > static
- (3) 작성 규칙
- 타입 추론 안 되므로, 타입을 반드시 명시
- 모두 대문자로 작성, 단어 사이는 _로 구분
- main 함수 밖에 선언 (프로그램 전체에서 사용하기 위해)
- (4) 작성 예
```rust
const NUMBER_OF_MONTHS: u32 = 12;
static SEASONS: [&str; 4] = ["Spring", "Summer", "Fall", "Winter"];
```
# let과 다른 점
- let은 기본적 불변, let mut로 mutable하게 만들 수 있다.
- const/static은 mut 사용 불가 (항상 불변)
- let은 함수 안에서 선언하는 지역 변수, 블록 스코프 내에서만 유효하다.
- const/static은 보통 전역으로 함수 밖에서 선언, 프로그램 전체에서 접근 가능
- let은 타입 추론 가능
- const/static은 타입 추론 안 되므로 타입 명시
- let은 런타임에 값이 결정될 수 있다.
```rust
let x = get_value();
```
- const/static은 컴파일 타임에 값이 결정되어야 한다. (계산된 고정 값만 가능하다.)
# 컴파일 시 값으로 대체
- const는 컴파일 시 값으로 대체된다.
```rust
// 컴파일 전
const MAX_SPEED: u32 = 100;
fn main() {
    let limit = MAX_SPEED;
    println!("{}", MAX_SPEED);
    let double = MAX_SPEED * 2;
}
// 컴파일 후
fn main() {
    let limit = 100;
    println!("{}", 100);
    let double = 100 * 2;
}
```
- const 상수는 최종 프로그램에 MAX_SPEED라는 이름이 없다.
- let 변수는 메모리에 공간을 할당받고, 프로그램 실행 중에 메모리 주소를 가진다.
- const처럼 하면 메모리 접근을 안 해도 돼서 빠르다.
- const처럼 하면 컴파일러 최적화에 유리하다.
# static 값 대체x
```rust
static SEASONS: [&str; 4] = ["Spring", "Summer", "Fall", "Winter"];
fn main() {
    println!("{:?}", SEASONS);
}
```
- static도 메모리에 공간을 할당받는다.
- 프로그램 실행 동안 특정 주소에 계속 있다.
- 컴파일해도 값으로 대체되지 않는다.
- const는 `복사본`처럼 사용할 때마다 값이 코드에 써진다.
- static은 하나의 고정된 메모리 위치를 코드에서 계속 참조한다.
# 값을 변경 가능한 전역 변수
- (1) static mut
```rust
static mut COUNTER: i32 = 0;
fn main() {
    unsafe {
        COUNTER += 1;
        println!("{}", COUNTER);
    } // unsafe 블록에서만 사용 가능
}
```
- 누가 데이터를 소유하는지 불명확한 문제점
- 스레드 동시 접근 시 데이터 레이스 문제점
- 코드 예측 가능성 저하 문제점
- (2) Mutex로 감싼 static
```rust
use std::sync::Mutex;
static COUNTER: Mutex<i32> = Mutex::new(0);
fn main() {
    let mut num = COUNTER.lock().unwrap();
    *num += 1;
    println!("{}", *num); // 1
}
```
- 스레드 안전하다.
- unsafe를 안 써도 된다.
# const > static 이유
- const는 메모리 접근 불필요로 더 빠름
- 대부분 상수는 간단한 값이라 `복사`해도 괜찮다.
- 컴파일러가 최적화하기 좋다.
# static 쓰는 상황
- (1) 큰 데이터
```rust
// 이걸 여러 곳에서 복사하면 비효율적
static LARGE_TABLE: [u8; 10000] = [0; 10000];
```
- (2) 메모리 주소가 필요
```rust
// 포인터로 접근해야 하는 경우
static VALUE: i32 = 42;
fn get_address() -> *const i32 {
    &VALUE as *const i32
}
```
- (3) 복잡한 타입
```rust
// 문자열 슬라이스 같은 것
static MESSAGE: &str = "Hello, World!";
```
# 고정 주소 여부
- (1) static, static mut
- 고정 주소다. 프로그램 실행 중 안 바뀐다.
- 데이터 영역에 저장된다.
- (2) let
- 고정 주소가 아니다. 함수 호출시마다 다르다.
- 스택에 할당된다.