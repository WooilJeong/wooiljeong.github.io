---
title: Lazypredict를 이용해 ML모델 선택하기(Classification)
categories: ml
tags: lazypredict
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

Python AutoML 라이브러리 중 하나인 Lazypredict를 이용해 여러 ML 모델들을 동시에 학습하고, 예측 성능을 비교해보자.

<br>

## Lazypredict

Lazy Predict는 인도의 어느 시니어 데이터 사이언티스트인 Shankar Rao Pandala라는 개인이 개발한 오픈소스 머신러닝 자동화 관련 파이썬 오픈소스 프로젝트이다. 현재는 Classification과 Regression에 대한 기능만 제공되고 있다. Lazypredict를 이용하면 코드 한 줄로 여러 ML 모델을 불러와 학습시킬 수 있고, 추론 결과도 확인할 수 있다. 여러 모델들의 성능 지표도 비교할 수 있어 성능이 더 좋은 모델을 가려낼 수도 있다. 다만, 파라미터를 조정하는 기능은 따로 제공되지 않는다는 한계가 있다. 

<br>

## 예제 데이터 로드

예제 데이터를 아래 링크에서 다운로드받아 Pandas DataFrame으로 로드한다. 예제 데이터는 심장병 해당 여부(target)가 포함된 데이터로 age, sex, cp 등의 특성을 통해 심장병 해당여부를 예측해보자. 

