---
title: 언어 샘플
description: 더 큰 예제와 이어지는 자연스러운 Dart 예제.
---

이 컬렉션은 완전하지 않습니다—예제를 통해 Dart를 배우고 싶은 사람들을 위한 간단한 소개 입니다.
[Dart cheatsheet codelab](/codelabs/dart-cheatsheet) 또는 언어, 라이브러리 투어도 참고해보세요.

<div class="card-grid no_toc_section">
  <div class="card">
    <h3><a href="/guides/language/language-tour">언어 투어</a></h3>
    <p>
      예제가 있는 Dart 언어의 포괄적인 투어.
      이 페이지 대부분의 <em>더보기</em>는 언어 투어로 이동합니다.
    </p>
  </div>
  <div class="card">
    <h3><a href="/guides/libraries/library-tour">라이브러리 투어</a></h3>
    <p>
      예제를 기반으로한 Dart 핵심 라이브러리 소개.
      내장 타입, 컬렉션, 날짜와 시간, 스트림 등을 어떻게 사용하는지 배울 수 있습니다.
    </p>
  </div>
</div>


## Hello World

모든 앱은 `main()` 함수를 가지고 있습니다.
콘솔에 텍스트를 띄우고 싶다면, 최상위 `print()` 함수를 사용하세요:

<?code-excerpt "misc/test/samples_test.dart (hello-world)"?>
```dart
void main() {
  print('Hello, World!');
}
```


## 변수

타입 안정성이 높은 Dart 코드일지라도, 대부분의 변수는 타입 추론 덕분에 명시적인 타입을 가질 필요가 없습니다:

<?code-excerpt "misc/test/samples_test.dart (var)"?>
```dart
var name = 'Voyager I';
var year = 1977;
var antennaDiameter = 3.7;
var flybyObjects = ['Jupiter', 'Saturn', 'Uranus', 'Neptune'];
var image = {
  'tags': ['saturn'],
  'url': '//path/to/saturn.jpg'
};
```

