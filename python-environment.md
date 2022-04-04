# Python 개발환경 가이드

## Anaconda 설치

설치 위치를 C:\Anaconda로 설정하여 설치한다.

![](<.gitbook/assets/image (14).png>)

Add Anaconda 3 to ....는 체크하지 않는다.

![](<.gitbook/assets/image (16) (1).png>)

> 이렇게 하면 conada 명령어를 찾지 못한다. 환경 변수에 잡혀 있지 않기 때문이다. cmd 창이나 powershell 창에서 conda 명령을 실행하려면 C:\Anaconda\condabin 폴더를 시스템 PATH 변수에 추가해야 한다.

PowerShell 창에서 conda를 사용하려면 Windows 시작 메뉴에서 Anaconda Powershell Prompt 클릭하여 실행한다.

![](<.gitbook/assets/image (15).png>)

conda 버전 확인한다.

![](<.gitbook/assets/image (8) (1).png>)

## Anaconda에서 Python 설치

### 가상환경이란

독립적인 작업환경에서 패키지 및 버전관리를 하기 위한 가상의 환경을 말한다. 필요한 라이브러리를 계속 Base에 설치하다보면, 시간이 지남에 따라 경고문구나 라이브러리들의 의존성(dependancy) 문제로 인해 다양한 오류들을 만나게 되는 어려움이 있을 수 있다. 또한 프로젝트마다 활용하는 다양한 라이브러리(특히 tensorflow 같은 경우)끼리 호환문제도 적지 않다. 다른 환경(컴퓨터, 서버 등)에서 동일한 작업을 하려면 이 환경을 갖간단히 똑같이 만들어야 하는데 conda를 이용하면 쉽게해결 할 수 있다. 이런 작업 환경들을 프로젝트별로 관리하고 공유할 수 있도록 도와 주는 것이 가상환경이다.

conda 명령어를 사용하여 사용할 python의 버전을 설정하여 가상 환경을 만든다.

```shell
conda create –name test python=3.9.0
```

가상환경이 생성되면 다음과 같은 내용이 출력되는 것을 볼 수 있다.

![](<.gitbook/assets/image (7) (1).png>)

## VSCode에서 Python Extension 설치

VSCode를 실행하여 Python extenstion을 설치한다.

![](<.gitbook/assets/image (6) (1).png>)

\
Terminal을 연다. 아래와 같은 경고 창이 뜨면 무시한다.

![](<.gitbook/assets/image (9) (1).png>)

PowerShell이 base 환경으로 기본적으로 설정되는 것을 볼 수 있다.

![](<.gitbook/assets/image (12).png>)

**가상환경 선택**

Ctrl+Shift+P 입력하고 select interprete를 입력한 다음에 생성한 가상환경의 python을 선택한다.

![](<.gitbook/assets/image (3).png>)

## VSCode의 Terminal에서 가상환경 선택

interpreter에서 가상환경의 python을 선택했음에더 여전이 termina에서 가상환경은 base로 되어 있다.

**가상환경 솰성화**

Terminal 창을 오픈한 다음에 다음의 명령어로 terminal의 가상환경을 활성화 한다.

```shell
conda activate test
```

## Anaconda 명령어

### PowerShell과 conda 연동

PowerShell와 conda를 상호 연동하려면 "conda init" 명령어를 실행해야 한다.

```shell
conda init
```

### version 확인 및 conda 업데이트

```shell
# 아나콘다 버전확인 
conda --version 
# 아나콘다 업데이트 
conda update conda
```

### 가상환경 생성

```shell
# 아나콘다 가상환경 생성
conda create --name(-n) 가상환경명 설치할 패키지
#예) 파이썬 3.5 버전 설치, test 이름으로 가상환경 생성
conda create --name test python=3.5
# 또는
conda create --n test python=3.5
```

package\_spec을 명시해서 필요한 패키지를 가상환경 생성시에 한번에 설치할 수 있다.

```shell
conda create --name YOUR_ENV_NAME python=3.6.5 tensorflow keras
```

### 아나콘다 가상환경 활성화, 비활성화

```shell
# 설치된 가상환경 리스트 확인
conda info --envs

# 가상환경 활성화
conda activate 가상환경명

# 가상환경 비활성화
conda deactivate 가상환경명 
```

