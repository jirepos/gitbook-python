# index 다루기
```python 
import pandas as pd
```
```python 
df = pd.DataFrame({
   "이름": ['홍길동', '박찬수', '정민호', '김길동'] ,
   "영어": [ 80, 95, 40, 80],
   "수학": [ 50 , 67, 85, 90],
   "사회": [ 50 , 67, 85, 90],
})
df
```
```
	이름	영어	수학	사회
0	홍길동	80	50	50
1	박찬수	95	67	67
2	정민호	40	85	85
3	김길동	80	90	90
```

## index 확인
인덱스를 별도로 설정하지 하지 않으면 0부터 숫자로 인덱스가 생성된다.
```python 
df.index
```
```
RangeIndex(start=0, stop=4, step=1)
```

## index 생성 
```
DataFrame.set_index(column, drop=True, append=False, inpcae=False)
```

* drop 기존 컬럼을 인덱스로 만들때, 기존 컬럼을 버릴지 여부
* append 기존 인덱스에 내가 원하는 컬럼까지 추가해서 인덱스를 생성할지 여부
* inplace 원본 데이터에 덮어쒸울지 여부

```python 
df.set_index("이름", inplace=True)
df
```
## index 초기화
index는 값이 중복되어도 관계 없다. 
```python 
df = pd.DataFrame({
   "이름": ['홍길동', '박찬수', '정민호', '김길동'] ,
   "영어": [ 80, 95, 40, 80],
   "수학": [ 50 , 67, 85, 90],
   "사회": [ 50 , 67, 85, 90],
}, index=[1,2,2,3])  # DataFrame을 만들 때 index 설정
df
```
```
	이름	영어	수학	사회
1	홍길동	80	50	50
2	박찬수	95	67	67
2	정민호	40	85	85
3	김길동	80	90	90
```
```python 

df.set_index("이름", drop=True, inplace=True) # 인덱스 설정
df
```
```
영어	수학	사회
이름			
홍길동	80	50	50
박찬수	95	67	67
정민호	40	85	85
김길동	80	90	90
```

```python 
df.reset_index(drop=False, inplace=True)   # index 재설정, 기존 인덱스는 유지 
df   # 인덱스가 0부터 차례로 초기화 된 것을 확인
```
```
	이름	영어	수학	사회
0	홍길동	80	50	50
1	박찬수	95	67	67
2	정민호	40	85	85
3	김길동	80	90	90
```

### index 재설정
```python 
df.index = ['정말로', '그렇지', '한양', '강인하']  # index 재설정
df
```
```
	이름	영어	수학	사회
정말로	홍길동	80	50	50
그렇지	박찬수	95	67	67
한양	정민호	40	85	85
강인하	김길동	80	90	90
```

### 인덱스 데이터  추출
```python 
df.index
```
```
Index(['정말로', '그렇지', '한양', '강인하'], dtype='object')
```

```python 
for idxname in df.index:
  print(idxname)
```  
```
정말로
그렇지
한양
강인하
```

기수를 사용하여 인덱스 값 꺼낼 수 있다. 
```python 
print(df.index[0])
print(df.index[1])
print(df.index[2])
```
```
정말로
그렇지
한양
```

### 인덱스 기준 정렬
```python 
df = pd.DataFrame({
   "이름": ['홍길동', '박찬수', '정민호', '김길동'] ,
   "영어": [ 80, 95, 40, 80],
   "수학": [ 50 , 67, 85, 90],
   "사회": [ 50 , 67, 85, 90],
})  
df.set_index("이름", inplace=True)
df.sort_index(inplace=True)
df
```

```
영어	수학	사회
이름			
김길동	80	90	90
박찬수	95	67	67
정민호	40	85	85
홍길동	80	50	50
```

