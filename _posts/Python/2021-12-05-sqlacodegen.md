---
title: "Python MySQL 테이블 ORM model.py 자동 작성"
categories: Python
tags: Sqlacodegen
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# Python MySQL 테이블 ORM model.py 자동 작성

## 배경

- FastAPI 프로젝트에 MySQL DB 테이블들을 SQLAlchemy에서 지원하는 ORM 방식의 모델로 매핑해야 하는 상황.
- 직접 models.py를 만들고 각 테이블에 해당하는 모델 클래스를 작성하여 CRUD를 해도 되지만 신규 테이블을 만들기 보다 기존 테이블을 사용해야하는 상황.
- MySQL Server에 존재하는 여러 테이블들을 하나 하나 클래스로 만들어주는 일이 굉장히 수고스러움.

<br>

## 방법

- sqlacodegen 라이브러리를 이용하여 기존 DB 테이블 정보를 바탕으로 테이블 각각에 대한 ORM 모델 클래스를 생성하는 스크립트를 자동으로 작성.

<br>

## 설치

```bash
pip install sqlacodegen
```

<br>

## ORM 모델 클래스 생성 스크립트 파일 추출

```bash
sqlacodegen mysql+pymysql://<user>:<password>@<host>:<port>/<db> > "./models.py"
```

이제 추출된 `models.py` 파일을 FastAPI 프로젝트 database 모듈에 복붙하여 CRUD를 하면 된다.