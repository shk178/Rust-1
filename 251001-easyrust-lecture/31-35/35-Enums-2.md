```rust
enum Mood {
    Happy,
    Sleepy,
    NotBad,
}

fn match_mood(mood :&Mood) -> i32 {
    let happiness_level = match mood {
        Mood::Happy => 10,
        Mood::Sleepy => 6,
        Mood::NotBad => 3
    };
    happiness_level
}

fn main() {
    let my_mood = Mood::NotBad;
    println!("{}", match_mood(&my_mood)); // 3
}
```
```rust
enum Mood {
    Happy,
    Sleepy,
    NotBad,
}

fn match_mood(mood :&Mood) -> i32 {
    use Mood::*;
    let happiness_level = match mood {
        Happy => 10,
        Sleepy => 6,
        NotBad => 3
    };
    happiness_level
}

fn main() {
    let my_mood = Mood::NotBad;
    println!("{}", match_mood(&my_mood)); // 3
}
```
```rust
enum Season {
    Spring,
    Summer,
    Autumn,
    Winter
}

fn main() {
    use Season::*;
    let four_seasons = vec![Spring, Summer, Autumn, Winter]; // Vec<Season>
    for season in four_seasons {
        println!("{}", season as u32);
        // 0
        // 1
        // 2
        // 3
    }
}
```