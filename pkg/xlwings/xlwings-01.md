# Python for Excel and Google Sheets

엑셀에서 Python을 사용하기 위해서 xlwings를 사용할 것이다.  xlwings를 사용하여 다음을 할 수 있다. 

* 엑셀과 상호 작용할 수 있다. 
* Python 코드로 VBA macros를 대체할 수 있다. 
* Python에서 사용자 정의 함수(UDF)를 작성할 수 있다. 


## 설치와 설정 

자세한 내용은 [Xlwings](https://www.xlwings.org/)을 참조한다. 

* [설치](https://docs.xlwings.org/en/stable/installation.html)     
* [설정](https://docs.xlwings.org/en/stable/addin.html)


## scratch로부터 프로젝트 생성 
scratch로 부터 프로젝트를 생성하려면 shell에서 다음을 실행한다. 

```
xlwings quickstart myproject
```

```
📂myproject
   📄myproject.py 
   📄myproject.xlsm 
```

myproject.py에는 다음과 같은 코드 스크래치가 생성된다. 
```python
import xlwings as xw


def main():
    wb = xw.Book.caller()
    sheet = wb.sheets[0]
    if sheet["A1"].value == "Hello xlwings!":
        sheet["A1"].value = "Bye xlwings!"
    else:
        sheet["A1"].value = "Hello xlwings!"


@xw.func
def hello(name):
    return f"Hello {name}!"


if __name__ == "__main__":
    xw.Book("myproject.xlsm").set_mock_caller()
    main()
```


myproject.xlsm 파일의 개발도구 Visual Basicdml Module1의 vb 코드에는 다음과 같은 코드가 자동으로 추가된다. 
```vb
Sub SampleCall()
    mymodule = Left(ThisWorkbook.Name, (InStrRev(ThisWorkbook.Name, ".", -1, vbTextCompare) - 1))
    RunPython "import " & mymodule & ";" & mymodule & ".main()"
End Sub
```

## python 코드 설명 
VB 코드에서 호출할 함수이다. 
```python
def main():
    wb = xw.Book.caller()
    sheet = wb.sheets[0]
    if sheet["A1"].value == "Hello xlwings!":
        sheet["A1"].value = "Bye xlwings!"
    else:
        sheet["A1"].value = "Hello xlwings!"
```        


맨 아래  __main__에서도 호출한다. 이 코드는 엑셀이 아닌 파이썬을 실행할 때 필요하다. 
```
if __name__ == "__main__":
    xw.Book("myproject.xlsm").set_mock_caller()
    main()
```    
**set_mock_caller**     
Python으로부터 코드가 호출 될 때  xw.Book.caller() mock하기 위해서 사용하는 Excel file을 설정한다. RunPython을 통하여 Excel로 부터 호출될 때 사용되지 않는다. 


**사용자 함수(UDF)**     
엑셀에서 사용자 함수로 사용하려면 @xw.func를 어노테이션을 사용한다. 
```python 
@xw.func
def hello(name):
    return f"Hello {name}!"
```

default addin settings는 다음을 기대한다. 
* Excel file과 같은 디렉토리에 있어야 하고 
* Excel file과 같은 이름이어야 하고,
* .py로 끝나야 한다. 



### vbcode 설명 
Microsoft Visual Basic for Application에서 모듈을 추가하가 그 아래 다음과 같이 Sub를 만든다. RunPython을 사용하여 module을 import하고 module의 함수를 호출한다. 
```python 
Sub SampleCall()
    mymodule = Left(ThisWorkbook.Name, (InStrRev(ThisWorkbook.Name, ".", -1, vbTextCompare) - 1))
    RunPython "import " & mymodule & ";" & mymodule & ".main()"
End Sub
```

RunPython으 함수를 호출안에 파라미터를 포함하는 것은 가능하지만 편리하지는 않다. RunPython은 반환값을 허용하지 않는다. 이것을 극복하기 위햇 UDF를 사용한다. 

> xlwings 리본 메뉴에 Run Main 버튼이 있는데 이 버튼을 클릭하면 Excel workbook과 같은 이름의 Python 모듈의 main() 함수를 실행한다. 이것은 SampleCall sub procedure가 없어도 호출한다. 



## RunPython 참조 에러 
스크래치로부터 프로젝트를 생성하지 않고 직접 생성하는 경우에 SampleCall 프로시저를 만들고 버튼에 매크로를 연결하여 실행하면 RunPython을 찾을 수 없다는 오류가 나온다. 이 경우는 VBA script에서 xlwings에 대한 참조가 없기 때문이다. 


* Visul Basic 창을 연다. Alt + F11도 가능 
* 도구 > 참조를 클릭한다. 
* xlwings를 체크한다. 

[Add-in & Settings](https://docs.xlwings.org/en/stable/addin.html)의 "Installation"을 참고한다. 






