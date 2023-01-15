---
title: Dart의 숫자
description: Learn how Dart numbers are slightly different on the web, when that might matter, and how you might adjust your code.
---

Dart 앱들은 대게 다수의 플랫폼을 타겟으로 합니다.
예를 들어, Flutter 앱은 iOS, Android 그리고 웹을 타겟으로 합니다.
앱이 플랫폼별 라이브러리를 사용하거나, 플랫폼에 의존하는 방식으로
숫자를 사용하지 않는 한 동일한 코드를 사용할 수 있습니다.

이번 페이지에서 네이티브와 웹의 숫자 구현 간의 차이점과
코드 작성 방법에 대해 자세히 설명합니다.

{{site.alert.secondary}}
  **다른 언어와 Dart의 숫자 구현**

  Dart는 성능, 코드 크기, 그리고 플랫폼 상호운용성을 위해
  숫자를 위한 플랫폼별 표현과 시멘틱을 항상 허용합니다.

  마찬가지로 C/C++에서 정수 값을 나타내는 `int` 타입은 네이티브 머신 아키텍쳐 (16-, 32-, 64-bit)에
  가장 잘 매핑되도록 플랫폼에 특화되어 있습니다.
  Java에서 분수 값을 나타내는 `float`과 `double` 타입은 원래 모든 플랫폼에서 IEEE 754를 엄격하게
  따르도록 설계되었지만, 효율성의 이유로 이 제약은 완화되었습니다
  (정확환 일관성을 위해서는 `strictfp`가 필요합니다.).
{{site.alert.end}}


## Dart 숫자 표현

Dart에서 모든 숫자는 일반 `Object` 타입 계층의 부분이고,
두 가지 구체적인 숫자 타입으로 정수를 나타내는 `int`와
분수를 나타내는 `double`이 존재합니다.

<img 
  src="/assets/img/number-class-hierarchy.svg" 
  alt="Object is the parent of num, which is the parent of int and double">

플랫폼에 따라서 숫자 타입의 구현에 차이가 있습니다.
특히, Dart는 두 가지 다른 타입을 타겟으로 컴파일합니다.

* **네이티브:** 대부분의 경우이며, 64-bit 모바일 또는 데스크탑 프로세서입니다.
* **웹:** 기본 실행 엔진으로 JavaScript가 사용됩니다.

다음 테이블은 일반적으로 Dart 숫자가 어떻게 구현되는지 보여줍니다:

<table class="table table-striped nowrap">
  <tr>
   <th>표현</th>
   <th>네이티브 <code>int</code></th>
   <th>네이티브 <code>double</code></th>
   <th>웹 <code>int</code></th>
   <th>웹 <code>double</code></th>
  </tr>
  <tr>
   <td><a href="https://en.wikipedia.org/wiki/Two%27s_complement">
     64-bit 부호화된 2의 보수</a>
   </td>
   <td>✅</td>
   <td></td>
   <td></td>
   <td></td>
  </tr>
  <tr>
   <td>
     <a href="https://en.wikipedia.org/wiki/Double-precision_floating-point_format">64-bit 부동소수점</a>
   </td>
   <td></td>
   <td>✅</td>
   <td>✅</td>
   <td>✅</td>
  </tr>
</table>

네이티브를 타겟으로 하는 경우,
`int`는 부호화된 64-bit 정수 표현으로
`double`은 64-bit IEEE 부동소수점 표현으로 프로세서에 맞게 매핑됩니다.

반면에 Dart가 JavaScript를 컴파일하고 상호 운용하는 웹에서는
64-bit 배정도 부동소수점을 단일 숫자 표현으로 사용합니다.
효율성을 위해 Dart는 `int`와 `double`을 64-bit 배정도 부동소수점으로 매핑합니다.
사용할 수 있는 타입 계층은 여전히 동일하지만,
숨겨진 구현이 서로 다르고 얽혀 있습니다.

