---
title: Python BigQuery - DataFrame을 테이블에 삽입하기
categories: gcp
tags: bigquery
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

파이썬을 이용해 판다스 데이터프레임을 빅쿼리 테이블에 삽입할 수 있다.

<br>

## 파이썬과 빅쿼리를 연동하기

파이썬으로 빅쿼리에 접근하기 위해서는 당연하게도 구글 클라우드 플랫폼에서 프로젝트를 만들어야 한다. 서비스 계정도 만들고 인증 정보를 파일로 내려받는 과정도 필요하다. 그리고 Python BigQuery 관련 라이브러리를 설치 후 사용하면 된다. 복잡해 보이지만 의외로 간단하다. Python과 BigQuery를 연동하는 방법은 [여기](https://wooiljeong.github.io/python/python-bigquery/)에 정리해두었으니 참고하면 된다.

<br>

## 빅쿼리 클라이언트 객체 만들기

아래와 같이 서비스 계정 인증 정보가 담긴 JSON 파일을 참조해 빅쿼리 클라이언트 객체를 만든다. 빅쿼리 클라이언트 객체를 이용해 빅쿼리에 접근할 수 있다.


```python
from google.cloud import bigquery
from google.oauth2 import service_account

# 서비스 계정 인증 정보가 담긴 JSON 파일 경로
KEY_PATH = "./key.json"
# Credentials 객체 생성
credentials = service_account.Credentials.from_service_account_file(KEY_PATH)
# 빅쿼리 클라이언트 객체 생성
client = bigquery.Client(credentials = credentials, project = credentials.project_id)
```

<br>

## 데이터프레임 만들기

빅쿼리에 빈 테이블이 미리 정의되어 있다고 가정한다. 빅쿼리에 빈 테이블을 생성하는 방법은 [여기](https://wooiljeong.github.io/gcp/bigquery-dataset-table/)에 정리해두었으니 참고하면 된다. 이 테이블에는 `name`과 `age` 필드가 존재한다. `Pandas`를 이용해 샘플 데이터를 만들어보자.


```python
import pandas as pd

df = pd.DataFrame(
    {
        "name": ["Jason", "Paul", "Tom"],
        "age": [20, 22, 24],
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
      <th>name</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Jason</td>
      <td>20</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Paul</td>
      <td>22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Tom</td>
      <td>24</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 데이터프레임을 테이블에 삽입하기

'프로젝트 이름.데이터 세트 이름. 테이블 이름' 형식으로 입력하여 테이블 ID를 만든다. `client.get_table`을 통해 빅쿼리 테이블 객체를 생성한다. 그리고 `client.load_table_from_dataframe`을 이용해 위에서 만든 데이터프레임 객체를 빅쿼리 테이블에 삽입하는 요청을 실행한다.

```python
# 테이블 ID
table_id = "[프로젝트 이름].[데이터 세트 이름].[테이블 이름]"
# 테이블 객체 생성
table = client.get_table(table_id)
# 데이터프레임을 테이블에 삽입
client.load_table_from_dataframe(df, table)
```




    LoadJob<project=*****, location=asia-northeast3, id=*****>



<br>

## 삽입된 데이터를 테이블에서 조회하기

빅쿼리 테이블에 데이터가 정상적으로 삽입되었는지 확인하기 위해 SQL로 데이터를 조회하는 쿼리를 작성한다. `client.query`를 통해 빅쿼리 테이블의 데이터를 조회한 후 `.to_dataframe`을 이용해 조회 결과를 데이터프레임 형식으로 변환한다.

```python
query = f"""
SELECT *
FROM `[프로젝트 이름].[데이터 세트 이름].[테이블 이름]`
"""
check = client.query(query).to_dataframe()
check
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
      <th>name</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Jason</td>
      <td>20</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Paul</td>
      <td>22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Tom</td>
      <td>24</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 참고

- [빅쿼리 가이드](https://cloud.google.com/bigquery/docs/introduction)
