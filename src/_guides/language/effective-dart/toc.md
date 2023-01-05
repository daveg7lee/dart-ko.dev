    {% comment %}
    This file is generated from the other files in this directory.
    To re-generate it, please run the following command from root of
    the project:

      $ dart run deploy/effective-dart-rules/bin/main.dart

    {% endcomment %}
    
<div class='effective_dart--summary_column' markdown='1'>

### 스타일


**식별자**

* <a href='/guides/language/effective-dart/style#do-name-types-using-uppercamelcase'><code>UpperCamelCase</code>를 사용하여 타입을 명명하십시오.</a>
* <a href='/guides/language/effective-dart/style#do-name-extensions-using-uppercamelcase'><code>UpperCamelCase</code>를 사용하여 extensions을 명명하십시오.</a>
* <a href='/guides/language/effective-dart/style#do-name-libraries-and-source-files-using-lowercase_with_underscores'><code>lowercase_with_underscores</code>를 사용하여 라이브러리, 패키지, 디렉토리 그리고 소스 파일을 명명하십시오..</a>
* <a href='/guides/language/effective-dart/style#do-name-import-prefixes-using-lowercase_with_underscores'><code>lowercase_with_underscores</code>를 사용하여 import prefix를 명명하십시오.</a>
* <a href='/guides/language/effective-dart/style#do-name-other-identifiers-using-lowercamelcase'>다른 식별자들은 <code>lowerCamelCase</code>를 사용하여 명명하십시오.</a>
* <a href='/guides/language/effective-dart/style#prefer-using-lowercamelcase-for-constant-names'>상수인 변수는 <code>lowerCamelCase</code>를 사용하여 명명하는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/style#do-capitalize-acronyms-and-abbreviations-longer-than-two-letters-like-words'>두 글자 이상의 두문자어(acronym)와 약어(abbreviation)는 대문자로 명명하십시오.</a>
* <a href='/guides/language/effective-dart/style#prefer-using-_-__-etc-for-unused-callback-parameters'>사용하지 않는 콜백 파라미터을 <code>_</code>, <code>__</code> 와 같이 나타내는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/style#dont-use-a-leading-underscore-for-identifiers-that-arent-private'>Private이 아닌 식별자의 앞에 언더스코어(_)를 사용하지 마십시오.</a>
* <a href='/guides/language/effective-dart/style#dont-use-prefix-letters'>Prefix 문자를 사용하지 마십시오.</a>

**순서**

* <a href='/guides/language/effective-dart/style#do-place-dart-imports-before-other-imports'>"dart:" import를 다른 import 보다 먼저 작성하십시오.</a>
* <a href='/guides/language/effective-dart/style#do-place-package-imports-before-relative-imports'>"package:" import을 relative import 보다 먼저 작성하십시오.</a>
* <a href='/guides/language/effective-dart/style#do-specify-exports-in-a-separate-section-after-all-imports'>Export를 모든 import 이후에 분리된 섹션에서 작성하십시오.</a>
* <a href='/guides/language/effective-dart/style#do-sort-sections-alphabetically'>섹션을 알파벳 순서로 정렬하십시오.</a>

**포매팅**

* <a href='/guides/language/effective-dart/style#do-format-your-code-using-dart-format'><code>Dart format</code>을 사용하여 코드를 포맷하십시오.</a>
* <a href='/guides/language/effective-dart/style#consider-changing-your-code-to-make-it-more-formatter-friendly'>포매터(formatter)에게 친화적인 코드를 작성하도록 고려하십시오.</a>
* <a href='/guides/language/effective-dart/style#avoid-lines-longer-than-80-characters'>80자 보다 긴 라인을 만드는 것을 피하십시오.</a>
* <a href='/guides/language/effective-dart/style#do-use-curly-braces-for-all-flow-control-structures'>모든 흐름 제어 statements에 중괄호를 사용하십시오.</a>

</div>
<div class='effective_dart--summary_column' markdown='1'>


### 문서화


**주석**

* <a href='/guides/language/effective-dart/documentation#do-format-comments-like-sentences'>문장과 같은 형식으로 주석을 작성하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#dont-use-block-comments-for-documentation'>문서화에 블록 주석을 사용하지 마십시오.</a>

**문서 주석**

