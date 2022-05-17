# DataFrame 



## DataFrame 만들기

다음의 생성자를 사용하여 DataFrame을 생성할 수 있습니다.
```
pandas.DataFrame( data, index, columns, dtype, copy)
```

* data      
ndarray, series, map, lists, dict, constants 와 다른 DataFrame 과 같은 다양한 형식을 취할 수 있습니다.
* index    
row labels을 위한 것입니다.  전달되지 않으면 np.arrange(n)입니다. 
* columns    
column lable입니다. 선택적이고 디폴트는 np.arrange(n)입니다. 
* dtype    
각 컬럼의 데이터 타입입니다. 
* copy    
이 커맨드는 데이터의 카피를 위해 사용됩니다. 디폴트는 False입니다. 



### 빈 DataFrame 만들기

```python
import numpy as np
import pandas as pd
```
```python
df = pd.DataFrame()  # 빈 DataFrame을 만든다.
print(df)
```
```
Empty DataFrame
Columns: []
Index: []
```

### 리스트로부터 만들기
index와 column을 전달하지 않았기 때문에 숫자로 값이 설정되는 것을 볼 수 있습니다.

```python
data = [1,2,3,4,5]
df = pd.DataFrame(data)
print(df)
```
```
   0
0  1
1  2
2  3
3  4
4  5
```
아래의 경우는 문자열과 숫자의 배열을 전달하였다. 컬럼명을 설정해서 문자열은 Name이 되고 숫자는 Age가 됩니다. 
```python
data = [['Alex',10],['Bob',12],['Clarke',13]]
df = pd.DataFrame(data,columns=['Name','Age'])
print(df)
```
```
     Name  Age
0    Alex   10
1     Bob   12
2  Clarke   13
```

### 사전 사용하여 만들기
사전을 이용하여 DataFrame을 만듭니다.  사전의 키는 데이터프레임의 Columm 이름이 됩니다.  특정 열 집합이 데이터 사전과 함께 전달되면 전달된 열이 사전의 키를 재정의합니다.

```python
data = {'Name':['Tom', 'Jack', 'Steve', 'Ricky'],'Age':[28,34,29,42]}
df = pd.DataFrame(data)
print(df)
```
```
    Name  Age
0    Tom   28
1   Jack   34
2  Steve   29
3  Ricky   42
```


아래는 사전을 사용하여 DataFrame을 만드는데 index를 정의하였습니다. 이번에는 숫자가 아닌 전달된 인덱스가 행의 인덱스가 된 것을 볼 수 있습니다.

```python
data = {'Name':['Tom', 'Jack', 'Steve', 'Ricky'],'Age':[28,34,29,42]}
df = pd.DataFrame(data, index=['rank1','rank2','rank3','rank4'])
print(df)
```
```
        Name  Age
rank1    Tom   28
rank2   Jack   34
rank3  Steve   29
rank4  Ricky   42
```


#### 사전들을 담은 리스트
사전들을 담은 리스르로부터 DataFrame을 만듭니다.

```python
data = [{'a': 1, 'b': 2},{'a': 5, 'b': 10, 'c': 20}]
df = pd.DataFrame(data)
print(df)
```
```
   a   b     c
0  1   2   NaN
1  5  10  20.0
```
다음은 인덱스명을 정의합니다.
```python
data = [{'a': 1, 'b': 2},{'a': 5, 'b': 10, 'c': 20}]
df = pd.DataFrame(data, index=['first', 'second'])
print(df)
```
```
        a   b     c
first   1   2   NaN
second  5  10  20.0
```
사전에는 'c'가 있지만 columns에 컬럼을 정의하여 'c'가 없는 경우에는 'c' 열이 생성되지 않습니다.


```python
data = [{'a': 1, 'b': 2},{'a': 5, 'b': 10, 'c': 20}]

#With two column indices, values same as dictionary keys
df1 = pd.DataFrame(data, index=['first', 'second'], columns=['a', 'b'])
print(df1)
```

```
        a   b
first   1   2
second  5  10
```


```python

#With two column indices with one index with other name
df2 = pd.DataFrame(data, index=['first', 'second'], columns=['a', 'b1'])
print(df2)
```
```
        a  b1
first   1 NaN
second  5 NaN
```
colums에 설정한 'b1' 이 사전에는 없기 때문에 b1 열의 데이터들은 모두 NaN입니다.

