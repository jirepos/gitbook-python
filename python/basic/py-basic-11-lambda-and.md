# 람다,맵,리듀스,필터

## 람다(Lambda)

# 두 수의 합을 구하는 함수 
```python 
def add(x, y):
  return x + y

add(10,20)
```
```
30
```
lambda 형식으로 다음과 같이 작성한다.

```python
lambda 매개변수 : 표현식
```
```python 
(lambda x,y: x +y)(10, 20)
```
```
30
```
## 맵(Map)

```python
map(함수, 리스트)
```
```python 
list(map(lambda x: x ** 2, range(5)))
```
```
[0, 1, 4, 9, 16]
```

## 리듀스(reduce)
원소들을 누적적으로 함수에 적용한다.
```python
reduce(함수, 시퀀스)
```
```python 
from functools import reduce
reduce(lambda x, y: x + y, [0, 1, 2, 3, 4])
```
```
10
```


## 필터(filter)
```python
filter(함수, 리스트)
```

원소들을 함수에 적용시켜서 결과가 참인 값들로 새로운 리스트를 만든다.
```python 
list(filter(lambda x: x < 5, range(10))) # 파이썬 2 및 파이썬 3
```
```
[0, 1, 2, 3, 4]
```
