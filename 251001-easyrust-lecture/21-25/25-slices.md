- println! 매크로는 값의 표시 형식(format)을 {} 안에 써서 지정
- {} = Display 형식 (일반적인 출력)
- {:?} = Debug 형식 (배열, 슬라이스, 구조체 등 내부 내용 출력)
- {:#?} = pretty print (줄바꿈과 들여쓰기 출력)
```rust
// {}로는 배열(혹은 슬라이스)을 출력할 수 없다.
fn main() {
    let number = 10;
    let array = [1, 2, 3];
    println!("number: {}", number); // number: 10
    println!("array: {}", array); // error[E0277]: `[{integer}; 3]` doesn't implement `std::fmt::Display`
}
```
```rust
fn main() {
    let array_of_ten = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    let three_to_five = &array_of_ten[2..5];
    let start_at_two = &array_of_ten[1..];
    let end_at_five = &array_of_ten[..5];
    let everything = &array_of_ten[..];
    println!("Three to five: {:?}", three_to_five); // Three to five: [3, 4, 5]
    println!("start at two: {:?}", start_at_two); // start at two: [2, 3, 4, 5, 6, 7, 8, 9, 10]
    println!("end at five: {:?}", end_at_five); // end at five: [1, 2, 3, 4, 5]
    println!("everything: {:?}", everything); // everything: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
}
```
```rust
// {:#?}
/*
Three to five: [
    3,
    4,
    5,
]
start at two: [
    2,
    3,
    4,
    5,
    6,
    7,
    8,
    9,
    10,
]
end at five: [
    1,
    2,
    3,
    4,
    5,
]
everything: [
    1,
    2,
    3,
    4,
    5,
    6,
    7,
    8,
    9,
    10,
]
 */
```