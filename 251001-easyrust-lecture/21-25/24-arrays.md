# 컬렉션 Collection
- 여러 개의 값을 하나의 변수에 모아두는 것
# 배열 Array
- 배열은 기본적이고 빠른 컬렉션 타입
- 다른 컬렉션(Vec, HashMap 등)에 비해 기능은 적다.
- 크기를 바꿀 수 없음 (고정 크기)
- 빠름 (메모리에 연속적으로 저장되기 때문)
- 같은 타입의 데이터만 저장 가능
- [type; number] 형식으로 표현
- ["One", "Two"]의 타입은 [&str; 2]
- 크기가 다르면 다른 타입으로 간주
```rust
let array1 = ["One", "Two"];     // 타입: [&str; 2]
let array2 = ["One", "Two", "Five"]; // 타입: [&str; 3]
```
- 타입 알아내기 - 틀린 코드를 쓰면 타입 정보를 알려준다.
```rust
fn main() {
    let seasons = ["Spring", "Summer", "Autumn", "Winter"];
    let seasons2 = ["Spring", "Summer", "Fall", "Autumn", "Winter"];
    seasons.ddd(); // 잘못된 메서드
    seasons2.thd(); // 잘못된 메서드
}
// no method named `ddd` found for array `[&str; 4]`
// no method named `thd` found for array `[&str; 5]`
```
- 같은 값으로 배열 만들기
```rust
let my_array = ["a"; 10];
// ["a", "a", "a", "a", "a", "a", "a", "a", "a", "a"]
let mut buffer = [0; 640]; // 640개의 0으로 초기화
```
- 배열 값에 접근하기 (인덱싱)
```rust
let my_numbers = [0, 10, -20];
println!("{}", my_numbers[1]); // 10 출력
```
- 배열의 일부분 가져오기 (슬라이스 Slice)
```rust
// 배열의 일부분만 참조(&array[start..end]) 형태로 가져올 수 있다.
// 슬라이스(slice)라고 한다.
let array_of_ten = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let three_to_five = &array_of_ten[2..5]; // 3, 4, 5
let start_at_two = &array_of_ten[1..];   // 2부터 끝까지
let end_at_five = &array_of_ten[..5];    // 처음부터 5번째 전까지
let everything = &array_of_ten[..];      // 전체
// Three to five: [3, 4, 5]
// start at two: [2, 3, 4, 5, 6, 7, 8, 9, 10]
// end at five: [1, 2, 3, 4, 5]
// everything: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
- 인덱스와 범위 규칙
```
- [0]: 첫 번째 요소
- [1]: 두 번째 요소
- [0..2]: 0번째와 1번째 (2는 포함 안 함)
- [0..=2]: 0, 1, 2번째 (2 포함)
- ..: 마지막 숫자는 포함 안 됨
// Index ranges are exclusive (they do not include the last number)
- ..=: 마지막 숫자 포함됨
```