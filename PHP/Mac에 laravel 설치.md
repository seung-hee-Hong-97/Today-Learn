# Mac에 laravel 설치

2022년 6월 기준 라라벨의 가장 최신 버전은 9.X 입니다.

따라서 Mac에 laravel 9를 설치하는 매뉴얼을 작성하겠습니다.

[라라벨 공식 문서](https://laravel.kr/docs/9.x/valet)

## 1. Homebrew 설치

우선 macOS 용 패키지 관리자인 `Homebrew` 를 설치해야 합니다. 만일 이미 Homebrew가 설치되어 있다면 이 단계는 건너뛰면 됩니다.

[Homebrew 설치하기](https://brew.sh/index_ko) 를 클릭하여 Homebrew 사이트에 접속한 뒤 아래의 명령어를 복사합니다.

``` bash
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

복사한 명령어를 터미널에 입력하면, 맥에 로그인하는 비밀번호를 입력하라는 문구가 나옵니다.

본인의 비밀번호를 입력한 후, `Return/Enter` 키를 눌러줍니다.

위의 과정을 진행했다면 터미널에 Homebrew를 설치하는 과정이 쭉 나타날 것입니다. 설치가 완료되었다면, 아래의 명령어를 입력하여 정상적으로 설치가 되었는지 확인합니다.

``` bash
$ brew --version
```

> 만약 버전이 나타나지 않고, `WARNING: /opt/homebrew/bin is not in your PATH` 같은 문구가 뜨면서 에러가 발생할 경우 아래의 과정을 추가적으로 진행 해줍니다.

### Homebrew 설치가 정상적으로 안 됐을 경우

터미널의 shell은 `zsh` 입니다.

아래의 명령어를 입력하여 `zshrc` 에 들어갑니다.

``` bash
$ vi ~/.zshrc
```

각각 본인의 터미널의 shell에 맞게 명령어를 입력하여 해당 파일에 들어간 후 아래의 명령어를 입력하여 환경변수를 추가해줍니다.

``` bash
# (1) i를 눌러 입력모드로 전환 후, 아래의 경로를 입력
export PATH=/opt/homebrew/bin:$PATH

# (2) ESC를 눌러 입력모드 해제 후, :wq를 입력하여 저장 후 나옵니다.

# (3)  아래의 명령어를 입력하여 환경변수를 반영 해줍니다.
$ source ~/.zshrc

# (4) Homebrew가 설치되었는지 확인
$ brew --version
```

> 위의 과정을 추가적으로 진행하면 정상적으로 Homebrew의 버전이 나타납니다.



## 2. php 설치

현재 단계도 (1)단계와 마찬가지로 이미 설치가 완료되었다면 건너뛰면 됩니다.

우선 php를 설치하기 전에 Homebrew 업데이트를 진행합니다. 이 매뉴얼을 보고 처음부터 모든 것을 설치하게 되면 이미 최신 버전이겠지만, 이 전에 설치했다면 예전 버전일 확률이 높습니다.

따라서 아래의 명령어를 입력하여 업데이트를 진행합니다.

``` bash
$ brew update

# 위의 명령어를 입력하고 결과가 Already up-to-date 라고 나오면 이미 최신버전인 것 입니다.
```

업데이트를 완료했다면, 아래의 명령어를 입력하여 php를 설치합니다.

``` bash
$ brew install php
```

설치가 완료되었다면, 아래의 명령어를 입력하여 php를 재시작해줍니다.

``` bash
$ brew services restart php
```

위의 과정을 정상적으로 진행했다면 아래의 명령어를 입력하여, php가 잘 설치되었는지 확인합니다.

``` bash
$ php --version
```



## 3. Composer 설치

Composer는 php의 의존성 도구입니다. 

어떤 서비스를 정상적으로 실행시키기 위해 꼭 필요한 필수 라이브러리를 지정하여 리스트화 시켜놓으면 Composer가 해당 라이브러리를 저장소에서 자동으로 찾고 가져와 설치해주는 도구입니다.

비슷한 예시로는 Java의 Maven, Gradle 혹은 Node.js의 npm 등이 있습니다.

Homebrew를 이용하여 Composer를 전역으로 설치할 것입니다. 아래의 명령어를 입력하여 설치를 진행합니다.

``` bash
$ brew install composer
```

> 시스템에서 laravel 실행파일을 찾을 수 있도록 Composer의 시스템 전체 `vendor bin` 디렉토리를 `$PATH` 에 배치해야 합니다.

아래의 명령어를 입력하여 환경변수를 설정해줍니다.

```bash
# (1) zshrc 파일을 엽니다.
$ vi ~/.zshrc

# (2) i를 눌러 입력모드로 전환 후, 아래의 환경 변수를 추가합니다.
export PATH="$PATH:$HOME/.composer/vendor/bin"

# (3) ESC를 누른 후, :wq를 입력하여 저장 후 나옵니다.

# (4) 추가한 환경 변수를 반영해줍니다.
$ source ~/.zshrc

# (5) 잘 적용되었는지 확인합니다.
$ echo $PATH
```



## 4. Laravel 및 valet 설치

### 라라벨 설치

``` bash
$ composer global require laravel/installer
```

> `라라벨(Laravel)`은 오픈소스 PHP 웹 프레임워크 중 하나로 MVC(모델-뷰-컨트롤러) 아키텍처 패턴을 따라 웹 애플리케이션을 개발하기 위한 프레임워크 입니다.

### Valet 설치

``` bash
$ composer global require laravel/valet
```

> `Valet` 은 Mac에서 사용할 수 있는 라라벨 개발 환경입니다. 별 다른 추가 설정이나 프로그램이 필요 없습니다.
>
> 작은 메모리를 이용해 가볍고 유연하고 빠르게 개발을 할 수 있습니다.ㄴ

위의 과정을 진행했다면 `Laravel` 과 `Valet `이 정상적으로 설치되었습니다.



## 5. Valet 및 라라벨 프로젝트 실행

이제 Valet을 통한 라라벨을 실행하기 위한 모든 조건을 갖추었습니다.

### Valet 실행

``` bash
$ valet install
```

> 이 명령어를 실행하면 nginx가 같이 설치 됩니다.

### 라라벨 프로젝트 설치

라라벨 프로젝트는 `new` 명령어를 통해 설치할 수 있습니다.

``` bash
# laravel new '설치할 프로젝트명'
$ laravel new blog
```

만약 특정버전을 설치하고 싶다면 뒤에 버전명을 작성해주면 됩니다.

``` bash
$ composer create-project --prefer-dist laravel/laravel blog "5.4.*"
```

### Valet 명령어

**park**

``` bash
$ valet park
```

> park 명령어는 애플리케이션이 포함된 디렉토리를 머신에 등록합니다.
>
> 디렉토리가 발렛으로 파킹되면 해당 디렉토리 내의 모든 디렉토리는 웹 브라우저의 `http://<디렉토리명>.test` 에서 실행할 수 있습니다.
>
> Ex) http://blog.test

### 라라벨 프로젝트 실행

위에서 라라벨 프로젝트를 설치할 때 `laravel new blog` 라고 작성했기 때문에 `blog.test` 를 입력하면 해당 프로젝트가 호출됩니다.

<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/first-laravel-project.png?raw=true">