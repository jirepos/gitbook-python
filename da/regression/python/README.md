# Python  회귀분석 

Python에서 회귀분석을 하려면 다음 두 개의 라이브러리를 사용한다. 

* statsmodel
* scikit-learn 

statsmodel에서는 상수항을 결합해야 하고 scikit-learn에서는 상수항 결합을 라이브러리에서 하기 때문에 상수항 결합을 할 필요 없다. 

## scikit-learn
```python
import numpy as np
import pandas as pd
```
```python
d = { 
    "x" : [ 1,2,3,4,5,6,7,8,9,10],
    "y"  : [1,2,3,4,5,6,7,8,9,10]
}
df = pd.DataFrame(d)  # 독립변수와 종속변수가 있는 DataFrame
df
```
```
	x	y
0	1	1
1	2	2
2	3	3
3	4	4
4	5	5
5	6	6
6	7	7
7	8	8
8	9	9
9	10	10
```
```python
x = df[ ['x']]
y = df[ ['y']]
```

scikit-learn 패키지를 사용하여 선형 회귀분석을 하는 경우에는 linear_model 서브 패키지의 LinearRegression 클래스를 사용한다. 사용법은 다음과 같다.

```
model = LinearRegression(fit_intercept=True)
```

fit_intercept 인수는 모형에 상수항이 있는가 없는가를 결정하는 인수이다. 디폴트 값이 True다. 만약 상수항이 없으면 fit_intercept=False로 설정한다.

fit 메서드로 가중치 값을 추정한다. 상수항 결합을 자동으로 해주므로 사용자가 직접 add_constant 등의 명령를 써서 상수항 결합을 할 필요는 없다.




```python
from sklearn.linear_model import LinearRegression
model = LinearRegression().fit(x, y)
```

fit 메서드를 호출하면 모형 객체는 다음과 같은 속성을 가지게 된다. 또한 fit 메서드는 객체 자신을 반환한다.

* coef_ : 추정된 가중치 벡터
* intercept_ : 추정된 상수항

```python
print(model.intercept_, model.coef_)
```
```
[-8.8817842e-16] [[1.]]
```

predict 메서드로 새로운 입력 데이터에 대한 출력 데이터 예측한다.
```python
xnew = pd.DataFrame( { "x": [1,2,3,4]})
y_new = model.predict(xnew) 
y_new
```
```
array([[1.],
       [2.],
       [3.],
       [4.]])
```

## statsmodel
```python
d = { 
    "x" : [ 1,2,3,4,5,6,7,8,9,10],
    "y"  : [1,2,3,4,5,6,7,8,9,10]
}
df = pd.DataFrame(d)  # 독립변수와 종속변수가 있는 DataFrame
df
```
```
x	y
0	1	1
1	2	2
2	3	3
3	4	4
4	5	5
5	6	6
6	7	7
7	8	8
8	9	9
9	10	10
```
```python
import statsmodels.api as sm

x = df[ ['x']]  # 독립변수 분리
x
```
```

x
0	1
1	2
2	3
3	4
4	5
5	6
6	7
7	8
8	9
9	10
```
```python
y = df[ ['y']] # 종속변수 분리
y
```
```

y
0	1
1	2
2	3
3	4
4	5
5	6
6	7
7	8
8	9
9	10
```
```python
# 상수 추가 
# OLS(x, y)와 같이 사용할 때에는 상수항을 추가해 주어야 한다.
x0 = sm.add_constant(x)
x0
# 결과에  const 컬럼이 추가된 것을 확인할 수 있다. 
```
```

const	x
0	1.0	1
1	1.0	2
2	1.0	3
3	1.0	4
4	1.0	5
5	1.0	6
6	1.0	7
7	1.0	8
8	1.0	9
9	1.0	10
```
```python
model = sm.OLS(x0, y)
```
formula를 사용할 수도 있다. ormula 문자열을 만드는 방법은 ~ 기호의 왼쪽에 종속변수의 이름을 넣고 ~ 기호의 오른쪽에 독립변수의 이름을 넣는다

```python
# formula를 사용할 수도 있다. 
model = sm.OLS.from_formula('y ~ x', data=df)
```
```python
result = model.fit() # 모델을 적합시킨다
```
RegressionResults 클래스 객체의 summary 메서드는 복잡한 형태의 보고서를 보여준다.

```python
print(result.summary())
```
```
                            OLS Regression Results                            
==============================================================================
Dep. Variable:                      y   R-squared:                       1.000
Model:                            OLS   Adj. R-squared:                  1.000
Method:                 Least Squares   F-statistic:                 3.589e+31
Date:                Tue, 17 May 2022   Prob (F-statistic):          6.75e-124
Time:                        03:07:28   Log-Likelihood:                 328.15
No. Observations:                  10   AIC:                            -652.3
Df Residuals:                       8   BIC:                            -651.7
Df Model:                           1                                         
Covariance Type:            nonrobust                                         
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
Intercept           0   1.04e-15          0      1.000   -2.39e-15    2.39e-15
x              1.0000   1.67e-16   5.99e+15      0.000       1.000       1.000
==============================================================================
Omnibus:                        2.741   Durbin-Watson:                   0.056
Prob(Omnibus):                  0.254   Jarque-Bera (JB):                1.083
Skew:                           0.353   Prob(JB):                        0.582
Kurtosis:                       1.550   Cond. No.                         13.7
==============================================================================
```
RegressionResults 클래스 객체의 predict 메서드를 사용하면 새로운 xnew 값에 대응하는 y 값을 예측할 수 있다.
```python
result.predict({"x": [3, 4, 5, 2, 3] })
```
```
0    3.0
1    4.0
2    5.0
3    2.0
4    3.0
dtype: float64
```
RegressionResults 클래스는 분석 결과를 다양한 속성에 저장해주므로 추후 사용자가 선택하여 활용할 수 있다. 자주 사용되는 속성으로는 다음과 같은 것들이 있다.

* params: 가중치 벡터
* resid: 잔차 벡터

가중치 벡터의 값은 다음처럼 확인한다.
```python
result.params
```
```
Intercept    0.0
x            1.0
dtype: float64
```


## 참조
[선형회귀분석의 기초](https://datascienceschool.net/03%20machine%20learning/04.02%20%EC%84%A0%ED%98%95%ED%9A%8C%EA%B7%80%EB%B6%84%EC%84%9D%EC%9D%98%20%EA%B8%B0%EC%B4%88.html)      
[sklearn 단순회귀분석](https://zetawiki.com/wiki/Sklearn_%EB%8B%A8%EC%88%9C%ED%9A%8C%EA%B7%80%EB%B6%84%EC%84%9D)     
