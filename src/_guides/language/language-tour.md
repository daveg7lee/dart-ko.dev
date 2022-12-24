---
title: Dart로 떠나는 여행
description: Dart 언어의 중요한 기능에 대해 학습합니다.
short-title: Language tour
js: [{url: 'https://dartpad.dev/inject_embed.dart.js', defer: true}]
---
<?code-excerpt replace="/ *\/\/\s+ignore_for_file:[^\n]+\n//g; /(^|\n) *\/\/\s+ignore:[^\n]+\n/$1/g; /(\n[^\n]+) *\/\/\s+ignore:[^\n]+\n/$1\n/g; / *\/\/\s+ignore:[^\n]+//g; /([A-Z]\w*)\d\b/$1/g"?>

이 페이지에서는 다른 프로그래밍 언어를 사용 할 줄 안다는 가정 하에 
변수, 연산자, 클래스 및 라이브러리에 이르는 각 주요 Dart 기능을 사용하는 방법을 알려드립니다. 
언어에 대한 간략한 설명을 보고 싶다면, [샘플 페이지](/samples)를 참고하세요!

Dart의 핵심 라이브러리에 대해 학습하고 싶다면 [library tour](/guides/libraries/library-tour)를 참고하세요. 
언어의 기능에 대한 자세한 정보를 얻고 싶다면 [Dart 언어 설명서][]을 참고하세요.

{{site.alert.note}}
  DartPad를 사용하여 Dart 언어를 체험해볼 수 있습니다.
  ([DartPad란?](/tools/dartpad)).
  **<a href="{{site.dartpad}}" target="_blank" rel="noopener">
  DartPad 열기</a>**

  위의 페이지에서 Dart의 활용 예제들을 보여주기 위해 임베디드 DartPads를 사용합니다. 
  {% include dartpads-embedded-troubleshooting.md %}
{{site.alert.end}}

