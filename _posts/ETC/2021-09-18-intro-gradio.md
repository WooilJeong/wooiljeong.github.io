---
title: "Gradio - Python으로 머신러닝 모델 웹 앱 배포하기"
categories: ETC
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

    Running locally at: http://127.0.0.1:7862/
    To create a public link, set `share=True` in `launch()`.
    Interface loading below...
    



<iframe
    width="900"
    height="500"
    src="http://127.0.0.1:7862/"
    frameborder="0"
    allowfullscreen
></iframe>






    (<Flask 'gradio.networking'>, 'http://127.0.0.1:7862/', None)

<br>

## 인터페이스

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
    return "Hello " + name + "!"


iface = gr.Interface(
    fn=greet,
    inputs=gr.inputs.Textbox(lines=2, placeholder="Name Here..."),
    outputs="text")
iface.launch()
```

    Running locally at: http://127.0.0.1:7863/
    To create a public link, set `share=True` in `launch()`.
    Interface loading below...
    



<iframe
    width="900"
    height="500"
    src="http://127.0.0.1:7863/"
    frameborder="0"
    allowfullscreen
></iframe>






    (<Flask 'gradio.networking'>, 'http://127.0.0.1:7863/', None)

<br>

## 다중 입출력

여러 입력값과 여러 출력값을 갖는 더 복잡한 함수를 가정해보자. 문자열, 불리언 그리고 숫자를 입력 받고, 문자열과 숫자를 출력하는 함수를 정의하고 인터페이스를 만들어보자. 입력 및 출력 컴포넌트에 다음과 같이 리스트를 넘겨주면 된다.


```python
import gradio as gr


def greet(name, is_morning, temperature):
    salutation = "Good morning" if is_morning else "Good evening"
    greeting = "%s %s. It is %s degrees today" % (
        salutation, name, temperature)
    celsius = (temperature - 32) * 5 / 9
    return greeting, round(celsius, 2)


iface = gr.Interface(
    fn=greet,
    inputs=["text", "checkbox", gr.inputs.Slider(0, 100)],
    outputs=["text", "number"])
iface.launch()
```

    Running locally at: http://127.0.0.1:7864/
    To create a public link, set `share=True` in `launch()`.
    Interface loading below...
    



<iframe
    width="900"
    height="500"
    src="http://127.0.0.1:7864/"
    frameborder="0"
    allowfullscreen
></iframe>






    (<Flask 'gradio.networking'>, 'http://127.0.0.1:7864/', None)

<br>

## 이미지 작업

이미지를 입력받아 이미지를 출력하는 함수를 생각해보자. `Image` 컴포넌트 사용시 함수는 `(너비, 높이, 3)` 모양으로 지정된 numpy 배열을 입력받는다. 이때, 마지막 차원은 RGB 값을 나타낸다. 마찬가지로 numpy 배열을 출력한다.


```python
import gradio as gr
import numpy as np


def sepia(img):
    sepia_filter = np.array([[.393, .769, .189],
                             [.349, .686, .168],
                             [.272, .534, .131]])
    sepia_img = img.dot(sepia_filter.T)
    sepia_img /= sepia_img.max()
    return sepia_img


iface = gr.Interface(fn=sepia,
                     inputs=gr.inputs.Image(shape=(200, 200)),
                     outputs="image")
iface.launch()
```

    Running locally at: http://127.0.0.1:7865/
    To create a public link, set `share=True` in `launch()`.
    Interface loading below...
    



<iframe
    width="900"
    height="500"
    src="http://127.0.0.1:7865/"
    frameborder="0"
    allowfullscreen
></iframe>






    (<Flask 'gradio.networking'>, 'http://127.0.0.1:7865/', None)

<br>

## 데이터 작업

Gradio는 numpy 배열, pandas 데이터프레임 그리고 plotly 그래프와 같은 일반적인 데이터 라이브러리들의 입출력을 지원한다. (함수 내부의 디테일한 데이터 조작 과정은 무시해도 된다.)


```python
import gradio as gr
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt


