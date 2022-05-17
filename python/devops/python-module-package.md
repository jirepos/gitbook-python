# Python 모듈과 패키지

## 라이브러리

라이브러리와 패키지는 둘다 다른 프로그램에서 포함하여 사용할 수 있는 일종의 코드 모음이라고 생각하면 쉽다.

* 여러 모듈과 패키지를 묶어 라이브러리라고 한다.
* 파이썬을 설치할 때 본적으로 설치되는 라이브러리를 표준라이브러리(Python Standard Library)라고 한다. (예: time, sys, os, math, random, urllib 등)
* python.org가 아닌 외부 3rd party에서 개발한 모듈과 패키지를 묶어서 외부 라이브러리(Third Party Library)라고 한다. (예: requests, scrapy, webbrowser 등)
* 3rd party를 통해 개발된 모듈, 패키지가 표준라이브러리보다 더 우수하거나 사용하기 쉬운 경우도 있다. (예: Numpy, Scipy, requests)

## 모듈과 패키지

### 모듈

모듈이란 함수나 전역 변수 또는 클래스를 모아 놓은 파일이다. 모듈은 다른 파이썬 프로그램에서 불러와 사용할 수 있게끔 만든 파이썬 파일이라고도 할 수 있다. 우리는 파이썬으로 프로그래밍을 할 때 굉장히 많은 모듈을 사용한다. 다른 사람들이 이미 만들어 놓은 모듈을 사용할 수도 있고 우리가 직접 만들어서 사용할 수도 있다. 여기에서는 모듈을 어떻게 만들고 사용할 수 있는지 알아보겠다.

### 패키지

패키지(Packages)는 도트(.)를 사용하여 파이썬 모듈을 계층적(디렉터리 구조)으로 관리할 수 있게 해준다. 예를 들어 모듈 이름이 A.B인 경우에 A는 패키지 이름이 되고 B는 A 패키지의 B모듈이 된다.

## 모듈 만들기

프로젝트 루트에 두 개의 모듈(파일)을 만들 것이다.

```
project_root
  util.py
  main.py 
```

프로젝트 루트에 util.py를 만든다.

```python
def hello():
  print("hello")
```

프로젝트 루트에 main.py를 만든다.

```python
from util import hello

hello()
```

만든 모듈을 불러 올 때에는 from 다음에 모듈 이름(파일이름) 을 쓰고 import 다음에는 함수이름을 쓴다.

```python
from 모듈이름 import 함수이름 
```

모듈에 변수나 함수, 클래스가 여러개 일 때는 import 다음에 \*을 써주면 한번에 다 불러올 수 있다.

```python
from 모듈이름 import *
```

util 모듈에 greeting이라는 함수를 하나 더 만든다.

```python
def hello():
  print("hello")
  
def greeting():
  print("greeting")  
```

main 모듈에서 greeting() 함수를 사용하는 코드를 추가한다. import에 greeting을 추가해야 한다.

```python
from util import hello, greeting
hello()
greeting()
```

import util 구문을 사용하여 모듈을 가져울 수 있다. 이 경우에는 모듈내의 객체를 식별하기 위해서 "모듈명.함수명" 형식으로 함수를 사용한다.

```python
import util 

util.hello()
util.greeting()
```

### 모듈과 시작점

util.py 모듈에 다음을 추가한다.

```python
if __name__ == "__main__": 
  hello()
```

이 구문이 있으면 모듈을 명령행으로 실행할 때 바로 실행되도록 한다. 모듈로서 import 될 때에는 실행되지 않는다.

```shell
$ python util.py 
```

파이썬은 최초로 시작하는 스크립트 파일과 모듈의 차이가 없다. 어떤 스크립트 파일이든 시작점도 될 수 있고, 모듈도 될 수 있다. 그래서 name 변수를 통해 현재 스크립트 파일이 시작점인지 모듈인지 판단한다.

if name == 'main': 처럼 name 변수의 값이 'main'인지 확인한느 코드는 현재 스크립트 파일이 프로그램의 시작점이 맞는지 판단한다.

### 모듈의 위치 탐색

파이썬에서 모듈을 다음의 순서대로 찾는다.

* 첫 번째는 프로그램이 실행된 디렉토리내에서 모듈을 찾는다.
* 두 번째는 PYTHONPATH 라는 환경변수에 지정된 디렉토리에서 찾는다.
* 세 번째는 파이썬 라이브러리 디렉토리에서 찾는다. 이곳은 파이썬을 설치한곳 아래 Lib 디렉토리이다.

### import keyword

#### import로 모듈 가져오기

* 모듈은 import 키워드로 가져올 수 있다. 모듈을 여러 개 가져올 때는 모듈을 콤마로 구분한다.
* import 모듈
* import 모듈1, 모듈2
* 모듈.변수
* 모듈.함수
* 모듈.클래스

