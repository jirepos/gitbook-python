# Pandas 날짜 함수

```python 
from datetime import datetime
from datetime import date 
from datetime import timedelta
from datetime import time
from datetime import timezone

import numpy as np
import pandas as pd
```


## pandas.to_datetime
여러 개의 컬럼을 datetime64 series로 변환한다. 
```python 
df = pd.DataFrame({'year': [2015, 2016],
                   'month': [2, 3],
                   'day': [4, 5]})
print(df) 

r = pd.to_datetime(df)  # 여러개의 컬럼을 datetime64 Series로 변환 
print(type(r))
print(r)
```
```
   year  month  day
0  2015      2    4
1  2016      3    5
<class 'pandas.core.series.Series'>
0   2015-02-04
1   2016-03-05
dtype: datetime64[ns]
```


문자열을 넣을 수도 있다.

```python 
r = pd.to_datetime(['2018-10-26 12:00', '2018-10-26 13:00'], utc=True)
print(type(r))
display(r)
```
```
<class 'pandas.core.indexes.datetimes.DatetimeIndex'>
DatetimeIndex(['2018-10-26 12:00:00+00:00', '2018-10-26 13:00:00+00:00'], dtype='datetime64[ns, UTC]', freq=None)
```


문자열과 datetime을 같이 넣을 수도 있다. 

```python 
r = pd.to_datetime(['2018-10-26 12:00', '2018-10-26 13:00'
                    , datetime(2020, 1, 1, 18)
                    , datetime(2020, 1, 1, 18) ], utc=True)
print(type(r))
display(r)
```
```
<class 'pandas.core.indexes.datetimes.DatetimeIndex'>
DatetimeIndex(['2018-10-26 12:00:00+00:00', '2018-10-26 13:00:00+00:00',
               '2020-01-01 18:00:00+00:00', '2020-01-01 18:00:00+00:00'],
              dtype='datetime64[ns, UTC]', freq=None)
```




### dt 객체 사용

```python 
# 문자열 DataFrame 생성 

df = pd.DataFrame({'Birth':['2019-01-01 09:10:00',
                            '2019-01-08 09:20:30',
                            '2019-02-01 10:20:00',
                            '2019-02-02 11:40:50',
                            '2019-02-28 15:10:20',
                            '2019-04-10 19:20:50',
                            '2019-06-30 21:20:50',
                            '2019-07-20 23:30:59']})
print(df)

# datetime으로 변환 
r = pd.to_datetime(df['Birth'], format='%Y-%m-%d %H:%M:%S', errors='raise')
print(type(r))  #  Series로 반환 
print(r)  # datetime64로 타입이 변경

df['Birth'] = r   # Series를 변경 
print(df)
```
```
                 Birth
0  2019-01-01 09:10:00
1  2019-01-08 09:20:30
2  2019-02-01 10:20:00
3  2019-02-02 11:40:50
4  2019-02-28 15:10:20
5  2019-04-10 19:20:50
6  2019-06-30 21:20:50
7  2019-07-20 23:30:59
<class 'pandas.core.series.Series'>
0   2019-01-01 09:10:00
1   2019-01-08 09:20:30
2   2019-02-01 10:20:00
3   2019-02-02 11:40:50
4   2019-02-28 15:10:20
5   2019-04-10 19:20:50
6   2019-06-30 21:20:50
7   2019-07-20 23:30:59
Name: Birth, dtype: datetime64[ns]
                Birth
0 2019-01-01 09:10:00
1 2019-01-08 09:20:30
2 2019-02-01 10:20:00
3 2019-02-02 11:40:50
4 2019-02-28 15:10:20
5 2019-04-10 19:20:50
6 2019-06-30 21:20:50
7 2019-07-20 23:30:59
```


```python 
r = df['Birth'].dt.date 
print("type is " , type(r))  # r은 Series
display(r)  
```
```
type is  <class 'pandas.core.series.Series'>
0    2019-01-01
1    2019-01-08
2    2019-02-01
3    2019-02-02
4    2019-02-28
5    2019-04-10
6    2019-06-30
7    2019-07-20
Name: Birth, dtype: object
```

