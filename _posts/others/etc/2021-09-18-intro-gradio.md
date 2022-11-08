---
title: "Gradio - Python으로 머신러닝 모델 웹 앱 배포하기"
categories: etc
tags: Gradio
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# Gradio - Python으로 머신러닝 모델 웹 앱 배포하기

*[Gradio](https://www.gradio.app/)의 'Getting Started'를 번역한 글임.

Gradio는 머신러닝 모델을 웹 앱 형태로 빌드하고 배포할 수 있도록 돕는 파이썬 라이브러리이다. 실제로 코드 몇 줄이면 머신러닝 모델을 사용할 수 있는 웹 기반 인터페이스를 구현할 수 있다. Gradio의 장점은 다음과 같다.

**Gradio 장점**  
- 추가적인 모델 검증이 가능하다. 다양한 입력값으로 모델의 결과를 인터렉티브하게 테스트할 수 있다.
- 데모를 구현하기 좋다.
- 구현 및 배포가 쉽다. 공개 링크를 공유하면 누구나 웹 앱에 접근할 수 있다.

<br>

## Gradio 설치

셸에서 다음 명령으로 Gradio를 설치한다.

```bash
pip install gradio
```

<br>

## Gradio 실행 예시


```python
import gradio as gr


def greet(name):
    return "Hello " + name + "!!"


iface = gr.Interface(fn=greet, inputs="text", outputs="text")
iface.launch()
```

![png](/assets/img/post_img/2021-09-18-intro-gradio/1.png){: .align-center}

<br>

## 인터페이스 클래스

Gradio는 사용하기 쉬운 인터페이스로 거의 모든 Python 함수를 래핑할 수 있다. 간단한 세금 계산기부터 사전학습 된 모델에 이르기까지 다양한 함수를 인터페이스에 연동할 수 있다. 

핵심인 `Interface` 클래스는 세 가지 파라미터로 초기화된다.

- `fn`: 래핑할 함수
- `inputs`: 입력 컴포넌트의 타입
- `outputs`: 출력 컴포넌트의 타입

위 세 가지 파라미터를 사용해 인터페이스를 빠르게 만들고 `launch()`할 수 있다. 

<br>

## 커스터마이징 가능한 컴포넌트

예를 들어, 입력 텍스트 필드를 더 크게 만들고, 텍스트 힌트를 넣고 싶다고 한다면 다음과 같이 축약된 문자열 대신 실제 입력 클래스인 `Textbox`를 사용하면 된다. 컴포넌트 커스터마이징과 관련한 더 많은 정보는 [공식문서](https://gradio.app/docs)를 참조하면 된다.


```python
import gradio as gr


def greet(name):
    return "Hello " + name + "!!"


iface = gr.Interface(
    fn=greet,
    inputs=gr.inputs.Textbox(lines=2, placeholder="이름을 입력하세요."),
    outputs="text")
iface.launch()
```

![png](/assets/img/post_img/2021-09-18-intro-gradio/2.png){: .align-center}


<br>

## 다중 입출력

여러 입력값과 여러 출력값을 갖는 더 복잡한 함수를 가정해보자. 문자열, 불리언 그리고 숫자를 입력 받고, 문자열과 숫자를 출력하는 함수를 정의하고 인터페이스를 만들어보자. 입력 및 출력 컴포넌트에 다음과 같이 리스트를 넘겨주면 된다.


```python
import gradio as gr

def greet(name, is_morning, temperature):
    salutation = "Good morning" if is_morning else "Good evening"
    greeting = f"{salutation} {name}. It is {temperature} degrees today"
    celsius = (temperature - 32) * 5 / 9
    return greeting, round(celsius, 2)

demo = gr.Interface(
    fn=greet,
    inputs=["text", "checkbox", gr.Slider(0, 100)],
    outputs=["text", "number"],
)
demo.launch()
```

![png](/assets/img/post_img/2021-09-18-intro-gradio/3.png){: .align-center}    


<br>

## 이미지 작업

Gradio는 Image, DataFrame, Video 혹은 Label과 같은 다양한 유형의 구성 요소를 지원한다. 이미지를 이미지로 변환하는 기능을 사용해보자.

```python
import numpy as np
import gradio as gr

def sepia(input_img):
    sepia_filter = np.array([
        [0.393, 0.769, 0.189], 
        [0.349, 0.686, 0.168], 
        [0.272, 0.534, 0.131]
    ])
    sepia_img = input_img.dot(sepia_filter.T)
    sepia_img /= sepia_img.max()
    return sepia_img

demo = gr.Interface(sepia, gr.Image(shape=(200, 200)), "image")
demo.launch()
```

![png](/assets/img/post_img/2021-09-18-intro-gradio/4.png){: .align-center}    

<br>

Image 구성 요소를 입력으로 사용할 때 함수는 Shape(너비, 높이, 3)의 NumPy Array을 받는다. 여기서 마지막 차원은 RGB 값을 나타낸다. NumPy Array 형태로 이미지를 변환한다.

type 키워드 인수를 사용하여 구성 요소에서 사용하는 데이터 유형을 설정할 수도 있다. 예를 들어 함수가 NumPy Array 대신 이미지 경로를 사용하도록 하려면 입력 이미지 구성 요소를 다음과 같이 작성할 수 있다.

```python
gr.Image(type="filepath", shape=...)
```

또, 입력 이미지 구성 요소에는 편집 버튼 🖉이 있어 이미지를 자르고 확대할 수 있다. 이런 식으로 이미지를 조작하면 기계 학습 모델의 편향이나 숨겨진 결함을 드러내는 데 도움이 될 수 있다!

Gradio [문서](https://gradio.app/docs/)에서 많은 구성 요소와 사용 방법에 대해 자세히 읽을 수 있다.

<br>


## 블록 클래스

Gradio에는 인터페이스 클래스 말고도 블록이라는 클래스도 존재한다. 이 클래스는 더 자유도 높은 기능을 제공한다.

## 블록 예시

```python
import gradio as gr

def greet(name):
    return "Hello " + name + "!"

with gr.Blocks() as demo:
    name = gr.Textbox(label="Name")
    output = gr.Textbox(label="Output Box")
    greet_btn = gr.Button("Greet")
    greet_btn.click(fn=greet, inputs=name, outputs=output)

demo.launch()
```

![png](/assets/img/post_img/2021-09-18-intro-gradio/5.png){: .align-center}    


- 블록은 with 절로 만들어지며 이 절 안에 생성된 모든 구성 요소는 자동으로 앱에 추가된다.
- 구성 요소는 만들어진 순서대로 앱에 세로로 나타난다. 
- 버튼이 생성되었고, 이 버튼에 클릭 이벤트 리스너가 추가되었다. 인터페이스와 마찬가지로 click 메서드는 Python 함수, 입력 구성 요소 및 출력 구성 요소를 사용한다.

<br>

## 더 높은 자유도

```python
import numpy as np
import gradio as gr

def flip_text(x):
    return x[::-1]

def flip_image(x):
    return np.fliplr(x)

with gr.Blocks() as demo:
    gr.Markdown("Flip text or image files using this demo.")
    with gr.Tab("Flip Text"):
        text_input = gr.Textbox()
        text_output = gr.Textbox()
        text_button = gr.Button("Flip")
    with gr.Tab("Flip Image"):
        with gr.Row():
            image_input = gr.Image()
            image_output = gr.Image()
        image_button = gr.Button("Flip")

    with gr.Accordion("Open for More!"):
        gr.Markdown("Look at me...")

    text_button.click(flip_text, inputs=text_input, outputs=text_output)
    image_button.click(flip_image, inputs=image_input, outputs=image_output)

demo.launch()
```

![png](/assets/img/post_img/2021-09-18-intro-gradio/6.png){: .align-center}    


## Flagging

출력 인터페이스의 아래에 "Flag" 버튼이 있다. 사용자가 모델을 테스트하면서 에러와 같은 이슈를 발견하게 될 때, 인터페이스 작성자가 해당 이슈를 검토할 수 있도록 입력에 플래그를 지정할 수 있다. 인터페이스 생성자의 `flagging_dir` 인수에 설정한 경로 안의 CSV 파일이 플래그가 지정된 입력을 기록한다. 인터페이스에 이미지 및 오디오 컴포넌트와 같은 파일 데이터가 포함된 경우 플래그가 지정된 데이터도 저장하기 위해 폴더가 생성된다.

예를 들어, 위의 계산기 인터페이스를 사용하면 아래와 같이 플래그가 지정된 경로에 데이터가 저장된다.

```
+-- calculator.py
+-- flagged/
|   +-- logs.csv
```

**flagged/logs.csv**  

```
num1,operation,num2,Output
5,add,7,12
6,subtract,1.5,4.5
```

위의 세피아 인터페이스를 사용하면 아래와 같이 플래그가 지정된 경로에 데이터가 저장된다.

```
+-- sepia.py
+-- flagged/
|   +-- logs.csv
|   +-- im/
|   |   +-- 0.png
|   |   +-- 1.png
|   +-- Output/
|   |   +-- 0.png
|   |   +-- 1.png
```

**flagged/logs.csv**  

```
im,Output
im/0.png,Output/0.png
im/1.png,Output/1.png
```

플래그가 지정된 경로를 수동으로 찾아가 플래그가 지정된 입력을 검토하거나, `examples` 인수를 플래그가 지정된 경로로 지정하여 Gradio 인터페이스의 예제에 로드할 수 있다. 사용자가 플래그 지정 이유를 제공하도록 하려면 인터페이스의 `flagging_options` 인수에 문자열 리스트를 전달할 수 있다. 사용자는 플래그를 지정할 때 문자열 중 하나를 선택해야 하며, 이는 CSV 파일에 추가 열로 저장된다.

<br>

## 인터페이스 공유 (Publicly & Privacy)

`launch()` 메서드 안에 `share=True`를 설정하면 인터페이스를 공개적으로 공유가 가능하다.

```python
gr.Interface(classify_image, "image", "label").launch(share=True)
```

이렇게 하면 누구나 접근 가능한 공개 링크가 생성된다. 이 링크를 공유하면 상대방의 브라우저에서 모델을 시험해 볼 수 있다. 서버에서 처리가 이루어지므로(서버가 켜져 있는 한) 종속성에 대해 걱정할 필요가 없다. colab 노트북에서 작업하는 경우 공유 링크가 항상 자동으로 생성된다. 일반적으로 XXXXX.gradio.app와 같다. 링크가 Gradio 링크를 통해 제공되지만 Gradio는 로컬 서버의 프록시일 뿐이며 인터페이스를 통해 전송된 데이터를 저장하지 않는다고 한다. 

이 링크는 공개적으로 누구나 접근할 수 있기 때문에 누구나 모델을 사용해 예측을 할 수 있으니 주의해야한다. 작성한 함수에 민감한 정보가 노출되지 않도록 하거나 서버에 중요한 변경 사항이 발생하지 않도록 해야한다. `share=False` (기본값)를 설정하면 특정 사용자와 포트 포워딩을 통해 공유할 수 있는 로컬 링크만 생성된다. 공유 링크는 72시간 후에 만료된다. 영구 호스팅 방법은 아래 내용을 참조하면 된다.

![PNG](https://www.gradio.app/static/home/img/sharing.svg)

<br>

## 인증 (Authentication)


접근을 제한하기 위해 인터페이스 앞에 인증 페이지를 넣을 수 있다. `launch()` 메서드의 `auth` 인자를 사용하면 허용되는 사용자 이름/암호 튜플 리스트를 전달할 수 있다. 또는 커스텀 인증 처리를 위해서는 사용자 이름과 암호를 인자로 사용하는 함수를 전달하고, 인증을 허용하려면 True를 반환하고 그렇지 않으면 False를 반환한다.

<br>

## 영구 호스팅 (Permanent Hosting)


Gradio 인프라에서 호스팅하여 인터페이스를 영구적으로 공개 공유할 수 있다. 이 경우 Gradio 프리미엄 계정을 만들어야 한다. 먼저 **gradio.app**에서 Gradio에 로그인하고 상단의 Sign In을 클릭한다. Github 계정으로 로그인하면 Github 프로필에서 Gradio에서 호스팅할 저장소를 지정할 수 있다. Gradio `launch()` 명령을 실행하는 저장소 내에서 파일을 지정해야 한다. 이렇게 하면 Gradio가 인터페이스를 시작하고 공유 가능한 공개 링크를 제공해준다.

<br>

## 더 알아보기

- [고급기능](https://www.gradio.app/advanced_features) - 추가적인 강력한 기능
- [호스팅](https://www.gradio.app/introducing-hosted) - [Hub](https://www.gradio.app/hub)에서 온라인으로 모델을 영구적으로 호스팅하는 방법
- [공식문서](https://www.gradio.app/docs) - 사용 가능한 모든 컴포넌트

<br>

## 참고
[인터페이스 추가 제안하기](https://www.gradio.app/#contact-box)
[Gradio 공식 Github Repository](https://github.com/gradio-app/gradio)
