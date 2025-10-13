```rust
// struct = and
// enum = or
// enumeration (e = from)
enum ThingsInTheSky {
    Sun, // 0
    Stars // 1
}

fn create_skystate(time: i32) -> ThingsInTheSky {
    match time {
        6..=18 => ThingsInTheSky::Sun,
        _ => ThingsInTheSky::Stars
    }
}

fn check_skystate(state: &ThingsInTheSky) {
    match state {
        ThingsInTheSky::Sun => println!("I can see the sun"),
        ThingsInTheSky::Stars => println!("I can see the stars")
    }
}

fn main() {
    let time = 8;
    let sky_state = create_skystate(time);
    check_skystate(&sky_state); // I can see the sun
    check_skystate(&create_skystate(20)); // I can see the stars
}
```