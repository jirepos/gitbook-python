# 정리중 




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




## References
[Xlwings Tutorial](https://www.dataquest.io/blog/python-excel-xlwings-tutorial/)      
