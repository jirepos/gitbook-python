# Templates 

Template를 사용하는 방법을 살펴본다. 

```
📂project_rooot
   📂static
     📄styles.css
   📂templates
      📄test.html
```

read_item 메서드를 main.py에 정의한다. Jinja2 "context"에서 key-value 쌍의 하나인 rquest를 전달하고 TemplateReponse를 반환한다. 

```python
# main.py
from fastapi import FastAPI, Request
from fastapi.responses import HTMLResponse
from fastapi.staticfiles import StaticFiles
from fastapi.templating import Jinja2Templates  

app = FastAPI()

app.mount("/static", StaticFiles(directory="static"), name="static")


templates = Jinja2Templates(directory="templates")

# TemplateResponse 반환 
@app.get("/items/{id}", response_class=HTMLResponse)
async def read_item(request: Request, id: str):
    return templates.TemplateResponse("item.html", {"request": request, "id": id})

```


## Templates and static files 
tempaltes 펄도 아래에 test.html을 다음과 같이 생성한다.  url_for()를 template 내부에 사용할 수 있다. 

이 파일은 당신이 전달한 "context" dict로부터 id를 취하여 표시할 것이다. 
```
{"request": request, "id": id}
```

**test.html**    
```html
<html>
<head>
    <title>Item Details</title>
    <link href="{{ url_for('static', path='/styles.css') }}" rel="stylesheet">
</head>
<body>
    <h1>Item ID: {{ id }}</h1>
</body>
</html>
```
### css file
static/styles.css 파일을 다음과 같이 생성한다. 
```css
h1 {
    color: green;
}
```

## References
[Templates](https://fastapi.tiangolo.com/advanced/templates/)          
[Static Files](https://fastapi.tiangolo.com/tutorial/static-files/)    
[Requset](https://www.starlette.io/requests/)      