---
title: "Python openpyxl - DataFrame을 엑셀에 삽입하기"
categories: python
tags: openpyxl
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

Pandas로 직접 DataFrame을 엑셀 파일로 저장하는 기능을 제공해주기도 하지만, Openpyxl을 이용하면 자유도 높게 DataFrame을 엑셀 파일로 저장할 수 있다.

## 라이브러리 임포트하기

`Pandas`, `Openpyxl` 등 라이브러리를 임포트한다.

```python
import pandas as pd
import openpyxl
from openpyxl.utils.dataframe import dataframe_to_rows
```

임의의 DataFrame 객체를 만든다.

<br>

## 임의의 데이터프레임 만들기

`Pandas`를 이용해 임의의 데이터프레임을 만들어준다.

```python
# 임의의 데이터 프레임 생성
df = pd.DataFrame(
    {
        "col1": [1, 2, 3, None, 0],
        "col2": ["a", "b", "c", "d", "e"]
    }
)
df
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>a</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.0</td>
      <td>b</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.0</td>
      <td>c</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>d</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>e</td>
    </tr>
  </tbody>
</table>
</div>


<br>

## 통합 문서 시트에 데이터프레임 삽입하고 파일로 저장하기

통합 문서 객체를 만들고, 시트를 선택한 후 `dataframe_to_rows`를 이용해 시트에 DataFrame을 삽입한다. `index`를 `True`로 지정하면 DataFrame의 인덱스 값도 삽입할 수 있다. `header`를 `False`로 지정하면 컬럼명을 제외하고 값을 삽입할 수 있다.


```python
# 통합 문서 객체 생성
wb = openpyxl.Workbook()
# 시트 선택
ws = wb.active
# 시트에 데이터프레임 삽입
for r in dataframe_to_rows(df, index=False, header=True):
    ws.append(r)
# 엑셀 파일에 저장
wb.save("./test.xlsx")
```

![PNG](/assets/img/post_img/2022-09-25-openpyxl-pandas/1.png){: .align-center}

<br>

## 참고

- https://openpyxl.readthedocs.io/en/latest/pandas.html