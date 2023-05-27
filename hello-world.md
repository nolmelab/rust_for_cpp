# 소개 - 안녕, 월드!

C 또는 C++를 사용하는 이유는 아마도 시스템에 대한 낮은 수준의 액세스가 필요하거나, 모든 
성능을 최대한 활용해야 하거나, 아니면 둘 다 필요하기 때문일 것입니다. Rust는 메모리와 
관련해 동일한 수준의 추상화, 동일한 성능을 제공하면서도 더 안전하고 생산성을 높이는 것을 
목표로 합니다.

구체적으로 C++ 대신 사용할 수 있는 많은 언어가 있습니다: Java, Scala, Haskell, Python 등 
많은 언어가 있지만, 추상화 수준이 너무 높거나(메모리에 직접 액세스할 수 없고 가비지 
컬렉션을 사용해야 하는 등) 성능 문제가 있거나(성능을 예측할 수 없거나 충분히 빠르지 않음) 
사용할 수 없는 경우가 있습니다. Rust는 가비지 컬렉션을 사용하도록 강요하지 않으며, 
C++에서와 마찬가지로 메모리에 대한 원시 포인터를 가지고 놀 수 있습니다. Rust는 C++의 
'사용한 만큼 비용을 지불한다'는 철학을 따릅니다. 기능을 사용하지 않는다면 해당 기능의 
존재에 대한 성능 오버헤드를 지불하지 않아도 됩니다. 또한 Rust의 모든 언어 기능에는 예측 
가능한(그리고 일반적으로 적은) 비용이 발생합니다.

이러한 제약 조건으로 인해 Rust가 (드물게) C++를 대체할 수 있는 실행 가능한 대안이 되기는 
하지만, Rust는 메모리 안전성이라는 장점도 있습니다. Rust의 타입 시스템은 C++에서 흔히 
발생하는 메모리 오류(초기화되지 않은 메모리 액세스, 댕글 포인터 등)가 발생하지 않도록 
보장하며, Rust에서는 모두 불가능합니다. 또한, 다른 제약 조건이 허용하는 경우 Rust는 다른 
안전 문제도 방지하기 위해 노력합니다. 예를 들어 모든 배열 인덱싱은 바운드를 검사합니다
(물론 비용을 피하고 싶다면 (안전성을 희생하더라도) 안전하지 않은 블록에서 이를 수행할 수 
있습니다.) Rust에서는 다른 많은 안전하지 않은 것들과 함께 이 작업을 수행할 수 있습니다. 
결정적으로, Rust는 안전하지 않은 블록의 불안전성이 안전하지 않은 블록에 머물러 나머지 
프로그램에 영향을 미치지 않도록 보장합니다. 마지막으로, Rust는 최신 프로그래밍 언어에서 
많은 개념을 가져와 시스템 프로그래밍 언어에 도입했습니다. 이를 통해 Rust로 프로그래밍하는 
것이 더 생산적이고 효율적이며 즐거워지기를 바랍니다.

이 섹션의 나머지 부분에서는 Rust를 다운로드하여 설치하고, 최소한의 Cargo 프로젝트를 
생성하고, Hello World를 구현하겠습니다.

## 러스트 설치

