# column 다루기

```python 
import pandas as pd
import numpy as np; 
```

컬럼렴을 설정하지 않으면 자동으로 0부터 시작되는 컬럼명이 부여된다. 
```python 
np.random.seed(1234)
matrix = np.random.randint(10, 100, size=(5,3))
df = pd.DataFrame(matrix)
df
```
```
0	1	2
0	57	93	48
1	63	86	34
2	25	59	33
3	36	40	53
4	40	36	68
```
## Column 이름 바꾸기
columns 속성을 이용하여 한 번에 컬럼명을 설정할 수 있다. 

```python 
df.columns = [ 'A1', 'A2', 'A3']
df
```
```
A1	A2	A3
0	57	93	48
1	63	86	34
2	25	59	33
3	36	40	53
4	40	36	68
```

```python
df.columns
```
```
Index(['A1', 'A2', 'A3'], dtype='object')
```

```python 
type(df.columns)
```
```
pandas.core.indexes.base.Index
```
## 특정 컬럼의 이름 바꾸기
```python 
df.rename( columns={  'A1': '영어', 'A2': '수학'}, inplace=True)
df
```
```
영어	수학	A3
0	57	93	48
1	63	86	34
2	25	59	33
3	36	40	53
4	40	36	68
```

## 컬럼 이름 변경
### 특정 문자 변경

```python 
df.columns = [ '영어 점수', '국어 점수', '수학 점수']
df.columns = df.columns.str.replace(" ", "_")
df
```
```
	영어_점수	국어_점수	수학_점수
0	57	93	48
1	63	86	34
2	25	59	33
3	36	40	53
4	40	36	68
```

