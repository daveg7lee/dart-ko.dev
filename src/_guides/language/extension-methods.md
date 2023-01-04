---
title: Extension 메서드
description: Learn how to add to existing APIs.
---
Extension 메서드는 이미 존재하는 라이브러리에 기능을 추가합니다.
Extension 메서드가 무엇인지 모르는 상태에서 사용하고 있을 수도 있습니다.
예를 들어, IDE의 코드 완성을 사용할 때,
IDE가 일반적인 메서드와 함께 extension 메서드를 추천합니다.

<iframe width="560" height="315"
  src="https://www.youtube.com/embed/D3j0OSfT9ZI"
  frameborder="0"
  allow="accelerometer; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen>
</iframe>
<em>비디오를 통해 학습하고 싶다면 위의 영상을 참고하세요.</em>


## 개요

다른 사람들이 개발한 API를 사용하거나
많이 쓰이지 않는 라이브러리를 구현할 때,
해당 API를 변경하는 것은 실용적이지 않거나 불가능할 때가 있습니다.
하지만 기능을 추가하고 싶다면 어떻게 해야할까요?

예를 들어, 다음과 같이 문자열을 정수로 파싱하는 코드를 생각해봅시다:

```dart
int.parse('42')
```

`String`에 다음과 같은 기능이 있다면 더 짧고 쉽게 처리가 가능할 것입니다:

```dart
'42'.parseInt()
```

위의 코드를 사용하려면
`String` 클래스의 extension이 포함된 라이브러리를 import하면 됩니다:

<?code-excerpt "extension_methods/lib/string_extensions/usage_simple_extension.dart (basic)" replace="/  print/print/g"?>
```dart
import 'string_apis.dart';
// ···
print('42'.parseInt()); // Extension 메서드 사용.
```

Extension은 메서드뿐만 아니라,
getter, setter, 연산자 같은 멤버에도 사용이 가능합니다.
또한, extension은 이름을 가지고 있어서 API 충돌이 발생할 때 유용합니다.
다음은 문자열에 사용 가능한 `NumberParsing`이라는 extension을 사용해,
extension 메서드 `parseInt()`를 구현하는 예제입니다:

<?code-excerpt "extension_methods/lib/string_extensions/string_apis.dart (parseInt)"?>
```dart
extension NumberParsing on String {
  int parseInt() {
    return int.parse(this);
  }
  // ···
}
```
<div class="prettify-filename">lib/string_apis.dart</div>

다음 섹션은 extension 메서드의 _사용법_,
그 섹션에서는 extension 메서드의 _구현법_ 을 설명합니다. 


## Extension 메서드 사용법

모든 Dart 코드와 마찬가지로 extension 메서드는 라이브러리 안에 있습니다.
우리는 이미 extension 메서드를 어떻게 사용하는지 배웠습니다.
해당 메서드가 있는 라이브러리를 import하고 일반 메서드처럼 사용하면 됩니다:

<?code-excerpt "extension_methods/lib/string_extensions/usage_simple_extension.dart (import-and-use)" replace="/  print/print/g"?>
```dart
// String에 사용 가능한 extention이 있는 라이브러리를 import 합니다.
import 'string_apis.dart';
// ···
print('42'.padLeft(5)); // String 메서드 사용.
print('42'.parseInt()); // Extension 메서드 사용.
```

