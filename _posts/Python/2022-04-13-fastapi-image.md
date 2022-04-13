---
title: "FastAPI - Pillow Image List 전달하기"
categories: Python
tags: FastAPI
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## FastAPI 이미지 전달 기능 만들기

pillow 이미지 객체를 bytes 형식으로 변환하고, base64를 이용해 인코딩 및 디코딩하는 과정을 거치면 API를 통해 여러 이미지 데이터를 리스트에 담아 보낼 수 있다.

```python
from fastapi import APIRouter
from fastapi.responses import JSONResponse
from PIL import Image
import requests
import base64
import io

router = APIRouter()

def from_image_to_bytes(img):
    """
    pillow image 객체를 bytes로 변환
    """
    # Pillow 이미지 객체를 Bytes로 변환
    imgByteArr = io.BytesIO()
    img.save(imgByteArr, format=img.format)
    imgByteArr = imgByteArr.getvalue()
    # Base64로 Bytes를 인코딩
    encoded = base64.b64encode(imgByteArr)
    # Base64로 ascii로 디코딩
    decoded = encoded.decode('ascii')
    return decoded

@router.get("/image/")
async def func():
    # 이미지 주소
    img_url = "https://dimg.donga.com/ugc/CDB/WEEKLY/Article/5b/b3/22/85/5bb32285000ed2738de6.jpg"
    # 이미지 불러오기
    img = Image.open(requests.get(img_url, stream=True).raw)
    # 이미지 데이터 타입 변환
    img_converted = from_image_to_bytes(img)
    # 이미지 리스트 생성
    img_list = [img_converted, img_converted]
    return JSONResponse(img_list)
```

<br>

## 이미지 요청하기

주피터 노트북을 열고 아래와 같이 이미지를 요청해본다.

```python
import io
import base64
import requests
from PIL import Image

# FastAPI URL
url = "http://localhost:5000/image"
# 요청
result = requests.get(url)
# 딕셔너리 변환
res = json.loads(result.content)
# 이미지 Bytes 리스트
bytes_list = list(map(lambda x: base64.b64decode(x), res))
# 이미지 Pillow Image 리스트
image_list = list(map(lambda x: Image.open(io.BytesIO(x)), bytes_list))
image_list[0]
```

![Untitled](FastAPI%20-%20%2027a0d/Untitled.png)
![PNG](/assets/img/post_img/2022-04-13-fastapi-image/img1.png){: .align-center}