다음 그림은 네이티브와 웹의 플랫폼별 숫자 타입 계층을 보여줍니다.
그림에서 알 수 있듯이,
네이티브의 구체적인 `int` 타입은 `int` 인터페이스만 구현합니다.
그러나 웹의 `int`에 대한 타입은 `int`와 `double`을 모두 구현합니다.

<img 
  src="/assets/img/number-platform-specific.svg" 
  alt="Implementation classes vary by platform; for JavaScript, the class that implements int also implements double">


{{site.alert.note}}
  Dart는 효율성을 위해 `int`와 `double`을
  다른 방식으로 표현하지만, 이 구현 클래스들은 숨겨져 있습니다(위의 파란색).
  일반적으로, 플랫폼별 타입을 무시해도 되고 `int`와 `double`을 구체적인 타입으로 생각해도 됩니다.
{{site.alert.end}}

웹의 `int`는 분수 부분이 없는 배정도 부동소수점 값으로 표현됩니다.
배정도 부동소수점은 53 비트의 정수 정밀도를 제공하므로 실제로 잘 작동합니다.
그러나, `int` 값은 동시에 항상 `double` 값입니다.


## 동작의 차이

대부분의 정수와 double 산술은 본질적으로 동일하게
작동합니다. 그러나, 특히 코드가 정밀도, 문자열 포매팅 또는 런타임 유형에
대해 엄격하다면 중요한 차이가 존재합니다.

이 섹션에서 설명한 것 처럼 산술 결과가 다를 경우 **플랫폼마다** 다르게 작동하며
**변경**될 수 있습니다.

{{site.alert.note}}
  이 페이지에서 설명하는 플랫폼별 동작은 더 일관적이거나 더 성능이 향상되도록 변경될 수 있습니다.
{{site.alert.end}}


### 정밀도

다음 표는 정밀도에 따라 숫자 표현이 달라지는 것을 보여줍니다.
`math`는 `dart:math` 라이브러리를 나타내며
`math.pow(2, 53)`은 2<sup>53</sup>입니다.
웹에서, 정수는 53 비트 이후에는 정밀도를 잃습니다.
2<sup>53</sup>와 2<sup>53</sup>+1는 같은 값으로 매핑됩니다.
네이티브의 숫자는 63 비트로 값을 나타내고 1 비트로 부호를 나타내므로 위의 두 값은 다른 값으로 매핑됩니다.

2<sup>63</sup>-1 to 2<sup>63</sup>를 비교하면
오버플로의 효과를 확인해볼 수 있습니다.
네이티브에서 후자는 2의 보수 산술에 의해 -2<sup>63</sup>로 오버플로 됩니다.
웹에서는 이러한 값들이 정밀도의 손실로 인한 근사치로
표현되기 때문에 오버플로가 발생하지 않습니다.

<table class="table table-striped nowrap">
  <tr>
   <th>표현</th>
   <th>네이티브</th>
   <th>웹</th>
  </tr>
  <tr>
   <td><code>math.pow(2, 53) - 1</code></td>
   <td><code>9007199254740991</code></td>
   <td><code>9007199254740991</code></td>
  </tr>
  <tr>
   <td><code>math.pow(2, 53)</code></td>
   <td><code>9007199254740992</code></td>
   <td><code>9007199254740992</code></td>
  </tr>
  <tr>
   <td><code>math.pow(2, 53) + 1</code></td>
   <td><code>9007199254740993</code></td>
   <td><code>9007199254740992</code></td>
  </tr>
  <tr>
   <td><code>math.pow(2, 62)</code></td>
   <td><code>4611686018427387904</code></td>
   <td><code>4611686018427388000</code></td>
  </tr>
  <tr>
   <td><code>math.pow(2, 63) - 1</code></td>
   <td><code>9223372036854775807</code></td>
   <td><code>9223372036854776000</code></td>
  </tr>
  <tr>
   <td><code>math.pow(2, 63)</code></td>
   <td><code>-9223372036854775808</code></td>
   <td><code>9223372036854776000</code></td>
  </tr>
  <tr>
   <td><code>math.pow(2, 64)</code></td>
   <td><code>0</code></td>
   <td><code>18446744073709552000</code></td>
  </tr>
