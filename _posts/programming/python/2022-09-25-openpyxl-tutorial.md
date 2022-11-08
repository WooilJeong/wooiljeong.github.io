---
title: "Python openpyxl 설치 및 사용법"
categories: python
tags: openpyxl
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 파이썬으로 엑셀 파일을 핸들링하는 방법

openpyxl은 엑셀 파일(.xlsx, .xlsm, .xltx, xltm 등)을 읽고 쓸 수 있는 기능을 제공하는 Python 라이브러리이다. Python Office Open XML 형식을 읽고 쓸 수 있는 라이브러리가 별로 없어서 만들어졌다고 한다. openpyxl을 설치하고 python으로 엑셀 통합 문서, 워크 시트 그리고 셀을 다루는 방법을 살펴보자. 작업을 완료한 뒤 통합 문서 객체를 파일이나 스트림 형태로 저장하는 방법도 알아보자.

<br>

## 설치

다음 명령으로 openpyxl을 설치한다.


```bash
pip install openpyxl
```

<br>

## 통합 문서(Work Book) 생성

엑셀 파일이 있어야만 openpyxl을 사용할 수 있는 것은 아니다. `Workbook` 클래스를 불러와 통합 문서(Work Book) 객체를 만들 수 있다.


```python
from openpyxl import Workbook
wb = Workbook()
```

통합 문서는 항상 하나 이상의 워크 시트로 생성된다. `Workbook.active` 속성을 이용해 워크 시트 객체를 만들 수 있다. 참고로 첫 번째 시트를 활성화시키는 것이 기본값으로 설정되어 있다.


```python
ws = wb.active
ws
```




    <Worksheet "Sheet">



`Workbook.create_sheet()` 메서드를 이용하면 새 워크 시트를 만들 수 있다.


```python
ws1 = wb.create_sheet("시트1")      # 끝에 삽입 (기본값)
ws2 = wb.create_sheet("시트2", 0)   # 첫 번째 위치에 삽입
ws3 = wb.create_sheet("시트3", -1)  # 끝에서 두 번째 위치에 삽입
```

시트 목록을 확인해보면 'Sheet'가 최초에 있었고, '시트1'이 맨 끝에 만들어 졌으며, '시트2'가 맨 첫 번째 위치에 만들어 진 후 '시트3'이 맨 끝에서 두 번째 위치에 만들어진 것을 알 수 있다.


```python
wb.sheetnames
```




    ['시트2', 'Sheet', '시트3', '시트1']



`Worksheet.title` 속성으로 워크 시트의 이름을 변경할 수 있다.


```python
ws.title = "새 시트"
wb.sheetnames
```




    ['시트2', '새 시트', '시트3', '시트1']



워크 시트의 이름이 적힌 탭의 배경색은 흰색이 기본값이다. `Worksheet.sheet_properties.tabColor` 속성에 `RRGGBB` 색상 코드를 입력하면 배경색을 변경할 수 있다.

참고로 색상 코드는 구글에 color hex code 같은 키워드로 검색하면 여러 색상의 코드를 찾을 수 있다.


```python
ws.sheet_properties.tabColor = "1072BA"
```

통합 문서 객체에서 시트명을 키값으로 하여 워크 시트를 가져올 수도 있다.


```python
ws = wb['새 시트']
ws
```




    <Worksheet "새 시트">



`Workbook.sheetname` 속성을 이용하면 통합 문서의 워크 시트 목록을 확인할 수 있다. (위에서 이미 이 속성을 이용해 워크 시트 목록을 확인해봤다.)


```python
wb.sheetnames
```




    ['시트2', '새 시트', '시트3', '시트1']



반복문을 이용해 워크 시트를 확인할 수도 있다.


```python
for sheet in wb:
    print(sheet.title)
```

    시트2
    새 시트
    시트3
    시트1
    

`Workbook.copy_worksheet()` 메서드를 통해 단일 통합 문서 안에서 워크 시트들의 복사본을 만들 수 있다.


```python
source = wb.active
target = wb.copy_worksheet(source)

print(f"원본 시트: {source.title}, 복사된 시트: {target.title}")
```

    원본 시트: 시트2, 복사된 시트: 시트2 Copy
    

다만, 값, 스타일, 하이퍼링크 그리고 코멘트를 포함한 셀과 차원, 형식 그리고 속성을 포함한 특정 워크 시트 속성만 복사할 수 있다. 이미지, 차트와 같은 다른 모든 통합 문서/워크 시트 속성은 복사되지 않는다.

