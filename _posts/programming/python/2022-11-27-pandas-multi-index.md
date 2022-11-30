---
title: Pandas MultiIndex 컬럼 정렬 및 레벨 스왑 방법
categories: python
tags: pandas
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 판다스 멀티 인덱스 컬럼을 정렬하고 레벨 스왑하는 방법

보통 Pandas DataFrame을 다룰 때, 단일 레벨의 컬럼을 다루지만, 상황에 따라 다중 레벨로 이루어진 MultiIndex 컬럼을 다뤄야할 수도 있다. 이 때, sort_index()를 이용해 레벨 별로 컬럼의 순서를 정렬할 수 있고, swaplevel()을 이용해 레벨 간 컬럼의 위치를 서로 바꿀 수 있다.

<br>

## 라이브러리 임포트하기

실습에 사용할 Pandas와 NumPy를 임포트한다.

```python
import numpy as np
import pandas as pd
```

<br>

## 튜플 컬럼 리스트 생성

멀티 인덱스 컬럼을 만들기 위해 다음과 같은 튜플 리스트를 만들어 준다. 레벨1은 영문 컬럼이 되고, 레벨2는 숫자 컬럼으로 만든다.

```python
# 튜플 컬럼 리스트 생성
tuple_columns_list = [("A", "1"),
                      ("A", "2"),
                      ("A", "3"),
                      ("B", "1"),
                      ("B", "2"),
                      ("B", "3"),
                      ]
print(tuple_columns_list)
```

    [('A', '1'), ('A', '2'), ('A', '3'), ('B', '1'), ('B', '2'), ('B', '3')]
    

<br>

## 멀티 인덱스 컬럼 생성

pandas의 MultiIndex.from_tuples() 메서드를 이용해 위에서 정의한 튜플 리스트를 멀티 인덱스 객체로 만든다.

```python
# 멀티 인덱스 컬럼 생성
multi_index_columns = pd.MultiIndex.from_tuples(tuple_columns_list)
print(multi_index_columns)
```

    MultiIndex([('A', '1'),
                ('A', '2'),
                ('A', '3'),
                ('B', '1'),
                ('B', '2'),
                ('B', '3')],
               )
    

<br>

## 넘파이 배열 생성

넘파이를 이용해 다음과 같이 데이터 프레임에 들어갈 샘플 데이터를 만든다.

```python
# 넘파이 배열 생성
values = np.array([
    [200, 400, 800, 200, 400, 800],
    [300, 600, 1200, 300, 600, 1200],
])
print(values)
```

    [[ 200  400  800  200  400  800]
     [ 300  600 1200  300  600 1200]]
    

<br>

## 판다스 데이터 프레임 생성

pandas의 DataFrame() 메서드를 이용해 위에서 정의한 멀티 인덱스를 컬럼으로 지정하고, 넘파이 배열을 값으로 지정하여 데이터 프레임을 생성한다.

```python
# 판다스 데이터 프레임 생성
df = pd.DataFrame(data=values, columns=multi_index_columns)
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

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">A</th>
      <th colspan="3" halign="left">B</th>
    </tr>
    <tr>
      <th></th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>200</td>
      <td>400</td>
      <td>800</td>
      <td>200</td>
      <td>400</td>
      <td>800</td>
    </tr>
    <tr>
      <th>1</th>
      <td>300</td>
      <td>600</td>
      <td>1200</td>
      <td>300</td>
      <td>600</td>
      <td>1200</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 멀티 인덱스 컬럼 정렬

데이터 프레임의 sort_index() 속성을 이용하면 멀티 인덱스 컬럼의 순서를 정렬할 수 있다. axis 옵션을 1로 지정하는 것은 컬럼의 순서를 바꾸겠다는 의미이다. 그리고 level 옵션은 정렬할 컬럼의 목록을 의미한다. 여기서는 인덱스로 0과 1을 입력하였는데, 이는 각각 0번째 인덱스(영문 컬럼)와 1번째 인덱스(숫자 컬럼)를 의미한다.

### 레벨1 내림차순, 레벨2 오름차순 정렬

ascending은 앞에서 지정한 level의 순서와 매핑이 되며, 아래와 같이 False, True로 설정된 경우 0번 인덱스는 내림차순, 1번 인덱스는 오름차순으로 정렬하게 된다.

```python
# 멀티 인덱스 컬럼 정렬
df.sort_index(axis=1, level=[0, 1], ascending=[False, True])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">B</th>
      <th colspan="3" halign="left">A</th>
    </tr>
    <tr>
      <th></th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>200</td>
      <td>400</td>
      <td>800</td>
      <td>200</td>
      <td>400</td>
      <td>800</td>
    </tr>
    <tr>
      <th>1</th>
      <td>300</td>
      <td>600</td>
      <td>1200</td>
      <td>300</td>
      <td>600</td>
      <td>1200</td>
    </tr>
  </tbody>