* <a href='/guides/language/effective-dart/documentation#do-use--doc-comments-to-document-members-and-types'>멤버와 타입에 대한 문서 주석을 작성 할 때 <code>///</code>를 사용하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#prefer-writing-doc-comments-for-public-apis'>공개 API를 위한 문서 주석 작성을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#consider-writing-a-library-level-doc-comment'>라이브러리 수준의 문서 주석 작성을 고려하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#consider-writing-doc-comments-for-private-apis'>비공개 API를 위한 문서 주석 작성을 고려하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#do-start-doc-comments-with-a-single-sentence-summary'>한 줄 요약과 함께 문서 주석을 시작하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#do-separate-the-first-sentence-of-a-doc-comment-into-its-own-paragraph'>문서 주석의 문단에서 첫 번째 줄을 분리시키십시오.</a>
* <a href='/guides/language/effective-dart/documentation#avoid-redundancy-with-the-surrounding-context'>주변 문맥과 중복되는 문장을 피하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#prefer-starting-function-or-method-comments-with-third-person-verbs'>함수 혹은 메서드 주석을 삼인칭 동사로 시작하는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#prefer-starting-a-non-boolean-variable-or-property-comment-with-a-noun-phrase'>Non-boolean 변수나 프로퍼티에 대한 주석은 명사구로 시작하는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#prefer-starting-a-boolean-variable-or-property-comment-with-whether-followed-by-a-noun-or-gerund-phrase'>Boolean 변수나 프로퍼티에 대한 주석은 명사구 또는 동명사구가 따라오는 "Whether"와 함께 사용하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#dont-write-documentation-for-both-the-getter-and-setter-of-a-property'>프로퍼티의 getter와 setter에 대해 모두 문서를 작성하지마십시오.</a>
* <a href='/guides/language/effective-dart/documentation#prefer-starting-library-or-type-comments-with-noun-phrases'>라이브러리나 타입에 대한 주석은 명사구로 시작하는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#consider-including-code-samples-in-doc-comments'>문서 주석에 샘플 코드 추가를 고려하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#do-use-square-brackets-in-doc-comments-to-refer-to-in-scope-identifiers'>In-scope 식별자를 참조하기 위해 대괄호를 사용하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#do-use-prose-to-explain-parameters-return-values-and-exceptions'>예외, 반환 값, 파라미터를 설명 할 때 산문체를 사용하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#do-put-doc-comments-before-metadata-annotations'>메타데이터 어노테이션(metadata annotation) 선언 전에 주석을 넣으십시오.</a>

**마크다운**

* <a href='/guides/language/effective-dart/documentation#avoid-using-markdown-excessively'>과도한 마크다운 사용을 피하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#avoid-using-html-for-formatting'>포매팅(formatting)에 HTML을 사용하는 것을 피하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#prefer-backtick-fences-for-code-blocks'>코드 블럭을 넣을 때 백틱 펜스(backtick fences) 사용을 지향하십시오.</a>

**글쓰기**

* <a href='/guides/language/effective-dart/documentation#prefer-brevity'>간결하게 작성하는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#avoid-abbreviations-and-acronyms-unless-they-are-obvious'>많이 사용되지 않는 약어나 두문자어의 사용을 피하십시오.</a>
* <a href='/guides/language/effective-dart/documentation#prefer-using-this-instead-of-the-to-refer-to-a-members-instance'>멤버의 인스턴스를 지칭할 때, "the" 보다 "this"를 사용하는 것을 지향하십시오.</a>

</div>
<div style='clear:both'></div>
<div class='effective_dart--summary_column' markdown='1'>


### 사용법


**라이브러리**

* <a href='/guides/language/effective-dart/usage#do-use-strings-in-part-of-directives'><code>part of</code> 명령어에 문자열을 사용하십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-import-libraries-that-are-inside-the-src-directory-of-another-package'>다른 패키지의 <code>src</code> 디렉토리에 있는 라이브러리를 import하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-allow-an-import-path-to-reach-into-or-out-of-lib'>Import 경로를 lib 안팎으로 지정하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#prefer-relative-import-paths'>상대 경로를 사용하여 import하는 것을 지향하십시오.</a>

**Null**

