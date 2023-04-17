---
title: 호출 가능한 클래스
description: Dart에서 호출 가능한 클래스를 사용하고 생성하는 법을 학습힙니다.
js: [{url: 'https://dartpad.dev/inject_embed.dart.js', defer: true}]
toc: false
---

Dart 클래스의 인스턴스를 함수처럼 호출하고 싶다면, `call()` 메소드를 구현하세요.

`call()` 메서드는 함수를 정의하는 모든 클래스가 함수를 에뮬레이트(emulate)하도록 허용합니다.
이 함수는 일반 [함수][functions] 처럼 매개변수 그리고 반환 타입 같은 기능을 지원합니다.

다음의 예제에서, `WannabeFunction` 클래스는
3개의 문자열을 받아서 각 문자열을 공백으로 구분하고
느낌표를 추가하는 `call()` 함수를 정의합니다. **Run**을 클릭해 코드를 실행하세요.

<?code-excerpt "misc/lib/language_tour/callable_classes.dart"?>
```dart:run-dartpad:height-350px:ga_id-callable_classes
class WannabeFunction {
  String call(String a, String b, String c) => '$a $b $c!';
}

var wf = WannabeFunction();
var out = wf('Hi', 'there,', 'gang');

void main() => print(out);
```

[functions]: /language/functions