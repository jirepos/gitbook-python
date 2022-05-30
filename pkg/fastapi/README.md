# FastAPI

## 프로젝트 구조

```
📂project_rooot
   📂static
      📄start.html
   📂data      
      📄readme.txt
   📂templatese      
      📄test.html
   📂app
      📄sample.py
   📄main.py
```

* static 정적파일
* data 데이터 파일 
* templates 템플릿 
* Python pakage or module
* main.py:  entry point 

## 파일 읽기 
main.py가 파일경로의 시작이된다. 따라서 data 폴더의 readme.txt 파일을 읽으려면  다음과 경로를 설정한다. 
```python
# data/readme.txt
f = open(os.path.join("data/", "readme.txt"),  "r", encoding='utf-8')
```




## References
[Templates](https://fastapi.tiangolo.com/advanced/templates/)          
[Static Files](https://fastapi.tiangolo.com/tutorial/static-files/)     
[Requset](https://www.starlette.io/requests/)     
[Seperate Files](https://www.tutorialsbuddy.com/python-fastapi-bigger-applications-multiple-separate-files)     


