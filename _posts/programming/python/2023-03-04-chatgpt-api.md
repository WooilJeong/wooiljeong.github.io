---
title: "ChatGPT API Python 사용법 (feat.DALL-E, Karlo)"
categories: python
tags: ChatGPT PyKakao DALLE OpenAI
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# ChatGPT API Python 사용법 (feat.DALL-E, Karlo)

파이썬으로 쉽게 챗GPT API(ChatCompletions)를 사용할 수 있습니다. 먼저, 챗GPT API의 사용법을 간단히 알아보고, 이미지 생성을 위해 필요한 프롬프트를 자동으로 작성하는 방법을 설명합니다. 이렇게 작성한 프롬프트를 활용하여 DALL-E와 Karlo API를 사용하여 이미지를 생성하는 방법도 다뤄보겠습니다.


<br>

## 화성에 살게 된다면 거주지는 어떤 모습일까?

인간이 화성에 정착해 살게 된다면 어떤 집에 살게 될지 궁금하다면, 이제 아래와 같이 AI에게 간단한 요청만 하면 되는 시대가 되었습니다. 상상을 확장하고 구체화하는 것이 너무나 쉬운 일이 되었습니다. 간단한 요청만으로 AI의 상상을 시각화한 결과를 살펴보겠습니다. (기획: 정우일, 글: ChatGPT, 그림: Karlo, DALL-E)

<br>

> ### "인간이 화성에 정착한다면 건축물의 모습은 어떤 모습일지 자유롭게 상상해서 외형을 묘사해주세요."

<br>

### ChatGPT의 나름 이유있는(?) 상상


화성에 정착한다면 건축물은 지구와는 달리 매우 다른 환경에 적응해야 합니다. 그러므로 건축물의 외형도 많이 달라질 것으로 예상됩니다.

첫째로 먼지와 폭풍우 등의 기상 조건을 고려해야 합니다. 건축물은 외부 영향과 교감하지 않고 방어할 수 있어야 합니다. 그렇기 때문에 건물의 외벽은 얇아서는 안 됩니다. **원통형**이나 **구형** 등을 생각할 수 있으며, 일반적인 적출이 아닌 설정을 고려할 수 있습니다.

둘째로, 화성에서의 에너지 문제를 해결해야 합니다. 한강의 수력 발전소처럼 대규모인 경우는 건축적 인프라로 신경 써야 합니다. 또한, 화성은 자외선이 강하므로 건축물은 **자외선 차단 렌즈**가 필요할 것입니다. 따라서 건축물은 자외선 차단재를 사용하여 철저하게 보호할 필요가 있습니다.

마지막으로, 필요한 다른 물질을 도입하기 위해 건축물에는 방사선 방어 대책이 필요합니다. 이를 위해 건축물 내부에 **목재나 금속 등의 장치**를 사용하여 방사능을 차단할 수 있습니다.

이러한 고려 사항을 고려하여, 화성에는 고대 시대의 전통적인 건축양식이 아닌 전처기 건축이 출현할 것으로 예상됩니다. 그러므로 건축물은 간결하면서도 기능적으로 구성될 것으로 보입니다.

<br>

### Karlo가 그린 그림

![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/img2.png){: width="90%" .align-center}

<br>

### DALL-E가 그린 그림

![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/img3.png){: width="90%" .align-center}

<br>

## ChatGPT API란?

