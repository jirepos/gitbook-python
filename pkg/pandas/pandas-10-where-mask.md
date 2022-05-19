# where 와 mask

## where

```
DataFrame.where(cond,
                other= NaN,
                inplace= False,
                axis= None,
                level= None,
                errors= 'raise',
                try_cast= False) 
```                
* cond	
부울Series 또는DataFrame, 배열과 유사한 구조 또는 콜 러블입니다. DataFrame의 각 값을 확인하기위한 조건 / 조건을 나타냅니다. 조건이 True이면 원래 값이 대체되지 않습니다. 그렇지 않으면NaN 값으로 대체됩니다.
* other	
스칼라,Series /DataFrame 또는 콜 러블입니다. 조건이 False인 경우 원래 값에 배치 될 값을 나타냅니다.
* inplace	
부울 값입니다. 데이터 작업에 대해 알려줍니다. True이면 스스로 변경합니다.
* axis	
정수 값입니다. 작업 축에 대해 행 또는 열을 알려줍니다.
* level	
정수 값입니다. 레벨에 대해 알려줍니다.
* errors	
문자열입니다. 오류에 대해 알려줍니다. 두 가지 옵션을 허용합니다: raise 또는ignore. 값이 raise이면 예외가 발생할 수 있습니다. 값이 ignore이면 예외를 무시하고 오류가 있으면 원래 객체를 반환합니다.
* try_cast	
부울 값입니다. 가능한 경우 함수의 출력을 원래 입력 유형으로 캐스트합니다.

```python
import pandas as pd
```

### where 사용해보기
```python 
dataframe=pd.DataFrame({
                        'A': 
                            {0: 60, 
                            1: 100, 
                            2: 80,
                            3: 78,
                            4: 95,
                            5: 45,
                            6: 67,
                            7: 12,
                            8: 23,
                            9: 50},
                        'B': 
                            {0: 90, 
                            1: 75, 
                            2: 82, 
                            3: 64, 
                            4: 45,
                            5: 35,
                            6: 74,
                            7: 52,
                            8: 93,
                            9: 18}
                        })

print(dataframe)
```



아래 코드를 실행하면 50보다 크지 않은 값, 즉 조건을 충족하지 않는 값은 NaN값으로 대체됩니다.

```python 
dataframe1 = dataframe.where(dataframe>50)
print(dataframe1)
```
```
    A   B
0   60  90
1  100  75
2   80  82
3   78  64
4   95  45
5   45  35
6   67  74
7   12  52
8   23  93
9   50  18
```




아래는 조건을 충족하지 않는 값은 사용자 정의 값으로 대체됩니다.
```python 
dataframe1 = dataframe.where(dataframe>50, other= 0)
print(dataframe1)
```
```
      A     B
0   60.0  90.0
1  100.0  75.0
2   80.0  82.0
3   78.0  64.0
4   95.0   NaN
5    NaN   NaN
6   67.0  74.0
7    NaN  52.0
8    NaN  93.0
9    NaN   NaN
```


### 단일 조건
C 열의 값이 'a'인 행을 선택한다. 조건과 일치하지 않는 행은 NaN을 표시한다. 
```python 
df = pd.DataFrame({'A': [-20, -10, 0, 10, 20],
                   'B': [1, 2, 3, 4, 5],
                   'C': ['a', 'b', 'b', 'b', 'a']})


print(df.where(df['C'] == 'a'))
```
```
      A    B    C
0 -20.0  1.0    a
1   NaN  NaN  NaN
2   NaN  NaN  NaN
3   NaN  NaN  NaN
4  20.0  5.0    a
```



Nan 대신에 다른 값을 넣고 싶으면 다음과 같이한다. 
```
df.where(조건, NaN표시값)
```
```python 
print(df.where(df['C'] == 'a', 0))
```
```
  A  B  C
0 -20  1  a
1   0  0  0
2   0  0  0
3   0  0  0
4  20  5  a
```

조건과 일치하지 않는 행의 값은 모두 0으로 채워진다. 

### 복수 조건
검색 조건을 복수로 설정하며년 AND 또는 OR를 사용한다. 

```python 
print(df.where((df['C'] == 'a') & (df['A'] == 20)))
```
```
      A    B    C
0   NaN  NaN  NaN
1   NaN  NaN  NaN
2   NaN  NaN  NaN
3   NaN  NaN  NaN
4  20.0  5.0    a
```

& 는 AND 조건이다. 두가지 조건을 만족하는 행을 출력한다. 조건을 만족하지 않는 행은 0으로 NaN으로 채운다.

이번에는 OR 조건을 사용해 보자.
```python 
print(df.where((df['C'] == 'a') | (df['A'] == 10)))
```
```
      A    B    C
0 -20.0  1.0    a
1   NaN  NaN  NaN
2   NaN  NaN  NaN
3  10.0  4.0    b
4  20.0  5.0    a
```

'|'는 OR .조건이다. 둘 중에 하나의 조건을 만족하면 된다. 조건에 만족하지 않는 행은 NaN을 채운다. 

### NOT 조건
일치하지 않는 조건을 검색하고 싶은 경우에는 !=를 사용한다.
```python 
print(df.where(df['C'] != 'a'))
```
```
      A    B    C
0   NaN  NaN  NaN
1 -10.0  2.0    b
2   0.0  3.0    b
3  10.0  4.0    b
4   NaN  NaN  NaN
```


## mask
mask 메서드는 입력변수에 대한 내용은 False일 때 데이터를 선택한다는 것을 제외하고 where 메서드와 동일하고 Series에서의 메서드 적용 방식도 동일하다.
```python 
print(df.mask(df['C'] == 'a', 0))    # where와 반대이므로 a가 아닌것
```
```
    A  B  C
0   0  0  0
1 -10  2  b
2   0  3  b
3  10  4  b
4   0  0  0
```
