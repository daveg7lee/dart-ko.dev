---
title: 효율적인 Dart
description: 일관되고 지속가능하며 효과적인 Dart 라이브러리를 빌드하는 최고의 관례들
permalink: /guides/language/effective-dart
nextpage:
  url: /guides/language/effective-dart/style
  title: Style
---

지난 몇 년 동안, 우리는 많은 Dart 코드를 작성했고 어떤것이 제대로 작동하는지 아닌지에 대해 많은 것을 배웠습니다.
우리가 공유하는 정보와 함께라면 일관성있고 견고하며 빠른 코드 작성이 가능할 것입니다.
두가지 중심이 되는 주제가 있습니다.

 1. **일관되게 작성하세요.** 포매팅(foramtting)과 케이싱(casing)과 같은 것들에 대해,
    어느 것이 더 나은지에 대한 논쟁은 주관적이며 해결이 불가능합니다.
    우리가 알고 있는 것은 *일관성*이 객관적으로 도움이 된다는 것입니다.

    두 코드 피스가 다르게 느껴진다면, 그 둘은 각기 다른 의미를 가져야 합니다.
    어떤 코드가 눈길을 끈다면, 눈길을 끌만한 이유가 있어야 합니다.

 2. **간단하게 작성하세요.** Dart는 개발자에게 친숙하도록 디자인 되었습니다. 그렇기 때문에
    C, Java, JavaScript와 같은 다른 언어들의 statements나 expressions를 물려받았습니다.
    하지만 우리는 그런 언어들을 개선할 여지가 많이 있기 때문에 Dart를 개발하였습니다.
    문자열 보간 부터 initializing formals까지, 개발자들의 의도를 간단하고 쉽게 표현하기 위해
    Dart는 다양한 기능들을 가지고 있습니다.

    코드를 표현할 다양한 방법이 있다면, 일반적으로 간결한 것을 선택하는 것이 좋을 것입니다.
    그것이 모든 프로그램을 한 줄로 표현하도록 [code golf][]하라는 의미는 아닙니다.
    Dart를 사용하는 개발자들의 목표는 불필요하게 *빽빽*하지 않고 *실속*있는 코드를 작성하는 것입니다.

[code golf]: https://kr.wikipedia.org/wiki/Code_golf

## 가이드

가이드를 쉽게 이해하도록 하기 위해 우리는 가이드라인을 몇 가지 페이지로 구분하였습니다:

  * **[Style Guide][]** &ndash; 이 가이드에서는 코드를 배치하고 구성하는 규칙에 대해 정의하고,
    [dart format]에서 다루지 않는 것들을 다룹니다. Style guide에서는 
    `camelCase`, `using_underscores`와 같이 식별자들이 어떻게 포맷되는지 구체적으로 알려줍니다.
    
  * **[Documentation Guide][]** &ndash; 이 가이드에서는 주석에 대한 모든 것을
    알려줍니다. 문서 주석 부터, 일반적인 코드에 대한 주석을 모두 다룹니다.

  * **[Usage Guide][]** &ndash; 이 가이드 에서는 기능을 구현하기 위해
    언어의 기능을 최대한 활용하는 방법에 대해 알려줍니다.
    Statement나 expression을 여기에서 다룹니다.

  * **[Design Guide][]** &ndash; 이 가이드는 가장 소프트한 파트이지만, 가장 넓은 범위를
    다룹니다. 라이브러리를 위한 일관되고 사용 가능한 API를 설계하는 법을 알려줍니다.
    Type signature나 declaration에 대해 궁금하다면, 이 가이드를 살펴보세요.