Extension 메서드를 사용하기 위해 알아야할 것은 모두 확인했습니다.
That's all you usually need to know to use extension methods.
As you write your code, you might also need to know
how extension methods depend on static types (as opposed to `dynamic`) and
how to resolve [API conflicts](#api-conflicts).

### Static types and dynamic

You can't invoke extension methods on variables of type `dynamic`.
For example, the following code results in a runtime exception:

<?code-excerpt "extension_methods/lib/string_extensions/usage_simple_extension.dart (dynamic)" plaster="none" replace="/  \/\/ print/print/g"?>
```dart
dynamic d = '2';
print(d.parseInt()); // Runtime exception: NoSuchMethodError
```

Extension methods _do_ work with Dart's type inference.
The following code is fine because
the variable `v` is inferred to have type `String`:

<?code-excerpt "extension_methods/lib/string_extensions/usage_simple_extension.dart (var)"?>
```dart
var v = '2';
print(v.parseInt()); // Output: 2
```

The reason that `dynamic` doesn't work is that
extension methods are resolved against the static type of the receiver.
Because extension methods are resolved statically,
they're as fast as calling a static function.

For more information about static types and `dynamic`, see
[The Dart type system](/guides/language/type-system).

### API conflicts

If an extension member conflicts with
an interface or with another extension member,
then you have a few options.

One option is changing how you import the conflicting extension,
using `show` or `hide` to limit the exposed API:

<?code-excerpt "extension_methods/lib/string_extensions/usage_import.dart" replace="/  //g"?>
```dart
// Defines the String extension method parseInt().
import 'string_apis.dart';

// Also defines parseInt(), but hiding NumberParsing2
// hides that extension method.
import 'string_apis_2.dart' hide NumberParsing2;

// ···
// Uses the parseInt() defined in 'string_apis.dart'.
print('42'.parseInt());
```

Another option is applying the extension explicitly,
which results in code that looks as if the extension is a wrapper class:

<?code-excerpt "extension_methods/lib/string_extensions/usage_explicit.dart" replace="/  //g"?>
```dart
// Both libraries define extensions on String that contain parseInt(),
// and the extensions have different names.
import 'string_apis.dart'; // Contains NumberParsing extension.
import 'string_apis_2.dart'; // Contains NumberParsing2 extension.

// ···
// print('42'.parseInt()); // Doesn't work.
print(NumberParsing('42').parseInt());
print(NumberParsing2('42').parseInt());
```

If both extensions have the same name,
then you might need to import using a prefix:

<?code-excerpt "extension_methods/lib/string_extensions/usage_prefix.dart" replace="/  //g"?>
```dart
// Both libraries define extensions named NumberParsing
// that contain the extension method parseInt(). One NumberParsing
// extension (in 'string_apis_3.dart') also defines parseNum().
import 'string_apis.dart';
import 'string_apis_3.dart' as rad;

// ···
// print('42'.parseInt()); // Doesn't work.

// Use the ParseNumbers extension from string_apis.dart.
print(NumberParsing('42').parseInt());

// Use the ParseNumbers extension from string_apis_3.dart.
print(rad.NumberParsing('42').parseInt());

// Only string_apis_3.dart has parseNum().
print('42'.parseNum());
```

As the example shows,
you can invoke extension methods implicitly even if you import using a prefix.
The only time you need to use the prefix is
to avoid a name conflict when invoking an extension explicitly.


## Implementing extension methods

Use the following syntax to create an extension:

```
extension <extension name> on <type> {
  (<member definition>)*
}
```

For example, here's how you might implement an extension on the `String` class:

<?code-excerpt "extension_methods/lib/string_extensions/string_apis.dart"?>
```dart
extension NumberParsing on String {
  int parseInt() {
    return int.parse(this);
  }

  double parseDouble() {
    return double.parse(this);
  }
}
```
<div class="prettify-filename">lib/string_apis.dart</div>

To create a local extension that's visible only in
the library where it's declared,
either omit the extension name or give it a name
that starts with an underscore (`_`).

The members of the extension can be methods, getters, setters, operators.
Extensions can also have static fields and static helper methods.

## Implementing generic extensions

Extensions can have generic type parameters.
For example, here's some code that extends the built-in `List<T>` type
with a getter, an operator, and a method:

<?code-excerpt "extension_methods/lib/fancylist.dart"?>
```dart
extension MyFancyList<T> on List<T> {
  int get doubleLength => length * 2;
  List<T> operator -() => reversed.toList();
  List<List<T>> split(int at) => [sublist(0, at), sublist(at)];
}
```

The type `T` is bound based on the static type of the list that
the methods are called on.
{% comment %}
TODO (https://github.com/dart-lang/site-www/issues/2171):
Add more info about generic extensions. 
For example, in the following code, `T` is `PENDING` because PENDING:

[PENDING: example]

[PENDING: Explain why it matters in normal usage.]
{% endcomment %}

## Resources

For more information about extension methods, see the following:

* [Article: Dart Extension Methods Fundamentals][article]
* [Feature specification][specification]
* [Extension methods sample][sample]

{% comment %}
* Video
* 2.7 blog post?
* Release notes?
* Examples?
{% endcomment %}

[specification]: https://github.com/dart-lang/language/blob/master/accepted/2.7/static-extension-methods/feature-specification.md#dart-static-extension-methods-design

[article]: https://medium.com/dartlang/extension-methods-2d466cd8b308

[sample]: https://github.com/dart-lang/samples/tree/master/extension_methods
