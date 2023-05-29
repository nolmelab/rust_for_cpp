# 흐름 제어

## If

`if` 문은 Rust에서 C++와 거의 동일합니다. 한 가지 차이점은 중괄호는 필수이지만 테스트 
중인 조건식 (조건 표현식) 주위의 괄호는 필요하지 않습니다.

또 다른 하나는 `if`가 표현식이므로 C++의 삼항 `?:` 연산자와 같은 방식으로 사용할 수 
있습니다(이전 섹션에서 블록의 마지막 표현식이 세미콜론으로 끝나지 않으면 블록의 값이 
된다고 했습니다). 따라서 다음 두 함수는 같은 일을 합니다:

```rust
fn foo(x: i32) -> &'static str {
    let result: &'static str;
    if x < 10 {
        result = "less than 10";
    } else {
        result = "10 or more";
    }
    return result;
}

fn bar(x: i32) -> &'static str {
    if x < 10 {
        "less than 10"
    } else {
        "10 or more"
    }
}
```

따라서, 러스트에서는 삼항 연산자 `?:`가 없습니다.

(왜 `mut result`가 아닌가요? `foo`의 코드는 `result`를 불변으로 만들고, 가능한 두 곳에서만 
초기화합니다. Rust는 `return result`가 나올 때쯤이면 초기화가 보장된다는 것을 알 수 
있습니다. `mut`에 대해 이미 들어본 사람들이게 설명하는 내용입니다.)

첫 번째는 C++로 작성할 수 있는 내용을 상당히 문자 그대로 번역한 것입니다. 그리고
두 번째는 더 나은 (표현식을 활용한) Rust 스타일입니다.

`let result = if x < 10 ...` 와 같이 `if` 문의 결과를 바로 지정하도록 할 수도 있습니다. 
이것도 러스트 스타일입니다.

## 반복

Rust에는 C++와 마찬가지로 `while` 루프가 있습니다:

```rust
fn main() {
    let mut x = 10;
    while x > 0 {
        println!("Current value: {}", x);
        x -= 1;
    }
}
```

Rust에는 `do...while` 루프가 없지만 `loop` 문이 있습니다.
그냥 영원히 반복합니다:

```rust
fn main() {
    loop {
        println!("Just looping");
    }
}
```

Rust에는 C++처럼 'break'와 'continue'가 있습니다.

## for 반복문

Rust에도 `for` 루프가 있지만 약간 다릅니다. 정수의 벡터가 있고
그 벡터를 모두 인쇄하고 싶다고 가정해 봅시다 (벡터/배열은 나중에 자세히 다룰 것입니다,
반복자(iterator), 제네릭(generic)은 나중에 더 자세히 다루겠습니다. 지금은
`Vec<T>`는 `T`의 시퀀스이고 `iter()`는 반복하고 싶은 어떤 것에서 반복자를 반환합니다). 
간단한 `for` 반복문은 다음처럼 보입니다:

```rust
fn print_all(all: Vec<i32>) {
    for a in all.iter() {
        println!("{}", a);
    }
}
```

all.iter() 대신 &all/all을 사용해도 됩니다. 어떤 차이가 있는지 직접 확인하면 좋은 연습
문제가 됩니다.  

If we want to index over the indices of `all` (a bit more like a standard C++
for loop over an array), you could do

'all'을 인덱스로 순환하려면(배열에 대한 표준 C++에 대한 루프와 비슷합니다), 
다음과 같이 할 수 있습니다:

```rust
fn print_all(all: Vec<i32>) {
    for i in 0..all.len() {
        println!("{}: {}", i, all[i]);
    }
}
```

`len` 함수는 이름에서 길이를 돌려주는 함수라는 점이 자명합니다. `0..10`에서 `..`은 
범위(Range)를 만드는 연산자입니다. 

열거 반복자(enumuerate iterator)를 사용하여 위 예제를 러스트 스타일로 작성하면 다음과 
같습니다:

```rust
fn print_all(all: Vec<i32>) {
    for (i, a) in all.iter().enumerate() {
        println!("{}: {}", i, a);
    }
}
```

여기서 `enumerate()`는 반복자 `iter()`에서 반복할 인덱스와 원소를 반복할 때 돌려줍니다. 

* 다음 예시에는 [차용한 포인터](borrowed.md) 섹션에서 다룬 고급 주제
차용한 포인터 섹션에서 다룬 고급 주제를 포함합니다.

* 정수 벡터가 있고 벡터를 함수에 참조로 넘긴 후 함수는 벡터를 내부에서 변경한다고 
가정합시다. 여기서  `for` 루프는 가변 반복자(mutable iterator)를 통해 가변 참조를 주고 
`*`로 C++의 포인터처럼 주소를 얻어서 변경합니다.  

```rust
fn double_all(all: &mut Vec<i32>) {
    for a in all.iter_mut() {
        *a += *a;
    }
}
```

