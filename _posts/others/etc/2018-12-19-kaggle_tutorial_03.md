---
title: "캐글(Kaggle) - 튜토리얼 03"
categories: etc
tags: Kaggle
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---
## Step 3 : 랜덤포레스트(Random Forest)로 예측하기

**1) 파이썬으로 랜덤 포레스트 분석하기**

랜덤 포레스트에 대한 자세한 내용은 이 튜토리얼의 범위를 벗어나므로 여기서는 깊이있게 다루지는 않는다. 그러나 일반적인 수준에서 랜덤 포레스트에 대해 이해하는 것만으로도 머신러닝에 대해 이해하는데 도움이 될 것이다.

일반인의 관점에서 볼 때, 랜덤 포레스트 기법은 의사결정나무에서의 과적합 문제를 처리할 수 있게 도와준다. 학습 데이터 셋을 사용하여 여러 개의 분류 나무들을 (매우 깊게)성장시킬 것이다. 예측을 수행할 때, 각 나무의 예측 결과는 최종 예측을 위한 일종의 투표로 계산된다. 예를 들어, 학습된 나무 중 2개는 테스트 셋의 승객이 생존할 것이라 예측하는 반면, 나머지 1개는 생존하지 못할 것이라고 예측하는 경우, 해당 승객은 생존할 것이라고 최종 판단을 내리는 식이다. 나무들을 과적합시키는 이와 같은 접근법은 실제로 최종 판단을 할 때에는 다수결에 의한 예측을 하므로 과적합을 피할 수 있게 된다.

파이썬에서 랜덤 포레스트를 만드는 것은 의사 결정 나무를 만드는 것과 거의 같지만 큰 차이점 두 가지가 존재한다. 첫째로 다른 클래스가 사용된다. 둘째로 새로운 인자가 필요하다. 또한, 사이킷 런에서 필요한 라이브러리를 가져와야한다.

여기에서는 ```DecisionTreeClassifier()``` 클래스 대신에 ```RandomForestClassifier()``` 클래스를 사용한다. ```n_estimators```는 ```RnadomForestClassifier()``` 클래스를 사용할 때 설정해야한다. 이 인자는 고려하고자 하는 나무의 수를 설정할 수 있게 해준다.

```python
# `RandomForestClassifier` 임포트 하기
from sklearn.ensemble import RandomForestClassifier

# 'Pclass, Age, Sex, Fare, SibSp, Parch, and Embarked variables'
features_forest = train[["Pclass", "Age", "Sex", "Fare", "SibSp", "Parch", "Embarked"]].values

# my_forest 생성 및 적합시키기
forest = RandomForestClassifier(max_depth = 10, min_samples_split=2, n_estimators = 100, random_state = 1)
my_forest = forest.fit(features_forest, target)

# 적합된 랜덤포레스트 모델의 스코어 값 산출하기
print(my_forest.score(features_forest, target))

# 테스트 셋 특징 예측값 계산 및 예측 벡터의 길이 출력
test_features = test[["Pclass", "Age", "Sex", "Fare", "SibSp", "Parch", "Embarked"]].values
pred_forest = my_forest.predict(test_features)
print(len(pred_forest))
```


**2) 해석 및 비교하기**

의사 결정 나무의 ```.feature_importances_``` 속성을 어떻게 고려했는지 기억하자. 랜덤 포레스트에서도 동일한 속성을 요청하고 포함된 변수의 관련성을 해석할 수 있다. 모델을 빠르고 쉽게 비교할 수도 있다. 이를 위해 ```score()``` 메소드를 사용할 수 있다. ```score()``` 메소드는 특징 데이터와 타겟 벡터를 가지고, 모델의 평균 정확도를 계산한다. 이 방법은 의사 결정 나무 및 랜덤 포레스트 둘 모두에서 사용할 수 있다. 중요한 점은 이 측도의 값이 높은 것이 좋지만, 극단적으로 높을 경우 과적합일 수 있으므로 주의해야 한다는 것이다.

```python
# `.feature_importances_` 속성 요청 및 출력
print(my_tree_two.feature_importances_)
print(my_forest.feature_importances_)

# 두 모델의 평균 정확도 값 계산 및 출력
print(my_tree_two.score(features_two, target))
print(my_forest.score(features_forest, target))
```


**3) 결론 및 제출**

이전 튜토리얼에서 발견한 내용을 토대로 어떤 기능이 가장 중요한지 그리고 어떤 모델인지 판단해보자. 마지막 튜토리얼이 끝나면 랜덤 포레스트 모델을 캐글에 제출할 수 있다. ```my_forest```, ```my_tree_two``` 및 ```feature_importances_```를 사용하여 질문에 답해보자.


```
1)
The most important feature was "Age", but it was more significant for "my_tree_two"

2)
The most important feature was "Sex", but it was more significant for "my_tree_two"

3)
The most important feature was "Sex", but it was more significant for "my_forest"

4)
The most important feature was "Age", but it was more significant for "my_forest"
```

```
정답은 2)
```
