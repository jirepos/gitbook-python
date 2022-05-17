# DataFrame 합치기

* merge
* join
* concat


```python
import numpy as np
import pandas as pd
```

## concat
defalut값으로 axis=0이 적용되기 때문에 행방향(위아래)으로 데이터프레임을 이어붙입니다.

```python
df1 = pd.DataFrame({'a':['a0','a1','a2','a3'],
                    'b':['b0','b1','b2','b3'],
                    'c':['c0','c1','c2','c3']},
                  index = [0,1,2,3])

df2 = pd.DataFrame({'a':['a2','a3','a4','a5'],
                    'b':['b2','b3','b4','b5'],
                    'c':['c2','c3','c4','c5'],
                    'd':['d2','d3','d4','d5']},
                   index = [2,3,4,5])
```
```
df3 = pd.concat([df1,df2])
print(df3)
```
```
   a   b   c    d
0  a0  b0  c0  NaN
1  a1  b1  c1  NaN
2  a2  b2  c2  NaN
3  a3  b3  c3  NaN
2  a2  b2  c2   d2
3  a3  b3  c3   d3
4  a4  b4  c4   d4
5  a5  b5  c5   d5
```

ignore_index=True을 줘서 인덱스를 재배열 할 수 있습니다.
```python
df3 = pd.concat([df1,df2], ignore_index=True)
print(df3)
```
```
    a   b   c    d
0  a0  b0  c0  NaN
1  a1  b1  c1  NaN
2  a2  b2  c2  NaN
3  a3  b3  c3  NaN
4  a2  b2  c2   d2
5  a3  b3  c3   d3
6  a4  b4  c4   d4
7  a5  b5  c5   d5
```
이번에는 열방향axis=1(좌우)으로 이어붙입니다.

```python
df3 = pd.concat([df1,df2],axis=1)
print(df3)
```


```
     a    b    c    a    b    c    d
0   a0   b0   c0  NaN  NaN  NaN  NaN
1   a1   b1   c1  NaN  NaN  NaN  NaN
2   a2   b2   c2   a2   b2   c2   d2
3   a3   b3   c3   a3   b3   c3   d3
4  NaN  NaN  NaN   a4   b4   c4   d4
5  NaN  NaN  NaN   a5   b5   c5   d5
```


pd.concat()함수는 또한 default로 outer를 가집니다. 이어붙이는 방식을 outer는 합집합, inner는 교집합을 의미합니다. 

그러면 이번에는 inner옵션을 줘서 이어붙일 두 데이터에 모두 존재하는 행인덱스만 가져와 보겠습니다. 

```python
df3 = pd.concat([df1,df2],axis=1, join='inner')   #열방향(axis=1), 교집합(inner)
print(df3)
```
```
    a   b   c   a   b   c   d
2  a2  b2  c2  a2  b2  c2  d2
3  a3  b3  c3  a3  b3  c3  d3
```



## Series를 DataFrame에 합치기

```python
s1 = pd.Series(['e0','e1','e2','e3'], name = 'e')
s2 = pd.Series(['f0','f1','f2'], name = 'f', index = [3,4,5])
s3 = pd.Series(['g0','g1','g2','g3'], name = 'g')
```
```python
r= pd.concat([df1,s1], axis=1)
print(r, '\n')

r = pd.concat([df2,s2], axis=1)
print(r, '\n')
```
```
   key   vals   e
0   kim  100.0  e0
1  park  200.0  e1
2   joe  300.0  e2
3   NaN    NaN  e3 

    key   vals    f
0   kim  100.0  NaN
1  park  200.0  NaN
2   joe  300.0  NaN
3   NaN    NaN   f0
4   NaN    NaN   f1
5   NaN    NaN   f2 
```

### 시리즈끼리 붙이기

```python
r = pd.concat([s1, s3], axis = 1)  #열방향 연결, 데이터프레임
print(r)
print(type(r), '\n')
```
```python 
r = pd.concat([s1, s3], axis = 0)  #행방향 연결, 시리즈
print(r)
print(type(r), '\n') 
```



