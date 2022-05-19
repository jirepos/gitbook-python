# 그룹함수

## groupby()
```python 
import numpy as np 
import pandas as pd

df = pd.DataFrame( { 
    'key': ['a', 'b', 'c', 'a', 'b', 'c'],
    'score1': [ 10,20,30, 10,20,30], 
    'score2': [ 5,5,5,4,4,4]
})

df
```
```
key	score1	score2
0	a	10	5
1	b	20	5
2	c	30	5
3	a	10	4
4	b	20	4
5	c	30	4
```




groupby 함수의 반환 값은 GroupBy 클래스 객체이다. 이 객체를 이용해서 다양한 요약정보를 확인할 수 있다.
```python 
grouped = df.groupby('key')
grouped  # DataFrameGroupBy 라는 객체 
```
```
<pandas.core.groupby.generic.DataFrameGroupBy object at 0x7fb650feb510>
```

집단별 크기는 grouped.size(), 집단별 합계는 grouped.sum(), 집단별 평균은 grouped.mean() 을 사용한다.

```python 
grouped.size()
```
```
key
a    2
b    2
c    2
dtype: int64
```



DataFrameGroupBy object는 Iterable하다. 
```python 
for name, group in grouped:
    print(name)
    display(group)
```
```
a
key	score1	score2
0	a	10	5
3	a	10	4
b
key	score1	score2
1	b	20	5
4	b	20	4
c
key	score1	score2
2	c	30	5
5	c	30	4
```

```python 
# 각 그룹별 head 확인
grouped.head(3)
```
```
key	score1	score2
0	a	10	5
1	b	20	5
2	c	30	5
3	a	10	4
4	b	20	4
5	c	30	4
```


```python
grouped.mean()
```
```
score1	score2
key		
a	10.0	4.5
b	20.0	4.5
c	30.0	4.5
```


```python 
# key 값으로 그룹핑하여 합계구하기
df.groupby('key').sum()
```
```
score1	score2
key		
a	20	9
b	40	9
c	60	9
```


복수 컬럼을 groupby할 수 있다.
```python 
# key와 score1으로 그룹핑하여 합하기 
df.groupby(['key', 'score1']).sum()
```
```

          score2
key	score1	
a	10	9
b	20	9
c	30	9
```

```python 
df.groupby('key')['score1'].min()
```
```
key
a    10
b    20
c    30
Name: score1, dtype: int64
```


## aggregate()
그룹으로 묶은 결과의 요약 통계를 구할 수 있다.

```python 
# key로 그룹핑하여 최소값과 최대값을 구한다.
df.groupby('key').aggregate([ min, np.median, max])
```
```
	score1	score2
min	median	max	min	median	max
key						
a	10	10.0	10	4	4.5	5
b	20	20.0	20	4	4.5	5
c	30	30.0	30	4	4.5	5
```



```python 
# score1은 min 값을 찾고, score2는 np.sum으로 합계를 구한다. 
df.groupby('key').aggregate({ 'score1': 'min', 'score2': np.sum })
```
```
score1	score2
key		
a	10	9
b	20	9
c	30	9
```


```python 
df.groupby('key').agg({ 'score1': 'min', 'score2': np.sum })
```
```
	score1	score2
key		
a	10	9
b	20	9
c	30	9
```

### 사용자 함수
```python 
# Input: Series, Output: scalar 인 함수를 정의
def my_agg_func(s):
    return s.max()
```
```python 
df.groupby('key').aggregate({ 'score1': my_agg_func })
```
```
	score1
key	
a	10
b	20
c	30
```




```python 
# lambda로 변경 
df.groupby('key').aggregate({ 'score1': (lambda s: s.max()) })
```
```
score1
key	
a	10
b	20
c	30
```

함수에 추가 인수도 가능하다.
```python 
# Input: Series, Output: scalar 인 함수를 정의
def my_agg_func2(s, p1, p2):
    return s.between(p1, p2).max()
```
```python 
df.groupby('key').aggregate({ 'score1':  (lambda s:  my_agg_func2(s, 10,20) )  })
```
```
score1
key	
a	True
b	True
c	False
```


## filter()
그룹의 속성을 기준으로 데이터를 필터링할 때 사용한다.
```python 
df = pd.DataFrame( { 
    'key': ['a', 'b', 'c', 'a', 'b', 'c'],
    'score1': [ 10,20,30, 10,20,30], 
    'score2': [ 5,5,5,4,4,4]
})
```