간단하게 파이썬 표준 라이브러리의 math 모듈을 가져와 서 사용해 보자.

```python
import math 
print(math.pi)
```

#### import as 로 모듈 이름 지정하기

모듈의 함수를 사용할 때 math.sqrt처럼 일일이 math를 입력하기 귀찮을 때 import as를 사용하여 모듈의 이름을 지정할 수 있다.

* import 모듈 as 별칭

```python
import math as m 

print(m.sqrt(4.0))
```

#### from import로 모듈의 일부만 가져오기

이번에는 from import로 원하는 변수만 가져올 수 있다.

* from 모듈 import 변수

```python
from math import pi 
print(pi)
```

함수나 클래스를 가져올 때에도 사용할 수 있다.

* from 모듈 import 함수
* from 모듈 import 클래스

모듈에서 변수, 함수, 클래스 등 여러개를 가져올 때도 사용할 수 있다.

* from 모듈 import 변수, 함수, 클래스

from import는 모듈의 모든 변수, 함수, 클래스를 가져오는 기능도 있다.

* from 모듈 import \*

#### from import로 모듈의 일부를 가져온 뒤 이름 지정하기

* from 모듈 import 변수 as 이름
* from 모듈 import 함수 as 이름
* from 모듈 import 클래스 as 이름

그럼 여러 개를 가져왔을 때 각각 이름을 지정할 수는 없을까? 이때는 각 변수, 함수, 클래스 등을 콤마로 구분하여 as를 여러 개 지정하면 된다.

* from 모듈 import 변수 as 이름1, 함수 as 이름2, 클래스 as 이름3

## 패키지 만들기

패키지는 모듈을 디렉토리형식으로 구조화한 것이다. 모듈들을 넣어 둔 디렉토리 명이 패키지명이 된다.

폴더 안에 \__init\_\_.py 파일이 있으면 해당 폴더는 패키지로 인식한다. 파이썬 3.3 이상부터는 init.py 파일이 없어도 패키지로 인식된다.하지만 하위 버전에도 호환되도록 init.py 파일을 작성하는 것을 권장한다.

> \__init\_\_.py 파일이 없어도 된다고 나와 있지만 배포할 때 이 파일이 없으면 패키지가 배포되지 않는다. 꼭 만들도록 한다.

### 간단한 패키지 만들기

프로젝트 루트에 calc라는 폴더(패키지)를 만든다. 그 안에 util.py를 생성한다.

```
project_root/
   calc/
     __init__.py
     util.py 
   main.py 
```

util.py 모듈에 add() 함수를 추가한다.

```python
def add(a,b):
  return a + b
```

main.py 모듈에 util 모듈을 import한다.

```python
from calc.util import add 

result = add(1,2)
print(result)
```

F5를 눌러 실행하면 terminal에 3이 출력될 것이다.

### 하위 패키지 만들기

이번에는 calc 패키지 하위에 string이라는 패키지를 만들고 그 아래 string_util.py 모듏을 만든다.

```
project_root/
   calc/
     __init__.py
     util.py 
     string/  # 새로 생성
       string_uti.py
   main.py 
```

sring_util.py에 다음과 같이 append 함수를 작성한다.

```python
def append(a,b):
  return a + b
```

main.py 모듈에서 from import 키워드를 사용하여 패키지를 import하여 append 함수를 사용할 수 있다.

```python
from calc.util import add 
from calc.string.string_util import append 

result = add(1,2)
print(result)
result2 = append("kim", "park")
print(result2)
```

### 패키지 import하기

패키지는 특정 기능과 관련된 여러 모듈을 묶은 것인데, 패키지에 들어있는 모듈도 import를 사용하여 가져올 수 있다.

* import 패키지.모듈
* import 패키지.모듈1, 패키지.모듈2
* 패키지.모듈.변수
* 패키지.모듈.함수
* 패키지.모듈.클래스

#### import as로 패키지 모듈 이름 지정하기

* import 패키지.모듈 as 이름

패키지도 from import를 사용하여 모듈에서 변수, 함수, 클래스를 가져올 수 있다.

* from 패키지.모듈 import 변수
* from 패키지.모듈 import 함수
* from 패키지.모듈 import 클래스
* from 패키지.모듈 import 변수, 함수, 클래스

패키지의 모듈에서 모든 변수, 함수, 클래스를 가져오는 방법은 다음과 같다.

* from 패키지.모듈 import \*

#### from import로 패키지의 모듈의 일부를 가져온 뒤 이름 지정하기

* from 패키지.모듈 import 변수 as 이름
* from 패키지.모듈 import 변수 as 이름, 함수 as 이름, 클래스 as 이름
