# xlwings 기본 사용법

먼저 xlwings를 임포트한다. 

```python 
import xlwings as xw
```



## WorkBook 
인자없이 Book()을 호출하면 새로운 통합문서가 생성된다. 
```python 
# 새 통합문서를 만듭니다.
wb = xw.Book()
```


엑셀파일의 경로를 인수로 입력하면 해당 통합문서(Book) 객체에 대응하는 파이썬 객체가 생성된다
```python 
wb = xw.Book('data.xlsx)
```

## Sheet

### 시트 연결 
```python 
sht = wb.sheet[0] 
```
Sheet의 이름을 사용해도 된다. 
```python 
 ws = wb.sheets["Sheet1"]  # Sheet 이름으로 꺼냄 
```

활성화된 엑세을 연결합니다. 
```python
sht=xw.sheets.active
```


### 시트 생성 
```python 
wb = xw.Book('data.xlsx') # 워크북 연결 
sheet = wb.sheets.add() # 시트 생성
```

### 시트 삭제 
```python 
sheet = wb.sheets.add()
sheet.delete() # 시트 삭제 
```

### 시트 이름 구하기 
```python 
def listSheets():
    '''
    워크시트의 이름을 현재 시트에 쓴다. 
    '''
    wb = xw.Book.caller()
    sheet = wb.sheets[0]
    #return [s.name for s in wb.sheets]
    colName = "A"
    rowIndex = 2
    for s in wb.sheets:
        sheet[colName + str(rowIndex)].value = s.name
        rowIndex += 1
```        



## Range 
### Range 가져와서 값 설정 
A1 레인지를 선택하고 값을 설정한다.  A1 셀에 1이 입력된다. 
```python 
wb = xw.Book() # 새로운 워크북 생성 
sheet = wb.sheets[0]
sheet.range('A1').value = 1  # A1 선택 후 값 설정 
```
배열을 입력하면 A1만 값이 입력되는 게 아니라 B1에도 값이 입력된다. 
```python 
 sheet.range('A1').value = [1,2]   # A1을 선택하지만 값은 두 개이므로 오른쪽 B1에도  값이 입력된다. 
``` 
2차원 배열로 값을 설정하면 A1, B1, A2, B2에 값이 입력된다. 
```python 
sheet.range('A1').value = [ [1,2] ,[3,4]]
```

datetime으로 날짜 생성해서 값을 입력한다. 
```python
sheet.range('A1').value = [1, dt.date.today()] 
```
### Range 값 구하기 
range에 값을 설정하고 그 값을 확인해 본다. 
```
sheet.range('A1:C3').value = [ [1,2,3], [4,5,6], [7,8,9] ] # 값 입력 
a = sheet.range('A1').value 
b = sheet.range('A1:A3').value 
c = sheet.range('A1:C3').value 
print(a)  # 1.0 
print(b)  # [1.0, 4.0, 7.0]
print(c)  # [[1.0, 2.0, 3.0], [4.0, 5.0, 6.0], [7.0, 8.0, 9.0]]
```    
### expand 사용 
expand를 사용하여 연속된 범위의 데이터를 선택할 수 있다. 

**table**     
expand의 'table'을 전달하면 주어진 범위에서 오른쪽과 아래로 비어있는 셀 전까지 범위를 선택한다.  Excel에서는 ctrl + shift + down 하고 ctrl + shift + right 한다. 

```python
sheet.range('A1:C3').value = [ [1,2,3], [4,5,6], [7,8,9] ] # 값 입력 
r = sheet.range('A1').expand('table').value  # 테이블 형태로 값을 가져옴
# 값을 확인해보자 
print(r)    # [[1.0, 2.0, 3.0], [4.0, 5.0, 6.0], [7.0, 8.0, 9.0]]
```    

**down**    
이번에는 아래로 선택해보자.  
```python 
r = sheet.range('A1').expand('down').value  # 주어진 셀의 아래로 값을 가져옴
print(r) # [1.0, 4.0, 7.0]
```

**right**  
이번에는 오른쪽으로 선택해보자. 

```python
r = sheet.range('A1').expand('right').value  # 주어진 셀의 오른쪽으로 값을 가져옴
print(r) # [1.0, 2.0, 3.0]
```

주소는 address를 통하여 구할 수 있다. 
```python 
sheet.range('A1:C3').value = [ [1,2,3], [4,5,6], [7,8,9] ] # 값 입력 
print(sheet.range('A1').expand('right').address)   # $A$1:$C$1
```

### options 사용 
```python 
sheet.range('A1').options(expand='table').value
# [[1.0, 2.0, 3.0, 4.0],
# ['A', 'B', None, datetime.datetime(2020, 1, 6, 0, 0)],
# [11.0, 12.0, 13.0, 14.0]]
sheet.range('A1').options(expand='down').value
# [1.0, 'A', 11.0]
sheet.range('A1').options(expand='right').value
# [1.0, 2.0, 3.0, 4.0]
```


## pandas 데이터프레임으로 가져오기 

```python
wb = xw.Book("pandas.xlsx") # 새로운 워크북 생성 
sheet = wb.sheets[0]
sheet.range('A1').expand('table')
df = sheet.range('A1').options(pd.DataFrame, index=False,  expand='table').value  # pandas 데이터프레임 형태로 가져옴
print(df) 
```    

```
    Name   Age     City
0   Hong  25.0    Seoul
1   Park  35.0    Busan
2  Jeong  42.0  Gwangju
```

header 옵션도 있다 header를 False로 설정하면 헤더를 가져오지 않는다. 

```
sheet.range('A1').options(pd.DataFrame, index=False, header=False, expand='table').value
```


## References
[Xlwings Tutorial](https://www.dataquest.io/blog/python-excel-xlwings-tutorial/)      