[챗GPT(ChatGPT) API](https://platform.openai.com/docs/guides/chat)는 2023년 3월에 출시된 오픈AI의 새로운 서비스입니다. 이 API를 사용하면 챗GPT를 앱이나 제품에 쉽게 통합할 수 있습니다. 물론 여전히 [챗GPT 애플리케이션](https://chat.openai.com/chat)에 직접 접속해서도 사용할 수 있습니다. 다만, API 사용은 유료이며, 대화에 사용된 토큰(Tokens)의 크기(1,000토큰 당 $0.002(약 3원))에 따라 과금됩니다.

- [API 사용요금](https://openai.com/pricing)
    - Model: gpt-3.5-turbo
    - Usage: $0.002 / 1K tokens


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

![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/ex1.png){: width="90%" .align-center}

'Create new secret key' 버튼을 클릭합니다.

![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/ex2.png){: width="90%" .align-center}


초록색 '복사' 버튼을 클릭해 인증키를 복사하고 'OK' 버튼을 클릭합니다.

![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/ex3.png){: width="90%" .align-center}


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
OPENAI_API_KEY = "오픈AI에서 발급받은 인증키"

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
KAKAO_API_KEY = "카카오 디벨로퍼스에서 발급받은 인증키"

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




    
![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/img1.png){: width="90%" .align-center}
    



<br>

## ChatGPT X Karlo X DALL-E 이미지 생성하기

앞에서 API를 다루는 방법에 대해 알아보았습니다. 그럼 ChatGPT, Karlo, DALL-E를 모두 활용해서 AI의 상상을 시각화하는 방법에 대해 알아보겠습니다. 이 과정에서 사람은 그저 궁금한 걸 묘사해달라고 하기만 하면 됩니다.

<br>

### 라이브러리 불러오기

필요한 라이브러리를 불러오고 인증키를 설정합니다.


```python
import openai
from PyKakao import Karlo

# 발급받은 API 키 설정
OPENAI_API_KEY = "오픈AI에서 발급받은 인증키"
KAKAO_API_KEY = "카카오에서 발급받은 인증키"

# openai API 키 인증
openai.api_key = OPENAI_API_KEY

# Karlo API 인스턴스 생성
karlo = Karlo(service_key = KAKAO_API_KEY)

# 모델 - GPT 3.5 Turbo 선택
model = "gpt-3.5-turbo"
```

<br>

### 챗GPT에게 상상해달라고 요청하기

'query'에 궁금한 점을 묘사해달라는 요청을 작성해 할당합니다. ChatGPT API의 시스템 메시지를 작성해 어시스턴트가 묘사를 잘하는 역할이라고 설정합니다. API를 호출한 결과를 다음 과정에서 사용할 것이므로 결과를 'answer'에 할당합니다.


```python
# 질문 작성하기
query = "인간이 화성에 정착한다면 건축물의 모습은 어떤 모습일지 자유롭게 상상해서 외형을 묘사해주세요."

# 메시지 설정하기
messages = [
    {
        "role": "system",
        "content": "You are a helpful assistant who is good at detailing."
    },
    {
        "role": "user",
        "content": query
    }
]

# ChatGPT API 호출하기
response = openai.ChatCompletion.create(
    model=model,
    messages=messages
)
answer = response['choices'][0]['message']['content']
print(answer)
```

    화성에 정착한다면 건축물은 지구와는 달리 매우 다른 환경에 적응해야 합니다. 그러므로 건축물의 외형도 많이 달라질 것으로 예상됩니다.
    
    첫째로 먼지와 폭풍우 등의 기상 조건을 고려해야 합니다. 건축물은 외부 영향과 교감하지 않고 방어할 수 있어야 합니다. 그렇기 때문에 건물의 외벽은 얇아서는 안 됩니다. 원통형이나 구형 등을 생각할 수 있으며, 일반적인 적출이 아닌 설정을 고려할 수 있습니다.
    
    둘째로, 화성에서의 에너지 문제를 해결해야 합니다. 한강의 수력 발전소처럼 대규모인 경우는 건축적 인프라로 신경 써야 합니다. 또한, 화성은 자외선이 강하므로 건축물은 자외선 차단 렌즈가 필요할 것입니다. 따라서 건축물은 자외선 차단재를 사용하여 철저하게 보호할 필요가 있습니다.
    
    마지막으로, 필요한 다른 물질을 도입하기 위해 건축물에는 방사선 방어 대책이 필요합니다. 이를 위해 건축물 내부에 목재나 금속 등의 장치를 사용하여 방사능을 차단할 수 있습니다.
    
    이러한 고려 사항을 고려하여, 화성에는 고대 시대의 전통적인 건축양식이 아닌 전처기 건축이 출현할 것으로 예상됩니다. 그러므로 건축물은 간결하면서도 기능적으로 구성될 것으로 보입니다.
    

<br>

### 더 구체적인 묘사를 요청하기

앞에서 ChatGPT가 응답한 결과 'answer'를 어시스턴트 메시지로 추가합니다. 이렇게 하면 API 요청 시 ChatGPT가 이전에 자신이 응답한 결과를 알 수 있어 대화의 흐름을 유지할 수 있습니다. 이번에는 사용자 메시지로 외형 묘사를 더 자세히 해달라고 작성합니다. 이번에도 마찬가지로 결과를 'answer2'에 할당합니다.


```python
# 이전 어시스턴트 응답 메시지 추가
messages.append(
    {
        "role": "assistant",
        "content": answer
    },
)

# 사용자 메시지 추가
messages.append(
    {
        "role": "user", 
        "content": "위 내용을 바탕으로 외형적인 모습을 더 자세히 상상해서 묘사해주세요."
    }
)

# ChatGPT API 호출하기
response = openai.ChatCompletion.create(
    model=model,
    messages=messages
)
answer2 = response['choices'][0]['message']['content']
print(answer2)
```

    화성에 정착한다면 건축물은 지구와는 달리 매우 다른 환경에 적응해야 합니다. 그러므로 건축물의 외형도 많이 달라질 것으로 예상됩니다.
    
    화성에서의 기상 조건은 지구와는 크게 다릅니다. 먼지와 폭풍우 등의 자연 현상이 매우 많고 강하기 때문에 건축물은 외부 영향과 교감하지 않고 방어할 수 있어야 합니다. 따라서 건물의 외벽은 매우 튼튼하면서도 엄청난 내구성과 경량을 가진 철재로 만들어질 것입니다. 건물의 형태는 원통형이나 구형 등으로 균형을 유지할 수 있도록 설계될 것입니다.
    
    화성에서의 에너지 문제를 해결하기 위해서 건축물은 대규모인 경우 천공 태양광 패널로 구성될 수 있습니다. 이를 통해 건물 내부 전기를 공급할 수 있고 자외선 차단을 위한 창문은 자외선 차단재를 사용하여 철저하게 보호될 것입니다.
    
    또한, 건물 내부는 방사능을 차단하는 장치로 보호될 것입니다. 이를 위해서 건축물 내부에는 목재나 금속 등으로 된 장치와 방사능 차단재가 사용될 것입니다. 따라서 건물 외부 벽면도 완전한 방사능 차단 재질로 만들어질 것입니다.
    
    화성 건축물은 화성의 환경적 특성 상 간결하면서도 기능적인 구조의 건물이 될 것입니다. 굴착 방식으로 만들어질 수도 있고, 외부와 내부를 잘 분리해 단단한 기반 위에 건축할 수도 있을 것입니다. 이 같은 구조들은 화성 정착에 필수적인 역할을 할 것입니다.
    

<br>

### 영어 번역 요청하기

이번에는 앞의 응답결과를 영어로 번역해달라고 요청합니다. 메시지를 새로 구성해 시스템 메시지로 어시스턴트의 역할을 번역을 잘하는 역할로 설정합니다. 출력 결과는 'answer3'에 할당합니다.


```python
# 새 메시지 구성
messages = [
    {
        "role": "system",
        "content": "You are a helpful assistant who is good at translating."
    },
    {
        "role": "assistant",
        "content": answer2
    }
]

# 사용자 메시지 추가
messages.append(
    {
        "role": "user", 
        "content": "영어로 번역해주세요."
    }
)

# ChatGPT API 호출하기
response = openai.ChatCompletion.create(
    model=model,
    messages=messages
)
answer3 = response['choices'][0]['message']['content']
print(answer3)
```

    If settling on Mars, buildings will need to adapt to a very different environment compared to Earth. Therefore, the appearance of buildings is expected to be quite different.
    
    The weather conditions on Mars are vastly different from those on Earth. Natural phenomena such as dust storms and gusts are very frequent and strong, so buildings need to be able to withstand external influences and defend against them. Therefore, the exterior walls of buildings will be made of very sturdy and lightweight steel with great durability. The shape of the building will be designed to maintain balance, such as cylindrical or spherical.
    
    To address the energy problem on Mars, buildings may be composed of large-scale perforated solar panels. Thus, interior electricity can be supplied, and windows for ultraviolet ray protection will be thoroughly protected by using ultraviolet-blocking agents.
    
    In addition, the building's interior will be protected by a device that blocks radiation. To accomplish this, devices made of materials such as wood or metal and radiation-blocking agents will be used inside the building. Therefore, the exterior walls of the building will also be made of completely radiation-blocking materials.
    
    Mars buildings will be functional structures designed to be concise and functional given Martian environmental characteristics. They can be made by excavation or built on a sturdy foundation, well-separating the exterior and interior. Such structures will play a crucial role in the settlement of Mars.
    

<br>

### 이미지 생성을 위한 프롬프트 작성 요청하기

영어로 번역된 결과를 바탕으로 이미지 생성 AI 사용 시 필요한 프롬프트 문장을 작성합니다. 새 메시지를 구성하고, 시스템 메시지로 이미지 생성을 위해 프롬프트를 생성하는 걸 잘하는 역할을 어시스턴트에게 부여합니다. 그리고 사용자 메시지로 프롬프트를 작성해달라는 내용을 작성합니다. 이 결과를 'answer4'에 할당합니다.


```python
# 새 메시지 구성
messages = [
    {
        "role": "system",
        "content": "You are an assistant who is good at creating prompts for image creation."
    },
    {
        "role": "assistant",
        "content": answer3
    }
]

# 사용자 메시지 추가
messages.append(
    {
        "role": "user", 
        "content": "Condense up to 4 outward description to focus on nouns and adjectives separated by ,"
    }
)

# ChatGPT API 호출하기
response = openai.ChatCompletion.create(
    model=model,
    messages=messages
)
answer4 = response['choices'][0]['message']['content']
print(answer4)
```

    Sturdy, lightweight, steel walls. Cylindrical or spherical shape. Large-scale perforated solar panels. Radiation-blocking interior devices.
    

<br>

### 프롬프트 완성하기

프롬프트에 스타일을 추가하면 더 고품질의 이미지를 얻을 수 있습니다. 앞에서 추출한 키워드들과 이미지를 꾸며주는 파라미터를 적절히 섞어 'prompt'에 할당합니다. 뒤에서 이 값을 Karlo와 DALL-E API의 인자로 입력합니다.


```python
# 이미지 생성을 위한 프롬프트
params = ", concept art, realistic lighting, ultra-detailed, 8K, photorealism, digital art"
prompt = f"Futuristic mars villages, {answer4}{params}"
print(prompt)
```

    Futuristic mars villages, Sturdy, lightweight, steel walls. Cylindrical or spherical shape. Large-scale perforated solar panels. Radiation-blocking interior devices., concept art, realistic lighting, ultra-detailed, 8K, photorealism, digital art
    

<br>

### Karlo에게 이미지 생성 요청하기

앞에서 생성한 이미지 생성을 위한 프롬프트 결과인 'prompt' 값을 karlo api에 전달해 이미지 생성을 요청합니다.


```python
# 이미지 생성하기 REST API 호출
img_dict = karlo.text_to_image(prompt, 1)

# 생성된 이미지 정보
img_str = img_dict.get("images")[0].get('image')

# base64 string을 이미지로 변환
img = karlo.string_to_image(base64_string = img_str, mode = 'RGBA')
img
```




    
![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/img2.png){: width="90%" .align-center}
    



<br>

### DALL-E에게 이미지 생성 요청하기

'openai' 라이브러리를 이용하면 ChatGPT API 외에도 DALL-E API도 사용할 수 있습니다. 'openai.Image.create()'에 이미지 생성을 위해 필요한 프롬프트를 입력하고, 출력할 이미지의 갯수와 크기를 설정하기만 하면 됩니다. 생성된 이미지는 URL 형태로 제공되므로 requests 라이브러리로 해당 URL의 이미지를 별도로 불러와야 합니다.


```python
import requests
from PIL import Image
from io import BytesIO

response = openai.Image.create(
  prompt=prompt,
  n=1,
  size="1024x1024"
)
image_url = response['data'][0]['url']
res = requests.get(image_url)
img = Image.open(BytesIO(res.content))
img
```

    
![PNG](/assets/img/post_img/2023-03-04-chatgpt-api/img3.png){: width="90%" .align-center}
    

<br>

## 참고

- [공식문서 - Chat completions](https://platform.openai.com/docs/guides/chat)
- [GitHub 저장소 - PyKakao](https://github.com/WooilJeong/PyKakao)
- [GitHub 저장소 - Chat2Image Creator](https://github.com/kairess/Chat2Image-Creator)
- [PyKakao - Python으로 카카오 Karlo API 사용하기](https://wooiljeong.github.io/python/pykakao-karlo/)
- [빵형의 개발도상국 - 자신의 상상을 그림으로 그리는 인공지능 - ChatGPT API 사용법](https://www.youtube.com/watch?v=sLgYJIpqUJg)
- [Top 50 Text-to-Image Prompts for AI Art Generators Midjourney and DALL-E
AI Wiki](https://mpost.io/top-50-text-to-image-prompts-for-ai-art-generators-midjourney-and-dall-e/)
- [50 Best DALL.E 2 Architecture Prompts](https://decentralizedcreator.com/best-dall-e-2-architecture-prompts/)