가변/불변 (mutable/immutable), 참조는 러스트 소유권 관련한 중요 요소들입니다. 여기서는
iter_mut()로 가변 참조를 얻고, `*`로 참조에 접근해서 그 내용을 변경할 수 있다는 점이 
핵심입니다.  

## switch와 비슷한 (더 강력한) match 

러스트는 C++의 switch 문과 비슷하지만 매우 훨씬 강력한 match 표현식이 있습니다. 아래 
단순한 버전은 매우 익숙할 겁니다: 

```rust
fn print_some(x: i32) {
    match x {
        0 => println!("x is zero"),
        1 => println!("x is one"),
        10 => println!("x is ten"),
        y => println!("x is something else {}", y),
    }
}
```

몇 가지 문법 차이가 있습니다. 러스트는 `=>`를 사용하여 일치한 값에 실행할 표현식으로 
이동하고, 매치 팔(match arm)들은 `,`로 구분됩니다. 마지막 `,`는 생략할 수 있습니다. 

또한 몇 가지 의미론적 차이점도 있습니다. 일치하는 패턴은 일치하는 
표현식(위의 예제에서 `x`)의 모든 가능한 값을 포함해야 합니다. 위 예제에서는 `x`의
가능한 모든 값을 포함해야 합니다.

y => ...` 줄을 제거하고 어떤 일이 일어나는지 확인해 보세요. 그러면 0, 1, 10에 대해서만
일치하고 다른 수 많은 정수는 일치시킬 수 없게 됩니다. 컴파일 에러 메세지로 확인함하면 
좋습니다. 

마지막 팔(match arm)에서 `y`는 일치하는 값에 바인딩됩니다(이 경우 `x`).
이렇게 쓸 수도 있습니다:

```rust
fn print_some(x: i32) {
    match x {
        x => println!("x is something else {}", x)
    }
}
```

여기서 매치 암의 `x`는 인수를 숨기는 새 변수를 도입합니다.  내부 범위에서 변수를 선언하는 
것과 같습니다.

변수 이름을 지정하고 싶지 않다면 이름 없는 변수에 `_`를 사용하면 됩니다,
와일드카드 매치를 사용하는 것과 같습니다. 아무 작업도 하지 않으려면 빈 브랜치를 제공하면 
됩니다:

```rust
fn print_some(x: i32) {
    match x {
        0 => println!("x is zero"),
        1 => println!("x is one"),
        10 => println!("x is ten"),
        _ => {}
    }
}
```

또 다른 의미상의 차이점은 한 팔(match arm)에서 다음 팔(match arm)로 넘어가지 않는다는 
것입니다.  다음 팔로 넘어가지 않으므로 `if...else if...else`처럼 작동합니다.

이후 포스팅에서 매치가 얼마나 강력한 기능인지 살펴보겠습니다. 지금은 값에 대한 `or` 
연산자와 매치 팔의 `if`를 소개합니다. 예제를 통해 쉽게 이해할 수 있기를 바랍니다:

```rust
fn print_some_more(x: i32) {
    match x {
        0 | 1 | 10 => println!("x is one of zero, one, or ten"),
        y if y < 20 => println!("x is less than 20, but not zero, one, or ten"),
        y if y == 200 => println!("x is 200 (but this is not very stylish)"),
        _ => {}
    }
}
```

`if` 표현식과 마찬가지로 `match` 문도 실제로는 표현식이므로 다음과 같이 다시 작성할 수 
있습니다.

```rust
fn print_some_more(x: i32) {
    let msg = match x {
        0 | 1 | 10 => "one of zero, one, or ten",
        y if y < 20 => "less than 20, but not zero, one, or ten",
        y if y == 200 => "200 (but this is not very stylish)",
        _ => "something else"
    };

    println!("x is {}", msg);
}
```

닫는 중괄호 뒤에 세미콜론이 있는 것은 `let` 이 문이기 때문입니다.
문은 문이며 `let msg = ...;` 형식을 취해야 하기 때문입니다. rhs를
일치 표현식(일반적으로 세미콜론이 필요하지 않음)으로 채우지만 `let` 문은 
세미콜론을 필요로 합니다. 이 점이 항상 저를 괴롭힙니다.

동기부여: 러스트의 match 문은 C++ switch 문에서 흔한 버그를 피할 수 있게 합니다. 
즉, `break`을 잊고 다음 문이 실행되는 걸 막아 줍니다. 뒤에 나오는 enum (열거형) 일치를 
추가하면  `match` 문에서 모두 처리하도록 확인합니다. 

## 메서드 호출 

러스트에도 메서드(struct의 구현함수)가 C++과 유사하게 있습니다. 이들 메서드는 항상 
`.` 연산자로 호출되고 `->` 연산자는 없습니다 (뒤에 더 나옵니다). 위에서 `len`, `iter`가
그런 메서드에 해당합니다. 뒤에서 어떻게 메서드를 정의하고 호출하는지 더 설명합니다. 
흐름 제어와 관련하여 C++, 또는 Java (또는 C#)의 사용법이 대체로 러스트에도 그대로 
적용됩니다. 

