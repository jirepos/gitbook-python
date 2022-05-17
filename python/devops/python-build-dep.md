# Python 빌드 및 배포

> 주의: Anaconda를 사용하는 경우에는 pip를 사용하지 말고 conda 명령어를 사용해서 라이브러리를 설치해야 한다.

패키지를 **init**.py를 통해서 식별하는 것과 같이 파이썬은 setup.py을 통해 프로젝트의 최상위 디렉토리를 결정한다. 즉. 프로젝트 폴더를 만들면 최상위 폴더에 setup.py 파일을 둔다.

```
project_root/
  setup.py
```

Python 프로젝트를 빌드하고 배포하려면 setup.py 파일을 작성하고 이 파일을 사용하여 빌드하고 배포한다.

## setup.py 작성

프로젝트 루트에 setup.py 파일 생성한다. setuptools를 사용해 setup 설정을 하기 때문에, 아직 setuptools가 인스톨 되어 있지 않다면 인스톨 하도록 한다

```shell
pip install setuptools
```

### setuptools 임포트

```python
import io
from setuptools import find_packages, setup
```

### setup() 함수 작성

setup() 함수에서 사용하는 중요한 파라미터들을 살펴보자.

**name** 프로젝트 이름\
**version** 배포버전\
**description** 프로젝트 설명\
**author** 저자\
**author_email** 저자의 이메일\
**license** 라이선스\
**packages** 빌드에 포함될 패키지들을 나열한다. 빌드에 포함들 package들. 한 프로젝트에 여러 package가 있을수도 있고 또한 빌드에서 제외할 package들 (예를 들어 test 코드나 doc 등등)이 있을수 있음으로 그러한 설정을 한다. 대부분 setuptools.find_packages 함수를 사용하여 자동으로 포함될 package들을 찾게 하고 대신에 제외되어야 할 package들을 exclude 인자를 통해 설정해준다.

다음과 같이 자동으로 찾게 한다.

```python
packages = find_packages(),
#packages = find_packages(where='src', exclude = ['docs', 'tests*']),
#packages = find_packages(exclude = ['docs', 'tests*']),
```

**package_dir**\
프로젝트 루트 아래에 패키지 디렉토리가 없고 src와 같은 특정 디렉토리 아래에 패키지를 두는 경우에 이 옵션을 사용한다. 다른 규칙을 사용하여 소스 디렉터리를 배치하는 것도 문제가 되지 않는다.

**install_requires**\
가장 중요한 부분은 이 프로젝트가 사용하는 패키지들을 지정하는 것인데, install_requires 옵션을 사용합니다. 전부 6개의 패키지를 지정했는데, 모두 django==1.6b4 와 같이 구체적인 버전을 지정했습니다. 처음부터 버전을 지정하지는 않고, 어느 정도 호환성 확인이 된 후에 버전을 확인하여 고정하게 됩니다. 버전 확인에는 pip freeze 명령을 사용하면 됩니다. 패키지 이름의 대소문자는 중요하지 않습니다.

**keywords** 해당 프로젝트 관련 키워드\
**python_requires** 지원하는 파이썬 버전 설명

**package_data**\
파일을 포함시키기 위해선 포함시키고자 하는 파일들을 이 옵션에 명시해 줘야 포함된다. 그렇지 않으면 포함이 되지 않는다. 이 옵션 설정을 해주지 않거나 잘못 설정해주어서 문제가 생기는 경우가 꽤 있으니 주의하자.

기본적으로 setup은 파이썬 파일만 빌드에 포함시킨다.파일만 빌드에 포함시킨다. 파이썬 파일 이 아닌 외부 파일을 포함시키기 위해선 포함시키고자 하는 파일들을 이 옵션에 명시해 줘야 포함된다

프로젝트 구조가 다음과 같다고 가정하자.

```
project_root/
  andytestutil/
    __init__.py
    data/ 
       domains.txt
```

domains.txt를 배포에 포함시키려면 package_data를 다음과 같이 설정한다.

```python
    package_data={"andyutil":[
      'data/domains.txt'
    ]},
    zip_safe=False,
```

**zip_safe**\
위의 package_data 설정을 하였으면 zip_safe 설정도 해주어야 하며 False로 설정해주어야 한다.