* <a href='/guides/language/effective-dart/usage#dont-explicitly-initialize-variables-to-null'>변수를 명시적으로 <code>null</code>로 초기화하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-use-an-explicit-default-value-of-null'>디폴트 값으로 <code>null</code>을 명시적으로 사용하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#prefer-using--to-convert-null-to-a-boolean-value'><code>Null</code>을 boolean 값으로 바꿀 때 <code>??</code> 사용을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/usage#avoid-late-variables-if-you-need-to-check-whether-they-are-initialized'>초기화 여부를 확인해야하는 변수를 <code>late</code>로 선언하는 것을 피하십시오.</a>
* <a href='/guides/language/effective-dart/usage#consider-assigning-a-nullable-field-to-a-local-variable-to-enable-type-promotion'>타입 프로모션을 활성화하고 싶다면, nullable 필드를 로컬 변수에 할당하는 것을 고려하십시오.</a>

**문자열**

* <a href='/guides/language/effective-dart/usage#do-use-adjacent-strings-to-concatenate-string-literals'>문자열 리터럴을 합치고 싶다면, 인접 문자열을 사용하십시오.</a>
* <a href='/guides/language/effective-dart/usage#prefer-using-interpolation-to-compose-strings-and-values'>문자열과 값들을 합성하고 싶다면, interpolation 사용을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/usage#avoid-using-curly-braces-in-interpolation-when-not-needed'>Interpolation에 불필요한 중괄호 사용을 피하십시오.</a>

**컬렉션**

* <a href='/guides/language/effective-dart/usage#do-use-collection-literals-when-possible'>가능하다면 컬렉션 리터럴을 사용하십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-use-length-to-see-if-a-collection-is-empty'>컬렉션이 비었는지 확인하기 위해 <code>.length</code>를 사용하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#avoid-using-iterableforeach-with-a-function-literal'><code>Iterable.forEach()</code>를 함수 리터럴과 함께 사용하는 것을 피하십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-use-listfrom-unless-you-intend-to-change-the-type-of-the-result'>결과 값의 타입을 바꾸려는 것이 아니라면, <code>List.from()</code>을 사용하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#do-use-wheretype-to-filter-a-collection-by-type'>컬렉션을 타입으로 필터하고 싶다면, <code>whereType()</code>을 사용하십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-use-cast-when-a-nearby-operation-will-do'>코드의 주변에서 <code>cast()</code>와 같은 역할을 하는 연산이 있다면, <code>cast()</code>를 사용하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#avoid-using-cast'><code>cast()</code> 사용을 피하십시오.</a>

**함수**

* <a href='/guides/language/effective-dart/usage#do-use-a-function-declaration-to-bind-a-function-to-a-name'>함수의 이름과 함수를 바인드하기 위해 함수 선언식을 사용하세요.</a>
* <a href='/guides/language/effective-dart/usage#dont-create-a-lambda-when-a-tear-off-will-do'>Tear-off가 있을 때 labmda를 생성하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#do-use--to-separate-a-named-parameter-from-its-default-value'>Named 파라미터와 그것의 디폴트 값을 분리시키고 싶다면, <code>=</code> 을 사용하십시오.</a>

**변수**

* <a href='/guides/language/effective-dart/usage#do-follow-a-consistent-rule-for-var-and-final-on-local-variables'>로컬 변수에 <code>var</code>과 <code>final</code>을 사용 할 때 일관된 규칙을 따르십시오.</a>
* <a href='/guides/language/effective-dart/usage#avoid-storing-what-you-can-calculate'>계산 할 수 있는 값들을 저장하는 것을 피하십시오.</a>

**멤버**

* <a href='/guides/language/effective-dart/usage#dont-wrap-a-field-in-a-getter-and-setter-unnecessarily'>필드를 불필요하게 getter나 setter 안에 포함시키지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#prefer-using-a-final-field-to-make-a-read-only-property'>Read-only 프로퍼티를 만들 때, <code>final</code> 필드 사용을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/usage#consider-using--for-simple-members'>간단한 멤버를 선언 할 때, <code>=&gt;</code> 사용을 고려하십시오. </a>
* <a href='/guides/language/effective-dart/usage#dont-use-this-when-not-needed-to-avoid-shadowing'>Named 생성자에 리다이렉트 하거나 섀도잉(shadowing)을 피하기 위함이 아니라면, <code>this.</code>를 사용하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#do-initialize-fields-at-their-declaration-when-possible'>가능하다면 필드의 선언과 함께 초기화를 진행하십시오.</a>

**생성자**

