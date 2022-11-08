---
title: "NAVER API - 지오코딩(Geocoding)"
categories: python
tags: naverapi
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## 파이썬으로 지오코딩하기

주소의 텍스트를 입력받아 좌표를 포함한 상세정보들을 제공해주는 Geocoding 기능을 네이버 클라우드 플랫폼에서 제공해주고 있다. 최대 월 이용한도 3,000,000건이 무료로 제공된다. REST API 방식도 제공되므로, 파이썬으로 간단하게 Geocoding 기능을 이용할 수 있다. 

<br>

## 서비스 이용 신청하기

Geocoding API를 이용하기 위해서는 네이버 클라우드 플랫폼에 회원가입 후 결제수단을 등록해야한다. 이후 아래와 같이 [Maps API](https://www.ncloud.com/product/applicationService/maps) 에서 **이용 신청하기**를 눌러준다.

![PNG](/assets/img/post_img/2022-07-21-geocoding/0.png){: .align-center}

<br>

콘솔 창 하단의 **Application 등록**을 누르고, 이용약관을 살펴본 후 다음 절차를 진행한다.

![PNG](/assets/img/post_img/2022-07-21-geocoding/1.png){: .align-center}

<br>

애플리케이션 이름을 적어준다.

![PNG](/assets/img/post_img/2022-07-21-geocoding/2.png){: .align-center}

<br>

**Maps - Geocoding** 을 체크 후 **등록**을 누르면 애플리케이션 등록이 완료된다.

![PNG](/assets/img/post_img/2022-07-21-geocoding/3.png){: .align-center}

<br>

API의 당일 사용량, 당월 사용량 등을 확인할 수 있다. 아래 창에서 애플리케이션 이름 영역의 **인증 정보**을 누르면 **Client ID** 와 **Client Secret** 정보를 볼 수 있는데, REST API 요청 시 필요한 값들이므로 미리 복사해둔다.

![PNG](/assets/img/post_img/2022-07-21-geocoding/4.png){: .align-center}

<br>

## Geocoding API 요청하기

[Geocoding 개요](https://api.ncloud-docs.com/docs/ai-naver-mapsgeocoding) 에 소개된 내용을 바탕으로 아래와 같이 파이썬으로 Geocoding API를 요청하면 된다.

```python
import requests

# NCP 콘솔에서 복사한 클라이언트ID와 클라이언트Secret 값
client_id = ""
client_secret = ""

# 주소 텍스트
query = "서울특별시 강남구 강남대로 310"

endpoint = "https://naveropenapi.apigw.ntruss.com/map-geocode/v2/geocode"
url = f"{endpoint}?query={query}"

# 헤더
headers = {
    "X-NCP-APIGW-API-KEY-ID": client_id,
    "X-NCP-APIGW-API-KEY": client_secret,
}

# 요청
res = requests.get(url, headers=headers)
res.json()
```

```
{'status': 'OK',
 'meta': {'totalCount': 1, 'page': 1, 'count': 1},
 'addresses': [{'roadAddress': '서울특별시 강남구 강남대로 310 유니온센터오피스텔',
   'jibunAddress': '서울특별시 강남구 역삼동 837-11 유니온센터오피스텔',
   'englishAddress': '310, Gangnam-daero, Gangnam-gu, Seoul, Republic of Korea',
   'addressElements': [{'types': ['SIDO'],
     'longName': '서울특별시',
     'shortName': '서울특별시',
     'code': ''},
    {'types': ['SIGUGUN'], 'longName': '강남구', 'shortName': '강남구', 'code': ''},
    {'types': ['DONGMYUN'], 'longName': '역삼동', 'shortName': '역삼동', 'code': ''},
    {'types': ['RI'], 'longName': '', 'shortName': '', 'code': ''},
    {'types': ['ROAD_NAME'],
     'longName': '강남대로',
     'shortName': '강남대로',
     'code': ''},
    {'types': ['BUILDING_NUMBER'],
     'longName': '310',
     'shortName': '310',
     'code': ''},
    {'types': ['BUILDING_NAME'],
     'longName': '유니온센터오피스텔',
     'shortName': '유니온센터오피스텔',
     'code': ''},
    {'types': ['LAND_NUMBER'],
     'longName': '837-11',
     'shortName': '837-11',
     'code': ''},
    {'types': ['POSTAL_CODE'],
     'longName': '06253',
     'shortName': '06253',
     'code': ''}],
   'x': '127.0315025',
   'y': '37.4909898',
   'distance': 0.0}],
 'errorMessage': ''}
```