### Series 사전으로 부터 생성
이번에는 Series로 사전을 정의하여 DataFrame을 생성합니다.

```python
d = {'one' : pd.Series([1, 2, 3], index=['a', 'b', 'c']),
   'two' : pd.Series([1, 2, 3, 4], index=['a', 'b', 'c', 'd'])}

df = pd.DataFrame(d)
print(df)
```
```
   one  two
a  1.0    1
b  2.0    2
c  3.0    3
d  NaN    4
```
```python
d = {
    "one": pd.Series([1.0, 2.0, 3.0], index=["a", "b", "c"]),
    "two": pd.Series([1.0, 2.0, 3.0, 4.0], index=["a", "b", "c", "d"]),
}

df = pd.DataFrame(d)
print(df)  # df 살펴보기 
print(type(df))  # 타입확인
```
```
   one  two
a  1.0  1.0
b  2.0  2.0
c  3.0  3.0
d  NaN  4.0
<class 'pandas.core.frame.DataFrame'>
```
```python
# 리스트를 만든다 
a = [ 1,2,3,5]  
b = [ 6,7,8,9]

# 시리즈를 만든다 
s1 = pd.Series(np.array(a))
print(s1) 
s2 = pd.Series(np.array(b))
print(s2)

# 사전을 만든다 
d = { 
    "a": s1 , 
    "b": s2 
}

# 사전을 인자로 전달하여 DataFrame을 만든다 
df = pd.DataFrame(d)
print(df)  # DataFrame 확인 


print("\n")
# 시리즈의 첫번째 요소값 구하기 
print( "Series 'a'의 첫번째 요소 값:", df["a"][0])
```

```
0    1
1    2
2    3
3    5
dtype: int64
0    6
1    7
2    8
3    9
dtype: int64
   a  b
0  1  6
1  2  7
2  3  8
3  5  9


Series 'a'의 첫번째 요소 값: 1
```

## 데이터 보기
### head() vs tail()
head()와 tail()의 사용법을 살펴보겠습니다. 

```python
d = {
    "one": pd.Series([1.0, 2.0, 3.0], index=["a", "b", "c"]),
    "two": pd.Series([1.0, 2.0, 3.0, 4.0], index=["a", "b", "c", "d"]),
}

df = pd.DataFrame(d)
df.head()   # 데이터프레임의 상단을 볼 수 있다
```
```

one	two
a	1.0	1.0
b	2.0	2.0
c	3.0	3.0
d	NaN	4.0
```
```python
df.head(10)  # 10개 출력
```
```
	one	two
a	1.0	1.0
b	2.0	2.0
c	3.0	3.0
d	NaN	4.0
```
```python
df.tail()  # 하단 행 보기
```
```

one	two
a	1.0	1.0
b	2.0	2.0
c	3.0	3.0
d	NaN	4.0
```
```python
df.tail(10) # 10개 하단 행 보기
```
```

one	two
a	1.0	1.0
b	2.0	2.0
c	3.0	3.0
d	NaN	4.0
```

시리즈를 구하여 시리즈의 인덱스를 이용하여 값을 구해보겠습니다.

```python
s1 = df["one"]   # 시리즈 꺼내고 
print(s1)    # 시리즈 확인
print(type(s1))  # 시리즈 타입 확인
print(s1['a'])   # 인덱스 사용하여 값 꺼내고 
```
```
a    1.0
b    2.0
c    3.0
d    NaN
Name: one, dtype: float64
<class 'pandas.core.series.Series'>
1.0
```


### 인덱스 확인
```python
print(df.index)
```
```
Index(['a', 'b', 'c', 'd'], dtype='object')
```

### 컬럼확인
```python
print(df.columns)
```
```
Index(['one', 'two'], dtype='object')
```

### 열갯수와 행갯수 세기
shape를 통해 dataframe의 row와 column의 수를 알 수 있다.
```python
df.shape
```
```
(4, 2)
```

len으로 행갯수 세기
```python
print(len(df))
```
```
4
```
shape로 행 갯수 세기
```python
print(df.shape[0])
```
```
4
```
index로 행 갯수 세기
```python
print(len(df.index))
```
```
4
```


열 갯수 세기
```python
print(df.shape[1])
print(len(df.columns))
```
```
2
2
```