**classifiers**\
PYPI에 등록될 메타 데이터 설정이다. 예를 들어 서포트 하는 python 버젼 정보를 명시할수 있다. 하지만 이건 PYPI에 등록될 메타 데이터일 뿐이고 실제 빌드에는 영향을 주지 않는다. 이점 주의하자.

```python
// setup.py
from setuptools import setup, find_packages

import io
from setuptools import find_packages, setup

setup(
    name             = 'andytestutil',
    version          = '1.0',
    description      = 'For testing python app deploy',
    author           = 'behappytwice',
    author_email     = '4youwith@gmail.com',
    url              = 'https://gitlab.com/latteonterrace/python_start.git',
    download_url     = 'https://gitlab.com/latteonterrace/python_start.git',
    install_requires = [ ],
    packages= find_packages(),
    keywords         = ['test', 'lesson'],
    python_requires  = '>=3',
    package_data={"andyutil":[
      'data/domains.txt'
    ]},
    zip_safe=False,
    classifiers      = [
        'Programming Language :: Python :: 3',
        'Programming Language :: Python :: 3.2',
        'Programming Language :: Python :: 3.3',
        'Programming Language :: Python :: 3.4',
        'Programming Language :: Python :: 3.5',
        'Programming Language :: Python :: 3.6'
    ]
)
```

#### 버전 명시 방법

package == 0.5.1 : 정확한 버전\
package >= 0.5.1 : 0.5.1과 같거나 높은 버전\
package == 0.5.\* : 0.5로 시작되는 버전\
package \~= 0.5.0 : 0.5.0 버전과 호환성이 있는 버전

\~=: Compatible release clause\
\==: Version matching clause\
!=: Version exclusion clause\
<=, >=: Inclusive ordered comparison clause\
<, >: Exclusive ordered comparison clause\
\===: Arbitrary equality clause.

## README.md 파일 배포

만일 README 파일이 마크다운 형태로 되어 있다면 setup.cfg 파일을 프로젝트 루트에 아래와 같이 설정 해준다.

```
[metadata]
description-file = README.md
```

README.md 파일도 프로젝트 루트에 생성한다.

## MANIFEST.in

이 파일에는 내부 패키지 디렉토리에 들어 있지는 않지만 포함시키고 싶은 파일을 담는다. 일반적으로 이런 파일은 README 파일이나 라이선스 파일 및 이와 비슷한 파일들이 있다. 중요한 파일은 requirements.txt이다. 이 파일은 pip가 다른 필수 패키지를 설치하는데 사용된다.

MANIFEST.in 파일의 예

```
include LICENSE
include README.md
include requrements.txt 
```

### 의존성

의존성은 setup.py의 install_requires 섹션과 requirements.txt 파일에 모두 지정할 수 있다. pip는 install_requirements의 의존성은 자동으로 설치하지만 requirements.txt 파일의 의존성은 자동으로 설치하지 않는다. 그러한 요구사항을 설치하려면 다음과 같이 명시적으로 실행해야 한다.

```shell
pip install -r requirements.txt
```

## 빌드하기

빌드 하는 방법은 몇가지가 있지만 공식적으로는 wheel을 사용하여 빌드하는것이 권장된다. wheel은 build package인데 일반적인 source distribution보다 더 빨리 인스톨 되기에 공식적으로 권장된다. 아래 커맨드를 실행하여 wheel을 인스톨 하고 빌드를 하도록 하자.

```shell
pip install wheel
python setup.py bdist_wheel
```

twine 가이드 문서에서는 sdist 옵션을 사용하는 것으로 설명이 되어 있다. sdist 옵션을 사용하면 소스를 tar.gz으로 압축하여 dist 폴더에 생성한다.

```shell
python setup.py sdist bdist_wheel
```

## 배포하기

이제 배포만 하면 된다. 배포는 twine을 사용해서 배포하도록 한다. Twine은 PyPi에 Python 패키지를 배포하기 위한 유틸리티이다. python setup.py upload 커맨드를 사용하지 않고 twine을 사용하는 이유는 upload 커맨드는 일반 HTTP를 사용해서 배포하기 때문에 아이디나 패스워드가 노출될수 있는데 twine은 TLS를 사용하기 때문이다.

twine이 설치되어 있지 않으면 설치한다.

```shell
 pip install twine
```

