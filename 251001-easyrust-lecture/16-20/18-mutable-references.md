- & = imutable reference / shared reference
- &mut = mutable reference / unique reference
- &mut &mut = **로 역참조
# 참조(&)와 역참조(*)
- &는 참조(reference)를 만든다는 뜻
- 값을 직접 복사하지 않고, 그 값이 저장된 위치를 가리키는 포인터(주소)를 만든다.
```rust
let x = 8; // x → i32 타입
let ref_x = &x; // ref_x → &i32 타입 (i32에 대한 참조)
```
- x 자체는 이미 메모리에 주소를 가진다.
- &x는 그 주소값을 다른 변수(ref_x)에 저장한 것이다.
- Rust가 &x를 만들면:
- x가 어디에 저장되어 있는지를 나타내는 주소값을 하나 생성하고,
- 그 주소를 ref_x라는 변수 안에 넣는다.
- ref_x는 x의 위치를 가리키는 포인터 역할을 한다.
- Rust는 포인터 대신 참조라고 부른다.
- 참조를 통해 값을 읽으려면 *(역참조 연산자)를 써야 한다.
```rust
println!("{}", *ref_x); // ref_x가 가리키는 값(8)을 가져옴
// &: 주소를 가리키는 (주소값을 담은) 참조값을 만든다.
// *: 참조가 담고 있는 주소로 가서 실제 값을 가져온다.
```
# 가변 참조(&mut)
- 참조를 통해 값을 바꾸려면 가변 참조를 쓴다.
```rust
let mut my_number = 8;        // 원본 변수를 mut로 선언해야 함
let num_ref = &mut my_number; // 가변 참조 생성
*num_ref += 10;               // 역참조해서 값 변경
println!("{}", my_number);    // 18 출력
// num_ref는 &mut i32 (i32에 대한 가변 참조)
```
# 가변 참조 규칙
- 규칙 1: 불변 참조는 여러 개를 동시에 가질 수 있다.
- 규칙 2: 가변 참조는 단 하나만 가질 수 있다.
- 규칙 3: 가변 참조와 불변 참조를 동시에 사용할 수 없다.
- 데이터 경쟁(Data race)을 막기 위함이다.
```rust
let mut number = 10;
let number_ref = &number;     // immutable 참조
let number_change = &mut number; // mutable 참조
*number_change += 10;
println!("{}", number_ref);   // 컴파일 에러
// 읽는 중(number_ref)인데 수정하려고(number_change)하기 때문
```
- Rust 컴파일러는 참조의 사용 시점을 분석해서, 충돌이 없으면 컴파일을 허용한다.
```rust
let mut number = 10;
let number_change = &mut number;
*number_change += 10;  // 수정 완료
let number_ref = &number; // 이제 읽기용 참조 생성
println!("{}", number_ref); // 20 출력
// 컴파일러가 분석(borrow checker)해서 안전하다고 판단
```
# Shadowing과 참조
- Shadowing은 기존 변수를 덮어쓰는 게 아니라
- 새 변수를 만들어 가리는 것이다.
- 기존 변수는 파괴되지 않는다.
- 이전 값을 가리키는 참조는 여전히 유효하다.
```rust
let country = String::from("Austria");
let country_ref = &country;
let country = 8;
println!("{}, {}", country_ref, country); // Austria, 8
```