---
title: "파이썬(Python)에서 빅쿼리(BigQuery) 데이터 불러오기"
categories: Python
tags: Bigquery
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

> # 빅쿼리 데이터를 파이썬 데이터프레임으로 불러오기

<br>

## 내용

- GCP(Google Cloud Platform) 프로젝트 설정
    - 프로젝트 생성
    - 서비스 계정 생성
    - 서비스 계정 키 생성 후 JSON 파일 추출
    - 서비스 계정에 빅쿼리 관련 역할 추가


- Python과 BigQuery 연동
    - 라이브러리 설치
    - 서비스 계정 키 설정
    - GCP 클라이언트 정의
    - 데이터 조회 쿼리 실행
    
<br>

## GCP(Google Cloud Platform) 프로젝트 설정

- [GCP(Google Cloud Platform)](https://console.cloud.google.com/)

<br>

### 프로젝트 생성

- GCP 접속 후 좌측 상단 'Google Cloud Platform' 로고 우측의 '프로젝트 선택' 메뉴 클릭
- '새 프로젝트' 클릭 후 프로젝트 만들기 진행

<br>

### 서비스 계정 생성

- GCP 좌측 상단 '탐색 메뉴' 클릭 후 'IAM 및 관리자'의 '서비스 계정'으로 이동
- '+ 서비스 계정 만들기' 클릭 후 서비스 계정 만들기 진행

<br>

### 서비스 계정 키 생성 후 JSON 파일 추출

- 생성된 서비스 계정 클릭 후 '키' 탭 이동
- '키 추가' - '새 키 만들기' - 키 유형 'JSON' 선택 - '만들기' 클릭

<br>

### 서비스 계정에 빅쿼리 관련 역할 추가

- GCP 좌측 상단 '탐색 메뉴' 클릭 후 'IAM 및 관리자'의 'IAM'으로 이동
- '추가' 클릭 - 생성된 서비스 계정 이메일 추가 및 'BigQuery 관리자' 역할 선택 후 저장

<br>

## Python과 BigQuery 연동

<br>

### 라이브러리 설치

```bash
pip install google-cloud-bigquery
```

<br>

### 서비스 계정 키 설정


```python
from google.cloud import bigquery
from google.oauth2 import service_account

# 서비스 계정 키 JSON 파일 경로
key_path = "./new-project.json"

# Credentials 객체 생성
credentials = service_account.Credentials.from_service_account_file(key_path)
```

<br>

### GCP 클라이언트 정의


```python
# GCP 클라이언트 객체 생성
client = bigquery.Client(credentials = credentials, 
                         project = credentials.project_id)
```

<br>

### 데이터 조회 쿼리 실행


```python
# 데이터 조회 쿼리
sql = f"""
SELECT
  name, gender,
  SUM(number) AS total
FROM
  `bigquery-public-data.usa_names.usa_1910_2013`
GROUP BY
  name, gender
ORDER BY
  total DESC
LIMIT
  10
"""

# 데이터 조회 쿼리 실행 결과
query_job = client.query(sql)

# 데이터프레임 변환
df = query_job.to_dataframe()
df.head()
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
      <th>gender</th>
      <th>total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>James</td>
      <td>M</td>
      <td>4924235</td>
    </tr>
    <tr>
      <th>1</th>
      <td>John</td>
      <td>M</td>
      <td>4818746</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Robert</td>
      <td>M</td>
      <td>4703680</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Michael</td>
      <td>M</td>
      <td>4280040</td>
    </tr>
    <tr>
      <th>4</th>
      <td>William</td>
      <td>M</td>
      <td>3811998</td>
    </tr>
  </tbody>
</table>
</div>


<br>


## BigQuery 공식문서

https://cloud.google.com/bigquery/docs?hl=ko#docs