## 예제
이 예제는 [Pandas Gropuby Examples](https://www.machinelearningplus.com/pandas/pandas-groupby-examples/)에서 옮긴 것입니다. 

### pandas.DataFrame.groupby
**Syntax**     
```
DataFrame.groupby(by=None, axis=0, level=None, as_index=True, sort=True,group_keys=True, observed=False, dropna=True)
```

* **매개변수**:
  * **by**: 매핑 또는 함수 또는 레이블 또는 레이블 목록(기본값:None) . 우리는 이것을 유사한 특성을 가진 그룹을 결정하는 데 사용합니다.
  * **axis(축)**: 0 또는 1(기본값: 0) . 이것을 사용하여 DataFrame이 분할될 방향을 지정할 수 있습니다.
  * **level(수준)**: Int 또는 문자열 또는 배열(기본값:None) . DataFrame에 다중 레벨 인덱스가 있는 경우 레벨 이름을 지정하는 데 사용됩니다.
  * **as_index**: 부울(기본값: True) . 출력 데이터 구조의 인덱스가 그룹 레이블과 같아야 하는지 여부를 지정하는 데 사용됩니다. 이 매개변수는 DataFrame 입력에만 관련됩니다.
  * **sort(정렬)**: 부울(기본값: True) . 그룹 키에 따라 출력을 정렬할지 여부를 지정하는 데 사용됩니다. 각 그룹 내 관측치의 순서에는 영향을 미치지 않습니다.
  * **group_keys**: 부울(기본값: True) . 그룹 키를 추가할지 여부를 결정하는 데 사용할 수 있습니다.
  * **observed(관찰됨)**: 부울(기본값: False) . 범주 그룹만 표시할지 여부를 지정하는 데 사용됩니다. 이 매개변수는 범주형 그룹이 하나 이상 있는 경우에만 적용할 수 있습니다.
  * **dropna**: 부울(기본값:True) . 출력을 반환하기 전에 그룹 키에서 NA 값을 삭제할지 여부를 지정하는 데 사용됩니다. 기본적으로 그룹을 만드는 동안 삭제됩니다.
* **반환값**: 팬더로 구성된 그룹의 세부 정보를 포함하는 groupby 객체.

```python 
# Create the data of the DataFrame as a dictionary
data_df = {'Name': ['Asha', 'Harsh', 'Sourav', 'Riya', 'Hritik',
                    'Shivansh', 'Rohan', 'Akash', 'Soumya', 'Kartik'],

           'Department': ['Administration', 'Marketing', 'Technical', 'Technical', 'Marketing',
                          'Administration', 'Technical', 'Marketing', 'Technical', 'Administration'],

           'Employment Type': ['Full-time Employee', 'Intern', 'Intern', 'Part-time Employee', 'Part-time Employee',
                               'Full-time Employee', 'Full-time Employee', 'Intern', 'Intern', 'Full-time Employee'],

           'Salary': [120000, 50000, 70000, 70000, 55000,
                      120000, 125000, 60000, 50000, 120000],

           'Years of Experience': [5, 1, 2, 3, 4,
                                   7, 6, 2, 1, 6]}

# Create the DataFrame
df = pd.DataFrame(data_df)
df
```
```	
Name	Department	Employment Type	Salary	Years of Experience
0	Asha	Administration	Full-time Employee	120000	5
1	Harsh	Marketing	Intern	50000	1
2	Sourav	Technical	Intern	70000	2
3	Riya	Technical	Part-time Employee	70000	3
4	Hritik	Marketing	Part-time Employee	55000	4
5	Shivansh	Administration	Full-time Employee	120000	7
6	Rohan	Technical	Full-time Employee	125000	6
7	Akash	Marketing	Intern	60000	2
8	Soumya	Technical	Intern	50000	1
9	Kartik	Administration	Full-time Employee	120000	6
```

이제 groupby아래와 같이 'Department' 유형에 따라 데이터를 그룹화하는 기능을 사용합니다.
```python 
# Use pandas groupby to group rows by department and get only employees of technical department
df_grouped = df.groupby('Department')

df_grouped.get_group('Technical')
```
```
	Name	Department	Employment Type	Salary	Years of Experience
2	Sourav	Technical	Intern	70000	2
3	Riya	Technical	Part-time Employee	70000	3
6	Rohan	Technical	Full-time Employee	125000	6
8	Soumya	Technical	Intern	50000	1
```





다른 부서의 평균 급여를 찾고 그룹화된 df에서 '급여' 열을 가져와 평균을 구한다고 가정해 보겠습니다.
```python 
# Group by department and find average salary of each group
df.groupby('Department')['Salary'].mean()
```
```
Department
Administration    120000.0
Marketing          55000.0
Technical          78750.0
Name: Salary, dtype: float64
```


이것은 함수를 사용하는 일반적인 방법입니다. 이제 가능한 모든 다양한 방법에 대해 자세히 살펴보겠습니다.

### 데이터를 그룹화하는 프로세스
데이터를 그룹화하는 프로세스는 세 단계로 나눌 수 있습니다.
* 분할(Splitting): `groupby`를 수행하려는 열을 식별합니다. 이것은 groupby방법을 사용하여 * 쉽게 수행됩니다.
* 적용(Applying) : 기능을 적용하거나 그룹별로 연산을 수행
* 결합(Combining): 기능을 적용한 후 결과를 하나의 개체에 수집합니다.

### Using a single key of groupby function in pandas
groupby팬더에서 단일 키(여기서 '키'는 데이터 프레임의 열임)를 사용하여 함수를 사용하여 그룹을 형성할 수 있습니다. 키는 매핑, 함수 또는 pandas DataFrame의 열 이름일 수 있습니다.

이 경우 groupby 키는 "Department"라는 열입니다.
```python 
# Separate the rows into groups that have the same department
groups = df.groupby(by='Department')
```

groups여러 방법을 사용 하여 출력의 다양한 측면을 볼 수 있습니다 . groups
메소드를 사용하여 그룹 키 값이 동일한 행의 인덱스 레이블을 볼 수 있습니다.

출력은 사전의 키가 그룹 키이고 각 키의 값이 동일한 그룹 키 값을 갖는 행 인덱스 레이블(row index labels)인 사전이 됩니다.
```python 
# View the indices of the rows which are in the same group
print(groups.groups)
```
```
{'Administration': [0, 5, 9], 'Marketing': [1, 4, 7], 'Technical': [2, 3, 6, 8]}
```


보시다시피 행 인덱스 0, 5 , 9 는 그룹 키 값 'Administration'을 가지므로 함께 그룹화되었습니다.
행 인덱스 1, 4, 7 및 2, 3, 6, 8 은 각각 '마케팅' 및 '기술'이라는 공통 값을 가지므로 함께 그룹화되었습니다.

### Using multiple keys in groupby function in pandas
키 목록을 by 매개변수 groupby에 전달하여 팬더의 기능을 사용하여 팬더에서 그룹을 만드는 데 여러 키를 사용할 수도 있습니다 .

```python 
# Separate the rows into groups that have the same Department and Employment Type
groups = df.groupby(by=['Department', 'Employment Type'])
```

groups 메서드를 사용하여 그룹 을 봅니다

```python 
# View the indices of the rows which are in the same group
groups.groups
```
```
{('Administration', 'Full-time Employee'): [0, 5, 9], ('Marketing', 'Intern'): [1, 7], ('Marketing', 'Part-time Employee'): [4], ('Technical', 'Full-time Employee'): [6], ('Technical', 'Intern'): [2, 8], ('Technical', 'Part-time Employee'): [3]}
```



### Iterating through the groups
'for' 루프를 사용하여 각 그룹의 공통 그룹 키 값과 동일한 그룹의 일부인 pandas DataFrame의 행을 볼 수 있습니다.
```python 
# Separate the rows into groups that have the same department
groups = df.groupby(by='Department')

# View the name and the details of each group
for name, dept in groups:
    print(name)
    print(dept)
    print('\n')
```
```
Administration
       Name      Department     Employment Type  Salary  Years of Experience
0      Asha  Administration  Full-time Employee  120000                    5
5  Shivansh  Administration  Full-time Employee  120000                    7
9    Kartik  Administration  Full-time Employee  120000                    6


Marketing
     Name Department     Employment Type  Salary  Years of Experience
1   Harsh  Marketing              Intern   50000                    1
4  Hritik  Marketing  Part-time Employee   55000                    4
7   Akash  Marketing              Intern   60000                    2


Technical
     Name Department     Employment Type  Salary  Years of Experience
2  Sourav  Technical              Intern   70000                    2
3    Riya  Technical  Part-time Employee   70000                    3
6   Rohan  Technical  Full-time Employee  125000                    6
8  Soumya  Technical              Intern   50000                    1
```



### Selecting a group
특정 그룹의 그룹 키 값을 get_group 메소드 에 전달하여 해당 그룹의 관찰에 액세스할 수 있습니다 .
```python 
# Separate the rows into groups that have the same department
groups = df.groupby('Department')

# View only that group whose group key value is 'Technical'
print(groups.get_group('Technical'))
```
```
     Name Department     Employment Type  Salary  Years of Experience
2  Sourav  Technical              Intern   70000                    2
3    Riya  Technical  Part-time Employee   70000                    3
6   Rohan  Technical  Full-time Employee  125000                    6
8  Soumya  Technical              Intern   50000                    1
```



### Applying functions to a group
그룹에 적용하여 그룹의 통계 요약을 얻거나, 그룹의 관찰을 변환하거나, 특정 기준에 따라 그룹을 필터링할 수 있는 많은 기능이 있습니다.

기능을 크게 세 가지 범주로 분류할 수 있습니다.

* 집계: 이 함수는 그룹에 있는 관찰 패턴이나 추세에 대한 통찰력을 추론하는 데 * 유용할 수 있는 그룹의 다양한 통계 값을 계산하는 데 사용됩니다.
* 변환: 이 기능은 그룹의 관찰을 특정 변경 및 조정하는 데 사용됩니다.
* 여과: 이 기능은 특정 기준에 따라 그룹을 부분 집합화하는 데 사용됩니다.

#### 집계(Aggregation) 
그룹에 적용할 수 있는 몇 가지 집계 함수가 있습니다. 'sum' 은 그룹의 숫자 특성 합계를 구하고 'count' 는 각 그룹의 발생 횟수를 구하거나 'mean' 은 산술 평균을 구합니다. 그룹의 숫자 기능. 집계
방법을 사용하여 집계 함수를 적용합니다.

```python 
# Separate the rows into groups that have the same department
groups = df.groupby(['Department'])

# View the sum of the numeric features of each group
groups.aggregate('sum')
```
```
	Salary	Years of Experience
Department		
Administration	360000	18
Marketing	165000	7
Technical	315000	12
```


먼저 키로 그룹을 만든 다음 그룹의 메서드 에 집계 함수를 전달하여 여러 키에 집계 함수를 적용할 수도 있습니다 .
전달된 키는 출력을 포함하는 데이터 구조의 다중 레벨 색인을 형성합니다

```python 
# Separate the rows into groups that have the same department nad employment type
groups = df.groupby(['Employment Type', 'Department'])

# View the average of the numeric features of each group
groups.aggregate('mean')
```
```
		Salary	Years of Experience
Employment Type	Department		
Full-time Employee	Administration	120000.0	6.0
Technical	125000.0	6.0
Intern	Marketing	55000.0	1.5
Technical	60000.0	1.5
Part-time Employee	Marketing	55000.0	4.0
Technical	70000.0	3.0
```




#### Applying multiple functions
집계 메서드 에 함수 목록을 전달하면 그룹의 여러 통계 값을 한 눈에 볼 수 있습니다.

```python 
# Separate the rows into groups that have the same department
groups = df.groupby('Department')

# View the sum and the average of the numeric features of each group
groups.aggregate(['mean', 'sum'])
```
```
	Salary	Years of Experience
mean	sum	mean	sum
Department				
Administration	120000.0	360000	6.000000	18
Marketing	55000.0	165000	2.333333	7
Technical	78750.0	315000	3.000000	12
```




#### Applying different functions to different keys
모든 키에 동일한 집계 기능을 적용할 필요는 없습니다. 사전을 사용하여 다른 그룹 키에 다른 기능을 적용할 수도 있습니다. 딕셔너리의 키는 그룹 키가 되고 키의 값은 이에 적용되는 기능이 됩니다.

```python 
# Separate the rows into groups that have the same employment type
groups = df.groupby(['Employment Type'])

# Compute the sum on the salary feature and the mean on the Years of Experience feature of the groups
groups.aggregate({'Salary': 'sum', 'Years of Experience': 'mean'})
```
```
Salary	Years of Experience
Employment Type		
Full-time Employee	485000	6.0
Intern	230000	1.5
Part-time Employee	125000	3.5
```



#### Transformation

> deprecated 되었음. 사용하지 말 것. 

변환 함수는 각 그룹의 관찰을 변경하는 데 사용됩니다. 그룹의 관찰을 확장하기 위한 표준화와 같은 중요한 기술을 적용하는 데 사용할 수 있습니다.

변환 방법을 사용하여 변환 기능을 적용합니다.

변환 기능은 다음과 같습니다.

1. 그룹 청크와 동일한 크기의 출력을 반환합니다. 그렇지 않으면 출력을 그룹 청크와 동일한 크기로 브로드캐스트할 수 있습니다.
2. 열 단위로 작동합니다.
3. 내부 작업에는 이것을 사용할 수 없습니다. 형성된 그룹은 변경할 수 없는 것으로 간주되어야 하며 여기에 변형 기능을 적용하면 예기치 않은 결과가 발생할 수 있습니다.

```python 
# Separate the rows into groups that have the same department
groups = df.groupby('Department')

# Define the transformation function to be applied
fun = lambda x: (x - x.mean())/x.std()

# Transform the groups
groups.transform(fun)   # deprecated
```
```
/usr/local/lib/python3.7/dist-packages/ipykernel_launcher.py:8: FutureWarning: Dropping invalid columns in DataFrameGroupBy.transform is deprecated. In a future version, a TypeError will be raised. Before calling .transform, select only columns which should be valid for the transforming function.
```

#### Filtration
데이터 포인트 또는 관찰이 특정 기준을 충족하지 않으면 이를 필터링할 수 있습니다. 필터 메서드를
사용하여 필터링 기능을 적용합니다.

```python 
# Separate the rows of the DataFrame into groups which have the same salary
groups = df.groupby('Salary')

# Filter out the groups whose average salary is less than 100000
groups.filter(lambda x: x['Salary'].mean() > 100000)
```
```
	Name	Department	Employment Type	Salary	Years of Experience
0	Asha	Administration	Full-time Employee	120000	5
5	Shivansh	Administration	Full-time Employee	120000	7
6	Rohan	Technical	Full-time Employee	125000	6
9	Kartik	Administration	Full-time Employee	120000	6
```



#### Other Useful methods and functions that often go with groupby

**The *first* method**     
이 방법을 사용하여 각 그룹의 첫 번째 관찰을 봅니다.


```python 
# Separate the rows into groups that have the same department
groups = df.groupby('Department')

# View the first observation of each group
groups.first()
```
```
	Name	Employment Type	Salary	Years of Experience
Department				
Administration	Asha	Full-time Employee	120000	5
Marketing	Harsh	Intern	50000	1
Technical	Sourav	Intern	70000	2
```




** THe *describe* method**     
이 방법을 사용하여 그룹의 통계 요약을 표시합니다.
pandas DataFrames의 describe 메소드와 유사합니다.

```python
# Separate the rows into groups that have the same department
groups = df.groupby('Department')

# View the statistical summary of the groups
groups.describe()
```
```
	Salary	Years of Experience
count	mean	std	min	25%	50%	75%	max	count	mean	std	min	25%	50%	75%	max
Department																
Administration	3.0	120000.0	0.000000	120000.0	120000.0	120000.0	120000.0	120000.0	3.0	6.000000	1.000000	5.0	5.50	6.0	6.50	7.0
Marketing	3.0	55000.0	5000.000000	50000.0	52500.0	55000.0	57500.0	60000.0	3.0	2.333333	1.527525	1.0	1.50	2.0	3.00	4.0
Technical	4.0	78750.0	32242.570204	50000.0	65000.0	70000.0	83750.0	125000.0	4.0	3.000000	2.160247	1.0	1.75	2.5	3.75	6.0
```


**The *size* method**     
이 방법은 각 그룹에 존재하는 관측치의 수를 보여줍니다.

```python 
# Separate the rows into groups that have the same department
groups = df.groupby('Department')

# View the number of observations present in each group
groups.size()
```
```
Department
Administration    3
Marketing         3
Technical         4
dtype: int64
```


**The *nunique* method**      
이 방법은 그룹의 각 기능에서 고유한 관측값의 수를 보여줍니다

```python
# Separate the rows into groups that have the same department
groups = df.groupby('Department')

# View the number of unique observations in each feature of the groups
groups.nunique()
```
```

Name	Employment Type	Salary	Years of Experience
Department				
Administration	3	1	1	3
Marketing	3	2	3	3
Technical	4	3	3	4
```



**Renaming columns**     
기능을 적용한 후 이름 바꾸기 방법을 사용하여 그룹 기능의 이름 을 더 설명적으로 변경할 수도 있습니다.
이 방법을 사용하려면 키가 원래 열 이름이고 값이 원래 이름을 대체할 새 열 이름인 사전이 필요합니다.
```python 
# Separate the rows into groups that have the same department
groups = df.groupby('Department')

# Change the column name 'Salary' to 'Department Expenditure'
groups.aggregate('sum').rename(columns={'Salary': 'Department Expenditure'})
```
```
	Department Expenditure	Years of Experience
Department		
Administration	360000	18
Marketing	165000	7
Technical	315000	12
```


## References
[Pandas groupby 활용하기](https://sysyn.tistory.com/7)     