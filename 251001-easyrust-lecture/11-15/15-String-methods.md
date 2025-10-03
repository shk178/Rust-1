```rust
fn main() {
// String
    // .capacity
    // .push
    // .push_str
    // .pop
    // .with_capacity
    // method = .function으로 사용하는 함수
    let mut my_name = "David".to_string();
    my_name.push('!');
    println!("{}", my_name); // David!
    my_name.push_str("!!");
    println!("{}", my_name); // David!!!
    println!("{}", my_name.capacity()); // 10
}
``
# https://doc.rust-lang.org/std/string/struct.String.html#method.capacity
- Returns this String’s capacity, in bytes.
```rust
let s = String::with_capacity(10);
assert!(s.capacity() >= 10);
// String 타입의 변수를 생성하면서 초기 용량을 10으로 설정
// 문자열 s는 아직 아무 문자도 담고 있지 않지만
// 최대 10개의 문자를 저장할 수 있는 메모리를 미리 확보했다.
// 성능 최적화를 위해 사용한다.
// 문자열을 반복적으로 추가할 때 재할당을 줄일 수 있다.
// assert!는 조건이 true가 아니면 프로그램을 panic 상태로 종료
// s.capacity()가 10 이상인지 확인
// with_capacity(10)로 만들어서, 항상 참이므로 에러 없이 실행
```
# 스택과 힙 메모리 공간 관리
- (1) 운영체제(OS)
- 프로그램이 실행될 때, 전체적인 메모리 공간을 할당
- 예를 들어, 스택과 힙을 위한 영역을 프로세스에 배정
- (2) 런타임(Runtime)
- 프로그램이 실행되는 동안, 메모리의 세부 관리를 담당하는 소프트웨어 계층
- Java는 JVM이라는 런타임 위에서 돌아가고
- Python은 인터프리터가 런타임 역할
- Rust에서는 런타임이 거의 없거나 매우 얇은 수준
- 대부분의 작업이 컴파일 타임에 결정
- 힙 메모리 할당, panic 처리, 스레드 생성 등 일부 기능은 런타임에 수행
- 힙 할당과 해제는 내부적으로 표준 라이브러리와 할당자(allocator)가 처리
- (3) 메모리 할당 후 스택
- 관리: 컴파일러가 자동 관리
- 용도: 함수 호출, 지역 변수
- 메모리 해제: 함수 종료 시 자동으로
- (4) 메모리 할당 후 힙
- 관리: 프로그래머나 런타임
- 용도: 동적 크기 데이터 (String, Vec)
- 메모리 해제: 명시적 해제 또는 자동으로
# 재할당 (Reallocation)
- 기존 힙 메모리 공간이 부족하면
- 더 큰 공간을 새로 확보하고
- 기존 데이터를 복사한 뒤
- 이전 메모리 자동으로 해제 (소유권 시스템이 처리)
# .push를 하면
- Rust String은 동적 배열(Vec<u8>) 기반이라서
- 내부에 capacity(용량)과 length(현재 길이)를 따로 관리
- capacity: 현재 힙에 확보해 둔 메모리 크기 (바이트 단위)
- len: 실제로 사용 중인 문자열 길이
- push 할 때 현재 capacity가 남아 있으면 → reallocation 없음
- capacity를 초과하면 → 새 메모리 할당, 기존 데이터 복사
```rust
let mut my_name = "David".to_string();
println!("{}", my_name.capacity()); // 보통 10 이상 (환경마다 다를 수 있음)
my_name.push('!');   // 길이 = 6, capacity 충분 → realloc 없음
my_name.push_str("!!"); // 길이 = 8, capacity(10) 안 넘음 → realloc 없음
my_name.push_str("Hello World"); // reallocation 발생
```
- String::with_capacity(n)을 쓰면 예상되는 메모리를 확보해 realloc을 줄일 수 있다.
---