Rust는 [http://www.rust-lang.org/install.html](http://www.rust-lang.org/install.html) 
에서 다운로드할 수 있습니다. 여기에서 다운로드할 수 있는 것은 Rust 컴파일러, 표준 
라이브러리, Rust용 패키지 관리자 및 빌드 도구인 Cargo입니다.

Rust는 안정 버전, 베타 버전, 야간(nightly) 버전의 세 가지 채널로 제공됩니다. Rust는 6주마다 
새로운 릴리스가 출시되는 빠른 릴리스 일정에 따라 작동합니다. 릴리스 날짜가 되면 야간 버전은 
베타가 되고 베타는 안정 버전이 됩니다.

야간 버전은 매일 밤 업데이트되며, 최신 기능을 실험해보고 싶거나 라이브어리가 미래 러스트 
버전에서도 동작하는지 확인하려는 사용자에게 이상적입니다.

안정 버전은 대부분의 사용자에게 적합한 선택입니다. Rust의 안정성 보장은 안정 채널에만 
적용됩니다.

베타 버전은 주로 코드가 예상대로 작동하는지 확인하기 위해 사용자의 CI(Continunous Integration, 
지속적인 통합)에서 사용하도록 설계 되었습니다.

그래서, 아마도 대부분 안정 버전을 원할 것입니다.  

리눅스나 OS X를 사용한다면 다음이 가장 손쉬운 설치 방법입니다.

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

윈도우에서는 아래와 같이 쉽게 실행할 수 있습니다. 

```
choco install rust
```

<details>
<summary> 놀미 노트 </summary>
choco는 chocolatey(초코레티) 패키지 관리자를 설치해야 합니다.   
[설치법](https://www.lainyzine.com/ko/article/how-to-install-package-manager-chocolatey-on-windows-10/)
을 참고하여 진행해 볼 수 있습니다. 

더 쉬운 방법은 rustup_init.exe를 [러스트업 링크](https://rustup.rs/)에서 다운 받는 방법입니다.
</details>

다른 설치 방법은 [http://www.rust-lang.org/install.html](http://www.rust-lang.org/install.html).
에서 참고합니다. 

러스트 언어의 소스는 [github.com/rust-lang/rust](https://github.com/rust-lang/rust)에서 
볼 수 있습니다. 
 
컴파일러를 빌드하려면, 리눅스나 OSX 기준으로 `./configure && make rustc`를 실행하면 됩니다.
더 자세한 정보를 원하면 [소스에서 빌드하기](https://github.com/rust-lang/rust#building-from-source)
링크를 참고하세요.

## 안녕, 세계!

Rust 프로그램을 빌드하는 가장 쉽고 일반적인 방법은 Cargo를 사용하는 것입니다. `hello`라는 
프로젝트를 시작하려면 `cargo new --bin hello`를 실행합니다. 이렇게 하면 'hello'라는 새
디렉터리가 생성되고 그 안에 `Cargo.toml` 파일과 `main.rs`라는 파일이 있는 `src` 
디렉터리가 생성됩니다.

`Cargo.toml`은 프로젝트에 대한 종속성 및 기타 메타데이터를 정의합니다. 나중에 나중에 
자세히 살펴보겠습니다.

모든 소스 코드는 `src` 디렉터리에 저장됩니다. main.rs`에는 이미
"안녕, 세계!" 프로그램이 이미 포함되어 있습니다. 다음과 같이 보입니다:

```rust
fn main() {
    println!("안녕, 세계!");
}
```

프로그램을 빌드하려면 `cargo build`를 실행합니다. 빌드하고 실행하려면 `cargo run`을 
실행합니다. 만약 `cargo run`을 실행하면 콘솔에 메세지가 표시됩니다. 성공!

카고는 'target' 디렉터리를 만들고 실행 파일을 그 안에 넣습니다.

컴파일러를 직접 사용하려면 `rustc src/main.rs`를 실행하면 됩니다.
`main`이라는 실행 파일을 생성합니다. 더 많은 옵션은 `rustc --help`를 참고하세요.

OK, back to the code. A few interesting points - we use `fn` to define a
function or method. `main()` is the default entry point for our programs (we'll
leave program args for later). There are no separate declarations or header
files as with C++. `println!` is Rust's equivalent of printf. The `!` means that
it is a macro. A subset of the standard library is available without needing to
be explicitly imported/included (the prelude). The `println!` macro is included
as part of that subset.

자, 코드로 돌아가 보겠습니다. 몇 가지 흥미로운 점이 있습니다. `fn`을 사용하여 함수나 
메서드를 정의합니다. `main()`은 프로그램의 기본 진입점입니다 (프로그램 인수는 나중을 위해 
남겨두겠습니다).  

C++처럼 별도의 선언이나 헤더 파일이 없습니다. `println!`은 Rust의 printf에 해당합니다. 
느낌표 `!`는 매크로를 뜻합니다. 표준 라이브러리의 prelude(프렐루드, 전주곡)라 불리는 하위 
집합은 명시적으로 가져올 필요 없이 사용할 수 있습니다.  println!` 매크로는 해당 하위 
집합(전주, prelude)의 일부로 일부로 포함됩니다.

예제를 조금 바꿔 보겠습니다:

```rust
fn main() {
    let world = "세계!";
    println!("안녕, {}!", world);
}
```

`let`은 변수를 도입하는 데 사용되며, `world`는 변수 이름이고, 이 변수는
문자열입니다(엄밀히 말하면 유형은 `&'static str`이지만 나중에 자세히 설명합니다). `world` 
는 유형을 지정할 필요 없이 자동으로 추론됩니다.

`println!` 문에서 `{}`를 사용하는 것은 printf에서 `%s`를 사용하는 것과 같습니다. 사실
는 이보다 조금 더 일반적인데, Rust는 변수가 이미[^1] 문자열이 아닌 경우
문자열로 변환하려고 시도하기 때문입니다(C++의 `operator<<()`처럼).
이런 종류들을 갖고 놀아볼 수 있습니다. 여러 문자열이나 숫자(정수나 부동 소숫점 같은)를 
시도해 보세요. 

원하는 경우 명시적으로 'world'의 유형을 지정할 수 있습니다:

```rust
let world: &'static str = "world";
```

In C++ we write `T x` to declare a variable `x` with type `T`. In Rust we write
`x: T`, whether in `let` statements or function signatures, etc. Mostly we omit
explicit types in `let` statements, but they are required for function
arguments. Let's add another function to see it work:

C++에서는 `T x`를 작성하여 `T` 타입의 변수 `x`를 선언합니다. Rust에서는 다음과 같이 
작성합니다. `x: T`를 `let` 문이나 함수 서명 등에서 사용합니다. `let`문에서 대부분 
명시적인 타잎을 생략합니다. 함수 아규먼트에서는 타잎을 필요로 합니다. 다른 함수를 추가하여
동작하는 것을 확인해 보겠습니다. 

```rust
fn foo(_x: &'static str) -> &'static str {
    "월드"
}

fn main() {
    println!("안녕{}!", foo("bar"));
}
```

The function `foo` has a single argument `_x` which is a string literal (we pass
it "bar" from `main`)[^2].

함수 `foo`에는 문자열 리터럴인 단일 인자 `_x`가 있습니다(`main`에서 "bar"를 전달합니다)[^2].

함수의 반환 유형은 `->` 뒤에 주어집니다. 함수가 아무 것도 반환하지 않는다면
(C++의 void 함수), 반환 유형을 지정할 필요가 없습니다. 암묵적으로 ()를 반환하고 
명확하게 하려면 `-> ()`를 추가합니다. `()`는 러스트의 `void` 타잎입니다. 

만약 함수 본문(또는 나중에 살펴 볼 다른 어떤 블록들)이 세미콜론(`;`)으로 마지막 표현식이 
끝나지 않는다면 그것이 반환값이 됩니다. 따라서, 이런 경우 `return` 문이 필요 없습니다. 
따라서, `foo` 함수는 `세계`를 반환합니다. `return` 키워드는 여전히 있어서 일찍 반환할 때
사용할 수 있습니다. `"세계"`를 `return "세계"`로 변경해도 갖은 효과를 갖습니다.  

## 왜?

위의 언어 기능 중 일부에 대해 동기부여를 하고 싶습니다. 로컬 타잎 추론은 안전이나 성능을 
희생하지 않으면서도 편리하고 유용합니다(최신 버전의 C++에서도 지원됩니다. `auto`나 
`decltype`). 

사소하지만 편리한 점은 언어의 항목들이 키워드(`fn`, `let` 등)로 일관되게 표시된다는 
점입니다. 이는 눈이나 툴로 살펴보기가 쉬워지며 러스트의 구문은 C++보다 더 일관성이 
있습니다.   

println! 매크로는 printf 보다 안전합니다. 인자의 개수가 문자열에 있는 위치(`{}`) 개수와 
맞는지 확인하고 인자의 타잎을 검사합니다. 이는 printf에서 다른 타잎으로 잘못 출력하거나
스택 메모리의 더 아래 쪽을 출력하는 것과 같은 실수를 방지해 줍니다. 

사소하지만 이런 기능들이 러스트의 디자인 철학을 묘사하는데 도움이 되길 바랍니다.  

[^1]: 이것은 `Display` 트레이트(특성)을 사용하는 프로그래머 지정 변환입니다. Java의 
`toString`과 비슷하게 작동합니다. `{:?}`를 사용하여 컴파일러가 생성한 표현을 출력할 수도 
있는데 디버깅에 도움이 됩니다. printf처럼 많은 여러가지 옵션이 있습니다.


[^2]: We don't actually use that argument in `foo`. 

Usually, Rust will warn us about this. 
By prefixing the argument name with `_` we avoid
these warnings. In fact, we don't need to name the argument at all, we could
just use `_`.

[^2]: 우리는 실제로 `foo`에서 이 인수를 사용하지 않습니다. 일반적으로 Rust는 이에 대해 
경고합니다. 인수 이름 앞에 `_`를 붙이면 이러한 경고를 피할 수 있습니다. 사실, 인자 이름을 
전혀 지정할 필요가 없습니다. 그냥 `_`를 사용하면 됩니다.

