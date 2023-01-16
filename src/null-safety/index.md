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
더 자세한 정보는, [Dart 3 견고한 null 안전성 트래킹 이슈][]를 참고하세요.
{{site.alert.end}}

[Dart 3 견고한 null 안전성 트래킹 이슈]: https://github.com/dart-lang/sdk/issues/49530

## Introduction through examples

With null safety,
all of the variables in the following code are non-nullable:

```dart
// In null-safe Dart, none of these can ever be null.
var i = 42; // Inferred to be an int.
String name = getFileName();
final b = Foo();
```

<a id="creating-variables"></a>
To indicate that a variable might have the value `null`,
just add `?` to its type declaration:

```dart
int? aNullableInt = null;
```

- To try an interactive example,
  see the [null safety codelab][Null safety codelab].
- To learn more about this topic, see
  [Understanding null safety](/null-safety/understanding-null-safety).


## Null safety principles

Dart null safety support is based on the following three core design principles:

* **Non-nullable by default**. Unless you explicitly tell Dart that a variable
   can be null, it's considered non-nullable. This default was chosen
   after research found that non-null was by far the most common choice in APIs.

* **Incrementally adoptable**. You choose _what_ to migrate to null safety, and _when_.
  You can migrate incrementally, mixing null-safe and
  non-null-safe code in the same project. We provide tools to help you
  with the migration.

* **Fully sound**. Dart’s null safety is sound, which enables compiler optimizations.
  If the type system determines that something isn’t null, then that thing can _never_ be
  null. Once you migrate your whole project
  and its dependencies to null safety, 
  you reap the full benefits of soundness—not only 
  fewer bugs, but smaller binaries and faster execution.


## Enabling/disabling null safety {#enable-null-safety}

You can use sound null safety in Dart 2.12 and Flutter 2.0 or later.
Dart 3 and later will [_only_ support sound null safety][Dart 3 sound null safety tracking issue].

<a id="constraints"></a>
To enable sound null safety, set the
[SDK constraint lower-bound](/tools/pub/pubspec#sdk-constraints)
to a [language version][] of 2.12 or later.
For example, your `pubspec.yaml` file might have the following constraints:

```yaml
environment:
  sdk: '>=2.12.0 <3.0.0'
```

[language version]: /guides/language/evolution#language-versioning

### Migrating an existing package or app {#migrate}

The Dart SDK includes the `dart migrate` tool.
This tool helps you migrate code that supports sound null safety. 
Use `dart migrate` if you wrote Dart code with Dart 2.12 or earlier.

```terminal
$ cd my_app
$ dart migrate
```

To learn how to migrate your code to null safety,
see the [migration guide][].


## Where to learn more

For more information about null safety, see the following resources:

* [Null safety codelab][]
* [Understanding null safety][]
* [Migration guide for existing code][migration guide]
* [Null safety FAQ][]
* [DartPad with null safety]({{site.dartpad}})
* [Null safety sample code][calculate_lix]
* [Null safety tracking issue][110]
* [Dart blog][]

[110]: https://github.com/dart-lang/language/issues/110
[calculate_lix]: https://github.com/dart-lang/samples/tree/master/null_safety/calculate_lix
[`dart create`]: /tools/dart-create
[Dart blog]: https://medium.com/dartlang
[migration guide]: /null-safety/migration-guide
[Null safety FAQ]: /null-safety/faq
[Null safety codelab]: /codelabs/null-safety
[Understanding null safety]: /null-safety/understanding-null-safety

