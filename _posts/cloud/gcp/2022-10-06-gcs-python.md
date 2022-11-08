---
title: Python GCS 연동하기
categories: gcp
tags: gcs
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

Python으로 GCS(Google Cloud Storage)에 접근하는 방법에 대해 알아보자.

<br>

## 내용

- GCP(Google Cloud Platform) 프로젝트 설정
    - 프로젝트 생성
    - 서비스 계정 생성
    - 서비스 계정 키 생성 후 JSON 파일 추출
    - 서비스 계정에 스토리지 관련 역할 추가


- Python과 GCS 연동
    - 라이브러리 설치
    - 서비스 계정 키 설정
    - GCS 클라이언트 정의
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
- config 폴더 생성 후 해당 경로에 JSON 파일 다운로드

<br>

### 서비스 계정에 GCS 관련 역할 추가

- GCP 좌측 상단 '탐색 메뉴' 클릭 후 'IAM 및 관리자'의 'IAM'으로 이동
- '추가' 클릭 - 생성된 서비스 계정 이메일 추가 및 '저장소 관리자' 역할 선택 후 저장

<br>

## Python과 GCS 연동

<br>

### 구글 클라우드 스토리지 클라이언트 설치

```bash
pip install google-cloud-storage
```

<br>

### 서비스 계정 키 설정


```python
from google.cloud import storage
from google.oauth2 import service_account
```


```python
# 서비스 계정 인증 정보가 담긴 JSON 파일 경로
KEY_PATH = "./config/key.json"
# Credentials 객체 생성
credentials = service_account.Credentials.from_service_account_file(KEY_PATH)
```

<br>

### GCS 클라이언트 정의


```python
# 구글 스토리지 클라이언트 객체 생성
client = storage.Client(credentials = credentials, project = credentials.project_id)
```

<br>

## 참고

- [구글 클라우드 스토리지 파이썬 클라이언트 공식문서](https://googleapis.dev/python/storage/latest/client.html)