</table>
</div>



<br>

### 레벨1 오름차순, 레벨2 내림차순 정렬

```python
# 멀티 인덱스 컬럼 정렬
df.sort_index(axis=1, level=[0, 1], ascending=[True, False])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">A</th>
      <th colspan="3" halign="left">B</th>
    </tr>
    <tr>
      <th></th>
      <th>3</th>
      <th>2</th>
      <th>1</th>
      <th>3</th>
      <th>2</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>800</td>
      <td>400</td>
      <td>200</td>
      <td>800</td>
      <td>400</td>
      <td>200</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1200</td>
      <td>600</td>
      <td>300</td>
      <td>1200</td>
      <td>600</td>
      <td>300</td>
    </tr>
  </tbody>
</table>
</div>



<br>

### 레벨1 내림차순, 레벨2 내림차순 정렬


```python
# 멀티 인덱스 컬럼 정렬
df.sort_index(axis=1, level=[0, 1], ascending=[False, False])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">B</th>
      <th colspan="3" halign="left">A</th>
    </tr>
    <tr>
      <th></th>
      <th>3</th>
      <th>2</th>
      <th>1</th>
      <th>3</th>
      <th>2</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>800</td>
      <td>400</td>
      <td>200</td>
      <td>800</td>
      <td>400</td>
      <td>200</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1200</td>
      <td>600</td>
      <td>300</td>
      <td>1200</td>
      <td>600</td>
      <td>300</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 멀티 인덱스 레벨 간 위치 변경

멀티 인덱스 컬럼의 레벨 내 순서를 정렬하는 것 외에도 레벨 간 순서를 변경하는 것도 가능하다. 데이터 프레임의 swaplevel() 속성을 이용하면 되는데, 이 때, 서로 위치를 변경할 인덱스들(0, 1)를 입력하고, axis 옵션을 1로 설정하면 된다.

### 레벨1과 레벨2 스왑

아래와 같이 명령을 실행하면 영문 컬럼과 숫자 컬럼의 위치가 서로 바뀐 것을 알 수 있다.

```python
df2 = df.swaplevel(0,1,axis=1)
df2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
    <tr>
      <th></th>
      <th>A</th>
      <th>A</th>
      <th>A</th>
      <th>B</th>
      <th>B</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>200</td>
      <td>400</td>
      <td>800</td>
      <td>200</td>
      <td>400</td>
      <td>800</td>
    </tr>
    <tr>
      <th>1</th>
      <td>300</td>
      <td>600</td>
      <td>1200</td>
      <td>300</td>
      <td>600</td>
      <td>1200</td>
    </tr>
  </tbody>
</table>
</div>



<br>

### 응용

위에서 살펴본 방법들을 응용하면 다음과 같이 멀티 인덱스 컬럼의 레벨 간 위치 변경 후 레벨 내 순서를 정렬할 수 있다.

```python
df2.sort_index(axis=1, level=[0,1], ascending=[True,True])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="2" halign="left">1</th>
      <th colspan="2" halign="left">2</th>
      <th colspan="2" halign="left">3</th>
    </tr>
    <tr>
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>A</th>
      <th>B</th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>200</td>
      <td>200</td>
      <td>400</td>
      <td>400</td>
      <td>800</td>
      <td>800</td>
    </tr>
    <tr>
      <th>1</th>
      <td>300</td>
      <td>300</td>
      <td>600</td>
      <td>600</td>
      <td>1200</td>
      <td>1200</td>
    </tr>
  </tbody>
</table>
</div>


