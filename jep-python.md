# Jep-자바 임베디드 Python

Jep는 JNI를 통해 Java에 CPython을 포함한다. 공식 사이트는 [여기](https://github.com/ninia/jep)를 참조한다.

JVM에 CPython을 포함하면 다음과 같은 이점이 있다.

* 네이티브 Python 인터프리터를 사용하는 것이 대안보다 훨씬 빠를 수 있다.
* Python은 성숙하고 잘 지원되며 잘 문서화되어 있다.
* 네이티브 CPython 확장 및 Python 기반의 고품질 Python 모듈에 액세스한다.
* 컴파일러와 다양한 Python 도구는 언어만큼이나 성숙하다.
* Python은 해석 된 언어이므로 재 컴파일하지 않고도 설정된 Java 코드를 스크립팅 할 수 있다.
* Java와 Python은 모두 크로스 플랫폼이므로 다른 운영 체제에 배포 할 수 있다.

## 설치

pip install jep소스를 실행 하거나 다운로드하고 python setup.py build install. 빌드하고 설치하려면 JDK, Python 및 선택적으로 numpy를 미리 설치해야합니다.

## 종속성

* Python 2.7, 3.3, 3.4, 3.5, 3.6, 3.7, 3.8 또는 3.9
* 자바> = 1.7
* NumPy> = 1.7 (선택 사항)
