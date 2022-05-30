# Class 



## class와 메서드 만들기 
클래스를 정의하고 메서드를 다음과 같이 정의할 수 있다. 
```
class 클래스이름:
    def 메서드(self):
        코드
```        

hello 메서드를 정의해 보자. 

```python
class MyCommon:
  def hello(self):
    print('Hello')
```
다음과 같이 테스트 함수를 작성하고 실행한다. 
```python
from my_common import MyCommon 

def main():
  m = MyCommon()
  m.hello()
if __name__ == "__main__":
  main()
```
**output**    
```
Hello
```

클래스의 인스턴스는 다음과 같이 만든다. 
```
인스턴스 = 클래스()
```
```
m = MyCommon() 
```




### 파라미터 전달 
self 다음에 파라미터를 추가한다. 

```python 
  def hello2(self, greeting):
    print(greeting)
```
다음과 같이 파라미터를 전달한다. 
```
def hello2():
  m = MyCommon()
  m.hello2("안녕")
```
**output**    
```
안녕
```





```python
class MyCommon:
  def __init__(self):
    self.name = "MyCommon"
```
```python
def main():
  mcommon = MyCommon()
  print(mcommon.name)
```




## 속성 사용하기 

\_\_init\_\_ 메서드 안에서 self.속성에 값을 할당한다. 
```
class 클래스이름:
    def __init__(self):
        self.속성 = 값
```        

init() 생성자 메서드에서 속성을 정의한다.  메서드 내부에서는 self.으로 속성에 접근한다. 
```python
class MyCommon:
  def __init__(self):
    self.greeting = "안녕하세요"  # self로 속성 정의 
    
  def say_greeting(self):
    print(self.greeting)  # 메서드 내부에서 속성에 접근할 때 self 사용 
```

>  self는 인스턴스 자기 자신을 의미한다. 


다음과 같이 코드를 작성하고 실행한다. 

```python

from my_common import MyCommon 


def greeting():
  m = MyCommon()
  m.say_greeting()


if __name__ == "__main__":
  greeting()
```
**output**    
```
안녕하세요
```

외부에서 속성을 사용할 때는 다음과 같은 형식을 사용한다 
```
인스턴스.속성
```
greeting() 메서드를 다음과 같이 실행하고 실행해 본다. 
```
def greeting():
  m = MyCommon()
  m.say_greeting()
  print(m.greeting)
```  
**output**    
```
안녕하세요
안녕하세요
```

### 비공개 속성 사용 
비공개 속성은 밑줄 두개를 사용한다. 비공개 속성은 외부에서 접근할 수 없다. 

```python 
class MyCommon:
  def __init__(self):
    self.greeting = "안녕하세요"
    self.__age = 10 
```    

> 공개 속성과 비공개 속성
클래스 바깥에서 접근할 수 있는 속성을 공개 속성(public attribute)이라 부르고, 클래스 안에서만 접근할 수 있는 속성을 비공개 속성(private attribute)이라 부른다. 