* <a href='/guides/language/effective-dart/usage#do-use-initializing-formals-when-possible'>가능하다면 initializing formal을 사용하십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-use-late-when-a-constructor-initializer-list-will-do'>생성자 initializer list가 변수의 값을 초기화해준다면, <code>late</code>을 사용하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#do-use--instead-of--for-empty-constructor-bodies'>비어있는 생성자 바디에 <code>{}</code> 대신에 <code>;</code> 을 사용하십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-use-new'><code>new</code>를 사용하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-use-const-redundantly'><code>const</code>을 중복되게 사용하지 마십시오.</a>

**에러 핸들링**

* <a href='/guides/language/effective-dart/usage#avoid-catches-without-on-clauses'><code>on</code> 구문 없이 에러를 캐치하는 것을 피하십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-discard-errors-from-catches-without-on-clauses'><code>on</code> 구문 없이 캐치된 에러들을 버리지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#do-throw-objects-that-implement-error-only-for-programmatic-errors'>프로그래밍 오류에 대해서만 <code>Error</code>를 implement한 객체를 throw 하십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-explicitly-catch-error-or-types-that-implement-it'><code>Error</code> 또는 이를 구현하는 타입을 명시적으로 캐치하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#do-use-rethrow-to-rethrow-a-caught-exception'>캐치된 예외를 rethrow 하고 싶다면, <code>rethrow</code>를 사용하십시오.</a>

**비동기**

* <a href='/guides/language/effective-dart/usage#prefer-asyncawait-over-using-raw-futures'>Future을 async/await와 같이 사용하는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/usage#dont-use-async-when-it-has-no-useful-effect'>특별한 효과가 없다면 <code>async</code>를 사용하지 마십시오.</a>
* <a href='/guides/language/effective-dart/usage#consider-using-higher-order-methods-to-transform-a-stream'>Stream을 변형시키고 싶다면 고차함수(higher-order method) 사용을 고려하십시오.</a>
* <a href='/guides/language/effective-dart/usage#avoid-using-completer-directly'>Completer를 직접적으로 사용하는 것을 피하십시오.</a>
* <a href='/guides/language/effective-dart/usage#do-test-for-futuret-when-disambiguating-a-futureort-whose-type-argument-could-be-object'>타입 인자가 <code>Object</code>일 수도 있는 <code>FutureOr&lt;T&gt;</code>를 명확하게 하고, <code>Future&lt;T&gt;</code>를 테스트 하십시오.</a>

</div>
<div class='effective_dart--summary_column' markdown='1'>


### 디자인


**명명법**

* <a href='/guides/language/effective-dart/design#do-use-terms-consistently'>일관적인 용어를 사용하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-abbreviations'>약어 사용을 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#prefer-putting-the-most-descriptive-noun-last'>서술하는 명사를 마지막에 두는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#consider-making-the-code-read-like-a-sentence'>문장 처럼 코드를 읽을 수 있게 작성하는 것을 고려하십시오.</a>
* <a href='/guides/language/effective-dart/design#prefer-a-noun-phrase-for-a-non-boolean-property-or-variable'>Non-boolean 프로퍼티나 변수에 명사구 사용을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#prefer-a-non-imperative-verb-phrase-for-a-boolean-property-or-variable'>Boolean 프로퍼티나 변수에 비명령형 동사구 사용을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#consider-omitting-the-verb-for-a-named-boolean-parameter'>Named boolean <em>파라미터</em>에 동사를 생략하는 것을 고려하십시오.</a>
* <a href='/guides/language/effective-dart/design#prefer-the-positive-name-for-a-boolean-property-or-variable'>Boolean 프로퍼티나 변수에 "긍정적인" 이름의 사용을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#prefer-an-imperative-verb-phrase-for-a-function-or-method-whose-main-purpose-is-a-side-effect'>주된 목적이 사이드 이펙트인 함수 또는 메서드에는 명령형 동사구 사용을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#prefer-a-noun-phrase-or-non-imperative-verb-phrase-for-a-function-or-method-if-returning-a-value-is-its-primary-purpose'>함수 또는 메서드의 주된 목적이 값을 반환하는 것이라면, 비명령형 동사구 사용을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#consider-an-imperative-verb-phrase-for-a-function-or-method-if-you-want-to-draw-attention-to-the-work-it-performs'>함수나 메서드가 수행하는 작업에 집중하고 싶다면, 명령형 동사구 사용을 고려하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-starting-a-method-name-with-get'>메서드의 이름을 <code>get</code>로 시작하는 것을 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#prefer-naming-a-method-to___-if-it-copies-the-objects-state-to-a-new-object'>객체의 상태를 새로운 객체로 복사하는 메서드는 <code>to___()</code>로 명명하는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#prefer-naming-a-method-as___-if-it-returns-a-different-representation-backed-by-the-original-object'>원본 객체가 지원하는 다른 표현을 반환하는 메서드의 경우 <code>as___()</code>로 명명하는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-describing-the-parameters-in-the-functions-or-methods-name'>함수나 메서드의 이름에 파라미터를 묘사하는 것을 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-follow-existing-mnemonic-conventions-when-naming-type-parameters'>타입 파라미터를 명명할 때, 기존에 존재하는 니모닉 컨벤션(mnemonic conventions)을 따르십시오.</a>