그리고 PYPI에 이미 계정이 있어야 하는데, 만일 PYPI 계정이 없다면 twine이 그 또한 가이드 해준다. 아래 커맨드를 사용하여 PYPI에 배포하자.

```shell
twine upload dist/*
```

이제 pip install 을 실행하여 팩케지를 어디서든 누구나 인스톨 할수 있다. 주의 할점은 인스톨은 곧바로 되어 검색은 PYPI가 인덱스 하는데 시간이 걸린다. 즉 pip search 명령어를 치면 곧바로 검색이 안될수 있다. 배포한 package 관리는 PYPI 사이트에 로그인해서 해당 package의 페이지로 가면 된다.

> pip search는 지원되지 않는다. 2021.05.06

업로드가 완료되면 pypi 싸이트에서 내가 올린 프로젝트를 볼 수 있다.

## 배포된 라이브러리 사용하기

pip를 이용하여 library를 로컬에 설치한다.

```shell
pip install andystring
```

라이브러리를 import하여 사용한다.

```python

from andystring.StringUtil import appendString
from andyutil.util import * 

print(appendString('aaa', 'bbb'))
print(add(1,2))
```

## src 폴더에 패키지 생성하기

파이썬 프로젝트에서 패키지들은 프로젝트 루트 디렉토리 바로 아래에 둔다. 아무 패키지에도 속하지 않는 모듈은 프로젝트 루트에 두고 다음과 같이 foo 패키지는 프로젝트 루트 디렉토리 바로 아래에 있게 된다.

```
project_root/
   setup.py
   foo/
     __init__.py
     string_util.py
```

그러나 모든 파이썬 소스들을 src와 같은 특정 디렉토리 아래에 두고 싶은 경우가 있을 수 있다. 다음과 같이 프로젝트 구조를 생성할 수 있다.

```
project_root/
   setup.py
   src/
      foo/
        __init__.py
        string_util.py
```

위와 같은 구조의 프로젝트를 빌드하고 배포하려면 packages와 package_dir 옵션을 사용해야 한다.

**package_dir**\
위와 같이 모든 파이썬 소스들을 src 디렉토리 아래에 두고 빌드할 패키지를 찾을 때 src 디렉토리에서 찾게 하려면 다음과 같이 설정한다. 루트 패키지는 src 디렉토리이다. 루트 패키지(어떤 패키지에도 속하지 않은 것)은 src에 있고, foo 패키지의 모듈은 src/foo에 있다.

```python
 package_dir={'': 'src'},
```

이 딕셔너리의 키는 패키지 이름이고, 빈 패키지 이름은 루트 패키지를 나타낸다. 값은 배포 루트에 상대적인 이름이다. 이 경우 packages=\['foo']라고 할 때, 파일 **src/foo/\__init\_\_.py**가 존재한다고 약속하는 것이다.

**packages**\
이 옵션은 빌드에 포함될 패키지를 설정한다. Setuptools 가 제공하는 find_packages() 함수는 디렉토리를 검색하여 패키지들의 목록을 만들어 준다. 혹 찾아야 할 디렉토리가 다른 곳에 있거나, 포함시키지 말아야 할 패키지가 있다면 인자로 지정할 수 있다.

다음과 같이 packages=\['foo']라고 하면 설정 스크립트(setup.py)가 있는 디렉토리에 상대적으로 foo/\__init\_\_.py 파일을 찾을 것이다.

```python
  packages=['foo']
```

그러나 package_dir 옵션을 사용하여 pacakge dir을 src로 설정했기 때문에 src에 상대적인 경로로 foo 패키지를 찾을 것이다.

이 둘을 합치면 다음과 같다.

```python
    package_dir={'': 'src'},
    packages = ['andyutil'],
```

다음과 같이 foo 패키지 아래에 bar 패키지를 둘 수도 있다.

```
project_root/
   setup.py
   src/
      foo/
        __init__.py
        string_util.py
        bar/
           __init__.py
           util.py 
```

그러나 foo.bar 패키지는 빌드에 포함되지 않는다. 빌드에 포함하려면 다음과 같이 한다.

```python
packages  = find_packages(where='src'),
```

### 다른 패키지 생성

foo 패키지와 동일한 경로에 xyz라는 패키지를 다음과 같이 생성한다.