## merge
merge()함수는 두 데이터프레임을 각 데이터에 존재하는 고유값(key)을 기준으로 병합할때 사용합니다. 
```
pd.merge(df_left, df_right, how='inner', on=None)
```

DataFrame1을 다음과 같이 만듭니다.


```python
d1 = {   "key": [ "kim", "park", "joe"]
        ,"data1" : [ 100, 200, 300]
       }
df1 = pd.DataFrame(d1)
df1
```
```

key	data1
0	kim	100
1	park	200
2	joe	300
```



DataFrame2를 다음과 같이 만듭니다.

```python
d2 = { "key": [ "kim", "yo", "woo"]
      ,"data2": [ 400, 500, 600]
      }
df2 = pd.DataFrame(d2)
df2
```
```
	key	data2
0	kim	400
1	yo	500
2	woo	600
```

### inner

merge 함수의 디폴트 옵션은 공통 열이름을 기준으로 inner join을 수행합니다.  inner join으로 공통된 열이름(key)를 기준으로 data2가 합쳐지고 공통되지 않은 키 yo와 woo는 생략됩니다. 

```python
df3 = pd.merge(df1, df2)
df3
```
```

  key	data1	data2
0	kim	100	400
```
공통된 키가 kim 밖에 없으므로  kim row만 선택되고 kim의 data1 값과 data2 값이 병합된 것을 볼 수 있습니다.



### outer
how 옵션에 outer를 사용하면 key가 일치하지 않더라도 병합합니다.

```python
df3 = pd.merge(df1, df2, on='key', how='outer')
df3
```
```

key	data1	data2
0	kim	100.0	400.0
1	park	200.0	NaN
2	joe	300.0	NaN
3	yo	NaN	500.0
4	woo	NaN	600.0
```

### left
왼쪽 키를 기준으로 병합합니다. 왼쪽 키에 없는 것은 병합되지 않습니다.

```python
df3 = pd.merge(df1, df2, on='key', how='left')
df3
```
```
	key	data1	data2
0	kim	100	400.0
1	park	200	NaN
2	joe	300	NaN
```

### right
오른쪽 키를 기준으로 병합합니다. 오른쪽 키에 없는 것은 병합되지 않습니다.
```python
df3 = pd.merge(df1, df2, on='key', how='right')
df3
```
```
	key	data1	data2
0	kim	100.0	400
1	yo	NaN	500
2	woo	NaN	600
```

### left_on, right_on
컬럼명이 다르지만 left_on과 right_on을 사용하여 컬럼명을 지정합니다. 키 값이 같은 것만 출력합니다.


```python
d1 = {   "lkey": [ "kim", "park", "joe"]
        ,"data1" : [ 100, 200, 300]
       }
df1 = pd.DataFrame(d1)

d2 = { "rkey": [ "kim", "yo", "woo"]
      ,"data2": [ 400, 500, 600]
      }
df2 = pd.DataFrame(d2)

df3 = pd.merge(df1, df2,left_on="lkey", right_on="rkey" )  # default는 inner join
df3
```
```
	lkey	data1	rkey	data2
0	kim	100	kim	400
```


## join
join 함수는 merge() 함수를 기반으로 만들어졌으며 작동방식이 비슷하지만 행 인덱스를 기준으로 데이터프레임을 병합한다는 점에서 차이가 있습니다.


```python
d1 = {   "lkey": [ "kim", "park", "joe"]
        ,"data1" : [ 100, 200, 300]
       }
df1 = pd.DataFrame(d1)
print(df1) 

d2 = { "rkey": [ "kim", "yo", "woo"]
      ,"data2": [ 400, 500, 600]
      }
df2 = pd.DataFrame(d2)
print(df2) 

```

```
   lkey  data1
0   kim    100
1  park    200
2   joe    300
  rkey  data2
0  kim    400
1   yo    500
2  woo    600
lkey	data1	rkey	data2
0	kim	100	kim	400
1	park	200	yo	500
2	joe	300	woo	600
```

## References

[DataFrame 합치기](https://yganalyst.github.io/data_handling/Pd_12/)     
[DataFrame 병합](https://kimdingko-world.tistory.com/207)     
