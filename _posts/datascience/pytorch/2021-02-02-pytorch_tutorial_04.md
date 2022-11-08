---
title: "PyTorch 튜토리얼 - 04 분류기 학습(Training a classifier)"
categories: pytorch
tags: python
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

> # PyTorch 튜토리얼 - 04 분류기 학습(Training a classifier)

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

## 분류기 학습(Training a Classifier)

이전 튜토리얼에서 신경망을 정의하고 손실 값을 계산하며 또, 가중치를 업데이트하는 방법에 대해 살펴보았다. 

<br>

### 데이터 핸들링하기

보통 이미지, 텍스트, 오디오 또는 영상 데이터를 처리해야 할 때, 데이터를 NumPy 배열로 불러오는 표준 Python 패키지를 사용할 수 있다. 그런 다음 이 배열을 `torch.Tensor`로 변환 할 수 있다.

**데이터 유형 별 유용한 라이브러리**  

- 이미지 데이터: Pilow, OpenCV
- 오디오 데이터: scipy, librosa
- 텍스트 데이터: NLTK, SpaCy

특히, 컴퓨터 비전 분야의 경우에는 `torchvision` 패키지가 유용하다. 이 패키지에는 Imagenet, CIFAR10, MNIST 등과 같은 데이터를 불러올 수 있는 데이터 로더와 이미지 변환기가 포함되어 있다. (`torchvision.datasets`, `torch.utils.data.DataLoader`)

여기에서는 CIFAR10 데이터 세트를 사용한다. 이 데이터에는 '비행기', '자동차', '새', '고양이', '사슴', '개', '개구리', '말', '배', '트럭' 과 같은 클래스가 존재한다. CIFAR10의 이미지 크기는 3x32x32이다. 즉, 3개의 컬러(Red, Green, Blue) 채널과 32x32 픽셀을 의미한다.