### 가상환경 확인

현 아나콘다의 가상환경들의 리스트 전부 보기

```shell
conda env list
```

### 패키지 설치 , 패키지 확인

```shell
# 패키지 설치
# conda install 패키지명
conda install simplejson

#패키지 리스트 확인
conda list
```

### 패키지 삭제

```shell
conda remove 패키지명
```

### 가상환경 삭제

```shell
# 패키지 삭제
conda remove --name 가상환경명 --all
# 또는
conda remove -n 가상환경명 --all
```

### 아나콘다 클린(clean)

```shell
# 아나콘다 클린
conda clean --all
# 또는
conda clean -a
```

### 가상환경 추출

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

### 다른 사람에게도 똑같은 가상환경 전수하기

```shell
conda env export > conda_requirements.txt
```

이렇게 만들어둔 requirements.,txt 를 다른사람이 github 로 부터 받는다면 그 사람은 다음과 같은 명령어로 동일한 환경의 가상환경을 구축할 수 있다.

```shell
conda env create -f conda_requirements.txt
```

이렇게 만들어진 파일을 github 에서 받는다면, 받는쪽에서는

### pip install 내용만 전달하기

```shell
pip freeze > requirements.txt
```

이렇게 만들어진 파일을 github 에서 받는다면, 받는쪽에서는

```shell
pip install -r requirements.txt
```

로 동일한 pip 환경을 구축할 수 있습니다.

## Anaconda 가상환경 폴더

Anaconda를 C:\Anaconda3 폴더에 설치했다고 가정한다. 폴더 구조는 다음과 같다.

```
Anaconda3/
  python.exe
  envs
  pkgs/
```

**ROOT\_DIR**\
아나콘다 또는 Miniconda가 설치된 디렉토리를 말한다. 여기서는 C:\Anaconda3가 된다. 루트 디렉토리 아래에는 anaconda가 사용하는 기본 python 인터프리터가 있다.

**pkgs**\
아나콘다를 처음 설치하면 모든 패키지들이 pkgs 폴더에 설치된다. 이 패키지들은 conda를 사용할 때 참조된다. 기본환경(base)에서는 이 패키지들 모두가 참조된다.

가상환경을 만들 때 선택한 패키지들만이 pkgs 폴더의 패지들과 연결된다. 다음의 하위 디렉토리들이 기본 Anaconda 환경을 구성한다.

```
/bin
/include
/lib
/share
```

**envs**\
가상환경을 만들면 envs 디렉토리 아래에 생성된다. 다음과 같이 가상환경을 생성했다고 가정하자.

```shell
conda create --name jep python=3.9.0
```

Anaconda3/envs 디렉토리 하위에 jep 디렉토릴가 생성된다.

```
Anaconda3/
  envs/
    jep/
```

jep 디렉토리 바로 아래에 3.9.0 버전의 python이 설치된다.

```
Anaconda3/
  envs/
    jep/
       python.exe
```

numpy 패키지를 설치를 해보자. 가상환경 디렉토리의 Lib 디렉토리 아래의, site\_packages 디렉토리 아래에 설치되는 것을 확인할 수 있다.

```
Anaconda3/
  envs/
    jep/
       python.exe
       Lib/
         site_packages/
           numpy/
```

> 가상환경에서 pip로 설치를 해도 같은 경로에 설치가 되는 것을 확인하였다. 그러나 여전히 아나콘다 환경에서 pip 로 패키지 설치를 하라는 튜토리얼이나 강좌가 많이 있다. 한가지 이유는 conda 패키지는 미리 빌드된 패키지를 만들고 패키지 의존성까지 맞추기 때문에, pip의 pypi 서버만큼 빨리 최신버전이 올라오지 않는다. 그래서 tensorflow 의 아주 최신 버전이 pip 로는 설치가 되는데, conda 로는 설치가 안되는 상황이 발생하기도 하고, 이런 때에는 부득이 pip 로 설치해야 할 것이다. 하지만, 부득이 pip 로 어떤 패키지를 설치하게 되면, 이후에 conda update나 conda install, uninstall 등으로 패키지를 변경할 경우에 pip 로 설치되었던 패키지는 업데이트에서 누락이 되거나, 다른 버전이 두가지가 깔려있는 것처럼 보이거나 하는 경우가 발생할 수 있다. 초보자들이 개발환경을 설정하면서 겪는 쓸데없는 어려움이 pip 와 conda 로 패키지를 깔았다 지웠다 하면서 생긴 것이 아닐까 하는 의심이 있다. 자신이 그런 어려움을 겪고 있다면, 위에 링크한 아나콘다 패키지 설치 원칙을 한번 잘 읽어보고 주의하길 바란다.

