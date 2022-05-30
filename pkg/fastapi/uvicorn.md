# Uvicorn


## Install
```
$ pip install uvicorn[standard]
```


Application 생성한다. 

**example.py**     
```python 
from typing import Union
import uvicorn

from fastapi import FastAPI, Request
from fastapi.staticfiles import StaticFiles
from fastapi.templating import Jinja2Templates
from fastapi.templating import Jinja2Templates
from fastapi.responses import HTMLResponse
from app.sample import hello 

app = FastAPI()
```    
다음과 같이 실행한다. 
```
$ uvicorn example:app
```


* --reload flag whatchgod 를 사용한다. 
* example:app example 모듈의 app 객체 


## 프로그램적으로 실행하기 
```
import uvicorn

async def app(scope, receive, send):
    ...

if __name__ == "__main__":
    uvicorn.run("example:app", host="127.0.0.1", port=5000, log_level="info")
```




## References
[Uvicorn Home](https://www.uvicorn.org/)     
