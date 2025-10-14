```rust
#[derive(Debug)]
struct Animal {
    age: u8,
    a_type: AnimalType
}
#[derive(Debug)]
enum AnimalType {
    Cat,
    Dog
}
impl Animal {
    fn new_cat(age: u8) -> Self {
        Self {
            age,
            a_type: AnimalType::Cat,
        }
    }
    fn new_dog(age: u8) -> Self {
        Self {
            age,
            a_type: AnimalType::Dog,
        }
    }
    fn print(&self) { // 코드에서 self 바꾸려면 (&mut self)
        println!("{:?}", self);
    }
}
fn main() {
    let my_cat = Animal::new_cat(10);
    let my_dog = Animal::new_dog(11);
    my_cat.print(); // &self는 자동으로 가져와진다.
    // Animal { age: 10, a_type: Cat }
    my_dog.print(); // syntactic sugar (.: dot operator)
    // Animal { age: 11, a_type: Dog }
    Animal::print(&my_cat);
    // Animal { age: 10, a_type: Cat }
    Animal::print(&my_dog);
    // Animal { age: 11, a_type: Dog }
}
```
- Syntactic sugar(문법 설탕):
- 프로그래밍 언어에서 더 간단하고 읽기 쉬운 문법을 제공하는 기능
- 실제로는 복잡한 동작을 수행하지만, 개발자가 편하게 사용할 수 있도록 문법을 만듦
```rust
my_cat.print(); // syntactic sugar
Animal::print(&my_cat); // 동일 의미
```
- Dot operator(.):
- Rust에서 구조체의 필드나 메서드에 접근할 때 사용하는 연산자
- 객체지향 언어에서 흔히 쓰이는 방식
- self를 바꾼다는 의미:
- 메서드가 인스턴스를 수정할 수 있는지 여부
```
메서드 시그니처 - 의미
&self - 읽기 전용. 값을 참조만 함. 수정 불가
&mut self - 가변 참조. 값을 수정 가능
self - 소유권 이동. 인스턴스를 소비함
```
```rust
#[derive(Debug)]
struct Animal {
    age: u8,
    a_type: AnimalType
}
#[derive(Debug)]
enum AnimalType {
    Cat,
    Dog
}
impl Animal {
    fn new(age: u8, a_type: AnimalType) -> Self {
        Self {
            age,
            a_type,
        }
    }
    fn print(&self) {
        println!("{:?}", self);
    }
    fn change_to_cat(&mut self) {
        self.a_type = AnimalType::Cat;
    }
    fn change_to_dog(&mut self) {
        self.a_type = AnimalType::Dog;
    }
}
fn main() {
    use AnimalType::*;
    // &mut self로 넘기려면 let mut로 선언
    let mut my_cat = Animal::new(10, Cat);
    let mut my_dog = Animal::new(11, Dog);
    my_cat.change_to_dog();
    my_dog.change_to_cat();
    my_cat.print(); // Animal { age: 10, a_type: Dog }
    my_dog.print(); // Animal { age: 11, a_type: Cat }
}
```