## 아나콘다 최신 버전 업데이트

```shell
conda update -n base conda
```

이 명령은 기본 뼈대를 업데이트하라는 내용. 아나콘다에는 이미 패키지가 들어 있다고 했으니까 패키지도 같이 업데이트해야겠죠? 이렇게 입력하면 됩니다.

```shell
conda update --all
```

마지막 단계는 PIP 업데이트. 파이선 3.4부터 PIP를 내장하고 있는데 자동 업데이트 방식은 아닙니다. 그래서 기회가 있을 때마다 업데이트 해놓는 게 좋습니다.

```shell
python -m pip install --upgrade pip
```

아래 방법으로도 가능하다.

```shell
pip install --upgrade pip
```

### 기타

#### conda-forge

따라서 conda-forge는 패키지를 설치할 수있는 추가 채널입니다. 이런 의미에서 기본 채널이나 사람들이 패키지를 게시 한 다른 수백 개 (수천?)보다 특별한 것은 아닙니다. https://anaconda.org에 가입하고 자신의 Conda 패키지를 업로드하면 자신의 채널을 추가 할 수 있습니다.

채널 옵션을 변경하는 방법은 두 가지가 있습니다. 하나는 패키지를 설치할 때마다 채널을 지정하는 것입니다.

```shell
conda install -c some-channel packagename
```

물론 패키지는 해당 채널에 존재해야합니다. 동일한 채널을 자주 사용하는 경우 구성에 채널을 추가 할 수 있습니다. 당신은 쓸 수 있습니다

```shell
conda config --add channels some-channel
```

some-channel 채널을 channels 구성 목록의 맨 위에 추가하십시오. 이것은 some-channel에게 가장 높은 우선 순위를 부여합니다 (우선 순위는 하나 이상의 채널에 특정 패키지가있을 때 어떤 채널이 선택되는지 결정합니다 (in part)). 목록의 끝에 채널을 추가하고 가장 낮은 우선 순위를 지정하려면 다음을 입력하십시오.

```shell
conda config --append channels some-channel
```

추가 한 채널을 삭제하려면 다음을 작성하십시오.

```shell
conda config --remove channels some-channel
```

아나콘다가 관리하는 conda-forge 채널 대신 defaults 채널을 사용해야하는 네 가지 주요 이유가 있습니다.

conda-forge의 패키지는 defaults 채널의 패키지보다 최신 버전 일 수 있습니다. conda-forge 채널에는 defaults에서 사용할 수없는 패키지가 있습니다 openblas (conda-forge) 대신 mkl (defaults)와 같은 종속성을 사용하는 것이 좋습니다.

## VSCode에서 Debugging하기

### test.py 생성

test.py 파일을 만들고 파이썬 코드를 작성한다.

```python
print("hello Python")
print("hello Python")
print("hello Python")
print("hello Python")
print("hello Python")
```

### breakpoint 설정

숫자 왼쪽 부분을 클릭하여 break point 를 설정한다.

![](<.gitbook/assets/image (11).png>)

### debug 실행(F5)

F5를 클릭하여 DEBUG MODE로 실행한다.

![](<.gitbook/assets/image (10) (1).png>)

Break Point가 설정된 곳에서 실행이 멈춘 것을 확인할 수 있다.

\\

![](<.gitbook/assets/image (1).png>)

## VSCode에서 Python 소스 디렉토리 만들기

간단한 테스트 코드를 작성하는 것이 아니라면 가급적 프로젝트 구조 및 모듈화를 고민하면서 개발하는 것이 좋다. 프로젝트 루트에 src 디렉토리를 만들고 모듈화를 하면서 개발하자.

프로젝트 구조는 다음과 같다.

```shell
project_root/
  src/
```

