---
title: "ChatGPT API Python 사용법 (feat.DALL-E, Karlo)"
categories: python
tags: ChatGPT, PyKakao, DALL-E, OpenAI
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# ChatGPT API Python 사용법 (feat.DALL-E, Karlo)

파이썬으로 쉽게 챗GPT API를 사용할 수 있습니다. 먼저, 챗GPT API의 사용법을 간단히 알아보고, 이미지 생성을 위해 필요한 프롬프트를 자동으로 작성하는 방법을 설명합니다. 이렇게 작성한 프롬프트를 활용하여 DALL-E와 Karlo API를 사용하여 이미지를 생성하는 방법도 다뤄보겠습니다.


<br>

## ChatGPT가 묘사하고 Karlo, DALL-E가 그린 그림

> ### "화성에 동물이 살았었다면 어떤 모습이었을지 묘사해주세요."


<br>

### ChatGPT의 묘사


화성에 동물이 살았다면, 그들은 높은 산까지 올라갈 수 있는 긴 다리와 끝없이 넓은 광활한 평원을 달릴 수 있는 빠른 속도를 지닌 몸체를 가졌을 것입니다. 그들은 빨간 모래 위에 깃들어 앉거나 선명한 청록색 바위의 기세로 서있는 모습이 특징적일 것입니다. 어떤 종류의 동물이었는지에 따라 꼬리에서 뾰족한 가시들이나 확대된 눈이나 귀, 멋진 뿔들 등의 특징을 가지고 있을 수 있습니다. 당신이 상상할 수 있는 어떠한 형태의 화성 동물이더라도, 화성에 존재하는 풍경과 잘 어울렸을 것입니다.

좀 더 구체적인 묘사를 드리자면,화성에 서식하는 동물 중 하나는 몸무게가 가벼운 편의 동물입니다. 그들은 높은 산에서 수평면까지 내려다볼 수 있는 능력을 가진 길고 가늘게 뻗은 다리를 가지고 있습니다. 그들의 발바닥에는 거친 질감의 부드러운 구강(쿠션)이 있어, 길고 다리를 뻗어 높은 땅에서 뛰어내리더라도 충격을 흡수할 수 있습니다. 그들은 깡통 모양의 몸통, 뾰족하고 긴 꼬리, 그리고 뿔 같은 뾰족한 귀를 가지고 있습니다. 눈은 크고 확대되어 있으며, 대부분의 시야를 커버하기 위해 동공이 높이 위치해 있습니다. 그들은 높은 산봉우리에 얹혀 살고 있으며 재래식 옹이로 구멍을 뚫어 해당 구멍 안에서 살거나 다른 동물과 함께 논다. 그들의 몸은 흰색과 청록색, 갈색, 검은색 등의 색상으로 뒤얽히며, 청록색이 강조되는 경향이 있습니다.

<br>

### Karlo가 그린 그림

![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/img2.png){: width="80%" .align-center}

<br>

### DALL-E가 그린 그림

![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/img3.png){: width="80%" .align-center}

<br>

## ChatGPT API란?

