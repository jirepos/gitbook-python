# 비교
```python 
import numpy as np
import pandas as pd
```

## Series 비교

```python 
s1 = pd.Series(["a", "b", "c", "d", "e"])
s2 = pd.Series(["a", "a", "c", "b", "e"])
```

```python 
s1.compare(s2)
```

```
self	other
1	b	a
3	d	b
```

```python
s1.compare(s2, align_axis=0)
```
``
1  self     b
   other    a
3  self     d
   other    b
dtype: object
```


```python 
s1.compare(s2, keep_shape=True)
```
```
self	other
0	NaN	NaN
1	b	a
2	NaN	NaN
3	d	b
4	NaN	NaN
```


```python 
s1.compare(s2, keep_shape=True, keep_equal=True)
```
```
self	other
0	a	a
1	b	a
2	c	c
3	d	b
4	e	e
```


## DataFrame 비교

* 연산 메서드 
  * eq() == 
  * ne() !=
  * lt() <
  * gt() > 
  * le() <=
  * ge() >=

```python
df = pd.DataFrame([ [1,2,3], [4,5,6], [7,8,9] ] , columns=['a', 'b', 'c'])
df
```
```
	a	b	c
0	1	2	3
1	4	5	6
2	7	8	9
```


###  equal 비교 

# 메서드를 사용한 비교
```python 
df.eq(5)
```
```
a	b	c
0	False	False	False
1	False	True	False
2	False	False	False
```


# 연산자를 사용한 비교
```python 
df == 5 
```
```
a	b	c
0	False	False	False
1	False	True	False
2	False	False	False
```



### DataFrame간의 비교 
```python 
(2 * df) == df + df 
```
```
a	b	c
0	True	True	True
1	True	True	True
2	True	True	True
```
```python 
(2 * df).equals( df + df)
```
```
True
```



## References
[DataFrame비교](https://blog.naver.com/PostView.naver?blogId=youji4ever&logNo=222265612447)     
[객체간의 비교](https://kongdols-room.tistory.com/127)      
