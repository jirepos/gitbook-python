# DataFrame 데이터 추출


```python
import numpy as np
import pandas as pd
```
```python
d = { 
    "country": [ '한국','영국', '미국'],
    "gdp": [ 100, 200, 300],
    "capital" : ['서울', '런던', '워싱턴']
}
df = pd.DataFrame(d)
df
```
```
country	gdp	capital
0	한국	100	서울
1	영국	200	런던
2	미국	300	워싱턴
```

```python
r = df['country']   # Series를 반환 
print(type(r))   # type이 Series인 것을 확인 
print(r)
```
```
<class 'pandas.core.series.Series'>
0    한국
1    영국
2    미국
Name: country, dtype: object
```
```python
r = df[ [ 'country' ]]  # DataFrame 반환 
print(type(r))  #  DataFrame인 것을 확인할 수 있음
r
```
```
<class 'pandas.core.frame.DataFrame'>
country
0	한국
1	영국
2	미국
```
```python
# 조건 
r = df[ df['country'] == '한국']  # DataFrame 반환 
print(type(r))  # DataFrame인 것을 확인
r # r 출력
```
```
<class 'pandas.core.frame.DataFrame'>
country	gdp	capital
0	한국	100	서울
```

```python
r = df[ (df['country'] =='한국') | ( df['country'] == '미국') ]  # 한국이나 미국인 데이터 
r
```
```
	country	gdp	capital
0	한국	100	서울
2	미국	300	워싱턴
```
```python
r = df[  (df['country'] != '한국') & ( df['gdp'] >= 200 ) ]  # 한국이 아니고 gdp >= 200인 데이터 
r
```
```
	country	gdp	capital
1	영국	200	런던
2	미국	300	워싱턴
```

```python
list = [ '한국', '미국']
r = df[ df['country'].isin(list)]   # list 안의 국가들 선택
r
```
```

country	gdp	capital
0	한국	100	서울
2	미국	300	워싱턴
```
```python
list = [ '한국', '미국']
r = df[ ~df['country'].isin(list)]   # list 안의 국가가 아닌 데이터
r
```
```
	country	gdp	capital
1	영국	200	런던
```