또한 통합 문서 간에 워크 시트를 복사할 수 없다. 통합 문서가 읽기 전용 또는 쓰기 전용 모드로 열려 있으면 워크시트를 복사할 수 없다.

<br>

## 데이터 다루기

### 단일 셀 접근

워크 시트에 접근하는 방법을 알았다면, 이제 워크 시트 안의 셀 내용을 수정할 수 있다. 셀은 워크 시트의 키값으로 접근할 수 있다.


```python
c = ws['A4']
c
```




    <Cell '새 시트'.A4>



이렇게 하면 A4에 있는 셀이 반환되거나 아직 존재하지 않는 경우 새로 만든다. 값을 직접 할당할 수 있다.


```python
ws['A4'] = 4
```

`Worksheet.cell()` 메서드로 행 및 열 표기법을 이용해 셀에 접근할 수도 있다.


```python
d = ws.cell(row=4, column=2, value=10)
d
```




    <Cell '새 시트'.B4>



참고로 워크 시트가 메모리에 생성된 경우 셀이 포함되어 있지 않는다. 셀은 처음 접근될 때 만들어 진다. 이러한 이유로 다음과 같은 반복문을 이용해 셀에 직접 값을 할당하지 않고도 메모리에 셀을 생성할 수 있다. 아래 코드를 실행하면 100 x 100 크기의 빈 셀들을 메모리에 만들 수 있다.


```python
for x in range(1, 101):
    for y in range(1, 101):
        ws.cell(row=x, column=y)
```

<br>

### 다중 셀 접근

슬라이싱을 통해 셀 범위에 접근할 수 있다.


```python
cell_range = ws['A1':"C2"]
cell_range
```




    ((<Cell '새 시트'.A1>, <Cell '새 시트'.B1>, <Cell '새 시트'.C1>),
     (<Cell '새 시트'.A2>, <Cell '새 시트'.B2>, <Cell '새 시트'.C2>))



행 또는 열의 범위도 비슷한 방법으로 구할 수 있다.


```python
colC = ws['C']
col_range = ws['C:D']
row10 = ws[10]
row_range = ws[5:10]
```

`Worksheet.iter_rows()` 메서드를 이용할 수도 있다.


```python
for row in ws.iter_rows(min_row=1, max_col=3, max_row=2):
    for cell in row:
        print(cell)
```

    <Cell '새 시트'.A1>
    <Cell '새 시트'.B1>
    <Cell '새 시트'.C1>
    <Cell '새 시트'.A2>
    <Cell '새 시트'.B2>
    <Cell '새 시트'.C2>
    

같은 방법으로 `Worksheet.iter_cols()`를 통해 열도 가져올 수 있다.


```python
for col in ws.iter_cols(min_row=1, max_col=3, max_row=2):
    for cell in col:
        print(cell)
```

    <Cell '새 시트'.A1>
    <Cell '새 시트'.A2>
    <Cell '새 시트'.B1>
    <Cell '새 시트'.B2>
    <Cell '새 시트'.C1>
    <Cell '새 시트'.C2>
    

다만, 성능 상의 이유로 `Workbook.iter_cols()` 메서드는 읽기 전용 모드에서 사용할 수 없다.

파일의 모든 행이나 열을 반복해야 하는 경우 `Worksheet.rows` 속성을 대신 사용할 수 있다.


```python
ws = wb.active
ws['C9'] = "Hello World"
tuple(ws.rows)
```




    ((<Cell '시트2'.A1>, <Cell '시트2'.B1>, <Cell '시트2'.C1>),
     (<Cell '시트2'.A2>, <Cell '시트2'.B2>, <Cell '시트2'.C2>),
     (<Cell '시트2'.A3>, <Cell '시트2'.B3>, <Cell '시트2'.C3>),
     (<Cell '시트2'.A4>, <Cell '시트2'.B4>, <Cell '시트2'.C4>),
     (<Cell '시트2'.A5>, <Cell '시트2'.B5>, <Cell '시트2'.C5>),
     (<Cell '시트2'.A6>, <Cell '시트2'.B6>, <Cell '시트2'.C6>),
     (<Cell '시트2'.A7>, <Cell '시트2'.B7>, <Cell '시트2'.C7>),
     (<Cell '시트2'.A8>, <Cell '시트2'.B8>, <Cell '시트2'.C8>),
     (<Cell '시트2'.A9>, <Cell '시트2'.B9>, <Cell '시트2'.C9>))



