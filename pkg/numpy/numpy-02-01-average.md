# 평균, 합계

## 평균 
```
import numpy as np
import matplotlib.pyplot as plt
```

각 과목별 점수
```
points = [ 90,75,40,40,90,65,70]
npoints = np.array(points)
npoints
```

```
array([90, 75, 40, 40, 90, 65, 70])
```


average()로 평균을 구한다.


```
np.average(npoints)
```
```
67.14285714285714
```


amax()로 최대값을 구한다.

```
x = np.amax(npoints)
x
```
```
90
```


numpy의 bincount와 argmax로 쉽게 최빈값을 찾을 수 있다. bincount는  0부터 최대 n-1까지 숫자가 몇 개 있는지 array로 돌려준다.

```
x = np.bincount(npoints)
x
```
```
array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
       0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 2])
```

```
y = np.argmax(x)
y
```
```
40
```


amin()으로 최소값을 구한다.
```
x = np.amin(npoints)
x
```
```
40
```


중간값(median)을 구한다.
```
x = np.median(npoints)
x
```
```
70.0
```


## 누적합계 cumsum 함수 
cumsum은 배열에서 주어진 축에 따라 누적되는 원소들의 누적 합을 계산하는 함수.

```
a = np.array([[1, 2, 3], [4, 5, 6]])
a
```
```
array([[1, 2, 3],
       [4, 5, 6]])
```       

```
print(np.cumsum(a))
```
```
[ 1  3  6 10 15 21]
```




각 원소들의 누적 합을 표시함. 각 row와 column의 구분은 없어지고, 순서대로 sum을 함.

```
print(np.cumsum(a, dtype = float))
```

```
[ 1.  3.  6. 10. 15. 21.]
```

결과 값의 변수 type을 설정하면서 누적 sum을 함.

```
print(np.cumsum(a, axis = 0))
```
```
[[1 2 3]
 [5 7 9]]
```

axis = 0은 같은 column 끼리의 누적 합을 함.


```
print(np.cumsum(a, axis = 1))
```
```
[[ 1  3  6]
 [ 4  9 15]]
```

axis = 1은 같은 row끼리의 누적 합을 함.


