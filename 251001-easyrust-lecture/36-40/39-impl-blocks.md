```rust
#[derive(Debug)]
struct Animal {
    age: u8,
    a_type: AnimalType
}
//Animal을 Debug print하려면
//Animal 뿐만 아니라 AnimalType도 Debug를 구현해야 한다.
#[derive(Debug)]
enum AnimalType {
    Cat,
    Dog
}
impl Animal {
    //new는 러스트에서 키워드 아님
    //Self: Animal을 반환 (Animal이라고 써도 된다.)
    fn new() -> Self { //여기까지가 function signature
    //여기서부터 function 시작
        Self {
            age: 10,
            a_type: AnimalType::Cat
        }
    }
    fn new2(age: u8, a_type: AnimalType) -> Self {
        Self {
            age,
            a_type
        }
    }
}
fn main() {
    let my_vec = vec![7, 8];
    println!("{}", my_vec.len()); //메서드 실행 (출력: 2)
    let my_animal = Animal::new(); //associated function
    //type하고 관계된 함수라는 뜻이다.
    //아직 my_animal이 안 만들어져서, 메서드가 아님
    let my_animal_2 = Animal::new2(11, AnimalType::Dog);
    println!("{:?}", my_animal); //Animal { age: 10, a_type: Cat }
    println!("{:?}", my_animal_2); //Animal { age: 11, a_type: Dog }
}
```
- 연관 함수: 타입에 직접 연결된 함수
- `self`를 인자로 받지 않음
- 객체 없이 타입 이름으로 호출함
- 메서드: 특정 인스턴스(객체)에 대해 호출되는 함수
- `self`, `&self`, `&mut self`를 첫 번째 인자로 받음
```rust
impl Animal {
    fn get_age(&self) -> u8 {
        self.age
    }
}
let age = my_animal.get_age(); //메서드 호출
```
- impl 블록 (implementation의 줄임말)
- 구조체(struct)나 열거형(enum)에 관련된 함수나 메서드를 정의하는 공간
- 특정 타입(예: 구조체 Animal)에 대해 함수(연관 함수)나 메서드를 구현할 수 있게 해준다.
```rust
struct Animal {
    age: u8,
}
impl Animal {
    fn new(age: u8) -> Self {
        Self { age }
    }
    fn get_age(&self) -> u8 {
        self.age
    }
}
/* impl 블록 안에서 할 수 있는 것들 */
//연관 함수 정의: self 없이 타입 이름으로 호출 (Animal::new())
//메서드 정의: self, &self, &mut self를 인자로 받아 인스턴스 기반으로 호출 (animal.get_age())
//Trait 구현: impl Debug for Animal { ... }처럼 특정 trait을 구현할 수도 있다.
/* impl 블록 */
//타입에 기능을 추가할 수 있다.
//객체지향 스타일로 코드를 구성할 수 있다.
//코드 재사용과 가독성이 좋아짐
```