---
title: Python GCS - Bucket 만들고 파일 업로드하기
categories: gcp
tags: gcs
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

파이썬을 이용해 구글 클라우드 스토리지 GCS에 버킷을 만들고 파일을 업로드할 수 있다. 

<br>

## 파이썬과 GCS 연동하기

파이썬으로 GCS에 접근하기 위해서는 당연하게도 구글 클라우드 플랫폼에서 프로젝트를 만들어야 한다. 서비스 계정도 만들고 인증 정보를 파일로 내려받는 과정도 필요하다. 그리고 Python GCS 관련 라이브러리를 설치 후 사용하면 된다. 복잡해 보이지만 의외로 간단하다. Python과 GCS를 연동하는 방법은 [여기](https://wooiljeong.github.io/gcp/gcs-python/)에 정리해두었으니 참고하면 된다.

<br>

## GCS 클라이언트 객체 만들기

아래와 같이 서비스 계정 인증 정보가 담긴 JSON 파일을 참조해 GCS 클라이언트 객체를 만든다. GCS 클라이언트 객체를 이용해 스토리지에 접근할 수 있다.


```python
from google.cloud import storage
from google.oauth2 import service_account

# 서비스 계정 인증 정보가 담긴 JSON 파일 경로
KEY_PATH = "./config/key.json"
# Credentials 객체 생성
credentials = service_account.Credentials.from_service_account_file(KEY_PATH)
# 구글 스토리지 클라이언트 객체 생성
client = storage.Client(credentials = credentials, project = credentials.project_id)
```

<br>

## Bucket 만들기

먼저 버킷 이름, 스토리지 클래스, 버킷 위치, 버킷 권한 그리고 버킷 내 객체 권한 등을 미리 설정한다. 버킷 이름은 기존에 누군가가 사용하지 않은 것으로 정의해야 한다. 스토리지 클래스는 [여기](https://cloud.google.com/storage/docs/storage-classes)에서 사용 목적에 맞는 유형을 찾아보면 되고, 버킷 위치의 경우 [여기](https://cloud.google.com/storage/docs/locations)를 참고하면 된다. 예제에서는 버킷 위치를 서울 리전을 사용하였지만, 무료 혜택을 주는 일부 위치도 존재하므로 앞의 링크를 잘 찾아보면 된다. 버킷과 버킷 내 객체 관련 권한 설정은 [여기](https://cloud.google.com/storage/docs/access-control/lists#predefined-acl)를 참고하면 된다. 아래 예제에서는 누구나 접근 가능하도록 설정하였는데, 민감한 데이터라면 다른 값으로 설정하면 된다. 그 밖에 스토리지 관련 [가격 책정](https://cloud.google.com/storage/pricing) 사항과 [무료 프로그램](https://cloud.google.com/free/docs/free-cloud-features#storage)도 참고하면 도움이 된다.


```python
# 버킷 이름
bucket_name = "test-bucket-abc-123"
# 스토리지 클래스 - ex. STANDARD: 표준
storage_class = "STANDARD"
# 버킷 위치 - ex. asia-northeast3: 서울
location = "asia-northeast3"
# 사전 정의된 ACL - ex. public-read 공개 읽기
predefined_acl = "public-read"
# 사전 정의된 객체 ACL - ex. public-read 공개 읽기
predefined_default_object_acl = "public-read"
```

<br>

버킷 생성에 필요한 설정을 마쳤으면, `client.bucket`으로 버킷 객체를 만든다. 스토리지 클래스 설정을 하고 `client.create_bucket`으로 버킷을 생성한다. 


```python

# 버킷 객체 생성
bucket = client.bucket(bucket_name)
# 스토리지 클래스 설정
bucket.storage_class = storage_class
# 버킷 생성
bucket = client.create_bucket(
    bucket,
    location=location,
    predefined_acl=predefined_acl,
    predefined_default_object_acl=predefined_default_object_acl,
)


# 버킷 ID
bucket.id
```

<br>

## Bucket에 파일 업로드하기

버킷에 파일을 업로드할 수 있는 방법은 아래 세 가지가 존재하는데, 여기에서는 파일 경로를 이용해 스토리지에 파일을 업로드할 수 있는 `blob.upload_from_filename`을 사용한다.

- blob.upload_from_file
- blob.upload_from_filename
- blob.upload_from_string


```python
# 버킷 이름
bucket_name = "test-bucket-abc-123"
# 블랍 이름
blob_name = "blob.csv"
# 적재할 파일 경로
file_path = "./data/test.csv"

# 버킷 선택
bucket = client.get_bucket(bucket_name)
# 블랍 객체 생성
blob = bucket.blob(blob_name)
# 파일 업로드
blob.upload_from_filename(file_path)
# 버킷에 업로드된 객체의 공개 URL
blob.public_url
```

<br>

## 참고

- [구글 클라우드 스토리지 파이썬 클라이언트 공식문서](https://googleapis.dev/python/storage/latest/client.html)
