# Python 프로젝트 구조

## 파이썬이 모듈(파일)을 찾는 방법

import 문을 통해 다른 파이썬 파일을 불러 올 때

* sys.path
* PYTHONPATH

에 있는 경로를 탐색한다.

### sys.path에 append()로 경로 추가

sys.path는 디렉토리 경로들이 기록된 문자열 리스트이다. 이 리스트에 경로를 추가하면 파이썬 파일을 import 문으로 불러 올수 있다.

/home 디렉토리에 common.py 파일을 생성했다고 한다면 sys.path.append()를 사용하여 경로를 추가할 수 있다.

```python
import sys
sys.path.append("/home")
import common
```

### sys.path의 기본값

/opt 폴더에 test.py 파일을 생성하고 다음과 같이 작성한다

```python
# test.py 
import sys
sys.path.append("/home")
import common
print(sys.path)
```

이 파일을 실행한다.

```shell
python test.py
```

그러면 sys.path에는 가장 먼저 test.py 파일이 속한 디렉토리의 절대 경로가 추가된다. sys.path에 다음의 순서대로 모듈을 찾는다.

* 파이썬 모듈(파일)이 실행되고 있는 현재 디렉토리
* PYAHTONPATH 환경변수에 정의되어 있는 디렉토리
* 파이썬과 함께 설치된 기본 라이브러리

import는 sys.path 리스트에 들어있는 경로들을 탐색하며 불러올 파이썬 파일을 찾는다. 리스트에 들어있는 맨 처음 경로부터 탐색을 시작한다. 특정 경로에서 불러올 파일을 찾았다면 남은 경로를 더 찾아보지 않고 탐색을 중지한다.

* 파이썬은 순서대로 찾고 없으면 ModuleNotFoundError를 반환한다.
* import는 현재 디렉터리에 있는 파일이나 파이썬 라이브러리가 저장된 디렉터리에 있는 모듈만 불러올 수 있다. 파이썬 라이브러리는 파이썬을 설치할 때 자동으로 설치되는 파이썬 모듈을 말한다.

## Python 프로젝트 구조

```
project
  └─docs/  # 문서 저장
  └─rsc/   # 사전이나 메타 데이터 저장
  └─scripts #  스크립트 저장
  └─src/   # 소스 디렉토리
    └─  package_name/
        __init__.py
    main.py  
  └─tests  # 단위 테스트용 코드
```