def sales_projections(employee_data):
    sales_data = employee_data.iloc[:, 1:4].astype("int").to_numpy()
    regression_values = np.apply_along_axis(lambda row:
                                            np.array(np.poly1d(np.polyfit([0, 1, 2], row, 2))), 0, sales_data)
    projected_months = np.repeat(np.expand_dims(
        np.arange(3, 12), 0), len(sales_data), axis=0)
    projected_values = np.array([
        month * month * regression[0] + month * regression[1] + regression[2]
        for month, regression in zip(projected_months, regression_values)])
    plt.plot(projected_values.T)
    plt.legend(employee_data["Name"])
    return employee_data, plt.gcf(), regression_values


iface = gr.Interface(sales_projections,
                     gr.inputs.Dataframe(
                         headers=["Name", "Jan Sales",
                                  "Feb Sales", "Mar Sales"],
                         default=[["Jon", 12, 14, 18], [
                             "Alice", 14, 17, 2], ["Sana", 8, 9.5, 12]]
                     ),
                     [
                         "dataframe",
                         "plot",
                         "numpy"
                     ],
                     description="Enter sales figures for employees to predict sales trajectory over year."
                     )
iface.launch()
```

    Running locally at: http://127.0.0.1:7866/
    To create a public link, set `share=True` in `launch()`.
    Interface loading below...
    



<iframe
    width="900"
    height="500"
    src="http://127.0.0.1:7866/"
    frameborder="0"
    allowfullscreen
></iframe>






    (<Flask 'gradio.networking'>, 'http://127.0.0.1:7866/', None)

<br>

## 입력 예시

사용자가 모델에 쉽게 로드할 수 있도록 입력 예시를 제공할 수 있다. 이는 모델과 관련된 데이터세트를 탐색하는 방법을 제공해줄 뿐 아니라 모델이 기대하는 입력값의 유형을 시연하는데도 도움이 된다. 입력 예시를 로드하기 위해서는 인터페이스 생성자의 `examples` 인자로 중첩 리스트를 넘겨주면 된다. 바깥 리스트 안의 각 하위 리스트는 데이터 샘플이고, 하위 리스트 안의 각 요소는 각 입력 컴포넌트의 입력을 나타낸다. 각 컴포넌트에 대한 입력 예시 형식은 [공식문서](https://gradio.app/docs)에 지정되어 있다.


```python
import gradio as gr

def calculator(num1, operation, num2):
    if operation == "add":
        return num1 + num2
    elif operation == "subtract":
        return num1 - num2
    elif operation == "multiply":
        return num1 * num2
    elif operation == "divide":
        return num1 / num2

iface = gr.Interface(calculator, 
    ["number", gr.inputs.Radio(["add", "subtract", "multiply", "divide"]), "number"],
    "number",
    examples=[
        [5, "add", 3],
        [4, "divide", 2],
        [-4, "multiply", 2.5],
        [0, "subtract", 1.2],
    ],
    title="test calculator",
    description="heres a sample toy calculator. enjoy!",
    flagging_options=["this", "or", "that"],
)

iface.launch()
```

    Running locally at: http://127.0.0.1:7867/
    To create a public link, set `share=True` in `launch()`.
    Interface loading below...
    



<iframe
    width="900"
    height="500"
    src="http://127.0.0.1:7867/"
    frameborder="0"
    allowfullscreen
></iframe>






    (<Flask 'gradio.networking'>, 'http://127.0.0.1:7867/', None)

<br>

## 라이브 인터페이스

인터페이스에서 `live=True`를 설정하여 인터페이스를 자동으로 응답하도록 만들 수 있다. 인터페이스는 사용자가 입력하는 즉시 다시 계산된다.


```python
import gradio as gr

def calculator(num1, operation, num2):
    if operation == "add":
        return num1 + num2
    elif operation == "subtract":
        return num1 - num2
    elif operation == "multiply":
        return num1 * num2
    elif operation == "divide":
        return num1 / num2

iface = gr.Interface(calculator, 
    ["number", gr.inputs.Radio(["add", "subtract", "multiply", "divide"]), "number"],
    "number",
    live=True
)

iface.launch()
```

    Running locally at: http://127.0.0.1:7868/
    To create a public link, set `share=True` in `launch()`.
    Interface loading below...
    



<iframe
    width="900"
    height="500"
    src="http://127.0.0.1:7868/"
    frameborder="0"
    allowfullscreen
></iframe>






    (<Flask 'gradio.networking'>, 'http://127.0.0.1:7868/', None)

<br>

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
