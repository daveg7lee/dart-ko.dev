---
title: "효과적인 Dart: 사용법"
description: 언어 기능을 사용하여 유지 관리 가능한 코드를 작성하기 위한 가이드라인.
nextpage:
  url: /guides/language/effective-dart/design
  title: 디자인
prevpage:
  url: /guides/language/effective-dart/documentation
  title: 문서화
---
<?code-excerpt replace="/([A-Z]\w*)\d\b/$1/g"?>
<?code-excerpt path-base="misc/lib/effective_dart"?>

당신이 작성한 모든 Dart 코드에 해당 가이드라인을 적용할 수 있습니다.
라이브러리의 *사용자*는 이러한 지침에 대해 일부는 알 필요가 없지만,
*유지관리자*는 반드시 알아야합니다.

## 라이브러리

이 가이드라인은 여러 파일로 작성한 프로그램을 일관적이고 유지보수가 가능한 방향으로
구성하는 데 도움이 될 수 있습니다. 가이드라인을 간결하게 하기 위해 여기에서 "import"를
사용하여 `import`와 `export`를 모두 나타냅니다. 가이드라인은 이 두 가지에 모두 적용됩니다.

### `part of` 명령어에 문자열을 사용하십시오.

많은 Dart 개발자들은 직접 `part`를 사용하지 않습니다. 개발자들은 라이브러리에 파일이 하나만
있을 때 코드를 더 쉽게 읽을 수 있다는 것을 깨달았습니다. `part`를 사용하여 라이브러리의 일부를
다른 파일로 분할하려는 경우, Dart는 다른 파일에 해당 라이브러리의 경로를 표시하도록 요청합니다.
이러한 이유들로, Dart는 `part of` 명령어가 속한 라이브러리의 *이름*을 사용할 수 있도록 허용합니다.
이로 인해 툴이 메인 라이브러리 파일에 해당하는 파일을 직접 찾기가 어려지고, 해당 부분이 실제로 어떤
라이브러리에 속하는지 모호해질 수 있습니다.

권장하는 현대 문법은 다른 명령어에서 사용하듯이 URI 문자열을 사용하여 라이브러리 파일을 직접 지정하는 것입니다.
`my_library.dart`라는 라이브러리가 있다고 하면, 다음과 유사할 것입니다:

<?code-excerpt "my_library.dart"?>
{% prettify dart tag=pre+code %}
library my_library;

part 'some/other/file.dart';
{% endprettify %}

그리고 부분이 되는 파일은 다음과 같을 것입니다:

{:.good}
<?code-excerpt "some/other/file.dart"?>
{% prettify dart tag=pre+code %}
part of '../../my_library.dart';
{% endprettify %}

다음과 같이 작성하지마세요:

{:.bad}
<?code-excerpt "some/other/file_2.dart"?>
{% prettify dart tag=pre+code %}
part of my_library;
{% endprettify %}

### 다른 패키지의 `src` 디렉토리에 있는 라이브러리를 import하지 마십시오.

{% include linter-rule-mention.md rule="implementation_imports" %}

`lib` 아래의 `src` 디렉토리는 패키지가 자체적으로 구현한 private 라이브러리로 [지정됩니다][package guide].
패키지 관리자가 패키지를 버전화하는 방식은 이런 규칙을 고려합니다.
패키지를 손상시키지 않고 관리자는 `src` 디렉토리 아래의 코드를 자유롭게 수정할 수 있습니다.

[package guide]: /tools/pub/package-layout

즉, 다른 패키지의 private 라이브러리를 import 할 경우, 해당 패키지의 사소하고 이론적으로
브레이킹 포인트가 아닌 릴리즈가 당신의 코드를 손상시킬 수 있습니다.


### Import 경로를 `lib` 안팎으로 지정하지 마십시오.

{% include linter-rule-mention.md rule="avoid_relative_lib_imports" %}

`package:` import는 패키지가 컴퓨터의 어디에 저장되어 있든 상관없이
패키지의 `lib` 디렉토리 내부의 라이브러리에 접근할 수 있게 해줍니다.
이것이 가능하려면, `lib`가 다른 파일들과 비교하여 디스크의 상대적인 위치에 존재해야 하는 import가 불가능해야 합니다.
다시 말해, `lib` 내부 파일의 상대적인 import 경로는 `lib` 디렉토리 외부의 파일에
도달하거나 접근할 수 없으며 `lib` 외부의 라이브러리는 `lib` 디렉토리에 도달하기 위해 상대적인 경로를 사용할 수 없습니다.
위의 두 가지를 시도하면 혼란스러운 에러와 프로그램에 손상을 발생시킵니다.

예를 들어, 다음과 같은 디렉토리 구조가 있다고 생각해봅시다:

```text
my_package
└─ lib
   └─ api.dart
   test
   └─ api_test.dart
```

`api_test.dart`가 두가지 방법으로 `api.dart`를 import 합니다:

{:.bad}
{% prettify dart tag=pre+code %}
import 'package:my_package/api.dart';
import '../lib/api.dart';
{% endprettify %}

Dart는 위의 두가지 import가 전혀 관계가 없는 라이브러리에 대한 import로 간주합니다.
Dart와 개발자가 이러한 혼란을 피하기 위해서는, 다음과 같은 두가지 규칙을 지켜야합니다:

* Import 경로에 `/lib/`를 사용하지 마십시오.
* `lib` 디렉토리에서 벗어나기 위해 `../`를 사용하지 마십시오.

패키지의 `lib` 디렉토리에 접근하고 싶다면
(같은 패키지의 `test` 다른 최상위 디렉토리에서도 마찬가지),
`package:` import를 사용하세요.

{:.good}
{% prettify dart tag=pre+code %}
import 'package:my_package/api.dart';
{% endprettify %}

패키지는 `lib` 디렉토리에 접근해서는 안 되며
패키지의 다른 위치에서 라이브러리를 import 해서는 안 됩니다.


### 상대 경로를 사용하여 import하는 것을 지향하십시오.

{% include linter-rule-mention.md rule="prefer_relative_imports" %}

이전 규칙이 적용되지 않을 때, 항상 이 규칙을 따르세요.
Import가 `lib`에 도달하지 *않을* 때는, 상대 경로를 사용하여 import하는 것을 선호하십시오.
더 깔끔합니다.
예를 들어, 다음과 같은 디렉토리 구조가 있다고 생각해봅시다:

```text
my_package
└─ lib
   ├─ src
   │  └─ stuff.dart
   │  └─ utils.dart
   └─ api.dart
   test
   │─ api_test.dart
   └─ test_utils.dart
```

다음과 같이 라이브러리들을 import 해야합니다:

**lib/api.dart:**

{:.good}
{% prettify dart tag=pre+code %}
import 'src/stuff.dart';
import 'src/utils.dart';
{% endprettify %}

**lib/src/utils.dart:**

{:.good}
{% prettify dart tag=pre+code %}
import '../api.dart';
import 'stuff.dart';
{% endprettify %}

**test/api_test.dart:**

