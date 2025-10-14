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
impl AnimalType {
    fn check_type(&self) {
        use AnimalType::*;
        match self {
            Cat => println!("Cat2"),
            Dog => println!("Dog2"),
        }
    }
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
    fn check_type(&self) {
        use AnimalType::*;
        match self.a_type {
            Cat => println!("Cat"),
            Dog => println!("Dog"),
        };
    }
}
fn main() {
    use AnimalType::*;
    let my_cat = Animal::new(10, Cat);
    let my_dog = Animal::new(11, Dog);
    my_cat.check_type(); // Cat
    my_dog.check_type(); // Dog
    my_cat.a_type.check_type(); // Cat2
    my_dog.a_type.check_type(); // Dog2
}
```
```rust
#[derive(Debug)]
struct Animal {
    age: u8,
    a_type: AnimalType
}
#[derive(Debug)]
enum AnimalType {
    Cat(String),
    Dog(String)
}
impl AnimalType {
    fn get_name(&self) -> &String {
        match self {
            AnimalType::Cat(name) => name,
            AnimalType::Dog(name) => name,
        }
    }
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
}
fn main() {
    use AnimalType::*;
    let my_cat = Animal::new(10, Cat("kitty".to_string()));
    let my_dog = Animal::new(11, Dog("puppy".to_string()));
    my_cat.print(); // Animal { age: 10, a_type: Cat("kitty") }
    my_dog.print(); // Animal { age: 11, a_type: Dog("puppy") }
    println!("{}", my_cat.a_type.get_name()); // kitty
    println!("{}", my_dog.a_type.get_name()); // puppy
}
```