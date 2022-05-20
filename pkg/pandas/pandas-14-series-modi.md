# Series 값 변경

```python 
import pandas as pd 
```
```python 
s = pd.Series([0,1,2,3,4,5])
s
```
```
0    0
1    1
2    2
3    3
4    4
5    5
dtype: int64
```


## 직접 변경
```python 
s[0] = 3
s
```
```
0    3
1    1
2    2
3    3
4    4
5    5
dtype: int64
```
## replace 사용
첫번째 인자는 인덱스가 아니다. 시리즈의 값이다. 
```python 
s.replace(1,999)
```
```
0      3
1    999
2      2
3      3
4      4
5      5
dtype: int64
```

inplace=True하기 전까지는 변경 값이 적용되지 않는다
```python 
s
```
```
0    3
1    1
2    2
3    3
4    4
5    5
dtype: int64
```
```python
s.replace(1, 888, inplace=True)
s
```
```
0      3
1    888
2      2
3      3
4      4
5      5
dtype: int64
```

```
s.replace(0, 777, inplace=True)    # 값에 0 이 없으므로 변경되는 것이 없다.
s
```
```
0      3
1    888
2      2
3      3
4      4
5      5
dtype: int64
```
## DataFrame 사용
```python 
df = pd.DataFrame({'A': [0, 1, 2, 3, 4],
                   'B': [5, 6, 7, 8, 9],
                   'C': ['a', 'b', 'c', 'd', 'e']})
df
```
```
A	B	C
0	0	5	a
1	1	6	b
2	2	7	c
3	3	8	d
4	4	9	e
```
```python 
df.replace(0, 5)
```
```
	A	B	C
0	5	5	a
1	1	6	b
2	2	7	c
3	3	8	d
4	4	9	e
```
### 특정 컬럼만 변경
```python 
df = pd.DataFrame({'A': [0, 1, 2, 3, 4],
                   'B': [5, 6, 7, 8, 9],
                   'C': ['a', 'b', 'c', 'd', 'e']})
df
```
```
	A	B	C
0	0	5	a
1	1	6	b
2	2	7	c
3	3	8	d
4	4	9	e
```
```python 
df.replace({'A': 0, 'B': 5}, 100)
```
```
A	B	C
0	100	100	a
1	1	6	b
2	2	7	c
3	3	8	d
4	4	9	e
```



## 정규식 사용
```python 
df.replace({'c': r'^%.$'}, { 'd': r'^%.$' }, regex=True)
```
```
A	B	C
0	0	5	a
1	1	6	b
2	2	7	c
3	3	8	d
4	4	9	e
```

```python 
df = pd.DataFrame({'A': ['bat', 'foo', 'bait'],
                   'B': ['abc', 'bar', 'xyz']})
df
```
```
A	B
0	bat	abc
1	foo	bar
2	bait	xyz
```

```python 
df.replace(to_replace=r'^ba.$', value='new', regex=True)  # ba로 시작하고 한글자 더있는 값을 new로 변경
```
```
A	B
0	new	abc
1	foo	new
2	bait	xyz
```

```python 
df.replace({'A': r'^ba.$'}, {'A': 'new'}, regex=True)  # ba로 시작하고 한글자 더 있는 것을 new로 변경
```

```
	A	B
0	new	abc
1	foo	bar
2	bait	xyz
```
```python 
df.replace(regex=r'^ba.$', value='new')  # ba로 시작하고 한글자 더 있는 값을 new로 변경
```
```
	A	B
0	new	abc
1	foo	new
2	bait	xyz
```


```python 
df.replace(regex={r'^ba.$': 'new', 'foo': 'xyz'})  # ba로 시작하는 것은 new로, foo는 xyz로 변경
```
```
	A	B
0	new	abc
1	xyz	new
2	bait	xyz
```
```python 
df = pd.DataFrame({
    "c": [ "100%", "30%", "40%"],
    "d": [ "80%", "65%", "41%"]
})
df
```
```
	c	d
0	100%	80%
1	30%	65%
2	40%	41%
```
```python 
df.replace(regex={"%": ""})  # %는 제거
```
```
c	d
0	100	80
1	30	65
2	40	41
```