# 파이썬 개발환경 with PyEnv 

Pyenv와 virtualenv를 사용하려고 했으나 지금은 많이 사용하지 않고 windows에서 설정을 하기가 너무 어렵다.  Python 3.9.4 부터 가상환경을 지원해서 python이 제공하는 가상환경을 사용하기로 한다. 





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