## row 선택 및 column 선택
* 행번호로 서택 iloc
* label이나 조건표현으로 선택 loc


### row 선택

```python
# row 선택
display(df.iloc[0])   # 첫번째 행 
display(df.iloc[1])   # 두번째 행 
display(df.iloc[-1])  # 마직막 행
```
```
one    1.0
two    1.0
Name: a, dtype: float64
one    2.0
two    2.0
Name: b, dtype: float64
one    NaN
two    4.0
Name: d, dtype: float64
```


### 컬럼 선택
```python
display(df.iloc[:, 0]) # 첫번째 열만
display(df.iloc[:, 1]) # 두번째 열만
display(df.iloc[:, -1]) # 마지막 열만
```
```
a    1.0
b    2.0
c    3.0
d    NaN
Name: one, dtype: float64
a    1.0
b    2.0
c    3.0
d    4.0
Name: two, dtype: float64
a    1.0
b    2.0
c    3.0
d    4.0
Name: two, dtype: float64
```

### 행과 열을 선택
```python
display(df.iloc[0:5]) # 첫 5개 행만
display(df.iloc[:, 0:2])  # 첫 2개 열만
```
```
	one	two
a	1.0	1.0
b	2.0	2.0
c	3.0	3.0
d	NaN	4.0
one	two
a	1.0	1.0
b	2.0	2.0
c	3.0	3.0
d	NaN	4.0
```
```python
d = {
    "one": pd.Series([1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0, 8.0, 9.0, 10.0], index=["a", "b", "c", "d", "e", "f", "g", "h", "i" , "j"]),
    "two": pd.Series([10,9,8,7,6,5,4,3,2,1], index=["a", "b", "c", "d", "e", "f", "g", "h", "i" , "j"]),
    "three": pd.Series([10,9,8,7,6,5,4,3,2,1], index=["a", "b", "c", "d", "e", "f", "g", "h", "i" , "j"]),
    "four": pd.Series([10,9,8,7,6,5,4,3,2,1], index=["a", "b", "c", "d", "e", "f", "g", "h", "i" , "j"]),
    "five": pd.Series([10,9,8,7,6,5,4,3,2,1], index=["a", "b", "c", "d", "e", "f", "g", "h", "i" , "j"]),
    "six": pd.Series([10,9,8,7,6,5,4,3,2,1], index=["a", "b", "c", "d", "e", "f", "g", "h", "i" , "j"]),     
    "seven": pd.Series([10,9,8,7,6,5,4,3,2,1], index=["a", "b", "c", "d", "e", "f", "g", "h", "i" , "j"]),
    "eight": pd.Series([10,9,8,7,6,5,4,3,2,1], index=["a", "b", "c", "d", "e", "f", "g", "h", "i" , "j"]),     
    "nine": pd.Series([10,9,8,7,6,5,4,3,2,1], index=["a", "b", "c", "d", "e", "f", "g", "h", "i" , "j"]),     
    "ten": pd.Series([11,9,8,7,6,5,4,3,2,1], index=["a", "b", "c", "d", "e", "f", "g", "h", "i" , "j"]),          
}

df = pd.DataFrame(d)
```
```python
df.iloc[[0,3,6,9], [0,5,6]] # 1st, 4th, 7th, 10th 행과 + 1st 6th 7th 열만
```
```

one	six	seven
a	1.0	10	10
d	4.0	7	7
g	7.0	4	4
j	10.0	1	1
```

```python
df.iloc[0:5, 5:8] # 첫 5개 행과 5th, 6th, 7th 열만
```
```

six	seven	eight
a	10	10	10
b	9	9	9
c	8	8	8
d	7	7	7
e	6	6	6
```

### label이나 조건표현으로 선택


```python
df.loc["a"]   # "a"행만 반환 series 반환 
```
```
one       1.0
two      10.0
three    10.0
four     10.0
five     10.0
six      10.0
seven    10.0
eight    10.0
nine     10.0
ten      11.0
Name: a, dtype: float64
```
```python
df.loc[['a', 'j']]   # a행과  j행 반환 , DataFrame 반환 
```
```

one	two	three	four	five	six	seven	eight	nine	ten
a	1.0	10	10	10	10	10	10	10	10	11
j	10.0	1	1	1	1	1	1	1	1	1
```