```
project_root/
   setup.py
   src/
      foo/
        __init__.py
        string_util.py
        bar/
           __init__.py
           util.py 
      xyz/
        __init__.py
        math.py
```

xyz 패키지를 빌드에 포함하려면 다음과 같이 한다.

```python
  packages=['foo', 'xyz'], 
```

위에서 보았듯이 foo.bar는 포함이 안된다. 따라서 다음과 같이 설정한다.

```python
packages  = find_packages(where='src'),
```

## 빌드 파일 청소하기

다시 빌드하려면 빌드된 파일들을 모두 삭제해 주어야 하는데 잘 안된다. 다음 명령어로 빌드된 파일들을 삭제할 수 있다.

```shell
python setup.py clean --all
```

직접 삭제하려면 다음 경로에 있는 디렉토리나 파일들을 삭제한다.

```
project_root/
  build/
  dist/
  src/
     xxx.egg-info
```

## requirements.txt 추가 및 dependency 해결해 주기

아래는 numpy 라이브러리를 사용하는 간단한 프로그램이다.

```python
import numpy as np

arr = np.array([
    [1,2],
    [3,4],
])
print(arr)
```

위와 같은 단순한 코드를 실행할 때 numpy 라이브러리가 설치되어 있지 않다면 다음과 같은 오류가 발생한다.

```shell
(test) PS F:\src\hyeon\latteonterrace\python\python_start> python main.py
Traceback (most recent call last):
  File "F:\src\hyeon\latteonterrace\python\python_start\main.py", line 3, in <module>
    import numpy as np
ModuleNotFoundError: No module named 'numpy'
(test) PS F:\src\hyeon\latteonterrace\python\python_start> 
```

패키지를 배포할 때 의존성 문제를 해결하려면

* 내가 작업하는 패키지의 작업 환경을 알아야 하고
* 그 작업 환경을 사용자가 패키지를 설치할 때 반영되어야 한다.

### 작업환경 알아내기

우선 numpy를 설치한다.

```shell
conda install numpy
```

이 가상 환경 ypcc의 라이브러리 구성을 확인하려면 pip를 사용한다.

```shell
pip freze
```

다음과 같이 출력될 것이다.

```shell
(test) PS F:\src\hyeon\latteonterrace\python\python_start> pip freeze
andystring==1.0
andytestutil==1.1
bleach==3.3.0
certifi==2020.12.5
chardet==4.0.0
colorama==0.4.4
docutils==0.17.1
idna==2.10
importlib-metadata==4.0.1
keyring==23.0.1
mkl-fft==1.3.0
mkl-random @ file:///C:/ci/mkl_random_1618854182866/work
mkl-service==2.3.0
numpy @ file:///C:/ci/numpy_and_numpy_base_1618497416732/work
urllib3==1.26.4
webencodings==0.5.1
wincertstore==0.2
zipp==3.4.1
... 생략
```

이 정보를 알고 있으면 다른 사용자가 내 라이브러리를 사용할 때 이 정보를 이용해서 환경을 구축할 수 있다. 이것을 공유하는 방법은 requirements.txt 파일을 이용하는 것이다.

```shell
pip freeze > requirements.txt
```

## conda 가상환경에서 의존성 관리

### conda 가상환경 추출

requirements.txt 파일을 사용하는 방법을 설명했지만 conda 환경에서는 conda를 이용하자.

모든 세팅이 되어 있는 가상환경을 다른 머신으로 복사하고 싶을 때 사용하면 된다. 아래 명령어는 현재 환경을 environment.yml 파일로 저장한다.

```shell
conda env export --name YOUR_ENV_NAME > environment.yml
```

### 추출한 가상환경으로 새로운 가상환경 생성

앞서 추출한 environment.yml로 가상환경을 생성한다. 설치되어 있던 모든 패키지가 자동으로 설치된다.

```shell
conda env create -f ./environment.yml
```

> 아나콘다 가상 환경에 패키지를 설치할 때는 pip 대신 conda를 사용해야 합니다. 만약 pip를 사용하면 아나콘다 설치 폴더의 Lib/site-packages 안에 패키지가 저장되므로 주의해야 합니다.

## 정리할 내용

* 가이드 문서 배포
* 의존성 추가

## Reference

[배포하기](https://newsight.tistory.com/296)\
[기타 유용한 팁](https://newsight.tistory.com/296)