{% comment %}
[TODO #2950: Look for null, ?, !, late, optional, Map, List, Set.
(Anything else?)
Look for dynamic. Look for code that isn't auto-included (no code-excerpt.)]
{% endcomment %}

## 기본적인 Dart 프로그램

아래 코드는 Dart의 가장 기본적인 기능들을 사용합니다.

<?code-excerpt "misc/test/language_tour/basic_test.dart"?>
```dart
// 함수 정의.
void printInteger(int aNumber) {
  print('The number is $aNumber.'); // 콘솔에 프린트.
}

// 앱의 실행을 시작하는 부분.
void main() {
  var number = 42; // 변수의 선언과 초기화.
  printInteger(number); // 함수의 호출.
}
```

아래는 이 프로그램이 사용한 모든 (혹은 거의 모든) Dart 앱에 적용할 수 있는 
요소들입니다:

<code>// <em>이건 주석입니다.</em> </code>
:   단일행 주석입니다.
    Dart에서는 다중행 그리고 문서 주석도 지원합니다.
    주석에 대해 더 자세히 알고 싶다면, [주석](#comments)을 참고하세요.

`void`
:   사용하지 않을 값을 나타내는 특수한 타입입니다.
    `printInteger()` and `main()` 같이 명시적으로 값을 반환하지 않는 함수들은 
    `void` 반환 타입을 가집니다.
    
`int`
:   정수를 나타내는 또 다른 타입입니다.
    추가적인 [내장 타입](#built-in-types)으로
    `String`, `List`, 및 `bool`이 있습니다.

`42`
:   숫자 리터럴 입니다. 숫자 리터럴은 컴파일 타임 상수입니다.

`print()`
:   산출물을 출력하는 간단한 방법입니다.

`'...'` (또는 `"..."`)
:   문자열 리터럴입니다.

<code>$<em>변수이름</em></code> (또는 <code>${<em>표현식</em>}</code>)
:   문자열 보간(String interpolation): 문자열 리터럴 내부의 변수 또는 표현식의 문자열과
    동일한 값을 포함합니다. 더 자세한 정보를 원한다면,
    [Strings](#strings)을 참고하세요.

`main()`
:   앱의 실행이 시작되는, 특수하고 필수적인 최상위 함수입니다.
    더 자세한 정보를 원한다면,
    [main() 함수](#the-main-function)를 참고하세요.

`var`
:   타입을 특정하지 않고 변수를 선언하는 방법입니다.
    초기 값 (`42`)으로 의해 이 변수의 타입은 (`int`)로 결정됩니다.


{{site.alert.note}}
  이 사이트의 코드는 [Dart 스타일 가이드](/guides/language/effective-dart/style)의
  관습을 따릅니다.
{{site.alert.end}}


## 주요 컨셉

Dart 언어를 학습 할 때 다음을 잘 기억해야합니다:

-   변수로 할당 할 수 있는 모든 것은 *객체*이고, 모든 객체는 *클래스*의 instance 입니다.
    숫자, 함수 그리고 `null`까지 모두 객체입니다.
    `null`을 제외하고 (만약 [sound null safety][ns])를 활성화했다면),
     모든 객체들은 [`Object`][] 클래스를 상속받습니다.

    {{site.alert.version-note}}
      [Null safety][ns]는 in Dart 2.12에 처음 소개되었습니다..
      null safety를 사용하기 위해서 최소 2.12의 [language version][]이 필요합니다.
    {{site.alert.end}}

-   Dart는 타입에 엄격하지만, 추론할 수 있기 때문에 타입 어노테이션은 자율에 맡깁니다.
    위의 코드에서 `number`는 `int` 타입으로 추론됩니다.

-   [Null safety][ns]를 활성화했다면,
    어떤 변수가 null 값을 갖는 것을 허락하지 않았을 때 
    그 변수가 null 값을 가질 수 없게 할 수 있습니다.
    타입의 끝에 물음표 (`?`)를 추가하면 해당 변수를
    nullable로 만들 수 있습니다.
    예를 들어, `int?` 타입의 변수는 정수 또는 `null` 값을 가집니다.
    만약 표현식이 null로 평가되지 않지만 Dart가 이에 동의하지 않는다는 것을
    *알고* 있다면, `!`을 추가하여 null이 아니라고 주장(assert) 할 수 있습니다
    (만약 null이라면 예외를 throw 합니다).
    예시: `int x = nullableButNotNullInt!`

-   만약에 어떤 타입이든 적용이 가능하다고 명시하고 싶다면, 
    `Object?` ([null safety][ns-enable]를 활성화했다면)
    또는 `Object`를 타입으로 설정해주면 된다.
    먄약 런타임까지 타입 체킹을 미뤄야 한다면,
    [특수 타입인 `dynamic`][ObjectVsDynamic]를 사용하세요.

-   Dart는 `List<int>` (정수의 list)
    또는 `List<Object>` (아무 타입의 list) 같은 제네릭 타입을 지원합니다.

-   Dart는 클래스나 객체에 묶여있는 함수들 
    (각각 *static*, *instance* 메서드) 뿐만 아니라, 
    `main()` 같은 최상위 함수를 지원합니다.
    *중첩 함수* 또는 *지역 함수*처럼 함수 안에 함수를 생성할 수 있습니다.

-   유사하게, Dart는 클래스나 객체에 묶여있는 변수들 (각각 static, instance 변수)
    뿐만 아니라, 최상위 *변수* 또한 지원합니다. 
    Instance 변수들은 *필드*, *프로퍼티* 로도 알려져있습니다.

-   Java와 다르게, Dart는 `public`, `projected` 그리고 `private` 같은 키워드가 없습니다.
    식별자가 언더 스코어 (`_`)로 시작한다면, 이것은 해당 라이브러리에 귀속된(private) 것입니다.
    더 자세한 정보를 원한다면, [Libraries and visibility](#libraries-and-visibility)
    를 참고하세요.

-   *식별자*는 문자 또는 언더 스코어 (`_`)로 시작 할 수 있고,
    어떠한 문자와 숫자의 결합이 가능합니다.

-   Dart는 런타임 값을 가지는 *식 (expression)*과
    그렇지 않은 *문 (statement)*를 가지고 있습니다.
    예를 들어, [조건식 (conditional expression)](#conditional-expressions)인
    `condition ? expr1 : expr2` 은 `expr1` 또는 `expr2`의 값을 가집니다.
    위의 expression과 값을 가지지 않는 
    [if-else 문](#if-and-else)를 비교해봅시다.
    문은 때로 하나 혹은 그 이상의 식을 포함하지만,
    식은 직접적으로 문을 포함할 수 없습니다.

-   Dart tools는 두가지 문제를 리포트 해줍니다: _경고_ 그리고 _에러_.
    경고는 코드가 제대로 작동하지 않을 수도 있다는 것을 의미하지만,
    프로그램의 실행을 막지 않습니다. 에러는 컴파일 타임 또는 런타임으로 구분됩니다.
    컴파일 타임 에러는 코드가 실행되는 것을 막습니다;
    런타임 에러는 코드가 실행되는 동안 [예외](#exceptions)를 발생시킵니다.


## 키워드

다음 테이블 리스트는 Dart 언어가 특별히 관리하는 단어들입니다.

{% assign ckw = '&nbsp;<sup title="contextual keyword" alt="contextual keyword">1</sup>' %}
{% assign bii = '&nbsp;<sup title="built-in-identifier" alt="built-in-identifier">2</sup>' %}
{% assign lrw = '&nbsp;<sup title="limited reserved word" alt="limited reserved word">3</sup>' %}
<div class="table-wrapper" markdown="1">
| [abstract][]{{bii}}   | [else][]              | [import][]{{bii}}     | [show][]{{ckw}}   |
| [as][]{{bii}}         | [enum][]              | [in][]                | [static][]{{bii}} |
| [assert][]            | [export][]{{bii}}     | [interface][]{{bii}}  | [super][]         |
| [async][]{{ckw}}      | [extends][]           | [is][]                | [switch][]        |
| [await][]{{lrw}}      | [extension][]{{bii}}  | [late][]{{bii}}       | [sync][]{{ckw}}   |
| [break][]             | [external][]{{bii}}   | [library][]{{bii}}    | [this][]          |
| [case][]              | [factory][]{{bii}}    | [mixin][]{{bii}}      | [throw][]         |
| [catch][]             | [false][]             | [new][]               | [true][]          |
| [class][]             | [final][]             | [null][]              | [try][]           |
| [const][]             | [finally][]           | [on][]{{ckw}}         | [typedef][]{{bii}}|
| [continue][]          | [for][]               | [operator][]{{bii}}   | [var][]           |
| [covariant][]{{bii}}  | [Function][]{{bii}}   | [part][]{{bii}}       | [void][]          |
| [default][]           | [get][]{{bii}}        | [required][]{{bii}}   | [while][]         |
| [deferred][]{{bii}}   | [hide][]{{ckw}}       | [rethrow][]           | [with][]          |
| [do][]                | [if][]                | [return][]            | [yield][]{{lrw}}  |
| [dynamic][]{{bii}}    | [implements][]{{bii}} | [set][]{{bii}}        |                   |
{:.table .table-striped .nowrap}
</div>

[abstract]: #abstract-classes
[as]: #type-test-operators
[assert]: #assert
[async]: #asynchrony-support
[await]: #asynchrony-support
[break]: #break-and-continue
[case]: #switch-and-case
[catch]: #catch
[class]: #instance-variables
[const]: #final-and-const
{% comment %}
  [TODO #2950: Make sure that points to a place that talks about const constructors,
  as well as const literals and variables.]
{% endcomment %}
[continue]: #break-and-continue
[covariant]: /guides/language/sound-problems#the-covariant-keyword
[default]: #switch-and-case
[deferred]: #lazily-loading-a-library
[do]: #while-and-do-while
[dynamic]: #important-concepts
[else]: #if-and-else
[enum]: #enumerated-types
[export]: /guides/libraries/create-library-packages
[extends]: #extending-a-class
[extension]: #extension-methods
[external]: https://spec.dart.dev/DartLangSpecDraft.pdf#External%20Functions
[factory]: #factory-constructors
[false]: #booleans
[final]: #final-and-const
[finally]: #finally
[for]: #for-loops
[Function]: #functions
[get]: #getters-and-setters
[hide]: #importing-only-part-of-a-library
[if]: #if-and-else
[implements]: #implicit-interfaces
[import]: #using-libraries
[in]: #for-loops
[interface]: #implicit-interfaces
[is]: #type-test-operators
[late]: #late-variables
[library]: #libraries-and-visibility
[mixin]: #adding-features-to-a-class-mixins
[new]: #using-constructors
[null]: #default-value
[on]: #catch
[operator]: #_operators
[part]: /guides/libraries/create-library-packages#organizing-a-library-package
[required]: #named-parameters
[rethrow]: #catch
[return]: #functions
[set]: #getters-and-setters
[show]: #importing-only-part-of-a-library
[static]: #class-variables-and-methods
[super]: #extending-a-class
[switch]: #switch-and-case
[sync]: #generators
[this]: #constructors
[throw]: #throw
[true]: #booleans
[try]: #catch
[typedef]: #typedefs
[var]: #variables
[void]: #built-in-types
{% comment %}
  TODO #2950: Add coverage of void to the language tour.
{% endcomment %}
[with]: #adding-features-to-a-class-mixins
[while]: #while-and-do-while
[yield]: #generators

이 단어들을 식별자로 사용하는 것을 지양하세요.
그러나, 만약 필요하다면, 윗첨자로 표시된 단어들은 식별자로 사용이 가능합니다.

* **1**로 표시된 단어들은 **맥락적인 키워드(contextual keywords)**로
  득정한 장소에서만 의미를 가집니다.
  어디서든 유효한 식별자로 하용이 가능합니다..

* **2**로 표시된 단어들은 **내장 식별자(built-in identifiers)**로
  이 키워드들은 거의 모든 곳에서 식별자로 사용이 가능하지만,
  클래스나 타입의 이름, import prefix로 사용은 불가능합니다.

* **3**으로 표시된 단어들은 [비동기 지원](#asynchrony-support)과
  관련된 제한된 단어들 입니다. `await` 또는 `yield`를
  `async`, `async*`, or `sync*`로 표시된 함수의 바디에서
  식별자로 사용 할 수 없습니다.

표의 나머지 단어들은 모두  **예약된 단어(reserved words)**들로,
식별자로 사용이 불가능 합니다.


## 변수

아래는 변수를 생성하고 초기화하는 예제입니다:

<?code-excerpt "misc/lib/language_tour/variables.dart (var-decl)"?>
```dart
var name = 'Bob';
```

변수는 레퍼런스(reference)를 저장합니다. `name` 이라는 변수는 "Bob"이라는 값을
가지고 있는 `String` 객체의 레퍼런스를 포함합니다.

`name`의 타입은 `String`으로 추론되지만, 구체적으로 명시하여 타입을 변경 할 수 있습니다.
만약 객체가 단일 타입으로 제한되지 않는다면,
`Object` 타입으로 명시하세요 (필요하다면 `dynamic` 사용).

<?code-excerpt "misc/lib/language_tour/variables.dart (type-decl)"?>
```dart
Object name = 'Bob';
```

추론될 타입으로 명시하여 선언하는 방법도 있습니다:

<?code-excerpt "misc/lib/language_tour/variables.dart (static-types)"?>
```dart
String name = 'Bob';
```

{{site.alert.note}}
  이 페이지에서는 [스타일 가이드 추천](/guides/language/effective-dart/design#types)
  에 따라 지역 변수에 대해 타입 어노테이션을 하지 않고, `var`을 사용합니다.
{{site.alert.end}}


### 디폴트 값

Nullable 타입을 가지는 초기화되지 않은 변수는
초기 값으로 `null`을 가질 수 있습니다.
([Null safety][ns]을 사용하지 않는다면,
모든 변수는 nullable 타입을 가집니다.)
Dart의 다른 모든 것과 마찬가지로 숫자도 객체이기 때문에,
숫자 타입의 변수도 처음에는 null 입니다.

<?code-excerpt "misc/test/language_tour/variables_test.dart (var-null-init)"?>
```dart
int? lineCount;
assert(lineCount == null);
```

{{site.alert.note}}
  프로덕션 코드는 `assert()` 호출을 무시합니다. 반면에 개발하는 동안에는, 
  _조건_이 false라면 <code>assert(<em>조건</em>)</code> 가 예외를
  throw 합니다. 더 자세한 것은, [Assert](#assert)를 참고하세요.
{{site.alert.end}}

Null safety를 활성화 했다면, non-nullable 변수를 사용하기 전에
값들을 초기화해야만합니다:

<?code-excerpt "misc/lib/language_tour/variables.dart (var-ns-init)"?>
```dart
int lineCount = 0;
```

지역 변수를 선언과 동시에 초기화 할 필요는 없지만,
사용하기 전에 값을 할당해야 합니다.
예를 들어, 다음 코드는 `lineCount`가 `print()`로 전달될 때까지
null이 아님을 알 수 있기 때문에 유효합니다:

<?code-excerpt "misc/lib/language_tour/variables.dart (var-ns-flow)"?>
```dart
int lineCount;

if (weLikeToCount) {
  lineCount = countLines();
} else {
  lineCount = 0;
}

print(lineCount);
```

최상위, 클래스 변수는 지연 초기화 됩니다;
변수가 처음 사용 될 때, 초기화 코드가 실행됩니다.


### Late 변수

Dart 2.12에서 `late` 수식어가 추가되었습니다. 두 가지 사용례가 있습니다:

* 선언 이후에 초기화되는 non-nullable 변수를 선언하는 것
* 변수를 초기화를 지연하는 것

보통 Dart의 제어 흐름 분석기는 non-nullable 변수가
non-null 값으로 설정되어 있는지 사용하기 전에 알아챌 수 있지만,
가끔 실패할 때도 있습니다.
가장 흔히 사용되는 두가지 케이스는 최상위 변수와 인스턴스 변수입니다:
Dart는 종종 그 변수들이 설정되었는지 판단할 수 없기 때문에,
시도하지 않습니다.

변숫값의 설정이 사용 전에 보장되지만,
Dart가 동의하지 않는다면,
해당 변수를 `late`로 표시하여 에러를 해결할 수 있습니다:

<?code-excerpt "misc/lib/language_tour/variables.dart (var-late-top-level)" replace="/late/[!$&!]/g"?>
```dart
[!late!] String description;

void main() {
  description = 'Feijoada!';
  print(description);
}
```

{{site.alert.warn}}
  `late` 변수의 초기화를 실패하였다면,
  해당 변수를 사용 할 때 런타임 에러가 발생합니다.
{{site.alert.end}}

`late`로 표시한 변수를 선언과 동시에 초기화하면,
변수가 처음 사용될 때 initializer가 실행됩니다.
지연 초기화는 다음과 같은 상황에 유용합니다:

* 변수가 당장 필요하진 않지만,
  초기화 비용이 비쌀 때.
* 인스턴스 변수를 초기화에 initializer가
  `this`에 대한 접근이 필요할 때.

다음 예제에서,
`temperature` 변수가 사용되지 않으면,
비싼 함수인 `readThermometer()`가 호출되지 않습니다:

<?code-excerpt "misc/lib/language_tour/variables.dart (var-late-lazy)" replace="/late/[!$&!]/g"?>
```dart
// 이 프로그램에서 readThermometer()에 대한 유일한 호출입니다.
[!late!] String temperature = readThermometer(); // 지연 초기화.
```


### Final 그리고 const

변수를 변경할 생각이 없다면, `var` 대신 `final`이나 `const`를 사용하거나,
지정한 타입에 추가하여 사용하세요.
final 변수는 오직 한 번만 설정될 수 있습니다; const 변수는 컴파일 타임 상수입니다.
(const 변수는 내부적으로 final입니다.)

{{site.alert.note}}
  [인스턴스 변수](#instance-variables)는 `final`로 설정될 수 있지만, `const`는 될 수 없습니다.
{{site.alert.end}}

다음은 `final` 변수를 생성, 설정하는 예제입니다:

<?code-excerpt "misc/lib/language_tour/variables.dart (final)"?>
```dart
final name = 'Bob'; // 타입 어노테이션이 없음
final String nickname = 'Bobby';
```

`final` 변수의 값은 변경할 수 없습니다:

{:.fails-sa}
<?code-excerpt "misc/lib/language_tour/variables.dart (cant-assign-to-final)"?>
```dart
name = 'Alice'; // 에러: final 변수는 한 번만 설정될 수 있습니다.
```


**컴파일 타임 상수**인 변수를 생성할 때 `const`를 사용하세요.
const 변수가 클래스 레벨의 변수라면, `static const`로 표시하세요.
변수를 선언할 때, 숫자, 문자열 리터럴, 상수 변수, 
또는 상수 숫자에 대한 산술 연산의 결과 같은 값들은 컴파일 타임 상수로 선언하세요:

<?code-excerpt "misc/lib/language_tour/variables.dart (const)"?>
```dart
const bar = 1000000; // 압력의 단위(dynes/cm2)
const double atm = 1.01325 * bar; // 표준 대기
```

`const` 키워드는 상수 변수를 선언할 때만 쓰이는 것이 아닙니다.
상수 _값_을 만드는 데 사용할 수 있을 뿐만 아니라,
상수 값을 _만드는_ 생성자를 선언할 수도 있습니다.
모든 변수는 상수 값을 가질 수 있습니다.

<?code-excerpt "misc/lib/language_tour/variables.dart (const-vs-final)"?>
```dart
var foo = const [];
final bar = const [];
const baz = []; // `const []`와 동일
```

위의 `baz`처럼, `const` 선언의 초기화 식에 `const`를 생략해도 됩니다.
더 자세히 알고 싶다면, [const를 중복으로 사용하지 마십시오][]를 참고하세요.

이전에 `const` 값을 가지고 있었더라도,
non-final, non-const 변수의 값을 변경할 수 있습니다.

<?code-excerpt "misc/lib/language_tour/variables.dart (reassign-to-non-final)"?>
```dart
foo = [1, 2, 3]; // const [] 였음
```

You can't change the value of a `const` variable:

{:.fails-sa}
<?code-excerpt "misc/lib/language_tour/variables.dart (cant-assign-to-const)"?>
```dart
baz = [42]; // 에러: 상수 변수는 값이 할당될 수 없습니다.
```

[타입 체크와 캐스트](#type-test-operators) (`is` 그리고 `as`),
[컬렉션 `if`](#collection-operators),
그리고 [전개 연산자(spread operator)](#spread-operator) (`...` 그리고 `...?`)를
사용하는 상수 정의가 가능합니다.:

<?code-excerpt "misc/lib/language_tour/variables.dart (const-dart-25)"?>
```dart
const Object i = 3; // i는 정수 값을 가지는 const Object입니다.
const list = [i as int]; // 타입 캐스트를 사용하세요.
const map = {if (i is int) i: 'int'}; // is와 컬렉션 if를 사용하세요.
const set = {if (list is List<int>) ...list}; // ...를 사용하여 전개.
```

{{site.alert.note}}
  `final` 객체는 수정될 수 없지만,
  객체의 필드는 수정이 가능합니다.
  반면에, `const` 객체와 객체의 필드는 _불변_하기 때문에
  변경할 수 없습니다.
{{site.alert.end}}

상수 값을 만들기 위해 `const`를 사용하는 방법에 대한 더 자세한 내용은 
[Lists](#lists), [Maps](#maps), and [Classes](#classes)을 참고하세요.


## 내장 타입

Dart 언어는 다음과 같은 특수한 내장 타입을 지원합니다:

- [Numbers](#numbers) (`int`, `double`)
- [Strings](#strings) (`String`)
- [Booleans](#booleans) (`bool`)
- [Lists](#lists) (`List`, *arrays*로도 부릅니다.)
- [Sets](#sets) (`Set`)
- [Maps](#maps) (`Map`)
- [Runes](#characters) (`Runes`; 때로 `characters` API로 대체됩니다.)
- [Symbols](#symbols) (`Symbol`)
- `null` (`Null`)

이런 타입들은 리터럴을 사용하여 객체를 생성할 수 있습니다.
예를 들어, `'this is a string'`은 문자열 리터럴이고,
`true`는 boolean 리터럴입니다.

Dart의 모든 변수는 객체 (*클래스*의 인스턴스)이기 때문에,
변수를 초기화 할 때 *생성자*를 사용할 수 있습니다.
몇몇 내장 타입은 자신만의 생성자를 가지고 있습니다.
예를 들어, `Map()` 생성자를 사용하여 map을 생성할 수 있습니다.

Dart 언어의 일부 타입들은 특수한 역할을 수행합니다:

* `Object`: `Null`을 제외한 모든 Dart 클래스의 superclass.
* `Enum`: 모든 eunm의 superclass.
* `Future`, `Stream`: [비동기 지원](#asynchrony-support)에서 사용됩니다.
* `Iterable`: [for-in 루프][iteration] 그리고
  동기식 [제네레이터 함수](#generator)에서 사용됩니다.
* `Never`: 식(expression)의 평가(evaluating)를 완료할 수 없음을 나타냅니다.
  항상 예외를 throw하는 함수에서 보통 사용됩니다.
* `dynamic`: 정적 타입 체킹의 비활성화를 의미합니다.
  대개 `Object` 또는 `Object?`를 대신 사용하세요.
* `void`: 값이 사용되지 않는다는 것을 의미합니다.
  보통 반환 타입으로 사용됩니다.

{% comment %}
[TODO: move/add for-in coverage to language tour?]
{% endcomment %}

`Object`, `Object?`, `Null`, 그리고 `Never` 클래스는
[Understanding null safety][]의 [top-and-bottom][]
섹션에 묘사되어 있는 것처럼, 클래스 계층에서 특별한 역할을 수행합니다.

{% comment %}
If we decide to cover `dynamic` more,
here's a nice example that illustrates what dynamic does:
  dynamic a = 2;
  String b = a; // No problem! Until runtime, when you get an uncaught error.

  Object c = 2;
  String d = c;  // Problem!
{% endcomment %}


### Numbers

Dart의 숫자는 두 가지 유형이 있습니다:

[`int`][]

:   [사용하는 플랫폼에 따라서][dart-numbers]
    정수 값은 64비트 이하로 표현됩니다.
    네이티브 플랫폼에서는 -2<sup>63</sup> ~ 2<sup>63</sup> - 1
    까지 표현됩니다.
    웹에서는, Javascript numbers (가수부가 없는 64-bits 부동소수점 표현)
    -253 ~ 253 - 1 사이의 수로 표현됩니다.

[`double`][]

:   IEEE 754 standard를 따라 64-bit (배정도) 부동 소수점 표현을 사용합니다.

`int` 와 `double`는 모두 [`num`][]의 서브타입입니다.
num 타입은 +, -, /, * 같은 기본적인 연산자 사용이 가능하고, `abs()`,` ceil()`,
그리고 `floor()` 같은 함수의 사용도 가능합니다.
(\>\> 같은 Bitwise 연산자는 `int` 클래스에 정의되어 있습니다.)
만일 찾고있는 것이 num과 num의 서브타입이 가지고 있지 않다면, [dart:math][]
라이브러리를 참고하세요.

정수는 소수점이 없는 숫자입니다. 다음은 정수 리터럴을 정의하는 예제입니다:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (integer-literals)"?>
```dart
var x = 1;
var hex = 0xDEADBEEF;
```

숫자가 소수점을 가지고 있다면, 그것은 double 입니다. 다음은 double 리터럴을 정의하는 예제입니다:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (double-literals)"?>
```dart
var y = 1.1;
var exponents = 1.42e5;
```

변수를 num으로 선언할 수도 있습니다. 이렇게 선언하면, 해당 변수는
정수, double 값을 모두 가질 수 있습니다.

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (declare-num)"?>
```dart
num x = 1; // x는 int, double 둘 다 가능합니다.
x += 2.5;
```

정수 리터럴은 필요하다면, 자동으로 double 변환됩니다:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (int-to-double)"?>
```dart
double z = 1; // double z = 1.0 와 동일합니다.
```

다음은 예제에서 문자열을 숫자로 그리고 그 반대의 변환도 수행합니다.

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (number-conversion)"?>
```dart
// String -> int
var one = int.parse('1');
assert(one == 1);

// String -> double
var onePointOne = double.parse('1.1');
assert(onePointOne == 1.1);

// int -> String
String oneAsString = 1.toString();
assert(oneAsString == '1');

// double -> String
String piAsString = 3.14159.toStringAsFixed(2);
assert(piAsString == '3.14');
```

`int` 타입은 비트 필드에서 플래그를 조작하고 마스킹하는 데 유용한
전통 비트 단위 쉬프트 (`<<`, `>>`, `>>>`),
보수 (`~`), AND (`&`), OR (`|`), 그리고 XOR (`^`) 연산자를 지원합니다.
예제:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (bit-shifting)"?>
```dart
assert((3 << 1) == 6); // 0011 << 1 == 0110
assert((3 | 4) == 7); // 0011 | 0100 == 0111
assert((3 & 4) == 0); // 0011 & 0100 == 0000
```

더 많은 예제를 보고 싶다면,
[비트 단위 및 쉬프트 연산자](#bitwise-and-shift-operators) 섹션을 참고하세요.

리터럴 숫자는 컴파일 타임 상수입니다.
피연산자가 숫자를 평가(evaluate)하는 컴파일 타임 상수인 이상,
산술 표현식(expression)도 컴파일 타임 상수 입니다.

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (const-num)"?>
```dart
const msPerSecond = 1000;
const secondsUntilRetry = 5;
const msUntilRetry = secondsUntilRetry * msPerSecond;
```

더 많은 정보를 원한다면, [Dart의 숫자][dart-numbers]를 참고하세요.

[dart-numbers]: /guides/language/numbers


### Strings

Dart의 문자열 (`String` 객체)는 UTRF-16 코드 유닛의 시퀀스를 홀드합니다.
문자열을 만들 때 작은 따옴표, 큰 따옴표 모두 사용이 가능합니다:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (quoting)"?>
```dart
var s1 = 'Single quotes work well for string literals.';
var s2 = "Double quotes work just as well.";
var s3 = 'It\'s easy to escape the string delimiter.';
var s4 = "It's even easier to use the other delimiter.";
```

`${`*`표현식`*`}`을 사용하여 표현식 안에 값을 넣을 수 있습니다.
표현식이 식별자라면, {}을 생략할 수 있습니다. 객체와 일치하는 문자열을 얻기 위해,
Dart는 객체의 `toString()` 메서드를 호출합니다.

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (string-interpolation)"?>
```dart
var s = 'string interpolation';

assert('Dart has $s, which is very handy.' ==
    'Dart has string interpolation, '
        'which is very handy.');
assert('That deserves all caps. '
        '${s.toUpperCase()} is very handy!' ==
    'That deserves all caps. '
        'STRING INTERPOLATION is very handy!');
```

{{site.alert.note}}
  `==` 연산자는 두 객체가 동일한지 테스트합니다.
  두 문자열이 동일한 코드 유닛의 시퀀스를 포함한다면, 같은 문자열로 판단합니다.
{{site.alert.end}}

인접 문자열 리터럴 (adjacent string literal) 또는
`+` 연산자를 사용해 문자열을 합칠 수 있습니다:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (adjacent-string-literals)"?>
```dart
var s1 = 'String '
    'concatenation'
    " works even over line breaks.";
assert(s1 ==
    'String concatenation works even over '
        'line breaks.');

var s2 = 'The + operator ' + 'works, as well.';
assert(s2 == 'The + operator works, as well.');
```

작은 따옴표 또는 큰 따옴표로 Triple quote를 형성해
멀티 라인 문자열을 만들 수 있습니다.

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (triple-quotes)"?>
```dart
var s1 = '''
You can create
multi-line strings like this one.
''';

var s2 = """This is also a
multi-line string.""";
```

`r`을 사용하여 "로우" (raw) 문자열을 생성할 수 있습니다:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (raw-strings)"?>
```dart
var s = r'In a raw string, not even \n gets special treatment.';
```

유니코드 문자로 문자열을 표현하는 방법에 대해 자세히 알고 싶다면,
[Runes and grapheme clusters](#characters)을 참고하세요.

Null, 숫자, 문자열 또는 boolean 값을 평가하는
보간된 표현식 (interpolated expression)이
컴파일 타임 상수인 이상, 리터럴 문자열은 컴파일 타임 상수입니다.

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (string-literals)"?>
```dart
// 이 값들은 const 문자열로 작동합니다.
const aConstNum = 0;
const aConstBool = true;
const aConstString = 'a constant string';

// 이 값들은 const 문자열로 작동하지 않습니다.
var aNum = 0;
var aBool = true;
var aString = 'a string';
const aConstList = [1, 2, 3];

const validConstString = '$aConstNum $aConstBool $aConstString';
// const invalidConstString = '$aNum $aBool $aString $aConstList';
```

문자열 사용에 대해 더 자세히 알고 싶다면,
[문자열과 정규 표현식](/guides/libraries/library-tour#strings-and-regular-expressions)을
참고하세요.


### Booleans

Dart는 Boolean형 타입을 `bool`로 명명하였습니다. 
컴파일 타임 상수인 Boolean 리터럴:
`true`와 `false`가 유일한 bool 타입 객체입니다.

Dart의 type safety는
<code>if (<em>nonbooleanValue</em>)</code> 또는
<code>assert (<em>nonbooleanValue</em>)</code>
같은 코드를 사용할 수 없다는 것을 의미합니다.
대신, 다음과 같이 명시적으로 값을 확인해야합니다.

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (no-truthy)"?>
```dart
// 빈 문자열인지 확인합니다.
var fullName = '';
assert(fullName.isEmpty);

// 0인지 확인합니다.
var hitPoints = 0;
assert(hitPoints <= 0);

// null인지 확인합니다.
var unicorn;
assert(unicorn == null);

// NaN인지 확인합니다.
var iMeantToDoThis = 0 / 0;
assert(iMeantToDoThis.isNaN);
```


### Lists

아마 모든 프로그래밍 언어에서 가장 흔한 컬렉션은 *배열*이나 정렬된 객체의 그룹일 겁니다.
Dart에서 배열은 [`List`][] 객체로 존재하며, 대부분의 사람들이 *list*라고 부릅니다.

Dart list 리터럴은
쉼표로 구분된 식 또는 값 목록으로 표시되며,
대괄호('[]')로 둘러싸여 있습니다.
다음은 간단한 Dart list 입니다:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (list-literal)"?>
```dart
var list = [1, 2, 3];
```

{{site.alert.note}}
  Dart는 위의 `list`를 `List<int>` 타입이라고 추정합니다. 정수가 아닌 객체를
  이 list에 추가를 시도하면, analyzer나 런타임이 에러를 발생시킵니다.
  더 많은 정보를 원한다면,
  [타입 추론](/guides/language/type-system#type-inference)
  을 참고하세요.
{{site.alert.end}}

<a name="trailing-comma"></a>
Dart 컬렉션 리터럴의 마지막 아이템 뒤에 쉼표를 추가할 수 있습니다.
이 _trailing comma_ 는 컬렉션에 영향을 미치진 않지만,
복사-붙여넣기 에러 예방을 도와줍니다.

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (trailing-commas)"?>
```dart
var list = [
  'Car',
  'Boat',
  'Plane',
];
```

리스트는 0 부터 시작하는 제로 베이스 인덱싱을 사용하고,
`list.length - 1`가 list의 마지막 인덱스입니다.
`.length` 프로퍼티를 사용하여 list의 길이를 구할 수 있고,
서브스크립트 연산자 (`[]`)를 사용하여 list의 값에 접근할 수 있습니다:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (list-indexing)"?>
```dart
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);

list[1] = 1;
assert(list[1] == 1);
```

컴파일 타임 상수인 리스트를 생성하고 싶다면,
list 리터럴 앞에 `const`를 추가하세요:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (const-list)"?>
```dart
var constantList = const [1, 2, 3];
// constantList[1] = 1; // 이 라인은 에러를 발생시킵니다.
```

<a id="spread-operator"> </a>

Dart는 컬렉션에 여러 값들을 간편하게 삽입해주는
**전개 연산자** (`...`)와 **null-aware 전개 연산자**
를 지원합니다.

예를들면 list의 모든 값들을 다른 list에 삽입하기 위해
전개 연산자(...) 를 사용할 수 있습니다.

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (list-spread)"?>
```dart
var list = [1, 2, 3];
var list2 = [0, ...list];
assert(list2.length == 4);
```

전개 연산자의 오른편 표현식의 값이 null 일 수 있다면,
null-aware 전개 연산자 (`...?`)를 사용하여 예외를 피할 수 있습니다:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (list-null-spread)"?>
```dart
var list2 = [0, ...?list];
assert(list2.length == 1);
```

더 많은 전개 연산자 예제와 정보를 원한다면,
[전개 연산자 제안서][spread proposal]을 참고하세요.

<a id="collection-operators"> </a>
Dart는 조건 (`if`)과 반복 (`for`)을 사용하여
컬렉션을 빌드할 수 있는 **컬렉션 if** 와 **컬렉션 for**
을 제공합니다.

다음은 **컬렉션 if**를 사용하여 3개 또는 4개의 항목이 있는 리스트를 생성한는 예제입니다:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (list-if)"?>
```dart
var nav = ['Home', 'Furniture', 'Plants', if (promoActive) 'Outlet'];
```

다음은 **컬렉션 for**을 사용하여 list 항목을
다른 목록에 추가하기 전에 해당 항목을 조작하는 예제입니다:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (list-for)"?>
```dart
var listOfInts = [1, 2, 3];
var listOfStrings = ['#0', for (var i in listOfInts) '#$i'];
assert(listOfStrings[1] == '#1');
```

컬렉션 `if` 와 `for`에 대한 더 자세한 정보와 예제를 원한다면,
[control flow collections proposal][collections proposal]을 참고하세요.

[collections proposal]: https://github.com/dart-lang/language/blob/master/accepted/2.3/control-flow-collections/feature-specification.md

[spread proposal]: https://github.com/dart-lang/language/blob/master/accepted/2.3/spread-collections/feature-specification.md

List 타입은 리스트를 조작하는 다양하고 간편한 메서드들을 가지고 있습니다.
리스트에 대한 더 많은 정보를 원한다면,
[제네릭](#generics) 그리고
[컬렉션](/guides/libraries/library-tour#collections)을 참고하세요.


### Sets

Dart의 set은 유니크한 항목들로 이루어진 정렬되지 않은 컬렉션입니다.
Dart는 set 리터럴과 [`Set`][] 타입을 지원합니다.

다음은 set 리터럴을 사용하여 Dart의 set을 생성하는 코드입니다:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (set-literal)"?>
```dart
var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};
```

{{site.alert.note}}
  Dart는 위의 `halogens`을 `Set<String>` 타입으로 추정합니다. 알맞지 않는 타입을
  set에 추가하면, analyzer 또는 런타임이 에러를 발생시킵니다.
  더 많은 정보를 원한다면,
  [타입 추론](/guides/language/type-system#type-inference)을
  참고하세요.
{{site.alert.end}}

빈 set을 생성하고 싶다면, 타입 인자 앞에 `{}`을 사용하거나,
`Set` 타입의 변수에 `{}`을 할당하세요:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (set-vs-map)"?>
```dart
var names = <String>{};
// Set<String> names = {}; // 이 코드도 작동합니다.
// var names = {}; // Set이 아닌 map을 생성합니다.
```

{{site.alert.info}}
  **Set 또는 map?** Map 리터럴 문법은 set 리터럴 문법과 유사합니다.
  Map 리터럴을 우선으로 고려하기 때문에, `{}`의 디폴트는 `Map` 타입입니다.
  만약 `{}` 또는 이것이 할당될 변수에 타입을 명시하는 것을 잊었다면,
  Dart는 `Map<dynamic, dynamic>` 타입의 객체를 생성합니다. 
{{site.alert.end}}

`add()` 또는 `addAll()` 메서드를 사용해 존재하는 set에 항목을 추가하세요:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (set-add-items)"?>
```dart
var elements = <String>{};
elements.add('fluorine');
elements.addAll(halogens);
```

Set에 있는 항목들의 수를 알고 싶다면, `.length`를 사용하세요:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (set-length)"?>
```dart
var elements = <String>{};
elements.add('fluorine');
elements.addAll(halogens);
assert(elements.length == 5);
```

컴파일 타임 상수인 set을 생성하고 싶다면,
set 리터럴 앞에 `const`를 추가하세요:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (const-set)"?>
```dart
final constantSet = const {
  'fluorine',
  'chlorine',
  'bromine',
  'iodine',
  'astatine',
};
// constantSet.add('helium'); // 이 라인은 에러를 발생시킵니다.
```

Set은 list 처럼 전개 연산자 (`...` 그리고 `...?`)와
컬렉션 `if` and `for`을 지원합니다.
더 많은 정보를 원한다면,
[list 전개 연산자](#spread-operator) 그리고
[list 컬렉션 연산자](#collection-operators)를 참고하세요.

Set에 대한 더 많은 정보를 원한다면,
[제네릭](#generics) 과
[Sets](/guides/libraries/library-tour#sets)을 참고하세요.

### Maps

일반적으로, map은 key와 value로 구성된 객체입니다.
key와 value 모두 어떤 타입의 객체든 할당이 가능합니다.
각 *key*들은 유일하지만 *value*는 중복될 수 있습니다.
Dart는 map 리터럴과 [`Map`][] 타입으로 map을 지원합니다.

다음은 map 리터럴을 사용하여 Dart의 map을 생성하는 코드입니다.

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (map-literal)"?>
```dart
var gifts = {
  // Key:    Value
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};

var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};
```

{{site.alert.note}}
  Dart는 `gifts`의 타입을 `Map<String, String>`로 그리고
  `nobleGases`의 타입을 `Map<int, String>`로 추정합니다.
  Map에 알맞지 않는 타입의 값을 추가하면, analyzer나 런타임이
  에러를 발생시킵니다. 더 많은 정보를 원한다면,
  [타입 추론](/guides/language/type-system#type-inference)을
  참고하세요.
{{site.alert.end}}

Map 생성자를 사용하여 생성하는 것도 가능합니다:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (map-constructor)"?>
```dart
var gifts = Map<String, String>();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';

var nobleGases = Map<int, String>();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';
```

{{site.alert.note}}
  C# 또는 Java를 할 줄 안다면, `Map()` 대신 `new Map()`을
  기대했을 겁니다. Dart에서 `new` 키워드의 사용은 선택입니다.
  더 자세한 사항은 [생성자 사용하기](#using-constructors)를 참고하세요.
{{site.alert.end}}

서브스크립트 할당 연산자 (`[]=`)를 사용하여
기존의 map에 key-value 쌍을 추가하세요:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (map-add-item)"?>
```dart
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds'; // key-value 쌍 추가
```


서브스크립트 연산자 (`[]`)를 사용하여 map에서 원하는 값에 접근하세요:
Retrieve a value from a map using the subscript operator (`[]`):

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (map-retrieve-item)"?>
```dart
var gifts = {'first': 'partridge'};
assert(gifts['first'] == 'partridge');
```

Map에 존재하지 않는 key로 접근하면, `null`을 반환합니다:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (map-missing-key)"?>
```dart
var gifts = {'first': 'partridge'};
assert(gifts['fifth'] == null);
```

Map에 있는 key-value 쌍의 개수를 알고 싶다면, `.length`을 사용하세요:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (map-length)"?>
```dart
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds';
assert(gifts.length == 2);
```

컴파일 타임 상수인 map을 생성하고 싶다면,
map 리터럴 앞에 `const`를 추가하세요:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (const-map)"?>
```dart
final constantMap = const {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};

// constantMap[2] = 'Helium'; // 이 라인은 에러를 발생시킵니다.
```

Map은 list 처럼 전개 연산자 (`...` 그리고 `...?`)와
컬렉션 `if` 그리고 `for`을 지원합니다.
더 자세한 사항과 예제를 보고 싶다면,
[전개 연산자 제안서][spread proposal] 와
[흐름 제어 컬렉션 제안서][collections proposal]를 참고하세요.

Map에 대한 더 많은 정보를 원한다면,
[제네릭](#generics) 섹션과
라이브러리 투어의
[`Maps` API](/guides/libraries/library-tour#maps)를
참고하세요.

<a id="characters"></a>
### Runes 그리고 grapheme clusters

Dart에서 [runes][]는 문자열의 유니코드 코드 포인트를 나타냅니다.
[characters package][]를 사용하여
[Unicode (extended) grapheme clusters][grapheme clusters]
라고도 부르는 사용자가 인식하는 문자를 보거나 조작할 수 있습니다.

유니코드는 세상의 모든 문자, 숫자, 기호 시스템에 대해 고유한 숫자 값을 정의합니다. 
Dart의 문자열은 UTF-16 코드 단위의 시퀀스이기 때문에
문자열 내에서 유니코드 코드 포인트를 표현하려면 특별한 문법이 필요합니다.
유니코드 코드 포인트를 표현하는 가장 흔한 방법은 `\uXXXX` 형태로 나타내는 것이고,
XXXX는 16진수 4-digit 값입니다. 예를 들면 하트 문자(♥)는 `\u2665`입니다.
4개의 16진수 보다 적거나, 많이 사용하고 싶다면, 중괄호 안에 값을 넣으면 됩니다.
예를 들면 웃는 이모지(😆)는 `\u{1f606}`으로 나타냅니다.

만약 유니코드 문자 각각을 읽고 써야한다면,
characters 패키지에 의해 String에 정의 되어 있는 characters getter를 사용하세요.
반환된 [`Characters`][] 객체는 graphem clusters의 시퀀스로 이루어진 문자열 입니다.
아래는 characters API를 사용한 예제 입니다:

<?code-excerpt "misc/lib/language_tour/characters.dart"?>
```dart
import 'package:characters/characters.dart';

void main() {
  var hi = 'Hi 🇩🇰';
  print(hi);
  print('The end of the string: ${hi.substring(hi.length - 1)}');
  print('The last character: ${hi.characters.last}');
}
```

실행 환경에 따라 출력은 다음과 같습니다:

```terminal
$ dart run bin/main.dart
Hi 🇩🇰
The end of the string: ???
The last character: 🇩🇰
```

문자열 조작을 위한 characters 패키지 사용에 대해 더 자세히 알고 싶다면,
[예제][characters example]와 [API reference][characters API] 를 참고하세요.


### Symbols

[`Symbol`][] 객체는 Dart 프로그램에 선언된 연산자나 식별자를 나타냅니다.
아마 Symbol을 사용할 필요가 없을지도 모릅니다.
하지만 축소(minification)를 수행하면 식별자의 이름은 변경되지만,
식별자의 symbol은 변경되지 않기 때문에 symbol은 이름으로 식별자를 참조하는 API에 매우 유용합니다.

식별자에 대한 symbol를 가져오려면 symbol 리터럴을 사용하면 됩니다.
Symbol 리터럴은 `#` 뒤에 식별자를 위치시키면 됩니다:

```nocode
#radix
#bar
```

{% comment %}
The code from the following excerpt isn't actually what is being shown in the page

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (symbols)"?>
```dart
// MOVE TO library tour?

void main() {
  print(Function.apply(int.parse, ['11']));
  print(Function.apply(int.parse, ['11'], {#radix: 16}));
  print(Function.apply(int.parse, ['11a'], {#onError: handleError}));
  print(Function.apply(
      int.parse, ['11a'], {#radix: 16, #onError: handleError}));
}

int handleError(String source) {
  return 0;
}
```
{% endcomment %}

Symbol 리터럴은 컴파일 타임 상수입니다.


## 함수

Dart는 진정항 객체 지향 언어이므로, 함수도
[Function.][Function API reference]
라는 타입을 가지는 객체로 존재합니다.
이건 함수가 변수나 다른 함수의 인자로 전달할 수 있다는 것을 의미합니다.
또한 함수인 것처럼 Dart 클래스의 인스턴스를 호출할 수 있습니다.
더 자세한 사항을 원한다면, [호출가능한 클래스](#callable-classes)를 참고하세요.

다음은 함수를 구현하는 예제입니다:

<?code-excerpt "misc/lib/language_tour/functions.dart (function)"?>
```dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

비록 Effective Dart에서
[public APIs를 위한 타입 어노테이션](/guides/language/effective-dart/design#do-type-annotate-fields-and-top-level-variables-if-the-type-isnt-obvious)
을 추천하지만, 타입을 생략해도 함수는 제대로 작동합니다:

<?code-excerpt "misc/lib/language_tour/functions.dart (function-omitting-types)"?>
```dart
isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

하나의 표현식만 가지는 함수를 선언 할 때 약칭(shorthand) 문법의 사용이 가능합니다:

<?code-excerpt "misc/lib/language_tour/functions.dart (function-shorthand)"?>
```dart
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```

<code>=> <em>표현식</em></code> 문법은
<code>{ return <em>표현식</em>; }</code>의 약칭입니다.
`=>` 노테이션은 _화살표_ 문법으로도 불립니다.

{{site.alert.note}}
  *문(statement)*이 아닌 오직 *식(expression)*만이 화살표 (=\>)와
  세미콜론 (;) 사이에 올 수 있습니다. 예를 들어, [if 문](#if-and-else)은 불가능하지만,
  [조건식](#conditional-expressions)은 가능합니다.
{{site.alert.end}}

### 매개변수

함수는 *required positional* 매개변수를 얼마든지 가질 수 있습니다.
이 매개변수들은 *named* 매개변수 또는 *optional positional* 매개변수의 뒤에 나올 수 있습니다.
(둘 다 사용하는 것은 불가능합니다.)

{{site.alert.note}}
  [Flutter][] 위젯 생성자 처럼 몇몇 API들은 필수 매개변수에 대해서도
  named 매개 변수만 사용합니다. 다음 섹션에서 자세히 살펴봅시다.
{{site.alert.end}}

함수에 인자를 넘겨줄 때나 함수의 매개변수를 정의 할 때
[trailing commas][]를 사용할 수 있습니다.


#### Named 매개변수

Named 매개변수는 `required`로 표시되지 않는 이상
선택적인 매개변수입니다.

함수를 정의할 때,
<code>{<em>매개변수1</em>, <em>매개변수2</em>, …}</code>
를 사용하여 named 매개변수를 표시하세요.
디폴트 값을 제공하지 않거나
named 매개변수를 `required`로 표시하지 않으면
해당 매개 변수의 타입은 디폴트 값이 `null`이 되므로
nullable로 지정해야 합니다:

<?code-excerpt "misc/lib/language_tour/functions.dart (specify-named-parameters)"?>
```dart
/// [bold] 그리고 [hidden] 플래그 설정 ...
void enableFlags({bool? bold, bool? hidden}) {...}
```

함수를 호출할 때,
<code><em>매개변수이름</em>: <em>값</em></code> 을
사용하여 넘겨줄 named 인자를 특정할 수 있습니다.
예제:

<?code-excerpt "misc/lib/language_tour/functions.dart (use-named-parameters)"?>
```dart
enableFlags(bold: true, hidden: false);
```

<a id="default-parameters"></a>
`Null`이 아닌 값으로 named 매개변수의 디폴트 값을 정의하려면 `=`를 사용하세요.
디폴트 값은 반드시 컴파일 타임 상수로 지정되야합니다.
예제:

<?code-excerpt "misc/lib/language_tour/functions.dart (named-parameter-default-values)"?>
```dart
/// [bold] 그리고 [hidden] 플래그 설정 ...
void enableFlags({bool bold = false, bool hidden = false}) {...}

// bold는 true로 설정됩니다; hidden은 false로 설정됩니다..
enableFlags(bold: true);
```
Named 매개변수를 호출자가 반드시 값을 전달하게 하고 싶다면,
`required`로 어노테이트 하세요:

<?code-excerpt "misc/lib/language_tour/functions.dart (required-named-parameters)" replace="/required/[!$&!]/g"?>
```dart
const Scrollbar({super.key, [!required!] Widget child});
```

`child` 인자 없이 `Scollbar`의 생성을 시도하면,
analyzer가 이슈를 보고합니다.

{{site.alert.note}}
  `required`로 표시된 매개변수는
  여전피 nullable 입니다:

  <?code-excerpt "misc/lib/language_tour/functions.dart (required-named-parameters-nullable)" replace="/Widget\?/[!$&!]/g; /ScrollbarTwo/Scrollbar/g;"?>
  ```dart
  const Scrollbar({super.key, required [!Widget?!] child});
  ```
{{site.alert.end}}

Positional 인자들을 먼저 배치하고 싶을 수 있지만,
Dart는 그것을 요구하지 않습니다.
Dart는 named 인자가 API에 적합할 때,
그것을 인자 목록의 아무 곳에나 배치 할 수 있습니다:

<?code-excerpt "misc/lib/language_tour/functions.dart (named-arguments-anywhere)"?>
```dart
repeat(times: 2, () {
  ...
});
```

#### Optional positional 매개변수

함수 매개변수들의 세트를 `[]`로 감싸는 것은
해당 매개변수들을 optional positional 매개변수로 표시합니다.
디폴트 값을 제공하지 않으면, 매개변수의 디폴트 값이
`null`이 되므로 타입은 반드시 nullable이 되어야 합니다:

<?code-excerpt "misc/test/language_tour/functions_test.dart (optional-positional-parameters)"?>
```dart
String say(String from, String msg, [String? device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}
```

다음은 optional 매개변수 없이 함수를 호출하는 예제입니다:

<?code-excerpt "misc/test/language_tour/functions_test.dart (call-without-optional-param)"?>
```dart
assert(say('Bob', 'Howdy') == 'Bob says Howdy');
```

다음은 3번째 매개변수를 포함하여 함수를 호출하는 예제입니다:

<?code-excerpt "misc/test/language_tour/functions_test.dart (call-with-optional-param)"?>
```dart
assert(say('Bob', 'Howdy', 'smoke signal') ==
    'Bob says Howdy with a smoke signal');
```

`Null`이 아닌 값으로 optional positional 매개변수의 디폴트 값을 정의하려면 `=`를 사용하세요.
디폴트 값은 반드시 컴파일 타임 상수로 지정되야합니다.
예제:

<?code-excerpt "misc/test/language_tour/functions_test.dart (optional-positional-param-default)"?>
```dart
String say(String from, String msg, [String device = 'carrier pigeon']) {
  var result = '$from says $msg with a $device';
  return result;
}

assert(say('Bob', 'Howdy') == 'Bob says Howdy with a carrier pigeon');
```


### main() 함수

모든 앱은 엔트리 포인트 역할을 하는 최상위 `main()` 함수를 반드시 가지고 있어야 합니다.
`main()` 함수는 `void`를 반환하고 optional `List<String>` 매개변수를 인자롤 가집니다.

다음은 `main()` 함수의 예제입니다:

<?code-excerpt "misc/test/samples_test.dart (hello-world)"?>
```dart
void main() {
  print('Hello, World!');
}
```

다음은 인자를 가지는 커맨드 라인 앱의 `main()` 함수 예제입니다:

<?code-excerpt "misc/test/language_tour/functions_test.dart (main-args)"?>
```dart

// 다음과 같이 앱을 실행하세요: dart args.dart 1 test
void main(List<String> arguments) {
  print(arguments);

  assert(arguments.length == 2);
  assert(int.parse(arguments[0]) == 1);
  assert(arguments[1] == 'test');
}
```

커맨드 라인 인자를 정의, 파싱하기 위해
[args library]({{site.pub-pkg}}/args)를 사용해도 됩니다.

### 일급 객체로서의 함수

다음과 같이 다른 함수의 인자로 함수를 넘기는 것이 가능합니다:

<?code-excerpt "misc/lib/language_tour/functions.dart (function-as-param)"?>
```dart
void printElement(int element) {
  print(element);
}

var list = [1, 2, 3];

// printElement를 매개변수로 넘깁니다.
list.forEach(printElement);
```

다음과 같이 변수에 함수를 할당하는 것도 가능합니다:

<?code-excerpt "misc/test/language_tour/functions_test.dart (function-as-var)"?>
```dart
var loudify = (msg) => '!!! ${msg.toUpperCase()} !!!';
assert(loudify('hello') == '!!! HELLO !!!');
```

위 예제에서는 익명 함수를 사용합니다.
익명 함수에 대해서는 다음 섹션에서 살펴봅시다.

### 익명 함수

대부분의 함수들은 `main()`나 `printElement()` 처럼 이름이 있습니다.
하지만 _익명 함수_, _람다_, _클로져_ 같이 이름이 없는 함수가 있습니다.
익명 함수를 변수에 선언해서 컬렉션에 추가하고 제거하는 것도 가능합니다.

괄호 안에 콤마로 분리된 매개변수들, optional 타입 어노테이션,
0개 또는 그 이상의 매개변수 같이 익명 함수가 가지는 특징들이 named 함수와 비슷해보입니다.

다음 코드 블럭은 함수 바디를 포함합니다:

<code>
([[<em>Type</em>] <em>param1</em>[, …]]) { <br>
&nbsp;&nbsp;<em>codeBlock</em>; <br>
}; <br>
</code>

다음 예제는 타입을 명시하지 않은 매개변수 `item`을 가지는 익명 함수를
`map` 함수에 넘기는 예제입니다.
이 함수는 list의 모든 아이템을 순회하며, 각 문자열을 대문자로 변환합니다.
그 다음 `forEach`로 넘겨지는 익명 함수에서,
변환된 문자열과 문자열의 길이를 출력합니다.

<?code-excerpt "misc/test/language_tour/functions_test.dart (anonymous-function)"?>
```dart
const list = ['apples', 'bananas', 'oranges'];
list.map((item) {
  return item.toUpperCase();
}).forEach((item) {
  print('$item: ${item.length}');
});
```

코드를 실행하려면 **Run**을 클릭하세요.

<?code-excerpt "misc/test/language_tour/functions_test.dart (anonymous-function-main)"?>
```dart:run-dartpad:height-400px:ga_id-anonymous_functions
void main() {
  const list = ['apples', 'bananas', 'oranges'];
  list.map((item) {
    return item.toUpperCase();
  }).forEach((item) {
    print('$item: ${item.length}');
  });
}
```

함수가 하나의 표현식이나 반환문을 가진다면,
화살표 노테이션을 사용하여 이를 줄일 수 있습니다.
다음 라인을 DartPad에 붙혀넣은 후 **Run**을 클릭하면,
이것이 기능적으로 동일한지 확인할 수 있습니다.

<?code-excerpt "misc/test/language_tour/functions_test.dart (anon-func)"?>
```dart
list
    .map((item) => item.toUpperCase())
    .forEach((item) => print('$item: ${item.length}'));
```


### 렉시컬 스코프 (lexical scope)

Dart는 `lexically scoped` 언어로
변수의 범위가 코드의 레이아웃에 따라 정적으로 결정된다는 것을 의미합니다.
변수의 범위를 확인하고 싶다면 "중괄호의 끝을 따라가면" 됩니다.

다음은 각 스코프 레벨에 있는 변수를 포함하는 중첩 함수의 예제입니다:

<?code-excerpt "misc/test/language_tour/functions_test.dart (nested-functions)"?>
```dart
bool topLevel = true;

void main() {
  var insideMain = true;

  void myFunction() {
    var insideFunction = true;

    void nestedFunction() {
      var insideNestedFunction = true;

      assert(topLevel);
      assert(insideMain);
      assert(insideFunction);
      assert(insideNestedFunction);
    }
  }
}
```

`nestedFunction()`가 모든 레벨에서 변수를 어떻게 사용할 수 있는지 주목하세요.
최상위 수준까지 모든 수준의 변수 사용이 가능합니다.


### 렉시컬 클로저 (lexical closure)

*클로저* 는 함수가 이것의 원래 스코프의 밖에서 쓰여졌다고 해도,
해당 함수 렉시컬 스코프의 변수에 접근 할 수 있는 함수 객체입니다. 

함수는 주변 스코프에 정의된 변수를 포함합니다.
다음의 예제에서, `makeAdder()`는 `addBy` 변수를 캡쳐합니다.
반환된 함수가 가는 곳 마다, `addBy`를 기억합니다.

<?code-excerpt "misc/test/language_tour/functions_test.dart (function-closure)"?>
```dart
/// [addBy]를 함수의 인자에 더하는 함수를 반환합니다.
Function makeAdder(int addBy) {
  return (int i) => addBy + i;
}

void main() {
  // 2를 더하는 함수를 생성합니다.
  var add2 = makeAdder(2);

  // 4를 더하는 함수를 생성합니다.
  var add4 = makeAdder(4);

  assert(add2(3) == 5);
  assert(add4(3) == 7);
}
```


### 동등성 테스트 함수

다음은 최상위 함수, 정적 메서드, 인스턴스 메서드의 동등성을 확인하는 테스트 코드 입니다:

<?code-excerpt "misc/lib/language_tour/function_equality.dart"?>
```dart
void foo() {} // 최상위 함수

class A {
  static void bar() {} // 정적 메서드
  void baz() {} // 인스턴스 메서드
}

void main() {
  Function x;

  // 최상위 함수를 비교.
  x = foo;
  assert(foo == x);

  // 정적 메서드를 비교.
  x = A.bar;
  assert(A.bar == x);

  // 인스턴스 메서드를 비교.
  var v = A(); // A의 인스턴스 #1
  var w = A(); // B의 인스턴스 #2
  var y = w;
  x = w.baz;

  // 두 클로저들은 같은 인스턴스 (#2)를 참조하므로 동일합니다.
  assert(y.baz == x);

  // 두 클로저들은 다른 인스턴스를 참조하므로 동일하지 않습니다.
  assert(v.baz != w.baz);
}
```


### 반환 값

모든 함수는 값을 반환합니다. 반환 값이 명시되어 있지 않으면,
`return null;`이 암묵적으로 함수의 바디에 추가됩니다.

<?code-excerpt "misc/test/language_tour/functions_test.dart (implicit-return-null)"?>
```dart
foo() {}

assert(foo() == null);
```


## 연산자

Dart는 다음 표의 연산자들을 지원합니다.
표는 Dart 연산자들의 관계에 대한 **근사**인 Dart의 연산자 결합법칙과
[연산자 우선순위](#operator-precedence-example)를 최고에서
최저의 순서로 알려줍니다.
[클래스 멤버로서 연산자](#_operators)를 구현하는 것이 가능합니다.

|-----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------|
| 설명                                     | 연산자                                                                                                                                                                                              | 결합법칙        |
|-----------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------|
| unary postfix                           | <code><em>expr</em>++</code>    <code><em>expr</em>--</code>    `()`    `[]`    `?[]`    `.`    `?.`    `!`                                                                                       | 없음          |
| unary prefix                            | <code>-<em>expr</em></code>    <code>!<em>expr</em></code>    <code>~<em>expr</em></code>    <code>++<em>expr</em></code>    <code>--<em>expr</em></code>      <code>await <em>expr</em></code>    | 없음          |
| multiplicative                          | `*`    `/`    `%`    `~/`                                                                                                                                                                         | 왼쪽          |
| additive                                | `+`    `-`                                                                                                                                                                                        | 왼쪽          |
| shift                                   | `<<`    `>>`    `>>>`                                                                                                                                                                             | 왼쪽          |
| bitwise AND                             | `&`                                                                                                                                                                                               | 왼쪽          |
| bitwise XOR                             | `^`                                                                                                                                                                                               | 왼쪽          |
| bitwise OR                              | `|`                                                                                                                                                                                               | 왼쪽          |
| relational&nbsp;and&nbsp;type&nbsp;test | `>=`    `>`    `<=`    `<`    `as`    `is`    `is!`                                                                                                                                               | 없음          |
| equality                                | `==`    `!=`                                                                                                                                                                                      | 없음          |
| logical AND                             | `&&`                                                                                                                                                                                              | 왼쪽          |
| logical OR                              | `||`                                                                                                                                                                                              | 왼쪽          |
| if null                                 | `??`                                                                                                                                                                                              | 왼쪽          |
| conditional                             | <code><em>expr1</em> ? <em>expr2</em> : <em>expr3</em></code>                                                                                                                                     | 오른쪽         |
| cascade                                 | `..` &nbsp;&nbsp; `?..`                                                                                                                                                                           | 오른쪽         |
| assignment                              | `=`    `*=`    `/=`    `+=`    `-=`    `&=`    `^=`    <em>etc.</em>                                                                                                                              | 오른쪽         |
{:.table .table-striped}

<!-- Authoritative -> 정직한 ? -->
{{site.alert.warning}}
  위의 표는 유용한 가이드로만 사용해야 합니다.
  연산자 우선순위와 결합법칙에 대한 개념은 언어 문법의 사실을 기반으로 한
  근사입니다. [Dart 언어 설명서][]에 정의된 문법에서
  Dart 연산자 관계의 정직한 동작에 대해 확인할 수 있습니다.
{{site.alert.end}}

연산자를 사용할 때는 식을 만듭니다. 다음은 연산자 식의 예제입니다:

<?code-excerpt "misc/test/language_tour/operators_test.dart (expressions)" replace="/,//g"?>
```dart
a++
a + b
a = b
a == b
c ? a : b
a is T
```

<a id="operator-precedence-example"></a>
[연산자 테이블](#operators)에서 각 연산자는 그 뒤에 오는 행의 연산자보다
높은 우선 순위를 가집니다. 예를 들어, 곱셈 연산자 `%`는
등식 연산자 `==` 보다 높은 우선 순위를 가지고 더 먼저 실행됩니다.
그리고 `==`는 논리 AND 연산자인 `&&` 보다 높은 우선 순위를 가집니다.
이런 우선 순위는 다음 두 줄의 라인이 같은 방식으로 실행된다는 것을 의미합니다:

<?code-excerpt "misc/test/language_tour/operators_test.dart (precedence)"?>
```dart
// 괄호는 가독성을 높혀줍니다.
if ((n % i == 0) && (d % i == 0)) ...

// 가독성은 좋지 않지만 위와 동일합니다.
if (n % i == 0 && d % i == 0) ...
```

{{site.alert.warning}}
  두 개의 피연산자를 가지는 연산자는, 맨 왼쪽 피연산자가 메서드를 결정합니다.
  예를 들어, `Vector` 객체와 `Point` 객체로 `aVector + aPoint`를 수행하면,
  `Vector`의 덧셈 (`+`)을 실행합니다.
{{site.alert.end}}


### 산술 연산자

Dart는 아래 표와 같이 일반적인 산술 연산자를 지원합니다.

|-----------------------------+-------------------------------------------|
| 연산자                        | 의미                                       |
|-----------------------------+-------------------------------------------|
| `+`                         | 더하기
| `-`                         | 빼기
| <code>-<em>expr</em></code> | 부정(negation)으로도 부르는 단항 빼기 (식 부호 역순)
| `*`                         | 곱하기
| `/`                         | 나누기
| `~/`                        | 정수를 반환하는 나누기
| `%`                         | 정수 나눗셈의 나머지를 반환 (modulo)
{:.table .table-striped}

Example:

<?code-excerpt "misc/test/language_tour/operators_test.dart (arithmetic)"?>
```dart
assert(2 + 3 == 5);
assert(2 - 3 == -1);
assert(2 * 3 == 6);
assert(5 / 2 == 2.5); // 결과는 double 타입
assert(5 ~/ 2 == 2); // 결과는 int 타입
assert(5 % 2 == 1); // 나머지

assert('5/2 = ${5 ~/ 2} r ${5 % 2}' == '5/2 = 2 r 1');
```

Dart는 prefix, postfix 증가 및 감소 연산자를 지원합니다.

|-----------------------------+-------------------------------------------|
| 연산자                        | 의미                                       |
|-----------------------------+-------------------------------------------|
| <code>++<em>var</em></code> | <code><em>var</em> = <em>var</em> + 1</code> (식의 값은 <code><em>var</em> + 1</code>)
| <code><em>var</em>++</code> | <code><em>var</em> = <em>var</em> + 1</code> (식의 값은 <code><em>var</em></code>)
| <code>--<em>var</em></code> | <code><em>var</em> = <em>var</em> - 1</code> (식의 값은 <code><em>var</em> - 1</code>)
| <code><em>var</em>--</code> | <code><em>var</em> = <em>var</em> - 1</code> (식의 값은 <code><em>var</em></code>)
{:.table .table-striped}

예제:

<?code-excerpt "misc/test/language_tour/operators_test.dart (increment-decrement)"?>
```dart
int a;
int b;

a = 0;
b = ++a; // b에 a의 값을 할당하기 전에 a를 증가시킵니다.
assert(a == b); // 1 == 1

a = 0;
b = a++; // b에 a의 값을 할당한 후에 a를 증가시킵니다.
assert(a != b); // 1 != 0

a = 0;
b = --a; // b에 a의 값을 할당하기 전에 a를 감소시킵니다.
assert(a == b); // -1 == -1

a = 0;
b = a--; // b에 a의 값을 할당한 후에 a를 감소시킵니다.
assert(a != b); // -1 != 0
```


### 동등, 관계 연산자

다음 표는 동등 및 관계 연산자의 뜻을 나열합니다.

|-----------+-------------------------------------------|
| 연산자      | 의미                                       |
|-----------+-------------------------------------------|
| `==`      |       동등; 아래 문단을 확인하세요
| `!=`      |       동등하지 않음
| `>`       |       큼
| `<`       |       작음
| `>=`      |       크거나 같음
| `<=`      |       작거나 같음
{:.table .table-striped}

두 객체 x와 y가 동일한 것인지 확인하고 싶다면, `==` 연산자를 사용하세요.
(드물게 두 객체가 정확하게 같은 것인지 확인하고 싶다면 [identical()][] 함수를 대신 사용하세요.)
`==` 연산자는 다음과 같이 작동합니다:

1.  *x* 또는 *y* 가 null 일 때, 모두 null이라면 true를, 둘 중에 하나만 null이라면
    false를 반환합니다.

2.  *y* 를 인자로 사용하여 *x*에서 `==` 메서드를 호출한 결과를 반환합니다.
    (`==` 같은 연산자들은 첫 번째 피연산자에서 호출되는 메서드입니다.
    더 자세한 사항은 [연산자](#_operators)를 참고하세요.)

다음은 각 동등 및 관계 연산자의 사용 예제입니다:

<?code-excerpt "misc/test/language_tour/operators_test.dart (relational)"?>
```dart
assert(2 == 2);
assert(2 != 3);
assert(3 > 2);
assert(2 < 3);
assert(3 >= 3);
assert(2 <= 3);
```


### 타입 테스트 연산자

`as`, `is`, 및 `is!` 연산자는 런타임에 타입을 간단하게 확인 할 수 있습니다.

|-----------+-------------------------------------------|
| 연산자      | 의미                                       |
|-----------+-------------------------------------------|
| `as`      | 타입캐스트 ([라이브러리 prefixes](#specifying-a-library-prefix)를 특정할 때도 사용)
| `is`      | 특정된 타입을 가지는 객체라면 True
| `is!`     | 특정된 타입을 가지는 객체가 아니라면 True
{:.table .table-striped}

`obj` 가 `T` 로 특정된 인터페이스의 구현체라면 `obj is T` 의 결과는 true 입니다.
예를 들어, `obj is Object?`는 항상 true 입니다.

어떤 객체가 캐스팅을 원하는 타입인 경우에만 `as` 연산자를 사용해여 캐스팅 할 수 있습니다. 예제:

<?code-excerpt "misc/lib/language_tour/classes/employee.dart (emp as Person)"?>
```dart
(employee as Person).firstName = 'Bob';
```

만약 객체가 타입 `T`라는 것을 확실하지 못한다면, 객체를 사용하기 전에 `is T`로 타입을 확인하세요.
<?code-excerpt "misc/lib/language_tour/classes/employee.dart (emp is Person)"?>
```dart
if (employee is Person) {
  // 타입 확인
  employee.firstName = 'Bob';
}
```

{{site.alert.note}}
  위의 두 코드는 같지 않습니다. 만약 `employee` 가 null이거나 `Person`이 아니라면,
  첫 번째 코드는 예외가 발생하지만, 두 번째는 그렇지 않습니다.
{{site.alert.end}}

### 할당 연산자

앞서 봤다시피, `=` 연산자를 사용해 값을 할당할 수 있습니다.
할당을 받는 변수가 null 일 때만 할당하고 싶다면, `??=` 연산자를 사용하면 됩니다.

<?code-excerpt "misc/test/language_tour/operators_test.dart (assignment)"?>
```dart
// a에 value를 할당합니다
a = value;
// b가 null이라면 value를 할당하고, 그렇지 않으면 그대로 둡니다.
b ??= value;
```

`+=` 같은 복합(compound) 할당 연산자는 할당과 연산을 결합합니다.

| `=`  | `*=`  | `%=`  | `>>>=` | `^=`
| `+=` | `/=`  | `<<=` | `&=`   | `|=`
| `-=` | `~/=` | `>>=`
{:.table}

복합 할당 연산자는 다음과 같이 작동합니다:

|-----------+----------------------+-----------------------|
|           | 복합 할당               | 동일한 식               |
|-----------+----------------------+-----------------------|
|**연산자 <em>op</em>:** | <code>a <em>op</em>= b</code> | <code>a = a <em>op</em> b</code>
|**예제:**                     |`a += b`                       | `a = a + b`
{:.table}

다음 예제에서 할당 및 복합 할당 연산자를 사용합니다:

<?code-excerpt "misc/test/language_tour/operators_test.dart (op-assign)"?>
```dart
var a = 2; // = 을 사용하여 할당
a *= 3; // 할당 및 곱셈: a = a * 3
assert(a == 6);
```


### 논리 연산자

논리 연산자를 사용하여 boolean 식을 반전하거나 결합하는 것이 가능합니다.

|-----------------------------+-------------------------------------------|
| 연산자                        | 의미                                       |
|-----------------------------+-------------------------------------------|
| <code>!<em>expr</em></code> | 뒤따르는 식을 반전합니다 (false -> true, true -> false)
| `||`                        | 논리 OR
| `&&`                        | 논리 AND
{:.table .table-striped}

다음은 논리 연산자를 사용하는 예제입니다:

<?code-excerpt "misc/lib/language_tour/operators.dart (op-logical)"?>
```dart
if (!done && (col == 0 || col == 3)) {
  // ...Do something...
}
```


### 비트 단위 그리고 쉬프트 연산자

Dart에서는 숫자를 이루는 각각의 비트를 조작하는 것이 가능합니다.
주로 비트 단위 및 쉬프트 연산자는 정수와 함께 사용됩니다.

|-----------------------------+-------------------------------------------|
| 연산자                        | 의미                                       |
|-----------------------------+-------------------------------------------|
| `&`                         | AND
| `|`                         | OR
| `^`                         | XOR
| <code>~<em>expr</em></code> | 단항 비트 단위 보수 (0s -> 1s; 1s -> 0s)
| `<<`                        | Shift left
| `>>`                        | Shift right
| `>>>`                       | Unsigned shift right
{:.table .table-striped}

다음은 비트 단위 및 쉬프트 연산자를 사용하는 예제입니다:

<?code-excerpt "misc/test/language_tour/operators_test.dart (op-bitwise)"?>
```dart
final value = 0x22;
final bitmask = 0x0f;

assert((value & bitmask) == 0x02); // AND
assert((value & ~bitmask) == 0x20); // AND NOT
assert((value | bitmask) == 0x2f); // OR
assert((value ^ bitmask) == 0x2d); // XOR
assert((value << 4) == 0x220); // Shift left
assert((value >> 4) == 0x02); // Shift right
assert((value >>> 4) == 0x02); // Unsigned shift right
assert((-value >> 4) == -0x03); // Shift right
assert((-value >>> 4) > 0); // Unsigned shift right
```

{{site.alert.version-note}}
  _Triple-shift_ 또는 _unsigned shift_ 로 불리는 `>>>` 연산자는
  적어도 2.14의 [language version][]이 필요합니다.
{{site.alert.end}}


### 조건 표현식

Dart에는 [if-else](#if-and-else)문을 간결하게 표현 할 수 있는 두 개의 연산자가 있습니다:

<code><em>조건</em> ? <em>표현식1</em> : <em>표현식2</em></code>
: 만약 _condition_ 이 참이라면, _표현식1_ 의 값을 반환하고, 아니라면 _표현식2_ 의 값을 반환합니다.

<code><em>표현식1</em> ?? <em>표현식2</em></code>
: 만약 _표현식1_ 이 null이 아니라면, _표현식1_ 의 값을 반환하고 null이라면,
  _표현식2_ 의 값을 반환합니다.

Boolean 표현식으로 어떤 값을 할당하는 상황이라면, `?` 와 `:` 을 사용해보세요.

<?code-excerpt "misc/lib/language_tour/operators.dart (if-then-else-operator)"?>
```dart
var visibility = isPublic ? 'public' : 'private';
```

Boolean 표현식이 null인지 확인하고 싶다면, `??` 를 사용하세요.

<?code-excerpt "misc/test/language_tour/operators_test.dart (if-null)"?>
```dart
String playerName(String? name) => name ?? 'Guest';
```

위의 예는 다음과 같이 두 개의 방법으로 표현 될 수 있지만, 깔끔하진 않습니다:

<?code-excerpt "misc/test/language_tour/operators_test.dart (if-null-alt)"?>
```dart
// ?: 연산자보다 조금 긴 버젼
String playerName(String? name) => name != null ? name : 'Guest';

// if-else 문을 사용한 훨씬 긴 버젼.
String playerName(String? name) {
  if (name != null) {
    return name;
  } else {
    return 'Guest';
  }
}
```

<a id="cascade"></a>
### Cascade 표기법

Cascades (`..`, `?..`) 는 같은 객체에 대해 연속적인 명령을 적용할 수 있게 해줍니다. 
인스턴스 멤버에 접근하거나 인스턴스 메서드 호출 또한 가능합니다.
이런 기능은 임시 변수를 만드는 과정을 줄이고 코드를 더 유동적으로 만들어 줍니다.

다음 코드를 살펴보세요:

<?code-excerpt "misc/lib/language_tour/cascades.dart (cascade)"?>
```dart
var paint = Paint()
  ..color = Colors.black
  ..strokeCap = StrokeCap.round
  ..strokeWidth = 5.0;
```

생성자 `Paint()`는 `paint` 객체를 반환합니다.
Casade 노테이션 뒤에 오는 코드들은 반환 될 값들을 무시하며,
해당 객체에 대해 작동합니다.

위의 코드는 아래의 코드와 동일합니다:

<?code-excerpt "misc/lib/language_tour/cascades.dart (cascade-expanded)"?>
```dart
var paint = Paint();
paint.color = Colors.black;
paint.strokeCap = StrokeCap.round;
paint.strokeWidth = 5.0;
```

만약 cascade를 사용하려는 객체의 필드가 null 일 수도 있다면,
_null-shorting_ cascade (?..)를 첫 번째 오퍼레이션으로 사용하세요.
`?..` 으로 시작하는 cascade는 뒤에 오는 cascade 오퍼레이션이
null 객체 일 수도 있는 객체에 대해 실행되지 않을 것을 보장합니다.

<?code-excerpt "misc/test/language_tour/browser_test.dart (cascade-operator)"?>
```dart
querySelector('#confirm') // 객체 찾기.
  ?..text = 'Confirm' // 객체의 멤버 사용.
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'))
  ..scrollIntoView();
```

{{site.alert.version-note}}
  `?..` 문법은 적어도 2.12의 [language version][]이 필요합니다.
{{site.alert.end}}

위의 코드는 다음과 동일합니다:

<?code-excerpt "misc/test/language_tour/browser_test.dart (cascade-operator-example-expanded)"?>
```dart
var button = querySelector('#confirm');
button?.text = 'Confirm';
button?.classes.add('important');
button?.onClick.listen((e) => window.alert('Confirmed!'));
button?.scrollIntoView();
```

다음과 같이 중첩 cascade도 가능합니다:

<?code-excerpt "misc/lib/language_tour/operators.dart (nested-cascades)"?>
```dart
final addressBook = (AddressBookBuilder()
      ..name = 'jenny'
      ..email = 'jenny@example.com'
      ..phone = (PhoneNumberBuilder()
            ..number = '415-555-0100'
            ..label = 'home')
          .build())
    .build();
```

실제 객체를 반환하는 함수에 대해서만 cascade를 사용해야 합니다.
다음과 같은 코드는 에러가 발생합니다:

<?code-excerpt "misc/lib/language_tour/operators.dart (cannot-cascade-on-void)" plaster="none"?>
```dart
var sb = StringBuffer();
sb.write('foo')
  ..write('bar'); // 에러: 메서드 'write'는 'void'에 정의되어 있지 않습니다.
```

The `sb.write()` 호출은 void를 반환하고,
`void`에는 cascade를 사용 할 수 없습니다.

{{site.alert.note}}
  엄밀히 말하자면, cascades를 위한 “double dot” 표기는 연산자가 아닙니다.
  이것은 그저 Dart 문법의 일부입니다.
{{site.alert.end}}

### 다른 연산자들

아마 다른 예제에서 아래의 연산자들을 본 경험이 있을 것 입니다:

|----------+------------------------------+--------------------|
| 연산자     | 이름                          | 의미                |
|----------+------------------------------+--------------------|
| `()`     | Function application         | 함수 호출을 나타냅니다.
| `[]`     | Subscript access             | 재정의 할 수 있는 연산자 `[]`의 호출을 나타냅니다; 예제: `fooList[1]`는 `fooList`의 인덱스 `1`에 위치한 요소에 접근하기 위해 정수 `1`을 전달합니다.
| `?[]`    | Conditional subscript access | `[]`와 비슷하지만 맨 왼쪽 피연산자가 null일 수 있습니다; 예제: `fooList[1]`는 `fooList`가 null이 아니라면, `fooList`의 인덱스 `1`에 위치한 요소에 접근하기 위해 정수 `1`을 전달합니다. (만약 null이라면 표현식은 null로 평가됩니다.)
| `.`      | Member access                | 표현식의 프로퍼티를 참조합니다; 예제: `foo.bar`는 표현식 `foo`의 프로퍼티 `bar`를 선택합니다.
| `?.`     | Conditional member access    | `.`와 비슷하지만, 맨 왼쪽 피연산자가 null일 수 있습니다; 예제: `foo?.bar`는 `foo`가 null이 아니라면 표현식 `foo`의 프로퍼티 `bar`를 선택합니다. (만약 null이라면 `foo?.bar`의 값은 null입니다.)
| `!`      | Null assertion operator      | 표현식을 내제된 non-nullable 타입으로 캐스트합니다. 만약 캐스트에 실패하면 런타임 예외를 발생시킵니다; 예제: `foo!.bar`는 `foo`가 null이 아닌지 assert 하고, `foo`가 null이 아니라면 프로퍼티 `bar`를 선택합니다. (만약 null이라면 런타임 예외를 발생시킵니다.)
{:.table .table-striped}

`.`, `?.`, 그리고 `..` 연산자에 대한 더 많은 정보는,
[클래스](#classes)를 참고하세요.


## Control flow statements

You can control the flow of your Dart code using any of the following:

-   `if` and `else`
-   `for` loops
-   `while` and `do`-`while` loops
-   `break` and `continue`
-   `switch` and `case`
-   `assert`

You can also affect the control flow using `try-catch` and `throw`, as
explained in [Exceptions](#exceptions).


### If and else

Dart supports `if` statements with optional `else` statements, as the
next sample shows. Also see [conditional expressions](#conditional-expressions).

<?code-excerpt "misc/lib/language_tour/control_flow.dart (if-else)"?>
```dart
if (isRaining()) {
  you.bringRainCoat();
} else if (isSnowing()) {
  you.wearJacket();
} else {
  car.putTopDown();
}
```

The statement conditions must be expressions
that evaluate to boolean values, nothing else.
See [Booleans](#booleans) for more information.


### For loops

You can iterate with the standard `for` loop. For example:

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (for)"?>
```dart
var message = StringBuffer('Dart is fun');
for (var i = 0; i < 5; i++) {
  message.write('!');
}
```

Closures inside of Dart’s `for` loops capture the _value_ of the index,
avoiding a common pitfall found in JavaScript. For example, consider:

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (for-and-closures)"?>
```dart
var callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks.add(() => print(i));
}

for (final c in callbacks) {
  c();
}
```

The output is `0` and then `1`, as expected. In contrast, the example
would print `2` and then `2` in JavaScript.

If the object that you are iterating over is an Iterable (such as List or Set)
and if you don't need to know the current iteration counter, 
you can use the `for-in` form of [iteration][]:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (collection)"?>
```dart
for (final candidate in candidates) {
  candidate.interview();
}
```

{{site.alert.tip}}
  To practice using `for-in`, follow the
  [Iterable collections codelab](/codelabs/iterables).
{{site.alert.end}}

Iterable classes also have a [forEach()][] method as another option:

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (forEach)"?>
```dart
var collection = [1, 2, 3];
collection.forEach(print); // 1 2 3
```


### While and do-while

A `while` loop evaluates the condition before the loop:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (while)"?>
```dart
while (!isDone()) {
  doSomething();
}
```

A `do`-`while` loop evaluates the condition *after* the loop:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (do-while)"?>
```dart
do {
  printLine();
} while (!atEndOfPage());
```


### Break and continue

Use `break` to stop looping:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (while-break)"?>
```dart
while (true) {
  if (shutDownRequested()) break;
  processIncomingRequests();
}
```

Use `continue` to skip to the next loop iteration:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (for-continue)"?>
```dart
for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue;
  }
  candidate.interview();
}
```

You might write that example differently if you’re using an
[`Iterable`][] such as a list or set:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (where)"?>
```dart
candidates
    .where((c) => c.yearsExperience >= 5)
    .forEach((c) => c.interview());
```


### Switch and case

Switch statements in Dart compare integer, string, or compile-time
constants using `==`. The compared objects must all be instances of the
same class (and not of any of its subtypes), and the class must not
override `==`.
[Enumerated types](#enumerated-types) work well in `switch` statements.

Each non-empty `case` clause ends with a `break` statement, as a rule.
Other valid ways to end a non-empty `case` clause are a `continue`,
`throw`, or `return` statement.

Use a `default` clause to execute code when no `case` clause matches:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (switch)"?>
```dart
var command = 'OPEN';
switch (command) {
  case 'CLOSED':
    executeClosed();
    break;
  case 'PENDING':
    executePending();
    break;
  case 'APPROVED':
    executeApproved();
    break;
  case 'DENIED':
    executeDenied();
    break;
  case 'OPEN':
    executeOpen();
    break;
  default:
    executeUnknown();
}
```

The following example omits the `break` statement in a `case` clause,
thus generating an error:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (switch-break-omitted)" plaster="none"?>
```dart
var command = 'OPEN';
switch (command) {
  case 'OPEN':
    executeOpen();
    // ERROR: Missing break

  case 'CLOSED':
    executeClosed();
    break;
}
```

However, Dart does support empty `case` clauses, allowing a form of
fall-through:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (switch-empty-case)"?>
```dart
var command = 'CLOSED';
switch (command) {
  case 'CLOSED': // Empty case falls through.
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
```

If you really want fall-through, you can use a `continue` statement and
a label:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (switch-continue)"?>
```dart
var command = 'CLOSED';
switch (command) {
  case 'CLOSED':
    executeClosed();
    continue nowClosed;
  // Continues executing at the nowClosed label.

  nowClosed:
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
```

A `case` clause can have local variables, which are visible only inside
the scope of that clause.


### Assert

During development, use an assert 
statement—<code>assert(<em>condition</em>, <em>optionalMessage</em>)</code>;—to
disrupt normal execution if a boolean condition is false. 
You can find examples of assert statements throughout this tour. 
Here are some more:

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (assert)"?>
```dart
// Make sure the variable has a non-null value.
assert(text != null);

// Make sure the value is less than 100.
assert(number < 100);

// Make sure this is an https URL.
assert(urlString.startsWith('https'));
```

To attach a message to an assertion,
add a string as the second argument to `assert`
(optionally with a [trailing comma][trailing commas]):

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (assert-with-message)"?>
```dart
assert(urlString.startsWith('https'),
    'URL ($urlString) should start with "https".');
```

The first argument to `assert` can be any expression that
resolves to a boolean value. If the expression’s value
is true, the assertion succeeds and execution
continues. If it's false, the assertion fails and an exception (an
[`AssertionError`][]) is thrown.

When exactly do assertions work?
That depends on the tools and framework you're using:

* Flutter enables assertions in [debug mode.][Flutter debug mode]
* Development-only tools such as [`webdev serve`][]
  typically enable assertions by default.
* Some tools, such as [`dart run`][] and [`dart compile js`][]
  support assertions through a command-line flag: `--enable-asserts`.

In production code, assertions are ignored, and
the arguments to `assert` aren't evaluated.

[webdev serve]: /tools/webdev#serve
[dart compile js]: /tools/dart-compile#js

## Exceptions

Your Dart code can throw and catch exceptions. Exceptions are errors
indicating that something unexpected happened. If the exception isn’t
caught, the [isolate](#isolates) that raised the exception is suspended,
and typically the isolate and its program are terminated.

In contrast to Java, all of Dart’s exceptions are unchecked exceptions.
Methods don't declare which exceptions they might throw, and you aren't
required to catch any exceptions.

Dart provides [`Exception`][] and [`Error`][]
types, as well as numerous predefined subtypes. You can, of course,
define your own exceptions. However, Dart programs can throw any
non-null object—not just Exception and Error objects—as an exception.

### Throw

Here’s an example of throwing, or *raising*, an exception:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (throw-FormatException)"?>
```dart
throw FormatException('Expected at least 1 section');
```

You can also throw arbitrary objects:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (out-of-llamas)"?>
```dart
throw 'Out of llamas!';
```

{{site.alert.note}}
  Production-quality code usually throws types that implement [`Error`][] or
  [`Exception`][].
{{site.alert.end}}

Because throwing an exception is an expression, you can throw exceptions
in =\> statements, as well as anywhere else that allows expressions:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (throw-is-an-expression)"?>
```dart
void distanceTo(Point other) => throw UnimplementedError();
```


### Catch

Catching, or capturing, an exception stops the exception from
propagating (unless you rethrow the exception).
Catching an exception gives you a chance to handle it:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try)"?>
```dart
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  buyMoreLlamas();
}
```

To handle code that can throw more than one type of exception, you can
specify multiple catch clauses. The first catch clause that matches the
thrown object’s type handles the exception. If the catch clause does not
specify a type, that clause can handle any type of thrown object:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try-catch)"?>
```dart
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // A specific exception
  buyMoreLlamas();
} on Exception catch (e) {
  // Anything else that is an exception
  print('Unknown exception: $e');
} catch (e) {
  // No specified type, handles all
  print('Something really unknown: $e');
}
```

As the preceding code shows, you can use either `on` or `catch` or both.
Use `on` when you need to specify the exception type. Use `catch` when
your exception handler needs the exception object.

You can specify one or two parameters to `catch()`.
The first is the exception that was thrown,
and the second is the stack trace (a [`StackTrace`][] object).

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try-catch-2)" replace="/\(e.*?\)/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
try {
  // ···
} on Exception catch [!(e)!] {
  print('Exception details:\n $e');
} catch [!(e, s)!] {
  print('Exception details:\n $e');
  print('Stack trace:\n $s');
}
{% endprettify %}

To partially handle an exception,
while allowing it to propagate,
use the `rethrow` keyword.

<?code-excerpt "misc/test/language_tour/exceptions_test.dart (rethrow)" replace="/rethrow;/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
void misbehave() {
  try {
    dynamic foo = true;
    print(foo++); // Runtime error
  } catch (e) {
    print('misbehave() partially handled ${e.runtimeType}.');
    [!rethrow;!] // Allow callers to see the exception.
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() finished handling ${e.runtimeType}.');
  }
}
{% endprettify %}


### Finally

To ensure that some code runs whether or not an exception is thrown, use
a `finally` clause. If no `catch` clause matches the exception, the
exception is propagated after the `finally` clause runs:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (finally)"?>
```dart
try {
  breedMoreLlamas();
} finally {
  // Always clean up, even if an exception is thrown.
  cleanLlamaStalls();
}
```

The `finally` clause runs after any matching `catch` clauses:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try-catch-finally)"?>
```dart
try {
  breedMoreLlamas();
} catch (e) {
  print('Error: $e'); // Handle the exception first.
} finally {
  cleanLlamaStalls(); // Then clean up.
}
```

Learn more by reading the
[Exceptions](/guides/libraries/library-tour#exceptions)
section of the library tour.

## Classes

Dart is an object-oriented language with classes and mixin-based
inheritance. Every object is an instance of a class, and all classes
except `Null` descend from [`Object`][].
*Mixin-based inheritance* means that although every class
(except for the [top class][top-and-bottom], `Object?`)
has exactly one superclass, a class body can be reused in
multiple class hierarchies.
[Extension methods](#extension-methods) are a way to
add functionality to a class without changing the class or creating a subclass.


### Using class members

Objects have *members* consisting of functions and data (*methods* and
*instance variables*, respectively). When you call a method, you *invoke*
it on an object: the method has access to that object’s functions and
data.

Use a dot (`.`) to refer to an instance variable or method:

<?code-excerpt "misc/test/language_tour/classes_test.dart (object-members)"?>
```dart
var p = Point(2, 2);

// Get the value of y.
assert(p.y == 2);

// Invoke distanceTo() on p.
double distance = p.distanceTo(Point(4, 4));
```

Use `?.` instead of `.` to avoid an exception
when the leftmost operand is null:

<?code-excerpt "misc/test/language_tour/classes_test.dart (safe-member-access)"?>
```dart
// If p is non-null, set a variable equal to its y value.
var a = p?.y;
```


### Using constructors

You can create an object using a *constructor*.
Constructor names can be either <code><em>ClassName</em></code> or
<code><em>ClassName</em>.<em>identifier</em></code>. For example,
the following code creates `Point` objects using the
`Point()` and `Point.fromJson()` constructors:

<?code-excerpt "misc/test/language_tour/classes_test.dart (object-creation)" replace="/ as .*?;/;/g"?>
```dart
var p1 = Point(2, 2);
var p2 = Point.fromJson({'x': 1, 'y': 2});
```

The following code has the same effect, but
uses the optional `new` keyword before the constructor name:

<?code-excerpt "misc/test/language_tour/classes_test.dart (object-creation-new)" replace="/ as .*?;/;/g"?>
```dart
var p1 = new Point(2, 2);
var p2 = new Point.fromJson({'x': 1, 'y': 2});
```

Some classes provide [constant constructors](#constant-constructors).
To create a compile-time constant using a constant constructor,
put the `const` keyword before the constructor name:

<?code-excerpt "misc/test/language_tour/classes_test.dart (const)"?>
```dart
var p = const ImmutablePoint(2, 2);
```

Constructing two identical compile-time constants results in a single,
canonical instance:

<?code-excerpt "misc/test/language_tour/classes_test.dart (identical)"?>
```dart
var a = const ImmutablePoint(1, 1);
var b = const ImmutablePoint(1, 1);

assert(identical(a, b)); // They are the same instance!
```

Within a _constant context_, you can omit the `const` before a constructor
or literal. For example, look at this code, which creates a const map:

<?code-excerpt "misc/test/language_tour/classes_test.dart (const-context-withconst)" replace="/pointAndLine1/pointAndLine/g"?>
```dart
// Lots of const keywords here.
const pointAndLine = const {
  'point': const [const ImmutablePoint(0, 0)],
  'line': const [const ImmutablePoint(1, 10), const ImmutablePoint(-2, 11)],
};
```

You can omit all but the first use of the `const` keyword:

<?code-excerpt "misc/test/language_tour/classes_test.dart (const-context-noconst)" replace="/pointAndLine2/pointAndLine/g"?>
```dart
// Only one const, which establishes the constant context.
const pointAndLine = {
  'point': [ImmutablePoint(0, 0)],
  'line': [ImmutablePoint(1, 10), ImmutablePoint(-2, 11)],
};
```

If a constant constructor is outside of a constant context
and is invoked without `const`,
it creates a **non-constant object**:

<?code-excerpt "misc/test/language_tour/classes_test.dart (nonconst-const-constructor)"?>
```dart
var a = const ImmutablePoint(1, 1); // Creates a constant
var b = ImmutablePoint(1, 1); // Does NOT create a constant

assert(!identical(a, b)); // NOT the same instance!
```


### Getting an object's type

To get an object's type at runtime,
you can use the `Object` property `runtimeType`,
which returns a [`Type`][] object.

<?code-excerpt "misc/test/language_tour/classes_test.dart (runtimeType)"?>
```dart
print('The type of a is ${a.runtimeType}');
```

{{site.alert.warn}}
  Use a [type test operator][] rather than `runtimeType`
  to test an object's type.
  In production environments, the test `object is Type` is more stable
  than the test `object.runtimeType == Type`.
{{site.alert.end}}

Up to here, you've seen how to _use_ classes.
The rest of this section shows how to _implement_ classes.


### Instance variables

Here’s how you declare instance variables:

<?code-excerpt "misc/lib/language_tour/classes/point_with_main.dart (class)"?>
```dart
class Point {
  double? x; // Declare instance variable x, initially null.
  double? y; // Declare y, initially null.
  double z = 0; // Declare z, initially 0.
}
```

All uninitialized instance variables have the value `null`.

All instance variables generate an implicit *getter* method.
Non-final instance variables and
`late final` instance variables without initializers also generate
an implicit *setter* method. For details,
see [Getters and setters](#getters-and-setters).

If you initialize a non-`late` instance variable where it's declared,
the value is set when the instance is created,
which is before the constructor and its initializer list execute.

<?code-excerpt "misc/lib/language_tour/classes/point_with_main.dart (class+main)" replace="/(double .*?;).*/$1/g" plaster="none"?>
```dart
class Point {
  double? x; // Declare instance variable x, initially null.
  double? y; // Declare y, initially null.
}

void main() {
  var point = Point();
  point.x = 4; // Use the setter method for x.
  assert(point.x == 4); // Use the getter method for x.
  assert(point.y == null); // Values default to null.
}
```

Instance variables can be `final`,
in which case they must be set exactly once.
Initialize `final`, non-`late` instance variables
at declaration,
using a constructor parameter, or
using a constructor's [initializer list](#initializer-list):

<?code-excerpt "misc/lib/effective_dart/usage_good.dart (field-init-at-decl)"?>
```dart
class ProfileMark {
  final String name;
  final DateTime start = DateTime.now();

  ProfileMark(this.name);
  ProfileMark.unnamed() : name = '';
}
```

If you need to assign the value of a `final` instance variable
after the constructor body starts, you can use one of the following:

* Use a [factory constructor](#factory-constructors).
* Use `late final`, but [_be careful:_][late-final-ivar]
  a `late final` without an initializer adds a setter to the API.


### Constructors

Declare a constructor by creating a function with the same name as its
class (plus, optionally, an additional identifier as described in
[Named constructors](#named-constructors)).
The most common form of constructor, the generative constructor, creates
a new instance of a class:

<?code-excerpt "misc/lib/language_tour/classes/point_alt.dart (constructor-long-way)" plaster="none"?>
```dart
class Point {
  double x = 0;
  double y = 0;

  Point(double x, double y) {
    // See initializing formal parameters for a better way
    // to initialize instance variables.
    this.x = x;
    this.y = y;
  }
}
```

The `this` keyword refers to the current instance.

{{site.alert.note}}
  Use `this` only when there is a name conflict. 
  Otherwise, Dart style omits the `this`.
{{site.alert.end}}


#### Initializing formal parameters

The pattern of assigning a constructor argument to an instance variable
is so common, 
Dart has initializing formal parameters to make it easy.

Initializing parameters can also be used to initialize
non-nullable or `final` instance variables,
which both must be initialized or provided a default value.

<?code-excerpt "misc/lib/language_tour/classes/point.dart (constructor-initializer)" plaster="none"?>
```dart
class Point {
  final double x;
  final double y;

  // Sets the x and y instance variables
  // before the constructor body runs.
  Point(this.x, this.y);
}
```

The variables introduced by the initializing formals
are implicitly final and only in scope of the initializer list.


#### Default constructors

If you don’t declare a constructor, a default constructor is provided
for you. The default constructor has no arguments and invokes the
no-argument constructor in the superclass.


#### Constructors aren’t inherited

Subclasses don’t inherit constructors from their superclass. A subclass
that declares no constructors has only the default (no argument, no
name) constructor.


#### Named constructors

Use a named constructor to implement multiple constructors for a class
or to provide extra clarity:

<?code-excerpt "misc/lib/language_tour/classes/point.dart (named-constructor)" replace="/Point\.\S*/[!$&!]/g" plaster="none"?>
{% prettify dart tag=pre+code %}
const double xOrigin = 0;
const double yOrigin = 0;

class Point {
  final double x;
  final double y;

  Point(this.x, this.y);

  // Named constructor
  [!Point.origin()!]
      : x = xOrigin,
        y = yOrigin;
}
{% endprettify %}

Remember that constructors are not inherited, which means that a
superclass’s named constructor is not inherited by a subclass. If you
want a subclass to be created with a named constructor defined in the
superclass, you must implement that constructor in the subclass.


#### Invoking a non-default superclass constructor

By default, a constructor in a subclass calls the superclass’s unnamed,
no-argument constructor.
The superclass's constructor is called at the beginning of the
constructor body. If an [initializer list](#initializer-list)
is also being used, it executes before the superclass is called.
In summary, the order of execution is as follows:

1. initializer list
1. superclass's no-arg constructor
1. main class's no-arg constructor

If the superclass doesn’t have an unnamed, no-argument constructor,
then you must manually call one of the constructors in the
superclass. Specify the superclass constructor after a colon (`:`), just
before the constructor body (if any).

In the following example, the constructor for the Employee class calls the named
constructor for its superclass, Person. Click **Run** to execute the code.

<?code-excerpt "misc/lib/language_tour/classes/employee.dart (super)" plaster="none"?>
```dart:run-dartpad:height-450px:ga_id-non_default_superclass_constructor
class Person {
  String? firstName;

  Person.fromJson(Map data) {
    print('in Person');
  }
}

class Employee extends Person {
  // Person does not have a default constructor;
  // you must call super.fromJson().
  Employee.fromJson(super.data) : super.fromJson() {
    print('in Employee');
  }
}

void main() {
  var employee = Employee.fromJson({});
  print(employee);
  // Prints:
  // in Person
  // in Employee
  // Instance of 'Employee'
}
```

Because the arguments to the superclass constructor are evaluated before
invoking the constructor, an argument can be an expression such as a
function call:

<?code-excerpt "misc/lib/language_tour/classes/employee.dart (method-then-constructor)"?>
```dart
class Employee extends Person {
  Employee() : super.fromJson(fetchDefaultData());
  // ···
}
```

{{site.alert.warning}}
  Arguments to the superclass constructor don't have access to `this`. For
  example, arguments can call static methods but not instance methods.
{{site.alert.end}}

<a name="super-parameters"></a>
To avoid having to manually pass each parameter
into the super invocation of a constructor,
you can use super-initializer parameters to forward parameters
to the specified or default superclass constructor.
This feature can't be used with redirecting constructors.
Super-initializer parameters have similar syntax and semantics to
[initializing formal parameters](#initializing-formal-parameters):

<?code-excerpt "misc/lib/language_tour/classes/super_initializer_parameters.dart (positional)" plaster="none"?>
```dart
class Vector2d {
  final double x;
  final double y;

  Vector2d(this.x, this.y);
}

class Vector3d extends Vector2d {
  final double z;

  // Forward the x and y parameters to the default super constructor like:
  // Vector3d(final double x, final double y, this.z) : super(x, y);
  Vector3d(super.x, super.y, this.z);
}
```

Super-initializer parameters cannot be positional 
if the super-constructor invocation already has positional arguments,
but they can always be named:

<?code-excerpt "misc/lib/language_tour/classes/super_initializer_parameters.dart (named)" plaster="none"?>
```dart
class Vector2d {
  // ...

  Vector2d.named({required this.x, required this.y});
}

class Vector3d extends Vector2d {
  // ...

  // Forward the y parameter to the named super constructor like:
  // Vector3d.yzPlane({required double y, required this.z})
  //       : super.named(x: 0, y: y);
  Vector3d.yzPlane({required super.y, required this.z}) : super.named(x: 0);
}
```

{{site.alert.version-note}}
  Using super-initializer parameters 
  requires a [language version][] of at least 2.17.
  If you're using an earlier language version,
  you must manually pass in all super constructor parameters.
{{site.alert.end}}

#### Initializer list

Besides invoking a superclass constructor, you can also initialize
instance variables before the constructor body runs. Separate
initializers with commas.

{% comment %}
[TODO #2950: Maybe change example or point to discussion of ! (in map section?).]
{% endcomment %}

<?code-excerpt "misc/lib/language_tour/classes/point_alt.dart (initializer-list)"?>
```dart
// Initializer list sets instance variables before
// the constructor body runs.
Point.fromJson(Map<String, double> json)
    : x = json['x']!,
      y = json['y']! {
  print('In Point.fromJson(): ($x, $y)');
}
```

{{site.alert.warning}}
  The right-hand side of an initializer doesn't have access to `this`.
{{site.alert.end}}

During development, you can validate inputs by using `assert` in the
initializer list.

<?code-excerpt "misc/lib/language_tour/classes/point_alt.dart (initializer-list-with-assert)" replace="/assert\(.*?\)/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
Point.withAssert(this.x, this.y) : [!assert(x >= 0)!] {
  print('In Point.withAssert(): ($x, $y)');
}
{% endprettify %}

Initializer lists are handy when setting up final fields. The following example
initializes three final fields in an initializer list. Click **Run** to execute
the code.

<?code-excerpt "misc/lib/language_tour/classes/point_with_distance_field.dart"?>
```dart:run-dartpad:height-340px:ga_id-initializer_list
import 'dart:math';

class Point {
  final double x;
  final double y;
  final double distanceFromOrigin;

  Point(double x, double y)
      : x = x,
        y = y,
        distanceFromOrigin = sqrt(x * x + y * y);
}

void main() {
  var p = Point(2, 3);
  print(p.distanceFromOrigin);
}
```


#### Redirecting constructors

Sometimes a constructor’s only purpose is to redirect to another
constructor in the same class. A redirecting constructor’s body is
empty, with the constructor call
(using `this` instead of the class name)
appearing after a colon (:).

<?code-excerpt "misc/lib/language_tour/classes/point_redirecting.dart"?>
```dart
class Point {
  double x, y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(double x) : this(x, 0);
}
```


#### Constant constructors

If your class produces objects that never change, you can make these
objects compile-time constants. To do this, define a `const` constructor
and make sure that all instance variables are `final`.

<?code-excerpt "misc/lib/language_tour/classes/immutable_point.dart"?>
```dart
class ImmutablePoint {
  static const ImmutablePoint origin = ImmutablePoint(0, 0);

  final double x, y;

  const ImmutablePoint(this.x, this.y);
}
```

Constant constructors don't always create constants.
For details, see the section on
[using constructors](#using-constructors).


#### Factory constructors

Use the `factory` keyword when implementing a constructor that doesn’t
always create a new instance of its class. For example, a factory
constructor might return an instance from a cache, or it might
return an instance of a subtype.
Another use case for factory constructors is
initializing a final variable using
logic that can't be handled in the initializer list.

{{site.alert.tip}}
  Another way to handle late initialization of a final variable
  is to [use `late final` (carefully!)][late-final-ivar].
{{site.alert.end}}

In the following example,
the `Logger` factory constructor returns objects from a cache,
and the `Logger.fromJson` factory constructor
initializes a final variable from a JSON object.

<?code-excerpt "misc/lib/language_tour/classes/logger.dart"?>
```dart
class Logger {
  final String name;
  bool mute = false;

  // _cache is library-private, thanks to
  // the _ in front of its name.
  static final Map<String, Logger> _cache = <String, Logger>{};

  factory Logger(String name) {
    return _cache.putIfAbsent(name, () => Logger._internal(name));
  }

  factory Logger.fromJson(Map<String, Object> json) {
    return Logger(json['name'].toString());
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
```

{{site.alert.note}}
  Factory constructors have no access to `this`.
{{site.alert.end}}

Invoke a factory constructor just like you would any other constructor:

<?code-excerpt "misc/lib/language_tour/classes/logger.dart (logger)"?>
```dart
var logger = Logger('UI');
logger.log('Button clicked');

var logMap = {'name': 'UI'};
var loggerJson = Logger.fromJson(logMap);
```


### Methods

Methods are functions that provide behavior for an object.

#### Instance methods

Instance methods on objects can access instance variables and `this`.
The `distanceTo()` method in the following sample is an example of an
instance method:

<?code-excerpt "misc/lib/language_tour/classes/point.dart (class-with-distanceTo)" plaster="none"?>
```dart
import 'dart:math';

class Point {
  final double x;
  final double y;

  Point(this.x, this.y);

  double distanceTo(Point other) {
    var dx = x - other.x;
    var dy = y - other.y;
    return sqrt(dx * dx + dy * dy);
  }
}
```

#### Operators {#_operators}

Operators are instance methods with special names.
Dart allows you to define operators with the following names:

`<`  | `+`  | `|`  | `>>>`
`>`  | `/`  | `^`  | `[]`
`<=` | `~/` | `&`  | `[]=`
`>=` | `*`  | `<<` | `~`
`-`  | `%`  | `>>` | `==`
{:.table}

{{site.alert.note}}
  You may have noticed that some [operators](#operators), like `!=`, aren't in
  the list of names. That's because they're just syntactic sugar. For example,
  the expression `e1 != e2` is syntactic sugar for `!(e1 == e2)`.
{{site.alert.end}}

{%- comment %}
  Internal note from https://github.com/dart-lang/site-www/pull/2691#discussion_r506184100:
  -  `??`, `&&` and `||` are excluded because they are lazy / short-circuiting operators
  - `!` is probably excluded for historical reasons
{% endcomment %}

An operator declaration is identified using the built-in identifier `operator`.
The following example defines vector 
addition (`+`), subtraction (`-`), and equality (`==`):

<?code-excerpt "misc/lib/language_tour/classes/vector.dart"?>
```dart
class Vector {
  final int x, y;

  Vector(this.x, this.y);

  Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
  Vector operator -(Vector v) => Vector(x - v.x, y - v.y);

  @override
  bool operator ==(Object other) =>
      other is Vector && x == other.x && y == other.y;

  @override
  int get hashCode => Object.hash(x, y);
}

void main() {
  final v = Vector(2, 3);
  final w = Vector(2, 2);

  assert(v + w == Vector(4, 5));
  assert(v - w == Vector(0, 1));
}
```


#### Getters and setters

Getters and setters are special methods that provide read and write
access to an object’s properties. Recall that each instance variable has
an implicit getter, plus a setter if appropriate. You can create
additional properties by implementing getters and setters, using the
`get` and `set` keywords:

<?code-excerpt "misc/lib/language_tour/classes/rectangle.dart"?>
```dart
class Rectangle {
  double left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom.
  double get right => left + width;
  set right(double value) => left = value - width;
  double get bottom => top + height;
  set bottom(double value) => top = value - height;
}

void main() {
  var rect = Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -8);
}
```

With getters and setters, you can start with instance variables, later
wrapping them with methods, all without changing client code.

{{site.alert.note}}
  Operators such as increment (++) work in the expected way, whether or
  not a getter is explicitly defined. To avoid any unexpected side
  effects, the operator calls the getter exactly once, saving its value
  in a temporary variable.
{{site.alert.end}}

#### Abstract methods

Instance, getter, and setter methods can be abstract, defining an
interface but leaving its implementation up to other classes.
Abstract methods can only exist in [abstract classes](#abstract-classes).

To make a method abstract, use a semicolon (;) instead of a method body:

<?code-excerpt "misc/lib/language_tour/classes/doer.dart"?>
```dart
abstract class Doer {
  // Define instance variables and methods...

  void doSomething(); // Define an abstract method.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // Provide an implementation, so the method is not abstract here...
  }
}
```


### Abstract classes

Use the `abstract` modifier to define an *abstract class*—a class that
can’t be instantiated. Abstract classes are useful for defining
interfaces, often with some implementation. If you want your abstract
class to appear to be instantiable, define a [factory
constructor](#factory-constructors).

Abstract classes often have [abstract methods](#abstract-methods).
Here’s an example of declaring an abstract class that has an abstract
method:

<?code-excerpt "misc/lib/language_tour/classes/misc.dart (abstract)"?>
```dart
// This class is declared abstract and thus
// can't be instantiated.
abstract class AbstractContainer {
  // Define constructors, fields, methods...

  void updateChildren(); // Abstract method.
}
```

<a id="interfaces"></a>
### Implicit interfaces

Every class implicitly defines an interface containing all the instance
members of the class and of any interfaces it implements. If you want to
create a class A that supports class B’s API without inheriting B’s
implementation, class A should implement the B interface.

A class implements one or more interfaces by declaring them in an
`implements` clause and then providing the APIs required by the
interfaces. For example:

<?code-excerpt "misc/lib/language_tour/classes/impostor.dart"?>
```dart
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final String _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Impostor implements Person {
  String get _name => '';

  String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(Person('Kathy')));
  print(greetBob(Impostor()));
}
```

Here’s an example of specifying that a class implements multiple
interfaces:

<?code-excerpt "misc/lib/language_tour/classes/misc.dart (point_interfaces)"?>
```dart
class Point implements Comparable, Location {...}
```


### Extending a class

Use `extends` to create a subclass, and `super` to refer to the
superclass:

<?code-excerpt "misc/lib/language_tour/classes/extends.dart" replace="/extends|super/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  // ···
}

class SmartTelevision [!extends!] Television {
  void turnOn() {
    [!super!].turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  // ···
}
{% endprettify %}

For another usage of `extends`, see the discussion of
[parameterized types](#restricting-the-parameterized-type)
in [generics](#generics).

<a name="overridable-operators"></a>

#### Overriding members

Subclasses can override instance methods (including [operators](#_operators)),
getters, and setters.
You can use the `@override` annotation to indicate that you are
intentionally overriding a member:

<?code-excerpt "misc/lib/language_tour/metadata/television.dart (override)" replace="/@override/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
class Television {
  // ···
  set contrast(int value) {...}
}

class SmartTelevision extends Television {
  [!@override!]
  set contrast(num value) {...}
  // ···
}
{% endprettify %}

An overriding method declaration must match
the method (or methods) that it overrides in several ways:

* The return type must be the same type as (or a subtype of)
  the overridden method's return type.
* Argument types must be the same type as (or a supertype of)
  the overridden method's argument types.
  In the preceding example, the `contrast` setter of `SmartTelevision`
  changes the argument type from `int` to a supertype, `num`.
* If the overridden method accepts _n_ positional parameters,
  then the overriding method must also accept _n_ positional parameters.
* A [generic method](#using-generic-methods) can't override a non-generic one,
  and a non-generic method can't override a generic one.

Sometimes you might want to narrow the type of
a method parameter or an instance variable.
This violates the normal rules, and
it's similar to a downcast in that it can cause a type error at runtime.
Still, narrowing the type is possible
if the code can guarantee that a type error won't occur.
In this case, you can use the 
[`covariant` keyword](/guides/language/sound-problems#the-covariant-keyword)
in a parameter declaration.
For details, see the 
[Dart 언어 설명서][].

{{site.alert.warning}}
  If you override `==`, you should also override Object's `hashCode` getter.
  For an example of overriding `==` and `hashCode`, see
  [Implementing map keys](/guides/libraries/library-tour#implementing-map-keys).
{{site.alert.end}}

#### noSuchMethod()

To detect or react whenever code attempts to use a non-existent method or
instance variable, you can override `noSuchMethod()`:

<?code-excerpt "misc/lib/language_tour/classes/no_such_method.dart" replace="/noSuchMethod(?!,)/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
class A {
  // Unless you override noSuchMethod, using a
  // non-existent member results in a NoSuchMethodError.
  @override
  void [!noSuchMethod!](Invocation invocation) {
    print('You tried to use a non-existent member: '
        '${invocation.memberName}');
  }
}
{% endprettify %}

You **can't invoke** an unimplemented method unless
**one** of the following is true:

* The receiver has the static type `dynamic`.

* The receiver has a static type that
defines the unimplemented method (abstract is OK),
and the dynamic type of the receiver has an implementation of `noSuchMethod()`
that's different from the one in class `Object`.

For more information, see the informal
[noSuchMethod forwarding specification.](https://github.com/dart-lang/language/blob/master/archive/feature-specifications/nosuchmethod-forwarding.md)

### Extension methods

Extension methods are a way to add functionality to existing libraries.
You might use extension methods without even knowing it.
For example, when you use code completion in an IDE,
it suggests extension methods alongside regular methods.

Here's an example of using an extension method on `String`
named `parseInt()` that's defined in `string_apis.dart`:

```dart
import 'string_apis.dart';
...
print('42'.padLeft(5)); // Use a String method.
print('42'.parseInt()); // Use an extension method.
```

For details of using and implementing extension methods, see the
[extension methods page][].

<a id="enums"></a>
### Enumerated types

Enumerated types, often called _enumerations_ or _enums_,
are a special kind of class used to represent
a fixed number of constant values.

{{site.alert.note}}
  All enums automatically extend the [`Enum`][] class.
  They are also sealed, 
  meaning they cannot be subclassed, implemented, mixed in, 
  or otherwise explicitly instantiated.

  Abstract classes and mixins can explicitly implement or extend `Enum`,
  but unless they are then implemented by or mixed into an enum declaration,
  no objects can actually implement the type of that class or mixin.
{{site.alert.end}}

#### Declaring simple enums

To declare a simple enumerated type,
use the `enum` keyword and
list the values you want to be enumerated:

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (enum)"?>
```dart
enum Color { red, green, blue }
```

{{site.alert.tip}}
  You can also use [trailing commas][] when declaring an enumerated type
  to help prevent copy-paste errors.
{{site.alert.end}}

#### Declaring enhanced enums

Dart also allows enum declarations to declare classes
with fields, methods, and const constructors
which are limited to a fixed number of known constant instances.

To declare an enhanced enum,
follow a syntax similar to normal [classes](#classes),
but with a few extra requirements:

* Instance variables must be `final`, 
  including those added by [mixins](#mixins).
* All [generative constructors](#constant-constructors) must be constant.
* [Factory constructors](#factory-constructors) can only return
  one of the fixed, known enum instances.
* No other class can be extended as [`Enum`] is automatically extended.
* There cannot be overrides for `index`, `hashCode`, the equality operator `==`.
* A member named `values` cannot be declared in an enum,
  as it would conflict with the automatically generated static `values` getter.
* All instances of the enum must be declared
  in the beginning of the declaration,
  and there must be at least one instance declared.

Here is an example that declares an enhanced enum
with multiple instances, instance variables,
a getter, and an implemented interface:

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (enhanced)"?>
```dart
enum Vehicle implements Comparable<Vehicle> {
  car(tires: 4, passengers: 5, carbonPerKilometer: 400),
  bus(tires: 6, passengers: 50, carbonPerKilometer: 800),
  bicycle(tires: 2, passengers: 1, carbonPerKilometer: 0);

  const Vehicle({
    required this.tires,
    required this.passengers,
    required this.carbonPerKilometer,
  });

  final int tires;
  final int passengers;
  final int carbonPerKilometer;

  int get carbonFootprint => (carbonPerKilometer / passengers).round();

  @override
  int compareTo(Vehicle other) => carbonFootprint - other.carbonFootprint;
}
```

To learn more about declaring enhanced enums,
see the section on [Classes](#classes).

{{site.alert.version-note}}
  Enhanced enums require a [language version][] of at least 2.17.
{{site.alert.end}}

#### Using enums

Access the enumerated values like
any other [static variable](#static-variables):

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (access)"?>
```dart
final favoriteColor = Color.blue;
if (favoriteColor == Color.blue) {
  print('Your favorite color is blue!');
}
```

Each value in an enum has an `index` getter,
which returns the zero-based position of the value in the enum declaration.
For example, the first value has index 0,
and the second value has index 1.

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (index)"?>
```dart
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
```

To get a list of all the enumerated values,
use the enum's `values` constant.

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (values)"?>
```dart
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
```

You can use enums in [switch statements](#switch-and-case), and
you'll get a warning if you don't handle all of the enum's values:

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (switch)"?>
```dart
var aColor = Color.blue;

switch (aColor) {
  case Color.red:
    print('Red as roses!');
    break;
  case Color.green:
    print('Green as grass!');
    break;
  default: // Without this, you see a WARNING.
    print(aColor); // 'Color.blue'
}
```

If you need to access the name of an enumerated value,
such as `'blue'` from `Color.blue`,
use the `.name` property:

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (name)"?>
```dart
print(Color.blue.name); // 'blue'
```


<a id="mixins"></a>
### Adding features to a class: mixins

Mixins are a way of reusing a class's code in multiple class
hierarchies.

To _use_ a mixin, use the `with` keyword followed by one or more mixin
names. The following example shows two classes that use mixins:

<?code-excerpt "misc/lib/language_tour/classes/orchestra.dart (Musician and Maestro)" replace="/(with.*) \{/[!$1!] {/g"?>
{% prettify dart tag=pre+code %}
class Musician extends Performer [!with Musical!] {
  // ···
}

class Maestro extends Person [!with Musical, Aggressive, Demented!] {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
{% endprettify %}

To _implement_ a mixin, create a class that extends Object and
declares no constructors.
Unless you want your mixin to be usable as a regular class,
use the `mixin` keyword instead of `class`.
For example:

<?code-excerpt "misc/lib/language_tour/classes/orchestra.dart (Musical)"?>
```dart
mixin Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
```

Sometimes you might want to restrict the types that can use a mixin.
For example, the mixin might depend on being able to invoke a method
that the mixin doesn't define.
As the following example shows, you can restrict a mixin's use
by using the `on` keyword to specify the required superclass:

<?code-excerpt "misc/lib/language_tour/classes/orchestra.dart (mixin-on)" plaster="none" replace="/on Musician2/[!on Musician!]/g" ?>
```dart
class Musician {
  // ...
}
mixin MusicalPerformer [!on Musician!] {
  // ...
}
class SingerDancer extends Musician with MusicalPerformer {
  // ...
}
```

In the preceding code,
only classes that extend or implement the `Musician` class
can use the mixin `MusicalPerformer`.
Because `SingerDancer` extends `Musician`,
`SingerDancer` can mix in `MusicalPerformer`.

### Class variables and methods

Use the `static` keyword to implement class-wide variables and methods.

#### Static variables

Static variables (class variables) are useful for class-wide state and
constants:

<?code-excerpt "misc/lib/language_tour/classes/misc.dart (static-field)"?>
```dart
class Queue {
  static const initialCapacity = 16;
  // ···
}

void main() {
  assert(Queue.initialCapacity == 16);
}
```

Static variables aren’t initialized until they’re used.

{{site.alert.note}}
  This page follows the [style guide
  recommendation](/guides/language/effective-dart/style#identifiers)
  of preferring `lowerCamelCase` for constant names.
{{site.alert.end}}

#### Static methods

Static methods (class methods) don't operate on an instance, and thus
don't have access to `this`.
They do, however, have access to static variables.
As the following example shows,
you invoke static methods directly on a class:

<?code-excerpt "misc/lib/language_tour/classes/point_with_distance_method.dart"?>
```dart
import 'dart:math';

class Point {
  double x, y;
  Point(this.x, this.y);

  static double distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}

void main() {
  var a = Point(2, 2);
  var b = Point(4, 4);
  var distance = Point.distanceBetween(a, b);
  assert(2.8 < distance && distance < 2.9);
  print(distance);
}
```

{{site.alert.note}}
  Consider using top-level functions, instead of static methods, for
  common or widely used utilities and functionality.
{{site.alert.end}}

You can use static methods as compile-time constants. For example, you
can pass a static method as a parameter to a constant constructor.


## Generics

If you look at the API documentation for the basic array type,
[`List`][], you’ll see that the
type is actually `List<E>`. The \<...\> notation marks List as a
*generic* (or *parameterized*) type—a type that has formal type
parameters. [By convention][], most type variables have single-letter names,
such as E, T, S, K, and V.

[By convention]: /guides/language/effective-dart/design#do-follow-existing-mnemonic-conventions-when-naming-type-parameters


### Why use generics?

Generics are often required for type safety, but they have more benefits
than just allowing your code to run:

* Properly specifying generic types results in better generated code.
* You can use generics to reduce code duplication.

If you intend for a list to contain only strings, you can
declare it as `List<String>` (read that as “list of string”). That way
you, your fellow programmers, and your tools can detect that assigning a non-string to
the list is probably a mistake. Here’s an example:

{:.fails-sa}
```dart
var names = <String>[];
names.addAll(['Seth', 'Kathy', 'Lars']);
names.add(42); // Error
```

Another reason for using generics is to reduce code duplication.
Generics let you share a single interface and implementation between
many types, while still taking advantage of static
analysis. For example, say you create an interface for
caching an object:

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (ObjectCache)"?>
```dart
abstract class ObjectCache {
  Object getByKey(String key);
  void setByKey(String key, Object value);
}
```

You discover that you want a string-specific version of this interface,
so you create another interface:

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (StringCache)"?>
```dart
abstract class StringCache {
  String getByKey(String key);
  void setByKey(String key, String value);
}
```

Later, you decide you want a number-specific version of this
interface... You get the idea.

Generic types can save you the trouble of creating all these interfaces.
Instead, you can create a single interface that takes a type parameter:

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (Cache)"?>
```dart
abstract class Cache<T> {
  T getByKey(String key);
  void setByKey(String key, T value);
}
```

In this code, T is the stand-in type. It’s a placeholder that you can
think of as a type that a developer will define later.


### Using collection literals

List, set, and map literals can be parameterized. Parameterized literals are
just like the literals you’ve already seen, except that you add
<code>&lt;<em>type</em>></code> (for lists and sets) or
<code>&lt;<em>keyType</em>, <em>valueType</em>></code> (for maps)
before the opening bracket. Here is an example of using typed literals:

<?code-excerpt "misc/lib/language_tour/generics/misc.dart (collection-literals)"?>
```dart
var names = <String>['Seth', 'Kathy', 'Lars'];
var uniqueNames = <String>{'Seth', 'Kathy', 'Lars'};
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};
```


### Using parameterized types with constructors

To specify one or more types when using a constructor, put the types in
angle brackets (`<...>`) just after the class name. For example:

<?code-excerpt "misc/test/language_tour/generics_test.dart (constructor-1)"?>
```dart
var nameSet = Set<String>.from(names);
```

{% comment %}[TODO #2950: It isn't idiomatic to use a constructor for an empty Map. Change to a class that doesn't have literal support.]{% endcomment %}

The following code creates a map that has integer keys and values of
type View:

<?code-excerpt "misc/test/language_tour/generics_test.dart (constructor-2)"?>
```dart
var views = Map<int, View>();
```


### Generic collections and the types they contain

Dart generic types are *reified*, which means that they carry their type
information around at runtime. For example, you can test the type of a
collection:

<?code-excerpt "misc/test/language_tour/generics_test.dart (generic-collections)"?>
```dart
var names = <String>[];
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true
```

{{site.alert.note}}
  In contrast, generics in Java use *erasure*, which means that generic
  type parameters are removed at runtime. In Java, you can test whether
  an object is a List, but you can’t test whether it’s a `List<String>`.
{{site.alert.end}}


### Restricting the parameterized type

When implementing a generic type,
you might want to limit the types that can be provided as arguments,
so that the argument must be a subtype of a particular type.
You can do this using `extends`.

A common use case is ensuring that a type is non-nullable
by making it a subtype of `Object`
(instead of the default, [`Object?`][top-and-bottom]).

<?code-excerpt "misc/lib/language_tour/generics/misc.dart (non-nullable)"?>
```dart
class Foo<T extends Object> {
  // Any type provided to Foo for T must be non-nullable.
}
```

You can use `extends` with other types besides `Object`.
Here's an example of extending `SomeBaseClass`,
so that members of `SomeBaseClass` can be called on objects of type `T`:

<?code-excerpt "misc/lib/language_tour/generics/base_class.dart" replace="/extends SomeBaseClass(?=. \{)/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
class Foo<T [!extends SomeBaseClass!]> {
  // Implementation goes here...
  String toString() => "Instance of 'Foo<$T>'";
}

class Extender extends SomeBaseClass {...}
{% endprettify %}

It's OK to use `SomeBaseClass` or any of its subtypes as the generic argument:

<?code-excerpt "misc/test/language_tour/generics_test.dart (SomeBaseClass-ok)" replace="/Foo.\w+./[!$&!]/g"?>
{% prettify dart tag=pre+code %}
var someBaseClassFoo = [!Foo<SomeBaseClass>!]();
var extenderFoo = [!Foo<Extender>!]();
{% endprettify %}

It's also OK to specify no generic argument:

<?code-excerpt "misc/test/language_tour/generics_test.dart (no-generic-arg-ok)" replace="/expect\((.*?).toString\(\), .(.*?).\);/print($1); \/\/ $2/g"?>
```dart
var foo = Foo();
print(foo); // Instance of 'Foo<SomeBaseClass>'
```

Specifying any non-`SomeBaseClass` type results in an error:

{:.fails-sa}
{% prettify dart tag=pre+code %}
var foo = [!Foo<Object>!]();
{% endprettify %}


### Using generic methods

Methods and functions also allow type arguments:

<!-- {{site.dartpad}}/a02c53b001977efa4d803109900f21bb -->
<!-- https://gist.github.com/a02c53b001977efa4d803109900f21bb -->
<?code-excerpt "misc/test/language_tour/generics_test.dart (method)" replace="/<T.(?=\()|T/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
[!T!] first[!<T>!](List<[!T!]> ts) {
  // Do some initial work or error checking, then...
  [!T!] tmp = ts[0];
  // Do some additional checking or processing...
  return tmp;
}
{% endprettify %}

Here the generic type parameter on `first` (`<T>`)
allows you to use the type argument `T` in several places:

* In the function's return type (`T`).
* In the type of an argument (`List<T>`).
* In the type of a local variable (`T tmp`).


## Libraries and visibility

The `import` and `library` directives can help you create a
modular and shareable code base. Libraries not only provide APIs, but
are a unit of privacy: identifiers that start with an underscore (`_`)
are visible only inside the library. *Every Dart app is a library*, even
if it doesn’t use a `library` directive.

Libraries can be distributed using [packages](/guides/packages).

{{site.alert.info}}
  If you're curious why Dart uses underscores instead of
  access modifier keywords like `public` or `private`, see
  [SDK issue 33383](https://github.com/dart-lang/sdk/issues/33383).
{{site.alert.end}}


### Using libraries

Use `import` to specify how a namespace from one library is used in the
scope of another library.

For example, Dart web apps generally use the [dart:html][]
library, which they can import like this:

<?code-excerpt "misc/test/language_tour/browser_test.dart (dart-html-import)"?>
```dart
import 'dart:html';
```

The only required argument to `import` is a URI specifying the
library.
For built-in libraries, the URI has the special `dart:` scheme.
For other libraries, you can use a file system path or the `package:`
scheme. The `package:` scheme specifies libraries provided by a package
manager such as the pub tool. For example:

<?code-excerpt "misc/test/language_tour/browser_test.dart (package-import)"?>
```dart
import 'package:test/test.dart';
```

{{site.alert.note}}
  *URI* stands for uniform resource identifier.
  *URLs* (uniform resource locators) are a common kind of URI.
{{site.alert.end}}


#### Specifying a library prefix

If you import two libraries that have conflicting identifiers, then you
can specify a prefix for one or both libraries. For example, if library1
and library2 both have an Element class, then you might have code like
this:

<?code-excerpt "misc/lib/language_tour/libraries/import_as.dart" replace="/(lib\d)\.dart/package:$1\/$&/g"?>
```dart
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// Uses Element from lib1.
Element element1 = Element();

// Uses Element from lib2.
lib2.Element element2 = lib2.Element();
```

#### Importing only part of a library

If you want to use only part of a library, you can selectively import
the library. For example:

<?code-excerpt "misc/lib/language_tour/libraries/show_hide.dart" replace="/(lib\d)\.dart/package:$1\/$&/g"?>
```dart
// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
```

<a id="deferred-loading"></a>
#### Lazily loading a library

_Deferred loading_ (also called _lazy loading_)
allows a web app to load a library on demand,
if and when the library is needed.
Here are some cases when you might use deferred loading:

* To reduce a web app's initial startup time.
* To perform A/B testing—trying out
  alternative implementations of an algorithm, for example.
* To load rarely used functionality, such as optional screens and dialogs.

{{site.alert.warn}}
  **Only `dart compile js` supports deferred loading.**
  Flutter and the Dart VM don't support deferred loading.
  To learn more, see
  [issue #33118](https://github.com/dart-lang/sdk/issues/33118) and
  [issue #27776.](https://github.com/dart-lang/sdk/issues/27776)
{{site.alert.end}}

To lazily load a library, you must first
import it using `deferred as`.

<?code-excerpt "misc/lib/language_tour/libraries/greeter.dart (import)" replace="/hello\.dart/package:greetings\/$&/g"?>
```dart
import 'package:greetings/hello.dart' deferred as hello;
```

When you need the library, invoke
`loadLibrary()` using the library's identifier.

<?code-excerpt "misc/lib/language_tour/libraries/greeter.dart (loadLibrary)"?>
```dart
Future<void> greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
```

In the preceding code,
the `await` keyword pauses execution until the library is loaded.
For more information about `async` and `await`,
see [asynchrony support](#asynchrony-support).

You can invoke `loadLibrary()` multiple times on a library without problems.
The library is loaded only once.

Keep in mind the following when you use deferred loading:

* A deferred library's constants aren't constants in the importing file.
  Remember, these constants don't exist until the deferred library is loaded.
* You can't use types from a deferred library in the importing file.
  Instead, consider moving interface types to a library imported by
  both the deferred library and the importing file.
* Dart implicitly inserts `loadLibrary()` into the namespace that you define
  using <code>deferred as <em>namespace</em></code>.
  The `loadLibrary()` function returns a [`Future`](/guides/libraries/library-tour#future).


### Implementing libraries

See
[Create Library Packages](/guides/libraries/create-library-packages)
for advice on how to implement a library package, including:

* How to organize library source code.
* How to use the `export` directive.
* When to use the `part` directive.
* When to use the `library` directive.
* How to use conditional imports and exports to implement
  a library that supports multiple platforms.


<a id="asynchrony"></a>
## Asynchrony support

Dart libraries are full of functions that
return [`Future`][] or [`Stream`][] objects.
These functions are _asynchronous_:
they return after setting up
a possibly time-consuming operation
(such as I/O),
without waiting for that operation to complete.

The `async` and `await` keywords support asynchronous programming,
letting you write asynchronous code that
looks similar to synchronous code.


<a id="await"></a>
### Handling Futures

When you need the result of a completed Future,
you have two options:

* Use `async` and `await`, as described here and in the
  [asynchronous programming codelab](/codelabs/async-await).
* Use the Future API, as described
  [in the library tour](/guides/libraries/library-tour#future).

Code that uses `async` and `await` is asynchronous,
but it looks a lot like synchronous code.
For example, here's some code that uses `await`
to wait for the result of an asynchronous function:

<?code-excerpt "misc/lib/language_tour/async.dart (await-lookUpVersion)"?>
```dart
await lookUpVersion();
```

To use `await`, code must be in an `async` function—a
function marked as `async`:

<?code-excerpt "misc/lib/language_tour/async.dart (checkVersion)" replace="/async|await/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
Future<void> checkVersion() [!async!] {
  var version = [!await!] lookUpVersion();
  // Do something with version
}
{% endprettify %}

{{site.alert.note}}
  Although an `async` function might perform time-consuming operations, 
  it doesn't wait for those operations. 
  Instead, the `async` function executes only
  until it encounters its first `await` expression.
  Then it returns a `Future` object,
  resuming execution only after the `await` expression completes.
{{site.alert.end}}

Use `try`, `catch`, and `finally` to handle errors and cleanup
in code that uses `await`:

<?code-excerpt "misc/lib/language_tour/async.dart (try-catch)"?>
```dart
try {
  version = await lookUpVersion();
} catch (e) {
  // React to inability to look up the version
}
```

You can use `await` multiple times in an `async` function.
For example, the following code waits three times
for the results of functions:

<?code-excerpt "misc/lib/language_tour/async.dart (repeated-await)"?>
```dart
var entrypoint = await findEntryPoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
```

In <code>await <em>expression</em></code>,
the value of <code><em>expression</em></code> is usually a Future;
if it isn't, then the value is automatically wrapped in a Future.
This Future object indicates a promise to return an object.
The value of <code>await <em>expression</em></code> is that returned object.
The await expression makes execution pause until that object is available.

**If you get a compile-time error when using `await`,
make sure `await` is in an `async` function.**
For example, to use `await` in your app's `main()` function,
the body of `main()` must be marked as `async`:

<?code-excerpt "misc/lib/language_tour/async.dart (main)" replace="/async|await/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
void main() [!async!] {
  checkVersion();
  print('In main: version is ${[!await!] lookUpVersion()}');
}
{% endprettify %}

{{site.alert.note}}
  The preceding example uses an `async` function (`checkVersion()`)
  without waiting for a result—a practice that can cause problems
  if the code assumes that the function has finished executing.
  To avoid this problem,
  use the [unawaited_futures linter rule][].
{{site.alert.end}}

[unawaited_futures linter rule]: /tools/linter-rules#unawaited_futures

For an interactive introduction to using futures, `async`, and `await`,
see the [asynchronous programming codelab](/codelabs/async-await).


<a id="async"></a>
### Declaring async functions

An `async` function is a function whose body is marked with
the `async` modifier.

Adding the `async` keyword to a function makes it return a Future.
For example, consider this synchronous function,
which returns a String:

<?code-excerpt "misc/lib/language_tour/async.dart (sync-lookUpVersion)"?>
```dart
String lookUpVersion() => '1.0.0';
```

If you change it to be an `async` function—for example,
because a future implementation will be time consuming—the
returned value is a Future:

<?code-excerpt "misc/lib/language_tour/async.dart (async-lookUpVersion)"?>
```dart
Future<String> lookUpVersion() async => '1.0.0';
```

Note that the function's body doesn't need to use the Future API.
Dart creates the Future object if necessary.
If your function doesn't return a useful value,
make its return type `Future<void>`.

For an interactive introduction to using futures, `async`, and `await`,
see the [asynchronous programming codelab](/codelabs/async-await).

{% comment %}
TODO #1117: Where else should we cover generalized void?
{% endcomment %}


<a id="await-for"></a>
### Handling Streams

When you need to get values from a Stream,
you have two options:

* Use `async` and an _asynchronous for loop_ (`await for`).
* Use the Stream API, as described
  [in the library tour](/guides/libraries/library-tour#stream).

{{site.alert.note}}
  Before using `await for`, be sure that it makes the code clearer and that you
  really do want to wait for all of the stream's results. For example, you
  usually should **not** use `await for` for UI event listeners, because UI
  frameworks send endless streams of events.
{{site.alert.end}}

An asynchronous for loop has the following form:

<?code-excerpt "misc/lib/language_tour/async.dart (await-for)"?>
```dart
await for (varOrType identifier in expression) {
  // Executes each time the stream emits a value.
}
```

The value of <code><em>expression</em></code> must have type Stream.
Execution proceeds as follows:

1. Wait until the stream emits a value.
2. Execute the body of the for loop,
   with the variable set to that emitted value.
3. Repeat 1 and 2 until the stream is closed.

To stop listening to the stream,
you can use a `break` or `return` statement,
which breaks out of the for loop
and unsubscribes from the stream.

**If you get a compile-time error when implementing an asynchronous for loop,
make sure the `await for` is in an `async` function.**
For example, to use an asynchronous for loop in your app's `main()` function,
the body of `main()` must be marked as `async`:

<?code-excerpt "misc/lib/language_tour/async.dart (number_thinker)" replace="/async|await for/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
void main() [!async!] {
  // ...
  [!await for!] (final request in requestServer) {
    handleRequest(request);
  }
  // ...
}
{% endprettify %}

For more information about asynchronous programming, in general, see the
[dart:async](/guides/libraries/library-tour#dartasync---asynchronous-programming)
section of the library tour.


<a id="generator"></a>
## Generators

When you need to lazily produce a sequence of values,
consider using a _generator function_.
Dart has built-in support for two kinds of generator functions:

* **Synchronous** generator: Returns an [`Iterable`] object.
* **Asynchronous** generator: Returns a [`Stream`] object.

To implement a **synchronous** generator function,
mark the function body as `sync*`,
and use `yield` statements to deliver values:

<?code-excerpt "misc/test/language_tour/async_test.dart (sync-generator)"?>
```dart
Iterable<int> naturalsTo(int n) sync* {
  int k = 0;
  while (k < n) yield k++;
}
```

To implement an **asynchronous** generator function,
mark the function body as `async*`,
and use `yield` statements to deliver values:

<?code-excerpt "misc/test/language_tour/async_test.dart (async-generator)"?>
```dart
Stream<int> asynchronousNaturalsTo(int n) async* {
  int k = 0;
  while (k < n) yield k++;
}
```

If your generator is recursive,
you can improve its performance by using `yield*`:

<?code-excerpt "misc/test/language_tour/async_test.dart (recursive-generator)"?>
```dart
Iterable<int> naturalsDownFrom(int n) sync* {
  if (n > 0) {
    yield n;
    yield* naturalsDownFrom(n - 1);
  }
}
```

## Callable classes

To allow an instance of your Dart class to be called like a function,
implement the `call()` method.

The `call()` method allows any class that defines it to emulate a function.
This method supports the same functionality as normal [functions](#functions)
such as parameters and return types.

In the following example, the `WannabeFunction` class defines a `call()` function
that takes three strings and concatenates them, separating each with a space,
and appending an exclamation. Click **Run** to execute the code.

<?code-excerpt "misc/lib/language_tour/callable_classes.dart"?>
```dart:run-dartpad:height-350px:ga_id-callable_classes
class WannabeFunction {
  String call(String a, String b, String c) => '$a $b $c!';
}

var wf = WannabeFunction();
var out = wf('Hi', 'there,', 'gang');

void main() => print(out);
```

## Isolates

Most computers, even on mobile platforms, have multi-core CPUs.
To take advantage of all those cores, developers traditionally use
shared-memory threads running concurrently. However, shared-state
concurrency is error prone and can lead to complicated code.

Instead of threads, all Dart code runs inside of *isolates*. 
Each Dart isolate uses a single thread of execution and
shares no mutable objects with other isolates.
Spinning up multiple isolates creates multiple threads of execution.
This enables multi-threading without its primary drawback,
[race conditions](https://en.wikipedia.org/wiki/Race_condition#In_software)

For more information, see the following:
* [Concurrency in Dart](/guides/language/concurrency)
* [dart:isolate API reference,][dart:isolate]
  including [Isolate.spawn()][] and
  [TransferableTypedData][]
* [Background parsing][background json] cookbook on the Flutter site
* [Isolate sample app][]

[Isolate.spawn()]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-isolate/Isolate/spawn.html
[TransferableTypedData]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-isolate/TransferableTypedData-class.html
[background json]: {{site.flutter-docs}}/cookbook/networking/background-parsing
[Isolate sample app]: https://github.com/flutter/samples/tree/main/isolate_example


## Typedefs

A type alias—often called a _typedef_ because
it's declared with the keyword `typedef`—is
a concise way to refer to a type.
Here's an example of declaring and using a type alias named `IntList`:

<?code-excerpt "misc/lib/language_tour/typedefs/misc.dart (int-list)"?>
```dart
typedef IntList = List<int>;
IntList il = [1, 2, 3];
```

A type alias can have type parameters:

<?code-excerpt "misc/lib/language_tour/typedefs/misc.dart (list-mapper)"?>
```dart
typedef ListMapper<X> = Map<X, List<X>>;
Map<String, List<String>> m1 = {}; // Verbose.
ListMapper<String> m2 = {}; // Same thing but shorter and clearer.
```

{{site.alert.version-note}}
  Before 2.13, typedefs were restricted to function types.
  Using the new typedefs requires a [language version][] of at least 2.13.
{{site.alert.end}}

We recommend using [inline function types][] instead of typedefs for functions,
in most situations.
However, function typedefs can still be useful:

<?code-excerpt "misc/lib/language_tour/typedefs/misc.dart (compare)"?>
```dart
typedef Compare<T> = int Function(T a, T b);

int sort(int a, int b) => a - b;

void main() {
  assert(sort is Compare<int>); // True!
}
```

[typedef-functions]: /guides/language/effective-dart/design#dont-use-the-legacy-typedef-syntax
[inline function types]: /guides/language/effective-dart/design#prefer-inline-function-types-over-typedefs


## Metadata

Use metadata to give additional information about your code. A metadata
annotation begins with the character `@`, followed by either a reference
to a compile-time constant (such as `deprecated`) or a call to a
constant constructor.

Three annotations are available to all Dart code: 
`@Deprecated`, `@deprecated`, and `@override`. 
For examples of using `@override`,
see [Extending a class](#extending-a-class).
Here’s an example of using the `@Deprecated` annotation:

<?code-excerpt "misc/lib/language_tour/metadata/television.dart (deprecated)" replace="/@Deprecated.*/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
class Television {
  /// Use [turnOn] to turn the power on instead.
  [!@Deprecated('Use turnOn instead')!]
  void activate() {
    turnOn();
  }

  /// Turns the TV's power on.
  void turnOn() {...}
  // ···
}
{% endprettify %}

You can define your own metadata annotations. Here’s an example of
defining a `@Todo` annotation that takes two arguments:

<?code-excerpt "misc/lib/language_tour/metadata/todo.dart"?>
```dart
class Todo {
  final String who;
  final String what;

  const Todo(this.who, this.what);
}
```

And here’s an example of using that `@Todo` annotation:

<?code-excerpt "misc/lib/language_tour/metadata/misc.dart"?>
```dart
@Todo('Dash', 'Implement this function')
void doSomething() {
  print('Do something');
}
```

Metadata can appear before a library, class, typedef, type parameter,
constructor, factory, function, field, parameter, or variable
declaration and before an import or export directive. You can
retrieve metadata at runtime using reflection.


## Comments

Dart supports single-line comments, multi-line comments, and
documentation comments.


### Single-line comments

A single-line comment begins with `//`. Everything between `//` and the
end of line is ignored by the Dart compiler.

<?code-excerpt "misc/lib/language_tour/comments.dart (single-line-comments)"?>
```dart
void main() {
  // TODO: refactor into an AbstractLlamaGreetingFactory?
  print('Welcome to my Llama farm!');
}
```


### Multi-line comments

A multi-line comment begins with `/*` and ends with `*/`. Everything
between `/*` and `*/` is ignored by the Dart compiler (unless the
comment is a documentation comment; see the next section). Multi-line
comments can nest.

<?code-excerpt "misc/lib/language_tour/comments.dart (multi-line-comments)"?>
```dart
void main() {
  /*
   * This is a lot of work. Consider raising chickens.

  Llama larry = Llama();
  larry.feed();
  larry.exercise();
  larry.clean();
   */
}
```


### Documentation comments

Documentation comments are multi-line or single-line comments that begin
with `///` or `/**`. Using `///` on consecutive lines has the same
effect as a multi-line doc comment.

Inside a documentation comment, the analyzer ignores all text
unless it is enclosed in brackets. Using brackets, you can refer to
classes, methods, fields, top-level variables, functions, and
parameters. The names in brackets are resolved in the lexical scope of
the documented program element.

Here is an example of documentation comments with references to other
classes and arguments:

<?code-excerpt "misc/lib/language_tour/comments.dart (doc-comments)"?>
```dart
/// A domesticated South American camelid (Lama glama).
///
/// Andean cultures have used llamas as meat and pack
/// animals since pre-Hispanic times.
///
/// Just like any other animal, llamas need to eat,
/// so don't forget to [feed] them some [Food].
class Llama {
  String? name;

  /// Feeds your llama [food].
  ///
  /// The typical llama eats one bale of hay per week.
  void feed(Food food) {
    // ...
  }

  /// Exercises your llama with an [activity] for
  /// [timeLimit] minutes.
  void exercise(Activity activity, int timeLimit) {
    // ...
  }
}
```

In the class's generated documentation, `[feed]` becomes a link
to the docs for the `feed` method,
and `[Food]` becomes a link to the docs for the `Food` class.

To parse Dart code and generate HTML documentation, you can use Dart's
documentation generation tool, [`dart doc`](/tools/dart-doc).
For an example of generated documentation, see the 
[Dart API documentation.]({{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}) 
For advice on how to structure your comments, see
[Effective Dart: Documentation.](/guides/language/effective-dart/documentation)


## Summary

This page summarized the commonly used features in the Dart language.
More features are being implemented, but we expect that they won’t break
existing code. For more information, see the [Dart 언어 설명서][] and
[Effective Dart](/guides/language/effective-dart).

To learn more about Dart's core libraries, see
[A Tour of the Dart Libraries](/guides/libraries/library-tour).

[`AssertionError`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/AssertionError-class.html
[`Characters`]: {{site.pub-api}}/characters/latest/characters/Characters-class.html
[characters API]: {{site.pub-api}}/characters
[characters example]: {{site.pub-pkg}}/characters/example
[characters package]: {{site.pub-pkg}}/characters
[`dart run`]: /tools/dart-run
[dart:html]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-html
[dart:isolate]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-isolate
[dart:math]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-math
[Dart 언어 설명서]: /guides/language/spec
[dartdevc]: /tools/dartdevc
[const를 중복으로 사용하지 마십시오]: /guides/language/effective-dart/usage#dont-use-const-redundantly
[`double`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/double-class.html
[`Enum`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Enum-class.html
[`Error`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Error-class.html
[`Exception`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Exception-class.html
[extension methods page]: /guides/language/extension-methods
[Flutter]: {{site.flutter}}
[Flutter debug mode]: {{site.flutter-docs}}/testing/debugging#debug-mode-assertions
[forEach()]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Iterable/forEach.html
[Function API reference]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Function-class.html
[`Future`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-async/Future-class.html
[grapheme clusters]: https://unicode.org/reports/tr29/#Grapheme_Cluster_Boundaries
[identical()]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/identical.html
[`int`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/int-class.html
[`Iterable`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Iterable-class.html
[iteration]: /guides/libraries/library-tour#iteration
[js numbers]: https://stackoverflow.com/questions/2802957/number-of-bits-in-javascript-numbers/2803010#2803010
[language version]: /guides/language/evolution#language-versioning
[late-final-ivar]: /guides/language/effective-dart/design#avoid-public-late-final-fields-without-initializers
[`List`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/List-class.html
[`Map`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Map-class.html
[meta]: {{site.pub-pkg}}/meta
[ns]: /null-safety
[ns-enable]: /null-safety#enable-null-safety
[`num`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/num-class.html
[`Object`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Object-class.html
[ObjectVsDynamic]: /guides/language/effective-dart/design#avoid-using-dynamic-unless-you-want-to-disable-static-checking
[runes]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Runes-class.html
[`Set`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Set-class.html
[`StackTrace`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/StackTrace-class.html
[`Stream`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-async/Stream-class.html
[`String`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/String-class.html
[`Symbol`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Symbol-class.html
[top-and-bottom]: /null-safety/understanding-null-safety#top-and-bottom
[trailing commas]: #trailing-comma
[type test operator]: #type-test-operators
[`Type`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Type-class.html
[Understanding null safety]: /null-safety/understanding-null-safety
