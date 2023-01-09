---
title: Dart 타입 시스템
description: 타입 안전성을 지키는 Dart 코드를 작성하는 이유와 방법.
---
<?code-excerpt replace="/ *\/\/\s+ignore_for_file:[^\n]+\n//g; /([A-Z]\w*)\d\b/$1/g; /\b(main)\d\b/$1/g; /(^|\n) *\/\/\s+ignore:[^\n]+\n/$1/g; /(\n[^\n]+) *\/\/\s+ignore:[^\n]+\n/$1\n/g"?>
<?code-excerpt path-base="type_system"?>

Dart는 타입이 안전한 프로그래밍 언어입니다: Dart는 변수값이 항상 변수의 정적 또는 안전한 타입과
일치하는지 확인하기 위해 정적 타입 검사와 [런타임 검사](#runtime-checks)를 사용합니다.
_타입_ 은 필수적이지만, [타입 추론](#type-inference) 덕분에
타입 _어노테이션_ 은 선택적입니다.

타입을 포함하여 Dart 언어에 대한 전체적인 소개는
[언어 개요](/guides/language/language-tour)를 참고하세요.

정적 타입 검사를 사용하면 Dart의 [정적 분석][analysis]을
사용하여 컴파일 타임에 버그를 찾을 수 있습니다.
제네릭 클래스에 타입 어노테이션을 추가하여 대부분의 정적 분석 오류를 수정할 수 있습니다.
가장 일반적인 제네릭 클래스는 컬렉션 타입인 `List<T>`와 `Map<K,V>`입니다.

예를 들어, 아래 코드에서 `main()`은 리스트를 생성하고, 이를 정수 리스트로 출력하는
`printInts()` 함수에 전달합니다.

{:.fails-sa}
<?code-excerpt "lib/strong_analysis.dart (opening-example)" replace="/list(?=\))/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
void printInts(List<int> a) => print(a);

void main() {
  final list = [];
  list.add(1);
  list.add('2');
  printInts([!list!]);
}
{% endprettify %}

위의 코드는 `printInts(list)`를 호출할 때 `list`에 대해
타입 에러를 발생시킵니다 (강조 표시됨):

{:.console-output}
<?code-excerpt "analyzer-results-stable.txt" retain="/strong_analysis.*List.*argument_type_not_assignable/" replace="/-(.*?):(.*?):(.*?)-/-/g; /. • (lib|test)\/\w+\.dart:\d+:\d+//g"?>
```nocode
error - The argument type 'List<dynamic>' can't be assigned to the parameter type 'List<int>'. - argument_type_not_assignable
```

하이라이트된 에러는 `list<dynamic>`에서 `List<int>`로의 잘못된 암묵적 변환 때문입니다.
`list` 변수는 `List<dynamic>`을 정적 타입으로 가집니다. 이는 변수의 초기화 선언인
`var list = []` 가 analyzer에게 `dynamic` 보다 더 구체적인 타입 매개변수를
유추할 수 있는 충분한 정보를 제공하지 않기 때문입니다. `printInts()` 함수는 `List<int>`
타입의 인수를 예상하므로 타입의 불일치가 발생합니다.

리스트를 생성할 때 타입 어노테이션 `<int>` (코드에 하이라이트된 부분)을 추가하면
analyzer가 문자열 인수를 `int`형 매개변수에 할당할 수 없다는 메시지를 표시합니다.
`list.add('2')`에서 문자열 따옴표를 제거하면 코드가 정적 분석을 통과하고
정상적으로 실행됩니다.

{:.passes-sa}
<?code-excerpt "test/strong_test.dart (opening-example)" replace="/<int.(?=\[)|2/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
void printInts(List<int> a) => print(a);

void main() {
  final list = [!<int>!][];
  list.add(1);
  list.add([!2!]);
  printInts(list);
}
{% endprettify %}

[DartPad에서 체험해보세요!]({{site.dartpad}}/25074a51a00c71b4b000f33b688dedd0).

## 타입 안전성(soundness)이란?

*타입 안전성(soundness)*이란 프로그램이 유효하지 않는 상태가 되지 않도록 하는 것입니다.
안전한 *타입 시스템*은 프로그램이 표현식이 표현식의 정적 타입과 일치하지 않는 값으로
평가되는 상태에 진입할 수 없음을 의미합니다. 예를 들어 표현식의 정적 유형이 `String`인
경우 평가할 때 문자열만 얻을 수 있도록 런타임에서 보장됩니다.

Java나 C#의 타입 시스템 처럼 Dart의 타입 시스템도 안전합니다.
Dart는 정적 검사 (컴파일 타임 에러)와 런타임 검사를 조합하여 타입 안전성을 강요합니다.
예를 들어 `String`을 `int`에 할당하는 것은 컴파일 타입 에러입니다. 객체가 `String`이 아닌 경우
`as String`을 사용하여 객체를 `String`으로 변환하면 런타임 에러가 발생하여 실패합니다.


## 타입 안전성의 이점

안전한 타입 시스템은 많은 장점을 가지고 있습니다:

* 타입 관련 버그를 컴파일 타임에 찾을 수 있습니다.<br>
  안전한 타입 시스템은 코드의 타입이 애매하지 않도록 강제하므로,
  런타임에 발견하기 어려울 수 있는 타입 관련 버그를 컴파일 타임에 찾아줍니다.

* 코드의 가독성이 높아집니다.<br>
  코드를 작성할 때 특정 타입을 가지는 값에 의존하게 되기 때문에, 가독성이 높아집니다.
  타입이 안전한 Dart에서 타입은 거짓말을 하지 않습니다.

* 코드의 유지관리가 쉬워집니다.<br>
  안전한 타입 시스템은 코드를 수정할 때 영향을 받는 다른 코드에 대해 경고해줍니다.

* 더 나은 AOT(Ahead-Of-Time) 컴파일을 제공합니다.<br>
  AOT 컴파일은 타입 없이도 가능하지만, 생성된 코드는 훨씬 비효율적입니다.


## 정적 검사를 위한 팁

정적 타이핑 규칙은 대부분 이해하기 쉽습니다.
다음은 몇 가지 애매한 규칙들 입니다:

* 메서드를 재정의할 때 타입이 안전한 반환 값을 사용하세요.
* 메서드를 재정의할 때 타입이 안전한 매개변수를 사용하세요.
* 동적으로 타입이 지정된 List를 타입이 지정된 List 처럼 사용하지 마세요.

다음의 타입 계층을 사용하여 이러한 규칙에 대해 자세히 살펴봅시다:

<img src="images/type-hierarchy.png" alt="a hierarchy of animals where the supertype is Animal and the subtypes are Alligator, Cat, and HoneyBadger. Cat has the subtypes of Lion and MaineCoon">

<a name="use-proper-return-types"></a>

### 메서드를 재정의할 때 타입이 안전한 반환 값을 사용하세요.

자식 클래스 메서드의 반환 값의 타입은 부모 클래스의 메서드 또는 해당 서브타입의 반환 값의 타입과 동일해야 합니다.
`Animal` 클래스의 getter 메서드를 다음과 같이 생성합니다:

<?code-excerpt "lib/animal.dart (Animal)" replace="/Animal get.*/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
class Animal {
  void chase(Animal a) { ... }
  [!Animal get parent => ...!]
}
{% endprettify %}

`부모`의 getter 메서드는 `Animal`을 반환합니다. 자식 클래스인 `HoneyBadger`의 getter 메서드의 반환 타입을
`HoneyBadger` 또는 `Animal`의 다른 서브타입으로 대체할 수 있지만, 관련이 없는 타입으로는 불가능합니다.

{:.passes-sa}
<?code-excerpt "lib/animal.dart (HoneyBadger)" replace="/(\w+)(?= get)/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
class HoneyBadger extends Animal {
  @override
  void chase(Animal a) { ... }

  @override
  [!HoneyBadger!] get parent => ...
}
{% endprettify %}

{:.fails-sa}
{% prettify dart tag=pre+code %}
class HoneyBadger extends Animal {
  @override
  void chase(Animal a) { ... }
  
  @override
  [!Root!] get parent => ...
}
{% endprettify %}

<a name="use-proper-param-types"></a>

### 메서드를 재정의할 때 타입이 안전한 매개변수를 사용하세요.

자식 클래스 메서드의 매개변수는 부모 클래스 메서드의 매개변수 또는 해당 매개변수의
부모 타입과 동일한 타입이어야 합니다. 원래의 매개변수 타입을 대체하기 위해 서브타입을 사용하지 마세요.
그러면 매개변수 타입이 "좁아"집니다.

{{site.alert.note}}
  합당한 이유로 서브타입을 사용해야 한다면,  [`covariant` 키워드]
  (/guides/language/sound-problems#the-covariant-keyword)의 사용을 고려해보세요.
{{site.alert.end}}

`Animal` 클래스의 `chase(Animal)` 메서드를 다음과 같이 작성합니다:

<?code-excerpt "lib/animal.dart (Animal)" replace="/void chase.*/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
class Animal {
  [!void chase(Animal a) { ... }!]
  Animal get parent => ...
}
{% endprettify %}

`chase()` 메서드의 매개변수 타입은 `Animal` 입니다. `HoneyBadger`는 어느 것이든 선택할 수 있습니다.
따라서 `chase()` 메서드를 재정의할 때 매개변수 타입을 어떤 타입의 `Object`로도 지정할 수 있습니다.

{:.passes-sa}
<?code-excerpt "lib/animal.dart (chase-Object)" replace="/Object/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
class HoneyBadger extends Animal {
  @override
  void chase([!Object!] a) { ... }

  @override
  Animal get parent => ...
}
{% endprettify %}

`Mouse`는 `Animal`의 하위 클래스이며 아래 코드는 `chase()` 메서드의 매개변수 범위를
`Animal`에서 `Mouse`로 좁힙니다.

{:.fails-sa}
{% prettify dart tag=pre+code %}
class Mouse extends Animal {...}

class Cat extends Animal {
  @override
  void chase([!Mouse!] x) { ... }
}
{% endprettify %}

다음 코드는 a를 고양이 객체로 선언하고, 악어 객체를 넘겨줄 수 있기 때문에 타입이 안전하지 않습니다:

{% prettify dart tag=pre+code %}
Animal a = Cat();
a.chase([!Alligator!]()); // 타입과 고양이가 안전하지 않습니다.
{% endprettify %}

### 동적으로 타입이 지정된 리스트를 타입이 지정된 리스트처럼 사용하지 마세요.

`dynamic` 리스트는 다른 타입의 요소들을 한 리스트에 추가할 때 유용합니다.
그러나, `dynamic` 리스트를 타입이 지정된 리스트처럼 사용할 수 없습니다.

이 규칙은 제네릭 타입의 인스턴스에도 적용됩니다.

다음 코드는 `Dog` 객체를 가지는 `dynamic` 리스트를 생성하고
해당 리스트를 `Cat` 타입의 리스트에 할당합니다. 이 코드는 정적 분석에서 에러를 발생시킵니다.

{:.fails-sa}
{% prettify dart tag=pre+code %}
class Cat extends Animal { ... }

class Dog extends Animal { ... }

void main() {
  List<Cat> foo = [!<dynamic>!][Dog()]; // Error
  List<dynamic> bar = <dynamic>[Dog(), Cat()]; // OK
}
{% endprettify %}

## 런타임 검사

런타임 검사는 컴파일 타임에서 감지하지 못하는 타입 안전성 이슈를 처리합니다.

예를 들어, 다음 코드는 강아지의 리스트를 고양이의 리스트로 캐스팅하는 것은 에러이므로
런타임에 예외를 발생시킵니다:

{:.runtime-fail}
<?code-excerpt "test/strong_test.dart (runtime-checks)" replace="/animals as[^;]*/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
void main() {
  List<Animal> animals = [Dog()];
  List<Cat> cats = [!animals as List<Cat>!];
}
{% endprettify %}


## Type inference

The analyzer can infer types for fields, methods, local variables,
and most generic type arguments.
When the analyzer doesn't have enough information to infer
a specific type, it uses the `dynamic` type.

Here's an example of how type inference works with generics.
In this example, a variable named `arguments` holds a map that
pairs string keys with values of various types.

If you explicitly type the variable, you might write this:

<?code-excerpt "lib/strong_analysis.dart (type-inference-1-orig)" replace="/Map<String, dynamic\x3E/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
[!Map<String, dynamic>!] arguments = {'argA': 'hello', 'argB': 42};
{% endprettify %}

Alternatively, you can use `var` or `final` and let Dart infer the type:

<?code-excerpt "lib/strong_analysis.dart (type-inference-1)" replace="/var/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
[!var!] arguments = {'argA': 'hello', 'argB': 42}; // Map<String, Object>
{% endprettify %}

The map literal infers its type from its entries,
and then the variable infers its type from the map literal's type.
In this map, the keys are both strings, but the values have different
types (`String` and `int`, which have the upper bound `Object`).
So the map literal has the type `Map<String, Object>`,
and so does the `arguments` variable.


### Field and method inference

A field or method that has no specified type and that overrides
a field or method from the superclass, inherits the type of the
superclass method or field.

A field that does not have a declared or inherited type but that is declared
with an initial value, gets an inferred type based on the initial value.

### Static field inference

Static fields and variables get their types inferred from their
initializer. Note that inference fails if it encounters a cycle
(that is, inferring a type for the variable depends on knowing the
type of that variable).

### Local variable inference

Local variable types are inferred from their initializer, if any.
Subsequent assignments are not taken into account.
This may mean that too precise a type may be inferred.
If so, you can add a type annotation.

{:.fails-sa}
<?code-excerpt "lib/strong_analysis.dart (local-var-type-inference-error)"?>
{% prettify dart tag=pre+code %}
var x = 3; // x is inferred as an int.
x = 4.0;
{% endprettify %}

{:.passes-sa}
<?code-excerpt "lib/strong_analysis.dart (local-var-type-inference-ok)"?>
{% prettify dart tag=pre+code %}
num y = 3; // A num can be double or int.
y = 4.0;
{% endprettify %}

### Type argument inference

Type arguments to constructor calls and
[generic method](/guides/language/language-tour#using-generic-methods) invocations
are inferred based on a combination of downward information from the context
of occurrence, and upward information from the arguments to the constructor
or generic method. If inference is not doing what you want or expect,
you can always explicitly specify the type arguments.

{:.passes-sa}
<?code-excerpt "lib/strong_analysis.dart (type-arg-inference)"?>
{% prettify dart tag=pre+code %}
// Inferred as if you wrote <int>[].
List<int> listOfInt = [];

// Inferred as if you wrote <double>[3.0].
var listOfDouble = [3.0];

// Inferred as Iterable<int>.
var ints = listOfDouble.map((x) => x.toInt());
{% endprettify %}

In the last example, `x` is inferred as `double` using downward information.
The return type of the closure is inferred as `int` using upward information.
Dart uses this return type as upward information when inferring the `map()`
method's type argument: `<int>`.


## Substituting types

When you override a method, you are replacing something of one type (in the
old method) with something that might have a new type (in the new method).
Similarly, when you pass an argument to a function,
you are replacing something that has one type (a parameter
with a declared type) with something that has another type
(the actual argument). When can you replace something that
has one type with something that has a subtype or a supertype?

When substituting types, it helps to think in terms of _consumers_
and _producers_. A consumer absorbs a type and a producer generates a type.

**You can replace a consumer's type with a supertype and a producer's
type with a subtype.**

Let's look at examples of simple type assignment and assignment with
generic types.

### Simple type assignment

When assigning objects to objects, when can you replace a type with a
different type? The answer depends on whether the object is a consumer
or a producer.

Consider the following type hierarchy:

<img src="images/type-hierarchy.png" alt="a hierarchy of animals where the supertype is Animal and the subtypes are Alligator, Cat, and HoneyBadger. Cat has the subtypes of Lion and MaineCoon">

Consider the following simple assignment where `Cat c` is a _consumer_
and `Cat()` is a _producer_:

<?code-excerpt "lib/strong_analysis.dart (Cat-Cat-ok)"?>
{% prettify dart tag=pre+code %}
Cat c = Cat();
{% endprettify %}

In a consuming position, it's safe to replace something that consumes a
specific type (`Cat`) with something that consumes anything (`Animal`),
so replacing `Cat c` with `Animal c` is allowed, because `Animal` is
a supertype of `Cat`.

{:.passes-sa}
<?code-excerpt "lib/strong_analysis.dart (Animal-Cat-ok)"?>
{% prettify dart tag=pre+code %}
Animal c = Cat();
{% endprettify %}

But replacing `Cat c` with `MaineCoon c` breaks type safety, because the
superclass may provide a type of Cat with different behaviors, such
as `Lion`:

{:.fails-sa}
<?code-excerpt "lib/strong_analysis.dart (MaineCoon-Cat-err)"?>
{% prettify dart tag=pre+code %}
MaineCoon c = Cat();
{% endprettify %}

In a producing position, it's safe to replace something that produces a
type (`Cat`) with a more specific type (`MaineCoon`). So, the following
is allowed:

{:.passes-sa}
<?code-excerpt "lib/strong_analysis.dart (Cat-MaineCoon-ok)"?>
{% prettify dart tag=pre+code %}
Cat c = MaineCoon();
{% endprettify %}

### Generic type assignment

Are the rules the same for generic types? Yes. Consider the hierarchy
of lists of animals—a `List` of `Cat` is a subtype of a `List` of
`Animal`, and a supertype of a `List` of `MaineCoon`:

<img src="images/type-hierarchy-generics.png" alt="List<Animal> -> List<Cat> -> List<MaineCoon>">

In the following example, 
you can assign a `MaineCoon` list to `myCats`
because `List<MaineCoon>` is a subtype of `List<Cat>`:

{:.passes-sa}
<?code-excerpt "lib/strong_analysis.dart (generic-type-assignment-MaineCoon)" replace="/<MaineCoon/<[!MaineCoon!]/g"?>
{% prettify dart tag=pre+code %}
List<[!MaineCoon!]> myMaineCoons = ...
List<Cat> myCats = myMaineCoons;
{% endprettify %}

What about going in the other direction? 
Can you assign an `Animal` list to a `List<Cat>`?

{:.fails-sa}
<?code-excerpt "lib/strong_analysis.dart (generic-type-assignment-Animal)" replace="/<Animal/<[!Animal!]/g"?>
{% prettify dart tag=pre+code %}
List<[!Animal!]> myAnimals = ...
List<Cat> myCats = myAnimals;
{% endprettify %}

This assignment doesn't pass static analysis 
because it creates an implicit downcast, 
which is disallowed from non-`dynamic` types such as `Animal`.

{{site.alert.version-note}}
  In packages that use a [language version][] before 2.12
  (when support for [null safety][] was introduced),
  code can implicitly downcast from these non-`dynamic` types.
  You can disallow non-`dynamic` downcasts in a pre-2.12 project
  by specifying `implicit-casts: false` 
  in the [analysis options file.][analysis]
{{site.alert.end}}

To make this type of code pass static analysis, 
you can use an explicit cast. 

<?code-excerpt "lib/strong_analysis.dart (generic-type-assignment-implied-cast)" replace="/as.*(?=;)/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
List<Animal> myAnimals = ...
List<Cat> myCats = myAnimals [!as List<Cat>!];
{% endprettify %}

An explicit cast might still fail at runtime, though,
depending on the actual type of the list being cast (`myAnimals`).

### Methods

When overriding a method, the producer and consumer rules still apply.
For example:

<img src="images/consumer-producer-methods.png" alt="Animal class showing the chase method as the consumer and the parent getter as the producer">

For a consumer (such as the `chase(Animal)` method), you can replace
the parameter type with a supertype. For a producer (such as
the `parent` getter method), you can replace the return type with
a subtype.

For more information, see
[Use sound return types when overriding methods](#use-proper-return-types)
and [Use sound parameter types when overriding methods](#use-proper-param-types).


## Other resources

The following resources have further information on sound Dart:

* [Fixing common type problems](/guides/language/sound-problems) - 
  Errors you may encounter when writing sound Dart code, and how to fix them.
* [Fixing type promotion failures](/tools/non-promotion-reasons) - 
  Understand and learn how to fix type promotion errors.
* [Sound null safety](/null-safety) - 
  Learn about writing code with sound null safety enabled.
* [Customizing static analysis][analysis] - 
  How to set up and customize the analyzer and linter
  using an analysis options file.
* [Dart 2 migration guide](/dart-2) - 
  How to update Dart 1.x code to Dart 2.


[analysis]: /guides/language/analysis-options
[dartdevc]: /tools/dartdevc
[language version]: /guides/language/evolution#language-versioning
[null safety]: /null-safety