**CIFAR10**  
![image.png](https://pytorch.org/tutorials/_images/cifar10.png){: .align-center}

<br>

### 이미지 분류기 학습

이미지 분류기 학습을 위해 다음 단계를 수행한다.

1. `torchvision`을 이용해 CIFAR10 학습 이미지와 테스트 이미지를 불러와 정규화한다.
2. 합성곱 신경망(Convolutional Neural Network)을 정의한다.
3. 손실 함수(Loss Function)을 정의한다.
4. 학습 데이터로 신경망을 학습한다.
5. 테스트 데이터로 신경망을 테스트한다.

<br>

### 1. CIFAR10 데이터 로딩 및 정규화

`torchvision`을 이용하면 쉽게 CIFAR10 데이터를 불러올 수 있다.


```python
import torch
import torchvision
import torchvision.transforms as transforms
```

`torchvision`으로 불러온 데이터 세트의 출력은 [0, 1] 범위의 PILImage 이미지이다. 이 이미지를 [-1, 1] 범위로 정규화된 텐서로 만든다.


```python
transform = transforms.Compose(
    [transforms.ToTensor(),
     transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))])

trainset = torchvision.datasets.CIFAR10(root='./data', train=True,
                                        download=True, transform=transform)
trainloader = torch.utils.data.DataLoader(trainset, batch_size=4,
                                          shuffle=True, num_workers=2)

testset = torchvision.datasets.CIFAR10(root='./data', train=False,
                                       download=True, transform=transform)
testloader = torch.utils.data.DataLoader(testset, batch_size=4,
                                         shuffle=False, num_workers=2)

classes = ('plane', 'car', 'bird', 'cat',
           'deer', 'dog', 'frog', 'horse', 'ship', 'truck')
```

    Downloading https://www.cs.toronto.edu/~kriz/cifar-10-python.tar.gz to ./data/cifar-10-python.tar.gz



    0it [00:00, ?it/s]


    Extracting ./data/cifar-10-python.tar.gz to ./data
    Files already downloaded and verified


학습 이미지를 확인해보자.


```python
import matplotlib.pyplot as plt
import numpy as np

# functions to show an image


def imshow(img):
    img = img / 2 + 0.5     # unnormalize
    npimg = img.numpy()
    plt.imshow(np.transpose(npimg, (1, 2, 0)))
    plt.show()


# get some random training images
dataiter = iter(trainloader)
images, labels = dataiter.next()

# show images
imshow(torchvision.utils.make_grid(images))
# print labels
print(' '.join('%5s' % classes[labels[j]] for j in range(4)))
```


    
![png](/assets/img/post_img/2021-02-02-pytorch_tutorial/output_10_0.png)
    


    truck  bird horse plane

<br>

### 2. 합성곱 신경망(Convolutional Neural Network) 정의


```python
import torch.nn as nn
import torch.nn.functional as F


class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = x.view(-1, 16 * 5 * 5)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x


net = Net()
```

<br>

### 3. 손실 함수(Loss Function) 및 옵티마이저 정의

크로스-엔트로피 손실(Cross-Entropy loss)과 모멘텀을 고려한 SGD 옵티마이저를 정의한다.


```python
import torch.optim as optim

criterion = nn.CrossEntropyLoss()
optimizer = optim.SGD(net.parameters(), lr=0.001, momentum=0.9)
```

<br>

### 4. 신경망 학습

이제 데이터를 반복해서 신경망에 입력해 최적화하기만 하면 된다.


```python
for epoch in range(2):  # loop over the dataset multiple times

    running_loss = 0.0
    for i, data in enumerate(trainloader, 0):
        # get the inputs; data is a list of [inputs, labels]
        inputs, labels = data

        # zero the parameter gradients
        optimizer.zero_grad()

        # forward + backward + optimize
        outputs = net(inputs)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()

        # print statistics
        running_loss += loss.item()
        if i % 2000 == 1999:    # print every 2000 mini-batches
            print('[%d, %5d] loss: %.3f' %
                  (epoch + 1, i + 1, running_loss / 2000))
            running_loss = 0.0

print('Finished Training')
```

    [1,  2000] loss: 2.174
    [1,  4000] loss: 1.816
    [1,  6000] loss: 1.648
    [1,  8000] loss: 1.561
    [1, 10000] loss: 1.490
    [1, 12000] loss: 1.449
    [2,  2000] loss: 1.370
    [2,  4000] loss: 1.338
    [2,  6000] loss: 1.311
    [2,  8000] loss: 1.319
    [2, 10000] loss: 1.284
    [2, 12000] loss: 1.268
    Finished Training


학습된 모델을 저장해보자.


```python
PATH = './cifar_net.pth'
torch.save(net.state_dict(), PATH)
```

PyTorch 모델 저장에 대한 자세한 내용은 [여기](https://pytorch.org/docs/stable/notes/serialization.html)를 참조하면 된다.

<br>

### 5. 테스트 데이터로 신경망 테스트

신경망을 학습할 때, 학습 데이터를 총 2회 모델에 통과되도록 했다. 이제 신경망 모델이 학습한 것이 있는지 확인해봐야한다. 

신경망이 출력하는 클래스 레이블 예측값을 실제값과 비교하여 확인한다. 예측이 정확하다면 정답 예측 리스트에 해당 샘플을 추가한다.

테스트 이미지를 확인해보자.


```python
dataiter = iter(testloader)
images, labels = dataiter.next()

# print images
imshow(torchvision.utils.make_grid(images))
print('GroundTruth: ', ' '.join('%5s' % classes[labels[j]] for j in range(4)))
```


    
![png](/assets/img/post_img/2021-02-02-pytorch_tutorial/output_21_0.png)
    


    GroundTruth:    cat  ship  ship plane


저장된 모델을 다시 불러와보자. 


```python
net = Net()
net.load_state_dict(torch.load(PATH))
```




    <All keys matched successfully>



불러온 신경망 모델이 위 이미지를 보고 어떻게 예측하는지 확인해보자.


```python
outputs = net(images)
```

출력은 10개 클래스에 대한 가능도을 의미한다. 특정 클래스에 대한 가능도가 높을 수록, 신경망은 이미지가 특정 클래스라고 생각한다. 가장 가능도가 높은 클래스의 인덱스를 구해보자.


```python
_, predicted = torch.max(outputs, 1)

print('Predicted: ', ' '.join('%5s' % classes[predicted[j]]
                              for j in range(4)))
```

    Predicted:    cat  ship   car  ship


예측 성능이 나쁘지 않다.

신경망이 전체 데이터 세트에 대해 어떻게 동작하는지 확인해보자.


```python
correct = 0
total = 0
with torch.no_grad():
    for data in testloader:
        images, labels = data
        outputs = net(images)
        _, predicted = torch.max(outputs.data, 1)
        total += labels.size(0)
        correct += (predicted == labels).sum().item()

print('Accuracy of the network on the 10000 test images: %d %%' % (
    100 * correct / total))
```

    Accuracy of the network on the 10000 test images: 56 %


10개 클래스 중에서 무작위로 클래스를 선택하여 정답일 확률이 10%인 것을 감안하면, 신경망의 예측 정확도가 꽤 높아 보인다.

예측이 잘 된 경우와 그렇지 않은 경우는 어던 차이가 있을까?


```python
class_correct = list(0. for i in range(10))
class_total = list(0. for i in range(10))
with torch.no_grad():
    for data in testloader:
        images, labels = data
        outputs = net(images)
        _, predicted = torch.max(outputs, 1)
        c = (predicted == labels).squeeze()
        for i in range(4):
            label = labels[i]
            class_correct[label] += c[i].item()
            class_total[label] += 1


for i in range(10):
    print('Accuracy of %5s : %2d %%' % (
        classes[i], 100 * class_correct[i] / class_total[i]))
```

    Accuracy of plane : 65 %
    Accuracy of   car : 82 %
    Accuracy of  bird : 26 %
    Accuracy of   cat : 37 %
    Accuracy of  deer : 62 %
    Accuracy of   dog : 38 %
    Accuracy of  frog : 67 %
    Accuracy of horse : 61 %
    Accuracy of  ship : 65 %
    Accuracy of truck : 58 %

<br>

### GPU 환경에서 학습

Tensor를 GPU로 이동시키는 것과 마찬가지로 신경망을 GPU로 이동시킨다. 
CUDA를 사용할 수 있다면, 가장 처음 보이는 CUDA 장치를 선택한다. 


```python
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")

# Assuming that we are on a CUDA machine, this should print a CUDA device:

print(device)
```

    cuda:0


신경망을 CUDA 장치로 보낸다.


```python
net.to(device)
```




    Net(
      (conv1): Conv2d(3, 6, kernel_size=(5, 5), stride=(1, 1))
      (pool): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
      (conv2): Conv2d(6, 16, kernel_size=(5, 5), stride=(1, 1))
      (fc1): Linear(in_features=400, out_features=120, bias=True)
      (fc2): Linear(in_features=120, out_features=84, bias=True)
      (fc3): Linear(in_features=84, out_features=10, bias=True)
    )



입력값과 타겟값도 CUDA 장치로 보낸다.


```python
inputs, labels = data[0].to(device), data[1].to(device)
```

CPU 환경에서 GPU 환경으로 변경했음에도 학습 속도가 느릴 수 있는데, 그 이유는 신경망 자체가 작기 때문이다.

<br>

### 여러 GPU에서 학습

모든 GPU를 사용하여 더 빠른 학습 속도를 내고 싶다면 [데이터 병렬 처리](https://pytorch.org/tutorials/beginner/blitz/data_parallel_tutorial.html)를 참조하면 된다.

<br>

### 추가 참고 링크

- [Train neural nets to play video games](https://pytorch.org/tutorials/intermediate/reinforcement_q_learning.html)
- [Train a state-of-the-art ResNet network on imagenet](https://github.com/pytorch/examples/tree/master/imagenet)
- [Train a face generator using Generative Adversarial Networks](https://github.com/pytorch/examples/tree/master/dcgan)
- [Train a word-level language model using Recurrent LSTM networks](https://github.com/pytorch/examples/tree/master/word_language_model)
- [More examples](https://github.com/pytorch/examples)
- [More tutorials](https://github.com/pytorch/tutorials)
- [Discuss PyTorch on the Forums](https://discuss.pytorch.org/)
- [Chat with other users on Slack](https://pytorch.slack.com/?redir=%2Fmessages%2Fbeginner%2F)

<br>

## 파이토치 60분 블리츠

[원문 링크](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html)

- [PyTorch 튜토리얼 - 01 텐서(Tensors)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_01/)
- [PyTorch 튜토리얼 - 02 자동미분(autograd)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_02/)
- [PyTorch 튜토리얼 - 03 신경망(Neural Networks)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_03/)
- [PyTorch 튜토리얼 - 04 분류기 학습(Training a classifier)](https://wooiljeong.github.io/pytorch/pytorch_tutorial_04/)

