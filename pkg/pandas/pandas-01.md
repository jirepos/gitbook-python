# Pandas Series

```
import numpy as np
import pandas as pd
```

## 시리즈만들기
### 리스트로 시리즈 만들기
리스트로 간단하게 시리즈를 만들어 보겠습니다. 

```python 
# Make a series with list of Python 
s = pd.Series([1, 3, 5, 4, 6, 8])
print(s)
# int65 타입으로 시리즈가 만들어 진다
```
```
0    1
1    3
2    5
3    4
4    6
5    8
dtype: int64
```
시리즈의 타입을 확인해 보죠. pandas.core.series.Series 타입입니다.  
```python 
print(type(s))
```
```
<class 'pandas.core.series.Series'>
```

인덱스는 별도로 지정하지 않으면 순서대로 생성됩니다.

```python 
# 인덱스로 값을 꺼낸다. 
print(s[0])
print(s[3])
```
```
1
4
```
```python
#  0번째에서 3번째까지(3번째, 실제로는 4번째) 잘라낸다. 
s2 =  s[:3]
print(type(s2))   # type이 Series인 것을 확인할 수 있다. 
print(s2)  # 0~2까지 반환된 것을 확인할 수 있다. 
```
```
<class 'pandas.core.series.Series'>
0    1
1    3
2    5
dtype: int64
```

### 사전으로 시리즈 만들기 
사전의 key가 인덱스가 됩니다. 
```python
d = {"a": 0.0, "b": 1.0, "c": 2.0}
# d가 사전인지 확인 
print("Is this the type of dictionary? " ,  type(d))
# 사전이 맞네요 
pd.Series(d)
print(d['b'])   # 인덱스를 사용해서 값을 구합니다. 
```

```
Is this the type of dictionary?  <class 'dict'>
1.0
```

인덱스를 다시 설정하겠습니다.

```python 
# index를 다시 설정하는데 
print(pd.Series(d, index=["b", "c", "d", "a"]))
# d라는 인덱스의 값이 없어서 d의 갑은 NaN 
# 인덱스의 순서가 변경되는데 값은 정상적으로 매핑이 된다. 
print(d)
```

```
b    1.0
c    2.0
d    NaN
a    0.0
dtype: float64
{'a': 0.0, 'b': 1.0, 'c': 2.0}
```

다음은 시리즈 생성할 때 index를 같이 설정합니다.
```python 
s = pd.Series([1, 3.5, 5, 4, 6.2, 8], index=['a', 'b', 'c', 'd', 'e', 'f'])
print(s)  # 시리즈를 확인합니다. 
print(s['c'])  # index의 값을 구합니다.
```
```
a    1.0
b    3.5
c    5.0
d    4.0
e    6.2
f    8.0
dtype: float64
5.0
```



### 시리즈 인덱스

```python 
data = [ 1000, 2000, 3000]
s = pd.Series(data)
print(s.index)  # index를 지정하지 않으면 0부터 시작하는 숫자 값을 RangeIndex 타입으로 설정
print(s.index.to_list()) # 배열로 반환 
```
```
RangeIndex(start=0, stop=3, step=1)
[0, 1, 2]
```


```python 
# 인덱스 변경 
s.index = ["사과","배","딸기"]
print(s.index )  # 인덱스가 변경된다. 
print('\n')
print(s) # 시리즈 확인
```
```
Index(['사과', '배', '딸기'], dtype='object')


사과    1000
배     2000
딸기    3000
dtype: int64
```


### 키워드 사용하여 시리즈 생성
```python 
idx = ["사과","배","딸기"]
data = [ 1000,  2000, 3000 ]
s = pd.Series(data=data, index=idx)   # 키워드를 사용하여 시리즈를 생성
print(s)
```

```
사과    1000
배     2000
딸기    3000
dtype: int64
```
키값을 확인해보자
```python
print(s.keys())   # 인덱스 키 확인
```
```
Index(['사과', '배', '딸기'], dtype='object')
```
키와 데이터를 조회해보자
```python 
print(list(s.items()))
```

