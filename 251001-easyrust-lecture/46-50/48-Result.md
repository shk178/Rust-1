## Option
- 값의 존재 여부를 처리
- Some(value): 값이 존재하는 경우
- None: 값이 존재하지 않는 경우
- 인덱스 누락 등 값이 존재하지 않을 수 있는 방법이 하나뿐인 경우
- 범위를 벗어난 배열 인덱스에 액세스하는 경우처럼 실패하는 단일하고 명확한 방법이 있는 함수에 사용
## Result
- 성공하거나 실패할 수 있는 작업을 처리하고 구체적인 오류 세부 정보를 전달
- Ok(value): 성공한 작업
- Err(error): 실패한 작업
- 특정 오류 정보를 포함할 수 있으므로 다양한 이유로 실패할 수 있는 경우
- 연결 오류, 시간 초과 또는 권한 문제로 인해 실패할 수 있는 네트워크 요청 등 여러 가지 실패 모드가 있는 작업에 사용
- 성공 값과 오류 값을 여러 유형으로 저장할 수 있다.
- 예를 들어 `Result<u64, String>`의 성공 값은 u64, 오류 값은 String이다.
```rust
// Option → Result → Option
let number = Some(42);
let result = number.ok_or("에러"); // result는 Ok(42)
let option_again = result.ok(); // option_again은 Some(42)
```
## 유닛 타입
- 아무 값도 없음을 의미