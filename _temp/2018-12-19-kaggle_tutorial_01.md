---
title: "캐글 튜토리얼 01 (Kaggle Tutorial)-파이썬(Python)으로 시작하기"
date: 2018-12-19 20:00:00 -0400
categories: Tutorial
tags: Kaggle
---
## Step 1 : 파이썬으로 시작하기
캐글에 입문할 때 흔히 접하게 되는 대회 중 하나인 '타이타닉 대회'를 다루도록 한다. 이 대회의 문제를 해결하기 위해 파이썬과 머신러닝 기법이 사용되므로 간단한 예제를 살펴본다.



**1) 변수 정의 및 출력**
```Python
# x = 4 * 3 를 정의하고, x 를 출력
x = 4 * 3
print(x)

# y = 6 * 9 를 정의하고, x를 출력
y = 6 * 9
print(y)
```

**2) Pandas로 데이터 가져오기**
타이타닉 호가 침몰했을 때, 배에 탑승하고 있던 2,224명의 승객과 승무원 중 1,502명이 사망했다. 영화 '타이타닉'을 참고하면 배에 탑승하고 있던 사람들은 저마다 침몰에서 생존할 가능성이 달랐을 것이라고 추측할 수 있다. 파이썬과 머신러닝 기법을 이용하여 사람들의 생존 확률을 예측하는 방법을 탐색해 볼 것이다. 이를 위해 우선 Pandas를 이용하여 파이썬 환경에 학습 데이터 셋과 테스트 데이터 셋을 불러와야한다. 학습 데이터 셋은 모델을 만들 때 사용하며, 테스트 데이터 셋은 모델을 검증할 때 사용한다. 데이터 셋은 웹에 ```csv``` 파일 형식으로 저장되어 있다. 파일 주소는 아래 코드 안에 문자 형식으로 준비되어 있다. Pandas의 ```read_csv()```메소드로 데이터를 불러올 수 있다.

```Python
# Pandas 라이브러리 임포트하기
import pandas as pd

# 학습 데이터 셋과 테스트 데이터 셋을 불러와 두 개의 데이터프레임 정의하기
train_url = "http://s3.amazonaws.com/assets.datacamp.com/course/Kaggle/train.csv"
train = pd.read_csv(train_url)

test_url = "http://s3.amazonaws.com/assets.datacamp.com/course/Kaggle/test.csv"
test = pd.read_csv(test_url)

#학습 및 테스트 데이터프레임의 head 출력하기
print(train.head())
print(test.head())
```

**3) Data 이해하기**
실제 분석을 시작하기 전에 데이터의 구조를 이해하는 것이 중요하다. 학습 및 테스트 데이터 셋은 판다스의 데이터 표현 방식인 데이터프레임으로 표현되어 있다. ```.describe()``` 메소드를 이용하여 쉽게 데이터프레임을 탐색해 볼 수 있다. ```.describe()```은 데이터프레임의 행/열을 요약(관측치의 수, 평균, 최대값 등)해준다. 또, ```.shape```메소드를 사용하면 데이터프레임의 차원을 확인 할 수 있다. (ex. ```your_data.shape```)

학습 및 테스트 데이터 셋은 ```train```, ```test```변수로 사용할 수 있으며 다음과 같이 ```.describe()```, ```.shape```메소드를 적용해 볼 수 있다.

```Python
train.describe()
test.describe()
train.shape
test.shape
```

**4) Rose vs Jack 혹은 여성 vs 남성**
학습 데이터 셋에서 얼마나 많은 사람들이 생존했는지를 파악하려면 ```value_counts()```메소드를 사용하면 된다. ```[]```표기를 이용하면 관심있는 컬럼을 선택할 수 있다.

```Python
# 빈도
train["Survived"].value_counts()
# 0: 549명
# 1: 342명

# 비율
train["Survived"].value_counts(normalize = True)
# 0: 0.616162
# 1: 0.383838
```

위 코드를 실행하면, 학습 데이터 셋에서 549명(62%)이 사망하고, 342명(38%)이 생존했음을 알 수 있다.
휴리스틱하게 예측하는 간단한 방법은 "다수 승" 방법이 될 수 있다. 이 방법은 새로운 데이터 셋에 대해 모두 사망할 것이라고 예측하는 것을 의미한다.

좀 더 깊이 들어가면, 생존 컬럼의 부분 집합에 대해서도 빈도, 비율 계산을 적용할 수 있다. 예를 들어, 성별이 생존에 주요한 변수인지를 확인하려면 ```.value_counts()``` 메소드를 이용하여 생존한 남성과 여성의 수에 대한 양방향 비교 방법을 적용할 수 있다.

```Python
train["Survived"][train["Sex"] == 'male'].value_counts()
train["Survived"][train["Sex"] == 'female'].value_counts()
```

