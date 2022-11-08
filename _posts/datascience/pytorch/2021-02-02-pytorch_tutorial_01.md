---
title: "PyTorch 튜토리얼 - 01 텐서(Tensors)"
categories: pytorch
tags: python
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

> # PyTorch 튜토리얼 - 01 텐서(Tensors)

## 파이토치 60분 블리츠

[원문 링크](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)

- [PyTorch 튜토리얼 - 01 텐서(Tensors)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_01/)
- [PyTorch 튜토리얼 - 02 자동미분(autograd)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_02/)
- [PyTorch 튜토리얼 - 03 신경망(Neural Networks)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_03/)
- [PyTorch 튜토리얼 - 04 분류기 학습(Training a classifier)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_04/)

<br>

## PyTorch

PyTorch는 다음 두 가지 기능이 구현된 과학 연산을 위한 패키지이다.

1. NumPy를 대신하여 연산 시 GPU 성능을 사용
2. 신경망 구현에 유용한 자동 미분 연산

<br>

## 튜토리얼 목표

- PyTorch의 Tensor 라이브러리 및 신경망에 대한 이해
- 이미지 분류를 위한 신경망 모델 학습

**`Note`** torch, torchvision 설치 필요

<br>

## 텐서(Tensors)

Tensor는 배열 및 행렬과 비슷한 특수 데이터 구조다. PyTorch에서는 텐서를 사용하여 모델의 입출력값과 파라미터 값을 인코딩한다.

Tensor는 GPU 기반에서 동작한다는 점을 제외하면, NumPy의 ndarray와 비슷하다. ndarray에 익숙하다면, Tensor API를 곧바로 사용할 수 있다. 그렇지 않다면, Quick API를 참조하면 된다.


```python
import torch
import numpy as np
print(torch.__version__)
print(np.__version__)
```

    1.7.1
    1.19.2
    
<br>

### 텐서 초기화(Tensor Initialization)

텐서는 다양한 방법으로 초기화할 수 있다. 다음 예를 보자.

**데이터에서 직접 초기화**  
텐서는 데이터에서 직접 생성할 수 있다. 데이터 타입은 자동 할당된다.



```python
data = [[1, 2], [3, 4]]
x_data = torch.tensor(data)

print(x_data, type(x_data))
```

    tensor([[1, 2],
            [3, 4]]) <class 'torch.Tensor'>
    

