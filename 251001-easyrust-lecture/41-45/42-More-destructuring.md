```rust
struct Person {
    name: String,
    real_name: String,
    height: u8,
    happiness: bool
}
fn main() {
    let papa_doc = Person {
        name: "Papa Doc".to_string(),
        real_name: "Clarence".to_string(),
        height: 170,
        happiness: false
    };
    print!("They call him {}", papa_doc.name);
    println!(" but his real name is {}.", papa_doc.real_name);
    // They call him Papa Doc but his real name is Clarence.
    print!("He is {}cm tall", papa_doc.height);
    println!(" and is he happy? {}", papa_doc.happiness);
    // He is 170cm tall and is he happy? false
    let Person {name, real_name, height, happiness} = papa_doc; // DESTRUCTURING (소유권 이동됨)
    print!("They call him {}", name);
    println!(" but his real name is {}.", real_name);
    // They call him Papa Doc but his real name is Clarence.
    print!("He is {}cm tall", height);
    println!(" and is he happy? {}", happiness);
    // He is 170cm tall and is he happy? false
    let papa_doc = Person {
        name: "Papa Doc".to_string(),
        real_name: "Clarence".to_string(),
        height: 170,
        happiness: true
    };
    let Person {name: a, real_name: b, height: c, happiness: d} = papa_doc; // DESTRUCTURING (소유권 이동됨)
    print!("They call him {}", a);
    println!(" but his real name is {}.", b);
    // They call him Papa Doc but his real name is Clarence.
    print!("He is {}cm tall", c);
    println!(" and is he happy? {}", d);
    // He is 170cm tall and is he happy? true
}
```
```rust
struct Person {
    name: String,
    real_name: String,
    height: u8,
    happiness: bool,
}
#[derive(Debug)]
struct Person2 {
    name: String,
    height: u8
}
impl Person2 {
    fn from_person(input: Person) -> Self {
        let Person { name, height, .. } = input;
        Self {
            name,
            height
        }
    }
}
fn main() {
    let papa_doc = Person {
        name: "Papa Doc".to_string(),
        real_name: "Clarence".to_string(),
        height: 170,
        happiness: true
    };
    let person2 = Person2::from_person(papa_doc);
    println!("{:?}", person2) // Person2 { name: "Papa Doc", height: 170 }
}
```