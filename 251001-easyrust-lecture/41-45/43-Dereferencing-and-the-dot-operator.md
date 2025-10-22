```rust
fn main() {
    let my_number = 10; // i32
    let reference = &my_number; // &i32
    //println!("{}", my_number == reference);
    //error[E0277]: can't compare `{integer}` with `&{integer}`
    println!("{}", my_number == *reference); // true
}
```
```rust
struct Item {
    number: u8
}
fn main() {
    let item = Item {
        number: 10
    };
    let reference = &item.number; // &u8
    //println!("{}", item.number == reference);
    //error[E0308]: mismatched types
    println!("{}", item.number == *reference); // true
    println!("{}", 10 == *reference); // true
    //println!("{}", 10 == reference);
    //error[E0277]: can't compare `u8` with `&u8`
}
```
```rust
struct Item {
    number: u8
}
impl Item {
    fn compare_number(&self, other_number: u8) {
        //println!("Are they equal? {}", &self.number == other_number);
        //error[E0277]: can't compare `&u8` with `u8`
        println!("Are they equal? {}", self.number == other_number); // 자동 deref coercion
        // Rust가 자동으로 deref = &T, &&T 같은 참조를 가지고 있어도 T로 바꿔서 메서드를 호출
        // 참조는 값을 빌리는 것이고, deref는 그 값을 꺼내서 쓰는 것 (소유권은 바뀌지 않음)
    }
}
fn main() {
    let item = Item {
        number: 10
    };
    let reference = &item;
    let other_reference = &reference;
    item.compare_number(10); // Are they equal? true
    reference.compare_number(10); // Are they equal? true
    other_reference.compare_number(10); // Are they equal? true
}
```