---
title: "NAVER API - 리버스 지오코딩(Reverse Geocoding)"
categories: python
tags: naverapi
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## 파이썬으로 리버스 지오코딩하기

좌표를 통해 주소 정보(법정동, 행정동, 지번주소, 도로명주소 등)를 반환해주는 Reverse Geocoding 기능을 네이버 클라우드 플랫폼에서 제공해주고 있다. 최대 월 이용한도 3,000,000건이 무료로 제공된다. REST API 방식도 제공되므로, 파이썬으로 간단하게 Reverse Geocoding 기능을 이용할 수 있다. 

<br>

## 서비스 이용 신청하기

Reverse Geocoding API를 이용하기 위해서는 네이버 클라우드 플랫폼에 회원가입 후 결제수단을 등록해야한다. 이후 아래와 같이 [Maps API](https://www.ncloud.com/product/applicationService/maps) 에서 **이용 신청하기**를 눌러준다.

![PNG](/assets/img/post_img/2022-07-21-reverse-geocoding/0.png){: .align-center}

<br>

콘솔 창 하단의 **Application 등록**을 누르고, 이용약관을 살펴본 후 다음 절차를 진행한다.

![PNG](/assets/img/post_img/2022-07-21-reverse-geocoding/1.png){: .align-center}

<br>

애플리케이션 이름을 적어준다.

![PNG](/assets/img/post_img/2022-07-21-reverse-geocoding/2.png){: .align-center}

<br>

**Maps - Reverse Geocoding** 을 체크 후 **등록**을 누르면 애플리케이션 등록이 완료된다.

![PNG](/assets/img/post_img/2022-07-21-reverse-geocoding/3.png){: .align-center}

<br>

API의 당일 사용량, 당월 사용량 등을 확인할 수 있다. 아래 창에서 애플리케이션 이름 영역의 **인증 정보**을 누르면 **Client ID** 와 **Client Secret** 정보를 볼 수 있는데, REST API 요청 시 필요한 값들이므로 미리 복사해둔다.

![PNG](/assets/img/post_img/2022-07-21-reverse-geocoding/4.png){: .align-center}

<br>

## Reverse Geocoding API 요청하기

[Reverse Geocoding 개요](https://api.ncloud-docs.com/docs/ai-naver-mapsreversegeocoding) 에 소개된 내용을 바탕으로 아래와 같이 파이썬으로 Reverse Geocoding API를 요청하면 된다.

```python
import requests

# NCP 콘솔에서 복사한 클라이언트ID와 클라이언트Secret 값
client_id = ""
client_secret = ""

# 좌표 (경도, 위도)
coords = "126.969594,37.586541"
output = "json"
orders = 'addr'
endpoint = "https://naveropenapi.apigw.ntruss.com/map-reversegeocode/v2/gc"
url = f"{endpoint}?coords={coords}&output={output}&orders={orders}"

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
{'status': {'code': 0, 'name': 'ok', 'message': 'done'},
 'results': [{'name': 'addr',
   'code': {'id': '1111010100', 'type': 'L', 'mappingId': '09110101'},
   'region': {'area0': {'name': 'kr',
     'coords': {'center': {'crs': '', 'x': 0.0, 'y': 0.0}}},
    'area1': {'name': '서울특별시',
     'coords': {'center': {'crs': 'EPSG:4326',
       'x': 126.9783882,
       'y': 37.5666103}},
     'alias': '서울'},
    'area2': {'name': '종로구',
     'coords': {'center': {'crs': 'EPSG:4326',
       'x': 126.9788345,
       'y': 37.5735207}}},
    'area3': {'name': '청운동',
     'coords': {'center': {'crs': 'EPSG:4326',
       'x': 126.9692917,
       'y': 37.5891866}}},
    'area4': {'name': '',
     'coords': {'center': {'crs': '', 'x': 0.0, 'y': 0.0}}}},
   'land': {'type': '1',
    'number1': '50',
    'number2': '31',
    'addition0': {'type': '', 'value': ''},
    'addition1': {'type': '', 'value': ''},
    'addition2': {'type': '', 'value': ''},
    'addition3': {'type': '', 'value': ''},
    'addition4': {'type': '', 'value': ''},
    'coords': {'center': {'crs': '', 'x': 0.0, 'y': 0.0}}}}]}
```