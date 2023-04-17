---
title: Mixins
description: Dart의 클래스에 기능을 추가하는 방법에 대해 학습합니다.
toc: false
---

<?code-excerpt replace="/ *\/\/\s+ignore_for_file:[^\n]+\n//g; /(^|\n) *\/\/\s+ignore:[^\n]+\n/$1/g; /(\n[^\n]+) *\/\/\s+ignore:[^\n]+\n/$1\n/g; / *\/\/\s+ignore:[^\n]+//g; /([A-Z]\w*)\d\b/$1/g"?>

Mixins은 다수의 클래스 계층에서 클래스의 코드를 재사용 할 수 있는 방법입니다.

Mixin을 _사용_ 하려면, 다음 코드처럼 `with` 키워드와 사용 할 mixin의 이름을 명시하면 됩니다:

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

Mixin을 _구현_하려면 Object를 확장하며, 생성자가 없는 클래스를 생성하세요.
Mixin을 일반 클래스로 사용할 수 없도록 하려면 `class` 대신 `mixin` 키워드를 사용하세요:

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

Mixin을 사용할 수 있는 타입을 제한할 수도 있습니다.
예를 들어, mixin이 정의하지 않은 메서드를 호출할 수 있는지에 따라 달라질 수 있습니다.
다음 예제처럼 `on` 키워드로 사용할 수 있는 부모 클래스를 제한함으로써 mixin의 사용을 제한할 수 있습니다:

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

위의 코드에서 `Musician` 클래스를 확장, 구현하는 클래스들만 `MusicalPerformer` mixin을 사용 할 수 있습니다.
`SingerDancer`가 `Musician`을 확장하기 때문에, `SingerDancer`는 `MusicalPerformer` mixin을 사용 할 수 있습니다.
