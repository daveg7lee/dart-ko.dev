---
title: 견고한 null 안전성
description: Dart의 null 안전성 기능에 대해 살펴봅니다.
---

Dart 언어는 견고한 null 안전성을 가지고 있습니다.

Null 안전성은 `null`로 설정된 변수에 예기치 않게 액세스하는 오류를 방지합니다.
예를 들어, 정수를 기대하는 메서드가 `null`을 넘겨 받는다면, 런타임 에러가 발생합니다.
이러한 역참조 에러는 디버깅하기 어렵습니다.

견고한 null 안전성을 사용하면 변수는 디폴트로 null이 될 수 없습니다:
절대 `null`로 할당될 수 없고, 선언된 타입의 값(예: `int i=42`)을 할당받아야만 합니다.
변수를 Null이 될 수 있는 타입으로 선언하는 것도 가능합니다(예: `int? i`).
이런 식으로 선언하면 해당 변수는 `null` *또는* 정의된 타입의 값을 가질 수 있습니다.

견고한 null 안전성은 null일 수 없는 변수가 null이 아닌 값으로 초기화 되지 않았거나
`null`이 할당되는 경우를 표시하여 잠재적인 **런타임 에러**를 *편집 시* 분석 에러로 변환합니다.
이런 기능을 사용하여 앱을 개발하는 과정에서 에러를 수정하는 것이 가능합니다.

{{site.alert.warn}}
Dart 2.x SDK에서 프로젝트의 SDK 제한을 설정하여
견고한 null 안전성을 활성화 또는 비활성화할 수 있습니다.
[null 안전성의 활성화/비활성화](#enable-null-safety)을 참고하세요.

2023년에 출시될 예정인 Dart 3은 견고한 null 안전성을 강제합니다. Dart 3는 견고한 null 안전성 없이
코드를 실행할 수 없습니다. Dart 3과 호환하기 위해서 기존의 코드들은 견고한 null 안전성을
[사용](#migrate)해야합니다. 
더 자세한 정보는, [Dart 3 견고한 null 안전성 이슈 트래킹][]를 참고하세요.
{{site.alert.end}}

[Dart 3 견고한 null 안전성 이슈 트래킹]: https://github.com/dart-lang/sdk/issues/49530

## 예제

Null 안전성을 사용하면 다음 코드의 모든 변수들은 null이 될 수 없습니다:

```dart
// Null-safe Dart에서 아래의 변수들은 null이 될 수 없습니다.
var i = 42; // int로 추론됨.
String name = getFileName();
final b = Foo();
```

<a id="creating-variables"></a>
어떤 변수가 `null`일 수도 있다는 것을 나타내고 싶다면,
타입 선언에 `?`를 추가하세요:

```dart
int? aNullableInt = null;
```

- [Null 안전성 codelab][Null safety codelab]에 대한 예제를 체험해보세요.
- Null 안전성에 대해 더 자세히 알고 싶다면,
  [null 안전성의 이해](/null-safety/understanding-null-safety)를 참고하세요.


## Null 안전성 원칙

Dart null 안전성 지원은 다음 세 가지 핵심 디자인 원칙을 기반으로 합니다:

* **Null이 될 수 없는 것이 기본입니다**. Dart 코드에 명시적으로 null이 될 수 있다고 표시하는 것이 아니라면,
   기본적으로 null이 될 수 없는 값으로 인지합니다. 현재 API에서 null이 아닌 값이 가장 일반적인 선택임을 발견하였고
   non-nullable을 디폴트로 선택하였습니다.

* **점진적인 적용이 가능합니다**. _어떤_ 코드를 _언제_ null-safe한 코드로 마이그래이트 할지
  임의로 결정할 수 있습니다. 점진적으로 적용하여 동일한 프로젝트에 null-safe한 코드와
  null-safe 하지 않는 코드가 섞여있을 수도 있습니다. 우리는 마이그레이션 툴을 제공합니다.

* **완전히 견고합니다**. Dart의 null 안전성은 신뢰할 수 있으며, 컴파일러 최적화가 잘 되어있습니다.
  타입 시스템이 어떤 변수가 null이 될 수 없다고 결정하면, 해당 변수는 _절대_ null이 될 수 없습니다.
  전체 프로텍트와 프로젝트의 의존성을 null 안전성으로 마이그레이션하면 더 적은 버그, 더 작은 바이너리 파일,
  더 빠른 실행 속도 등 많은 이점을 누릴 수 있습니다.


## Null 안전성 활성화/비활성화 {#enable-null-safety}

견고한 null 안전성은 Dart 2.12와 Flutter 2.0 또는 이후의 버전부터 사용 가능합니다.
Dart 3와 이후의 버전은 [_오직_ 견고한 null 안전성만을 지원할 것입니다.][Dart 3 sound null safety tracking issue].

<a id="constraints"></a>

견고한 null 안전성을 활성화하고 싶다면,
[SDK 최소 버전 제한](/tools/pub/pubspec#sdk-constraints)을
2.12 [언어 버전][] 이상으로 설정해야 합니다.
예를 들어, `pubspec.yaml`은 다음과 같은 제한으로 설정할 수 있습니다:

```yaml
environment:
  sdk: '>=2.12.0 <3.0.0'
```

[언어 버전]: /guides/language/evolution#language-versioning

### 기존의 패키지, 앱 마이그레이션 {#migrate}

Dart SDK는 `dart migrate` 툴을 가지고 있습니다.
이 툴은 코드가 견고한 null 안전성을 지원하도록 마이그레이션 합니다.
Dart 2.12 또는 이전의 버전의 Dart 코드를 작성하였다면, `dart migrate`를 사용하세요.

```terminal
$ cd my_app
$ dart migrate
```

Null-safe한 코드로 마이그레이션하는 방법은
[마이그레이션 가이드][]를 참고하세요.


## 더 많은 자료

다음 리소스를 참고하여 null 안전성에 대해 자세히 학습하세요:

* [Null 안전성 codelab][]
* [Null 안전성의 이해][]
* [기존의 코드를 위한 마이그레이션 가이드][migration guide]
* [Null 안전성 FAQ][]
* [Null 안전성을 지원하는 DartPad]({{site.dartpad}})
* [Null 안전성 코드 샘플][calculate_lix]
* [Null 안전성 이슈 트래킹][110]
* [Dart blog][]

[110]: https://github.com/dart-lang/language/issues/110
[calculate_lix]: https://github.com/dart-lang/samples/tree/master/null_safety/calculate_lix
[`dart create`]: /tools/dart-create
[Dart blog]: https://medium.com/dartlang
[마이그레이션 가이드]: /null-safety/migration-guide
[Null 안전성 FAQ]: /null-safety/faq
[Null 안전성 codelab]: /codelabs/null-safety
[Null 안전성의 이해]: /null-safety/understanding-null-safety
