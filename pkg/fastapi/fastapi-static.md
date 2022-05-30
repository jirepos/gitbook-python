# StaticFiles

StaticFiles를 사용하여 디렉터리로부터 정적파일들을 제공할 수 있다. 

프로젝트 루트 아래에 static 폴더를 만들고 그 아래 정적파일들을 둔다. 
```
📂project_rooot
   📂static
      📄start.html
```

```python
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles

app = FastAPI()

app.mount("/static", StaticFiles(directory="static"), name="static")
```

* "/static"은 sub-application이 마운트할 sub-path를 참조한다. 
* directory="static"은 디렉터리 이름을 참조한다. 
* name="static"은 FastAPI에 의해 내부적으로 사용할 이름을 부여한다. 


다음과 같이 호출한다. 
```
http://127.0.0.1:8000/static/start.html
```


## References
[Templates](https://fastapi.tiangolo.com/advanced/templates/)          
[Static Files](https://fastapi.tiangolo.com/tutorial/static-files/)     
[Requset](https://www.starlette.io/requests/)     



