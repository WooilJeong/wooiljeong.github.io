---
title: Python BigQuery - 데이터 세트와 테이블 만들기
categories: gcp
tags: bigquery
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

파이썬을 이용해 빅쿼리 데이터 세트와 테이블을 생성할 수 있다.

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

## 데이터 세트 만들기

'프로젝트 이름.데이터 세트 이름' 형식으로 데이터 세트 ID를 입력한다. `bigquery.Dataset`을 이용해 데이터 세트 객체를 만들고, 위치를 서울(asia-northeast3)로 설정한다. 그리고 `client.create_dataset`을 이용해 빅쿼리에 데이터 세트 생성 요청을 보낸다.


```python
# 데이터 세트 ID - '프로젝트 이름.데이터 세트 이름' 형식으로 입력
dataset_id = "[프로젝트 이름].[데이터 세트 이름]"
# 데이터 세트 객체 생성
dataset = bigquery.Dataset(dataset_id)
# 데이터 세트 위치 설정 (서울)
dataset.location = "asia-northeast3"
# 데이터 세트 생성
dataset = client.create_dataset(dataset, timeout=30)
```

<br>

## 테이블 만들기

'프로젝트 이름.데이터 세트 이름.테이블 이름' 형식으로 테이블 ID를 입력한다. `bigquery.SchemaField`를 이용해 테이블의 필드 속성을 정의한다. 필드의 이름, 데이터 타입 그리고 `mode`는 값이 필수(REQUIRED)로 있어야 하는지 혹은 없어도(NULLABLE) 되는지를 의미한다. 테이블 ID와 정의한 스키마 객체를 `bigquery.Table`에 입력해 테이블 객체를 만든다. 그리고 `client.create_table`을 이용해 빅쿼리에 테이블 생성 요청을 보낸다.


```python
# 테이블 ID - '프로젝트 이름.데이터 세트 이름.테이블 이름' 형식으로 입력
table_id = "[프로젝트 이름].[데이터 세트 이름].[테이블 이름]"
# 스키마 객체 생성
schema = [
    bigquery.SchemaField("name", "STRING", mode="REQUIRED"),
    bigquery.SchemaField("age", "INTEGER", mode="NULLABLE"),
]
# 테이블 객체 생성
table = bigquery.Table(table_id, schema=schema)
# 테이블 생성
table = client.create_table(table)
```

<br>

## 참고

- [빅쿼리 가이드](https://cloud.google.com/bigquery/docs/introduction)