dt의 속성을 살펴보자.
```python 
df['Birth_date']       = df['Birth'].dt.date         # YYYY-MM-DD(문자)
df['Birth_year']       = df['Birth'].dt.year         # 연(4자리숫자)
df['Birth_month']      = df['Birth'].dt.month        # 월(숫자)
df['Birth_month_name'] = df['Birth'].dt.month_name() # 월(문자)

df['Birth_day']        = df['Birth'].dt.day          # 일(숫자)
df['Birth_time']       = df['Birth'].dt.time         # HH:MM:SS(문자)
df['Birth_hour']       = df['Birth'].dt.hour         # 시(숫자)
df['Birth_minute']     = df['Birth'].dt.minute       # 분(숫자)
df['Birth_second']     = df['Birth'].dt.second       # 초(숫자)

display(df)
```
```
Birth	Birth_date	Birth_year	Birth_month	Birth_month_name	Birth_day	Birth_time	Birth_hour	Birth_minute	Birth_second
0	2019-01-01 09:10:00	2019-01-01	2019	1	January	1	09:10:00	9	10	0
1	2019-01-08 09:20:30	2019-01-08	2019	1	January	8	09:20:30	9	20	30
2	2019-02-01 10:20:00	2019-02-01	2019	2	February	1	10:20:00	10	20	0
3	2019-02-02 11:40:50	2019-02-02	2019	2	February	2	11:40:50	11	40	50
4	2019-02-28 15:10:20	2019-02-28	2019	2	February	28	15:10:20	15	10	20
5	2019-04-10 19:20:50	2019-04-10	2019	4	April	10	19:20:50	19	20	50
6	2019-06-30 21:20:50	2019-06-30	2019	6	June	30	21:20:50	21	20	50
7	2019-07-20 23:30:59	2019-07-20	2019	7	July	20	23:30:59	23	30	59
```




```python 
df['Birth_quarter']       = df['Birth'].dt.quarter       # 분기(숫자)
#df['Birth_weekday_name']  = df['Birth'].dt.weekday_name  # deprecated
#df['Birth_weekday']       = df['Birth'].dt.weekday       # deprecated
#df['Birth_weekofyear']    = df['Birth'].dt.weekofyear    # deprecated
df['Birth_dayofyear']     = df['Birth'].dt.dayofyear     # 연 기준 몇일째(숫자)
df['Birth_days_in_month'] = df['Birth'].dt.days_in_month # 월 일수(숫자) (=daysinmonth)

display(df)
```

```python 
df['Birth_is_leap_year']     = df['Birth'].dt.is_leap_year     # 윤년 여부
df['Birth_is_month_start']   = df['Birth'].dt.is_month_start   # 월 시작일 여부
df['Birth_is_month_end']     = df['Birth'].dt.is_month_end     # 월 마지막일 여부
df['Birth_is_quarter_start'] = df['Birth'].dt.is_quarter_start # 분기 시작일 여부
df['Birth_is_quarter_end']   = df['Birth'].dt.is_quarter_end   # 분기 마지막일 여부
df['Birth_is_year_start']    = df['Birth'].dt.is_year_start    # 연 시작일 여부
df['Birth_is_year_end']      = df['Birth'].dt.is_year_end      # 연 마지막일 여부

display(df)
```


