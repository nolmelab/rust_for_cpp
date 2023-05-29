# 기본 타잎들과 연산자

러스트는 C++과 거의 동일한 산술 및 논리 연산자를 가지고 있습니다. bool은 양 언어에서 
동일하며 (true와 false 리터럴 또한 동일합니다). 러스트는 정수, 부호 없는 정수 및 부동 
소수점과 유사한 개념을 가지고 있습니다. 그러나 구문은 약간 다릅니다. 

러스트는 isize를 정수를 의미하는 것으로 사용하며, usize는 부호 없는 정수를 의미합니다. 
이러한 유형은 포인터 크기를 갖습니다. 예를 들어, 32비트 시스템에서 usize는 32비트 부호 
없는 정수를 의미합니다. 

러스트는 또한 명시적으로 크기가 지정된 유형을 가지고 있는데, 이는 u 또는 i 뒤에 8, 16, 32, 
64 또는 128이 옵니다. 예를 들어, u8은 8비트 부호 없는 정수이고, i32는 32비트 부호 있는 
정수입니다. 부동 소수점의 경우, 러스트는 f32와 f64를 사용합니다.

숫자 리터럴은 유형을 나타내기 위해 접미사를 가질 수 있습니다. 접미사가 없는 경우, 러스트는 
유형을 추론하려고 시도합니다. 추론할 수 없는 경우, i32나 f64를 사용합니다 
(소수점이 있는 경우 f64를 사용합니다).

예시들:

```rust
fn main() {
    let x: bool = true;
    let x = 34;   // 타입은 i32로 추론됩니다
    // let x = 2147483648; // 오류: `i32` 범위를 벗어난 리터럴입니다
    let x = 34isize;
    let x = 34usize;
    let x = 34u8;
    let x = 34i64;
    let x = 34f32;
}
```

부연 설명으로, 러스트는 변수를 재정의할 수 있기 때문에 위의 코드는 유효합니다. 각 let 문은
새로운 변수 x를 생성하고 이전 변수를 숨깁니다. 이는 변수가 기본적으로 불변성을 가지기 
때문에 예상 이상으로 유용합니다.

숫자 리터럴은 2진수, 8진수, 16진수뿐만 아니라 10진수로도 표현할 수 있습니다. 각각에는 
0b, 0o, 0x 접두사를 사용합니다. 숫자 리터럴 내에는 언더스코어(_)를 어디에든 사용할 수 
있으며, 무시됩니다. 

예시:

```rust
fn main() {
    let x = 12;                  // 10진수 (decimal)
    let x = 0b1100;              // 2진수 (binary)
    let x = 0o14;                // 8진수 (octal)
    let x = 0xe;                 // 16진수 (hexadecimal)
    let y = 0b_1100_0011_1011_0001; // 언더스코어를 사용한 2진수 (binary)
}
```

위의 코드는 숫자 리터럴을 사용하여 변수를 초기화하고 있습니다. 10진수, 2진수, 8진수, 
16진수의 예시를 보여주고 있으며, 언더스코어를 사용한 숫자 리터럴도 포함되어 있습니다.

러스트는 문자(char)와 문자열(string)을 가지고 있지만, 이들은 유니코드를 지원하기 때문에 
C++과 약간 다릅니다. 포인터, 참조, 그리고 벡터(배열)에 대한 소개 이후에 다시 이에 대해
이야기할 예정입니다.

Rust does not implicitly coerce numeric types. In general, Rust has much less
implicit coercion and subtyping than C++. Rust uses the `as` keyword for
explicit coercions and casting. Any numeric value can be cast to another numeric
type. `as` cannot be used to convert from numeric types to boolean types, but
the reverse can be done. E.g.,

```rust
fn main() {
    let x = 34usize as isize;   // cast usize to isize
    let x = 10 as f32;      // isize to float
    let x = 10.45f64 as i8; // float to i8 (loses precision)
    let x = 4u8 as u64;     // gains precision
    let x = 400u16 as u8;   // 144, loses precision (and thus changes the value)
    println!("`400u16 as u8` gives {}", x);
    let x = -3i8 as u8;     // 253, signed to unsigned (changes sign)
    println!("`-3i8 as u8` gives {}", x);
    //let x = 45 as bool;  // FAILS! (use 45 != 0 instead)
    let x = true as usize;  // cast bool to usize (gives a 1)
}
```

러스트는 숫자형 타입 간에 암묵적인 자동 형 변환을 수행하지 않습니다. 일반적으로, C++에 
비해 러스트는 암묵적인 형 변환과 하위 타입(subtyping)을 훨씬 적게 가지고 있습니다. 
러스트는 명시적인 형 변환과 캐스팅을 위해 as 키워드를 사용합니다. 모든 숫자 값은 다른 
숫자형 타입으로 캐스팅될 수 있습니다. as는 숫자형 타입에서 부울 타입으로의 변환에는 사용할
수 없지만, 그 반대는 가능합니다. 예시로,

```rust
fn main() {
    let x = 34usize as isize;   // usize를 isize로 캐스팅
    let x = 10 as f32;      // isize를 부동 소수점(float)으로 캐스팅
    let x = 10.45f64 as i8; // 부동 소수점(float)를 i8로 캐스팅 (정밀도 손실)
    let x = 4u8 as u64;     // 정밀도 증가
    let x = 400u16 as u8;   // 144, 정밀도 손실 (따라서 값이 변경됨)
    println!("`400u16 as u8` 결과: {}", x);
    let x = -3i8 as u8;     // 253, 부호 있는 정수를 부호 없는 정수로 캐스팅 (부호 변경)
    println!("`-3i8 as u8` 결과: {}", x);
    //let x = 45 as bool;  // 실패! (대신 45 != 0을 사용하세요)
    let x = true as usize;  // 부울(bool)을 usize로 캐스팅 (1을 반환)
}
```

위의 코드에서는 as 키워드를 사용하여 숫자 타입들을 서로 변환하고 있습니다. 주석 처리된 
부분은 주석이 제거되어야 하며, 해당 부분은 실패합니다.

러스트에는 다음과 같은 연산자들이 있습니다:

| 타입	            | 연산자                    |
|-------------------|---------------------------|
| 숫자	            | +, -, *, /, %             |
| 비트	            |, &, ^, <<, >>             |
| 비교	            | ==, !=, >, <, >=, <=      |
| 단축 회로 논리    | 	||, &&                  |

이 모든 연산자들은 C++과 동일한 방식으로 작동하지만, 러스트는 연산자를 적용할 수 있는 
타입에 대해 약간 더 엄격합니다. 비트 연산자는 정수에만 적용할 수 있으며, 논리 연산자는 
부울 타입에만 적용할 수 있습니다. 러스트에는 - 단항 연산자가 있으며, 숫자의 부호를 
바꿉니다. ! 연산자는 부울 값을 부정하고, 정수 타입의 모든 비트를 반전시킵니다 (후자의 경우 
C++의 ~와 동일합니다). 러스트에는 C++과 유사한 복합 할당 연산자인 +=가 있지만, 증가나 감소
연산자 (예: ++)는 없습니다.

