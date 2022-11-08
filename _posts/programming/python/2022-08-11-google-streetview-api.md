---
title: "Google API - 위경도 좌표로 스트리트뷰 사진 가져오기"
categories: python
tags: googleapi
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## Google Street View API

- 위경도 값으로 스트리트뷰 이미지 요청

```python
import requests
from PIL import Image
import io

# 위도, 경도
lat, lon = 48.857864549450355, 2.2951934711297723

# 관측 시야(Field Of View) - 최대 120 기본값 90
fov = "120"
# 방향 - 범위 0 ~ 360 (0 or 360::북, 180: 남)
heading = "-45"
# 카메라 상하 방향 설정 - 범위 -90 ~ 90 기본값 0
pitch = "30"

# 구글 API 키
google_api_key = "API키"

# URL
url = f"https://maps.googleapis.com/maps/api/streetview?size=400x300&location={lat},{lon}&fov={fov}&heading={heading}&pitch={pitch}&key={google_api_key}"

payload = {}
headers = {}
response = requests.request("GET", url, headers=headers, data=payload)

# 이미지 바이트 데이터
bytes_data = response.content

# 이미지 변환
img = Image.open(io.BytesIO(bytes_data))
img
```

![PNG](/assets/img/post_img/2022-08-11-google-streetview-api/1.png){: .align-center}

<br>


## Apply


```python
heading_list = list(range(-45, 360-45, 90))
img_list = []
for heading in heading_list:
    lat, lon = 48.857864549450355, 2.2951934711297723
    fov = "120"
    pitch = "30"
    google_api_key = "API키"
    url = f"https://maps.googleapis.com/maps/api/streetview?size=400x300&location={lat},{lon}&fov={fov}&heading={heading}&pitch={pitch}&key={google_api_key}"
    payload = {}
    headers = {}
    response = requests.request("GET", url, headers=headers, data=payload)
    bytes_data = response.content
    img = Image.open(io.BytesIO(bytes_data))
    img_list.append(img)
    
import matplotlib.pyplot as plt
%matplotlib inline

rows = 2
columns = 2


plt.figure(figsize=(10, 10))
for i in range(4):
    i += 1
    plt.subplot(rows, columns, i)
    plt.imshow(img_list[i-1])
    plt.xticks([])
    plt.yticks([])
    
plt.show()
```
    
![PNG](/assets/img/post_img/2022-08-11-google-streetview-api/2.png){: .align-center}
    
<br>

## References

- https://developers.google.com/maps/documentation/streetview/overview
- https://www.sphinfo.com/google-maps-platform-street-view-api/
