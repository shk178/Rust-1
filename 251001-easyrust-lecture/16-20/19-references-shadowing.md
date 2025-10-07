# references
- 참조의 종류가 겹치는 동안(active overlapping)에는 혼용할 수 없다.
```rust
let mut number = 10;
let number_ref = &number;
let number_change = &mut number;
// cannot borrow `number` as mutable because it is also borrowed as immutable
*number_change += 10; // immutable ref 사용
println!("{}", number_ref); // mutable ref 사용
```
- mutable ref 사용 다 하고, immutable ref를 선언해서 사용하거나
- immutable ref 사용 다 하고, mutable ref를 선언해서 사용해야 한다.
# shadowing
```rust
fn main() {
    let c = "대한민국";
    let c_ref = &c;
    let c = 8;
// c로는 대한민국 볼 수 없지만
// *c_ref로는 볼 수 있다.
// c_ref로 print!하면 자동 역참조 된다.
}
```