**라이브러리**

* <a href='/guides/language/effective-dart/design#prefer-making-declarations-private'>Private로 선언하는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#consider-declaring-multiple-classes-in-the-same-library'>동일한 라이브러리에 다수의 클래스를 선언을 고려하십시오.</a>

**클래스와 mixins**

* <a href='/guides/language/effective-dart/design#avoid-defining-a-one-member-abstract-class-when-a-simple-function-will-do'>간단한 함수가 대신할 수 있는, 단 하나의 멤버를 가지는 추상 클래스를 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-defining-a-class-that-contains-only-static-members'>오직 static 멤버들로만 이루어진 클래스를 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-extending-a-class-that-isnt-intended-to-be-subclassed'>서브 클래스로 의도되지 않은 클래스를 확장하는 것을 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-document-if-your-class-supports-being-extended'>클래스가 확장되는 것을 지원하는 경우 문서를 작성하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-implementing-a-class-that-isnt-intended-to-be-an-interface'>인터페이스로 의도되지 않은 클래스를 implement하는 것을 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-document-if-your-class-supports-being-used-as-an-interface'>클래스가 인터페이스로 사용되는 것을 지원하는 경우 문서를 작성하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-use-mixin-to-define-a-mixin-type'>Minxin 타입을 정의하고 싶다면, <code>mixin</code>을 사용하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-mixing-in-a-class-that-isnt-intended-to-be-a-mixin'>Mixin으로 의도되지 않은 타입을 믹싱(mixing)하는 것을 피하십시오.</a>

**생성자**

* <a href='/guides/language/effective-dart/design#consider-making-your-constructor-const-if-the-class-supports-it'>가능하다면 클래스의 생성자를 <code>const</code>로 작성하는 것을 고려하십시오.</a>

**멤버**

* <a href='/guides/language/effective-dart/design#prefer-making-fields-and-top-level-variables-final'>필드와 최상위 변수들을 <code>final</code>로 선언하는 것을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-use-getters-for-operations-that-conceptually-access-properties'>개념적으로 프로퍼티에 접근하는 연산자의 경우 getter를 사용하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-use-setters-for-operations-that-conceptually-change-properties'>개념적으로 프토퍼티를 변경하는 연산자의 경우 setter를 사용하십시오.</a>
* <a href='/guides/language/effective-dart/design#dont-define-a-setter-without-a-corresponding-getter'>대응되는 getter 없이 setter를 정의하지 마십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-using-runtime-type-tests-to-fake-overloading'>가짜 오버로딩에 런타임 타입 테스트를 사용하는 것을 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-public-late-final-fields-without-initializers'>Initializers가 없는 public <code>late final</code> 필드 정의를 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-returning-nullable-future-stream-and-collection-types'>Nullable <code>Future</code>, <code>Stream</code>, 그리고 컬렉션 타입의 반환을 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-returning-this-from-methods-just-to-enable-a-fluent-interface'>원활한 인터페이스의 활성화를 위해 메서드에서 <code>this</code>의 반환을 피하십시오.</a>

**타입**

