- unit struct
```rust
struct FileDirectory; // unit struct, 크기는 0
fn takes_file_directory(input: FileDirectory) { // warning: unused variable: `input`
    println!("I got a file directory");
}
fn main() {
    let x = FileDirectory;
    takes_file_directory(x); // 여기서 x의 소유권이 함수로 이동됨
    println!("{}", std::mem::size_of_val(&x)); // 에러: x는 이미 이동됨
}
```
- 해결 1: Copy 트레이트를 구현하면 x가 함수에 전달될 때 복사됨
```rust
#[derive(Copy, Clone)]
struct FileDirectory;

fn takes_file_directory(input: FileDirectory) {
    println!("I got a file directory");
}

fn main() {
    let x = FileDirectory;
    takes_file_directory(x); // 복사됨
    println!("{}", std::mem::size_of_val(&x)); // OK
}
/*
I got a file directory
0
 */
```
- 해결 2: 참조만 전달하도록 함수 매개변수, 인자 변경
```rust
struct FileDirectory;

fn takes_file_directory(input: &FileDirectory) {
    println!("I got a file directory");
}

fn main() {
    let x = FileDirectory;
    takes_file_directory(&x); // 참조 전달
    println!("{}", std::mem::size_of_val(&x)); // OK
}
/*
I got a file directory
0
 */
```
- tuple struct
```rust
struct Colour(u8, u8, u8);

fn main() {
    let my_colour = Colour(20, 50, 100);
    println!("{:?} {:?} {:?}", my_colour.0, my_colour.1, my_colour.2); // 20 50 100
}
```
```rust
struct Colour(u8, u8, u8);

fn main() {
    let my_colour = Colour(20, 50, 100);
    println!("{:?}", my_colour); // error[E0277]: `Colour` doesn't implement `Debug`
}
```
```rust
#[derive(Debug)] // attribute
struct Colour(u8, u8, u8); // warning: fields `0`, `1`, and `2` are never read

fn main() {
    let my_colour = Colour(20, 50, 100);
    println!("{:?}", my_colour); // Colour(20, 50, 100)
}
```
- named struct
```rust
struct Country {
    population: u32,
    capital: String,
    leader_name: String
}

fn main() {
    let canada = Country {
        population: 41_290_000,
        capital: "Ottawa".to_string(),
        leader_name: "Mark Carney".to_string()
    };
    println!("{}", canada.population);
    println!("{}", canada.capital);
    println!("{}", canada.leader_name);
}
/*
41290000
Ottawa
Mark Carney
 */
```
```rust
#[derive(Debug)]
struct Country {
    population: u32, // warning: fields `population`, `capital`, and `leader_name` are never read
    capital: String,
    leader_name: String
}

fn main() {
    let canada = Country {
        population: 41_290_000,
        capital: "Ottawa".to_string(),
        leader_name: "Mark Carney".to_string()
    };
    println!("{:?}", canada); // Country { population: 41290000, capital: "Ottawa", leader_name: "Mark Carney" }
}
```