범위표현 사용
```python
df.loc[ [ 'a', 'b'], 'one':'three']   # a행, b행 선택 ,  one에서 three까지 컬럼 범위 선택
```
```
	one	two	three
a	1.0	10	10
b	2.0	9	9
```


```python
d = { 
    "id" : pd.Series( [ 100,101,102,103,104]),
     "salary": pd.Series( [ 1500,1300,1200,1600,1800])
}

df = pd.DataFrame(d)
df
```
```

id	salary
0	100	1500
1	101	1300
2	102	1200
3	103	1600
4	104	1800
```

## row와 column 다루기
```python
df = pd.DataFrame( np.arange(0,9).reshape(3,3))
display(df)
```
```

0	1	2
0	0	1	2
1	3	4	5
2	6	7	8
```

pandas는 기본적으로 row에 인덱스를 0부터 차례대로 자연수를 부여한다.



### index 및 Column 설정
index 속성을 사용하여 index를 설정할 수 있습니다. 

```python
df.index=[ 'r0', 'r1', 'r3']
df
```
```
	0	1	2
r0	0	1	2
r1	3	4	5
r3	6	7	8
```
columns 속성을 사용하여 컬럼을 설정할 수 있습니다.

```python
df.columns = [ 'one', 'two', 'three']
df
```
```
	one	two	three
r0	0	1	2
r1	3	4	5
r3	6	7	8
```

#### 열(column)을 이용한 index 설정
다음과 같이 DataFrame을 생성합니다.
```python
df = pd.DataFrame( [ [ 0, 1, 2, 'r0'], [ 3,4,5, 'r1'], [6,7,8, 'r2']  ], columns=[ 'c0', 'c1', 'c2', 'c4'] )
df
```
```

c0	c1	c2	c4
0	0	1	2	r0
1	3	4	5	r1
2	6	7	8	r2
```


'c4' 컬럼을 사용하여 index를 설정합니다.  'c4'는 index이름이 됩니다.

```python
df.set_index('c4')
```

```
	c0	c1	c2
c4			
r0	0	1	2
r1	3	4	5
r2	6	7	8
```


set_index 구문은 다음과 같습니다. 
```
DataFrame.set_index(keys, drop=True, append=False, inplace=False)
```

* drop 인덱스로 설정한 열을 DataFrame에서 삭제할지 여부 
* append 기존에 존재하던 인덱스를 삭제할지 여부 
* inplace 원본 객체를 변경할지 여부


#### index 이름 설정
```python
df = pd.DataFrame(  [ [100, 200], [200,300] ], columns=['math', 'eng'])
df    
```
```

math	eng
0	100	200
1	200	300
```
```python
df.index = ["홍길동", "박찬호"]  # 인덱스 설정
df
```

```
	math	eng
홍길동	100	200
박찬호	200	300
```

생성한 인덱스에 이름을 설정합니다.


```python
df.index.name = "name"  # 인덱스 이름 설정
df
```
```

        math	eng
name		
홍길동	100	200
박찬호	200	300
```


### index 삭제
인덱스를 지워야할 경우는 그렇게 많지 않을 것입니다. 주로 reset_index를 이용해서 index를 리셋하는 것을 많이 사용합니다.혹은 index의 이름을 삭제하고 싶다면 del df.index.name을 통해 인덱스의 이름을 삭제할 수 있습니다.

### 행의 컬럼값들 변경
다음은 인덱스 2.5를 [10,10,10] 리스트로 바꿉니다.


```python
df.loc[2.5] = [ 10,10, 10]
display(df)
```
```

    48	49	50
2.5	10	10	10
12.6	4	5	6
4.8	7	8	9
2.0	60	50	40
```
### row 추가
#### loc 사용

```python
df = pd.DataFrame(data=np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]]), index= [2.5, 12.6, 4.8], columns=[48, 49, 50])
display(df)
```
```

48	49	50
2.5	1	2	3
12.6	4	5	6
4.8	7	8	9
```
```
df.loc[2] = [60, 50, 40]
display(df)
```
```

48	49	50
2.5	1	2	3
12.6	4	5	6
4.8	7	8	9
2.0	60	50	40
```
loc을 사용하면 새로운 index=2인 row를 만들고, 그 곳에 row를 추가합니다.