챗GPT(ChatGPT) API는 2023년 3월에 출시된 오픈AI의 새로운 서비스입니다. 이 API를 사용하면 챗GPT를 앱이나 제품에 쉽게 통합할 수 있습니다. 물론 여전히 [챗GPT 애플리케이션](https://chat.openai.com/chat)에 직접 접속해서도 사용할 수 있습니다. 

<br>

## 필요한 라이브러리 설치하기

- openai

'openai'는 오픈AI에서 제공하는 ChatGPT API (ChatCompletions API)를 파이썬으로 사용할 수 있도록 해주는 라이브러리입니다. ChatGPT API 외에도 DALL-E와 같은 이미지 생성 API도 이용할 수 있습니다.


- PyKakao

'PyKakao'는 카카오 디벨로퍼스에서 제공하는 다양한 API를 파이썬으로 사용할 수 있도록 해주는 라이브러리입니다. 입력한 문장을 이미지로 만드는 기능을 제공하는 인공지능 Karlo API를 사용할 것입니다.


- 설치 방법

명령 프롬프트(CMD)나 터미널에서 다음 명령어를 입력해 'openai'와 'PyKakao'를 설치합니다.

```bash
pip install openai
pip install PyKakao
```


<br>

## API 키 발급받기

<br>

### 오픈AI API 키 발급받기

[오픈AI 플랫폼](https://platform.openai.com/)에 접속해 회원가입 후 로그인합니다. 화면 우측 상단의 '프로필' -'View API Keys'를 클릭합니다.

![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/ex1.png){: width="80%" .align-center}

'Create new secret key' 버튼을 클릭합니다.

![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/ex2.png){: width="80%" .align-center}


초록색 '복사' 버튼을 클릭해 인증키를 복사하고 'OK' 버튼을 클릭합니다.

![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/ex3.png){: width="80%" .align-center}


<br>

### 카카오 API 키 발급받기

PyKakao 라이브러리로 카카오 API를 사용하기 위해서는 Kakao Developers에 가입해야 합니다. 가입 후 로그인한 상태에서 상단 메뉴의 내 애플리케이션을 선택합니다. ‘애플리케이션 추가하기’를 눌러 팝업창이 뜨면 ‘앱 이름’, ‘사업자명’을 입력하고, 운영정책에 동의 후 ‘저장’을 선택합니다. 추가한 애플리케이션을 선택하면 ‘앱 키’ 아래에 ‘REST API 키‘가 생성된 것을 확인할 수 있습니다.


<br>

## ChatGPT API 사용 방법


openai 라이브러리를 불러와 ChatGPT API를 사용하는 방법을 알아보겠습니다. ChatGPT API를 이용해 파이썬으로 ChatGPT에게 질문하고 응답을 받을 수 있습니다.

<br>

### openai 불러오기

오픈AI에서 제공하는 ChatGPT API를 파이썬으로 이용하기 위해 앞에서 설치한 'openai' 라이브러리를 불러옵니다. 오픈AI에서 발급받은 API 키 값을 'OPENAI_API_KEY' 변수에 할당합니다. 그리고 'openai.api_key'에 이 값을 할당합니다.


```python
import openai

# 발급받은 API 키 설정
OPENAI_API_KEY = "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"

# openai API 키 인증
openai.api_key = OPENAI_API_KEY
```

<br>

### ChatGPT API 사용하기

오픈AI의 ChatGPT API를 사용해 자연어로 작성된 질문에 대한 대답을 생성해보겠습니다.

- **model**: 'gpt-3.5-turbo' 모델을 설정합니다. 이 모델은 ChatGPT 애플리케이션에서 사용되는 것과 같은 언어 모델로 1000 토큰당 0.002달러로 기존 GPT-3.5 모델보다 10배 저렴하다고 합니다. 

- **query**: 질문을 입력합니다. 예를 들어, 텍스트로 이미지를 생성하는 방법 등을 물어볼 수 있습니다.

- **messages**: 생성하려는 대화의 히스토리를 정의하는데 사용됩니다. 대화에 참여하는 여러 **역할**('system(시스템)', 'assistant(도우미)', 'user(사용자)')과 **메시지 내용**을 설정할 수 있습니다. 보통 대화는 먼저 시스템 메시지로 형식을 정의합니다. 그 다음 사용자와 도우미의 메시지를 번갈아가며 정의합니다. 
    - **시스템 메시지**: 'You are a helpful assistant.'와 같은 메시지로 도우미에게 지시할 수 있습니다. 시스템이 챗봇에게 일종의 역할을 부여한다고 볼 수 있습니다. 
    - **사용자 메시지**: 도우미에게 직접 전달하는 내용입니다. 
    - **도우미 메시지**: 이전에 응답했던 결과를 저장해 대화의 흐름을 유지할 수 있도록 설정할 수 있습니다.

openai.ChatCompletion.create()에 위에서 정의한 파라미터를 입력하고 'response'에 값을 할당합니다. 'response['choices'][0]['message']['content']'를 호출하면 응답 메시지를 확인할 수 있습니다. 응답결과를 'answer'에 할당 후 출력합니다.


```python
# 모델 - GPT 3.5 Turbo 선택
model = "gpt-3.5-turbo"

# 질문 작성하기
query = "텍스트를 입력받아 이미지를 생성하는 방법을 알려주세요."

# 메시지 설정하기
messages = [
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": query}
]

# ChatGPT API 호출하기
response = openai.ChatCompletion.create(
    model=model,
    messages=messages
)
answer = response['choices'][0]['message']['content']
answer
```




    '텍스트를 입력받아 이미지를 생성하는 방법에는 여러 가지가 있습니다. 일반적으로는 다음과 같은 방법을 사용합니다.\n\n1. 소프트웨어를 사용하여 이미지 생성하기 - 예를 들어, Adobe Photoshop, GIMP, CorelDRAW 등의 소프트웨어를 사용하여 텍스트를 입력하고 다양한 폰트, 색상 및 배경을 선택하여 이미지를 만들 수 있습니다.\n\n2. 온라인 이미지 생성기 사용하기 - 여러 가지 온라인 이미지 생성기가 있습니다. 예를 들어, Canva, PicMonkey, Stencil 등의 이미지 생성기를 사용하여 텍스트를 입력하고 다양한 효과 및 배경을 선택하여 이미지를 만들 수 있습니다.\n\n3. Python 프로그래밍 언어를 사용하여 이미지 생성하기 - Python의 PIL 라이브러리를 사용하여 텍스트를 입력하고 이미지 스타일을 지정하여 이미지를 생성할 수 있습니다. 이를 위해서는 Python을 설치하고, PIL 라이브러리를 다운로드하고 설치해야 합니다.\n\n각각의 방법은 서로 다른 장단점이 있으므로, 텍스트 이미지 생성에 필요한 기능과 유용성을 고려하여 적절한 방법을 선택하시면 됩니다.'



<br>

## Karlo API 사용 방법

PyKakao 라이브러리를 불러와 Karlo API를 사용하는 방법을 알아보겠습니다. Karlo API를 이용해 텍스트를 이미지로 변환하는 기능을 구현할 수 있습니다. Karlo API에 대해 더 자세한 내용을 확인하려면 아래를 참고하면 됩니다.


**Karlo API와 관련된 더 많은 정보**

- [GitHub 저장소 - PyKakao](https://github.com/WooilJeong/PyKakao)
- [PyKakao - Python으로 카카오 Karlo API 사용하기](https://wooiljeong.github.io/python/pykakao-karlo/)
- [공식문서 - Kakao Developers - Karlo](https://developers.kakao.com/docs/latest/ko/karlo/common)

<br>

### PyKakao에서 Karlo 불러오기

카카오에서 제공하는 Karlo API를 파이썬으로 이용하기 위해 앞에서 설치한 'PyKakao' 라이브러리를 불러옵니다. 카카오 디벨로퍼스에서 발급받은 REST API 키 값을 'KAKAO_API_KEY' 변수에 할당합니다. 이 값을 'Karlo'에 입력해 Karlo API 인스턴스 'karlo'를 생성합니다.


```python
from PyKakao import Karlo

# 발급받은 API 키 설정
KAKAO_API_KEY = "4518aedfd6cd5c6d99a2d2c66ee4e2db"

# Karlo API 인스턴스 생성
karlo = Karlo(service_key = KAKAO_API_KEY)
```

<br>

### Karlo로 이미지 생성하기

텍스트 프롬프트를 기반으로 이미지를 생성하는 하는 방법에 대해 알아보겠습니다. “Cute magical flyng cat, soft golden fur, fantasy art drawn by Pixar concept artist, Toy Story main character, clear and bright eyes, sharp nose”와 같이 생성하고 싶은 이미지를 묘사하는 영문 텍스트를 'text'에 할당합니다. 그리고 'api.text_to_image(text, 1)' 함수를 호출합니다. 여기서 두번째 인자 ‘1’은 생성할 이미지의 갯수를 의미합니다.

함수 호출 결과는 생성된 이미지 정보가 들어 있는 딕셔너리 객체로 'img_dict' 변수에 저장됩니다. 이미지 데이터는 'images' 키의 리스트 첫번째 인덱스에 base64 인코딩된 문자열로 저장됩니다. 이 데이터는 'img_str' 변수에 저장됩니다.

'api.string_to_image' 함수를 사용해 base64 인코딩된 문자열을 이미지 객체로 변환합니다. base64_string 인자는 'img_str' 변수로 설정하고 'mode' 인자는 'RGBA'로 설정합니다. 이렇게 변환된 이미지 객체는 'img' 변수에 저장됩니다. 이 코드를 실행하면, 프롬프트에 제시된 텍스트를 기반으로 이미지가 생성되고, 이 이미지를 'img' 변수에 저장합니다. 'img.save()' 함수로 이미지를 파일로 저장할 수도 있습니다.


```python
# 프롬프트에 사용할 제시어
text = "Cute magical flyng cat, soft golden fur, fantasy art drawn by Pixar concept artist, Toy Story main character, clear and bright eyes, sharp nose"

# 이미지 생성하기 REST API 호출
img_dict = karlo.text_to_image(text, 1)

# 생성된 이미지 정보
img_str = img_dict.get("images")[0].get('image')

# base64 string을 이미지로 변환
img = karlo.string_to_image(base64_string = img_str, mode = 'RGBA')
img
```




    
![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/img1.png){: width="80%" .align-center}
    



<br>

## ChatGPT X Karlo X DALL-E 이미지 생성하기

챗GPT API를 이용해 이미지 생성에 필요한 프롬프트를 자동으로 작성하고, 이 프롬프트를 카카오의 Karlo와 오픈AI의 DALL-E에게 입력해 그림을 그리는 방법을 살펴보겠습니다.

<br>

### 라이브러리 불러오기

필요한 라이브러리를 불러오고 인증키를 설정합니다.


```python
import openai
from PyKakao import Karlo

# 발급받은 API 키 설정
OPENAI_API_KEY = "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"
KAKAO_API_KEY = "4518aedfd6cd5c6d99a2d2c66ee4e2db"

# openai API 키 인증
openai.api_key = OPENAI_API_KEY

# Karlo API 인스턴스 생성
karlo = Karlo(service_key = KAKAO_API_KEY)

# 모델 - GPT 3.5 Turbo 선택
model = "gpt-3.5-turbo"
```

<br>

### 챗GPT에게 묘사 요청하기

system 메시지로 assistant가 창의적인 묘사를 도와주도록 지시합니다. 그리고 묘사를 요청하는 질의를 작성합니다. 챗GPT API를 호출한 첫 번째 결과를 'answer'에 할당합니다. 이 값은 뒤에서 assistant의 메시지로 할당합니다.


```python
# 질문 작성하기
query = "화성에 동물이 살았었다면 어떤 모습이었을지 묘사해주세요."

# 메시지 설정하기
messages = [
        {"role": "system", "content": "You are an assistant who helps with a rich imagination. You respond with visual descriptions, not abstract ones."},
        {"role": "user", "content": query}
]

# ChatGPT API 호출하기
response = openai.ChatCompletion.create(
    model=model,
    messages=messages
)
answer = response['choices'][0]['message']['content']
answer
```




    '화성에 동물이 살았다면, 그들은 높은 산까지 올라갈 수 있는 긴 다리와 끝없이 넓은 광활한 평원을 달릴 수 있는 빠른 속도를 지닌 몸체를 가졌을 것입니다. 그들은 빨간 모래 위에 깃들어 앉거나 선명한 청록색 바위의 기세로 서있는 모습이 특징적일 것입니다. 어떤 종류의 동물이었는지에 따라 꼬리에서 뾰족한 가시들이나 확대된 눈이나 귀, 멋진 뿔들 등의 특징을 가지고 있을 수 있습니다. 당신이 상상할 수 있는 어떠한 형태의 화성 동물이더라도, 화성에 존재하는 풍경과 잘 어울렸을 것입니다.'



<br>

### 묘사 구체화 요청하기

앞에서 챗GPT가 응답한 결과를 다시 assistant의 메시지로 할당합니다. 그리고 더 구체적인 묘사를 요청하는 메시지를 추가합니다. 챗GPT API 응답 결과를 이번에는 'answer2'에 할당합니다. 


```python
# ChatGPT 답변 메시지 추가
messages.append(
    {
    "role": "assistant",
    "content": answer,
    }
)

# 사용자 메시지 추가
messages.append(
    {
    "role": "user",
    "content": "그 동물의 외형을 더 구체적으로 묘사해주세요.",
    },
)


# ChatGPT API 호출하기
response = openai.ChatCompletion.create(
    model=model,
    messages=messages
)
answer2 = response['choices'][0]['message']['content']
answer2
```




    '좀 더 구체적인 묘사를 드리자면,\n\n화성에 서식하는 동물 중 하나는 몸무게가 가벼운 편의 동물입니다. 그들은 높은 산에서 수평면까지 내려다볼 수 있는 능력을 가진 길고 가늘게 뻗은 다리를 가지고 있습니다. 그들의 발바닥에는 거친 질감의 부드러운 구강(쿠션)이 있어, 길고 다리를 뻗어 높은 땅에서 뛰어내리더라도 충격을 흡수할 수 있습니다. 그들은 깡통 모양의 몸통, 뾰족하고 긴 꼬리, 그리고 뿔 같은 뾰족한 귀를 가지고 있습니다. 눈은 크고 확대되어 있으며, 대부분의 시야를 커버하기 위해 동공이 높이 위치해 있습니다. 그들은 높은 산봉우리에 얹혀 살고 있으며 재래식 옹이로 구멍을 뚫어 해당 구멍 안에서 살거나 다른 동물과 함께 논다. 그들의 몸은 흰색과 청록색, 갈색, 검은색 등의 색상으로 뒤얽히며, 청록색이 강조되는 경향이 있습니다.'



<br>

### 영문으로 번역 요청하기

더 구체화된 응답을 이번에는 영문으로 번역 요청합니다. 이 경우에도 앞의 응답 결과인 'answer2'를 assistant의 메시지로 설정합니다.


```python
# ChatGPT 답변 메시지 추가
messages.append(
    {
    "role": "assistant",
    "content": answer2,
    }
)

# 사용자 메시지 추가
messages.append(
    {
    "role": "user",
    "content": "위 문장을 영문으로 번역해주세요.",
    },
)


# ChatGPT API 호출하기
response = openai.ChatCompletion.create(
    model=model,
    messages=messages
)
answer3 = response['choices'][0]['message']['content']
answer3
```




    'To provide a more concrete description, an animal that lived on Mars would have had long, slender legs capable of scaling high mountains and running across vast, wide-open plains at impressive speeds. They would have been characterized by their ability to perch atop red sand dunes or stand with the power of sharp teal-colored rocks. Depending on the particular species, they might have had spiky protrusions along their tails or enlarged eyes, ears, or splendid horns. Whatever form the Martian animal may take in your imagination, it would have fit in well with the surrounding scenery.'



<br>

### 이미지 생성을 위한 프롬프트 작성 요청하기

이번에는 영문 요약 결과를 바탕으로 이미지 생성에 적합한 형태의 프롬프트 작성을 요청합니다. 묘사를 돕는 assistant가 필요한 것이 아니라 이번엔 이미지 생성을 위한 프롬프트 작성을 위한 assistant가 필요한 것이므로 시스템 메시지를 적절히 변경해줍니다. 


```python
# 메시지 설정하기
messages = [
    {"role": "system", "content": "You are a capable assistant that helps generate prompts for image creation."},
    {"role": "assistant", "content": answer3},
    {"role": "user", "content": "Please write a prompt sentence suitable for generating images based on the information provided."}
]

# ChatGPT API 호출하기
response = openai.ChatCompletion.create(
    model=model,
    messages=messages
)
answer4 = response['choices'][0]['message']['content']
answer4
```




    'Create an image of an otherworldly, agile creature with long limbs and unique physical attributes that would enable it to move swiftly and gracefully amidst the otherworldly landscape of the Martian terrain.'



<br>

### Karlo에게 이미지 생성 요청하기

앞에서 생성한 이미지 생성을 위한 프롬프트 결과인 'answer4' 값을 karlo api에 전달해 이미지 생성을 요청합니다.


```python
# 프롬프트에 사용할 제시어
text = answer4

# 이미지 생성하기 REST API 호출
img_dict = karlo.text_to_image(text, 1)

# 생성된 이미지 정보
img_str = img_dict.get("images")[0].get('image')

# base64 string을 이미지로 변환
img = karlo.string_to_image(base64_string = img_str, mode = 'RGBA')
img
```




    
![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/img2.png){: width="80%" .align-center}
    



<br>

### DALL-E에게 이미지 생성 요청하기

'openai' 라이브러리를 이용하면 ChatGPT API 외에도 DALL-E API도 사용할 수 있습니다. 'openai.Image.create()'에 이미지 생성을 위해 필요한 프롬프트를 입력하고, 출력할 이미지의 갯수와 크기를 설정하기만 하면 됩니다. 생성된 이미지는 URL 형태로 제공되므로 requests 라이브러리로 해당 URL의 이미지를 별도로 불러와야 합니다.


```python
import requests
from PIL import Image
from io import BytesIO

response = openai.Image.create(
  prompt=answer4,
  n=1,
  size="1024x1024"
)
image_url = response['data'][0]['url']
res = requests.get(image_url)
img = Image.open(BytesIO(res.content))
img
```
    
![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/img3.png){: width="80%" .align-center}


<br>

## 참고

- [공식문서 - Chat completions](https://platform.openai.com/docs/guides/chat)
- [GitHub 저장소 - PyKakao](https://github.com/WooilJeong/PyKakao)
- [GitHub 저장소 - Chat2Image Creator](https://github.com/kairess/Chat2Image-Creator)
- [PyKakao - Python으로 카카오 Karlo API 사용하기](https://wooiljeong.github.io/python/pykakao-karlo/)
- [빵형의 개발도상국 - 자신의 상상을 그림으로 그리는 인공지능 - ChatGPT API 사용법](https://www.youtube.com/watch?v=sLgYJIpqUJg)