* <a href='/guides/language/effective-dart/design#do-type-annotate-variables-without-initializers'>Initializer가 초기화하지 않는 변수들은 타입 어노테이트하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-type-annotate-fields-and-top-level-variables-if-the-type-isnt-obvious'>필드와 최상위 변수들의 타입이 애매하다면, 타입  어노테이트하십시오.</a>
* <a href='/guides/language/effective-dart/design#dont-redundantly-type-annotate-initialized-local-variables'>초기화된 지역 변수에 대해 중복된 타입 어노테이트를 하지 마십시오.</a>
* <a href='/guides/language/effective-dart/design#do-annotate-return-types-on-function-declarations'>함수의 선언에서 반환 값의 타입을 어노테이트 하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-annotate-parameter-types-on-function-declarations'>함수의 선언에서 파라미터의 타입을 어노테이트 하십시오.</a>
* <a href='/guides/language/effective-dart/design#dont-annotate-inferred-parameter-types-on-function-expressions'>함수 표현식에서 추론된 파라미터 타입을 어노테이트 하지 마십시오.</a>
* <a href='/guides/language/effective-dart/design#dont-type-annotate-initializing-formals'>Initializing formal을 타입 어노테이트 하지 마십시오.</a>
* <a href='/guides/language/effective-dart/design#do-write-type-arguments-on-generic-invocations-that-arent-inferred'>타입이 추론되지 않은 generic invocation에 대해 타입 인자를 작성하십시오.</a>
* <a href='/guides/language/effective-dart/design#dont-write-type-arguments-on-generic-invocations-that-are-inferred'>타입이 추론된 generic invocation에 대해 타입 인자를 작성 하지 마십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-writing-incomplete-generic-types'>불완전한 generic 타입의 작성을 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-annotate-with-dynamic-instead-of-letting-inference-fail'>타입 추론이 실패하도록 두기 보다 <code>dynamic</code>으로 어노테이트 하십시오.</a>
* <a href='/guides/language/effective-dart/design#prefer-signatures-in-function-type-annotations'>함수 타입 어노테이션에서 시그니쳐 사용을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#dont-specify-a-return-type-for-a-setter'>Setter의 반환 타입을 특정하지 마십시오.</a>
* <a href='/guides/language/effective-dart/design#dont-use-the-legacy-typedef-syntax'>오래된 typedef의 문법을 사용하지 마십시오.</a>
* <a href='/guides/language/effective-dart/design#prefer-inline-function-types-over-typedefs'>typedef 보다 인라인 함수 타입의 사용을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#prefer-using-function-type-syntax-for-parameters'>파라미터에 함수 타입 문법의 사용을 지향하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-using-dynamic-unless-you-want-to-disable-static-checking'>정적인 타입 체킹을 비활성화하고 싶은 것이 아니라면, <code>dynamic</code>의 사용을 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-use-futurevoid-as-the-return-type-of-asynchronous-members-that-do-not-produce-values'>값을 생성하지 않는 비동기 멤버의 반환 값으로 <code>Future&lt;void&gt;</code>를 사용하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-using-futureort-as-a-return-type'><code>FutureOr&lt;T&gt;</code>를 반환 타입으로 사용하는 것을 피하십시오.</a>

**파라미터**

* <a href='/guides/language/effective-dart/design#avoid-positional-boolean-parameters'>Positional boolean 파라미터의 사용을 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-optional-positional-parameters-if-the-user-may-want-to-omit-earlier-parameters'>이전의 파라미터들을 생략하고 싶다면, optional positional 파라미터의 사용을 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-mandatory-parameters-that-accept-a-special-no-argument-value'>"no argument" 값을 전달받는 필수 파라미터를 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-use-inclusive-start-and-exclusive-end-parameters-to-accept-a-range'>파라미터가 범위를 전달받을 때, 첫 값을 포함하며 시작하고 마지막 값은 포함하지 마십시오.</a>

**동등성**

* <a href='/guides/language/effective-dart/design#do-override-hashcode-if-you-override-'><code>==</code>를 오버라이드 할 때, <code>hashCode</code>를 오버라이드 하십시오.</a>
* <a href='/guides/language/effective-dart/design#do-make-your--operator-obey-the-mathematical-rules-of-equality'><code>==</code> 연산자가 동등성의 수학적 규칙을 지키도록 하십시오.</a>
* <a href='/guides/language/effective-dart/design#avoid-defining-custom-equality-for-mutable-classes'>변할 수 있는 클래스에서 custom equality의 정의를 피하십시오.</a>
* <a href='/guides/language/effective-dart/design#dont-make-the-parameter-to--nullable'><code>==</code>의 파라미터를 nullable로 설정 하지 마십시오.</a>

</div>
<div style='clear:both'></div>
