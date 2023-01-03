AMD64(64-bit Intel)에서 Debian/Ubuntu 시스템을 사용하는 경우 다음 중 하나를 선택하여
설치하면 새 버전이 출시되었을 때 자동으로 업데이트됩니다.

* [apt-get으로 설치하기](#install-using-apt-get)
* [Debian 패키지로 설치하기](#install-a-debian-package)

#### apt-get으로 설치하기

다음과 같이 **한 번만 설치**하면 됩니다:

```terminal
$ sudo apt-get update
$ sudo apt-get install apt-transport-https
$ wget -qO- https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /usr/share/keyrings/dart.gpg
$ echo 'deb [signed-by=/usr/share/keyrings/dart.gpg arch=amd64] https://storage.googleapis.com/download.dartlang.org/linux/debian stable main' | sudo tee /etc/apt/sources.list.d/dart_stable.list
```

그런 다음 Dart SDK를 설치합니다:

```terminal
$ sudo apt-get update
$ sudo apt-get install dart
```

#### Debian 패키지로 설치하기

[Debian 패키지](#){:.debian-link-stable}를 사용해 `.deb` 패키지 포맷으로 Dart SDK를 다운로드합니다. 

#### PATH 환경 변수에 모든 Dart 바이너리에 대한 액세스를 추가하세요

SDK를 설치한 후 환경 변수의 **`PATH`에 SDK의 `bin` 디렉토리를 추가해야합니다**. 예를 들어,
활성화된 터미널 세션에 다음 명령을 사용하여 `PATH` 환경 변수를 변경하세요:

```terminal
$ export PATH="$PATH:/usr/lib/dart/bin"
```

다음 명령을 사용하여 PATH 환경 변수를 영구히 설정할 수 있습니다:

```terminal
$ echo 'export PATH="$PATH:/usr/lib/dart/bin"' >> ~/.profile
```