**NumPy 배열에서 초기화**  
텐서는 NumPy 배열에서 생성할 수 있다.(그 역도 가능하다. [Bridge with NumPy](https://pytorch.org/tutorials/beginner/blitz/tensor_tutorial.html#bridge-to-np-label))


```python
np_array = np.array(data)
x_np = torch.from_numpy(np_array)

print(x_np, type(x_np))
```

    tensor([[1, 2],
            [3, 4]], dtype=torch.int32) <class 'torch.Tensor'>
    

**또 다른 텐서**  
생성된 텐서는 명시적으로 재정의하지 않는 한, 인자 텐서의 속성(shape, datatype)을 유지한다. 


```python
x_ones = torch.ones_like(x_data)  # x_data의 속성을 유지
print(f"Ones Tensor: \n {x_ones} \n")

x_rand = torch.rand_like(x_data, dtype=torch.float)  # x_data의 데이터타입
print(f"Random Tensor: \n {x_rand} \n")
```

    Ones Tensor: 
     tensor([[1, 1],
            [1, 1]]) 
    
    Random Tensor: 
     tensor([[0.8900, 0.7087],
            [0.9414, 0.6104]]) 
    
    

**랜덤 혹은 상수 값 사용**  
`shape`은 텐서의 차원을 나타내는 튜플이다. 아래 함수에서, 결과 텐서의 차원을 결정한다.


```python
shape = (2, 3,)

rand_tensor = torch.rand(shape)
ones_tensor = torch.ones(shape)
zeros_tensor = torch.zeros(shape)

print(f"Random Tensor: \n {rand_tensor} \n")
print(f"Ones Tensor: \n {ones_tensor} \n")
print(f"Zeros Tensor: \n {zeros_tensor} \n")
```

    Random Tensor: 
     tensor([[0.6141, 0.7148, 0.4793],
            [0.3011, 0.6247, 0.0072]]) 
    
    Ones Tensor: 
     tensor([[1., 1., 1.],
            [1., 1., 1.]]) 
    
    Zeros Tensor: 
     tensor([[0., 0., 0.],
            [0., 0., 0.]]) 
    
<br>

### 텐서 속성(Tensor Attributes)

텐서 속성은 텐서의 shape, datatype 그리고 텐서가 저장된 device를 설명한다.


```python
tensor = torch.rand(3, 4)

print(f"Shape of tensor: {tensor.shape}")
print(f"Datatype of tensor: {tensor.dtype}")
print(f"Device tensor is stored on: {tensor.device}")
```

    Shape of tensor: torch.Size([3, 4])
    Datatype of tensor: torch.float32
    Device tensor is stored on: cpu
    
<br>

### 텐서 연산(Tensor Operations)

전치, 인덱싱, 슬라이싱, 수학 연산, 선형 대수, 랜덤 샘플링 등을 포함한 100 개 이상의 텐서 연산이 [여기](https://pytorch.org/docs/stable/torch.html)에 포괄적으로 설명되어 있다.

각각은 GPU에서 실행될 수 있다. (일반적으로 CPU보다 속도가 더 빠르다.) Colab을 사용하는 경우 '런타임' > '런타임 유형 변경'으로 이동하여 GPU를 할당할 수 있다.


```python
# 가능하다면 텐서를 GPU로 이동
if torch.cuda.is_available():
    tensor = tensor.to('cuda')
```

목록의 몇 가지 연산을 시도해보자. NumPy API에 익숙하다면, Tensor API를 쉽게 이용할 수 있다.

**표준 NumPy 같은 인덱싱과 슬라이싱**  


```python
tensor = torch.ones(4, 4)
tensor[:, 1] = 0
print(tensor)
```

    tensor([[1., 0., 1., 1.],
            [1., 0., 1., 1.],
            [1., 0., 1., 1.],
            [1., 0., 1., 1.]])
    

**텐서 조인하기**  
torch.cat을 사용하여 주어진 차원을 따라 일련의 텐서를 연결할 수 있다. torch.cat과 미묘하게 다른 또 다른 텐서 결합 연산인 [torch.stack](https://pytorch.org/docs/stable/generated/torch.stack.html)도 있다.


```python
t1 = torch.cat([tensor, tensor, tensor], dim=1)
print(t1)
```

    tensor([[1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
            [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
            [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
            [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.]])
    

**텐서 곱하기**  


```python
# Element-wise 곱하기
print(f"tensor.mul(tensor) \n {tensor.mul(tensor)} \n")
# Alternative syntax
print(f"tensor * tensor \n {tensor * tensor} \n")
```

    tensor.mul(tensor) 
     tensor([[1., 0., 1., 1.],
            [1., 0., 1., 1.],
            [1., 0., 1., 1.],
            [1., 0., 1., 1.]]) 
    
    tensor * tensor 
     tensor([[1., 0., 1., 1.],
            [1., 0., 1., 1.],
            [1., 0., 1., 1.],
            [1., 0., 1., 1.]]) 
    
    

**두 텐서 사이의 행렬 곱하기**  


```python
# Matrix-multiplication
print(f"tensor.matmul(tensor.T) \n {tensor.matmul(tensor.T)} \n")
# Alternative syntax
print(f"tensor @ tensor.T \n {tensor @ tensor.T} \n")
```

    tensor.matmul(tensor.T) 
     tensor([[3., 3., 3., 3.],
            [3., 3., 3., 3.],
            [3., 3., 3., 3.],
            [3., 3., 3., 3.]]) 
    
    tensor @ tensor.T 
     tensor([[3., 3., 3., 3.],
            [3., 3., 3., 3.],
            [3., 3., 3., 3.],
            [3., 3., 3., 3.]]) 
    
    

**내부 연산(In-place operations)**  
언더바(_) 표기가 있는 연산은 In-place로 수행된다. 예를 들어, `x.copy_(y)`, `x.t_()`는 `x`를 변형시킨다.


```python
print(tensor, "\n")
tensor.add_(5)
print(tensor)
```

    tensor([[1., 0., 1., 1.],
            [1., 0., 1., 1.],
            [1., 0., 1., 1.],
            [1., 0., 1., 1.]]) 
    
    tensor([[6., 5., 6., 6.],
            [6., 5., 6., 6.],
            [6., 5., 6., 6.],
            [6., 5., 6., 6.]])
    

**`Note`** In-place 연산은 메모리 절약에 도움이 되지만, 즉각적인 히스토리 손실로 인해 미분 계산 시 문제가 될 수 있다. 따라서 해당 연산은 비추천된다.

<br>

### NumPy를 사용한 브릿지

CPU 및 NumPy 배열의 텐서는 기본 메모리 위치를 공유할 수 있으며 하나를 변경하면 다른 하나도 변경된다.

**Tensor를 NumPy 배열로 변환**  


```python
t = torch.ones(5)
print(f"t: {t}")

n = t.numpy()
print(f"n: {n}")
```

    t: tensor([1., 1., 1., 1., 1.])
    n: [1. 1. 1. 1. 1.]
    

**텐서의 변경은 NumPy 배열에 반영된다**


```python
t.add_(1)
print(f"t: {t}")
print(f"n: {n}")
```

    t: tensor([2., 2., 2., 2., 2.])
    n: [2. 2. 2. 2. 2.]
    

**NumPy 배열을 Tensor로 변환**


```python
n = np.ones(5)
t = torch.from_numpy(n)
```

**NumPy 배열의 변경은 텐서에 반영된다**


```python
np.add(n, 1, out=n)
print(f"t: {t}")
print(f"n: {n}")
```

    t: tensor([2., 2., 2., 2., 2.], dtype=torch.float64)
    n: [2. 2. 2. 2. 2.]
   
<br>

## 파이토치 60분 블리츠

[원문 링크](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)

- [PyTorch 튜토리얼 - 01 텐서(Tensors)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_01/)
- [PyTorch 튜토리얼 - 02 자동미분(autograd)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_02/)
- [PyTorch 튜토리얼 - 03 신경망(Neural Networks)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_03/)
- [PyTorch 튜토리얼 - 04 분류기 학습(Training a classifier)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_04/)