{:.good}
{% prettify dart tag=pre+code %}
import 'package:my_package/api.dart'; // 'lib'에 도달하지 못함.

import 'test_utils.dart'; // 'test'라는 같은 폴더에 존재.
{% endprettify %}


## Null


### 변수를 명시적으로 `null`로 초기화하지 마십시오.

{% include linter-rule-mention.md rule="avoid_init_to_null" %}

변수가 non-nullable 타입을 초기화하기 전에 사용한다면, Dart는 컴파일 에러를 리포트합니다.
변수가 nullable이라면, 암묵적으로 `null`로 초기화됩니다.
Dart에는 "초기화되지 않은 메모리"라는 개념이 없고
명시적으로 `null`로 초기화하지 않아도 됩니다.

{:.good}
<?code-excerpt "usage_good.dart (no-null-init)"?>
{% prettify dart tag=pre+code %}
Item? bestDeal(List<Item> cart) {
  Item? bestItem;

  for (final item in cart) {
    if (bestItem == null || item.price < bestItem.price) {
      bestItem = item;
    }
  }

  return bestItem;
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (no-null-init)" replace="/ = null/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
Item? bestDeal(List<Item> cart) {
  Item? bestItem[! = null!];

  for (final item in cart) {
    if (bestItem == null || item.price < bestItem.price) {
      bestItem = item;
    }
  }

  return bestItem;
}
{% endprettify %}


### 디폴트 값으로 `null`을 명시적으로 사용하지 마십시오.

{% include linter-rule-mention.md rule="avoid_init_to_null" %}

디폴트 값이 없는 선택적인 nullable 매개변수를 사용하면,
Dart는 해당 변수의 디폴트 값을 암묵적으로 `null`로 사용하기 때문에
명시할 필요가 없습니다.

{:.good}
<?code-excerpt "usage_good.dart (default-value-null)"?>
{% prettify dart tag=pre+code %}
void error([String? message]) {
  stderr.write(message ?? '\n');
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (default-value-null)"?>
{% prettify dart tag=pre+code %}
void error([String? message = null]) {
  stderr.write(message ?? '\n');
}
{% endprettify %}

<a id="prefer-using--to-convert-null-to-a-boolean-value"></a>

### 항등 연산자에 `true` 또는 `false`을 사용하지 마십시오.

Boolean 리터럴에 대해 *non-nullable*인 boolean 표현식을 평가 할 때,
항등 연산자를 쓰는 것은 불필요합니다.
항등 연산자를 없애는 것이 항상 더 간단하며,
필요하다면 단항 부정 연산자인 `!`를 사용하세요:

{:.good}
<?code-excerpt "usage_good.dart (non-null-boolean-expression)"?>
{% prettify dart tag=pre+code %}
if (nonNullableBool) { ... }

if (!nonNullableBool) { ... }
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (non-null-boolean-expression)"?>
{% prettify dart tag=pre+code %}
if (nonNullableBool == true) { ... }

if (nonNullableBool == false) { ... }
{% endprettify %}

*Nullable*인 boolean 표현식을 평가하고 싶다면,
`??`을 사용하거나 명시적으로 `!= null` 확인을 해야합니다.

{:.good}
<?code-excerpt "usage_good.dart (nullable-boolean-expression)"?>
{% prettify dart tag=pre+code %}
// 값이 null 일 때, 표현식이 false로 평가되도록 하고 싶다면:
if (nullableBool ?? false) { ... }

// 값이 null 일 때, 표현식이 false로 평가되고
// 변수의 타입 프로모션을 원한다면:
if (nullableBool != null && nullableBool) { ... }
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (nullable-boolean-expression)"?>
{% prettify dart tag=pre+code %}
// 값이 null이라면 정적 에러가 발생합니다:
if (nullableBool) { ... }

// 값이 null 일 때, false로 평가되도록 하고 싶다면:
if (nullableBool == true) { ... }
{% endprettify %}

`nullableBool == true`은 실행 가능한 표현식이지만,
다음과 같은 이유로 사용해서는 안 됩니다:

* 코드가 `null`과 관련이 있음을 나타내지 않습니다.

* 명백히 `null`과 관련이 없기 때문에,
  항등 연산자가 중복되어 제거될 수 있는
  non-nullable의 경우로 쉽게 오인될 수 있습니다.
  왼쪽의 boolean 표현식이 null을 생성할 가능성이 없을 때만 true이고,
  null을 생성할 수 있을 때는 그렇지 않습니다.

* 이 표현식의 boolean 로직은 혼란을 야기합니다.
  `nullableBool`이 null이라면, `nullableBoll == true`는
  조건식이 `false`로 평가된다는 것을 의미합니다.

`??` 연산자로 null이 발생할 수도 있는 상황에서 더 명확한 표현이 가능하고,
중복된 연산자로 오인되지도 않습니다.
로직 또한 더 명확합니다;
표현식이 `null`이 될 때의 결과는 boolean 리터럴과 동일합니다.

`??`와 같은 null-aware 연산자를 조건식에 사용하는 것은
변수를 non-nullable 타입으로 변환하지 않습니다.
`if`문의 바디에서 변수의 타입을 변환하고 싶다면,
`??` 보다 명시적으로 `!= null` 확인을 하는 것이 더 바람직합니다.

### 초기화 여부를 확인해야하는 변수를 `late`로 선언하는 것을 피하십시오.

Dart는 `late` 변수가 초기화 또는 할당되었는지 알 수 있는 방법을
가지고 있지 않습니다. `late` 변수에 접근 했을 때,
initializer를 실행하거나, 예외를 발생시킵니다.
바로 초기화될 필요가 없는 상태는 `late`와 잘 어울립니다.
하지만 아직 초기화가 이루어지지 않았다면, 그 사실을 *알고 있어야* 합니다.

`late` 변수에 상태를 저장하고 변수의 설정을 추적하는 분리된 boolean 필드를 사용함으로써
변수의 초기화를 감지할 수 있지만, Dart는 `late` 변수의 초기화 상태를 *내부적으로* 유지하기 때문에
굳이 그럴 필요가 없습니다. non-`late` 또는 nullable 변수를 대신 사용하는 것이 더 명확할 수 있습니다.
그렇게 하면 변수가 `null`인지 확인함으로써 초기화 상태를 확인할 수 있습니다.

물론 `null` 값이 변수의 유효한 초기화 값이라면,
분리된 boolean 필드를 가지는 것이 타당할 것입니다.


### 타입 프로모션을 활성화하고 싶다면 지역 변수에 nullable 필드를 할당하는 것을 고려하십시오.

Nullable 변수가 `null`과 같지 않는 것을 확인하는 것은 변수를 non-nullable 타입으로 변환시킵니다.
이를 통해 변수의 멤버에 접근하고 non-nullable 타입을 전달받는 함수에 사용이 가능합니다.
안타깝게도 타입 프로모션은 지역 변수와 매개변수에 대해서만 유효하므로 필드와 최상위 변수는 프로모션이 되지 않습니다.

이 문제를 해결하는 한 가지 패턴은 필드의 값을 지역 변수에 할당하는 것입니다.
해당 변수에 대한 null 체킹은 형변환을 수행함으로, 안전하게 non-nullable 처럼 처리하는 것이 가능합니다.

{:.good}
<?code-excerpt "usage_good.dart (shadow-nullable-field)"?>
{% prettify dart tag=pre+code %}
class UploadException {
  final Response? response;

  UploadException([this.response]);

  @override
  String toString() {
    final response = this.response;
    if (response != null) {
      return 'Could not complete upload to ${response.url} '
          '(error code ${response.errorCode}): ${response.reason}.';
    }

    return 'Could not upload (no response).';
  }
}
{% endprettify %}

지역 변수에 할당하는 것은 필드 또는 최상위 변수가 사용되는 모든 위치에서
`!`를 사용하는 것보다 깔끔하고 안전할 수 있습니다:

{:.bad}
<?code-excerpt "usage_bad.dart (shadow-nullable-field)" replace="/!\./[!!!]./g"?>
{% prettify dart tag=pre+code %}
class UploadException {
  final Response? response;

  UploadException([this.response]);

  @override
  String toString() {
    if (response != null) {
      return 'Could not complete upload to ${response[!!!].url} '
          '(error code ${response[!!!].errorCode}): ${response[!!!].reason}.';
    }

    return 'Could not upload (no response).';
  }
}
{% endprettify %}

지역 변수를 사용할 때는 주의해야 합니다. 필드의 값을 갱신해야 할 때,
지역 변수를 대신해서 갱신하지 않도록 주의하세요 (지역 변수를 `final`로 설정하는 것은 이런 실수를 예방합니다).
또한, 지역 변수가 스코프 안에 있을 때 필드가 변경될 수 있는 경우 지역 변수에는 오래된 값이 있을 수 있습니다.
때때로 필드에 `!`를 사용하는 것이 가장 좋을 때가 있습니다.


## Strings

Here are some best practices to keep in mind when composing strings in Dart.

### DO use adjacent strings to concatenate string literals.

{% include linter-rule-mention.md rule="prefer_adjacent_string_concatenation" %}

If you have two string literals—not values, but the actual quoted literal
form—you do not need to use `+` to concatenate them. Just like in C and
C++, simply placing them next to each other does it. This is a good way to make
a single long string that doesn't fit on one line.

{:.good}
<?code-excerpt "usage_good.dart (adjacent-strings-literals)"?>
{% prettify dart tag=pre+code %}
raiseAlarm('ERROR: Parts of the spaceship are on fire. Other '
    'parts are overrun by martians. Unclear which are which.');
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (adjacent-strings-literals)"?>
{% prettify dart tag=pre+code %}
raiseAlarm('ERROR: Parts of the spaceship are on fire. Other ' +
    'parts are overrun by martians. Unclear which are which.');
{% endprettify %}

### PREFER using interpolation to compose strings and values.

{% include linter-rule-mention.md rule="prefer_interpolation_to_compose_strings" %}

If you're coming from other languages, you're used to using long chains of `+`
to build a string out of literals and other values. That does work in Dart, but
it's almost always cleaner and shorter to use interpolation:

{:.good}
<?code-excerpt "usage_good.dart (string-interpolation)"?>
{% prettify dart tag=pre+code %}
'Hello, $name! You are ${year - birth} years old.';
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (string-interpolation)"?>
{% prettify dart tag=pre+code %}
'Hello, ' + name + '! You are ' + (year - birth).toString() + ' y...';
{% endprettify %}

Note that this guideline applies to combining *multiple* literals and values.
It's fine to use `.toString()` when converting only a single object to a string.

### AVOID using curly braces in interpolation when not needed.

{% include linter-rule-mention.md rule="unnecessary_brace_in_string_interps" %}

If you're interpolating a simple identifier not immediately followed by more
alphanumeric text, the `{}` should be omitted.

{:.good}
<?code-excerpt "usage_good.dart (string-interpolation-avoid-curly)"?>
{% prettify dart tag=pre+code %}
var greeting = 'Hi, $name! I love your ${decade}s costume.';
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (string-interpolation-avoid-curly)"?>
{% prettify dart tag=pre+code %}
var greeting = 'Hi, ${name}! I love your ${decade}s costume.';
{% endprettify %}

## Collections

Out of the box, Dart supports four collection types: lists, maps, queues, and sets.
The following best practices apply to collections.

### DO use collection literals when possible.

{% include linter-rule-mention.md rule="prefer_collection_literals" %}

Dart has three core collection types: List, Map, and Set. The Map and Set
classes have unnamed constructors like most classes do. But because these
collections are used so frequently, Dart has nicer built-in syntax for creating
them:

{:.good}
<?code-excerpt "usage_good.dart (collection-literals)"?>
{% prettify dart tag=pre+code %}
var points = <Point>[];
var addresses = <String, Address>{};
var counts = <int>{};
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (collection-literals)"?>
{% prettify dart tag=pre+code %}
var addresses = Map<String, Address>();
var counts = Set<int>();
{% endprettify %}

Note that this guideline doesn't apply to the *named* constructors for those
classes. `List.from()`, `Map.fromIterable()`, and friends all have their uses.
(The List class also has an unnamed constructor, but it is prohibited in null
safe Dart.)

Collection literals are particularly powerful in Dart
because they give you access to the [spread operator][spread]
for including the contents of other collections,
and [`if` and `for`][control] for performing control flow while
building the contents:

[spread]: /guides/language/language-tour#spread-operator
[control]: /guides/language/language-tour#collection-operators

{:.good}
<?code-excerpt "usage_good.dart (spread-etc)"?>
{% prettify dart tag=pre+code %}
var arguments = [
  ...options,
  command,
  ...?modeFlags,
  for (var path in filePaths)
    if (path.endsWith('.dart')) path.replaceAll('.dart', '.js')
];
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (spread-etc)"?>
{% prettify dart tag=pre+code %}
var arguments = <String>[];
arguments.addAll(options);
arguments.add(command);
if (modeFlags != null) arguments.addAll(modeFlags);
arguments.addAll(filePaths
    .where((path) => path.endsWith('.dart'))
    .map((path) => path.replaceAll('.dart', '.js')));
{% endprettify %}


### DON'T use `.length` to see if a collection is empty.

{% include linter-rule-mention.md rule1="prefer_is_empty" rule2="prefer_is_not_empty" %}

The [Iterable][] contract does not require that a collection know its length or
be able to provide it in constant time. Calling `.length` just to see if the
collection contains *anything* can be painfully slow.

[iterable]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Iterable-class.html

Instead, there are faster and more readable getters: `.isEmpty` and
`.isNotEmpty`. Use the one that doesn't require you to negate the result.

{:.good}
<?code-excerpt "usage_good.dart (dont-use-length)"?>
{% prettify dart tag=pre+code %}
if (lunchBox.isEmpty) return 'so hungry...';
if (words.isNotEmpty) return words.join(' ');
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (dont-use-length)"?>
{% prettify dart tag=pre+code %}
if (lunchBox.length == 0) return 'so hungry...';
if (!words.isEmpty) return words.join(' ');
{% endprettify %}


### AVOID using `Iterable.forEach()` with a function literal.

{% include linter-rule-mention.md rule="avoid_function_literals_in_foreach_calls" %}

`forEach()` functions are widely used in JavaScript because the built in
`for-in` loop doesn't do what you usually want. In Dart, if you want to iterate
over a sequence, the idiomatic way to do that is using a loop.

{:.good}
<?code-excerpt "usage_good.dart (avoid-forEach)"?>
{% prettify dart tag=pre+code %}
for (final person in people) {
  ...
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (avoid-forEach)"?>
{% prettify dart tag=pre+code %}
people.forEach((person) {
  ...
});
{% endprettify %}

Note that this guideline specifically says "function *literal*". If you want to
invoke some *already existing* function on each element, `forEach()` is fine.

{:.good}
<?code-excerpt "usage_good.dart (forEach-over-func)"?>
{% prettify dart tag=pre+code %}
people.forEach(print);
{% endprettify %}

Also note that it's always OK to use `Map.forEach()`. Maps aren't iterable, so
this guideline doesn't apply.

### DON'T use `List.from()` unless you intend to change the type of the result.

Given an Iterable, there are two obvious ways to produce a new List that
contains the same elements:

<?code-excerpt "../../test/effective_dart_test.dart (list-from-1)"?>
{% prettify dart tag=pre+code %}
var copy1 = iterable.toList();
var copy2 = List.from(iterable);
{% endprettify %}

The obvious difference is that the first one is shorter. The *important*
difference is that the first one preserves the type argument of the original
object:

{:.good}
<?code-excerpt "../../test/effective_dart_test.dart (list-from-good)"?>
{% prettify dart tag=pre+code %}
// Creates a List<int>:
var iterable = [1, 2, 3];

// Prints "List<int>":
print(iterable.toList().runtimeType);
{% endprettify %}

{:.bad}
<?code-excerpt "../../test/effective_dart_test.dart (list-from-bad)"?>
{% prettify dart tag=pre+code %}
// Creates a List<int>:
var iterable = [1, 2, 3];

// Prints "List<dynamic>":
print(List.from(iterable).runtimeType);
{% endprettify %}

If you *want* to change the type, then calling `List.from()` is useful:

{:.good}
<?code-excerpt "../../test/effective_dart_test.dart (list-from-3)"?>
{% prettify dart tag=pre+code %}
var numbers = [1, 2.3, 4]; // List<num>.
numbers.removeAt(1); // Now it only contains integers.
var ints = List<int>.from(numbers);
{% endprettify %}

But if your goal is just to copy the iterable and preserve its original type, or
you don't care about the type, then use `toList()`.


### DO use `whereType()` to filter a collection by type.

{% include linter-rule-mention.md rule="prefer_iterable_wheretype" %}

Let's say you have a list containing a mixture of objects, and you want to get
just the integers out of it. You could use `where()` like this:

{:.bad}
<?code-excerpt "usage_bad.dart (where-type)"?>
{% prettify dart tag=pre+code %}
var objects = [1, 'a', 2, 'b', 3];
var ints = objects.where((e) => e is int);
{% endprettify %}

This is verbose, but, worse, it returns an iterable whose type probably isn't
what you want. In the example here, it returns an `Iterable<Object>` even though
you likely want an `Iterable<int>` since that's the type you're filtering it to.

Sometimes you see code that "corrects" the above error by adding `cast()`:

{:.bad}
<?code-excerpt "usage_bad.dart (where-type-2)"?>
{% prettify dart tag=pre+code %}
var objects = [1, 'a', 2, 'b', 3];
var ints = objects.where((e) => e is int).cast<int>();
{% endprettify %}

That's verbose and causes two wrappers to be created, with two layers of
indirection and redundant runtime checking. Fortunately, the core library has
the [`whereType()`][where-type] method for this exact use case:

[where-type]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Iterable/whereType.html

{:.good}
<?code-excerpt "../../test/effective_dart_test.dart (whereType)"?>
{% prettify dart tag=pre+code %}
var objects = [1, 'a', 2, 'b', 3];
var ints = objects.whereType<int>();
{% endprettify %}

Using `whereType()` is concise, produces an [Iterable][] of the desired type,
and has no unnecessary levels of wrapping.


### DON'T use `cast()` when a nearby operation will do.

Often when you're dealing with an iterable or stream, you perform several
transformations on it. At the end, you want to produce an object with a certain
type argument. Instead of tacking on a call to `cast()`, see if one of the
existing transformations can change the type.

If you're already calling `toList()`, replace that with a call to
[`List<T>.from()`][list-from] where `T` is the type of resulting list you want.

[list-from]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/List/List.from.html

{:.good}
<?code-excerpt "usage_good.dart (cast-list)"?>
{% prettify dart tag=pre+code %}
var stuff = <dynamic>[1, 2];
var ints = List<int>.from(stuff);
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (cast-list)"?>
{% prettify dart tag=pre+code %}
var stuff = <dynamic>[1, 2];
var ints = stuff.toList().cast<int>();
{% endprettify %}

If you are calling `map()`, give it an explicit type argument so that it
produces an iterable of the desired type. Type inference often picks the correct
type for you based on the function you pass to `map()`, but sometimes you need
to be explicit.

{:.good}
<?code-excerpt "usage_good.dart (cast-map)" replace="/\(n as int\)/n/g"?>
{% prettify dart tag=pre+code %}
var stuff = <dynamic>[1, 2];
var reciprocals = stuff.map<double>((n) => 1 / n);
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (cast-map)" replace="/\(n as int\)/n/g"?>
{% prettify dart tag=pre+code %}
var stuff = <dynamic>[1, 2];
var reciprocals = stuff.map((n) => 1 / n).cast<double>();
{% endprettify %}


### AVOID using `cast()`.

This is the softer generalization of the previous rule. Sometimes there is no
nearby operation you can use to fix the type of some object. Even then, when
possible avoid using `cast()` to "change" a collection's type.

Prefer any of these options instead:

*   **Create it with the right type.** Change the code where the collection is
    first created so that it has the right type.

*   **Cast the elements on access.** If you immediately iterate over the
    collection, cast each element inside the iteration.

*   **Eagerly cast using `List.from()`.** If you'll eventually access most of
    the elements in the collection, and you don't need the object to be backed
    by the original live object, convert it using `List.from()`.

    The `cast()` method returns a lazy collection that checks the element type
    on *every operation*. If you perform only a few operations on only a few
    elements, that laziness can be good. But in many cases, the overhead of lazy
    validation and of wrapping outweighs the benefits.

Here is an example of **creating it with the right type:**

{:.good}
<?code-excerpt "usage_good.dart (cast-at-create)"?>
{% prettify dart tag=pre+code %}
List<int> singletonList(int value) {
  var list = <int>[];
  list.add(value);
  return list;
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (cast-at-create)"?>
{% prettify dart tag=pre+code %}
List<int> singletonList(int value) {
  var list = []; // List<dynamic>.
  list.add(value);
  return list.cast<int>();
}
{% endprettify %}

Here is **casting each element on access:**

{:.good}
<?code-excerpt "usage_good.dart (cast-iterate)" replace="/\(n as int\)/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
void printEvens(List<Object> objects) {
  // We happen to know the list only contains ints.
  for (final n in objects) {
    if ([!(n as int)!].isEven) print(n);
  }
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (cast-iterate)"?>
{% prettify dart tag=pre+code %}
void printEvens(List<Object> objects) {
  // We happen to know the list only contains ints.
  for (final n in objects.cast<int>()) {
    if (n.isEven) print(n);
  }
}
{% endprettify %}

Here is **casting eagerly using `List.from()`:**

{:.good}
<?code-excerpt "usage_good.dart (cast-from)"?>
{% prettify dart tag=pre+code %}
int median(List<Object> objects) {
  // We happen to know the list only contains ints.
  var ints = List<int>.from(objects);
  ints.sort();
  return ints[ints.length ~/ 2];
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (cast-from)"?>
{% prettify dart tag=pre+code %}
int median(List<Object> objects) {
  // We happen to know the list only contains ints.
  var ints = objects.cast<int>();
  ints.sort();
  return ints[ints.length ~/ 2];
}
{% endprettify %}

These alternatives don't always work, of course, and sometimes `cast()` is the
right answer. But consider that method a little risky and undesirable—it
can be slow and may fail at runtime if you aren't careful.


## Functions

In Dart, even functions are objects. Here are some best practices
involving functions.


### DO use a function declaration to bind a function to a name.

{% include linter-rule-mention.md rule="prefer_function_declarations_over_variables" %}

Modern languages have realized how useful local nested functions and closures
are. It's common to have a function defined inside another one. In many cases,
this function is used as a callback immediately and doesn't need a name. A
function expression is great for that.

But, if you do need to give it a name, use a function declaration statement
instead of binding a lambda to a variable.

{:.good}
<?code-excerpt "usage_good.dart (func-decl)"?>
{% prettify dart tag=pre+code %}
void main() {
  void localFunction() {
    ...
  }
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (func-decl)"?>
{% prettify dart tag=pre+code %}
void main() {
  var localFunction = () {
    ...
  };
}
{% endprettify %}

### DON'T create a lambda when a tear-off will do.

{% include linter-rule-mention.md rule="unnecessary_lambdas" %}

When you refer to a function, method, or named constructor but omit the
parentheses, Dart creates a _tear-off_—a closure that takes the same
parameters as the function and invokes the underlying function when you call it.
If all you need is a closure that invokes a named function with the same
parameters as the closure accepts, don't manually wrap the call in a lambda.

{:.good}
<?code-excerpt "usage_good.dart (use-tear-off)"?>
{% prettify dart tag=pre+code %}
var charCodes = [68, 97, 114, 116];
var buffer = StringBuffer();

// Function:
charCodes.forEach(print);

// Method:
charCodes.forEach(buffer.write);

// Named constructor:
var strings = charCodes.map(String.fromCharCode);

// Unnamed constructor:
var buffers = charCodes.map(StringBuffer.new);
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (use-tear-off)"?>
{% prettify dart tag=pre+code %}
var charCodes = [68, 97, 114, 116];
var buffer = StringBuffer();

// Function:
charCodes.forEach((code) {
  print(code);
});

// Method:
charCodes.forEach((code) {
  buffer.write(code);
});

// Named constructor:
var strings = charCodes.map((code) => String.fromCharCode(code));

// Unnamed constructor:
var buffers = charCodes.map((code) => StringBuffer(code));
{% endprettify %}


### DO use `=` to separate a named parameter from its default value.

{% include linter-rule-mention.md rule="prefer_equal_for_default_values" %}

For legacy reasons, Dart allows both `:` and `=` as the default value separator
for named parameters. For consistency with optional positional parameters, use
`=`.

{:.good}
<?code-excerpt "usage_good.dart (default-separator)"?>
{% prettify dart tag=pre+code %}
void insert(Object item, {int at = 0}) { ... }
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (default-separator)"?>
{% prettify dart tag=pre+code %}
void insert(Object item, {int at: 0}) { ... }
{% endprettify %}


## Variables

The following best practices describe how to best use variables in Dart.

### DO follow a consistent rule for `var` and `final` on local variables.

Most local variables shouldn't have type annotations and should be declared
using just `var` or `final`. There are two rules in wide use for when to use one
or the other:

*   Use `final` for local variables that are not reassigned and `var` for those
    that are.

*   Use `var` for all local variables, even ones that aren't reassigned. Never use
    `final` for locals. (Using `final` for fields and top-level variables is
    still encouraged, of course.)

Either rule is acceptable, but pick *one* and apply it consistently throughout
your code. That way when a reader sees `var`, they know whether it means that
the variable is assigned later in the function.


### AVOID storing what you can calculate.

When designing a class, you often want to expose multiple views into the same
underlying state. Often you see code that calculates all of those views in the
constructor and then stores them:

{:.bad}
<?code-excerpt "usage_bad.dart (calc-vs-store1)"?>
{% prettify dart tag=pre+code %}
class Circle {
  double radius;
  double area;
  double circumference;

  Circle(double radius)
      : radius = radius,
        area = pi * radius * radius,
        circumference = pi * 2.0 * radius;
}
{% endprettify %}

This code has two things wrong with it. First, it's likely wasting memory. The
area and circumference, strictly speaking, are *caches*. They are stored
calculations that we could recalculate from other data we already have. They are
trading increased memory for reduced CPU usage. Do we know we have a performance
problem that merits that trade-off?

Worse, the code is *wrong*. The problem with caches is *invalidation*—how
do you know when the cache is out of date and needs to be recalculated? Here, we
never do, even though `radius` is mutable. You can assign a different value and
the `area` and `circumference` will retain their previous, now incorrect values.

To correctly handle cache invalidation, we would need to do this:

{:.bad}
<?code-excerpt "usage_bad.dart (calc-vs-store2)"?>
{% prettify dart tag=pre+code %}
class Circle {
  double _radius;
  double get radius => _radius;
  set radius(double value) {
    _radius = value;
    _recalculate();
  }

  double _area = 0.0;
  double get area => _area;

  double _circumference = 0.0;
  double get circumference => _circumference;

  Circle(this._radius) {
    _recalculate();
  }

  void _recalculate() {
    _area = pi * _radius * _radius;
    _circumference = pi * 2.0 * _radius;
  }
}
{% endprettify %}

That's an awful lot of code to write, maintain, debug, and read. Instead, your
first implementation should be:

{:.good}
<?code-excerpt "usage_good.dart (calc-vs-store)"?>
{% prettify dart tag=pre+code %}
class Circle {
  double radius;

  Circle(this.radius);

  double get area => pi * radius * radius;
  double get circumference => pi * 2.0 * radius;
}
{% endprettify %}

This code is shorter, uses less memory, and is less error-prone. It stores the
minimal amount of data needed to represent the circle. There are no fields to
get out of sync because there is only a single source of truth.

In some cases, you may need to cache the result of a slow calculation, but only
do that after you know you have a performance problem, do it carefully, and
leave a comment explaining the optimization.


## Members

In Dart, objects have members which can be functions (methods) or data (instance
variables). The following best practices apply to an object's members.

### DON'T wrap a field in a getter and setter unnecessarily.

{% include linter-rule-mention.md rule="unnecessary_getters_setters" %}

In Java and C#, it's common to hide all fields behind getters and setters (or
properties in C#), even if the implementation just forwards to the field. That
way, if you ever need to do more work in those members, you can without needing
to touch the call sites. This is because calling a getter method is different
than accessing a field in Java, and accessing a property isn't binary-compatible
with accessing a raw field in C#.

Dart doesn't have this limitation. Fields and getters/setters are completely
indistinguishable. You can expose a field in a class and later wrap it in a
getter and setter without having to touch any code that uses that field.

{:.good}
<?code-excerpt "usage_good.dart (dont-wrap-field)"?>
{% prettify dart tag=pre+code %}
class Box {
  Object? contents;
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (dont-wrap-field)"?>
{% prettify dart tag=pre+code %}
class Box {
  Object? _contents;
  Object? get contents => _contents;
  set contents(Object? value) {
    _contents = value;
  }
}
{% endprettify %}


### PREFER using a `final` field to make a read-only property.

If you have a field that outside code should be able to see but not assign to, a
simple solution that works in many cases is to simply mark it `final`.

{:.good}
<?code-excerpt "usage_good.dart (final)"?>
{% prettify dart tag=pre+code %}
class Box {
  final contents = [];
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (final)"?>
{% prettify dart tag=pre+code %}
class Box {
  Object? _contents;
  Object? get contents => _contents;
}
{% endprettify %}

Of course, if you need to internally assign to the field outside of the
constructor, you may need to do the "private field, public getter" pattern, but
don't reach for that until you need to.


### CONSIDER using `=>` for simple members.

{% include linter-rule-mention.md rule="prefer_expression_function_bodies" %}

In addition to using `=>` for function expressions, Dart also lets you define
members with it. That style is a good fit for simple members that just calculate
and return a value.

{:.good}
<?code-excerpt "usage_good.dart (use-arrow)"?>
{% prettify dart tag=pre+code %}
double get area => (right - left) * (bottom - top);

String capitalize(String name) =>
    '${name[0].toUpperCase()}${name.substring(1)}';
{% endprettify %}

People *writing* code seem to love `=>`, but it's very easy to abuse it and end
up with code that's hard to *read*. If your declaration is more than a couple of
lines or contains deeply nested expressions—cascades and conditional
operators are common offenders—do yourself and everyone who has to read
your code a favor and use a block body and some statements.

{:.good}
<?code-excerpt "usage_good.dart (arrow-long)"?>
{% prettify dart tag=pre+code %}
Treasure? openChest(Chest chest, Point where) {
  if (_opened.containsKey(chest)) return null;

  var treasure = Treasure(where);
  treasure.addAll(chest.contents);
  _opened[chest] = treasure;
  return treasure;
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (arrow-long)"?>
{% prettify dart tag=pre+code %}
Treasure? openChest(Chest chest, Point where) => _opened.containsKey(chest)
    ? null
    : _opened[chest] = (Treasure(where)..addAll(chest.contents));
{% endprettify %}

You can also use `=>` on members that don't return a value. This is idiomatic
when a setter is small and has a corresponding getter that uses `=>`.

{:.good}
<?code-excerpt "usage_good.dart (arrow-setter)"?>
{% prettify dart tag=pre+code %}
num get x => center.x;
set x(num value) => center = Point(value, center.y);
{% endprettify %}


### DON'T use `this.` except to redirect to a named constructor or to avoid shadowing. {#dont-use-this-when-not-needed-to-avoid-shadowing}

{% include linter-rule-mention.md rule="unnecessary_this" %}

JavaScript requires an explicit `this.` to refer to members on the object whose
method is currently being executed, but Dart—like C++, Java, and
C#—doesn't have that limitation.

There are only two times you need to use `this.`. One is when a local variable
with the same name shadows the member you want to access:

{:.bad}
<?code-excerpt "usage_bad.dart (this-dot)"?>
{% prettify dart tag=pre+code %}
class Box {
  Object? value;

  void clear() {
    this.update(null);
  }

  void update(Object? value) {
    this.value = value;
  }
}
{% endprettify %}

{:.good}
<?code-excerpt "usage_good.dart (this-dot)"?>
{% prettify dart tag=pre+code %}
class Box {
  Object? value;

  void clear() {
    update(null);
  }

  void update(Object? value) {
    this.value = value;
  }
}
{% endprettify %}

The other time to use `this.` is when redirecting to a named constructor:

{:.bad}
<?code-excerpt "usage_bad.dart (this-dot-constructor)"?>
{% prettify dart tag=pre+code %}
class ShadeOfGray {
  final int brightness;

  ShadeOfGray(int val) : brightness = val;

  ShadeOfGray.black() : this(0);

  // This won't parse or compile!
  // ShadeOfGray.alsoBlack() : black();
}
{% endprettify %}

{:.good}
<?code-excerpt "usage_good.dart (this-dot-constructor)"?>
{% prettify dart tag=pre+code %}
class ShadeOfGray {
  final int brightness;

  ShadeOfGray(int val) : brightness = val;

  ShadeOfGray.black() : this(0);

  // But now it will!
  ShadeOfGray.alsoBlack() : this.black();
}
{% endprettify %}

Note that constructor parameters never shadow fields in constructor initializer
lists:

{:.good}
<?code-excerpt "usage_good.dart (param-dont-shadow-field-ctr-init)"?>
{% prettify dart tag=pre+code %}
class Box extends BaseBox {
  Object? value;

  Box(Object? value)
      : value = value,
        super(value);
}
{% endprettify %}

This looks surprising, but works like you want. Fortunately, code like this is
relatively rare thanks to initializing formals and super initializers.


### DO initialize fields at their declaration when possible.

If a field doesn't depend on any constructor parameters, it can and should be
initialized at its declaration. It takes less code and avoids duplication when
the class has multiple constructors.

{:.bad}
<?code-excerpt "usage_bad.dart (field-init-at-decl)"?>
{% prettify dart tag=pre+code %}
class ProfileMark {
  final String name;
  final DateTime start;

  ProfileMark(this.name) : start = DateTime.now();
  ProfileMark.unnamed()
      : name = '',
        start = DateTime.now();
}
{% endprettify %}

{:.good}
<?code-excerpt "usage_good.dart (field-init-at-decl)"?>
{% prettify dart tag=pre+code %}
class ProfileMark {
  final String name;
  final DateTime start = DateTime.now();

  ProfileMark(this.name);
  ProfileMark.unnamed() : name = '';
}
{% endprettify %}

Some fields can't be initialized at their declarations because they need to reference
`this`—to use other fields or call methods, for example. However, if the
field is marked `late`, then the initializer *can* access `this`.

Of course, if a field depends on constructor parameters, or is initialized
differently by different constructors, then this guideline does not apply.


## Constructors

The following best practices apply to declaring constructors for a class.

### DO use initializing formals when possible.

{% include linter-rule-mention.md rule="prefer_initializing_formals" %}

Many fields are initialized directly from a constructor parameter, like:

{:.bad}
<?code-excerpt "usage_bad.dart (field-init-as-param)"?>
{% prettify dart tag=pre+code %}
class Point {
  double x, y;
  Point(double x, double y)
      : x = x,
        y = y;
}
{% endprettify %}

We've got to type `x` _four_ times here to define a field. We can do better:

{:.good}
<?code-excerpt "usage_good.dart (field-init-as-param)"?>
{% prettify dart tag=pre+code %}
class Point {
  double x, y;
  Point(this.x, this.y);
}
{% endprettify %}

This `this.` syntax before a constructor parameter is called an "initializing
formal". You can't always take advantage of it. Sometimes you want to have a
named parameter whose name doesn't match the name of the field you are
initializing. But when you *can* use initializing formals, you *should*.


### DON'T use `late` when a constructor initializer list will do.

Sound null safety requires Dart to ensure that a non-nullable field is
initialized before it can be read. Since fields can be read inside the
constructor body, this means you get an error if you don't initialize a
non-nullable field before the body runs.

You can make this error go away by marking the field `late`. That turns the
compile-time error into a *runtime* error if you access the field before it is
initialized. That's what you need in some cases, but often the right fix is to
initialize the field in the constructor initializer list:

{:.good}
<?code-excerpt "usage_good.dart (late-init-list)"?>
{% prettify dart tag=pre+code %}
class Point {
  double x, y;
  Point.polar(double theta, double radius)
      : x = cos(theta) * radius,
        y = sin(theta) * radius;
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (late-init-list)"?>
{% prettify dart tag=pre+code %}
class Point {
  late double x, y;
  Point.polar(double theta, double radius) {
    x = cos(theta) * radius;
    y = sin(theta) * radius;
  }
}
{% endprettify %}


The initializer list gives you access to constructor parameters and lets you
initialize fields before they can be read. So, if it's possible to use an initializer list,
that's better than making the field `late` and losing some static safety and
performance.


### DO use `;` instead of `{}` for empty constructor bodies.

{% include linter-rule-mention.md rule="empty_constructor_bodies" %}

In Dart, a constructor with an empty body can be terminated with just a
semicolon. (In fact, it's required for const constructors.)

{:.good}
<?code-excerpt "usage_good.dart (semicolon-for-empty-body)"?>
{% prettify dart tag=pre+code %}
class Point {
  double x, y;
  Point(this.x, this.y);
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (semicolon-for-empty-body)"?>
{% prettify dart tag=pre+code %}
class Point {
  double x, y;
  Point(this.x, this.y) {}
}
{% endprettify %}

### DON'T use `new`.

{% include linter-rule-mention.md rule="unnecessary_new" %}

Dart 2 makes the `new` keyword optional. Even in Dart 1, its meaning was never
clear because factory constructors mean a `new` invocation may still not
actually return a new object.

The language still permits `new` in order to make migration less painful, but
consider it deprecated and remove it from your code.

{:.good}
<?code-excerpt "usage_good.dart (no-new)"?>
{% prettify dart tag=pre+code %}
Widget build(BuildContext context) {
  return Row(
    children: [
      RaisedButton(
        child: Text('Increment'),
      ),
      Text('Click!'),
    ],
  );
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (no-new)" replace="/new/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
Widget build(BuildContext context) {
  return [!new!] Row(
    children: [
      [!new!] RaisedButton(
        child: [!new!] Text('Increment'),
      ),
      [!new!] Text('Click!'),
    ],
  );
}
{% endprettify %}


### DON'T use `const` redundantly.

{% include linter-rule-mention.md rule="unnecessary_const" %}

In contexts where an expression *must* be constant, the `const` keyword is
implicit, doesn't need to be written, and shouldn't. Those contexts are any
expression inside:

* A const collection literal.
* A const constructor call
* A metadata annotation.
* The initializer for a const variable declaration.
* A switch case expression—the part right after `case` before the `:`, not
  the body of the case.

(Default values are not included in this list because future versions of Dart
may support non-const default values.)

Basically, any place where it would be an error to write `new` instead of
`const`, Dart 2 allows you to omit the `const`.

{:.good}
<?code-excerpt "usage_good.dart (no-const)"?>
{% prettify dart tag=pre+code %}
const primaryColors = [
  Color('red', [255, 0, 0]),
  Color('green', [0, 255, 0]),
  Color('blue', [0, 0, 255]),
];
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (no-const)" replace="/ (const)/ [!$1!]/g"?>
{% prettify dart tag=pre+code %}
const primaryColors = [!const!] [
  [!const!] Color('red', [!const!] [255, 0, 0]),
  [!const!] Color('green', [!const!] [0, 255, 0]),
  [!const!] Color('blue', [!const!] [0, 0, 255]),
];
{% endprettify %}

## Error handling

Dart uses exceptions when an error occurs in your program. The following
best practices apply to catching and throwing exceptions.

### AVOID catches without `on` clauses.

{% include linter-rule-mention.md rule="avoid_catches_without_on_clauses" %}

A catch clause with no `on` qualifier catches *anything* thrown by the code in
the try block. [Pokémon exception handling][pokemon] is very likely not what you
want. Does your code correctly handle [StackOverflowError][] or
[OutOfMemoryError][]? If you incorrectly pass the wrong argument to a method in
that try block do you want to have your debugger point you to the mistake or
would you rather that helpful [ArgumentError][] get swallowed? Do you want any
`assert()` statements inside that code to effectively vanish since you're
catching the thrown [AssertionError][]s?

The answer is probably "no", in which case you should filter the types you
catch. In most cases, you should have an `on` clause that limits you to the
kinds of runtime failures you are aware of and are correctly handling.

In rare cases, you may wish to catch any runtime error. This is usually in
framework or low-level code that tries to insulate arbitrary application code
from causing problems. Even here, it is usually better to catch [Exception][]
than to catch all types. Exception is the base class for all *runtime* errors
and excludes errors that indicate *programmatic* bugs in the code.


### DON'T discard errors from catches without `on` clauses.

If you really do feel you need to catch *everything* that can be thrown from a
region of code, *do something* with what you catch. Log it, display it to the
user or rethrow it, but do not silently discard it.


### DO throw objects that implement `Error` only for programmatic errors.

The [Error][] class is the base class for *programmatic* errors. When an object
of that type or one of its subinterfaces like [ArgumentError][] is thrown, it
means there is a *bug* in your code. When your API wants to report to a caller
that it is being used incorrectly throwing an Error sends that signal clearly.

Conversely, if the exception is some kind of runtime failure that doesn't
indicate a bug in the code, then throwing an Error is misleading. Instead, throw
one of the core Exception classes or some other type.


### DON'T explicitly catch `Error` or types that implement it.

{% include linter-rule-mention.md rule="avoid_catching_errors" %}

This follows from the above. Since an Error indicates a bug in your code, it
should unwind the entire callstack, halt the program, and print a stack trace so
you can locate and fix the bug.

Catching errors of these types breaks that process and masks the bug. Instead of
*adding* error-handling code to deal with this exception after the fact, go back
and fix the code that is causing it to be thrown in the first place.


### DO use `rethrow` to rethrow a caught exception.

{% include linter-rule-mention.md rule="use_rethrow_when_possible" %}

If you decide to rethrow an exception, prefer using the `rethrow` statement
instead of throwing the same exception object using `throw`.
`rethrow` preserves the original stack trace of the exception. `throw` on the
other hand resets the stack trace to the last thrown position.

{:.bad}
<?code-excerpt "usage_bad.dart (rethrow)"?>
{% prettify dart tag=pre+code %}
try {
  somethingRisky();
} catch (e) {
  if (!canHandle(e)) throw e;
  handle(e);
}
{% endprettify %}

{:.good}
<?code-excerpt "usage_good.dart (rethrow)" replace="/rethrow/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
try {
  somethingRisky();
} catch (e) {
  if (!canHandle(e)) [!rethrow!];
  handle(e);
}
{% endprettify %}


## Asynchrony

Dart has several language features to support asynchronous programming.
The following best practices apply to asynchronous coding.

### PREFER async/await over using raw futures.

Asynchronous code is notoriously hard to read and debug, even when using a nice
abstraction like futures. The `async`/`await` syntax improves readability and
lets you use all of the Dart control flow structures within your async code.

{:.good}
<?code-excerpt "usage_good.dart (async-await)" replace="/async|await/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
Future<int> countActivePlayers(String teamName) [!async!] {
  try {
    var team = [!await!] downloadTeam(teamName);
    if (team == null) return 0;

    var players = [!await!] team.roster;
    return players.where((player) => player.isActive).length;
  } catch (e) {
    log.error(e);
    return 0;
  }
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (async-await)"?>
{% prettify dart tag=pre+code %}
Future<int> countActivePlayers(String teamName) {
  return downloadTeam(teamName).then((team) {
    if (team == null) return Future.value(0);

    return team.roster.then((players) {
      return players.where((player) => player.isActive).length;
    });
  }).catchError((e) {
    log.error(e);
    return 0;
  });
}
{% endprettify %}

### DON'T use `async` when it has no useful effect.

It's easy to get in the habit of using `async` on any function that does
anything related to asynchrony. But in some cases, it's extraneous. If you can
omit the `async` without changing the behavior of the function, do so.

{:.good}
<?code-excerpt "usage_good.dart (unnecessary-async)"?>
{% prettify dart tag=pre+code %}
Future<int> fastestBranch(Future<int> left, Future<int> right) {
  return Future.any([left, right]);
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (unnecessary-async)"?>
{% prettify dart tag=pre+code %}
Future<int> fastestBranch(Future<int> left, Future<int> right) async {
  return Future.any([left, right]);
}
{% endprettify %}

Cases where `async` *is* useful include:

* You are using `await`. (This is the obvious one.)

* You are returning an error asynchronously. `async` and then `throw` is shorter
  than `return Future.error(...)`.

* You are returning a value and you want it implicitly wrapped in a future.
  `async` is shorter than `Future.value(...)`.

{:.good}
<?code-excerpt "usage_good.dart (async)"?>
{% prettify dart tag=pre+code %}
Future<void> usesAwait(Future<String> later) async {
  print(await later);
}

Future<void> asyncError() async {
  throw 'Error!';
}

Future<String> asyncValue() async => 'value';
{% endprettify %}

### CONSIDER using higher-order methods to transform a stream.

This parallels the above suggestion on iterables. Streams support many of the
same methods and also handle things like transmitting errors, closing, etc.
correctly.

### AVOID using Completer directly.

Many people new to asynchronous programming want to write code that produces a
future. The constructors in Future don't seem to fit their need so they
eventually find the Completer class and use that.

{:.bad}
<?code-excerpt "usage_bad.dart (avoid-completer)"?>
{% prettify dart tag=pre+code %}
Future<bool> fileContainsBear(String path) {
  var completer = Completer<bool>();

  File(path).readAsString().then((contents) {
    completer.complete(contents.contains('bear'));
  });

  return completer.future;
}
{% endprettify %}

Completer is needed for two kinds of low-level code: new asynchronous
primitives, and interfacing with asynchronous code that doesn't use futures.
Most other code should use async/await or [`Future.then()`][then], because
they're clearer and make error handling easier.

[then]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-async/Future/then.html

{:.good}
<?code-excerpt "usage_good.dart (avoid-completer)"?>
{% prettify dart tag=pre+code %}
Future<bool> fileContainsBear(String path) {
  return File(path).readAsString().then((contents) {
    return contents.contains('bear');
  });
}
{% endprettify %}

{:.good}
<?code-excerpt "usage_good.dart (avoid-completer-alt)"?>
{% prettify dart tag=pre+code %}
Future<bool> fileContainsBear(String path) async {
  var contents = await File(path).readAsString();
  return contents.contains('bear');
}
{% endprettify %}


### DO test for `Future<T>` when disambiguating a `FutureOr<T>` whose type argument could be `Object`.

Before you can do anything useful with a `FutureOr<T>`, you typically need to do
an `is` check to see if you have a `Future<T>` or a bare `T`. If the type
argument is some specific type as in `FutureOr<int>`, it doesn't matter which
test you use, `is int` or `is Future<int>`. Either works because those two types
are disjoint.

However, if the value type is `Object` or a type parameter that could possibly
be instantiated with `Object`, then the two branches overlap. `Future<Object>`
itself implements `Object`, so `is Object` or `is T` where `T` is some type
parameter that could be instantiated with `Object` returns true even when the
object is a future. Instead, explicitly test for the `Future` case:

{:.good}
<?code-excerpt "usage_good.dart (test-future-or)"?>
{% prettify dart tag=pre+code %}
Future<T> logValue<T>(FutureOr<T> value) async {
  if (value is Future<T>) {
    var result = await value;
    print(result);
    return result;
  } else {
    print(value);
    return value;
  }
}
{% endprettify %}

{:.bad}
<?code-excerpt "usage_bad.dart (test-future-or)"?>
{% prettify dart tag=pre+code %}
Future<T> logValue<T>(FutureOr<T> value) async {
  if (value is T) {
    print(value);
    return value;
  } else {
    var result = await value;
    print(result);
    return result;
  }
}
{% endprettify %}

In the bad example, if you pass it a `Future<Object>`, it incorrectly treats it
like a bare, synchronous value.

[pokemon]: https://blog.codinghorror.com/new-programming-jargon/
[Error]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Error-class.html
[StackOverflowError]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/StackOverflowError-class.html
[OutOfMemoryError]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/OutOfMemoryError-class.html
[ArgumentError]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/ArgumentError-class.html
[AssertionError]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/AssertionError-class.html
[Exception]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Exception-class.html