혹은 `Worksheet.columns` 속성을 이용할 수도 있다.


```python
tuple(ws.columns)
```




    ((<Cell '시트2'.A1>,
      <Cell '시트2'.A2>,
      <Cell '시트2'.A3>,
      <Cell '시트2'.A4>,
      <Cell '시트2'.A5>,
      <Cell '시트2'.A6>,
      <Cell '시트2'.A7>,
      <Cell '시트2'.A8>,
      <Cell '시트2'.A9>),
     (<Cell '시트2'.B1>,
      <Cell '시트2'.B2>,
      <Cell '시트2'.B3>,
      <Cell '시트2'.B4>,
      <Cell '시트2'.B5>,
      <Cell '시트2'.B6>,
      <Cell '시트2'.B7>,
      <Cell '시트2'.B8>,
      <Cell '시트2'.B9>),
     (<Cell '시트2'.C1>,
      <Cell '시트2'.C2>,
      <Cell '시트2'.C3>,
      <Cell '시트2'.C4>,
      <Cell '시트2'.C5>,
      <Cell '시트2'.C6>,
      <Cell '시트2'.C7>,
      <Cell '시트2'.C8>,
      <Cell '시트2'.C9>))



다만, 성능상의 이유로 `Worksheet.columns` 속성은 읽기 전용 모드에서 사용할 수 없다.

<br>

### 셀의 값에만 접근

워크 시트의 값만 원하면 `Worksheet.values` 속성을 이용하면 된다. 워크 시트의 모든 행을 순회하지만 셀의 값만 출력한다.


```python
for row in ws.values:
    for value in row:
        print(value)
```

    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    None
    Hello World
    

`Worksheet.iter_rows()`와 `Worksheet.iter_cols()` 모두 `values_only` 파라미터를 취해 셀의 값만 반환할 수 있다.


```python
for row in ws.iter_rows(min_row=1, max_col=3, max_row=2, values_only=True):
    print(row)
```

    (None, None, None)
    (None, None, None)
    

<br>

## 엑셀 파일 읽기

`openpyxl.load_workbook()`을 통해 존재하는 통합 문서를 열 수 있다.


```python
from openpyxl import load_workbook
wb = load_workbook("document.xlsx")
wb.sheetnames
```




    ['Sheet']



<br>

## 엑셀 파일 저장하기

셀이 존재하면 값을 할당할 수 있다.


```python
c.value = "Hello, World"
print(c.value)
```

    Hello, World
    


```python
d.value = 3.14
print(d.value)
```

    3.14
    

<br>

### 파일로 저장

`Workbook.save()` 메서드를 이용하면 가장 쉽고 안전하게 통합 문서 객체를 저장할 수 있다. 다만, 이 작업은 별다른 경고 없이 기존 파일을 덮어쓰게 되므로 주의가 필요하다. 파일 이름 확장자는 반드시 `xlsx` 또는 `xlsm` 이어야 하는 것은 아니지만, 공식 확장자를 사용하지 않으면 다른 응용 프로그램에서 직접 여는 데 문제가 있을 수 있다.


```python
wb = Workbook()
wb.save("document.xlsx")
```

<br>

### 템플릿 파일로 저장

`template=True` 속성을 지정하면 문서를 템플릿으로 저장할 수 있다.


```python
from openpyxl import load_workbook
wb = load_workbook("document.xlsx")
wb.template = True
wb.save("template.xltx")
```

`template=False` 속성을 지정하면 템플릿을 문서로 저장할 수 있다.


```python
wb = load_workbook("template.xltx")
wb.template = False
wb.save("document.xlsx")
```

<br>

### 스트림으로 저장

FastAPI, Flask 그리고 Django와 같은 웹 프레임워크를 함께 사용하여 엑셀 통합 문서 객체를 스트림에 저장하고 싶은 경우 `io.BytesIO()`를 이용하면 된다.


```python
import io
stream = io.BytesIO()
wb.save(stream)
```

`io.BytesIO` 타입의 스트림 객체에 통합 문서를 저장했다면, `getvalue()` 메서드를 통해 다음과 같이 `Bytes` 형태의 값을 확인할 수 있다.


```python
type(stream.getvalue())
```




    bytes



<br>

## 참고

- [openpyxl docs](https://openpyxl.readthedocs.io/en/stable/tutorial.html)
