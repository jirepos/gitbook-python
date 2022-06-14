# apply를 이용한 컬럼값 변경 

apply() 함수를 이용하여 열(컬럼)의 값을 변경하는 것을 알아본다. 


```python 
import pandas as pd 
```

DataFrame을 만든다. 

```python
import pandas as pd
import numpy as np

df = pd.DataFrame([[1,2,3], [4,5,6], [7,8,9]], columns=['A','B','C'])
print (df)
```

```
   A  B  C
0  1  2  3
1  4  5  6
2  7  8  9
```

사용자 함수를 만들고 appply(함수명)을 사용하여 함수를 적용한다.

```python
def add_2(x):
    return x+2

# 모든 행열에 사용자 함수가 적용
df = df.apply(add_2)
print(df)
```
```
   A   B   C
0  3   4   5
1  6   7   8
2  9  10  11
```
모든 행렬의 데이터에 사용자 함수가 적용되어 값이 변경된 것을 확인할 수 있다.


## 특정 컬럼에 apply 적용
'A' 컬럼의 값들만 함수를 적용하는 것을 알아본다.

```python
df = pd.DataFrame([[1,2,3], [4,5,6], [7,8,9]], columns=['A','B','C'])
print (df)

# 사용자 함수 
def add_2(x):
    return x+2

print("-----")
df['A'] = df['A'].apply(add_2) # A 컬럼에 함수 적용하여 반환된 값을 A 컬럼에 적용
print (df)
```
```
   A  B  C
0  1  2  3
1  4  5  6
2  7  8  9
-----
   A  B  C
0  3  2  3
1  6  5  6
2  9  8  9
```


## 여러 컬럼에 함수 적용
다음과 같이 여러 컬럼에 함수를 적용할 수도 있다.


```python 
import numpy as np

df = pd.DataFrame([
                    [5,6,7,8],
                    [1,9,12,14],
                    [4,8,10,6]
                    ],
                  columns = ['a','b','c','d'])

print("The original dataframe:")
print(df)

def func(x):
    return x[0] + x[1]

df['e']  = df.apply(func, axis = 1)

print("The new dataframe:")
print(df)
```