</table>


### Identity

On native platforms, `double` and `int` are distinct types:
no value can be both a `double` and an `int` at the same time.
On the web, that isn't true.
Because of this difference,
identity can differ between platforms,
although equality (`==`) doesn't.

The following table shows some expressions that use equality and identity.
The equality expressions are the same on native and web;
the identity expressions are usually different.

<table class="table table-striped nowrap">
  <tr>
   <th>Expression</th>
   <th>Native</th>
   <th>Web</th>
  </tr>
  <tr>
   <td><code>1.0 == 1</code></td>
   <td><code>true</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>identical(1.0, 1)</code></td>
   <td><code>false</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>0.0 == -0.0</code></td>
   <td><code>true</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>identical(0.0, -0.0)</code></td>
   <td><code>false</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>double.nan == double.nan</code></td>
   <td><code>false</code></td>
   <td><code>false</code></td>
  </tr>
  <tr>
   <td><code>identical(double.nan, double.nan)</code></td>
   <td><code>true</code></td>
   <td><code>false</code></td>
  </tr>
  <tr>
   <td><code>double.infinity == double.infinity</code></td>
   <td><code>true</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>identical(double.infinity, double.infinity)</code></td>
   <td><code>true</code></td>
   <td><code>true</code></td>
  </tr>
</table>


### Types and type checking

On the web, the underlying `int` type is like a subtype of `double`:
it’s a double-precision value without a fractional part.
In fact, a type check on the web of the form `x is int`
returns true if `x` is a number (`double`) with
a zero-valued fractional part.

As a result, the following are true on the web:

* All Dart numbers (values of type `num`) are `double`.
* A Dart number can be both a `double` and an `int` at the same time.

These facts affect `is` checks and `runtimeType` properties.
A side effect is that `double.infinity` is interpreted as an `int`.
Because this is a platform-specific behavior,
it might change in the future.

<table class="table table-striped nowrap">
  <tr>
   <th>Expression</th>
   <th>Native</th>
   <th>Web</th>
  </tr>
  <tr>
   <td><code>1 is int</code></td>
   <td><code>true</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>1 is double</code></td>
   <td><code>false</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>1.0 is int</code></td>
   <td><code>false</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>1.0 is double</code></td>
   <td><code>true</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>(0.5 + 0.5) is int</code></td>
   <td><code>false</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>(0.5 + 0.5) is double</code></td>
   <td><code>true</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>3.14 is int</code></td>
   <td><code>false</code></td>
   <td><code>false</code></td>
  </tr>
  <tr>
   <td><code>3.14 is double</code></td>
   <td><code>true</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>double.infinity is int</code></td>
   <td><code>false</code></td>
   <td><code>true</code></td>
  </tr>
  <tr>
   <td><code>double.nan is int</code></td>
   <td><code>false</code></td>
   <td><code>false</code></td>
  </tr>
  <tr>
   <td><code>1.0.runtimeType</code></td>
   <td><code>double</code></td>
   <td><code>int</code></td>
  </tr>
  <tr>
   <td><code>1.runtimeType</code></td>
   <td><code>int</code></td>
   <td><code>int</code></td>
  </tr>
  <tr>
   <td><code>1.5.runtimeType</code></td>
   <td><code>double</code></td>
   <td><code>double</code></td>
  </tr>
</table>


### Bitwise operations

For performance reasons on the web,
bitwise (`&`, `|`, `^`, `~`) and shift (`<<`,`>>`, `>>>`) operators on `int`
use the native JavaScript equivalents.
In JavaScript, the operands are truncated to 32-bit integers
that are treated as unsigned.
This treatment can lead to surprising results on larger numbers.
In particular, if operands are negative or don't fit into 32 bits,
they're likely to produce different results between native and web.

