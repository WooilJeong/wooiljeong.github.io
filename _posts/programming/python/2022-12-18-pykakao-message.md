---
title: "PyKakao - Python으로 카카오톡 메시지 API 사용하기"
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

## 카카오 로그인 관련 설정하기

1. [Kakao Developers](https://developers.kakao.com/)에 접속
2. [내 애플리케이션](https://developers.kakao.com/console/app) 선택 후 위에서 생성한 애플리케이션 선택
3. 내비게이션 메뉴에서 **카카오 로그인** 클릭 후 **활성화 설정**의 **상태 버튼**(OFF)을 클릭
4. 팝업 창에서 **활성화** 버튼 클릭
5. 카카오 로그인 화면 하단의 **Redirect URI 등록** 버튼 클릭
6. 팝업 창에서 **Redirect URI** 항목에 로컬 주소인 'https://localhost:5000' 입력 후 **저장** 버튼 클릭
7. 내비게이션 메뉴에서 **카카오 로그인** 하위의 **동의항목**을 클릭
8. 페이지 하단의 접근권한 이동 후 카카오톡 메시지 전송의 설정 클릭
9. 동의 단계를 이용 중 동의로 선택하고 동의 목적 작성 후 저장 버튼 클릭

<br>

## 카카오톡 메시지 API 사용하기

메시지 API는 사용자가 카카오톡 친구에게 카카오톡 메시지를 보내는 기능을 제공합니다. PyKakao의 최신 버전에서는 '나에게 보내기' 기능만 이용할 수 있습니다.

[자세히 보기](https://developers.kakao.com/docs/latest/ko/message/rest-api)


### 카카오 메시지 API 클래스 임포트하기

PyKakao 라이브러리의 `Message` 클래스를 임포트합니다. `Message`의 인자로 위에서 발급받은 '**REST API 키**'를 입력하여 `api` 인스턴스를 생성합니다.

```python
from PyKakao import Message
api = Message(service_key = "REST API 키")
```

<br>

### 카카오 인증코드 발급 URL 생성

카카오 메시지 API를 이용하려면 액세스 토큰이 필요합니다. 액세스 토큰을 생성하려면 인증코드가 필요합니다. 아래와 같이 `api.get_url_for_generating_code` 메서드를 실행하여 얻은 카카오 인증코드 발급 URL을 출력합니다. 웹 브라우저를 이용해 출력된 URL에 접속합니다.

```python
auth_url = api.get_url_for_generating_code()
print(auth_url)
```

<br>

### 카카오 인증코드 발급 URL 접속 후 리다이렉트된 URL

웹 브라우저에서 카카오 인증코드 발급 URL에 접속하면 페이지가 리다이렉트됩니다. 이 때, 리다이렉트된 주소를 웹 브라우저의 주소창에서 복사합니다. 복사한 리다이렉트된 URL을 `url`에 할당합니다.

```python
url = ""
```

<br>

### 위 URL로 액세스 토큰 추출

`api.get_access_token_by_redirected_url` 메서드의 인자로 위 `url`을 입력하면 액세스 코드를 얻을 수 있습니다. 액세스 코드의 값을 아래와 같이 `access_token`에 할당합니다.

```python
access_token = api.get_access_token_by_redirected_url(url)
```

<br>

### 액세스 토큰 설정

위에서 추출한 액세스 코드를 아래와 같이 설정합니다.

```python
api.set_access_token(access_token)
```

<br>

### 피드 메시지 전송

[자세히 보기](https://developers.kakao.com/docs/latest/ko/message/rest-api#default-template-msg-me)

```python
content = {
            "title": "오늘의 디저트",
            "description": "아메리카노, 빵, 케익",
            "image_url": "https://mud-kage.kakao.com/dn/NTmhS/btqfEUdFAUf/FjKzkZsnoeE4o19klTOVI1/openlink_640x640s.jpg",
            "image_width": 640,
            "image_height": 640,
            "link": {
                "web_url": "http://www.daum.net",
                "mobile_web_url": "http://m.daum.net",
                "android_execution_params": "contentId=100",
                "ios_execution_params": "contentId=100"
            }
        }

item_content = {
            "profile_text" :"Kakao",
            "profile_image_url" :"https://mud-kage.kakao.com/dn/Q2iNx/btqgeRgV54P/VLdBs9cvyn8BJXB3o7N8UK/kakaolink40_original.png",
            "title_image_url" : "https://mud-kage.kakao.com/dn/Q2iNx/btqgeRgV54P/VLdBs9cvyn8BJXB3o7N8UK/kakaolink40_original.png",
            "title_image_text" :"Cheese cake",
            "title_image_category" : "Cake",
            "items" : [
                {
                    "item" :"Cake1",
                    "item_op" : "1000원"
                },
                {
                    "item" :"Cake2",
                    "item_op" : "2000원"
                },
                {
                    "item" :"Cake3",
                    "item_op" : "3000원"
                },
                {
                    "item" :"Cake4",
                    "item_op" : "4000원"
                },
                {
                    "item" :"Cake5",
                    "item_op" : "5000원"
                }
            ],
            "sum" :"Total",
            "sum_op" : "15000원"
        }

social = {
            "like_count": 100,
            "comment_count": 200,
            "shared_count": 300,
            "view_count": 400,
            "subscriber_count": 500
        }

buttons = [
            {
                "title": "웹으로 이동",
                "link": {
                    "web_url": "http://www.daum.net",
                    "mobile_web_url": "http://m.daum.net"
                }
            },
            {
                "title": "앱으로 이동",
                "link": {
                    "android_execution_params": "contentId=100",
                    "ios_execution_params": "contentId=100"
                }
            }
        ]

api.send_feed(content=content, item_content=item_content, social=social, buttons=buttons)
```

<br>

### 리스트 메시지 전송

[자세히 보기](https://developers.kakao.com/docs/latest/ko/message/rest-api#default-template-msg-me)

```python
header_title = "WEEKELY MAGAZINE"
header_link = {
            "web_url": "http://www.daum.net",
            "mobile_web_url": "http://m.daum.net",
            "android_execution_params": "main",
            "ios_execution_params": "main"
        }
contents = [
            {
                "title": "자전거 라이더를 위한 공간",
                "description": "매거진",
                "image_url": "https://mud-kage.kakao.com/dn/QNvGY/btqfD0SKT9m/k4KUlb1m0dKPHxGV8WbIK1/openlink_640x640s.jpg",
                "image_width": 640,
                "image_height": 640,
                "link": {
                    "web_url": "http://www.daum.net/contents/1",
                    "mobile_web_url": "http://m.daum.net/contents/1",
                    "android_execution_params": "/contents/1",
                    "ios_execution_params": "/contents/1"
                }
            },
            {
                "title": "비쥬얼이 끝내주는 오레오 카푸치노",
                "description": "매거진",
                "image_url": "https://mud-kage.kakao.com/dn/boVWEm/btqfFGlOpJB/mKsq9z6U2Xpms3NztZgiD1/openlink_640x640s.jpg",
                "image_width": 640,
                "image_height": 640,
                "link": {
                    "web_url": "http://www.daum.net/contents/2",
                    "mobile_web_url": "http://m.daum.net/contents/2",
                    "android_execution_params": "/contents/2",
                    "ios_execution_params": "/contents/2"
                }
            },
            {
                "title": "감성이 가득한 분위기",
                "description": "매거진",
                "image_url": "https://mud-kage.kakao.com/dn/NTmhS/btqfEUdFAUf/FjKzkZsnoeE4o19klTOVI1/openlink_640x640s.jpg",
                "image_width": 640,
                "image_height": 640,
                "link": {
                    "web_url": "http://www.daum.net/contents/3",
                    "mobile_web_url": "http://m.daum.net/contents/3",
                    "android_execution_params": "/contents/3",
                    "ios_execution_params": "/contents/3"
                }
            }
        ]
buttons = [
            {
                "title": "웹으로 이동",
                "link": {
                    "web_url": "http://www.daum.net",
                    "mobile_web_url": "http://m.daum.net"
                }
            },
            {
                "title": "앱으로 이동",
                "link": {
                    "android_execution_params": "main",
                    "ios_execution_params": "main"
                }
            }
        ]


api.send_list(header_title=header_title, header_link=header_link, contents=contents, buttons=buttons)
```

<br>

### 위치 메시지 전송

[자세히 보기](https://developers.kakao.com/docs/latest/ko/message/rest-api#default-template-msg-me)

```python
address = "경기 성남시 분당구 판교역로 235 에이치스퀘어 N동 7층"
address_title = "카카오 판교오피스"
content = {
                "title": "카카오 판교오피스",
                "description": "카카오 판교오피스 위치입니다.",
                "image_url": "https://mud-kage.kakao.com/dn/drTdbB/bWYf06POFPf/owUHIt7K7NoGD0hrzFLeW0/kakaolink40_original.png",
                "image_width": 800,
                "image_height": 800,
                "link": {
                    "web_url": "https://developers.kakao.com",
                    "mobile_web_url": "https://developers.kakao.com/mobile",
                    "android_execution_params": "platform=android",
                    "ios_execution_params": "platform=ios"
                }
            }
buttons = [
                {
                    "title": "웹으로 보기",
                    "link": {
                        "web_url": "https://developers.kakao.com",
                        "mobile_web_url": "https://developers.kakao.com/mobile"
                    }
                }
            ]

api.send_location(address=address, address_title=address_title, content=content, buttons=buttons)
```
<br>

### 커머스 메시지 전송

[자세히 보기](https://developers.kakao.com/docs/latest/ko/message/rest-api#default-template-msg-me)

```python
content = {
            "title": "Ivory long dress (4 Color)",
            "image_url": "https://mud-kage.kakao.com/dn/RY8ZN/btqgOGzITp3/uCM1x2xu7GNfr7NS9QvEs0/kakaolink40_original.png",
            "image_width": 640,
            "image_height": 640,
            "link": {
                "web_url": "https://style.kakao.com/main/women/contentId=100",
                "mobile_web_url": "https://style.kakao.com/main/women/contentId=100",
                "android_execution_params": "contentId=100",
                "ios_execution_params": "contentId=100"
            }
        }

commerce = {
            "regular_price": 208800,
            "discount_price": 146160,
            "discount_rate": 30
        }

button = [
            {
                "title": "구매하기",
                "link": {
                    "web_url": "https://style.kakao.com/main/women/contentId=100/buy",
                    "mobile_web_url": "https://style.kakao.com/main/women/contentId=100/buy",
                    "android_execution_params": "contentId=100&buy=true",
                    "ios_execution_params": "contentId=100&buy=true"
                }
            },
            {
                "title": "공유하기",
                "link": {
                    "web_url": "https://style.kakao.com/main/women/contentId=100/share",
                    "mobile_web_url": "https://style.kakao.com/main/women/contentId=100/share",
                    "android_execution_params": "contentId=100&share=true",
                    "ios_execution_params": "contentId=100&share=true"
                }
            }
        ]

api.send_commerce(content=content, commerce=commerce, button=button)
```
<br>


### 텍스트 메시지 전송

[자세히 보기](https://developers.kakao.com/docs/latest/ko/message/rest-api#default-template-msg-me)

```python
text = "텍스트 영역입니다. 최대 200자 표시 가능합니다."
link = {
            "web_url": "https://developers.kakao.com",
            "mobile_web_url": "https://developers.kakao.com"
        }
button_title = "바로 확인"
api.send_text(text=text, link={}, button_title=button_title)
```

<br>

## 참고

- [PyKakao 깃허브 저장소](https://github.com/WooilJeong/PyKakao)
- [PyKakao 사용자 모임 (카카오톡)](https://open.kakao.com/o/gh1N1kJe)
- [PyKakao - Python으로 다음 검색 API 사용하기](https://wooiljeong.github.io/python/pykakao-daum/)
- [PyKakao - Python으로 카카오 KoGPT API 사용하기](https://wooiljeong.github.io/python/pykakao-kogpt/)
- [PyKakao - Python으로 카카오 Karlo API 사용하기](https://wooiljeong.github.io/python/pykakao-karlo/)
- [PyKakao - Python으로 카카오 로컬 API 사용하기](https://wooiljeong.github.io/python/pykakao-local/)
- [PyKakao - Python으로 카카오톡 메시지 API 사용하기](https://wooiljeong.github.io/python/pykakao-message/)