모든 가이드의 링크는 다음을 참고하세요
[요약](#요약).

[dart format]: /tools/dart-format
[style guide]: /guides/language/effective-dart/style
[documentation guide]: /guides/language/effective-dart/documentation
[usage guide]: /guides/language/effective-dart/usage
[design guide]: /guides/language/effective-dart/design

## 가이드 읽는 법

각 가이드는 여러 개의 섹션으로 구분됩니다. 섹션은 몇 가지의 가이드라인을 가지고 있습니다.
개별 가이드라인은 아래의 단어 중 하나를 포함합니다:

* **하십시오** 해당 가이드라인들은 항상 지켜져야 하는 관례을 말합니다.
  이 관례들을 어길 타당한 이유가 거의 없을 겁니다.

* **하지 마십시오** 대부분의 상황에서 좋지 않는 아이디어에 대한 것으로, 위와 반대되는
  상황에 대한 가이드라인입니다. Dart의 역사적 부담이 적기 때문에, 우리는 다른 언어들 만큼 이러한
  지침이 많지 않기를 바랍니다.

* **지향 하십시오** 따르는 것이 *바람직한* 관례에 대한 가이드라인 입니다. 그러나,
  이 지침을 따르지 않아야 하는 상황이 존재할 것입니다. 이 가이드라인을 지키지 않는 것의
  의미를 충분히 이해했는지 확인하세요.

* **피하십시오** "지향"해야하는 가이드 라인과 반대의 상황을 말합니다:
하지 말아야 할 것들이지만, 드물게 합리적인 이유가 있을 수 있는 것들을 말합니다.

* **고려 하십시오** 상황, 전례 그리고 자신의 개인적인 선호도로 인해
  따를 수도, 그러지 않을 수도 있는 관례들에 대한 가이드라인입니다.

몇몇 가이드라인들은 위의 규칙들이 적용되지 *않는* **예외**들을 포함하고 있습니다. 그런 예외들이 있다면,
완전하지 않으므로 여러 케이스들에 대해 본인의 판단이 필요합니다.

위의 이야기는 당신이 이 가이드라인들을 지키지 않으면 큰일 날 것처럼 느껴질 수 있지만, 그렇게 극단적이진 않습니다.
위의 가이드라인들의 대부분은 상식적인 것이고 우리는 모두 합리적인 사람들입니다. 
우리의 목표는 훌륭하고 가독성이 좋으며 지속가능한 코드를 작성하는 것입니다.

Dart analyzer는 linter를 제공하여 이런 가이드라인들을 지키는 
훌륭하고 일관적인 코드를 작성 할 수 있게 해줍니다.
가이드라인들 따르도록 해주는 [linter 규칙][lints]이 존재한다면,
해당 가이드라인이 규칙과 링크해 줄 것입니다.
그 링크들은 다음과 같은 형식을 따릅니다:

{% include linter-rule-mention.md rule="unnecessary_getters_setters" %}

Linter 사용법을 배우고 싶다면,
[Linter 규칙 활성화하기][]와
[linter 규칙][lints]들의 목록을 참고하세요.

[Linter 규칙 활성화하기]: /guides/language/analysis-options#enabling-linter-rules
[lints]: /tools/linter-rules

## 용어 해설

가이드라인을 간단하게 유지하기 위해, 몇 가지 약칭를 사용하여 Dart의 구조를 나타냅니다.

* **라이브러리 멤버**는 getter, setter나 함수 같은 최상위 필드 입니다.
  기본적으로, 타입이 아닌 최상위 수준의 모든 항목들을 말합니다.

* **클래스 멤버**는 클래스에 선언되어 있는 생성자, 필드, getter, setter 함수
  그리고 연산자들을 의미합니다. 클래스 멤버들은 instance 혹은 static일 수 있고,
  추상적이거나 구체적일 수도 있습니다.

* **멤버**는 라이브러리 또는 클래스 멤버를 의미합니다.

* **변수**는 일반적으로 최상위 변수, 파라미터, 지역 변수를 의미합니다.
  Static 또는 instance 필드를 포함하지 않습니다.

* **타입**은 class, typedef, 또는 enum과 같이 이름이 있는 타입 선언을 의미합니다.

* **프로퍼티**는 최상위 변수, getter (클래스 내부 또는 최상위에 위치, instance 또는 static),
  setter (getter와 동일), 또는 필드 (instance 또는 static)를 의미합니다.
  거의 모든 "필드 같이" 명명된 구문들을 나타냅니다.

## 요약

{% include_relative toc.md %}
