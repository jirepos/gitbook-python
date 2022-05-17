# pip를 이용하여 github에서 패키지 설치하기

github에서 소스를 직접 컴파일하여 설치를 해보자. numpy를 설치한다. 일단 numpy의 github 사이트로 가자. url은 https://github.com/numpy/numpy이다. git url을 복사한다.

```
https://github.com/numpy/numpy.git
```

pip install을 이용하여 다음과 같이 설치한다.

```shell
pip install git+https://github.com/numpy/numpy.git
```

그런데 설치 중에 다음과 같은 오류가 발생한다.

```shell
  creating build\src.win-amd64-3.9\numpy\distutils
  building library "npymath" sources
  error: Microsoft Visual C++ 14.0 is required. Get it with "Build Tools for Visual Studio": https://visualstudio.microsoft.com/downloads/
  ----------------------------------------
  ERROR: Failed building wheel for numpy
```

Visual C++14.0이 필요한데 없어서 나는 오류이다.

Visual C++14를 설치하자.

다음의 URL로 간다. https://visualstudio.microsoft.com/ko/vs/older-downloads/

![](../../.gitbook/assets/python/devops/pip01.png)

Microsoft Build Tools 2015 업데이트 3를 다운로드 받아 설치한다.

그런데 설치를 실패했다.

그래서 Visual Studio 2019 Community 버전을 설치해서 오류를 해결했다.

다시 빌드....

그러나 다시 다음과 같은 오류가 발생했다.

```
Failed to build numpy
ERROR: Could not build wheels for numpy which use PEP 517 and cannot be installed directly
```

pip를 업그레이드 하라고 해서 pip를 업그레이드 했다.

```shell
python -m pip install --upgrade pip
```

그러나 오류가 발생하여 더 이상 진행하지 못하고 있다.
