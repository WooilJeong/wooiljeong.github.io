---
title: "NAVER API - 정적 지도(Static Map)"
categories: python
tags: naverapi
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## 파이썬으로 네이버 지도 시각화하기

네이버 클라우드 플랫폼의 **Static Map API**를 이용하면 파이썬으로 네이버 지도 시각화 이미지를 만들 수 있다. 최대 월 이용한도 3,000,000건이 무료로 제공된다. 

<br>

## 서비스 이용 신청하기

Static Map API를 이용하기 위해서는 네이버 클라우드 플랫폼에 회원가입 후 결제수단을 등록해야한다. 이후 아래와 같이 [Maps API](https://www.ncloud.com/product/applicationService/maps) 에서 **이용 신청하기**를 눌러준다.

![PNG](/assets/img/post_img/2022-07-21-static-map/0.png){: .align-center}

<br>

콘솔 창 하단의 **Application 등록**을 누르고, 이용약관을 살펴본 후 다음 절차를 진행한다.

![PNG](/assets/img/post_img/2022-07-21-static-map/1.png){: .align-center}

<br>

애플리케이션 이름을 적어준다.

![PNG](/assets/img/post_img/2022-07-21-static-map/2.png){: .align-center}

<br>

**Maps - Reverse Geocoding** 을 체크 후 **등록**을 누르면 애플리케이션 등록이 완료된다.

![PNG](/assets/img/post_img/2022-07-21-static-map/3.png){: .align-center}

<br>

API의 당일 사용량, 당월 사용량 등을 확인할 수 있다. 아래 창에서 애플리케이션 이름 영역의 **인증 정보**을 누르면 **Client ID** 와 **Client Secret** 정보를 볼 수 있는데, REST API 요청 시 필요한 값들이므로 미리 복사해둔다.

![PNG](/assets/img/post_img/2022-07-21-static-map/4.png){: .align-center}

<br>

## Static Map API 요청하기

[Static Map 개요](https://api.ncloud-docs.com/docs/ai-naver-mapsstaticmap)에 소개된 내용을 바탕으로 아래와 같이 파이썬으로 Static Map API를 요청하면 된다.

```python
from PIL import Image
import requests
import io

# NCP 콘솔에서 복사한 클라이언트ID와 클라이언트Secret 값
client_id = ""
client_secret = ""

# 좌표 (경도, 위도)
endpoint = "https://naveropenapi.apigw.ntruss.com/map-static/v2/raster"
headers = {
    "X-NCP-APIGW-API-KEY-ID": client_id,
    "X-NCP-APIGW-API-KEY": client_secret,
}
# 중심 좌표
lon, lat = "127.020326886309", "37.5164324582415"
_center = f"{lon},{lat}"
# 줌 레벨 - 0 ~ 20
_level = 16
# 가로 세로 크기 (픽셀)
_w, _h = 500, 300
# 지도 유형 - basic, traffic, satellite, satellite_base, terrain
_maptype = "satellite"
# 반환 이미지 형식 - jpg, jpeg, png8, png
_format = "png"
# 고해상도 디스펠레이 지원을 위한 옵션 - 1, 2
_scale = 1
# 마커
_markers = f"""type:d|size:mid|pos:{lon} {lat}|color:red"""
# 라벨 언어 설정 - ko, en, ja, zh
_lang = "ko"
# 대중교통 정보 노출 - Boolean
_public_transit = True
# 서비스에서 사용할 데이터 버전 파라미터 전달 CDN 캐시 무효화
_dataversion = ""

# URL
url = f"{endpoint}?center={_center}&level={_level}&w={_w}&h={_h}&maptype={_maptype}&format={_format}&scale={_scale}&markers={_markers}&lang={_lang}&public_transit={_public_transit}&dataversion={_dataversion}"
res = requests.get(url, headers=headers)

image_data = io.BytesIO(res.content)
image = Image.open(image_data)
image
```

![PNG](/assets/img/post_img/2022-07-21-static-map/5.png){: .align-center}