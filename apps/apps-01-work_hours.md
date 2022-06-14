# 근무시간 계산 


```python
import pandas as pd 
from datetime import date, timedelta, datetime
```



데이터프레임을 만든다. 


```python 
attn_data = {
    'attn_start': ['20220614 1010', '20220614 1020', '20220614 0930', ''],
    'attn_end'  : ['20220614 1110', '20220614 1850', '', '20220614 2030']
}
df = pd.DataFrame(attn_data)
display(df)
```
```
	attn_start	attn_end
0	20220614 1010	20220614 1110
1	20220614 1020	20220614 1850
2	20220614 0930	
3		20220614 2030
```

값이 없는 열 데이터 확인한다. 

```pytho 
df1 = df[ (df['attn_start'] == '') | ( df['attn_end'] =='')   ]
df1
```



비어 있는 데이터는 None을 반환하고 값이 있으면 datetime으로 변환하여 반환하는 함수를 만든다.

```python 
def to_datetime(val):
  if val == '':
    return None
  else:
    return  pd.to_datetime(val, format='%Y%m%d  %H%M%S') 
```


apply 함수를 사용하여 datetime으로 변환한 컬럼을 생성한다.
```python
df['start'] = df['attn_start'].apply(to_datetime)
df['end'] = df['attn_end'].apply(to_datetime)
df
```    
```
attn_start	attn_end	start	end
0	20220614 1010	20220614 1110	2022-06-14 10:01:00	2022-06-14 11:01:00
1	20220614 1020	20220614 1850	2022-06-14 10:02:00	2022-06-14 18:05:00
2	20220614 0930		2022-06-14 09:03:00	NaT
3		20220614 2030	NaT	2022-06-14 20:03:00
```

근무시간을 계산하는 함수를 만들고 적용한다.
```python
def time_diff(row):
  if row[2] == None or row[3] == None:
    return None
  else:
    return row[3] - row[2]
df['work_hours'] = df.apply(time_diff, axis=1)  
df
```

```
attn_start	attn_end	start	end	work_hours
0	20220614 1010	20220614 1110	2022-06-14 10:01:00	2022-06-14 11:01:00	0 days 01:00:00
1	20220614 1020	20220614 1850	2022-06-14 10:02:00	2022-06-14 18:05:00	0 days 08:03:00
2	20220614 0930		2022-06-14 09:03:00	NaT	NaT
3		20220614 2030	NaT	2022-06-14 20:03:00	NaT
```
