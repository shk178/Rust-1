```rust
fn take_fifth(value: Vec<i32>) -> i32 {
    value[4]
}
fn main() {
    let new_vec = vec![1, 2];
    //let index = take_fifth(new_vec);
    //thread 'main' panicked at 'index out of bounds: the len is 2 but the index is 4'
}
```
- 패닉은 문제가 발생하기 전에 프로그램이 멈춘다는 의미
- Rust는 함수가 불가능한 것을 원한다는 것을 알아차리고 멈춘다.
- 스택에서 값을 꺼내서 그렇게 할 수 없다고 알려준다.
```rust
fn take_fifth(value: Vec<i32>) -> Option<i32> {
    if value.len() < 5 {
        None
    } else {
        Some(value[4])
    }
}
fn main() {
    let new_vec = vec![1, 2];
    let bigger_vec = vec![1, 2, 3, 4, 5];
    println!("{:?}, {:?}", take_fifth(new_vec), take_fifth(bigger_vec));
    // None, Some(5)를 출력
    /*
    println!("{:?}, {:?}",
        take_fifth(new_vec).unwrap(), // None.unwrap()은 패닉을 일으킨다.
        take_fifth(bigger_vec).unwrap()
    );
     */
}
```
```rust
fn take_fifth(value: Vec<i32>) -> Option<i32> {
    if value.len() < 5 {
        None
    } else {
        Some(value[4])
    }
}
fn handle_option(my_option: Vec<Option<i32>>) {
  for item in my_option {
    match item {
      Some(number) => println!("Found a {}!", number),
      None => println!("Found a None!"),
    }
  }
}
fn main() {
    let new_vec = vec![1, 2];
    let bigger_vec = vec![1, 2, 3, 4, 5];
    let mut option_vec = Vec::new(); // Vec<Option<i32>> 생성
    option_vec.push(take_fifth(new_vec)); // vec에 None을 푸시
    option_vec.push(take_fifth(bigger_vec)); // vec에 Some(5)를 푸시
    handle_option(option_vec);
    //Found a None!
    //Found a 5!
}
```
```rust
fn take_fifth(value: Vec<i32>) -> Option<i32> {
    if value.len() < 5 {
        None
    } else {
        Some(value[4])
    }
}
fn main() {
    let new_vec = vec![1, 2];
    let bigger_vec = vec![1, 2, 3, 4, 5];
    let vec_of_vecs = vec![new_vec, bigger_vec];
    for vec in vec_of_vecs {
        let inside_number = take_fifth(vec);
        if inside_number.is_some() {
            println!("We got: {}", inside_number.unwrap());
        } else {
            println!("We got nothing.");
        }
    }
    //We got nothing.
    //We got: 5
}
```
```rust
enum Option<T> {
    None,
    Some(T),
}
enum Result<T, E> {
    Ok(T),
    Err(E),
}
fn main() {}
```
```rust
fn give_result(input: i32) -> Result<(), ()> {
    if input % 2 == 0 {
        return Ok(())
    } else {
        return Err(())
    }
}
fn main() {
    if give_result(5).is_ok() {
        println!("It's okay, guys")
    } else {
        println!("It's an error, guys")
    }
    // It's an error, guys
}
```
```rust
fn main() {
    let my_vec = vec![2, 3, 4];
    let get_one = my_vec.get(0);
    let get_two = my_vec.get(10);
    println!("{:?}", get_one); // Some(2)
    println!("{:?}", get_two); // None
}
```
- if let은 일치하면 무언가를 하고, 그렇지 않으면 아무것도 하지 않는다는 의미
- if let은 모든 것에 대해 매칭할 필요가 없을 때 사용
```rust
fn main() {
    let my_vec = vec![2, 3, 4];
    for index in 0..10 {
      if let Some(number) = my_vec.get(index) {
        println!("The number is: {}", number);
      }
    }
    //The number is: 2
    //The number is: 3
    //The number is: 4
}
// if let Some(number) = my_vec.get(index)는
// my_vec.get(index)에서 Some(number)를 얻으면을 의미
// =를 하나 사용 (불린이 아니다)
```
- while let은 if let을 위한 while 루프와 같다.
```rust
fn main() {
    let weather_vec = vec![
        vec!["Berlin", "cloudy", "5", "-7", "78"],
        vec!["Athens", "sunny", "not humid", "20", "10", "50"],
    ];
    for mut city in weather_vec {
        println!("For the city of {}:", city[0]); // 첫 번째 항목은 도시 이름
        while let Some(information) = city.pop() {
            // 더 이상 pop할 수 없을 때까지 계속
            // 벡터가 0개 항목에 도달하면 None을 반환하고 멈춘다.
            if let Ok(number) = information.parse::<i32>() {
                // information을 파싱하려고 시도
                // result를 반환: Ok(number)이면 출력
                println!("The number is: {}", number);
            }  // 에러를 얻으면 아무것도 하지 않는다.
        }
    }
    //For the city of Berlin:
    //The number is: 78
    //The number is: -7
    //The number is: 5
    //For the city of Athens:
    //The number is: 50
    //The number is: 10
    //The number is: 20
}
```