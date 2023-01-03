[Homebrew를 설치](https://brew.sh)
하고 다음 명령을 실행하세요:

```terminal
$ brew tap dart-lang/dart
$ brew install dart
```

{{site.alert.important}}
  **Homebrew의 `bin` 디렉토리가 `PATH`에 포함되어있는지** 확인하세요.
  경로를 올바르게 설정하면 `dart run` 및 `dart format`과 같은 Dart SDK 명령이
  제대로 작동합니다. PATH를 설정하는 데 도움이 필요하다며느
  [Homebrew FAQ](https://docs.brew.sh/FAQ)를 참고하세요.
{{site.alert.end}}

사용 가능한 릴리즈로 업데이트하려면 다음 명령을 실행하세요:

```terminal
$ brew upgrade dart
```

로컬 Dart SDK 버전을 변경하고 싶다면, 변경하길 원하는 버전을 아직
설치하지 않았다면 먼저 설치하세요. 예를 들어, Dart 2.12를
설치하고 싶다면:

```terminal
$ brew install dart@2.12
```

그런 다음 버전을 변경하고 싶다면,
현재 버전을 언링크하고 원하는 버전을 링크하세요.

```terminal
$ brew unlink dart@<old> && brew unlink dart@<new> && brew link dart@<new>
```

설치된 Dart 버전을 확인하고 싶다면 다음 명령을 실행하세요:

```terminal
$ brew info dart
```
