---
title: "캐글 튜토리얼 02 (Kaggle Tutorial)-의사결정나무(Decision Trees)로 예측하기"
date: 2018-12-19 20:00:00 -0400
categories: Tutorial
tags: Kaggle
---
## Step 2 : Decision Trees로 예측하기

**1) 의사결정나무(Decision Trees)**

[이전 스텝]((https://wooiljeong.github.io/tutorial/kaggle_tutorial_01/))에서, 더 높은 생존 가능성을 갖는 부분 집합을 찾기 위해 슬라이싱과 다이싱을 해보았다. 의사결정나무는 이 과정을 자동화해주고, 분류 모델 혹은 분류기를 만들어준다.
개념적으로, 의사결정나무 알고리즘은 뿌리 노드에서 모든 데이터르르 가지고 시작하고, 가장 적합하게 분할하는 방법을 찾기 위해 모든 변수들을 탐색한다. 일단 변수 하나가 선택되면, 분할을 수행하고, 한 단계(혹은 한 노드) 아래로 이동하고 이를 계속해서 반복한다. 의사결정나무의 가장 아래 부분의 마지막 노드는 터미널 노드로 알려져 있다. 해당 노드에서 관측치들의 다수표가 새로운 관측치의 정보를 예측하는 방법을 결정한다.

우선, 필수 라이브러리들을 임포트한다.

```python
import numpy as np
from sklearn import tree
```


<br>

**2) 데이터 클리닝과 포매팅(Cleaning and Formatting your Data)**

의사결정나무를 만들기에 앞서 데이터를 먼저 클리닝하는 작업을 진행해야한다. 그래야 분석에 필요한 가능한 모든 특징들을 사용할 수 있다. [Step 1](https://wooiljeong.github.io/tutorial/kaggle_tutorial_01/)에서 나이(Age) 변수는 결측치를 포함하고 있었다. 결측은 그 자체로 커다란 주제이지만 여기에선 중앙값으로 대체 하는 간단한 기술을 적용해 볼 것이다.

```python
train["Age"] = train["Age"].fillna(train["Age"].median())
```

또 다른 문제는 ```Sex```와 ```Embarked``` 변수가 범주형이지만 숫자형이 아니라는 점이다. 그러므로 각 범주에 고유한 정수를 할당하여 파이썬으로 정보를 핸들링할 수 있도록 만들어줘야 한다. ```Embarkded``` 역시 약간의 결측치를 가지고 있는데 가장 흔한 범주인 ```S``` 값으로 대체하는 방법이 있다.

```python
# 'Sex'변수가 남성이면 0, 여성이면 1을 할당
train["Sex"][train["Sex"] == "male"] = 0
train["Sex"][train["Sex"] == "female"] = 1

# 'Embarked'변수의 결측치를 "S"로 대체
train["Embarked"] = train["Embarked"].fillna("S")

# 문자 형식의 범주형 변수를 정수형으로 변환
train["Embarked"][train["Embarked"] == "S"] = 0
train["Embarked"][train["Embarked"] == "C"] = 1
train["Embarked"][train["Embarked"] == "Q"] = 2

#변환된 'Sex', 'Embarked' 변수 출력하기
print(train["Sex"])
print(train["Embarked"])
```

<br>

**3) 의사결정나무(Decision Trees) 만들기**

의사결정나무를 만들기 위해 ```scikit-learn```과 ```numpy``` 라이브러리를 사용할 것이다. ```scikit-learn```은 ```DecisionTreeClassifier``` 클래스로부터 ```tree``` 객체를 만드는데 사용된다. 우리가 사용할 메소드는 입력으로 ```numpy``` 배열을 취한다. 그러므로 미리 정의된```DataFrame```으로부터 이를 만들어야한다. 의사결정나무를 만들기 위해서는 다음이 필요하다.

- ```target``` : 학습 데이터의 목표/반응을 담고 있는 1차원 넘파이 배열 (ex. 'Survived')
- ```features``` : 학습 데이터의 특징/변수를 담고 있는 다차원 넘파이 배열 (ex. 'Sex', 'Age')

```python
target = train["Survived"].values
features = train[["Sex", "Age"]].values
my_tree = tree.DecisionTreeClassifier()
my_tree = my_tree.fit(features, target)
```

의사결정나무의 결과를 빠르게 확인하는 한 가지 방법은 포함된 특징들의 중요도를 보는 것이다. 이는 나무 객체의 ```.feature_importance_``` 속성을 요청하여 수행된다. 또 다른 빠른 결과 확인 측도는 ```features_one```과 ```target```를 인수로 하는 ```.score()``` 함수를 사용해 계산할 수 있는 평균 정확도가 있다.

```python
# 목표와 특징 넘파이 배열 생성 : target, features_one
target = train["Survived"].values
features_one = train[["Pclass", "Sex", "Age", "Fare"]].values

# 의사결정나무 적합시키기 : my_tree_one
my_tree_one = tree.DecisionTreeClassifier()
my_tree_one = my_tree_one.fit(features_one, target)

# 의사결정나무에 포함된 특징들의 중요도와 스코어
print(my_tree_one.feature_importances_)
# 결과 : [0.12063997 0.31274009 0.22489714 0.3417228 ]
print(my_tree_one.score(features_one, target))
# 결과 : 0.9775533108866442
```

<br>

**4) 의사결정나무(Decision Trees) 해석하기**

의사결정나무의 결과를 해석할 때 ```feature_importance_``` 속성을 사용하면 포함시키는 예측 변수의 의미를 간단하게 이해할 수 있다. 의사결정나무에 기반할 때, 어떤 변수가 배에 탑승한 사람들의 생존 여부에 가장 큰 영향을 주는지 알아 볼 것이다. 위 예에서 'Fare' 변수의 중요도가 0.34로 비교군 중 가장 높아 생존 여부에 가장 큰 영향을 주었다는 것을 알 수 있다.


<br>

**5) 예측하고 캐글에 제출하기**

캐글에 ```Submission```을 보내기 위해서는 테스트 데이터 셋의 관측치에 대해 생존율을 예측해야한다. 이전 챕터의 마지막 예제에서, 단일 부분집합에 기초해 간단한 예측을 했다. 다행히도 의사결정트리를 사용하면 일일이 하위 집합을 만드는 작업을 수행하지 않고도 예측값을 생성하는 몇 가지 간단한 기능을 사용할 수 있다.

첫째로, ```.predict()``` 메소드를 사용한다.


<br>

**6) 과적합과 이를 해결하기 위한 방법**




<br>

**7) 타이타닉 데이터 셋을 위한 Feature Engineering**














<br>
