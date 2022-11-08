---
title: Python BigQuery - 쿼리 결과를 테이블로 저장하기
categories: gcp
tags: bigquery
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

파이썬을 이용해 쿼리 결과를 빅쿼리의 새 테이블에 저장할 수 있다.

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

## 테이블 만들기

'프로젝트 이름.데이터 세트 이름.테이블 이름' 형식으로 테이블 ID를 입력한다.  `bigquery.QueryJobConfig`를 통해 쿼리 결과를 저장할 테이블을 설정해준다. 쿼리를 작성한 다음 `client.query`에 작성한 쿼리와 설정을 입력하여 쿼리 잡 객체를 생성한다. 쿼리 잡 객체 생성 후 `result()`를 호출하면 쿼리 결과가 새 테이블에 저장된다.

```python
# 테이블 ID - '프로젝트 이름.데이터 세트 이름.테이블 이름' 형식으로 입력
table_id = "[프로젝트 이름].[데이터 세트 이름].[테이블 이름]"
# 설정 객체 생성
job_config = bigquery.QueryJobConfig(destination=table_id)
# 쿼리 작성
query = f"""
SELECT *
FROM `[프로젝트 이름].[데이터 세트 이름].[테이블 이름]`
WHERE date BETWEEN '2022-01-01' AND '2022-12-31'
"""
# 쿼리 잡 객체 생성
query_job = client.query(query, job_config=job_config)
# 쿼리 잡 실행
query_job.result()
```

<br>

## 참고

- [빅쿼리 가이드](https://cloud.google.com/bigquery/docs/introduction)
