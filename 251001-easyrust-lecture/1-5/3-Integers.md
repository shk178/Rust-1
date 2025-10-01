- primitive types (primitive = very basic)
- Signed - 양수/음수 모두 표현
- i8, i16, i32, i64, i128, and isize.
- 숫자는 비트 수를 의미: `-2^(n-1)` ~ `2^(n-1) - 1`
- Unsigned - 양수만 표현
- u8, u16, u32, u64, u128, and usize.
- `0` ~ `2^n - 1`
- 특수 타입 - isize와 usize
- 실행 환경의 CPU 아키텍처에 따라 크기가 결정됨
- 32비트 시스템: isize = i32, usize = u32
- 64비트 시스템: isize = i64, usize = u64
- 기억하기 쉬운 값들
```
u8:   0 ~ 255              // 2^8 - 1
u16:  0 ~ 65,535           // 2^16 - 1 (약 6만)
u32:  0 ~ 4,294,967,295    // 2^32 - 1 (약 42억)
u64:  0 ~ 18.4경           // 2^64 - 1
i8:   -128 ~ 127           // -(2^7) ~ (2^7 - 1)
i16:  -32,768 ~ 32,767     // 대략 ±3만
i32:  -2,147,483,648 ~ 2,147,483,647  // 대략 ±21억
i64:  ±9.2경               // 대략 ±9경
```
- 주요 사용 사례
```
u8: 바이트 데이터, RGB 색상 값 (0-255)
i32: 기본 정수 타입 (타입 명시 안 하면 자동 선택)
u32: 인덱스가 아닌 큰 양수
usize: 배열/벡터 인덱스, 컬렉션 크기 (메모리 주소 관련)
isize: 포인터 연산, 상대적 오프셋
```
- Rust의 타입 안정성: 기본 타입 추론
```rust
fn main() {
    let my_num = 1; // 타입 명시 안 하면 기본값 i32
}
```
- Rust는 서로 다른 정수 타입 간의 연산을 허용하지 않음
```rust
fn main() {
    let your_num: u16 = 1;
    let your_other_num: u8 = 2;
    let your_third_num = your_num + your_other_num;
    // 컴파일 에러: u16과 u8은 직접 연산 불가
}
```
- 컴파일러는 하나의 변수에 대해 일관된 타입 하나만 추론
```rust
fn main() {
    let my_num = 1; // 타입 미정
    let my_other_num: u8 = 2; // u8 명시
    let my_third_num = my_num + my_other_num;
    // my_num이 u8로 추론됨
    // 만약 100 + 200이면 별개로 error: overflow 발생 (u8)
}
```
```rust
fn main() {
    let a = 1;
    let b: u8 = 2;
    let c: u16 = 3;
    let d = a + b;
    let e = a + c;
    // 컴파일 에러: a가 u8이면서 동시에 u16일 수 없음
}
```