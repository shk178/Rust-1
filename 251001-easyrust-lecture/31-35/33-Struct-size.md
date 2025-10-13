```rust
#[derive(Debug)]
struct Date {
    year: u16,
    month: u8,
    day: u8
}

fn main() {
    let year = 2025;
    let month = 10;
    let day = 13;
    let today = Date {
        year,
        month,
        day
    }; // [Clippy] year: year -> year
    println!("{} {} {}", size_of_val(&year), size_of_val(&month), size_of_val(&day)); // 2 1 1
    println!("{} {} {}", size_of_val(&today.year), size_of_val(&today.month), size_of_val(&today.day)); // 2 1 1
    println!("{}", size_of_val(&today)); // 4
}
```
- 모든 필드가 u8이므로 각 필드의 크기 = 1바이트, 정렬 기준도 = 1바이트
- 패딩이 없음: 정렬 기준이 모두 동일하고, 구조체 전체의 정렬 기준도 u8이기 때문
```rust
struct ThreeNums {
    one: u8,
    two: u8,
    three: u8
}
struct FourNums {
    one: u8,
    two: u8,
    three: u8,
    four: u8
}

fn main() {
    let x = ThreeNums {
        one: 1,
        two: 2,
        three: 3
    };
    let y = FourNums {
        one: 1,
        two: 2,
        three: 3,
        four: 4
    };
    println!("{}", size_of_val(&x)); // 3
    println!("{}", size_of_val(&y)); // 4
}
// 확인 방법
use std::mem::{size_of, align_of};

fn main() {
    println!("ThreeNums size: {}", size_of::<ThreeNums>()); // 3
    println!("ThreeNums align: {}", align_of::<ThreeNums>()); // 1

    println!("FourNums size: {}", size_of::<FourNums>()); // 4
    println!("FourNums align: {}", align_of::<FourNums>()); // 1
}
```
- 언제 alignment 때문에 사이즈가 바뀌는가
- 패딩이 들어가는 대표적인 예
```rust
struct Mixed {
    a: u8,   // 1바이트
    b: u32,  // 4바이트
}
// a는 1바이트지만, b는 4바이트 정렬 기준을 요구
// a 다음에 3바이트 패딩이 들어가서 b가 4바이트 경계에 맞춰짐
// 전체 크기 = 1 (a) + 3 (패딩) + 4 (b) = 8바이트
use std::mem::{size_of, align_of};

fn main() {
    println!("Mixed size: {}", size_of::<Mixed>()); // Mixed size: 8
    println!("Mixed align: {}", align_of::<Mixed>()); // Mixed align: 4
}
```