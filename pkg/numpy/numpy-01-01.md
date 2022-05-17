# numpy  부울린 인덱싱

numpy 부울린 인덱싱은 배열 각 요소의 선택여부를 True, False로 표현하는 방식이다. 만약 배열 a 가 2 x 3 의 배열이이라면, 부울린 인덱싱을 정의하는 numpy 배열도 2 x 3 으로 만들고 선택할 배열요소에 True를 넣고 그렇지 않으면 False를 넣으면 된다.
예를 들어, 아래 예제에서 3 x 3 배열 a 중 짝수만 뽑아내는 부울린 인덱싱 배열(numpy 배열)을 사용하여 짝수 배열 n 을 만드는 예이다.
```
lst = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
a = np.array(lst)
 
bool_indexing_array = np.array([
    [False,  True, False],
    [True, False,  True],
    [False,  True, False]
])
 
n = a[bool_indexing_array];
print(n)    
```
```
[2 4 6 8]
```



부울린 인덱싱 배열에 True/False 값을 일일이 지정하는 방법 이외에 표현식을 사용하여 부울린 인덱싱 배열을 생성하는 방법이 있다. 예를 들어, 배열 a 에 대해 짝수인 배열요소만 True로 만들고 싶다면, bool_indexing = (a % 2 == 0) 와 같이 표현할 수 있다. 아래 예제는 이러한 표현을 사용하는 것을 예시한 것이고, 특히 마지막에 a[ a % 2 == 0 ] 와 같이 부울린 인덱싱 표현식을 배열 인덱스안에 넣어 간단하게 표현할 수도 있다.
```
lst = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
a = np.array(lst)
 
# 배열 a 에 대해 짝수면 True, 홀수면 False 
bool_indexing = (a % 2 == 0)
print(bool_indexing)
# 출력: 부울린 인덱싱 배열
# [[False  True False]
#  [ True False  True]
#  [False  True False]]
```

```
[[False  True False]
 [ True False  True]
 [False  True False]]
```

```
# 부울린 인덱스를 사용하여 True인 요소만 뽑아냄
print(a[bool_indexing])
```

```
[2 4 6 8]
```


```
# 더 간단한 표현
n = a[ a % 2 == 0 ]
print(n)
```

```
[2 4 6 8]
```