#### append를 이용한 row 추가
때론 인덱스를 신경쓰지 않고 그냥 데이터의 가장 뒤에 row를 추가하고 싶을 수도 있습니다. 이 경우에는 append를 사용하면 좋습니다. 아래는 df 데이터프레임에 a를 추가하여 row를 추가하는 코드를 보여줍니다. 



```python
df = pd.DataFrame(data=np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]]), columns=[48, 49, 50])
display(df)
```
```
48	49	50
0	1	2	3
1	4	5	6
2	7	8	9
```
```python
# 추가할 데이터프레임 생성
a = pd.DataFrame(data=[[1,2,3]], columns=[48,49,50])
display(a)
```
```
48	49	50
0	1	2	3
```
```python
df = df.append(a) # append로 행 덧붙인다
display(df)
```
```

48	49	50
0	1	2	3
1	4	5	6
2	7	8	9
0	1	2	3
```

row를 추가한 후에 reset index를 통해 index를 0부터 새롭게 지정합니다. 

```python
df = df.reset_index(drop=True)
display(df)
```
```

48	49	50
0	1	2	3
1	4	5	6
2	7	8	9
3	1	2	3
```

### row 삭제

#### 중복 row 삭제
drop_duplicate를 사용하면 특정 컬럼의 값이 중복된 로우를 제거할 수 있습니다.. keep 키워드를 통해 중복된 것들 중 어떤 걸 킵할지 정할 수 있습니다.
```python
df = pd.DataFrame(data=np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9], [40, 50, 60], [23, 35, 37]]), 
                  index= [2.5, 12.6, 4.8, 4.8, 2.5], 
                  columns=[48, 49, 50])

display(df)
```

```
	48	49	50
2.5	1	2	3
12.6	4	5	6
4.8	7	8	9
4.8	40	50	60
2.5	23	35	37
```
```python
df = df.reset_index()
display(df)
```
```

index	48	49	50
0	2.5	1	2	3
1	12.6	4	5	6
2	4.8	7	8	9
3	4.8	40	50	60
4	2.5	23	35	37
```
```python
df = df.drop_duplicates(subset='index', keep='last').set_index('index')
display(df)
```
```

48	49	50
index			
12.6	4	5	6
4.8	40	50	60
2.5	23	35	37
```


#### index를 통한 row 삭제 
drop 명령어를 통해 특정 index를 가진 row를 삭제할 수 있습니다. df.index[1] 명령어는 1번 째 위치에 있는 index를 가져옵니다. 가져온 이 index를 drop에 인풋으로 넣어주면 해당 index를 가진 row를 삭제할 수 있습니다.



```python
# Check out your DataFrame `df`
df = pd.DataFrame(data=np.array([[1, 2, 3], [1, 5, 6], [7, 8, 9]]), columns=['A', 'B', 'C'])
display(df)
```
```

A	B	C
0	1	2	3
1	1	5	6
2	7	8	9
```
```python
# df.index[1] 명령어는 1번 째 위치에 있는 index를 가져온다
print(df.index[1])
```
```
1
```
```python
#  df.index[1] 명령어는 1번 째 위치에 있는 index를 가져온다. 가져온 이 index를 drop에 인풋으로 넣어주면 해당 index를 가진 row를 삭제할 수 있다.
print(df.drop(df.index[1]))
```
```
   A  B  C
0  1  2  3
2  7  8  9
```

```python
print(df.drop(0))
```
```
  A  B  C
1  1  5  6
2  7  8  9
```
```python
df2 = df.drop(0)
print(df2)
```
```
   A  B  C
1  1  5  6
2  7  8  9
```


## 데이터 수정하기

```python
df = pd.DataFrame(data=np.array([[1, 2, 3], [1, 5, 6], [7, 8, 9]]), columns=['A', 'B', 'C'])
display(df)
```
```

A	B	C
0	1	2	3
1	1	5	6
2	7	8	9
```
```python
df.loc[0]['A'] = 9999
display(df)
```
```
A	B	C
0	9999	2	3
1	1	5	6
2	7	8	9
```

## References
[Python Pandas - DataFrame](https://www.tutorialspoint.com/python_pandas/python_pandas_dataframe.htm)   
[iloc, loc를 사용한 행/열 선택](https://azanewta.tistory.com/34)   

https://kongdols-room.tistory.com/123





