# Python Package Manager

pip는 파이썬으로 작성된 패키지 소프트웨어를 설치 · 관리하는 패키지 관리 시스템이다. Python Package Index (PyPI)에서 많은 파이썬 패키지를 볼 수 있다. 파이썬 2.7.9 이후 버전과 파이썬 3.4 이후 버전은 pip를 기본적으로 포함한다.

## Python Package Manager

### pip 업그레이드

pip를 업그레이드하려면 다음과 같이 한다.

```shell
pip install --upgrade pip 
```

### 라이브러리 설치

install 옵션을 사용하여 패키지를 설치한다.

```shell
pip install package_name
```

pip은, pypi(홈페이지)라는 Python Package Index 홈페이지에 등록된 라이브러리들 중, 유저가 요청한 라이브러리의 최신 버전을 다운받게 된다. default가 최신버전이라는 거고, 아래와 같이 원하는 버전을 지정하여 다운 받을 수도 있다. pandas의 최신버전은 현재 0.21.0 인데, 만약 그 전 버전인 0.19.1을 원한다면:

```shell
$ pip install pandas==0.19.1
```

### pypi에 등록되지 않는 라이브러리 설치

문제는 필요한 라이브러리가 마이너 하거나, 너무 최근에 나와 pypi에 등록되지 않은 경우 이다.

```shell
$ install datamasters
Collecting datamasters
  Could not find a version that satisfies the requirement datamasters (from versions: )
No matching distribution found for datamasters
```

이런 라이브러리들은 보통 .whl 파일로서 바로 다운받을 수 있게 되어있는데1, 보통 다음과 같은 네이밍을 가지고 있다

```
pandas-0.21.0-cp35-cp35m-manylinux1_x86_64.whl
pandas-0.21.0-cp35-cp35m-win32.whl
pandas-0.21.0-cp35-cp35m-win_amd64.whl
[라이브러리명]-[버전]-[파이썬 버전]-[OS].whl
```

즉, whl파일을 받기전에 자신에 맞는 파이썬 버전과 OS를 체크하고 받아야 원활한 설치가 가능하다.

pip를 활용하되, whl 파일을 다운로드 받아서 직접 설치하는 방법을 알아보자. 필요한 라이브러리를 버전에 맞게 다운로드하여 윈도우의 커맨드창에서 다음과 같이 입력하여 설치한다.

```shell
pip install 파일명.whl
```

이 때 주의할 점은, 파일이 설치된 디렉토리에 들어가서 파일명만 입력하거나, 절대 경로를 모두 적어서 설치해야 한다.

### 라이브러리 업그레이드

```shell
pip install 패키지명 --upgrade
```

### 라이브러리 제거

uninstall 옵션을 사용하여 라이브러리를 제거한다.

```shell
pip uninstall 패키지명
```

### 설치된 라이브러리 목록 확인

list 옵션을 사용하여 설치된 라이브러리들을 확인한다.

```shell
pip list
```

### 파이썬 표준 라이브러리

[파이썬 표준 라이브러리](https://docs.python.org/ko/3/library/index.html)