```
[('사과', 1000), ('배', 2000), ('딸기', 3000)]
```




### 인덱스 재생성 
reindex()로 인덱스를 재생성하는데 순서는 변경이 되지만, 없는 인덱스는 값이 NaN으로 설정됩니다.


```python
s2 = s.reindex([ "사과","딸기" ,"고양이", "배"])   # 인덱스를 변경한다. 
print(s2) 
```
```
사과     1000.0
딸기     3000.0
고양이       NaN
배      2000.0
dtype: float64
```
fillna()을 사용하여 값을 채울 수 있습니다.
```python
s2 = s.reindex([ "사과","딸기" ,"고양이", "배"])   # 인덱스를 변경한다. 
s3 = s2.fillna(0)
print(s3)
```
```
사과     1000.0
딸기     3000.0
고양이       0.0
배      2000.0
dtype: float64
```
fill_value 옵션을 사용할 수도 있습니다.
```python 
s2 = s.reindex([ "사과","딸기" ,"고양이", "배"], fill_value =0)   # 인덱스를 변경한다. 
print(s2)
```
```
사과     1000
딸기     3000
고양이       0
배      2000
dtype: int64
```

### 값확인
```python
 s2.values  # 시리즈의 값을 배열로 반환 
 ```
 ```
 array([1000, 3000,    0, 2000])
 ```

 ## 인덱싱(Indexing)과 슬라이싱(Slicing)
 ```python 
 idx = ["사과","배","딸기"]
data = [ 1000,  2000, 3000 ]
s = pd.Series(data=data, index=idx)   # 키워드를 사용하여 시리즈를 생성
print(s)
```
```
사과    1000
배     2000
딸기    3000
dtype: int64
```
### 인덱싱
키 값을 사용해도 되고 정수 인덱스를 사용해도 됩니다. 

```python
# 키값은 정수가 아니지만 정수를 사용하여 값을 꺼낼 수 있음
print(s[0])  # 1000
print(s["사과"])  # 키를 이용해서 꺼낼 수 있음 
```
```
1000
1000
```

### 슬라이싱
```python
# 슬라이싱 
print(s[0:2])  # 암묵적 슬라이싱, 정수 인덱스 활용(마지막 '딸기' 미포함)

print("----------------------\n")
# 마스킹. 값에 조건을 걸어 구한다. 
print(s[s > 1000])

print("----------------------\n")

# 팬시 인덱싱 
print(s[ ['사과', '배' ]])
```
```
사과    1000
배     2000
dtype: int64
----------------------

배     2000
딸기    3000
dtype: int64
----------------------

사과    1000
배     2000
dtype: int64
```
### iloc vs loc
명시 슬라이싱과 암묵 슬라이싱때문에 혼동이 발생할 수 있기 때문에 loc, iloc를 주로 사용합니다. 
#### loc
loc는 레이블을 사용하여 값 그룹에 엑세스합니다. loc는 DataFrame에서 설명합니다.


#### iloc
iloc는 정수 및 위치별로 행 및 열 그룹에 엑세스합니다. iloc는 loc와 마찬가지로 뒤에 대괄호를 붙여 사용합니다. 

```python 
# Series.iloc[index_int]

d = s.iloc[0]  # 시리즈에서 요소 선택, 첫번째 요소 
print(d)
d = s.iloc[[0,1]] # 첫번째 두번째 요소 선택
print(d)
d = s.iloc[0:2] # 두번째 요소까지 선택
print(d)
```
```
1000
사과    1000
배     2000
dtype: int64
사과    1000
배     2000
dtype: int64
```

## 행갯수 세기
len()으로 행갯수 세기
```python
print(len(s))
```
```
3
```
size로 갯수 세기
```python
print(s.size)
```
```
3
```
index의 갯수로 세기
```python
print(len(s.index))
```
```
3
```
Null이 아닌 행갯수 세기
```python
print(s.count())
```
```
3
```
