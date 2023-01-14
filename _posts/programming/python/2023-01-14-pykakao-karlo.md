---
title: "PyKakao - Python으로 카카오 Karlo API 사용하기"
categories: python
tags: PyKakao
header:
  overlay_image: /assets/img/logo/PyKakao.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# 카카오 API를 사용하기 위한 오픈소스 파이썬 라이브러리

<div align="center">
  <img src="https://github.com/WooilJeong/PyKakao/blob/main/assets/img/logo.png?raw=true" width="300"/>
</div>

<br>

## PyKakao

[**PyKakao**](https://github.com/WooilJeong/PyKakao) 라이브러리를 사용하면 [Kakao Developers](https://developers.kakao.com/)에서 제공하는 여러 종류의 카카오 API를 파이썬으로 쉽게 사용할 수 있습니다. 예를 들어, [Daum 검색 API](https://developers.kakao.com/docs/latest/ko/daum-search/dev-guide)를 이용해서 웹에서 정보를 검색할 수 있고, [메시지 API](https://developers.kakao.com/docs/latest/ko/message/rest-api)를 사용해서 카카오톡 메시지를 전송할 수 있습니다. 또한, [로컬 API](https://developers.kakao.com/docs/latest/ko/local/dev-guide)를 통해 주변 정보를 조회할 수 있고, [KoGPT API](https://developers.kakao.com/docs/latest/ko/kogpt/rest-api)와 [Karlo API](https://developers.kakao.com/docs/latest/ko/karlo/rest-api)를 이용해 자연어 처리를 하거나 생성형 인공지능으로 새로운 이미지를 만들어 볼 수도 있습니다.

- [PyKakao 깃허브 저장소](https://github.com/WooilJeong/PyKakao)
- [PyKakao 사용자 모임 (카카오톡)](https://open.kakao.com/o/gh1N1kJe)
- [PyKakao - Python으로 다음 검색 API 사용하기](https://wooiljeong.github.io/python/pykakao-daum/)
- [PyKakao - Python으로 카카오 KoGPT API 사용하기](https://wooiljeong.github.io/python/pykakao-kogpt/)
- [PyKakao - Python으로 카카오 Karlo API 사용하기](https://wooiljeong.github.io/python/pykakao-karlo/)
- [PyKakao - Python으로 카카오 로컬 API 사용하기](https://wooiljeong.github.io/python/pykakao-local/)
- [PyKakao - Python으로 카카오톡 메시지 API 사용하기](https://wooiljeong.github.io/python/pykakao-message/)


<br>

## PyKakao 설치하기

1. 운영체제(OS)에 따라 아래 중 하나를 선택합니다.

- Windows: CMD(명령 프롬프트) 실행
- Mac: Terminal(터미널) 실행

2. 아래 Shell 명령어를 입력 후 실행합니다.

```bash
pip install PyKakao --upgrade
```

<br>

## REST API 키 발급받기

PyKakao 라이브러리로 카카오 API를 사용하기 위해서는 [Kakao Developers](https://developers.kakao.com/)에 가입해야 합니다. 가입 후 로그인한 상태에서 상단 메뉴의 [내 애플리케이션](https://developers.kakao.com/console/app)을 선택합니다. '애플리케이션 추가하기'를 눌러 팝업창이 뜨면 '앱 이름', '사업자명'을 입력하고, 운영정책에 동의 후 '저장'을 선택합니다. 추가한 애플리케이션을 선택하면 '앱 키' 아래에 '**REST API 키**'가 생성된 것을 확인할 수 있습니다.

<br>

## 카카오 Karlo API 사용하기

Karlo API는 사용자가 입력한 문장과 이미지를 기반으로 새로운 이미지를 만드는 기능을 제공합니다. 생성형 인공지능(AI) Karlo는 1억 8천만 장 규모의 이미지-텍스트 학습을 통해 사용자가 묘사한 내용을 이해하고, 픽셀 단위로 완전히 새로운 이미지를 생성합니다. 또한 사용자가 원하는 콘셉트에 맞춰 창작 활동을 할 수 있도록 사물, 배경, 조명, 구도, 다양한 화풍을 지원합니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/karlo/common#intro)

<br>

### 카카오 Karlo API 클래스 임포트하기

PyKakao 라이브러리의 `Karlo` 클래스를 임포트합니다. `Karlo`의 인자로 위에서 발급받은 '**REST API 키**'를 입력하여 `api` 인스턴스를 생성합니다.

```python
from PyKakao import Karlo
api = Karlo(service_key = "REST API 키")
```
<br>

### 이미지 생성하기

먼저 텍스트 프롬프트를 기반으로 이미지를 생성하는 하는 방법에 대해 알아보겠습니다. "Cute magical flyng cat, soft golden fur, fantasy art drawn by Pixar concept artist, Toy Story main character, clear and bright eyes, sharp nose"와 같이 생성하고 싶은 이미지를 묘사하는 영문 텍스트를 text 변수에 할당합니다. 그리고 api.text_to_image(text, 1) 함수를 호출합니다. 여기서 두번째 인자 '1'은 생성할 이미지의 개수를 의미합니다.

함수 호출 결과는 생성된 이미지 정보가 들어 있는 딕셔너리 객체로 img_dict 변수에 저장됩니다. 이미지 데이터는 images 키의 리스트 첫번째 인덱스에 base64 인코딩된 문자열로 저장됩니다. 이 데이터는 img_str 변수에 저장됩니다.

마지막으로 api.string_to_image 함수를 사용해 base64 인코딩된 문자열을 이미지 객체로 변환합니다. base64_string 인자는 img_str 변수로 설정하고 mode 인자는 RGBA로 설정합니다. 이렇게 변환된 이미지 객체는 img 변수에 저장됩니다. 이 코드를 실행하면, 프롬프트에 제시된 텍스트를 기반으로 이미지가 생성되고, 이 이미지를 img 변수에 저장됩니다. img.save() 함수로 이미지를 파일로 저장할 수도 있습니다.

```python
# 프롬프트에 사용할 제시어
text = "Cute magical flyng cat, soft golden fur, fantasy art drawn by Pixar concept artist, Toy Story main character, clear and bright eyes, sharp nose"

# 이미지 생성하기 REST API 호출
img_dict = api.text_to_image(text, 1)

# 생성된 이미지 정보
img_str = img_dict.get("images")[0].get('image')

# base64 string을 이미지로 변환
img = api.string_to_image(base64_string = img_str, mode = 'RGBA')

# 이미지 저장하기
img.save("./original.png")
```

<div align="center">
    <figure>
      <img src="https://github.com/WooilJeong/PyKakao/blob/main/assets/img/example_karlo_cat.png?raw=true" width="350" /><br>
    </figure>
</div>

<br>

### 이미지 변환하기

Karlo API를 이용하면 기존 이미지를 다른 형태로 변환할 수도 있습니다. 앞에서 파일로 저장한 이미지('./original.png')를 불러와 img 변수에 저장합니다. 

api.image_to_string(img) 라는 함수를 사용하여 img변수에 저장된 이미지를 Base64 인코딩 합니다. 인코딩된 이미지는 img_base64라는 변수에 저장됩니다. 

api.transform_image(image = img_base64, batch_size=1)라는 함수를 사용하여 인코딩된 이미지를 변환합니다. 이 함수는 img_base64변수에 저장된 이미지를 인자로 받으며, batch_size=1 인자는 한번에 변환할 이미지의 개수를 의미합니다. 함수를 호출하면 변환된 이미지를 포함하는 응답이 반환됩니다. 

마지막으로 api.string_to_image(response.get("images")[0].get("image"), mode = 'RGB')라는 함수를 사용하여 응답에서 첫번째 이미지를 추출합니다. 이 함수는 response.get("images")[0].get("image")라는 변수에 저장된 이미지를 인자로 받으며, mode='RGB'는 이미지의 색상 모드를 RGB로 설정합니다. 이 함수는 이미지 객체를 반환하며, 이 객체를 result 변수에 저장합니다.

```python
# 이미지 파일 불러오기
img = Image.open('./original.png')

# 이미지를 Base64 인코딩하기
img_base64 = api.image_to_string(img)

# 이미지 변환하기 REST API 호출
response = api.transform_image(image = img_base64, batch_size=1)

# 응답의 첫 번째 이미지 생성 결과 출력하기
result = api.string_to_image(response.get("images")[0].get("image"), mode = 'RGB')
```

<div align="center">

<table>
<thead>
<tr>
<th style="text-align:center">원본 이미지</th>
<th style="text-align:center">변환된 이미지 </th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center"><img src="https://github.com/WooilJeong/PyKakao/blob/main/assets/img/example_karlo_cat.png?raw=true" width="250" /></td>
<td style="text-align:center"><img src="https://github.com/WooilJeong/PyKakao/blob/main/assets/img/example_karlo_cat_transformed.png?raw=true" width="250" /></td>
</tr>
</tbody>
</table>

</div>

<br>

### 이미지 편집하기

기존 이미지의 특정 부분을 마스킹(검정색으로 표시)하고 텍스트 프롬프트 정보도 함께 입력하여 이미지를 수정할 수도 있습니다. Image.open('./original.png')라는 함수를 사용하여 './original.png' 경로에 있는 원본 이미지 파일을 불러옵니다. 그리고 Image.open('./mask.png')라는 함수를 사용하여 './mask.png' 경로에 있는 mask 이미지 파일을 불러옵니다. mask 이미지 파일은 이미지 편집 툴을 이용해 직접 만들어야 합니다. 이미지 파일은 img와 mask라는 변수에 저장됩니다.

api.image_to_string(img) 라는 함수를 사용하여 img변수에 저장된 원본 이미지를 Base64 인코딩 합니다. 같은 방식으로 api.image_to_string(mask) 라는 함수를 사용하여 mask변수에 저장된 mask 이미지를 Base64 인코딩 합니다. 인코딩된 원본 이미지와 mask 이미지는 img_base64와 mask_base64라는 변수에 저장됩니다.

다음으로, api.inpaint_image(image = img_base64, mask = mask_base64, text = text, batch_size = 1) 라는 함수를 사용하여 원본 이미지와 mask 이미지를 이용하여 inpainting(이미지 편집)을 합니다. 이 함수는 image = img_base64, mask = mask_base64, text = text, batch_size = 1 라는 인자를 사용합니다. 이 함수를 호출하면 편집된 이미지를 담고 있는 결과가 반환됩니다.

마지막으로 api.string_to_image(response.get("images")[0].get("image"), mode = 'RGB')라는 함수를 사용하여 결과에서 첫번째 이미지를 추출합니다. 이 함수는 이미지 객체를 반환하며, 이 객체를 result 변수에 저장합니다.

```python
# 이미지 파일 불러오기
img = Image.open('./original.png')
mask = Image.open('./mask.png')

# 이미지를 Base64 인코딩하기
img_base64 = api.image_to_string(img)
mask_base64 = api.image_to_string(mask)

# 프롬프트에 사용할 제시어
text = "whatever"

# 이미지 변환하기 REST API 호출
response = api.inpaint_image(image = img_base64, mask = mask_base64, text = text, batch_size = 1)

# 응답의 첫 번째 이미지 생성 결과 출력하기
result = api.string_to_image(response.get("images")[0].get("image"), mode = 'RGB')
```

<div align="center">

<table>
<thead>
<tr>
<th style="text-align:center">원본 이미지</th>
<th style="text-align:center">마스킹한 이미지</th>
<th style="text-align:center">편집된 이미지</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center"><img src="https://github.com/WooilJeong/PyKakao/blob/main/assets/img/example_karlo_cat.png?raw=true" width="350" /></td>
<td style="text-align:center"><img src="https://github.com/WooilJeong/PyKakao/blob/main/assets/img/example_karlo_mask.png?raw=true" width="350" /></td>
<td style="text-align:center"><img src="https://github.com/WooilJeong/PyKakao/blob/main/assets/img/example_karlo_inpaint.png?raw=true" width="350" /></td>
</tr>
</tbody>
</table>

</div>

<br>

## 참고

- [PyKakao 깃허브 저장소](https://github.com/WooilJeong/PyKakao)
- [PyKakao 사용자 모임 (카카오톡)](https://open.kakao.com/o/gh1N1kJe)
- [PyKakao - Python으로 다음 검색 API 사용하기](https://wooiljeong.github.io/python/pykakao-daum/)
- [PyKakao - Python으로 카카오 KoGPT API 사용하기](https://wooiljeong.github.io/python/pykakao-kogpt/)
- [PyKakao - Python으로 카카오 Karlo API 사용하기](https://wooiljeong.github.io/python/pykakao-karlo/)
- [PyKakao - Python으로 카카오 로컬 API 사용하기](https://wooiljeong.github.io/python/pykakao-local/)
- [PyKakao - Python으로 카카오톡 메시지 API 사용하기](https://wooiljeong.github.io/python/pykakao-message/)