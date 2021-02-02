---
title: "PyTorch 튜토리얼 - 02 자동미분(autograd)"
categories: ML
tags: PyTorch
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

> # PyTorch 튜토리얼 - 02 자동미분(autograd)

## 파이토치 60분 블리츠

[원문 링크](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)

- [PyTorch 튜토리얼 - 01 텐서(Tensors)](https://wooiljeong.github.io/ml/pytorch_tutorial_01/)
- [PyTorch 튜토리얼 - 02 자동미분(autograd)](https://wooiljeong.github.io/ml/pytorch_tutorial_02/)
- [PyTorch 튜토리얼 - 03 신경망(Neural Networks)](https://wooiljeong.github.io/ml/pytorch_tutorial_03/)
- [PyTorch 튜토리얼 - 04 분류기 학습(Training a classifier)](https://wooiljeong.github.io/ml/pytorch_tutorial_04/)

## PyTorch

파이토치는 다음 두 가지 목적으로 가지고 개발된 과학 계산 용 패키지.

1. NumPy 대신 GPU 및 기타 액셀러레이터 성능 사용
2. 신경망 구현에 유용한 자동 미분 라이브러리

## Goal

- PyTorch의 Tensor 라이브러리 및 신경망을 고수준에서 이해.
- 이미지 분류를 위한 소규모 신경망 학습

**`Note`** torch, torchvision 설치 필요

## torch.autograd 소개

`torch.autograd`는 신경망 학습을 지원하는 PyTorch의 자동 미분 엔진이다. 이 섹션에서는 autograd가 신경망 학습을 돕는 방법에 대한 개념적 이해를 얻을 수 있다.

### 배경

신경망(NN)은 일부 입력 데이터에서 실행되는 중첩 함수들의 모음이다. 이 함수들은 PyTorch에서 텐서에 저장되는 파라미터(가중치와 편향)에 의해 정의된다.

신경망 학습은 다음 두 가지 단계로 수행된다.

**순전파(Forward Propagation)**  
순전파 시 신경망은 올바른 결과에 대한 최선의 추측을 한다. 이 추측을 위해 각 함수를 통해 입력 데이터를 실행한다.

**역전파(Backward Propagation)**  
역전파 시 신경망은 추측 오류에 비례하여 파라미터를 조정한다. 출력으로부터 역방향으로 이동한다. 동시에 함수 각각에 대하여 오류의 미분값들(Gradients)을 수집한다. 그리고 경사 하강법(Gradient Descent)을 사용하여 파라미터를 최적화한다. 역전파에 대한 자세한 설명은 [3Blue1Brown](https://www.youtube.com/watch?v=tIeHLnjs5U8)의 영상을 참조하면 된다.

### PyTorch에서의 사용법

학습 단계 중 한 가지를 살펴보자. 아래 예제를 보면, `torchvision`으로부터 사전학습된 resnet18 모델을 불러온다. 이어서 채널 3, 높이 64, 너비 64 그리고 랜덤하게 초기화된 레이블을 가진 단일 이미지 텐서를 생성한다.


```python
import torch, torchvision

# 사전학습된 resnet18 모델 불러오기
model = torchvision.models.resnet18(pretrained=True)
# 채널3, 높이64, 너비64 인 단일 이미지 데이터
data = torch.rand(1, 3, 64, 64)
# 랜덤하게 초기화된 레이블
labels = torch.rand(1, 1000)
```

다음으로, 예측을 위해 입력 데이터를 모델의 각 레이어에 통과시킨다. 이것이 **순전파**다.


```python
# 모델을 통한 예측값 텐서 생성
prediction = model(data) # 전방향 Pass
```

모델의 예측값과 각각에 해당하는 레이블을 이용해 오류(손실)를 계산한다. 다음 단계는 이 오류를 네트워크를 통해 역전파하는 것이다. 오류 텐서에 대해 `.backward()`를 호출하면 역전파가 시작된다. 그런 다음 Autograd는 파라미터의 `.grad` 속성에 있는 각 모델 파라미터에 대한 기울기를 계산하고 저장한다.


```python
loss = (prediction - labels).sum()
loss.backward() # 역방향 Pass
```

다음으로, 옵티마이저를 불러온다. 이 경우 학습률이 0.01이고, 모멘텀이 0.9인 SGD를 불러온다. 옵티마이저에 모델의 모든 파라미터 값을 등록한다.


```python
optim = torch.optim.SGD(model.parameters(), lr=1e-2, momentum=0.9)
optim
```




    SGD (
    Parameter Group 0
        dampening: 0
        lr: 0.01
        momentum: 0.9
        nesterov: False
        weight_decay: 0
    )



마지막으로, `.step()`을 호출해 경사하강법을 실행한다. 옵티마이저는 `.grad`에 저장된 기울기로 각 파라미터를 조절한다.


```python
optim.step() # Gradient Descent
```

이상으로 신경망을 학습시키는 데 필요한 모든 것을 살펴보았다. 아래 섹션에서는 autograd의 작동 방식을 상세히 설명할 것이므로 Skip해도 무방하다.

### Autograd의 미분

`autograd`가 기울기들을 어떻게 수집하는지 살펴보자. 두 가지 텐서 'a'와 'b'를 `requires_grad=True`를 지정하여 만든다. 이는 모든 연산을 기록하라는 옵션입니다.


```python
import torch

a = torch.tensor([2., 3.], requires_grad=True)
b = torch.tensor([6., 4.], requires_grad=True)
```

`a`와 `b`로부터 또 다른 텐서 `Q`를 만든다.

$$
Q = 3a^{3} - b^{2}
$$


```python
Q = 3*a**3 - b**2
```

`a`와 `b`를 신경망의 파라미터로, `Q`를 오류로 가정하자. 신경망 학습 시 파라미터 각각에 대한 오류의 기울기들을 알고자 한다. 예를 들어,

$$
{\partial Q \over \partial a} = 9a^{2}
$$

$$
{\partial Q \over \partial b} = -2b
$$

`Q`에 대해 `.backward()`를 호출하면, autograd는 이 기울기들을 계산하고, 각 텐서들의 `.grad` 속성에 이 값들을 저장한다.

벡터이기 때문에 `Q.backward()`에 `gradient` 인자를 명시적으로 전달해야 한다. `gradient`는 `Q`와 같은 모양의 텐서이고, `Q` 각각에 대한 기울기들을 나타낸다. 예를 들면,

$$
{dQ \over dQ} = 1
$$

마찬가지로, Q를 스칼라로 집계할 수 있고, `Q.sum().backward()`와 같이 암시적으로 역전파를 호출할 수 있다. 


```python
external_grad = torch.tensor([1., 1.])
Q.backward(gradient=external_grad)
```

이제 기울기들은 `a.grad`와 `b.grad`에 저장된다.


```python
# 수집한 기울기들이 올바른지 확인
print(9*a**2 == a.grad)
print(-2*b == b.grad)
```

    tensor([True, True])
    tensor([True, True])
    

### Optional Reading - `autograd`를 이용한 벡터 연산

수학적으로, 벡터 값 함수 $\vec{y}=f(\vec{x})$를 가질 경우, $\vec{x}$ 각각에 대한 $\vec{y}$의 기울기는 자코비안 행렬(Jacobian matrix) $J$와 같다.



$\begin{align}J
     =
      \left(\begin{array}{cc}
      \frac{\partial \bf{y}}{\partial x_{1}} &
      ... &
      \frac{\partial \bf{y}}{\partial x_{n}}
      \end{array}\right)
     =
     \left(\begin{array}{ccc}
      \frac{\partial y_{1}}{\partial x_{1}} & \cdots & \frac{\partial y_{1}}{\partial x_{n}}\\
      \vdots & \ddots & \vdots\\
      \frac{\partial y_{m}}{\partial x_{1}} & \cdots & \frac{\partial y_{m}}{\partial x_{n}}
      \end{array}\right)\end{align}$



일반적으로 `torch.autograd`는 벡터-자코비안 곱 연산을 위한 엔진이다. 즉, 벡터 $\vec{v}$가 주어지면 $J^{T}\cdot \vec{v}$ 을 계산한다.

만일 $v$가 스칼라 함수의 기울기인 경우

$\begin{align}l
   =
   g\left(\vec{y}\right)
   =
   \left(\begin{array}{ccc}\frac{\partial l}{\partial y_{1}} & \cdots & \frac{\partial l}{\partial y_{m}}\end{array}\right)^{T}\end{align}$

연쇄 법칙(Chain Rule)에 의해 벡터-자코비안 곱은 $\vec{x}$ 각각에 대한 $l$의 기울기가 된다.

$\begin{align}J^{T}\cdot \vec{v}=\left(\begin{array}{ccc}
      \frac{\partial y_{1}}{\partial x_{1}} & \cdots & \frac{\partial y_{m}}{\partial x_{1}}\\
      \vdots & \ddots & \vdots\\
      \frac{\partial y_{1}}{\partial x_{n}} & \cdots & \frac{\partial y_{m}}{\partial x_{n}}
      \end{array}\right)\left(\begin{array}{c}
      \frac{\partial l}{\partial y_{1}}\\
      \vdots\\
      \frac{\partial l}{\partial y_{m}}
      \end{array}\right)=\left(\begin{array}{c}
      \frac{\partial l}{\partial x_{1}}\\
      \vdots\\
      \frac{\partial l}{\partial x_{n}}
      \end{array}\right)\end{align}$


벡터-자코비안 곱의 이러한 특성은 위 예시에서 사용한 것과 같다; `external_grad`는 $\vec{v}$를 나타난다.

### 계산 그래프(Computational Graph)

개념적으로 autograd는 [함수](https://pytorch.org/docs/stable/autograd.html#torch.autograd.Function) 객체로 구성된 DAG(Directed Acyclic Graph)에 데이터(텐서) 및 실행된 모든 연산(새로운 텐서와 함께)을 기록하여 유지한다. 이 DAG에서 잎은 입력 텐서이고 뿌리는 출력 텐서이다. 이 그래프를 뿌리에서 잎까지 추적하면 연쇄 법칙(Chain Rule)을 사용하여 기울기를 자동으로 계산할 수 있다.

순전파 시 autograd는 동시에 다음 두 가지를 한다.

- 요청된 연산을 실행하여 결과 텐서를 계산한다.
- DAG에서 연산의 기울기 함수를 유지한다.

역전파는 DAG 루트에서 `.backward()`가 호출될 때 시작된다.

- `autograd`는 각 `.grad_fn`으로부터 기울기를 계산한다.
- `autograd`는 각 텐서의 `.grad` 속성에 이를 누적한다.
- `autograd`는 연쇄 법칙(Chain Rule)을 사용하여, 잎 텐서까지 전파한다.

아래는 예제에서 DAG을 시각적으로 표현한 것이다. 그래프에서 화살표들은 순방향을 향하고 있다. 노드는 순방향에서의 각 연산의 역전파 함수를 나타낸다. 파란색 잎 노드는 잎 텐서 `a`와 `b`를 나타낸다.

![image.png](https://pytorch.org/tutorials/_images/dag_autograd.png)

### DAG에서의 제외

`torch.autograd`는 `requires_grad` flag가 `True`로 설정된 모든 텐서들의 연산들을 추적한다. 기울기가 필요하지 않은 텐서의 경우, 이 속성을 `False`로 설정하면 기울기 계산 DAG에서 이를 제외한다.

연산 출력 텐서는 단일 입력 텐서에서만 requires_grad=True가 있더라도 기울기가 필요하다.


```python
x = torch.rand(5, 5)
y = torch.rand(5, 5)
z = torch.rand((5, 5), requires_grad=True)

a = x + y
print(f"Does 'a' require gradients? : {a.requires_grad}")
b = x + z
print(f"Does 'b' require gradients? : {b.requires_grad}")
```

    Does 'a' require gradients? : False
    Does 'b' require gradients? : True
    

신경망에서, 기울기 계산을 하지 않는 파라미터는 보통 `frozen parameters`라고 부른다. 이러한 파라미터의 기울기가 필요하지 않다는 것을 미리 알고있는 경우 모델의 일부를 "freeze"하는 것은 유용하다.(이는 autograd 계산을 줄여 성능 상의 이점을 제공한다.)

DAG에서의 제외가 중요한 또 다른 일반적인 사용 사례는 [사전 학습 된 네트워크를 미세조정(Finetuning)](https://pytorch.org/tutorials/beginner/finetuning_torchvision_models_tutorial.html)하는 것이다.

미세조정 시 대부분의 모델을 freeze하고 새 레이블에 대한 예측을 수행하기 위해 분류  레이어만 수정한다. 이를 시연하기 위해 간단한 예를 살펴보자. 이전과 마찬가지로 사전 학습 된 resnet18 모델을 불러오고 모든 파라미터를 고정한다.


```python
from torch import nn, optim

model = torchvision.models.resnet18(pretrained=True)

# Freeze all the parameters in the network
for param in model.parameters():
    param.requires_grad = False
```

레이블이 10개인 새 데이터 세트에서 모델을 미세 조정하려 한다고 가정해보자. resnet에서 분류기는 마지막 선형 레이어이다. 분류기 역할을 하는 새로운 선형 레이어(기본적으로 freeze되지 않음)로 간단히 교체 할 수 있다.


```python
model.fc = nn.Linear(512, 10)
```

이제 `model.fc`의 파라미터를 제외한 모델의 모든 파라미터가 freeze된다. 기울기를 계산하는 유일한 파라미터는 `model.fc`의 가중치와 편향이다.


```python
# Optimize only the classifier
optimizer = optim.SGD(model.fc.parameters(), lr=1e-2, momentum=0.9)
```

옵티마이저에 모든 파라미터를 등록하더라도 기울기를 계산하는 (경사하강법으로 업데이트되는) 유일한 파라미터는 분류기의 가중치와 편향이다.

[torch.no_grad()](https://pytorch.org/docs/stable/generated/torch.no_grad.html)에서 컨텍스트 관리자와 동일한 제외 기능을 사용할 수 있다.

## 파이토치 60분 블리츠

[원문 링크](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)

- [PyTorch 튜토리얼 - 01 텐서(Tensors)](https://wooiljeong.github.io/ml/pytorch_tutorial_01/)
- [PyTorch 튜토리얼 - 02 자동미분(autograd)](https://wooiljeong.github.io/ml/pytorch_tutorial_02/)
- [PyTorch 튜토리얼 - 03 신경망(Neural Networks)](https://wooiljeong.github.io/ml/pytorch_tutorial_03/)
- [PyTorch 튜토리얼 - 04 분류기 학습(Training a classifier)](https://wooiljeong.github.io/ml/pytorch_tutorial_04/)

