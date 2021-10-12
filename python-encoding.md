# 파이썬에서 한글

파이썬에서 파일 쓰기를 하거나 크롤링을 할 때 한글이 깨지는 문제가 있다. 파이썬이 기본 UTF-8이 아니라서 비 영어권 사용자들은 코딩할 때 별도의 옵션을 제공해야 한다. 인코딩 타입을 지정하면 해결할 수 있다.

```python
file=open(fileName, 'w', encoding='utf-8')
```

크롤링하다가 다음과 같은 에러를 만날 수도 있다.

```
UnicodeEncodeError: 'cp949' codec can't encode character ...
```

코드 상단에 다음과 같이 코드를 추가하면 해결할 수 있다.

```python
import sys 
import io 
sys.stdout = io.TextIOWrapper(sys.stdout.detach(), encoding="utf-8")
sys.stderr = io.TextIOWrapper(sys.stderr.detach(), encoding="utf-8")
```
