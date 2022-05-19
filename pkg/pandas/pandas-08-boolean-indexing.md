# 불린 인덱싱

불리언 색인은 array의 색인이 True, False 값을 가지게 하여 True 인 경우에 해당 array 값을 출력하게 함.


```python
import pandas as pd 

df = pd.DataFrame( {
    "age": [ 10,20,30,40],
    "name": [ '홍길동', '박신혜', '정일종', '강무연']
})

df
```

```
	age	name
0	10	홍길동
1	20	박신혜
2	30	정일종
3	40	강무연
```


pandas 의 DataFrame의 Value에 True, False 값이 인덱스별로 설정됨.

```python 
df.age > 20
```
```
0    False
1    False
2     True
3     True
Name: age, dtype: bool
```


```pyhon 
# age가 20보다 큰 인덱스의 값이 출력된다.
df[df.age > 20]
```
```
age	name
2	30	정일종
3	40	강무연
```


```python 
# 나이가  > 10 이고 < 50 인 것들
(df.age > 10 ) & ( df.age < 50)
```
```
0    False
1     True
2     True
3     True
Name: age, dtype: bool
```


```python 
df[(df.age > 10 ) & ( df.age < 50)]
```
```
	age	name
1	20	박신혜
2	30	정일종
3	40	강무연
```


## ix 함수
1.3.5 에서는 ix[]를 지원하지 않아 작성하지 않는다. 
```python 
print( pd.__version__ )
```