## DatetimeIndex #1
아래 내용은 [여기](https://datascienceschool.net/01%20python/04.08%20%EC%8B%9C%EA%B3%84%EC%97%B4%20%EC%9E%90%EB%A3%8C%20%EB%8B%A4%EB%A3%A8%EA%B8%B0.html)를 참조한다. 

문자열 리스트로 DatetimeIndex를 생성한다. 


```python 
date_str = ["2019, 1, 1", "2019, 1, 4", "2019, 1, 5", "2019, 1, 6"]
idx = pd.to_datetime(date_str)
idx
```
```
DatetimeIndex(['2019-01-01', '2019-01-04', '2019-01-05', '2019-01-06'], dtype='datetime64[ns]', freq=None)
```


인덱스를 사용하여 시리즈나 DataFrame을 생성한다. 

```python 
np.random.seed(0)
s = pd.Series(np.random.randn(4), index=idx)
s
```
```
2019-01-01    1.764052
2019-01-04    0.400157
2019-01-05    0.978738
2019-01-06    2.240893
dtype: float64
```




## DatetimeIndex #2
DateimeIndex에 대한 아래 내용은 [여기](https://queirozf.com/entries/pandas-time-series-examples-datetimeindex-periodindex-and-timedeltaindex)를 참조한다. 

### index로 존재하는 컬럼 사용하기


```python 
# this is the original dataframe
df = pd.DataFrame({
    'name':[
        'john','mary','peter','jeff','bill'
    ],
    'date_of_birth':[
        '2000-01-01', '1999-12-20', '2000-11-01', '1995-02-25', '1992-06-30',
    ],
})
```
```python 
print(df.index)
```
```
RangeIndex(start=0, stop=5, step=1)
```



```python 
# convert the column (it's a string) to datetime type
datetime_series = pd.to_datetime(df['date_of_birth'])
```
```python
# create datetime index passing the datetime series
datetime_index = pd.DatetimeIndex(datetime_series.values)
```
```python 
df2=df.set_index(datetime_index)
# we don't need the column anymore
df2.drop('date_of_birth',axis=1,inplace=True)
```
```python
print(df2.index)
```
```
DatetimeIndex(['2000-01-01', '1999-12-20', '2000-11-01', '1995-02-25',
               '1992-06-30'],
              dtype='datetime64[ns]', freq=None)
```              





### 빈 기간을 위한 행들을 추가하기


```python 
df = pd.DataFrame({
    'name':[
        'john','mary','peter','jeff','bill'
    ],
    'day_born':[
        '1988-12-28', '1988-12-25', '1988-12-24', '1988-12-26',  '1988-12-30'
    ],
})
df

df.index
# RangeIndex(start=0, stop=5, step=1)

# build a datetime index from the date column
datetime_series = pd.to_datetime(df['day_born'])
datetime_index = pd.DatetimeIndex(datetime_series.values)

```
```python
# replace the original index with the new one
df3=df.set_index(datetime_index)

# we don't need the column anymore
df3.drop('day_born',axis=1,inplace=True)
```
```python
df3.index
```
```
DatetimeIndex(['1988-12-24', '1988-12-25', '1988-12-26', '1988-12-28',
               '1988-12-30'],
              dtype='datetime64[ns]', freq=None)
```

```python 
df3.sort_index(inplace=True)
df3.index
# DatetimeIndex(['1988-12-24', '1988-12-25', '1988-12-26', '1988-12-28',
#               '1988-12-30'],
#              dtype='datetime64[ns]', freq=None)
```
```
DatetimeIndex(['1988-12-24', '1988-12-25', '1988-12-26', '1988-12-28',
               '1988-12-30'],
              dtype='datetime64[ns]', freq=None)
```
           

```python 
# 'D' stands for 'DAYS'
df4=df3.asfreq('D')  # freq를 D로 설정 
df4.index   # 12/27, 12/29일이 추가된 것을 확인할 수 있다. 
# DatetimeIndex(['1988-12-24', '1988-12-25', '1988-12-26', '1988-12-27',
#                '1988-12-28', '1988-12-29', '1988-12-30'],
#              dtype='datetime64[ns]', freq='D')

### shift를 사용하여 lag 컬럼 만들기

# create a dummy dataset
df = pd.DataFrame(
    data={'reading': np.random.uniform(high=100,size=10)},
    index=pd.to_datetime([date(2019,1,d) for d in range(1,11)])
)

# D minus 1 = current day minus 1
df['reading_d_minus_1']=df['reading'].shift(1,freq='D')

# create columns for 2 days before as well
df['reading_d_minus_2']=df['reading'].shift(2,freq='D')

df
```
```

reading	reading_d_minus_1	reading_d_minus_2
2019-01-01	7.987833	NaN	NaN
2019-01-02	80.946347	7.987833	NaN
2019-01-03	62.758511	80.946347	7.987833
2019-01-04	80.010578	62.758511	80.946347
2019-01-05	37.292655	80.010578	62.758511
2019-01-06	87.599021	37.292655	80.010578
2019-01-07	20.855443	87.599021	37.292655
2019-01-08	11.988379	20.855443	87.599021
2019-01-09	2.172082	11.988379	20.855443
2019-01-10	93.699036	2.172082	11.988379
```

## date_range
pd.date_range 함수를 쓰면 모든 날짜/시간을 일일히 입력할 필요없이 시작일과 종료일 또는 시작일과 기간을 입력하면 범위 내의 인덱스를 생성해 준다.
```python 
r = pd.date_range("2018-4-1", "2018-4-30")
r
```
```
DatetimeIndex(['2018-04-01', '2018-04-02', '2018-04-03', '2018-04-04',
               '2018-04-05', '2018-04-06', '2018-04-07', '2018-04-08',
               '2018-04-09', '2018-04-10', '2018-04-11', '2018-04-12',
               '2018-04-13', '2018-04-14', '2018-04-15', '2018-04-16',
               '2018-04-17', '2018-04-18', '2018-04-19', '2018-04-20',
               '2018-04-21', '2018-04-22', '2018-04-23', '2018-04-24',
               '2018-04-25', '2018-04-26', '2018-04-27', '2018-04-28',
               '2018-04-29', '2018-04-30'],
              dtype='datetime64[ns]', freq='D')
```              

```python
r = pd.date_range(start="2018-4-1", periods=30)
r
```
```
DatetimeIndex(['2018-04-01', '2018-04-02', '2018-04-03', '2018-04-04',
               '2018-04-05', '2018-04-06', '2018-04-07', '2018-04-08',
               '2018-04-09', '2018-04-10', '2018-04-11', '2018-04-12',
               '2018-04-13', '2018-04-14', '2018-04-15', '2018-04-16',
               '2018-04-17', '2018-04-18', '2018-04-19', '2018-04-20',
               '2018-04-21', '2018-04-22', '2018-04-23', '2018-04-24',
               '2018-04-25', '2018-04-26', '2018-04-27', '2018-04-28',
               '2018-04-29', '2018-04-30'],
              dtype='datetime64[ns]', freq='D')
```              

freq 인수로 특정한 날짜만 생성되도록 할 수도 있다. 많이 사용되는 freq 인수값은 다음과 같다.

* s: 초
* T: 분
* H: 시간
* D: 일(day)
* B: 주말이 아닌 평일
* W: 주(일요일)
* W-MON: 주(월요일)
* M: 각 달(month)의 마지막 날
* MS: 각 달의 첫날
* BM: 주말이 아닌 평일 중에서 각 달의 마지막 날
* BMS: 주말이 아닌 평일 중에서 각 달의 첫날
* WOM-2THU: 각 달의 두번째 목요일
* Q-JAN: 각 분기의 첫달의 마지막 날
* Q-DEC: 각 분기의 마지막 달의 마지막 날


```python 
r = pd.date_range("2018-4-1", "2018-4-30", freq="B")   # 주말이 아닌 평일
r
```
```
DatetimeIndex(['2018-04-02', '2018-04-03', '2018-04-04', '2018-04-05',
               '2018-04-06', '2018-04-09', '2018-04-10', '2018-04-11',
               '2018-04-12', '2018-04-13', '2018-04-16', '2018-04-17',
               '2018-04-18', '2018-04-19', '2018-04-20', '2018-04-23',
               '2018-04-24', '2018-04-25', '2018-04-26', '2018-04-27',
               '2018-04-30'],
              dtype='datetime64[ns]', freq='B')
```
              

## shift 연산
시계열 데이터의 인덱스는 시간이나 날짜를 나타내기 때문에 날짜 이동 등의 다양한 연산이 가능하다. 예를 들어 shift 연산을 사용하면 인덱스는 그대로 두고 데이터만 이동할 수도 있다.

```python 
np.random.seed(0)
ts = pd.Series(np.random.randn(4), index=pd.date_range(
    "2018-1-1", periods=4, freq="M"))
ts
```
```
2018-01-31    1.764052
2018-02-28    0.400157
2018-03-31    0.978738
2018-04-30    2.240893
Freq: M, dtype: float64
```

```python 
ts.shift(1)
```
```
2018-01-31         NaN
2018-02-28    1.764052
2018-03-31    0.400157
2018-04-30    0.978738
Freq: M, dtype: float64
```
```python 
ts.shift(-1)
```
```
2018-01-31    0.400157
2018-02-28    0.978738
2018-03-31    2.240893
2018-04-30         NaN
Freq: M, dtype: float64
```

```python 
ts.shift(1, freq="M")  # 월을 shift 시킨다. 월이 1 증가했다 
```
```
2018-02-28    1.764052
2018-03-31    0.400157
2018-04-30    0.978738
2018-05-31    2.240893
Freq: M, dtype: float64
```
```python 
ts.shift(1, freq="W")
```
```
2018-02-04    1.764052
2018-03-04    0.400157
2018-04-01    0.978738
2018-05-06    2.240893
dtype: float64
```
## References
[Pandas Time Series Examples: DatetimeIndex, PeriodIndex and TimedeltaIndex](https://queirozf.com/entries/pandas-time-series-examples-datetimeindex-periodindex-and-timedeltaindex)     
[시계열 자료 다루기](https://datascienceschool.net/01%20python/04.08%20%EC%8B%9C%EA%B3%84%EC%97%B4%20%EC%9E%90%EB%A3%8C%20%EB%8B%A4%EB%A3%A8%EA%B8%B0.html)     