비율을 구하기 위해서는 ```normalize = True``` 옵션을 ```.value_counts()``` 메소드에 추가하면 된다.

```Python
# 생존자 vs 사망자
print(train["Survived"].value_counts())

# 생존자 vs 사망자 (비율)
print(train["Survived"].value_counts(normalize = True))

# 생존 남성 vs 사망 남성
print(train["Survived"][train["Sex"]=='male'].value_counts())

# 생존 여성 vs 사망 여성
print(train["Survived"][train["Sex"]=='female'].value_counts())

# 생존 남성 vs 사망 남성 (비율)
print(train["Survived"][train["Sex"]=='male'].value_counts(normalize=True))

# 생존 여성 vs 사망 여성 (비율)
print(train["Survived"][train["Sex"]=='female'].value_counts(normalize=True))
```

**5) 나이(Age)는 주요한 변수인가?**
생존에 영향을 줄 수 있는 또다른 변수는 나이(Age)다. 어린이들을 먼저 구조할 수 있기 때문에 어린이인지 아닌지를 의미하는 범주형 변수 ```Child```를 만들어 볼 수 있다. ```Child```가 1이라는 값을 가지면 나이가 18세 미만이고 0이라는 값을 가지면 나이가 18세 이상을 의미하는 식이다.

이 새로운 변수를 추가하기 위해서 두 가지를 해야한다. 하나는 새 컬럼을 생성하고, 나머지 하나는 탑승자들의 나이에 근거해 각 관측치(Row) 별로 값을 주는 것이다.

파이썬에서는 다음과 같이 판다스를 이용하여 쉽게 새로운 컬럼을 생성할 수 있다.

```Python
train["Child"] = 0
```
`train`이라는 데이터프레임에 `Child`라는 컬럼을 생성하고 모든 관측치에 0값을 주는 코드이다.

탑승자들의 나이에 근거해 값을 설정하기 위해서 ```[]``` 연산자 안에서 'boolean test'를 해야한다.

```Python
train["Child"][train['Age']<18]=1
```
학습 데이터 셋에서 나이(Age)가 18세 미만인 사람들의 ```Child``` 변수에 해당하는 값으로 1을 할당한다.

```Python
train["Child"].value_counts()
# 0 : 778명 (18세 이상)
# 1 : 113명 (18세 미만)
```
학습 데이터 셋 내에 18세 미만의 어린이 113명과 18세 이상의 어른 778명이 존재함을 알 수 있다.

```Python
train["Survived"][train["Child"]==1].value_counts(normalize=True)
# 1 : 0.54 (생존한 어린이 비율)
# 0 : 0.46 (사망한 어린이 비율)
train["Survived"][train["Child"]==0].value_counts(normalize=True)
# 1 : 0.64 (생존한 어른 비율)
# 0 : 0.36 (사망한 어른 비율)
```
어른의 경우 생존 비율이 64%로 어린이의 경우 생존 비율 54% 더 높게 나타난 것을 확인할 수 있다.


**6) 첫 번째 예측**
위 내용을 종합하면 학습 데이터 셋에서 여성의 생존 비율은 50% 보다 크고, 남성의 생존 비율은 50% 보다 작다는 것을 알았다. 따라서 이와 같은 정보를 이용해 생존 가능성을 예측할 수 있는 모델을 가지게 되었다. 바로 테스트 데이터 셋의 모든 여성은 생존할 것이고, 그 밖의 모든 남성은 사망할 것이라고 예측하는 것이다.

이와 같은 예측을 검증하기 위해서 테스트 데이터 셋을 사용한다. 학습 데이터 셋과 달리 테스트 데이터 셋에는 ```Survived``` 컬럼이 없다. 예측된 값을 사용해 해당 컬럼을 추가하여 업로드하면 Kaggle 에서는 이 값을 실제 값과 비교하여 모델 예측 성능에 대한 점수를 부여한다.

```Python
# 테스트 데이터 셋 복사
test_one = test

# 'Survived' 변수의 값을 모든 관측치에 대해 0으로 부여
test_one["Survived"] = 0

# 테스트 데이터 셋의 여성에 한해 'Survived' 변수의 값을 1로 변경 및 결과 출력
test_one["Survived"][test_one["Sex"]=="female"] = 1
print(test_one["Survived"])
```
성별을 기준으로 '남성'은 사망, '여성'은 생존으로 예측하는 모델을 테스트 데이터 셋에 적용하여 'Survived' 변수와 각 관측치에 해당하는 값을 생성하였다.




[캐글 튜토리얼](https://www.datacamp.com/community/open-courses/kaggle-python-tutorial-on-machine-learning)
[파이썬 튜토리얼](https://www.datacamp.com/courses/intro-to-python-for-data-science)
[Pandas 기초](https://doorbw.tistory.com/172)
