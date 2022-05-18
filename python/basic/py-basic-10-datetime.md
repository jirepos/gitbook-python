# 날짜와 시간 다루기


```python
from datetime import datetime
from datetime import date 
from datetime import timedelta
from datetime import time
from datetime import timezone
```

## timedelta
timedelta 클래스의 생성자는 주, 일, 시, 분, 초, 밀리 초, 마이크로 초를 인자로 받습니다.
```python
# 1주 2일 1시간 10분 20초, 합치면 9일 1시간 10분 20초가 된다.
d1 = timedelta(weeks = 1, days = 2, hours = 1, minutes = 10, seconds = 20)
print(d1)
# 2일 1시간 20초 
d2 = timedelta(days = 2, hours = 1, seconds = 20) 
print(d2)
# d1 - d2 하면 
d3 = d1 - d2 
print(d3)
```
```
9 days, 1:10:20
2 days, 1:00:20
7 days, 0:10:00
```
### 날짜 더하기
```python
today = date.today()  # 오늘 날짜
print(today)
one_week = timedelta(days=7)
next_week = today + one_week  # 오늘 날짜에 일주일(7)을 더한다. 
print(next_week)  # 7일후 날짜 

two_weeks = one_week * 2 
print(two_weeks)  # 7 X 2 = 14 days 
```
```
2022-05-17
2022-05-24
14 days, 0:00:00
```
## date
date 클래스의 생성자는 연, 월, 일 데이터를 인자로 받습니다. 시간정보가 없기 때문에 timezone 정보가 없습니다.
```python
d = date(2019, 12, 25)
print(d)
```
```
2019-12-25
```

### 오늘 날짜 구하기 
```python
d = date.today() # 오늘날짜 구하기
print(d)
```
```
2022-05-17
```
### 문자열을 date로 변환 
```python
d = date.fromisoformat('2022-05-22')
print(d)
```
```
2022-05-22
```
### date의 속성
```python
d = date.today() 
print(d.year) # 년  
print(d.month)  # 월 
print(d.day)   # 일 
```
```
2022
5
17
```
### 요일 파악
```python
d = date.today() 
print(d.weekday())  # 월요일이 0로 시작 
print(d.isoweekday()) # 월요일이 1로 시작
```
```
1
2
```
### 년월일 데이터 변경 
```python
d = date.today()
d2 = d.replace(year=2019) #  연도변경 
print(d2) 
```
```
2019-05-17
```
## time
time 클래스의 생성자는 시, 분, 초, 마이크로 초, 시간대를 인자로 받습니다. 모든 인자가 필수 인자가 아니며, 생략할 경우, 0이 기본값으로 사용됩니다.
```python
t = time(13 ,20, 20)
print(t)
```
```
13:42:35.458000+09:00
```

### 문자열을 시간으로 변환
time 클래스의 fromisoformat() 메서드는 HH[:MM[:SS[.fff[fff]]]][+HH:MM[:SS[.ffffff]]] 형태의 문자열을 time 객체로 변환해줍니다.
```python 
 t =time.fromisoformat('13:42:35.458+09:00')
 print(t)
```
```
13:42:35.458000+09:00
```

### 시간을 문자열로 변환
반대로 time 클래스의 isoformat 메서드는 time 객체를 HH[:MM[:SS[.fff[fff]]]][+HH:MM[:SS[.ffffff]]] 형태의 문자열로 변환해줍니다.
```python 
t = time(13, 42, 35, 458000, tzinfo=timezone(timedelta(hours=9)))
print(t)
```


### time 속성
```python 
t = time(13, 42, 35, 458000, tzinfo=timezone(timedelta(hours=9)))
print(t.hour)
print(t.minute)
print(t.second)
print(t.microsecond)
print(t.tzinfo)
```
```
13
42
35
458000
UTC+09:00
```


### time 데이터 변경
```python 
r = t.replace(hour=14)
print(r)
```
```
14:42:35.458000+09:00
```

## datetime
datetime 클래스의 생성자는 연, 월, 일, 시, 분, 초, 마이크로 초, 시간대를 인자로 받습니다. 시간 이하의 인자는 필수 인자가 아니며 생략할 경우, 0이 기본값으로 사용됩니다.
```python 
d = datetime(2020, 7, 18, 13, 26, 23) 
print(d)
```
```
2020-07-18 13:26:23
```

### date와 time 병합
combine() 메서드를 사용하면 기존에 생성해둔 date나 time 객체를 활용해서 datetime 객체를 생성할 수 있습니다.
```python 
d = date(2020, 7, 18)
t = time(13, 26, 23)
r = datetime.combine(d,t)
print(r)
```
```
2020-07-18 13:26:23
```

### 현재 시간 구하기
```python 
r = datetime.now()
print(r)
```
```
2022-05-17 22:11:40.157403
```
now() 메서드에 시간대를 인자로 넘기면 해당 시간대가 적용된 datetime 객체가 생성됩니다.
```python 
 r = datetime.now(timezone.utc) # 타입존을 설정하여 생성
 print(r)
```
```
2022-05-17 22:11:40.172207+00:00
```

### 날짜 문자열 변환
```python 
today = datetime.now()
r = today.strftime('%Y/%m/%d')
r
```
```
2022/05/17
```
### 문자열 날짜 변환
```python 
d =datetime.strptime('2020/07/18', '%Y/%m/%d')
d
```
```
datetime.datetime(2020, 7, 18, 0, 0)
```
포맷은 [파이썬 공식 홈페이지](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes)를 참고하세요.


## 날짜 처리

### 날짜 비교
datetime.date()를 사용하여 두 날짜를 비교할 수도 있습니다. datetime.date()메소드는year, month, day을 입력으로 사용합니다. 비교할 두 날짜를 만들고 간단한 비교 연산자를 사용하여 두 날짜를 비교합니다.
```python 
first_date = date(2020, 12, 16)
second_date = date(2015, 12, 16)

result = first_date < second_date
print(result)
```
```
False
```
datetime 모듈은 세 개의 매개 변수를 사용하여 연, 월, 일의 날짜를 만드는datetime()메서드를 제공합니다. 날짜를 가져온 후 비교 연산자를 사용하여 비교할 수 있습니다.
```python 
# date in yy/mm/dd format
first_date = datetime(2020, 5, 11)
second_date = datetime(2020, 6, 10)

print("first date is greater than second_date: ", first_date > second_date)
print("first date is smaller than second_date: ", first_date < second_date)
print("first date is not equal to second_date: ", first_date != second_date)
```
```
first date is greater than second_date:  False
first date is smaller than second_date:  True
first date is not equal to second_date:  True
```


## References
[Pandas 날짜 함수](https://truman.tistory.com/97)        
[Python datetime](https://www.daleseo.com/python-datetime/)      

