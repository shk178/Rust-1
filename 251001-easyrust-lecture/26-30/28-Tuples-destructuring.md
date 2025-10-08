# 튜플
- 컬렉션 타입 중 하나다.
- ()로 표현된다.
- 함수가 아무것도 반환하지 않으면 빈 튜플 ()이 반환된다.
```rust
fn do_something() {} // fn do_something() -> () {}와 같다.
fn just_prints() {
    println!("I am printing"); // 세미콜론(;)이 있으면 빈 튜플 반환
}
fn just_prints() {
    println!("I am printing") // 세미콜론 없음 → println!의 반환값을 반환
} // println! 매크로 자체가 ()(빈 튜플)을 반환
fn just_prints() -> String {
    println!("I am printing") // 에러
} // println!은 ()을 반환하므로, 반환값이 String이면 사용할 수 없다.
```
- 여러 개의 값을 튜플에 담을 수 있다.
- 서로 다른 데이터 타입을 튜플에 함께 저장할 수 있다.
- 각 항목은 0, 1, 2... 번호로 인덱싱된다.
- 항목에 접근할 때 [] 대신 .을 사용한다.
```rust
fn main() {
    let random_tuple = ("Here is a name", 8, vec!['a'], 'b', [8, 9, 10], 7.7);
    println!(
        "Inside the tuple is: First item: {:?}
Second item: {:?}
Third item: {:?}
Fourth item: {:?}
Fifth item: {:?}
Sixth item: {:?}",
        random_tuple.0,    // 첫 번째: 문자열 "Here is a name"
        random_tuple.1,    // 두 번째: 숫자 8
        random_tuple.2,    // 세 번째: 벡터 ['a']
        random_tuple.3,    // 네 번째: 문자 'b'
        random_tuple.4,    // 다섯 번째: 배열 [8, 9, 10]
        random_tuple.5,    // 여섯 번째: 소수 7.7
    )
}
// 튜플에 6가지 서로 다른 타입의 값을 저장
// 타입 (&str, i32, Vec<char>, char, [i32; 3], f64)
// 각 항목에 점(.)과 숫자로 접근
/*
Inside the tuple is: First item: "Here is a name"
Second item: 8
Third item: ['a']
Fourth item: 'b'
Fifth item: [8, 9, 10]
Sixth item: 7.7
 */
```
# 구조 해제 (Destructing)
- 튜플의 값들을 꺼내 여러 개의 별도 변수로 만든다.
```rust
fn main() {
    let str_vec = vec!["one", "two", "three"];
    let (a, b, c) = (str_vec[0], str_vec[1], str_vec[2]); 
    // 튜플을 풀어서 a, b, c 변수로 만듦
    println!("{:?}", b);  // "two" 출력 (디버그 포맷은 문자열 따옴표 출력)
}
```
- 안 쓰는 값은 _로 표시하면 변수로 안 만든다.
```rust
fn main() {
    let str_vec = vec!["one", "two", "three"];
    let (_, _, variable) = (str_vec[0], str_vec[1], str_vec[2]);
    // 첫 번째와 두 번째는 _로 무시
    // 세 번째만 variable이라는 변수로 만듦
}
```
# array 구조 해제
- 벡터는 동적이므로, 먼저 각 항목을 튜플로 만든다: (vec[0], vec[1], vec[2])
- 배열은 크기가 고정이므로, 배열 자체를 직접 구조 해제할 수 있다: [a, b, c]
```rust
fn main() {
    let array = ["one", "two", "three"];
    let (a, b, c) = (array[0], array[1], array[2]);
    println!("{:?}", b);  // "two"
}
```
```rust
fn main() {
    let array = ["one", "two", "three"];
    let [a, b, c] = array;  // 배열을 직접 구조 해제
    println!("{:?}", b);  // "two"
}
```
# 튜플 메모리 저장
- 각 데이터 타입은 자신의 크기만큼의 정렬 기준(alignment)을 가진다.
```
i8, bool: 1byte 정렬
i16: 2byte 정렬
i32, f32: 4byte 정렬
i64, f64: 8byte 정렬
```
- 각 필드는 자신의 정렬 기준을 만족해야 한다.
- 예를 들어 i32는 4byte 정렬이므로, 주소가 4의 배수(0, 4, 8, 12...)에 와야 한다.
- 튜플 전체의 정렬 기준은 내부 필드 중 가장 큰 정렬 기준이다.
- 예: (i8, i32, bool) → 가장 큰 정렬 기준은 i32의 4byte → 전체 튜플도 4byte 정렬
- (i8, i32, bool)
```
i8:    1byte  (주소 0)
[패딩: 3bytes] // i8 뒤에 패딩 3bytes: i32를 주소 4에 놓기 위함
i32:   4bytes (주소 4)
bool:  1byte  (주소 8)
[패딩: 3bytes] // bool 뒤에 패딩 3bytes: 배열에 여러 튜플이 있을 때 다음 튜플이 주소 12에서 시작되도록 (12는 4의 배수)
───────────────────
총 12bytes
```
- (i8, i8, i32)
```
i8:    1byte  (주소 0)
i8:    1byte  (주소 1) // i8 두 개는 1byte 정렬이므로 연속으로 올 수 있음
[패딩: 2bytes] // i32 앞에 패딩 2bytes: i32를 주소 4에 놓기 위함
i32:   4bytes (주소 4) // 튜플 끝이 8(4의 배수)이므로 추가 패딩 없음
───────────────────
총 8bytes
```
- (i8, i64, bool)
```
i8:    1byte  (주소 0)
[패딩: 7bytes] // i8 뒤에 패딩 7bytes: i64를 주소 8에 놓기 위함
i64:   8bytes (주소 8)
bool:  1byte  (주소 16)
[패딩: 7bytes] // bool 뒤에 패딩 7bytes: 다음 튜플이 주소 24에서 시작되도록 (24는 8의 배수)
───────────────────
총 24bytes
```
- 각 필드는 자신의 크기만큼 정렬되어야 함 (i32는 4의 배수 주소에)
- 필드 사이의 패딩: 다음 필드를 정렬 기준에 맞추기 위함
- 튜플 전체의 정렬 기준: 가장 큰 필드의 정렬 기준
- 튜플 끝의 패딩: 배열이나 구조체 안에서 다음 요소를 정렬하기 위함
- 메모리를 효율적으로 사용하면서도 CPU가 빠르게 접근할 수 있도록 정렬한다.
# 메모리 정보 함수
- size_of: 타입이 차지하는 메모리 크기(바이트)를 반환
```rust
use std::mem::size_of;
fn main() {
    println!("{}", size_of::<i32>());  // 4
    println!("{}", size_of::<i64>());  // 8
    println!("{}", size_of::<bool>());  // 1
    println!("{}", size_of::<(i8, i32, bool)>());  // 12
}
```
- align_of: 타입의 정렬 기준(alignment)을 반환
```rust
use std::mem::align_of;
fn main() {
    println!("{}", align_of::<i32>());  // 4
    println!("{}", align_of::<i64>());  // 8
    println!("{}", align_of::<bool>());  // 1
    println!("{}", align_of::<(i8, i32, bool)>());  // 4
}
```
- size_of_val과 align_of_val
- 타입이 아닌 값을 기반으로 크기와 정렬 기준을 알아본다.
```rust
use std::mem::{size_of, align_of};
fn main() {
    let t = (8i8, 100i32, true);
    println!("크기: {}", size_of_val(&t));     // 12
    println!("정렬 기준: {}", align_of_val(&t)); // 4
}
```
- size_of::<T>()는 타입을 사용
```rust
size_of::<i32>()  // i32 타입 자체
size_of::<bool>()  // bool 타입 자체
// 타입만 알면 되므로, 값이 필요 없다.
```
- size_of_val(&value)는 값을 사용
```rust
let x = 42i32;
size_of_val(&x)  // 변수 x의 값을 보고 크기를 알아봄
let y = true;
size_of_val(&y)  // 변수 y의 값을 보고 크기를 알아봄
// 타입을 모를 때 쓴다.
```
- 예시
```rust
use std::mem::{size_of, size_of_val};
fn main() {
    let num = 100i32;
    // 타입으로 알아보기
    println!("{}", size_of::<i32>());      // 4
    // 값으로 알아보기
    println!("{}", size_of_val(&num));     // 4
}
```