The following table shows how native and web platforms
treat bitwise and shift operators when the operands
are either negative or close to 32 bits:

<table class="table table-striped nowrap">
  <tr>
   <th>Expression</th>
   <th>Native</th>
   <th>Web</th>
  </tr>
  <tr>
   <td><code>-1 >> 0</code></td>
   <td><code>-1</code></td>
   <td><code>4294967295</code></td>
  </tr>
  <tr>
   <td><code>-1 ^ 2</code></td>
   <td><code>-3</code></td>
   <td><code>4294967293</code></td>
  </tr>
  <tr>
   <td><code>math.pow(2, 32).toInt()</code></td>
   <td><code>4294967296</code></td>
   <td><code>4294967296</code></td>
  </tr>
  <tr>
   <td><code>math.pow(2, 32).toInt() >> 1</code></td>
   <td><code>2147483648</code></td>
   <td><code>0</code></td>
  </tr>
  <tr>
   <td><code>(math.pow(2, 32).toInt()-1) >> 1</code></td>
   <td><code>2147483647</code></td>
   <td><code>2147483647</code></td>
  </tr>
</table>


### String representation

On the web, Dart generally defers to JavaScript to convert a number to a string
(for example, for a `print`).
The following table demonstrates how
converting the expressions in the first column can lead to different results.

<table class="table table-striped nowrap">
  <tr>
   <th>Expression</th>
   <th>Native <code>toString()</code></th>
   <th>Web <code>toString()</code></th>
  </tr>
  <tr>
   <td><code>1</code></td>
   <td><code>"1"</code></td>
   <td><code>"1"</code></td>
  </tr>
  <tr>
   <td><code>1.0</code></td>
   <td><code>"1.0"</code></td>
   <td><code>"1"</code></td>
  </tr>
  <tr>
   <td><code>(0.5 + 0.5)</code></td>
   <td><code>"1.0"</code></td>
   <td><code>"1"</code></td>
  </tr>
  <tr>
   <td><code>1.5</code></td>
   <td><code>"1.5"</code></td>
   <td><code>"1.5"</code></td>
  </tr>
  <tr>
   <td><code>-0</code></td>
   <td><code>"0"</code></td>
   <td><code>"-0.0"</code></td>
  </tr>
  <tr>
   <td><code>math.pow(2, 0)</code></td>
   <td><code>"1"</code></td>
   <td><code>"1"</code></td>
  </tr>
  <tr>
   <td><code>math.pow(2, 80)</code></td>
   <td><code>"0"</code></td>
   <td><code>"1.2089258196146292e+24"</code></td>
  </tr>
</table>


## What should you do?

Usually, you don't need to change your numeric code.
Dart code has been running on both native and web platforms for years,
and number implementation differences are rarely a problem.
Common, typical code—such as iterating through a range of small integers 
and indexing a list—behaves the same.

If you have tests or assertions that compare string results,
write them in a platform-resilient manner.
For example, suppose you're testing the value of string expressions
that have embedded numbers:

```dart
void main() {
  var count = 10.0 * 2;
  var message = "$count cows";
  if (message != "20.0 cows") throw Exception("Unexpected: $message");
}
```

The preceding code succeeds on native platforms but throws on the web
because `message` is `"20 cows"` (no decimal) on the web.
As an alternative, you might write the condition as follows,
so it passes on both native and web platforms:

```dart
if (message != "${20.0} cows") throw ... 
```

For bit manipulation, consider explicitly operating on 32-bit chunks,
which are consistent on all platforms.
To force a signed interpretation of a 32-bit chunk,
use `int.toSigned(32)`.

For other cases where precision matters,
consider other numeric types.
The [`BigInt`][] type
provides arbitrary-precision integers on both native and web.
The [`fixnum`][] package
provides strict 64-bit signed numbers, even on the web.
Use these types with care, though:
they often result in significantly bigger and slower code.

[`BigInt`]: {{site.dart-api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/BigInt-class.html
[`fixnum`]: {{site.pub-pkg}}/fixnum
