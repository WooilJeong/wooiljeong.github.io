---
title: "캐글(Kaggle) - 튜토리얼 02"
date: 2018-12-19 00:00:00 -0000
categories: ETC
tags: Kaggle
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---
## Step 2 : 의사결정나무(Decision Trees)로 예측하기

**1) 의사결정나무(Decision Trees)**

이전에 포스팅한 [Step 1](https://wooiljeong.github.io/tutorial/kaggle_tutorial_01/)에서, 더 높은 생존 가능성을 갖는 부분 집합을 찾기 위해 슬라이싱과 다이싱을 해보았다. 의사결정나무는 이 과정을 자동화해주고, 분류 모델 혹은 분류기를 만들어준다.
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

첫째로, ```.predict()``` 메소드를 사용한다. 이를 이용해 ```my_tree_one``` 모델에 예측을 위한 테스트 데이터 셋의 피처값들을 넣어준다. 피처를 추출하려면 모델을 학습 할 때와 마찬가지로 numpy 배열을 만들어야한다. 그러나, 작지만 중요한 문제를 먼저 처리해야한다. ```Fare```피처에는 결측값이 있어 대체값을 넣어줘야한다.

다음으로, 결과가 Kaggle의 제출 요구 사항과 일치하는지 확인해야한다. 정확히 418개의 항목과 2개의 컬럼이 존재하는 csv파일인지를 점검한다 : ```PassengerId```, ```Survived```

그런 다음 제공된 코드를 사용하여 ```DataFrame()```을 사용하여 새 데이터 프레임을 만들고 판다스의 ```to_csv()``` 메소드를 사용하여 csv파일을 만든다.


```python
# 중앙값으로 결측치 대체
#방법1
test.Fare[152] = test.Fare.median()
#방법2
test["Fare"][test["Fare"].isnull()==True] = test["Fare"].median()

# 테스트 데이터 셋으로부터 피처 추출 : Pclass, Sex, Age, and Fare.
test_features = test[['Pclass', 'Sex', 'Age', 'Fare']].values

# 테스트 데이터 셋을 이용하여 예측 수행
my_prediction = my_tree_one.predict(test_features)

# 두 컬럼으로 데이터프레임 만들기 : PassengerId & Survived
# Survived는 예측값을 담고 있음.
PassengerId = np.array(test["PassengerId"]).astype(int)
my_solution = pd.DataFrame(my_prediction, PassengerId, columns = ["Survived"])
print(my_solution)

# 데이터프레임이 418개 관측치를 가지고 있는지 확인
print(my_solution.shape)

# 예측 결과를 csv 파일로 저장
my_solution.to_csv("my_solution_one.csv", index_label = ["PassengerId"])
```


<br>

**6) 과적합과 이를 해결하기 위한 방법**

처음 의사결정나무를 만들 때, ```max_depth```와 ```min_samples_split```옵션은 ```None```이 디폴트 값으로 되어 있다. 이는 나무의 깊이에 제한이 없다는 것을 의미한다. 이는 빠르진 않지만 꽤 좋아보인다. 그렇지만 이 경우 과적합될 것이다. 즉, 모델이 학습 데이터를 매우 잘 설명하지만 실제로는 예측의 목적인 새로운 데이터에 대해 일반화가 어렵다는 것이다. 복잡한 의사결정나무 모델과 성별을 기반으로 한 간단한 모델에 대한 Kaggle 제출 결과를 살펴보면 어느 쪽이 더 나은지 알 수 있을 것이다.
어쩌면 덜 복잡한 모델을 만들어 복잡한 모델을 개선할 수도 있을 것이다. ```DecisionTreeRegressor```에서 모델의 깊이는 두 파라미터에 의해 정의된다 :

- ```max_depth``` : 의사결정나무의 분할이 중지되는 시점을 결정
- ```min_samples_split``` : Bucket에 있는 관측치의 양을 모니터링 함. 특정 임계값에 도달하지 못하면 더 이상 분할을 수행할 수 없음.(ex.최소 10명의 승객)

의사결정나무의 복잡도를 줄임으로써 모델의 일반화 성능과 예측의 유용성을 향상시킬 수 있다.

```python
# 피처들을 추가하여 features_two 생성
features_two = train[["Pclass","Age","Sex","Fare", "SibSp", "Parch", "Embarked"]].values
target = train["Survived"].values

# "max_depth"를 10, "min_samples_split"을 5로 설정하여 과적합 조절 : my_tree_two
max_depth = 10
min_samples_split = 5
my_tree_two = tree.DecisionTreeClassifier(max_depth = max_depth, min_samples_split = min_samples_split, random_state = 1)
my_tree_two = my_tree_two.fit(features_two, target)

# 새 의사결정나무의 Score 출력
print(my_tree_two.score(features_two, target))
# 결과:0.9057239057239057
```


<br>

**7) 타이타닉 데이터 셋을 위한 Feature Engineering**

데이터 과학은 인간 요소(Human element)의 혜택을 받는 기술이다. 피처 엔지니어링을 시작하자. 기존의 다양한 변수를 결합해 피처들을 창의적으로 엔지니어링할 것이다.

여기에서 피처 엔지니어링 그 자체를 자세히 살펴보는 것은 범위가 너무 넓기 때문에 어렵다. 새로운 예측 속성인 ```family_size```를 만들어 간단한 예제를 살펴볼 것이다.

배가 가라 앉는 상황에서 대가족인 경우 모이는 시간이 더 필요하기 때문에 생존 가능성이 낮다고 가정하는 것이 타당하다고 볼 수 있다. 가족의 크기는 ```SibSp``` 및 ```Parch``` 변수에 의해 결정된다. 이 변수는 특정 승객이 동행하는 가족 수를 나타낸다. 따라서 피처 엔지니어링을 할 때, 하습 및 테스트 데이터 셋에```SibSp```와 ```Parch```의 합계에 1을 합한 ```family_size```변수를 새로 만들 것이다.

- 새 학습 데이터 셋 ```train_two```을 생성하고 피처 엔지니어링한 ```family_size``` 변수를 추가한다.
- 새 의사결정나무 ```my_tree_three```를 만들어 새 피처 셋 ```features_three```에 적합 시키고 스코어를 확인한다.

```python
# 새로 정의한 피처를 추가해 train_two 생성
train_two = train.copy()
train_two["family_size"] = train_two["SibSp"] + train_two["Parch"] + 1

# 새 피처 셋을 생성하고 새 피처를 추가
features_three = train_two[["Pclass", "Sex", "Age", "Fare", "SibSp", "Parch", "family_size"]].values
target = train["Survived"].values

# 의사결정나무를 정의하고, 모델 적합
my_tree_three = tree.DecisionTreeClassifier()
my_tree_three = my_tree_three.fit(features_three, target)

# 의사결정나무의 스코어를 출력
print(my_tree_three.score(features_three, target))
# 결과 : 0.97979797979798
```



<br>
