---
title: 견고한 null 안전성
description: Dart의 null 안전성 기능에 대해 살펴봅니다.
---

Dart 언어는 견고한 null 안전성을 가지고 있습니다.

Dart 3는 항상 null 안전성을 준수합니다. Dart 2.x 버전에서는
[pubspec 설정](#enable-null-safety)을 통해 null 안전성을 활성화할 수 있습니다.

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

- [Null 안전성 codelab][Null 안전성 codelab]에서 예제를 체험해보세요.
- Null 안전성에 대해 더 자세히 알고 싶다면,
  [null 안전성의 이해](/null-safety/understanding-null-safety)를 참고하세요.


## Null 안전성 원칙

Dart의 null 안전성 지원은 다음 세 가지 핵심 디자인 원칙을 기반으로 합니다:

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


## Dart 3와 null 안전성

2023년 중반에 출시될 예정인 Dart 3은 항상 견고한 null 안전성을 준수합니다.
Dart 3는 null 안전성을 지키지 않은 코드의 실행을 방지합니다.

Null 안전성을 준수하지 않고 개발된 패키지들은 종속성을 해결할 때 문제가 발생합니다: 

```terminal
$ dart pub get

Because pkg1 doesn't support null safety, version solving failed.
The lower bound of "sdk: '>=2.9.0 <3.0.0'" must be 2.12.0 or higher to enable null safety.
```

Null 안전성을 준수하지 않는 라이브러리는 분석, 컴파일 에러를 발생시킵니다:

```terminal
$ dart analyze .
Analyzing ....                         0.6s

  error • lib/pkg1.dart:1:1 • The language version must be >=2.12.0. 
  Try removing the language version override and migrating the code.
  • illegal_language_version_override
```

```terminal
$ dart run bin/my_app.dart
../pkg1/lib/pkg1.dart:1:1: Error: Library doesn't support null safety.
// @dart=2.9
^^^^^^^^^^^^
```

이 문제를 해결하
pub.dev에서 설치한 패키지의 [null 안전성 버전](/null-safety/migration-guide#check-dependency-status)을
확인하고, 모든 소스 코드가 견고한 null 안전성을 지키도록 [마이그레이션](#migrate)하세요.

2023년 1월 말에 **Dart 3 알파**가 Dart dev 채널과 Flutter master 채널에서 사용 가능합니다;
자세한 사항은 [다운로드 페이지][]를 참고하세요.
Dart 3 상호운용을 위해 해당 릴리즈를 사용한 코드의 테스트를 추천합니다:

```terminal
$ dart --version                     # 3.0.0-151.0.dev 이상의 버전이 설치되어 있어야 합니다
$ dart pub get / flutter pub get     # 문제없이 진행되어야 합니다
$ dart analyze / flutter analyze     # 에러없이 통과해야 합니다
```

`pub get` 단계에서 실패한다면, [종속성 상태][]를 참고하세요.
`analyze` 단계에서 실패한다면, 애널라이저가 표시하는 문제를 코드에서 해결하세요.

[다운로드 페이지]: /get-dart/archive#dart-3-alpha
[종속성 상태]: /null-safety/migration-guide#종속-상태-확인

### Dart 3 역호환성

Dart 2.12 이상에서 null 안전성을 준수하도록 마이그레이션된 패키지와 앱은
Dart 3과 역호환될 가능성이 높습니다. 특히, SDK의 제약의 하한이 2.12.0 이상인
패키지의 경우 상한이 3.0.0 미만 버전으로 제한된 경우에도 pub으로 해결할 수 있습니다.
예를 들어, 다음과 같은 SDK 제약 조건을 가지는 패키지는 Dart 3.x SDK로 리졸브 할 수 있습니다:

```yaml
environment:
  sdk: '>=2.14.0 <3.0.0'
```

이를 통해 개발자들은 2.12 null 안전성으로 마이그레이션된 패키지와 함께 Dart 3 견고한 null 안전성을
2차적인 마이그레이션 없이 사용할 수 있습니다. 이것은 Dart 3 주요 변경 사항에 의존하지 않는
코드에만 적용이 가능하다는 것을 알아두세요:

* 여러개의 코어 라이브러리 API가 제거되었습니다; 자세한 사항은
  GitHub issues [#34233][] 그리고 [#49529][]를 참고하세요.
* 디폴트 매개 변수 값에 대한 이전 언어 문법([#2357][])의 지원이 중단되었습니다.


## Dart 2.x와 null 안전성 {#enable-null-safety}

Dart 2.12부터 2.19까지, null 안전성은 pubspec의 설정 옵션이었습니다.
Null 안전성은 Dart 2.12 이전의 SDK 버전에서는 사용이 불가능합니다.

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

Null 안전성을 지원하지 않는 코드는 null 안전성을 준수하도록 마이그레이션할 수 있습니다.
Dart 버전 2.12 ~ 2.19에 포함되어 있는 `dart migrate` 툴의 사용을 권장합니다.

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
* [기존의 코드를 위한 마이그레이션 가이드][마이그레이션 가이드]
* [Null 안전성 FAQ][]
* [Null 안전성 코드 샘플][calculate_lix]

[calculate_lix]: https://github.com/dart-lang/samples/tree/master/null_safety/calculate_lix
[마이그레이션 가이드]: /null-safety/migration-guide
[Null 안전성 FAQ]: /null-safety/faq
[Null 안전성 codelab]: /codelabs/null-safety
[Null 안전성의 이해]: /null-safety/understanding-null-safety
[#34233]: https://github.com/dart-lang/sdk/issues/34233
[#49529]: https://github.com/dart-lang/sdk/issues/49529
[#2357]: https://github.com/dart-lang/language/issues/2357
