# DataFrame 컬럼 추가, 삭제

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
## 컬럼 추가

### 단일값으로 컬럼 추가
```python 
df["과학"] = [ 100, 200, 300, 400]
df
```
```
이름	영어	수학	사회	과학
0	홍길동	80	50	50	100
1	박찬수	95	67	67	200
2	정민호	40	85	85	300
3	김길동	80	90	90	400
```
### 다른 컬럼을 이용한 컬럼 추가
```python 
df["검증"] = df["영어"] > 10
df
```
```
이름	영어	수학	사회	과학	검증
0	홍길동	80	50	50	100	True
1	박찬수	95	67	67	200	True
2	정민호	40	85	85	300	True
3	김길동	80	90	90	400	True
```

# 두 컬럼 이용 
```python
df["윤리"] = df["영어"] + df["수학"]
df
```
```
이름	영어	수학	사회	과학	검증	윤리
0	홍길동	80	50	50	100	True	130
1	박찬수	95	67	67	200	True	162
2	정민호	40	85	85	300	True	125
3	김길동	80	90	90	400	True	170
```


### Series로 컬럼 추가
변경시에도 가능하다.
```python 
df["역사"] = pd.Series([50,60,70,80])
df
```
```
이름	영어	수학	사회	과학	검증	윤리	역사
0	홍길동	80	50	50	100	True	130	50
1	박찬수	95	67	67	200	True	162	60
2	정민호	40	85	85	300	True	125	70
3	김길동	80	90	90	400	True	170	80
```

### 리스트로 컬럼 추가
```python 
df["문학"] = [ 10,20,30, 40]
df
```

```
이름	영어	수학	사회	과학	검증	윤리	역사	문학
0	홍길동	80	50	50	100	True	130	50	10
1	박찬수	95	67	67	200	True	162	60	20
2	정민호	40	85	85	300	True	125	70	30
3	김길동	80	90	90	400	True	170	80	40
```
## 컬럼 삭제

## 선택한 컬럼 삭제
```python 
del df["검증"]
df
```
```
이름	영어	수학	사회	과학	윤리	역사	문학
0	홍길동	80	50	50	100	130	50	10
1	박찬수	95	67	67	200	162	60	20
2	정민호	40	85	85	300	125	70	30
3	김길동	80	90	90	400	170	80	40
```
```pyton 
df = df.drop("영어" ,1 )  # axis=1일 때 세로축 삭제   , 다음 버전에서는 labels을 제외하곤 파라미터 없을 것
df
```
```
이름	수학	사회	과학	윤리	역사	문학
0	홍길동	50	50	100	130	50	10
1	박찬수	67	67	200	162	60	20
2	정민호	85	85	300	125	70	30
3	김길동	90	90	400	170	80	40
```

```python 
df = df.drop(["사회", "윤리"], axis=1)
df
```
```
이름	수학	과학	역사	문학
0	홍길동	50	100	50	10
1	박찬수	67	200	60	20
2	정민호	85	300	70	30
3	김길동	90	400	80	40
```