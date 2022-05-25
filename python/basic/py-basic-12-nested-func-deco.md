# 중첩(nested) 함수와 Decorator

## 중첩함수

```python 
def outer():   # 바깥 함수 정의 
  def inner():   # 내부 함수 정의 
    print("inner")
  print("before calling inner()")
  inner() # 내부 함수 호출 

outer()  # 바깥 함수 호출 
```
```
before calling inner()
inner
```



```python 
def outer_a(a):
  def inner_a(b):
    print(b)
  print(a)   # a 파라미터를 출력하고 
  return inner_a    # 내부 함수 참조를 반환 

inner_a = outer_a("it's outer_a")   # outer 실행하고 반환값 inner_a를 받음 
inner_a("it's inner_a")  # inner_a를 호출 
```
```
it's outer_a
it's inner_a
```





## Decorator
Decorator는 중첩함수(Nested Function)을 리턴하는 함수만 decorator 함수를 사용할 수 있다. 

```python 
def abc(func):   # decorator로 사용할 함수 정의 , 인자로 함수를 받음 
  print("abc")
  def abc_inner():
    print("abc_inner")
    func()   # 전달된 func,즉 main이 호출된다. 
  return abc_inner   # abc_inner를 호출하면 안되고 그냥 반환한다. 


@abc   # 데코레이터로 abc 함수를 지정 
def main():
  print("main")


main()  # main을 호출하면 abc()에 main이 전달된다. 
```

```
abc
abc_inner
main
```



```python 
def disp(count):   #  count=2가 전달된다. 
  def disp_inner(func):  # main이 전달 
    print("count:", count)
    def wrapper():    # 중첩함수를 하나 더 사용해야 
      func()
    return wrapper 
  return disp_inner 



@disp(2)  # 2를 count로 전달 
def main2():
  print("main")


main2() # main은 disp_inner의 인자로 전닫된다.
```

```
count: 2
main
```

