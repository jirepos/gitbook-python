# Pyinstaller

작성한 Python 스크립트를 실행파일로 배포해야 할 경우, pyinstaller라는 패키지를 사용하면 편리하다.

## pyinstaller 설치

```shell
pip install pyinstaller
```

## 배포파일(실행파일) 생성

```shell
pyinstaller mytest.py
```

## 생성된 실행파일 확인

아래 경로에 실행파일이 생성된다.

```shell
dist/mytest/
```
