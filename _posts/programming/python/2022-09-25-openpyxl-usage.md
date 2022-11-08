---
title: "Python openpyxl 간단한 사용방법"
categories: python
tags: openpyxl
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

openpyxl을 이용해 엑셀 통합 문서를 읽고, 쓰는 방법과 숫자 형식, 수식을 이용하는 방법을 알아보자. 또, 셀 병합 및 병합 해제 방법, 이미지 삽입 방법 그리고 행, 열 숨기기 방법도 살펴보자.

<br>

## 통합 문서 작성하기


```python
from openpyxl import Workbook
from openpyxl.utils import get_column_letter

# 통합 문서 객체 생성
wb = Workbook()
# 파일 저장 경로
dest_filename = "./output/empty_book.xlsx"
# 워크 시트
ws1 = wb.active
# 워크 시트 이름 변경
ws1.title = "range names"
# 행 1 ~ 39 순회
for row in range(1, 40):
    # 행에 0 ~ 599 입력
    ws1.append(range(600))

# 워크 시트 생성
ws2 = wb.create_sheet(title="Pi")
# F5 셀에 값 입력
ws2["F5"] = 3.14

# 워크 시트 생성
ws3 = wb.create_sheet(title="Data")
# 행 10 ~ 19 순회
for row in range(10, 20):
    # 열 27 ~ 53 순회
    for col in range(27, 54):
        # 행 및 열 번호로 셀 선택 후 열 이름을 셀의 값으로 입력
        _ = ws3.cell(column=col, row=row, value=f"{get_column_letter(col)}")

# 통합 문서 파일 저장
wb.save(filename = dest_filename)
```

<br>

## 통합 문서 읽기


```python
from openpyxl import load_workbook

# 통합 문서 열기
wb = load_workbook(filename = "./output/empty_book.xlsx")
# 시트 선택
sheet_ranges = wb["range names"]
# 셀 값 조회
sheet_ranges["D18"].value
```




    3



<br>

### 수식이 있는 셀에서 값을 불러올 경우

`data_only=True` 를 지정하여 통합 문서를 불러오면 수식이 있는 셀에서 수식이 아닌 값을 가져올 수 있다.


```python
wb = load_workbook("./output/test.xlsx", data_only=True)
ws = wb.active
ws['C1'].value
```




    40



<br>

### 수식이 있는 셀에서 수식을 불러올 경우

`data_only=False` 를 지정하여 통합 문서를 불러오면 수식이 있는 셀에서 값이 아닌 수식을 가져올 수 있다.


```python
wb = load_workbook("./output/test.xlsx", data_only=False)
ws = wb.active
ws['C1'].value
```




    '=SUM(A1:B1)'



<br>

## 숫자 형식 사용하기


```python
import datetime
from openpyxl import Workbook
wb = Workbook()
ws = wb.active
ws["A1"] = datetime.datetime(2022, 9, 25)
print(f"형식: {ws['A1'].number_format}, 값: {ws['A1'].value}")
```

    형식: yyyy-mm-dd h:mm:ss, 값: 2022-09-25 00:00:00
    

<br>

## 수식 사용하기

- **주의**: 수식 이름은 영문으로 작성해야 하며, 인수는 쉼표로 구분해야 함. 세미콜론과 같은 다른 구두점은 사용하지 않아야 함.


```python
from openpyxl import Workbook
wb = Workbook()
ws = wb.active
ws["A1"] = "=SUM(10, 20)"
print(f"형식: {ws['A1'].number_format}, 값: {ws['A1'].value}")
wb.save("formula.xlsx")
```

    형식: General, 값: =SUM(10, 20)
    

`FORMULAE` 를 통해 수식 이름을 확인할 수 있다.


```python
from openpyxl.utils import FORMULAE
"IF" in FORMULAE
```




    True



<br>

## 셀 병합


```python
from openpyxl.workbook import Workbook

wb = Workbook()
ws = wb.active

# 방법1
ws.merge_cells("A2:D2")
ws["A2"] = "셀 병합1"

# 방법2
ws.merge_cells(start_row=4, 
               start_column=1, 
               end_row=4, 
               end_column=4)
ws.cell(row=4, column=1, value="셀 병합2")

# 저장
wb.save("./output/merge.xlsx")
```

<br>

## 셀 병합 해제


```python
from openpyxl import load_workbook

wb = load_workbook("./output/merge.xlsx")
ws = wb.active

# 방법1
ws.unmerge_cells("A2:D2")

# 방법2
ws.unmerge_cells(start_row=4,
                 start_column=1,
                 end_row=4,
                 end_column=4)

# 저장
wb.save("./output/unmerge.xlsx")
```

<br>

## 이미지 삽입


```python
from openpyxl import Workbook
from openpyxl.drawing.image import Image

wb = Workbook()
ws = wb.active

# 이미지 불러오기
img = Image("./data/python-logo.png")
# 워크 시트의 셀에 이미지 삽입
ws.add_image(img, "B2")
# 저장
wb.save("./output/image.xlsx")
```

<br>

## 접기


```python
import openpyxl
wb = openpyxl.Workbook()
ws = wb.create_sheet()

# A ~ D 열 숨기기
ws.column_dimensions.group("A", "D", hidden=True)
# 1 ~ 10 행 숨기기
ws.row_dimensions.group(1, 10, hidden=True)
# 저장
wb.save("./output/group.xlsx")
```
