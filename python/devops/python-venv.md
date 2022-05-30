# 파이썬 개발환경 with PyEnv 
Pyenv와 virtualenv를 사용하려고 했으나 지금은 많이 사용하지 않고 windows에서 설정을 하기가 너무 어렵다.  Python 3.3 부터 가상환경을 지원해서 python이 제공하는 가상환경을 사용하기로 한다. 


Python의 가상환경은  Python 버전을 바꾸거나 하지는 못한다. 설치된 버전에서 프로젝트에 맞게 패키지만 분리하여 설치하고 관리하기 위한 것이다. 


**가상환경을 사용해야 하는 이유**   

* pip는 시스템 전역으로만 패키지를 설치할 수 있다. 
* 파이썬의 가상 환경을 이용하면 프로젝트 별로 따로 패키지를 설치하고, 
* 다른 프로젝트로 부터 격리시킬 수 있다. 





## 파이썬 가상환경 사용 

가상환경은 점(.)으로 시작하는게 관례이다. 
### 가상환경 생성
```
python -m venv .venv
```

해당 명령을 사용한 폴더 아래에 .venv 디렉터리가 생긴다. 

> .venv 디렉터리는 버전관리에 올릴 필요가 없으므로 .gitignore에 추가한다. 



### 가상환경 활성화 
```
.venv/bin/activate 
```

가상환경에서 패키지 설치 
```
(.venv)$ pip install requests
```
.venv/lib/pytohon3/stie-packages에 해당 패키지가 설치된다. 




### 가상환경 피활성화 
deactivate만 입력하면 된다. 

```
deactivate
```



## Pyenv를 사용한 가상환경 
Pyenv를 사용하면 여러개의 Python 버전을 시스템에 설치해서 사용할 수 있다. 그러나 Windows에서는 사용이 좀 어렵다.  Python을 실행하기 위해서는 path를 매뉴얼로 설정해야 해서 불편하다. 


Choco를 설치한다. 관리자권한으로 터미널을 실행해야 한다. 
다음을 실행한다.  자세한 내용은 [Setup/Install](https://docs.chocolatey.org/en-us/choco/setup#install-with-cmd.exe)을 참고한다. 
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```


Anaconda는 지운다. 
choco를 이용하여 설치한다. 

```
choco install pyenv-win
```

```
# pyenv의 버전확인
pyenv version
 
# pyenv의 업데이트
pip install --upgrade pyenv-win
 
# 설치가능한 버전 목록 확인
pyenv install -l
 
# python 3.9.6 버전으로 설치
pyenv install 3.9.6
 
# 설치한 모든 python version 확인
pyenv versions
 
# python 2개 버전(2.4.3 + 3.9.6) 동시설치
pyenv install 2.4.3 3.9.6
 
# 기본 적용할 python 버전 지정
pyenv global 3.9.6
 
# 해당폴더에 사용할 python 버전 지정
pyenv local 3.9.6
 
#설치된 특정버전의 python 제거
pyenv uninstall 3.9.6
```



Terminal에서 지정된 python을 쓰려면 Alais를 설정하여 사용한다. 

```
Set-Alias python (pyenv which python)
```

http://taewan.kim/post/python_virtual_env/


https://thekkom.tistory.com/69