[Heart Disease Dataset](https://www.kaggle.com/datasets/johnsmith88/heart-disease-dataset)


```python
import tqdm
import pandas as pd

df = pd.read_csv("./data/heart.csv")
```


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1025 entries, 0 to 1024
    Data columns (total 14 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   age       1025 non-null   int64  
     1   sex       1025 non-null   int64  
     2   cp        1025 non-null   int64  
     3   trestbps  1025 non-null   int64  
     4   chol      1025 non-null   int64  
     5   fbs       1025 non-null   int64  
     6   restecg   1025 non-null   int64  
     7   thalach   1025 non-null   int64  
     8   exang     1025 non-null   int64  
     9   oldpeak   1025 non-null   float64
     10  slope     1025 non-null   int64  
     11  ca        1025 non-null   int64  
     12  thal      1025 non-null   int64  
     13  target    1025 non-null   int64  
    dtypes: float64(1), int64(13)
    memory usage: 112.2 KB
    


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>sex</th>
      <th>cp</th>
      <th>trestbps</th>
      <th>chol</th>
      <th>fbs</th>
      <th>restecg</th>
      <th>thalach</th>
      <th>exang</th>
      <th>oldpeak</th>
      <th>slope</th>
      <th>ca</th>
      <th>thal</th>
      <th>target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>52</td>
      <td>1</td>
      <td>0</td>
      <td>125</td>
      <td>212</td>
      <td>0</td>
      <td>1</td>
      <td>168</td>
      <td>0</td>
      <td>1.0</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>53</td>
      <td>1</td>
      <td>0</td>
      <td>140</td>
      <td>203</td>
      <td>1</td>
      <td>0</td>
      <td>155</td>
      <td>1</td>
      <td>3.1</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>70</td>
      <td>1</td>
      <td>0</td>
      <td>145</td>
      <td>174</td>
      <td>0</td>
      <td>1</td>
      <td>125</td>
      <td>1</td>
      <td>2.6</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>61</td>
      <td>1</td>
      <td>0</td>
      <td>148</td>
      <td>203</td>
      <td>0</td>
      <td>1</td>
      <td>161</td>
      <td>0</td>
      <td>0.0</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>62</td>
      <td>0</td>
      <td>0</td>
      <td>138</td>
      <td>294</td>
      <td>1</td>
      <td>1</td>
      <td>106</td>
      <td>0</td>
      <td>1.9</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>sex</th>
      <th>cp</th>
      <th>trestbps</th>
      <th>chol</th>
      <th>fbs</th>
      <th>restecg</th>
      <th>thalach</th>
      <th>exang</th>
      <th>oldpeak</th>
      <th>slope</th>
      <th>ca</th>
      <th>thal</th>
      <th>target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1020</th>
      <td>59</td>
      <td>1</td>
      <td>1</td>
      <td>140</td>
      <td>221</td>
      <td>0</td>
      <td>1</td>
      <td>164</td>
      <td>1</td>
      <td>0.0</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1021</th>
      <td>60</td>
      <td>1</td>
      <td>0</td>
      <td>125</td>
      <td>258</td>
      <td>0</td>
      <td>0</td>
      <td>141</td>
      <td>1</td>
      <td>2.8</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1022</th>
      <td>47</td>
      <td>1</td>
      <td>0</td>
      <td>110</td>
      <td>275</td>
      <td>0</td>
      <td>0</td>
      <td>118</td>
      <td>1</td>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1023</th>
      <td>50</td>
      <td>0</td>
      <td>0</td>
      <td>110</td>
      <td>254</td>
      <td>0</td>
      <td>0</td>
      <td>159</td>
      <td>0</td>
      <td>0.0</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1024</th>
      <td>54</td>
      <td>1</td>
      <td>0</td>
      <td>120</td>
      <td>188</td>
      <td>0</td>
      <td>1</td>
      <td>113</td>
      <td>0</td>
      <td>1.4</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>


<br>

## target 컬럼 분할


```python
y_data = df.pop("target")
x_data = df

print("Shape of X: ", x_data.shape)
print("Shape of Y: ", y_data.shape)
```

    Shape of X:  (1025, 13)
    Shape of Y:  (1025,)
    


```python
# 분할 전 데이터셋 중에서 target 값이 True인 비율
raw_true_ratio = y_data.sum() / len(y_data)
print("> Target True 비율: ", raw_true_ratio)
```

    > Target True 비율:  0.5131707317073171
    

<br>

## 학습/검증 데이터 분할

참고로 `train_test_split`의 `stratify` 옵션은 target 즉 라벨값으로 지정하면 된다. `stratify`를 target으로 지정하게 되면, target의 클래스 비율을 유지한 상태로 데이터셋을 분할하게 된다. 이 옵션을 지정해주지 않으면 분할 전/후 데이터셋의 클래스 비율이 불균형 상태를 이룰 수 있다. 이러한 불균형 상태에서 모델을 학습할 경우 모델 성능에 영향을 줄 수 있다.

### 무작위 분할

`stratify`에 None 값을 지정해 데이터셋을 분할한 경우를 살펴보자. 위에서 데이터 분할 전 target 값이 True인 비율은 약 51%인 것을 확인했다. 데이터 분할 후 학습 데이터는 약 52%, 검증 데이터는 약 47%로 비율에 약간의 차이가 있는 것을 알 수 있다.


```python
from sklearn.model_selection import train_test_split

# 학습/검증 데이터 분할 (stratify 옵션 미적용)
x_train, x_test, y_train, y_test = train_test_split(
    x_data,
    y_data,
    test_size=0.2,
    random_state=2022,
    stratify=None
)

print(x_train.shape, y_train.shape)
print(x_test.shape, y_test.shape)
```

    (820, 13) (820,)
    (205, 13) (205,)
    


```python
# 학습 데이터 중에서 target 값이 True인 비율
train_true_ratio = y_train.sum() / len(y_train)
# 검증 데이터 중에서 target 값이 True인 비율
test_true_ratio = y_test.sum() / len(y_test)

print("데이터셋 분할 시 stratify 옵션 미적용 결과")
print("> 학습 데이터 Target True 비율: ", train_true_ratio)
print("> 검증 데이터 Target True 비율: ", test_true_ratio)
```

    데이터셋 분할 시 stratify 옵션 미적용 결과
    > 학습 데이터 Target True 비율:  0.5231707317073171
    > 검증 데이터 Target True 비율:  0.47317073170731705
    

<br>

### 계층적 분할

이번에는 `stratify`에 `y_data` 값을 지정해 데이터셋을 분할한 경우를 살펴보자. 데이터 분할 후 학습 데이터는 약 51%, 검증 데이터는 약 51%로 분할 전 클래스 비율을 유지한 상태로 분할된 것을 알 수 있다.


```python
from sklearn.model_selection import train_test_split

# 학습/검증 데이터 분할 (stratify 옵션 적용)
x_train, x_test, y_train, y_test = train_test_split(
    x_data,
    y_data,
    test_size=0.2,
    random_state=2022,
    stratify=y_data
)

print(x_train.shape, y_train.shape)
print(x_test.shape, y_test.shape)
```

    (820, 13) (820,)
    (205, 13) (205,)
    


```python
# 학습 데이터 중에서 target 값이 True인 비율
train_true_ratio = y_train.sum() / len(y_train)
# 검증 데이터 중에서 target 값이 True인 비율
test_true_ratio = y_test.sum() / len(y_test)

print("데이터셋 분할 시 stratify 옵션 적용 결과")
print("> 학습 데이터 Target True 비율: ", train_true_ratio)
print("> 검증 데이터 Target True 비율: ", test_true_ratio)
```

    데이터셋 분할 시 stratify 옵션 적용 결과
    > 학습 데이터 Target True 비율:  0.5134146341463415
    > 검증 데이터 Target True 비율:  0.5121951219512195
    

<br>

## Lazypredict를 통한 자동 모델 학습

`LazyClassifier`를 이용해 Lazypredict의 지도학습 분류기 인스턴스 `clf`를 만든다. 이렇게 만든 `clf`의 `fit` 메서드의 인자료 위에서 분할한 학습 데이터와 검증 데이터를 입력한다. 그 결과로 `models`와 `predictions`가 반환된다. `models`는 Scikit-Learn의 여러 분류 모델을 적용해 학습한 결과 데이터프레임이고, `predictions`은 각 모델 별 예측값을 모아둔 데이터프레임이다.


```python
from lazypredict.Supervised import LazyClassifier

clf = LazyClassifier(verbose=0, predictions=True)

models, predictions = clf.fit(x_train, x_test, y_train, y_test)
```

    100%|███████████████████████████████████████████████████████████████████████████████| 29/29 [00:00<00:00, 36.09it/s]
    


```python
models
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Accuracy</th>
      <th>Balanced Accuracy</th>
      <th>ROC AUC</th>
      <th>F1 Score</th>
      <th>Time Taken</th>
    </tr>
    <tr>
      <th>Model</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>LGBMClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.04</td>
    </tr>
    <tr>
      <th>LabelPropagation</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.03</td>
    </tr>
    <tr>
      <th>XGBClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.08</td>
    </tr>
    <tr>
      <th>DecisionTreeClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>RandomForestClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.13</td>
    </tr>
    <tr>
      <th>ExtraTreeClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>ExtraTreesClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>BaggingClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.03</td>
    </tr>
    <tr>
      <th>LabelSpreading</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.03</td>
    </tr>
    <tr>
      <th>SVC</th>
      <td>0.94</td>
      <td>0.94</td>
      <td>0.94</td>
      <td>0.94</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>AdaBoostClassifier</th>
      <td>0.92</td>
      <td>0.92</td>
      <td>0.92</td>
      <td>0.92</td>
      <td>0.07</td>
    </tr>
    <tr>
      <th>NuSVC</th>
      <td>0.90</td>
      <td>0.90</td>
      <td>0.90</td>
      <td>0.90</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>QuadraticDiscriminantAnalysis</th>
      <td>0.89</td>
      <td>0.89</td>
      <td>0.89</td>
      <td>0.89</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>GaussianNB</th>
      <td>0.88</td>
      <td>0.88</td>
      <td>0.88</td>
      <td>0.88</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>SGDClassifier</th>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>CalibratedClassifierCV</th>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.08</td>
    </tr>
    <tr>
      <th>LogisticRegression</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LinearSVC</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.03</td>
    </tr>
    <tr>
      <th>NearestCentroid</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LinearDiscriminantAnalysis</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>RidgeClassifier</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>RidgeClassifierCV</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>BernoulliNB</th>
      <td>0.85</td>
      <td>0.85</td>
      <td>0.85</td>
      <td>0.85</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>KNeighborsClassifier</th>
      <td>0.83</td>
      <td>0.83</td>
      <td>0.83</td>
      <td>0.83</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>PassiveAggressiveClassifier</th>
      <td>0.78</td>
      <td>0.78</td>
      <td>0.78</td>
      <td>0.78</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>Perceptron</th>
      <td>0.76</td>
      <td>0.76</td>
      <td>0.76</td>
      <td>0.76</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>DummyClassifier</th>
      <td>0.48</td>
      <td>0.48</td>
      <td>0.48</td>
      <td>0.48</td>
      <td>0.01</td>
    </tr>
  </tbody>
</table>
</div>




```python
predictions.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AdaBoostClassifier</th>
      <th>BaggingClassifier</th>
      <th>BernoulliNB</th>
      <th>CalibratedClassifierCV</th>
      <th>DecisionTreeClassifier</th>
      <th>DummyClassifier</th>
      <th>ExtraTreeClassifier</th>
      <th>ExtraTreesClassifier</th>
      <th>GaussianNB</th>
      <th>KNeighborsClassifier</th>
      <th>...</th>
      <th>PassiveAggressiveClassifier</th>
      <th>Perceptron</th>
      <th>QuadraticDiscriminantAnalysis</th>
      <th>RandomForestClassifier</th>
      <th>RidgeClassifier</th>
      <th>RidgeClassifierCV</th>
      <th>SGDClassifier</th>
      <th>SVC</th>
      <th>XGBClassifier</th>
      <th>LGBMClassifier</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 27 columns</p>
</div>



<br>

## 모델 별 분류 리포트 출력

모델 별 분류 성능 지표를 출력할 수도 있다.


```python
from sklearn.metrics import classification_report

for model_name in predictions.columns.tolist():
    print("="*60)
    print(f'{model_name}')
    print("="*60)
    print(classification_report(y_test, predictions[model_name]))
```

    ============================================================
    AdaBoostClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.95      0.89      0.92       100
               1       0.90      0.95      0.93       105
    
        accuracy                           0.92       205
       macro avg       0.92      0.92      0.92       205
    weighted avg       0.92      0.92      0.92       205
    
    ============================================================
    BaggingClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       1.00      1.00      1.00       100
               1       1.00      1.00      1.00       105
    
        accuracy                           1.00       205
       macro avg       1.00      1.00      1.00       205
    weighted avg       1.00      1.00      1.00       205
    
    ============================================================
    BernoulliNB
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.88      0.80      0.84       100
               1       0.82      0.90      0.86       105
    
        accuracy                           0.85       205
       macro avg       0.85      0.85      0.85       205
    weighted avg       0.85      0.85      0.85       205
    
    ============================================================
    CalibratedClassifierCV
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.95      0.78      0.86       100
               1       0.82      0.96      0.89       105
    
        accuracy                           0.87       205
       macro avg       0.89      0.87      0.87       205
    weighted avg       0.88      0.87      0.87       205
    
    ============================================================
    DecisionTreeClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       1.00      1.00      1.00       100
               1       1.00      1.00      1.00       105
    
        accuracy                           1.00       205
       macro avg       1.00      1.00      1.00       205
    weighted avg       1.00      1.00      1.00       205
    
    ============================================================
    DummyClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.46      0.45      0.46       100
               1       0.49      0.50      0.50       105
    
        accuracy                           0.48       205
       macro avg       0.48      0.48      0.48       205
    weighted avg       0.48      0.48      0.48       205
    
    ============================================================
    ExtraTreeClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       1.00      1.00      1.00       100
               1       1.00      1.00      1.00       105
    
        accuracy                           1.00       205
       macro avg       1.00      1.00      1.00       205
    weighted avg       1.00      1.00      1.00       205
    
    ============================================================
    ExtraTreesClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       1.00      1.00      1.00       100
               1       1.00      1.00      1.00       105
    
        accuracy                           1.00       205
       macro avg       1.00      1.00      1.00       205
    weighted avg       1.00      1.00      1.00       205
    
    ============================================================
    GaussianNB
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.89      0.85      0.87       100
               1       0.86      0.90      0.88       105
    
        accuracy                           0.88       205
       macro avg       0.88      0.88      0.88       205
    weighted avg       0.88      0.88      0.88       205
    
    ============================================================
    KNeighborsClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.85      0.80      0.82       100
               1       0.82      0.87      0.84       105
    
        accuracy                           0.83       205
       macro avg       0.84      0.83      0.83       205
    weighted avg       0.84      0.83      0.83       205
    
    ============================================================
    LabelPropagation
    ============================================================
                  precision    recall  f1-score   support
    
               0       1.00      1.00      1.00       100
               1       1.00      1.00      1.00       105
    
        accuracy                           1.00       205
       macro avg       1.00      1.00      1.00       205
    weighted avg       1.00      1.00      1.00       205
    
    ============================================================
    LabelSpreading
    ============================================================
                  precision    recall  f1-score   support
    
               0       1.00      1.00      1.00       100
               1       1.00      1.00      1.00       105
    
        accuracy                           1.00       205
       macro avg       1.00      1.00      1.00       205
    weighted avg       1.00      1.00      1.00       205
    
    ============================================================
    LinearDiscriminantAnalysis
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.96      0.74      0.84       100
               1       0.80      0.97      0.88       105
    
        accuracy                           0.86       205
       macro avg       0.88      0.86      0.86       205
    weighted avg       0.88      0.86      0.86       205
    
    ============================================================
    LinearSVC
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.95      0.76      0.84       100
               1       0.81      0.96      0.88       105
    
        accuracy                           0.86       205
       macro avg       0.88      0.86      0.86       205
    weighted avg       0.88      0.86      0.86       205
    
    ============================================================
    LogisticRegression
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.93      0.78      0.85       100
               1       0.82      0.94      0.88       105
    
        accuracy                           0.86       205
       macro avg       0.87      0.86      0.86       205
    weighted avg       0.87      0.86      0.86       205
    
    ============================================================
    NearestCentroid
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.94      0.76      0.84       100
               1       0.81      0.95      0.87       105
    
        accuracy                           0.86       205
       macro avg       0.87      0.86      0.86       205
    weighted avg       0.87      0.86      0.86       205
    
    ============================================================
    NuSVC
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.94      0.85      0.89       100
               1       0.87      0.95      0.91       105
    
        accuracy                           0.90       205
       macro avg       0.91      0.90      0.90       205
    weighted avg       0.91      0.90      0.90       205
    
    ============================================================
    PassiveAggressiveClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.77      0.77      0.77       100
               1       0.78      0.78      0.78       105
    
        accuracy                           0.78       205
       macro avg       0.78      0.78      0.78       205
    weighted avg       0.78      0.78      0.78       205
    
    ============================================================
    Perceptron
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.76      0.74      0.75       100
               1       0.76      0.77      0.76       105
    
        accuracy                           0.76       205
       macro avg       0.76      0.76      0.76       205
    weighted avg       0.76      0.76      0.76       205
    
    ============================================================
    QuadraticDiscriminantAnalysis
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.91      0.87      0.89       100
               1       0.88      0.91      0.90       105
    
        accuracy                           0.89       205
       macro avg       0.89      0.89      0.89       205
    weighted avg       0.89      0.89      0.89       205
    
    ============================================================
    RandomForestClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       1.00      1.00      1.00       100
               1       1.00      1.00      1.00       105
    
        accuracy                           1.00       205
       macro avg       1.00      1.00      1.00       205
    weighted avg       1.00      1.00      1.00       205
    
    ============================================================
    RidgeClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.96      0.74      0.84       100
               1       0.80      0.97      0.88       105
    
        accuracy                           0.86       205
       macro avg       0.88      0.86      0.86       205
    weighted avg       0.88      0.86      0.86       205
    
    ============================================================
    RidgeClassifierCV
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.96      0.74      0.84       100
               1       0.80      0.97      0.88       105
    
        accuracy                           0.86       205
       macro avg       0.88      0.86      0.86       205
    weighted avg       0.88      0.86      0.86       205
    
    ============================================================
    SGDClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.91      0.82      0.86       100
               1       0.84      0.92      0.88       105
    
        accuracy                           0.87       205
       macro avg       0.88      0.87      0.87       205
    weighted avg       0.88      0.87      0.87       205
    
    ============================================================
    SVC
    ============================================================
                  precision    recall  f1-score   support
    
               0       0.97      0.90      0.93       100
               1       0.91      0.97      0.94       105
    
        accuracy                           0.94       205
       macro avg       0.94      0.94      0.94       205
    weighted avg       0.94      0.94      0.94       205
    
    ============================================================
    XGBClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       1.00      1.00      1.00       100
               1       1.00      1.00      1.00       105
    
        accuracy                           1.00       205
       macro avg       1.00      1.00      1.00       205
    weighted avg       1.00      1.00      1.00       205
    
    ============================================================
    LGBMClassifier
    ============================================================
                  precision    recall  f1-score   support
    
               0       1.00      1.00      1.00       100
               1       1.00      1.00      1.00       105
    
        accuracy                           1.00       205
       macro avg       1.00      1.00      1.00       205
    weighted avg       1.00      1.00      1.00       205
    
    


```python
models
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Accuracy</th>
      <th>Balanced Accuracy</th>
      <th>ROC AUC</th>
      <th>F1 Score</th>
      <th>Time Taken</th>
    </tr>
    <tr>
      <th>Model</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>LGBMClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.04</td>
    </tr>
    <tr>
      <th>LabelPropagation</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.03</td>
    </tr>
    <tr>
      <th>XGBClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.08</td>
    </tr>
    <tr>
      <th>DecisionTreeClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>RandomForestClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.13</td>
    </tr>
    <tr>
      <th>ExtraTreeClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>ExtraTreesClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>BaggingClassifier</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.03</td>
    </tr>
    <tr>
      <th>LabelSpreading</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>0.03</td>
    </tr>
    <tr>
      <th>SVC</th>
      <td>0.94</td>
      <td>0.94</td>
      <td>0.94</td>
      <td>0.94</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>AdaBoostClassifier</th>
      <td>0.92</td>
      <td>0.92</td>
      <td>0.92</td>
      <td>0.92</td>
      <td>0.07</td>
    </tr>
    <tr>
      <th>NuSVC</th>
      <td>0.90</td>
      <td>0.90</td>
      <td>0.90</td>
      <td>0.90</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>QuadraticDiscriminantAnalysis</th>
      <td>0.89</td>
      <td>0.89</td>
      <td>0.89</td>
      <td>0.89</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>GaussianNB</th>
      <td>0.88</td>
      <td>0.88</td>
      <td>0.88</td>
      <td>0.88</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>SGDClassifier</th>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>CalibratedClassifierCV</th>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.87</td>
      <td>0.08</td>
    </tr>
    <tr>
      <th>LogisticRegression</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LinearSVC</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.03</td>
    </tr>
    <tr>
      <th>NearestCentroid</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LinearDiscriminantAnalysis</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>RidgeClassifier</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>RidgeClassifierCV</th>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>BernoulliNB</th>
      <td>0.85</td>
      <td>0.85</td>
      <td>0.85</td>
      <td>0.85</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>KNeighborsClassifier</th>
      <td>0.83</td>
      <td>0.83</td>
      <td>0.83</td>
      <td>0.83</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>PassiveAggressiveClassifier</th>
      <td>0.78</td>
      <td>0.78</td>
      <td>0.78</td>
      <td>0.78</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>Perceptron</th>
      <td>0.76</td>
      <td>0.76</td>
      <td>0.76</td>
      <td>0.76</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>DummyClassifier</th>
      <td>0.48</td>
      <td>0.48</td>
      <td>0.48</td>
      <td>0.48</td>
      <td>0.01</td>
    </tr>
  </tbody>
</table>
</div>



<br>

## 단일 모델 선택

여러 성능 지표 중 Balanced Accuracy 값이 가장 큰 모델 중 하나를 고르면, LGBMClassifier 모델이 선택된다. 


```python
models.loc[models['Balanced Accuracy'] == models['Balanced Accuracy'].max()].index[0]
```




    'LGBMClassifier'



해당 모델을 직접 불러와 학습 후 예측한 결과는 역시 위의 결과와 동일한 것을 확인할 수 있다.


```python
from lightgbm import LGBMClassifier
from sklearn.metrics import balanced_accuracy_score

lgbm = LGBMClassifier()
lgbm.fit(x_train, y_train)
y_pred = lgbm.predict(x_test)
balanced_accuracy_score(y_pred, y_pred)
```




    1.0



<br>

## 참고

- [lazypredict GitHub](https://github.com/shankarpandala/lazypredict)
- [kairess GitHub](https://github.com/kairess/auto-ml-auto-viz/blob/main/auto_ml.ipynb)
- [sklearn.model_selection.train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)