파이썬 소스들을 src 디렉토리 안에 둘 것이다.

src 디렉토리에 myutil.py 파일을 생성하고 다음과 같이 작성한다.

```python
def add(a,b):
  return a + b
```

프로젝트 루트에 main.py를 생성하고 다음과 같이 작성한다.

```python
import src.myutil as myutil 

print(myutil.add(1,2))
```

그런데 src.mytuil로 import하고 있다. 이것은 우리가 원하는 방식이 아니다. import myutil이어야 하는데 myutil이라고 입력을 하면 찾을 수 없다고 나온다.

![](<.gitbook/assets/image (4).png>)

### import가 moudule과 package를 찾아 가는 경로

util이라는 패키지가 있다고 가정하자.

```python
import util
```

이 util 패키지를 사용하려면 일단 util이 어디에 있는지 찾아야 한다. 이 때 파이썬은 3가지 장소를 순서대로 순환하면서 찾는다.

* sys.modules
* built-in modules
* sys.path

sys.modules는 파이썬이 module이나 package를 찾기 위해 가장 먼저 둘러보는 곳으로, 이미 import된 module과 package들을 dictionary 형태로 저장하고 있다. 즉, 이미 import된 것들은 다시 찾지 않도록 해주며, 새로 import하는 모듈과 패키지들은 여기에서 찾을 수 없다.

build-in modules는 파이썬에서 제공하는 공식 라이브러리들이다. 이 라이브러리들은 python에 포함되어 있다.

마지막으로 파이썬은 sys.path에서 모듈과 패키지들을 찾는다. sys.path는 다음 디렉토리들로 초기화 된다.

* 입력 스크립트를 포함하는 디렉토리(또는 파일이 지정되지 안았을 때에는 현재 디렉토리)
* PYTHONPAHT(디렉토리 이름들의 목록, 셀 변수 PATH와 같은 문법)
* 설치 의존적인 기본값

마지막 순서인 sys.path에서도 찾지 못하면 파이썬은 ModuleNotFoundError를 반환한다.

하지만 위의 순서를 따라 파이썬이 모두 찾아도 찾을 수 없는 모듈과 패키지가 있을 수 있는데, 바로 직접 개발한 로컬 패키지이다. 이런 경우 import 경로를 직접 입력해 주어야 한다.

**절대경로**\
절대 경로는 import하는 파일이나 경로에 상관없이 항상 동일한 경로로 작성한다. project>package1>module1.py라는 경로에 모듈이 위치하고 있다면 프로젝트 루트 아래의 디렉토리부터 시작한다.

```python
from package1.module1 import function1
```

현재 디렉토리는 default로 sys.path에 포함된다. 절대 경로는 현재 디렉토리부터 경로를 시작하게 된다.

**상대경로**\
상대 경로는 현재 import를 하는 위치를 기준으로 경로를 정의한다. (이 내용은 보완할 예정임)

### PYTHONPATH 설정하기

파이썬이 모듈을 찾는 방법에서 살펴 보았듯이 파이썬이 실행되는 스크립트 파일의 디렉토리를 기준으로 모듈을 탐색하기 때문에 src로 시작하지 않으면 모듈을 찾을 수 없다.

이 문제를 해결하기 위해서 PYTHONPATH를 설정해야 한다.

VSCode에서 Run and Devug 버튼을 클릭한다. 또는 Ctrl + Shift + D를 입력한다.

![](.gitbook/assets/image.png)

create a lunch.json file 링크를 클릭한다. Python File을 클릭한다.

![](<.gitbook/assets/image (2) (1).png>)

그러면, 프로젝트 루트 아래 .vscode 디렉토리 아래에 lunch.json 파일이 생성된다.

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: Current File",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal"
    }
  ]
}
```

이 파일에 env 옵션을 다음과 같이 추가한다.

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: Current File",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal",
      "env": {
        "PYTHONPATH":"${workspaceFolder}/src"
      }
    }
  ]
}
```

> 반드시 패키지로 만들 디렉토리는 \_\_init\_\_.py 파일을 생성해야 한다.

위와 같이 하면 실행하는데 문제는 없지만 여전히 import 구문에 오류가 표시된다. 아직 해결방법은 찾지 못했다.
