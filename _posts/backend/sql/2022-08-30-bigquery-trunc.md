---
title: "BigQuery - 빅쿼리에서 소수점 버림(TRUNC)하기"
categories: sql
tags: bigquery
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## BigQuery TRUNC 함수

32.16에 다음과 같이 TRUNC를 적용하여 소수점 아래 둘쨋자리까지 출력하게 하면 이상하게도 32.15가 출력된다.

```sql
SELECT TRUNC(32.16, 2) AS result;
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
      <th>result</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>32.1500</td>
    </tr>
  </tbody>
</table>
</div>

<br>

## NUMERIC으로 캐스팅한 후 TRUNC 함수 적용

32.16을 NUMERIC으로 캐스팅한 후 TRUNC를 적용하면 이상없이 잘 출력된다.

```sql
SELECT TRUNC(CAST(32.16 AS NUMERIC), 2) AS result;
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
      <th>result</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>32.160000000</td>
    </tr>
  </tbody>
</table>
</div>


<br>


## 문자열 필드 소수점 버림 TRUNC 하기

문자열 컬럼인 경우 아래와 같이 NUMERIC으로 캐스팅한 후 이 값에 TRUNC를 적용하면 된다.

```sql
with cte as (

  select "12" as area
  union all
  select "12.1" as area
  union all
  select "12.12" as area
  union all
  select "12.123" as area

) select cte.area, 
         SAFE_CAST(cte.area AS NUMERIC) as numArea,
         TRUNC(SAFE_CAST(cte.area AS NUMERIC), 2) as result,
from cte
;
```