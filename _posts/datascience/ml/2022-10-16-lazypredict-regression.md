---
title: Lazypredict를 이용해 ML모델 선택하기(Regression)
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

예제 데이터로 [와인 품질 데이터](https://archive.ics.uci.edu/ml/datasets/Wine+Quality)를 사용한다. 예측하고자 하는 값은 `quality`이고, 0 ~ 10 사이의 값을 갖는다. 데이터셋에 대한 자세한 설명은 앞의 링크를 참조하면 된다. 데이터 내의 여러 속성들을 바탕으로 와인의 품질 점수를 예측하는 Regression 모델을 만들어보자. 아래와 같이 우선 데이터를 로드한다.


```python
import pandas as pd

url = "https://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-red.csv"
df = pd.read_csv(url, sep=";")
```


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1599 entries, 0 to 1598
    Data columns (total 12 columns):
     #   Column                Non-Null Count  Dtype  
    ---  ------                --------------  -----  
     0   fixed acidity         1599 non-null   float64
     1   volatile acidity      1599 non-null   float64
     2   citric acid           1599 non-null   float64
     3   residual sugar        1599 non-null   float64
     4   chlorides             1599 non-null   float64
     5   free sulfur dioxide   1599 non-null   float64
     6   total sulfur dioxide  1599 non-null   float64
     7   density               1599 non-null   float64
     8   pH                    1599 non-null   float64
     9   sulphates             1599 non-null   float64
     10  alcohol               1599 non-null   float64
     11  quality               1599 non-null   int64  
    dtypes: float64(11), int64(1)
    memory usage: 150.0 KB
    


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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7.4</td>
      <td>0.70</td>
      <td>0.00</td>
      <td>1.9</td>
      <td>0.076</td>
      <td>11.0</td>
      <td>34.0</td>
      <td>0.9978</td>
      <td>3.51</td>
      <td>0.56</td>
      <td>9.4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7.8</td>
      <td>0.88</td>
      <td>0.00</td>
      <td>2.6</td>
      <td>0.098</td>
      <td>25.0</td>
      <td>67.0</td>
      <td>0.9968</td>
      <td>3.20</td>
      <td>0.68</td>
      <td>9.8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.8</td>
      <td>0.76</td>
      <td>0.04</td>
      <td>2.3</td>
      <td>0.092</td>
      <td>15.0</td>
      <td>54.0</td>
      <td>0.9970</td>
      <td>3.26</td>
      <td>0.65</td>
      <td>9.8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11.2</td>
      <td>0.28</td>
      <td>0.56</td>
      <td>1.9</td>
      <td>0.075</td>
      <td>17.0</td>
      <td>60.0</td>
      <td>0.9980</td>
      <td>3.16</td>
      <td>0.58</td>
      <td>9.8</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7.4</td>
      <td>0.70</td>
      <td>0.00</td>
      <td>1.9</td>
      <td>0.076</td>
      <td>11.0</td>
      <td>34.0</td>
      <td>0.9978</td>
      <td>3.51</td>
      <td>0.56</td>
      <td>9.4</td>
      <td>5</td>
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
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1594</th>
      <td>6.2</td>
      <td>0.600</td>
      <td>0.08</td>
      <td>2.0</td>
      <td>0.090</td>
      <td>32.0</td>
      <td>44.0</td>
      <td>0.99490</td>
      <td>3.45</td>
      <td>0.58</td>
      <td>10.5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1595</th>
      <td>5.9</td>
      <td>0.550</td>
      <td>0.10</td>
      <td>2.2</td>
      <td>0.062</td>
      <td>39.0</td>
      <td>51.0</td>
      <td>0.99512</td>
      <td>3.52</td>
      <td>0.76</td>
      <td>11.2</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1596</th>
      <td>6.3</td>
      <td>0.510</td>
      <td>0.13</td>
      <td>2.3</td>
      <td>0.076</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>0.99574</td>
      <td>3.42</td>
      <td>0.75</td>
      <td>11.0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1597</th>
      <td>5.9</td>
      <td>0.645</td>
      <td>0.12</td>
      <td>2.0</td>
      <td>0.075</td>
      <td>32.0</td>
      <td>44.0</td>
      <td>0.99547</td>
      <td>3.57</td>
      <td>0.71</td>
      <td>10.2</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1598</th>
      <td>6.0</td>
      <td>0.310</td>
      <td>0.47</td>
      <td>3.6</td>
      <td>0.067</td>
      <td>18.0</td>
      <td>42.0</td>
      <td>0.99549</td>
      <td>3.39</td>
      <td>0.66</td>
      <td>11.0</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>


<br>


## quality 컬럼 분할


```python
y_data = df.pop("quality")
x_data = df

print("Shape of X: ", x_data.shape)
print("Shape of Y: ", y_data.shape)
```

    Shape of X:  (1599, 11)
    Shape of Y:  (1599,)
    

<br>

## 학습/검증 데이터 분할


```python
from sklearn.model_selection import train_test_split

# 학습/검증 데이터 분할
x_train, x_test, y_train, y_test = train_test_split(
    x_data, 
    y_data, 
    test_size=0.2, 
    random_state=42)

print(x_train.shape, y_train.shape)
print(x_test.shape, y_test.shape)
```

    (1279, 11) (1279,)
    (320, 11) (320,)
    

<br>

## Lazypredict를 통한 자동 모델 학습

`LazyClassifier`를 이용해 Lazypredict의 지도학습 분류기 인스턴스 `clf`를 만든다. 이렇게 만든 `clf`의 `fit` 메서드의 인자료 위에서 분할한 학습 데이터와 검증 데이터를 입력한다. 그 결과로 `models`와 `predictions`가 반환된다. `models`는 Scikit-Learn의 여러 분류 모델을 적용해 학습한 결과 데이터프레임이고, `predictions`은 각 모델 별 예측값을 모아둔 데이터프레임이다.


```python
from lazypredict.Supervised import LazyRegressor

reg = LazyRegressor(verbose=0, predictions=True)

models, predictions = reg.fit(x_train, x_test, y_train, y_test)
```

    100%|███████████████████████████████████████████████████████████████████████████████| 41/41 [00:03<00:00, 12.45it/s]
    


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
      <th>Adjusted R-Squared</th>
      <th>R-Squared</th>
      <th>RMSE</th>
      <th>Time Taken</th>
    </tr>
    <tr>
      <th>Model</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ExtraTreesRegressor</th>
      <td>0.54</td>
      <td>0.55</td>
      <td>0.54</td>
      <td>0.22</td>
    </tr>
    <tr>
      <th>RandomForestRegressor</th>
      <td>0.52</td>
      <td>0.54</td>
      <td>0.55</td>
      <td>0.40</td>
    </tr>
    <tr>
      <th>LGBMRegressor</th>
      <td>0.48</td>
      <td>0.50</td>
      <td>0.57</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>HistGradientBoostingRegressor</th>
      <td>0.48</td>
      <td>0.49</td>
      <td>0.58</td>
      <td>0.46</td>
    </tr>
    <tr>
      <th>XGBRegressor</th>
      <td>0.47</td>
      <td>0.49</td>
      <td>0.58</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>BaggingRegressor</th>
      <td>0.47</td>
      <td>0.49</td>
      <td>0.58</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>NuSVR</th>
      <td>0.45</td>
      <td>0.46</td>
      <td>0.59</td>
      <td>0.06</td>
    </tr>
    <tr>
      <th>SVR</th>
      <td>0.44</td>
      <td>0.46</td>
      <td>0.59</td>
      <td>0.06</td>
    </tr>
    <tr>
      <th>GradientBoostingRegressor</th>
      <td>0.43</td>
      <td>0.45</td>
      <td>0.60</td>
      <td>0.15</td>
    </tr>
    <tr>
      <th>TransformedTargetRegressor</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LinearRegression</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>Lars</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>Ridge</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>RidgeCV</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>BayesianRidge</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>SGDRegressor</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LassoLarsIC</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>HuberRegressor</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>LassoCV</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>ElasticNetCV</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>LassoLarsCV</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>AdaBoostRegressor</th>
      <td>0.37</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>PoissonRegressor</th>
      <td>0.37</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LarsCV</th>
      <td>0.37</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>OrthogonalMatchingPursuitCV</th>
      <td>0.37</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LinearSVR</th>
      <td>0.36</td>
      <td>0.38</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>MLPRegressor</th>
      <td>0.33</td>
      <td>0.35</td>
      <td>0.65</td>
      <td>1.08</td>
    </tr>
    <tr>
      <th>KNeighborsRegressor</th>
      <td>0.31</td>
      <td>0.33</td>
      <td>0.66</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>TweedieRegressor</th>
      <td>0.30</td>
      <td>0.32</td>
      <td>0.67</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>GammaRegressor</th>
      <td>0.30</td>
      <td>0.32</td>
      <td>0.67</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>OrthogonalMatchingPursuit</th>
      <td>0.21</td>
      <td>0.24</td>
      <td>0.71</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>RANSACRegressor</th>
      <td>0.07</td>
      <td>0.10</td>
      <td>0.77</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>DecisionTreeRegressor</th>
      <td>0.03</td>
      <td>0.06</td>
      <td>0.78</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>ElasticNet</th>
      <td>-0.04</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>DummyRegressor</th>
      <td>-0.04</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>Lasso</th>
      <td>-0.04</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LassoLars</th>
      <td>-0.04</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>ExtraTreeRegressor</th>
      <td>-0.09</td>
      <td>-0.05</td>
      <td>0.83</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>PassiveAggressiveRegressor</th>
      <td>-0.18</td>
      <td>-0.14</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>GaussianProcessRegressor</th>
      <td>-3.42</td>
      <td>-3.27</td>
      <td>1.67</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>KernelRidge</th>
      <td>-50.33</td>
      <td>-48.56</td>
      <td>5.69</td>
      <td>0.05</td>
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
      <th>AdaBoostRegressor</th>
      <th>BaggingRegressor</th>
      <th>BayesianRidge</th>
      <th>DecisionTreeRegressor</th>
      <th>DummyRegressor</th>
      <th>ElasticNet</th>
      <th>ElasticNetCV</th>
      <th>ExtraTreeRegressor</th>
      <th>ExtraTreesRegressor</th>
      <th>GammaRegressor</th>
      <th>...</th>
      <th>RANSACRegressor</th>
      <th>RandomForestRegressor</th>
      <th>Ridge</th>
      <th>RidgeCV</th>
      <th>SGDRegressor</th>
      <th>SVR</th>
      <th>TransformedTargetRegressor</th>
      <th>TweedieRegressor</th>
      <th>XGBRegressor</th>
      <th>LGBMRegressor</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.47</td>
      <td>5.10</td>
      <td>5.34</td>
      <td>6.00</td>
      <td>5.62</td>
      <td>5.62</td>
      <td>5.34</td>
      <td>6.00</td>
      <td>5.30</td>
      <td>5.40</td>
      <td>...</td>
      <td>5.77</td>
      <td>5.30</td>
      <td>5.35</td>
      <td>5.34</td>
      <td>5.34</td>
      <td>5.44</td>
      <td>5.35</td>
      <td>5.41</td>
      <td>5.32</td>
      <td>5.34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5.41</td>
      <td>5.20</td>
      <td>5.07</td>
      <td>5.00</td>
      <td>5.62</td>
      <td>5.62</td>
      <td>5.09</td>
      <td>5.00</td>
      <td>5.19</td>
      <td>5.31</td>
      <td>...</td>
      <td>5.66</td>
      <td>5.22</td>
      <td>5.06</td>
      <td>5.06</td>
      <td>5.09</td>
      <td>5.09</td>
      <td>5.06</td>
      <td>5.31</td>
      <td>4.81</td>
      <td>5.11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.61</td>
      <td>5.30</td>
      <td>5.65</td>
      <td>5.00</td>
      <td>5.62</td>
      <td>5.62</td>
      <td>5.63</td>
      <td>5.00</td>
      <td>5.41</td>
      <td>5.55</td>
      <td>...</td>
      <td>5.79</td>
      <td>5.46</td>
      <td>5.66</td>
      <td>5.66</td>
      <td>5.66</td>
      <td>5.60</td>
      <td>5.66</td>
      <td>5.56</td>
      <td>4.78</td>
      <td>5.02</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5.20</td>
      <td>5.00</td>
      <td>5.46</td>
      <td>5.00</td>
      <td>5.62</td>
      <td>5.62</td>
      <td>5.46</td>
      <td>7.00</td>
      <td>5.31</td>
      <td>5.48</td>
      <td>...</td>
      <td>4.79</td>
      <td>5.19</td>
      <td>5.46</td>
      <td>5.46</td>
      <td>5.46</td>
      <td>5.33</td>
      <td>5.46</td>
      <td>5.49</td>
      <td>5.20</td>
      <td>5.34</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.76</td>
      <td>6.00</td>
      <td>5.73</td>
      <td>6.00</td>
      <td>5.62</td>
      <td>5.62</td>
      <td>5.73</td>
      <td>6.00</td>
      <td>6.00</td>
      <td>5.69</td>
      <td>...</td>
      <td>5.73</td>
      <td>6.00</td>
      <td>5.73</td>
      <td>5.73</td>
      <td>5.74</td>
      <td>5.71</td>
      <td>5.73</td>
      <td>5.70</td>
      <td>6.00</td>
      <td>5.94</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 41 columns</p>
</div>



<br>

## 단일 모델 선택

여러 성능 지표 중 RMSE 값이 가장 작은 모델 중 하나를 고르면, ExtraTreesRegressor 모델이 선택된다. 


```python
models.loc[models['RMSE'] == models['RMSE'].min()].index[0]
```




    'ExtraTreesRegressor'



해당 모델을 직접 불러와 직접 학습을 진행한 후 검증 데이터에 대해 예측값을 생성해보자.


```python
from sklearn.ensemble import ExtraTreesRegressor
from sklearn.metrics import r2_score, mean_squared_error

# 학습
etr = ExtraTreesRegressor(n_estimators=100,random_state=0).fit(x_train, y_train)

# 예측값
y_pred = etr.predict(x_test)
```

검증 데이터에 대한 모델의 예측 성능을 평가해보면, 위에서 Lazypredict를 이용한 결과와 유사한 것을 알 수 있다.


```python
# R-Squared
r_squared = r2_score(y_test, y_pred)
# Adjusted R-Squared
def adjusted_rsquared(r2, n, p):
    return 1 - (1 - r2) * ((n - 1) / (n - p - 1))
adj_rsquared = adjusted_rsquared(
    r_squared, x_test.shape[0], x_test.shape[1]
)
# RMSE
import numpy as np
rmse = np.sqrt(mean_squared_error(y_test, y_pred))

print(f"""
R-Squared: {r_squared:.2f}
Adjusted R-Squared: {adj_rsquared:.2f}
RMSE: {rmse:.2f}
""")
```

    
    R-Squared: 0.55
    Adjusted R-Squared: 0.54
    RMSE: 0.54
    
    

<br>

## 표준화 후 결과 비교

데이터 표준화 후 Lazypredict를 사용하면 학습 결과에 영향을 줄 것으로 생각하고, 테스트를 진행했지만, 사실 결과는 위에서 진행한 것과 같다. 그 이유는 Lazypredict의 내부 소스코드에 이미 StandardScaler가 적용되어 있기 때문이다. 이외에 모델의 성능을 향상시키기 위해서 Feature Engineering이나 파라미터를 튜닝하는 방식 등을 추가적으로 적용해 볼 수 있겠다.


```python
from sklearn.preprocessing import StandardScaler

# 학습 데이터 기준으로 표준화
scaler = StandardScaler()
x_train_scaled = scaler.fit_transform(x_train)
x_test_scaled = scaler.transform(x_test)

print(x_train_scaled.shape, y_train.shape)
print(x_test_scaled.shape, y_test.shape)
```

    (1279, 11) (1279,)
    (320, 11) (320,)
    

아래 결과를 참고하면 스케일러에 x_train의 각 컬럼의 평균과 표준편차가 저장된 것을 확인할 수 있다.


```python
pd.DataFrame({"scaler_mean": scaler.mean_, "scaler_std": scaler.scale_})
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
      <th>scaler_mean</th>
      <th>scaler_std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8.32</td>
      <td>1.72</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.53</td>
      <td>0.18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.27</td>
      <td>0.20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.56</td>
      <td>1.44</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.09</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>5</th>
      <td>15.88</td>
      <td>10.31</td>
    </tr>
    <tr>
      <th>6</th>
      <td>46.66</td>
      <td>32.93</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1.00</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>8</th>
      <td>3.31</td>
      <td>0.15</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.66</td>
      <td>0.17</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10.42</td>
      <td>1.05</td>
    </tr>
  </tbody>
</table>
</div>




```python
x_train.describe().T[['mean','std']]
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
      <th>mean</th>
      <th>std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>fixed acidity</th>
      <td>8.32</td>
      <td>1.72</td>
    </tr>
    <tr>
      <th>volatile acidity</th>
      <td>0.53</td>
      <td>0.18</td>
    </tr>
    <tr>
      <th>citric acid</th>
      <td>0.27</td>
      <td>0.20</td>
    </tr>
    <tr>
      <th>residual sugar</th>
      <td>2.56</td>
      <td>1.44</td>
    </tr>
    <tr>
      <th>chlorides</th>
      <td>0.09</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>free sulfur dioxide</th>
      <td>15.88</td>
      <td>10.31</td>
    </tr>
    <tr>
      <th>total sulfur dioxide</th>
      <td>46.66</td>
      <td>32.94</td>
    </tr>
    <tr>
      <th>density</th>
      <td>1.00</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>pH</th>
      <td>3.31</td>
      <td>0.15</td>
    </tr>
    <tr>
      <th>sulphates</th>
      <td>0.66</td>
      <td>0.17</td>
    </tr>
    <tr>
      <th>alcohol</th>
      <td>10.42</td>
      <td>1.05</td>
    </tr>
  </tbody>
</table>
</div>




```python
from lazypredict.Supervised import LazyRegressor

reg = LazyRegressor(verbose=0, predictions=True)

models, predictions = reg.fit(x_train_scaled, x_test_scaled, y_train, y_test)
```

    100%|███████████████████████████████████████████████████████████████████████████████| 41/41 [00:03<00:00, 12.51it/s]
    

표준화된 데이터를 Lazypredict 내부에서 한 번 더 표준화하므로 사실 표준화를 하지 않은 데이터를 사용한 것과 결과가 동일한 것을 알 수 있다.


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
      <th>Adjusted R-Squared</th>
      <th>R-Squared</th>
      <th>RMSE</th>
      <th>Time Taken</th>
    </tr>
    <tr>
      <th>Model</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ExtraTreesRegressor</th>
      <td>0.54</td>
      <td>0.55</td>
      <td>0.54</td>
      <td>0.22</td>
    </tr>
    <tr>
      <th>RandomForestRegressor</th>
      <td>0.52</td>
      <td>0.54</td>
      <td>0.55</td>
      <td>0.41</td>
    </tr>
    <tr>
      <th>LGBMRegressor</th>
      <td>0.48</td>
      <td>0.50</td>
      <td>0.57</td>
      <td>0.04</td>
    </tr>
    <tr>
      <th>HistGradientBoostingRegressor</th>
      <td>0.48</td>
      <td>0.49</td>
      <td>0.58</td>
      <td>0.47</td>
    </tr>
    <tr>
      <th>XGBRegressor</th>
      <td>0.47</td>
      <td>0.49</td>
      <td>0.58</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>BaggingRegressor</th>
      <td>0.47</td>
      <td>0.49</td>
      <td>0.58</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>NuSVR</th>
      <td>0.45</td>
      <td>0.46</td>
      <td>0.59</td>
      <td>0.06</td>
    </tr>
    <tr>
      <th>SVR</th>
      <td>0.44</td>
      <td>0.46</td>
      <td>0.59</td>
      <td>0.06</td>
    </tr>
    <tr>
      <th>GradientBoostingRegressor</th>
      <td>0.43</td>
      <td>0.45</td>
      <td>0.60</td>
      <td>0.15</td>
    </tr>
    <tr>
      <th>TransformedTargetRegressor</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LinearRegression</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>Lars</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>Ridge</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>RidgeCV</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>BayesianRidge</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>SGDRegressor</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LassoLarsIC</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>HuberRegressor</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>LassoCV</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>ElasticNetCV</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>LassoLarsCV</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>AdaBoostRegressor</th>
      <td>0.37</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>PoissonRegressor</th>
      <td>0.37</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LarsCV</th>
      <td>0.37</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>OrthogonalMatchingPursuitCV</th>
      <td>0.37</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LinearSVR</th>
      <td>0.36</td>
      <td>0.38</td>
      <td>0.63</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>MLPRegressor</th>
      <td>0.33</td>
      <td>0.35</td>
      <td>0.65</td>
      <td>1.09</td>
    </tr>
    <tr>
      <th>KNeighborsRegressor</th>
      <td>0.31</td>
      <td>0.33</td>
      <td>0.66</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>TweedieRegressor</th>
      <td>0.30</td>
      <td>0.32</td>
      <td>0.67</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>GammaRegressor</th>
      <td>0.30</td>
      <td>0.32</td>
      <td>0.67</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>OrthogonalMatchingPursuit</th>
      <td>0.21</td>
      <td>0.24</td>
      <td>0.71</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>RANSACRegressor</th>
      <td>0.07</td>
      <td>0.10</td>
      <td>0.77</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>DecisionTreeRegressor</th>
      <td>0.03</td>
      <td>0.06</td>
      <td>0.78</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>ElasticNet</th>
      <td>-0.04</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>DummyRegressor</th>
      <td>-0.04</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>Lasso</th>
      <td>-0.04</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LassoLars</th>
      <td>-0.04</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>ExtraTreeRegressor</th>
      <td>-0.09</td>
      <td>-0.05</td>
      <td>0.83</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>PassiveAggressiveRegressor</th>
      <td>-0.18</td>
      <td>-0.14</td>
      <td>0.86</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>GaussianProcessRegressor</th>
      <td>-3.42</td>
      <td>-3.27</td>
      <td>1.67</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>KernelRidge</th>
      <td>-50.33</td>
      <td>-48.56</td>
      <td>5.69</td>
      <td>0.06</td>
    </tr>
  </tbody>
</table>
</div>


<br>


## PCA 적용 후 Lazypredict 사용

표준화된 데이터를 가지고 PCA를 통해 차원을 축소한 뒤 Lazypredict를 사용해보자.


```python
from sklearn.decomposition import PCA

pca = PCA(n_components=9)
pca.fit(x_train)

x_train_ = pca.transform(x_train_scaled)
x_test_ = pca.transform(x_test_scaled)
```

ExtraTreesRegressor 모델의 성능 지표들의 값이 위 결과 보다 약간 좋아진 걸 알 수 있다.


```python
from lazypredict.Supervised import LazyRegressor

reg = LazyRegressor(verbose=0, predictions=True)
models, predictions = reg.fit(x_train_, x_test_, y_train, y_test)
models
```

    100%|███████████████████████████████████████████████████████████████████████████████| 41/41 [00:03<00:00, 12.48it/s]
    




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
      <th>Adjusted R-Squared</th>
      <th>R-Squared</th>
      <th>RMSE</th>
      <th>Time Taken</th>
    </tr>
    <tr>
      <th>Model</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ExtraTreesRegressor</th>
      <td>0.56</td>
      <td>0.57</td>
      <td>0.53</td>
      <td>0.21</td>
    </tr>
    <tr>
      <th>RandomForestRegressor</th>
      <td>0.56</td>
      <td>0.57</td>
      <td>0.53</td>
      <td>0.43</td>
    </tr>
    <tr>
      <th>LGBMRegressor</th>
      <td>0.52</td>
      <td>0.54</td>
      <td>0.55</td>
      <td>0.04</td>
    </tr>
    <tr>
      <th>HistGradientBoostingRegressor</th>
      <td>0.52</td>
      <td>0.54</td>
      <td>0.55</td>
      <td>0.42</td>
    </tr>
    <tr>
      <th>BaggingRegressor</th>
      <td>0.52</td>
      <td>0.53</td>
      <td>0.55</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>GradientBoostingRegressor</th>
      <td>0.49</td>
      <td>0.51</td>
      <td>0.57</td>
      <td>0.22</td>
    </tr>
    <tr>
      <th>XGBRegressor</th>
      <td>0.48</td>
      <td>0.50</td>
      <td>0.57</td>
      <td>0.11</td>
    </tr>
    <tr>
      <th>SVR</th>
      <td>0.45</td>
      <td>0.46</td>
      <td>0.59</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>NuSVR</th>
      <td>0.44</td>
      <td>0.46</td>
      <td>0.59</td>
      <td>0.06</td>
    </tr>
    <tr>
      <th>AdaBoostRegressor</th>
      <td>0.39</td>
      <td>0.40</td>
      <td>0.62</td>
      <td>0.11</td>
    </tr>
    <tr>
      <th>KNeighborsRegressor</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>Lars</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>TransformedTargetRegressor</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LinearRegression</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>Ridge</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>RidgeCV</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>BayesianRidge</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LassoLarsIC</th>
      <td>0.38</td>
      <td>0.40</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LassoCV</th>
      <td>0.38</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.04</td>
    </tr>
    <tr>
      <th>ElasticNetCV</th>
      <td>0.38</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>SGDRegressor</th>
      <td>0.38</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LassoLarsCV</th>
      <td>0.38</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LarsCV</th>
      <td>0.38</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>HuberRegressor</th>
      <td>0.37</td>
      <td>0.39</td>
      <td>0.63</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>PoissonRegressor</th>
      <td>0.37</td>
      <td>0.38</td>
      <td>0.63</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>OrthogonalMatchingPursuitCV</th>
      <td>0.36</td>
      <td>0.38</td>
      <td>0.64</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LinearSVR</th>
      <td>0.35</td>
      <td>0.37</td>
      <td>0.64</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>MLPRegressor</th>
      <td>0.35</td>
      <td>0.37</td>
      <td>0.64</td>
      <td>1.08</td>
    </tr>
    <tr>
      <th>TweedieRegressor</th>
      <td>0.28</td>
      <td>0.30</td>
      <td>0.68</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>GammaRegressor</th>
      <td>0.28</td>
      <td>0.30</td>
      <td>0.68</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>RANSACRegressor</th>
      <td>0.24</td>
      <td>0.27</td>
      <td>0.69</td>
      <td>0.04</td>
    </tr>
    <tr>
      <th>OrthogonalMatchingPursuit</th>
      <td>0.24</td>
      <td>0.26</td>
      <td>0.69</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>DecisionTreeRegressor</th>
      <td>0.19</td>
      <td>0.21</td>
      <td>0.72</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>ExtraTreeRegressor</th>
      <td>0.12</td>
      <td>0.15</td>
      <td>0.75</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>LassoLars</th>
      <td>-0.03</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>DummyRegressor</th>
      <td>-0.03</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>ElasticNet</th>
      <td>-0.03</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>Lasso</th>
      <td>-0.03</td>
      <td>-0.01</td>
      <td>0.81</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>PassiveAggressiveRegressor</th>
      <td>-0.61</td>
      <td>-0.57</td>
      <td>1.01</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>GaussianProcessRegressor</th>
      <td>-2.42</td>
      <td>-2.33</td>
      <td>1.47</td>
      <td>0.10</td>
    </tr>
    <tr>
      <th>KernelRidge</th>
      <td>-50.04</td>
      <td>-48.60</td>
      <td>5.69</td>
      <td>0.05</td>
    </tr>
  </tbody>
</table>
</div>



Lazypredict의 Regression 기능을 간단하게 살펴본 결과, 이 라이브러리는 최적의 ML 모델을 자동으로 찾아준다기 보다는 간략하게 여러 ML 모델들의 성능을 비교해보는 용도로 사용하는 것이 적합해 보인다.

<br>

## 참고

- [lazypredict GitHub](https://github.com/shankarpandala/lazypredict)
