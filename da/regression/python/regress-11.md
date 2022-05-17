# statsmodel 다중선형회귀분석


이 예제에서는 데이터를 분리하지 않았다. 데이터 분리에대해서는 sklearn의 데이터셋 분리를 참고한다.

## 회귀분석
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib as mpl
```
```python
df = pd.DataFrame({
'radio_ads': [3,4,9,4,5,5,2,6,5,3],
'tv_ads':    [1,3,4,1,4,1,4,2,4,2],
'retention': [5,1,6,2,8,3,4,9,7,4],
})

X = df[['radio_ads','tv_ads']]
y = df['retention']

import statsmodels.api as sm

X = sm.add_constant(X)
model = sm.OLS(y, X)
result = model.fit()

print( result.params )
print( "R²=", result.rsquared )  # 결정계수 
```
```
const        1.366972
radio_ads    0.472477
tv_ads       0.522936
dtype: float64
R²= 0.25166839909010097
```

**회귀식** 
 
```
y = 0.472477x1 -0.522936x2 + 1.366972
``` 
```python
print(result.summary())
```
```
                            OLS Regression Results                            
==============================================================================
Dep. Variable:              retention   R-squared:                       0.252
Model:                            OLS   Adj. R-squared:                  0.038
Method:                 Least Squares   F-statistic:                     1.177
Date:                Tue, 17 May 2022   Prob (F-statistic):              0.363
Time:                        04:39:37   Log-Likelihood:                -21.773
No. Observations:                  10   AIC:                             49.55
Df Residuals:                       7   BIC:                             50.45
Df Model:                           2                                         
Covariance Type:            nonrobust                                         
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
const          1.3670      2.441      0.560      0.593      -4.405       7.139
radio_ads      0.4725      0.452      1.046      0.330      -0.596       1.541
tv_ads         0.5229      0.654      0.799      0.450      -1.024       2.070
==============================================================================
Omnibus:                        0.013   Durbin-Watson:                   1.931
Prob(Omnibus):                  0.993   Jarque-Bera (JB):                0.238
Skew:                           0.010   Prob(JB):                        0.888
Kurtosis:                       2.245   Cond. No.                         17.5
==============================================================================
```