정적 타입, `final`, `const` 키워드 디폴트 값을 포함한 Dart의 변수에 대해 
[더보기](/guides/language/language-tour#variables).

## 흐름 제어문

Dart는 자주 사용하는 흐름 제어문을 지원합니다:

<?code-excerpt "misc/test/samples_test.dart (control-flow)"?>
```dart
if (year >= 2001) {
  print('21st century');
} else if (year >= 1901) {
  print('20th century');
}

for (final object in flybyObjects) {
  print(object);
}

for (int month = 1; month <= 12; month++) {
  print(month);
}

while (year < 2016) {
  year += 1;
}
```

`break`과 `continue`, `switch`와 `case`, `assert`를
가지는 Dart 흐름 제어문에 대해 
[더보기](/guides/language/language-tour#control-flow-statements) 


## 함수

함수의 인자와 반환값의 타입을 지정하는 것을
[추천합니다](/guides/language/effective-dart/design#types):

<?code-excerpt "misc/test/samples_test.dart (functions)"?>
```dart
int fibonacci(int n) {
  if (n == 0 || n == 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

var result = fibonacci(20);
```

단일 표현식을 가지는 함수를 구현할 때 `=>` (_화살표_) 문법은 유용합니다.
이 문법은 익명 함수를 인자로 넘겨줄 때 특히 유용합니다:

<?code-excerpt "misc/test/samples_test.dart (arrow)"?>
```dart
flybyObjects.where((name) => name.contains('turn')).forEach(print);
```

이 코드는 익명 함수를 보여주는 것 외에도 (`where()`의 인자),
함수를 인자로 사용할 수 있음을 보여줍니다:
최상위 `print()` 함수는 `forEach()`의 인자가 됩니다.

Optional 매개변수, 디폴트 매개변수 값 그리고 렉시컬 스코프를 포함하는 Dart의 함수에 대해
[더보기](/guides/language/language-tour#functions).

## 주석

Dart의 주석은 보통 `//`로 시작합니다.

```dart
// 이것은 흔한 단일 주석입니다.

/// 이것은 문서 라이브러리, 클래스, 클래스의 멤버에 사용되는 문서화 주석입니다.
/// IDE 같은 툴이나 dartdoc은 문서화 주석를 특수하게 처리합니다.

/* 이런 형태의 주석도 지원합니다. */
```

문서화 도구의 작동 방식을 비록해 Dart의 주석에 대한 자세한 내용
[더보기](/guides/language/language-tour#comments)

## Imports

라이브러리에 정의된 API에 접근하고 싶다면, `import`를 사용하세요.

<?code-excerpt "misc/test/samples_test.dart (import)" plaster="none"?>
```
// 핵심 라이브러리 import
import 'dart:math';

// 외부 패키지의 라이브러리 import
import 'package:test/test.dart';

// 파일 import
import 'path/to/my_other_file.dart';
```

라이브러리 prefix, `show`와 `hide`, `deferred` 키워드를 사용한 지연 로딩을 비롯한
Dart 라이브러리와 가시성에 대해
[더보기](/guides/language/language-tour#libraries-and-visibility) 


## 클래스

다음은 메서드, 두개의 생성자, 세개의 프로퍼티를 가지는 클래스 예제입니다.
하나의 프로퍼티는 직접적으로 설정이 불가능하여,
변수가 아닌 getter 메서드를 사용하여 정의됩니다.

<?code-excerpt "misc/lib/samples/spacecraft.dart (class)"?>
```dart
class Spacecraft {
  String name;
  DateTime? launchDate;

  // Read-only non-final 프로퍼티
  int? get launchYear => launchDate?.year;

  // 멤버 할당에 신택틱 슈가를 사용한 생성자
  Spacecraft(this.name, this.launchDate) {
    // 여기에 초기화 코드가 이어집니다.
  }

  // 디폴트 생성자로 포워드하는 Named 생성자.
  Spacecraft.unlaunched(String name) : this(name, null);

  // 메서드.
  void describe() {
    print('Spacecraft: $name');
    // Getter에는 타입 프로모션이 작동하지 않습니다.
    var launchDate = this.launchDate;
    if (launchDate != null) {
      int years = DateTime.now().difference(launchDate).inDays ~/ 365;
      print('Launched: $launchYear ($years years ago)');
    } else {
      print('Unlaunched');
    }
  }
}
```

`Spacecraft` 클래스를 다음과 같이 사용할 수 있습니다:

<?code-excerpt "misc/test/samples_test.dart (use class)" plaster="none"?>
```dart
var voyager = Spacecraft('Voyager I', DateTime(1977, 9, 5));
voyager.describe();

var voyager3 = Spacecraft.unlaunched('Voyager III');
voyager3.describe();
```

Getter, setter, 리디렉팅 생성자, `factory` 생성자, 선택적인 `new`와 `const`, initializer list
를 비롯하여 Dart의 클래스에 대해 [더보기](/guides/language/language-tour#classes).


## Enum

Enum은 해당 타입의 다른 인스턴스가 없도록 미리 정의된 값 또는 인스턴스 집합을 열거하는 수단입니다.

다음은 미리 정의된 행성 타입의 간단한 목록을 정의하는 `enum`의 예제입니다:

<?code-excerpt "misc/lib/samples/spacecraft.dart (simple-enum)"?>
```dart
enum PlanetType { terrestrial, gas, ice }
```

다음은 정의된 상수 인스턴스 집합, 즉 태양계의 행성에 대해 설명하는
클래스의 발전된 enum 선언 예제입니다.

<?code-excerpt "misc/lib/samples/spacecraft.dart (enhanced-enum)"?>
```dart
/// 우리 태양계에 있는 행성들과 행성들의 프로퍼티에 대한 Enum
enum Planet {
  mercury(planetType: PlanetType.terrestrial, moons: 0, hasRings: false),
  venus(planetType: PlanetType.terrestrial, moons: 0, hasRings: false),
  // ···
  uranus(planetType: PlanetType.ice, moons: 27, hasRings: true),
  neptune(planetType: PlanetType.ice, moons: 14, hasRings: true);

  /// 상수를 생성하는 생성자
  const Planet(
      {required this.planetType, required this.moons, required this.hasRings});

  /// 모든 인스턴스 변수들은 final
  final PlanetType planetType;
  final int moons;
  final bool hasRings;

  /// 발전된 enum은 getter와 다른 메서드를 지원합니다.
  bool get isGiant =>
      planetType == PlanetType.gas || planetType == PlanetType.ice;
}
```

다음과 같이 `Planet` enum을 사용할 것입니다:

<?code-excerpt "misc/test/samples_test.dart (use enum)" plaster="none"?>
```dart
final yourPlanet = Planet.earth;

if (!yourPlanet.isGiant) {
  print('Your planet is not a "giant planet".');
}
```

발전된 enum의 요구 사항, 자동으로 도입된 프로퍼티, 열거된 값을 이름으로 접근,
switch문 지원 등을 포함한 Dart의 enum에 대해 
[더보기](/guides/language/language-tour#enums)


## 상속

Dart는 단일 상속이 가능합니다.

<?code-excerpt "misc/lib/samples/spacecraft.dart (extends)"?>
```dart
class Orbiter extends Spacecraft {
  double altitude;

  Orbiter(super.name, DateTime super.launchDate, this.altitude);
}
```

선택적인 `@overrid` 어노테이션, 클래스 확장 등에 대해 
[더보기](/guides/language/language-tour#extending-a-class) 


## Mixin

Mixin은 여러 클래스 계층에서 코드를 재사용하는 방법입니다.
다음은 mixin을 선언하는 코드입니다:

<?code-excerpt "misc/lib/samples/spacecraft.dart (mixin)"?>
```dart
mixin Piloted {
  int astronauts = 1;

  void describeCrew() {
    print('Number of astronauts: $astronauts');
  }
}
```

Mixin의 기능을 클래스에 추가하고 싶다면, 해당 클래스를 mixin으로 확장하면 됩니다.

<?code-excerpt "misc/lib/samples/spacecraft.dart (mixin-use)" replace="/with/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
class PilotedCraft extends Spacecraft [!with!] Piloted {
  // ···
}
{% endprettify %}

이제 `PilotedCraft`는 `astronauts` 필드와 `describeCrew()` 메서드를 가집니다.

Mixin에 대해
[더보기](/guides/language/language-tour#adding-features-to-a-class-mixins).


## 인터페이스와 추상 클래스

Dart에는 `interface` 키워드가 없습니다.
대신, 모든 클래스가 암묵적으로 인터페이스를 정의합니다.
그리므로, 클래스를 `implement` 할 수 있습니다.

<?code-excerpt "misc/lib/samples/spacecraft.dart (implements)"?>
```dart
class MockSpaceship implements Spacecraft {
  // ···
}
```

암묵적 인터페이스에 대해
[더보기](/guides/language/language-tour#implicit-interfaces).

구체적인 클래스로 확장 또는 구현되는 추항 클래스를 생성하는 것이 가능합니다.
추상 클래스는 바디가 비어있는 추상 메서드를 가지는 것이 가능합니다.
Abstract classes can contain abstract methods (with empty bodies).

<?code-excerpt "misc/lib/samples/spacecraft.dart (abstract)" replace="/abstract/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
[!abstract!] class Describable {
  void describe();

  void describeWithEmphasis() {
    print('=========');
    describe();
    print('=========');
  }
}
{% endprettify %}

`Describable`를 확장하는 모든 클래스는, 확장자의 `describe()` 구현을 호출하는
`describeWithEmphasis()` 메서드를 가집니다.

추상 클래스와 메서드에 대해
[더보기](/guides/language/language-tour#abstract-classes).


## Async

`async`과 `await`를 사용해 콜백 지옥에서 벗어나고 코드의 가독성을 높힐 수 있습니다.

<?code-excerpt "misc/test/samples_test.dart (async)" replace="/async/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
const oneSecond = Duration(seconds: 1);
// ···
Future<void> printWithDelay(String message) [!async!] {
  await Future.delayed(oneSecond);
  print(message);
}
{% endprettify %}

위의 메서드는 다음과 같습니다:

<?code-excerpt "misc/test/samples_test.dart (Future.then)"?>
```dart
Future<void> printWithDelay(String message) {
  return Future.delayed(oneSecond).then((_) {
    print(message);
  });
}
```

다음 예제가 보여주듯이, `async`과 `await`는 가독성이 높은
비동기 코드를 작성하는 것을 도와줍니다.

<?code-excerpt "misc/test/samples_test.dart (await)"?>
```dart
Future<void> createDescriptions(Iterable<String> objects) async {
  for (final object in objects) {
    try {
      var file = File('$object.txt');
      if (await file.exists()) {
        var modified = await file.lastModified();
        print(
            'File for $object already exists. It was modified on $modified.');
        continue;
      }
      await file.create();
      await file.writeAsString('Start describing $object in this file.');
    } on IOException catch (e) {
      print('Cannot create description for $object: $e');
    }
  }
}
```
`async*`를 사용해 가독성 높은 코드로 스트림을 생성할 수 있습니다.

<?code-excerpt "misc/test/samples_test.dart (async*)"?>
```dart
Stream<String> report(Spacecraft craft, Iterable<String> objects) async* {
  for (final object in objects) {
    await Future.delayed(oneSecond);
    yield '${craft.name} flies by $object';
  }
}
```

`async` 함수, `Future`, `Stream` 그리고 비동기 루프 (`await for`)을 비롯한
비동기 지원에 대해
[더보기](/guides/language/language-tour#asynchrony-support)


## 예외

예외를 발생시키고 싶다면, `throw`를 사용하세요:

<?code-excerpt "misc/test/samples_test.dart (throw)"?>
```dart
if (astronauts == 0) {
  throw StateError('No astronauts.');
}
```

예외를 캐치하고 싶다면, `try`문을 `on` 또는 `catch` (혹은 모두)와 함께 사용하세요:

<?code-excerpt "misc/test/samples_test.dart (try)"?>
```
try {
  for (final object in flybyObjects) {
    var description = await File('$object.txt').readAsString();
    print(description);
  }
} on IOException catch (e) {
  print('Could not describe object: $e');
} finally {
  flybyObjects.clear();
}
```

위의 코드는 비동기적이라는 것을 알아두세요:
`try`는 `async` 함수 코드와 동기식 코드에서 모두 동작합니다.

스택 트레이스, `rethrow`, 그리고 `Error`와 `Exception`의 차이를 비롯하여
예외에 대해 
[더보기](/guides/language/language-tour#exceptions).


## 다른 토픽

[언어 투어](/guides/language/language-tour)와
[라이브러리 투아](/guides/libraries/library-tour)에 많은 코드 샘플이 존재합니다.
[Dart API reference,]({{site.dart-api}})에도 종종 샘플이 등장합니다.
