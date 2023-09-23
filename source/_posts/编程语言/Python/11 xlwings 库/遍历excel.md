---
categories:
  - 编程语言
  - Python
  - 11 xlwings 库
---
# 遍历excel

```python
import openpyxl

wb = openpyxl.load_workbook('1.xlsx')

# 获取workbook中所有的表格
sheets = wb.sheetnames

print(sheets)

# 循环遍历所有sheet
for i in range(len(sheets)):
    sheet = wb[sheets[i]]

    print('\n\n第' + str(i + 1) + '个sheet: ' + sheet.title + '->>>')

    for r in range(1, sheet.max_row + 1):
        if r == 1:
            print('\n' + ''.join(
                [str(sheet.cell(row=r, column=c).value).ljust(17) for c in range(1, sheet.max_column + 1)]))
        else:
            print(''.join([str(sheet.cell(row=r, column=c).value).ljust(20) for c in range(1, sheet.max_column + 1)]))


```



