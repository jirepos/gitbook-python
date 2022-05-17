# random

random 모듈의 다양한 함수를 사용해서 특정 범위, 개수, 형태를 갖는 난수 생성에 활용할 수 있습니다.

```
import numpy as np
```

## rand()

random.rand() 함수는 주어진 형태의 난수 어레이를 생성합니다.
```
a = np.random.rand(5)
print(a)
```
```
[0.89119488 0.7199807  0.02795657 0.53454484 0.68083921]
```
```
b = np.random.rand(2, 3)
print(b)
```
```
[[0.80510305 0.6928911  0.24254103]
 [0.0607397  0.67507393 0.95970992]]
```

만들어진 난수 어레이는 주어진 값에 의해 결정되며, [0, 1] 범위에서 균일한 분포를 갖습니다.

## randint()
random.randint() 함수는 [최소값, 최대값)의 범위에서 임의의 정수를 만듭니다.
```
a = np.random.randint(2, size=5)
print(a)
```
```
[0 1 0 0 0]
```
```
b = np.random.randint(2, 4, size=5)
print(b)
```
```
[2 3 3 2 2]
```
```
c = np.random.randint(1, 5, size=(2, 3))
print(c)
```
```
[[3 3 1]
 [3 1 4]]
```


np.random.randint(2, size=5)는 [0, 2) 범위에서 다섯개의 임의의 정수를 생성합니다.

np.random.randint(2, 4, size=5)는 [2, 4) 범위에서 다섯개의 임의의 정수를 생성합니다.

np.random.randint(1, 5, size=(2, 3))는 [1, 5) 범위에서 (2, 3) 형태의 어레이를 생성합니다.



## randn()
random.randn() 함수는 표준정규분포 (Standard normal distribution)로부터 샘플링된 난수를 반환합니다.

```
a = np.random.randn(5)
print(a)
```
```
[ 0.52575161  0.77252148 -1.17981952 -0.5148624  -0.05645037]
```
```
b = np.random.randn(2, 3)
print(b)
```
```
[[-1.92109182e+00 -3.67133541e-01  1.91102761e-03]
 [-4.44998542e-01 -9.66800315e-01 -8.12986819e-01]]
```
```
sigma, mu = 1.5, 2.0

c = sigma * np.random.randn(5) + mu
print(c)
```
```
[0.89037764 1.04867219 1.44127973 0.13671483 2.26720721]
```

표준정규분포 N(1, 0)이 아닌, 평균 μ, 표준편차 σ 를 갖는 정규분포 N(μ, σ2)의 난수를 생성하기 위해서는

σ * np.random.randn(…) + μ 와 같은 형태로 사용할 수 있습니다.

## standard_normal()

random.standard_normal() 함수는 표준정규분포 (Standard normal distribution)로부터 샘플링된 난수를 반환합니다.

standard_normal() 함수는 randn() 함수와 비슷하지만, 튜플을 인자로 받는다는 점에서 차이가 있습니다.

```
d = np.random.standard_normal(3)
print(d)
```

```
[ 0.96882887  0.58934781 -0.53467801]
```

```
e = np.random.standard_normal((2, 3))
print(e)
```

```
[[ 0.21902985 -0.98778171  0.17344456]
 [-0.04757754  0.3924394   0.68302612]]
```

## normal()
random.normal() 함수는 정규 분포 (Normal distribution)로부터 샘플링된 난수를 반환합니다.
```
a = np.random.normal(0, 1, 2)
print(a)

b = np.random.normal(1.5, 1.5, 4)
print(b)

c = np.random.normal(3.0, 2.0, (2, 3))
print(c)
```
```
[-0.69300319 -0.13899936]
[ 3.51111585 -0.53274155  2.24590135  1.06768658]
[[3.27188802 0.52795847 1.0429955 ]
 [1.81041823 2.41804771 1.53837881]]
```
어레이 a는 정규 분포 N(0,1)로부터 얻은 임의의 숫자 2개,

어레이 b는 정규 분포 N(1.5,1.52)로부터 얻은 임의의 숫자 4개,

어레이 c는 정규 분포 N(3.0,2.02)로부터 얻은 (2, 3) 형태의 임의의 숫자 어레이입니다.

## random_sample()
random.random_sample() 함수는 [0.0, 1.0) 범위에서 샘플링된 임의의 실수를 반환합니다.
```
a = np.random.random_sample()
print(a)

b = np.random.random_sample((5, 2))
print(b)

c = 5 * np.random.random_sample((3, 2)) - 3
print(c)
```
```
0.792507367214257
[[0.97610716 0.00480262]
 [0.23304898 0.73128194]
 [0.54728435 0.03134051]
 [0.07127394 0.65021676]
 [0.38425608 0.38611926]]
[[-1.2258768   1.00867716]
 [ 0.4731079  -0.46004769]
 [-0.03086501 -0.98983907]]
```
## seed()
random.seed() 함수는 난수 생성에 필요한 시드를 설정합니다.
```
np.random.seed(0)

print(np.random.rand(2, 3))

```

```
[[0.5488135  0.71518937 0.60276338]
 [0.54488318 0.4236548  0.64589411]]
```
(2, 3) 형태의 2차원 난수 어레이가 생성되었습니다.

코드를 실행할 때마다 동일한 난수가 생성됩니다.

## 참고

https://codetorial.net/numpy/random.html
