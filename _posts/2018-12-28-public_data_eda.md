---
title: "파이썬을 이용한 공공데이터 분석 - 단독/다가구 주택 매매 데이터 분석 EDA편"
date: 2018-12-28 20:00:00 -0400
categories: Analysis
tags: Python EDA
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.5 # same as adding an opacity of 0.5 to a black background
---


# 부동산 데이터 탐색
* 단독다가구(매매)

## 라이브러리


```python
import numpy as np
import pandas as pd
import os
import glob
```

## 데이터 불러오기


```python
path = "Dataset/HOUSE_SALE"
allFiles = glob.glob(os.path.join(path,"*.csv"))
```


```python
#allFiles
```


```python
np_array_list = []
for file_ in allFiles:
    df = pd.read_csv(file_,index_col=None,skiprows = 15, engine='python',encoding="euc-kr")
    np_array_list.append(df.as_matrix())

comb_np_array = np.vstack(np_array_list)
dataset = pd.DataFrame(comb_np_array)
dataset.columns = df.columns
```

    C:\Users\wooil\Anaconda3\envs\PDA\lib\site-packages\ipykernel_launcher.py:4: FutureWarning: Method .as_matrix will be removed in a future version. Use .values instead.
      after removing the cwd from sys.path.



```python
dataset.columns
```




    Index(['시군구', '번지', '주택유형', '도로조건', '연면적(㎡)', '대지면적(㎡)', '계약년월', '계약일',
           '거래금액(만원)', '건축년도', '도로명'],
          dtype='object')




```python
dataset.columns = ["add_1", "add_2", "class", "road_condition",
                   "GFA", "land_area", "year_month", "day",
                   "price", "built_year", "road_name"]
```

## 전체 데이터셋 구조

### Shape


```python
dataset.shape
# 관측치 : 313,710
# 항목 : 11
```




    (313710, 11)



### Head


```python
dataset.head()
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
      <th>add_1</th>
      <th>add_2</th>
      <th>class</th>
      <th>road_condition</th>
      <th>GFA</th>
      <th>land_area</th>
      <th>year_month</th>
      <th>day</th>
      <th>price</th>
      <th>built_year</th>
      <th>road_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강원도 강릉시 교동</td>
      <td>1***</td>
      <td>다가구</td>
      <td>12m미만</td>
      <td>371.58</td>
      <td>219.3</td>
      <td>201512</td>
      <td>1~10</td>
      <td>63,000</td>
      <td>2003</td>
      <td>하슬라로206번길</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강원도 강릉시 교동</td>
      <td>7**</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>122.52</td>
      <td>121</td>
      <td>201512</td>
      <td>11~20</td>
      <td>9,000</td>
      <td>1985</td>
      <td>화부산로117번길</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강원도 강릉시 교동</td>
      <td>1**</td>
      <td>단독</td>
      <td>25m이상</td>
      <td>193.29</td>
      <td>156</td>
      <td>201512</td>
      <td>1~10</td>
      <td>11,500</td>
      <td>1978</td>
      <td>율곡로</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강원도 강릉시 교동</td>
      <td>9**</td>
      <td>단독</td>
      <td>12m미만</td>
      <td>88.62</td>
      <td>178.2</td>
      <td>201512</td>
      <td>21~31</td>
      <td>11,000</td>
      <td>1976</td>
      <td>원대로20번길</td>
    </tr>
    <tr>
      <th>4</th>
      <td>강원도 강릉시 교동</td>
      <td>1**</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>81.67</td>
      <td>185</td>
      <td>201512</td>
      <td>1~10</td>
      <td>9,700</td>
      <td>1982</td>
      <td>강릉대로211번길</td>
    </tr>
  </tbody>
</table>
</div>



### Describe
- 범주형 변수 : add_1(16493), add_2(42), class(2), road_condition(5), day(6), road_name(74958)
- 이산 및 연속형 변수 : year_month(34), GFA, land_area, price, built_year


```python
dataset.describe()
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
      <th>add_1</th>
      <th>add_2</th>
      <th>class</th>
      <th>road_condition</th>
      <th>GFA</th>
      <th>land_area</th>
      <th>year_month</th>
      <th>day</th>
      <th>price</th>
      <th>built_year</th>
      <th>road_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>313710</td>
      <td>313710</td>
      <td>313710</td>
      <td>313710</td>
      <td>313710.0</td>
      <td>313710.0</td>
      <td>313710</td>
      <td>313710</td>
      <td>313710</td>
      <td>311759.0</td>
      <td>298737</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>16493</td>
      <td>42</td>
      <td>2</td>
      <td>5</td>
      <td>51788.0</td>
      <td>10077.0</td>
      <td>34</td>
      <td>6</td>
      <td>22436</td>
      <td>129.0</td>
      <td>74958</td>
    </tr>
    <tr>
      <th>top</th>
      <td>대구광역시 남구 대명동</td>
      <td>1**</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>59.5</td>
      <td>132.0</td>
      <td>201610</td>
      <td>11~20</td>
      <td>20,000</td>
      <td>1980.0</td>
      <td>중앙로</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>2280</td>
      <td>42780</td>
      <td>267381</td>
      <td>223355</td>
      <td>901.0</td>
      <td>2249.0</td>
      <td>12271</td>
      <td>102903</td>
      <td>3723</td>
      <td>13359.0</td>
      <td>586</td>
    </tr>
  </tbody>
</table>
</div>



### 변수 별 고유값 확인


```python
for i in range(len(dataset.columns)):
    print("="*100, "\n\n",
          "# ",list(dataset.columns)[i]," #\n\n",
          dataset[list(dataset.columns)[i]].unique(),"\n\n",
          "="*100, sep="")
```

    ====================================================================================================

    # add_1 #

    ['강원도 강릉시 교동' '강원도 강릉시 구정면 덕현리' '강원도 강릉시 구정면 제비리' ... '충청북도 진천군 진천읍 송두리'
     '충청북도 충주시 금가면 하담리' '충청북도 충주시 살미면 공이리']

    ====================================================================================================
    ====================================================================================================

    # add_2 #

    ['1***' '7**' '1**' '9**' '2*' '3**' '6**' '4**' '2**' '5**' '6*' '8*'
     '1*' '5*' '4*' '*' '8**' '3*' '9*' '7*' '2***' '3***' '4***' '5***'
     '6***' '7***' '산5*' '산2**' '산3*' '산9*' '산7*' '산*' '산1**' '산4*' '산2*'
     '산1*' '산6*' '산3**' '산8*' '산5**' '산1***' '산2***']

    ====================================================================================================
    ====================================================================================================

    # class #

    ['다가구' '단독']

    ====================================================================================================
    ====================================================================================================

    # road_condition #

    ['12m미만' '8m미만' '25m이상' '25m미만' '-']

    ====================================================================================================
    ====================================================================================================

    # GFA #

    [371.58 122.52 193.29 ... 481.51 717.48 366.63]

    ====================================================================================================
    ====================================================================================================

    # land_area #

    [219.3 121.0 156.0 ... 170.11 3652.2 118.27]

    ====================================================================================================
    ====================================================================================================

    # year_month #

    [201512 201601 201602 201603 201604 201605 201606 201607 201608 201609
     201610 201611 201612 201701 201702 201703 201704 201705 201706 201707
     201708 201709 201710 201711 201712 201801 201802 201803 201804 201805
     201806 201807 201808 201809]

    ====================================================================================================
    ====================================================================================================

    # day #

    ['1~10' '11~20' '21~31' '21~29' '21~30' '21~28']

    ====================================================================================================
    ====================================================================================================

    # price #

    ['63,000' '9,000' '11,500' ... '57,382' '14,528' '112,400']

    ====================================================================================================
    ====================================================================================================

    # built_year #

    [2003.0 1985.0 1978.0 1976.0 1982.0 1989.0 1990.0 1998.0 2000.0 2005.0
     1999.0 2007.0 1979.0 1988.0 1971.0 2015.0 2014.0 1980.0 1970.0 1993.0
     1981.0 1950.0 2001.0 2012.0 1996.0 2004.0 1994.0 1974.0 1983.0 1968.0
     1995.0 1992.0 1925.0 2013.0 1957.0 1984.0 1960.0 nan 1997.0 2002.0 2010.0
     1987.0 2008.0 1969.0 2009.0 1955.0 1991.0 1986.0 2011.0 1962.0 1965.0
     2006.0 1977.0 1972.0 1961.0 1959.0 1956.0 1958.0 1975.0 1963.0 1954.0
     1951.0 1944.0 1952.0 1942.0 1945.0 1973.0 1967.0 1948.0 1906.0 1900.0
     1930.0 1909.0 1964.0 1946.0 1915.0 1943.0 1929.0 1934.0 1936.0 1940.0
     1953.0 1947.0 1935.0 1914.0 1939.0 1937.0 1910.0 1920.0 1926.0 1941.0
     1905.0 1921.0 1932.0 1924.0 1938.0 1966.0 1928.0 1931.0 1927.0 1922.0
     1949.0 1902.0 1901.0 1933.0 1919.0 1916.0 1918.0 1923.0 1917.0 2016.0
     1903.0 1911.0 1912.0 1907.0 1904.0 1913.0 1908.0 993.0 970.0 199.0 2017.0
     987.0 998.0 994.0 977.0 2018.0 0.0 976.0 992]

    ====================================================================================================
    ====================================================================================================

    # road_name #

    ['하슬라로206번길' '화부산로117번길' '율곡로' ... '교동11길' '사천후동길' '상방8길']

    ====================================================================================================


### price 변수 콤마 제거


```python
# "price" 변수 천 단위 표시(",") 제거 후 정수 타입으로 변환
dataset["price"] = dataset["price"].str.replace(",","").astype(int)
```

### 결측치 처리


```python
for i in range(len(dataset.columns)) :
    print(list(dataset.columns)[i],"결측치 수 :",sum(dataset[list(dataset.columns)[i]].isnull()))
```

    add_1 결측치 수 : 0
    add_2 결측치 수 : 0
    class 결측치 수 : 0
    road_condition 결측치 수 : 0
    GFA 결측치 수 : 0
    land_area 결측치 수 : 0
    year_month 결측치 수 : 0
    day 결측치 수 : 0
    price 결측치 수 : 0
    built_year 결측치 수 : 1951
    road_name 결측치 수 : 14973



```python
# built_year 결측치 확인
dataset[dataset["built_year"].isnull()==True].head()
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
      <th>add_1</th>
      <th>add_2</th>
      <th>class</th>
      <th>road_condition</th>
      <th>GFA</th>
      <th>land_area</th>
      <th>year_month</th>
      <th>day</th>
      <th>price</th>
      <th>built_year</th>
      <th>road_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>70</th>
      <td>강원도 동해시 천곡동</td>
      <td>1***</td>
      <td>단독</td>
      <td>12m미만</td>
      <td>82.83</td>
      <td>241.4</td>
      <td>201512</td>
      <td>21~31</td>
      <td>17100</td>
      <td>NaN</td>
      <td>항골길</td>
    </tr>
    <tr>
      <th>105</th>
      <td>강원도 속초시 영랑동</td>
      <td>1*</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>104.2</td>
      <td>461</td>
      <td>201512</td>
      <td>11~20</td>
      <td>15000</td>
      <td>NaN</td>
      <td>속초등대길</td>
    </tr>
    <tr>
      <th>137</th>
      <td>강원도 원주시 관설동</td>
      <td>1***</td>
      <td>다가구</td>
      <td>12m미만</td>
      <td>402.65</td>
      <td>205.4</td>
      <td>201512</td>
      <td>11~20</td>
      <td>45000</td>
      <td>NaN</td>
      <td>라옹정길</td>
    </tr>
    <tr>
      <th>171</th>
      <td>강원도 원주시 무실동</td>
      <td>1***</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>54.55</td>
      <td>595</td>
      <td>201512</td>
      <td>1~10</td>
      <td>19100</td>
      <td>NaN</td>
      <td>무실양지길</td>
    </tr>
    <tr>
      <th>178</th>
      <td>강원도 원주시 봉산동</td>
      <td>1***</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>71.58</td>
      <td>173</td>
      <td>201512</td>
      <td>1~10</td>
      <td>14680</td>
      <td>NaN</td>
      <td>봉산2길</td>
    </tr>
  </tbody>
</table>
</div>




```python
# built_year 결측치 대체(중앙값:1989)
dataset["built_year"][dataset["built_year"].isnull()==True] = dataset["built_year"].median()
```

    C:\Users\wooil\Anaconda3\envs\PDA\lib\site-packages\ipykernel_launcher.py:2: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy




```python
# road_name 결측치 확인
dataset[dataset["road_name"].isnull()==True].head()
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
      <th>add_1</th>
      <th>add_2</th>
      <th>class</th>
      <th>road_condition</th>
      <th>GFA</th>
      <th>land_area</th>
      <th>year_month</th>
      <th>day</th>
      <th>price</th>
      <th>built_year</th>
      <th>road_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>154</th>
      <td>강원도 원주시 단구동</td>
      <td>1***</td>
      <td>다가구</td>
      <td>12m미만</td>
      <td>296.86</td>
      <td>194.7</td>
      <td>201512</td>
      <td>21~31</td>
      <td>31000</td>
      <td>1998</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>172</th>
      <td>강원도 원주시 무실동</td>
      <td>7**</td>
      <td>단독</td>
      <td>-</td>
      <td>215.25</td>
      <td>1077</td>
      <td>201512</td>
      <td>1~10</td>
      <td>81000</td>
      <td>2005</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>333</th>
      <td>강원도 평창군 봉평면 평촌리</td>
      <td>2**</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>78.69</td>
      <td>387</td>
      <td>201512</td>
      <td>1~10</td>
      <td>1500</td>
      <td>1963</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>362</th>
      <td>강원도 홍천군 홍천읍 상오안리</td>
      <td>2**</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>296</td>
      <td>1340</td>
      <td>201512</td>
      <td>21~31</td>
      <td>5000</td>
      <td>1996</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>371</th>
      <td>강원도 화천군 간동면 방천리</td>
      <td>1***</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>93</td>
      <td>660</td>
      <td>201512</td>
      <td>11~20</td>
      <td>3500</td>
      <td>1989</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 결측치 포함 관측치 비율 확인
print("도로명 주소 결측치 비율 : ", round(sum(dataset["road_name"].isnull()) / len(dataset), 4) * 100, "%", sep="")
print("="*50)
# 도로명 주소 결측있는 관측치와 결측없는 관측치에 대해 연속형 변수 값의 평균 비교
print("결측_GFA_평균 : ",dataset["GFA"][dataset["road_name"].isnull()==True].mean(),sep="")
print("무결_GFA_평균 : ",dataset["GFA"][dataset["road_name"].isnull()==False].mean(),sep="")
print("="*50)
print("결측_land_area_평균 : ",dataset["land_area"][dataset["road_name"].isnull()==True].mean(),sep="")
print("무결_land_area_평균 : ",dataset["land_area"][dataset["road_name"].isnull()==False].mean(),sep="")
print("="*50)
print("결측_price_평균 : ",dataset["price"][dataset["road_name"].isnull()==True].mean(),sep="")
print("무결_price_평균 : ",dataset["price"][dataset["road_name"].isnull()==False].mean(),sep="")
print("="*50)
print("결측_built_year_평균 : ",dataset["built_year"][dataset["road_name"].isnull()==True].mean(),sep="")
print("무결_built_year_평균 : ",dataset["built_year"][dataset["road_name"].isnull()==False].mean(),sep="")
```

    도로명 주소 결측치 비율 : 4.77%
    ==================================================
    결측_GFA_평균 : 158.81444934214974
    무결_GFA_평균 : 173.22403023395808
    ==================================================
    결측_land_area_평균 : 322.35804848727645
    무결_land_area_평균 : 262.17897347834105
    ==================================================
    결측_price_평균 : 39740.26046884392
    무결_price_평균 : 40141.29863391545
    ==================================================
    결측_built_year_평균 : 1987.4820009350165
    무결_built_year_평균 : 1987.1075661869804


1.결측치 유무를 기준으로 두 집단 간에 각 연속형 변수 별로 통계적으로 유의미한 차이가 있는지 검정을 해볼 여지도 있지만,  
휴리스틱하게 판단해보자면 분포에 큰 차이는 없어 보인다고 판단

2.결측치를 대체하지 않고, 그냥 삭제해도 전체 데이터 손실률이 4.77% 정도로 크리티컬하게 큰 수준은 아니라고 판단




```python
# road_name 결측치 삭제
dataset = dataset[dataset["road_name"].isnull() == False]
```

### 데이터 형 변환


```python
dataset["GFA"] = dataset["GFA"].astype(float)
dataset["land_area"] = dataset["land_area"].astype(float)
dataset["year_month"] = dataset["year_month"].astype(object)
dataset["built_year"] = dataset["built_year"].astype(int)
```


```python
# 데이터 타입 확인
dataset.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 298737 entries, 0 to 313709
    Data columns (total 11 columns):
    add_1             298737 non-null object
    add_2             298737 non-null object
    class             298737 non-null object
    road_condition    298737 non-null object
    GFA               298737 non-null float64
    land_area         298737 non-null float64
    year_month        298737 non-null object
    day               298737 non-null object
    price             298737 non-null int32
    built_year        298737 non-null int32
    road_name         298737 non-null object
    dtypes: float64(2), int32(2), object(7)
    memory usage: 25.1+ MB


## Feature Engineering

- add_1 변수를 분할하여 add_do, add_si 변수 생성


```python
# "add_1" 변수를 분할하여, "add_do", "add_si" 변수 생성
dataset['add_do'] = dataset['add_1'].apply(lambda e: e.split()[0])
dataset['add_si'] = dataset['add_1'].apply(lambda e: e.split()[1])
```


```python
# "add_2" 변수 삭제
del(dataset["add_2"])
```


```python
# dataset 컬럼 순서 변경 하여 df 데이터프레임 생성
df = dataset[["add_1", "add_do", "add_si", "road_name", "class", "road_condition", "year_month", "day",
              "GFA", "land_area", "built_year", "price"]]
```

- FAR(Floor Area Rate) 변수 생성
$$
{FAR} = {GFA \over land area}
$$
- "FAR"이 1이상 이면 1, 1이하 이면, 0값을 할당(Categorical Variable)


```python
df["FAR"] = 0
df["FAR"][df["GFA"]/df["land_area"] >= 1] = 1
df["FAR"]=df["FAR"].astype("object")
```

    C:\Users\wooil\Anaconda3\envs\PDA\lib\site-packages\ipykernel_launcher.py:1: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """Entry point for launching an IPython kernel.
    C:\Users\wooil\Anaconda3\envs\PDA\lib\site-packages\ipykernel_launcher.py:2: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy

    C:\Users\wooil\Anaconda3\envs\PDA\lib\site-packages\pandas\core\generic.py:7626: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      self._update_inplace(new_data)
    C:\Users\wooil\Anaconda3\envs\PDA\lib\site-packages\IPython\core\interactiveshell.py:2961: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      exec(code_obj, self.user_global_ns, self.user_ns)
    C:\Users\wooil\Anaconda3\envs\PDA\lib\site-packages\ipykernel_launcher.py:3: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      This is separate from the ipykernel package so we can avoid doing imports until



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
      <th>add_1</th>
      <th>add_do</th>
      <th>add_si</th>
      <th>road_name</th>
      <th>class</th>
      <th>road_condition</th>
      <th>year_month</th>
      <th>day</th>
      <th>GFA</th>
      <th>land_area</th>
      <th>built_year</th>
      <th>price</th>
      <th>FAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강원도 강릉시 교동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>하슬라로206번길</td>
      <td>다가구</td>
      <td>12m미만</td>
      <td>201512</td>
      <td>1~10</td>
      <td>371.58</td>
      <td>219.3</td>
      <td>2003</td>
      <td>63000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강원도 강릉시 교동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>화부산로117번길</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>201512</td>
      <td>11~20</td>
      <td>122.52</td>
      <td>121.0</td>
      <td>1985</td>
      <td>9000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강원도 강릉시 교동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>율곡로</td>
      <td>단독</td>
      <td>25m이상</td>
      <td>201512</td>
      <td>1~10</td>
      <td>193.29</td>
      <td>156.0</td>
      <td>1978</td>
      <td>11500</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강원도 강릉시 교동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>원대로20번길</td>
      <td>단독</td>
      <td>12m미만</td>
      <td>201512</td>
      <td>21~31</td>
      <td>88.62</td>
      <td>178.2</td>
      <td>1976</td>
      <td>11000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>강원도 강릉시 교동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>강릉대로211번길</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>201512</td>
      <td>1~10</td>
      <td>81.67</td>
      <td>185.0</td>
      <td>1982</td>
      <td>9700</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
## add_do (도/광역시/특별자치시/특별자치도/특별시)
## add_si (시/군/구)
print(df["add_do"].unique())
print("\n")
print(df["add_si"].unique())
```

    ['강원도' '경기도' '경상남도' '경상북도' '광주광역시' '대구광역시' '대전광역시' '부산광역시' '서울특별시'
     '세종특별자치시' '울산광역시' '인천광역시' '전라남도' '전라북도' '제주특별자치도' '충청남도' '충청북도']


    ['강릉시' '고성군' '동해시' '삼척시' '속초시' '양양군' '영월군' '원주시' '인제군' '정선군' '철원군' '춘천시'
     '태백시' '평창군' '홍천군' '화천군' '횡성군' '가평군' '고양덕양구' '고양일산동구' '고양일산서구' '과천시' '광명시'
     '광주시' '구리시' '군포시' '김포시' '남양주시' '동두천시' '부천시' '성남분당구' '성남수정구' '성남중원구'
     '수원권선구' '수원영통구' '수원장안구' '수원팔달구' '시흥시' '안산단원구' '안산상록구' '안성시' '안양동안구'
     '안양만안구' '양주시' '양평군' '여주시' '연천군' '오산시' '용인기흥구' '용인수지구' '용인처인구' '의왕시'
     '의정부시' '이천시' '파주시' '평택시' '포천시' '하남시' '화성시' '거제시' '거창군' '김해시' '남해군' '밀양시'
     '사천시' '산청군' '양산시' '의령군' '진주시' '창녕군' '창원마산합포구' '창원마산회원구' '창원성산구' '창원의창구'
     '창원진해구' '통영시' '하동군' '함안군' '함양군' '합천군' '경산시' '경주시' '고령군' '구미시' '군위군' '김천시'
     '문경시' '봉화군' '상주시' '성주군' '안동시' '영덕군' '영양군' '영주시' '영천시' '예천군' '울릉군' '울진군'
     '의성군' '청도군' '청송군' '칠곡군' '포항남구' '포항북구' '광산구' '남구' '동구' '북구' '서구' '달서구'
     '달성군' '수성구' '중구' '대덕구' '유성구' '강서구' '금정구' '기장군' '동래구' '부산진구' '사상구' '사하구'
     '수영구' '연제구' '영도구' '해운대구' '강남구' '강동구' '강북구' '관악구' '광진구' '구로구' '금천구' '노원구'
     '도봉구' '동대문구' '동작구' '마포구' '서대문구' '서초구' '성동구' '성북구' '송파구' '양천구' '영등포구'
     '용산구' '은평구' '종로구' '중랑구' '금남면' '부강면' '연기면' '연서면' '장군면' '전의면' '조치원읍' '울주군'
     '강화군' '계양구' '남동구' '미추홀구' '부평구' '연수구' '옹진군' '강진군' '고흥군' '곡성군' '광양시' '구례군'
     '나주시' '담양군' '목포시' '무안군' '보성군' '순천시' '신안군' '여수시' '영광군' '영암군' '완도군' '장성군'
     '장흥군' '진도군' '함평군' '해남군' '화순군' '고창군' '군산시' '김제시' '남원시' '무주군' '부안군' '순창군'
     '완주군' '익산시' '임실군' '장수군' '전주덕진구' '전주완산구' '정읍시' '진안군' '서귀포시' '제주시' '계룡시'
     '공주시' '금산군' '논산시' '당진시' '보령시' '부여군' '서산시' '서천군' '아산시' '예산군' '천안동남구'
     '천안서북구' '청양군' '태안군' '홍성군' '괴산군' '단양군' '보은군' '영동군' '옥천군' '음성군' '제천시' '증평군'
     '진천군' '청주상당구' '청주서원구' '청주청원구' '청주흥덕구' '충주시' '양구군' '전동면' '소정면' '연동면' '고운동']


## "Class" EDA


```python
print("# 빈도 \n\n",
      df["class"].value_counts(),sep="")
print("\n")
print("# 비율 \n\n",
      df["class"].value_counts(normalize=True), sep="")
```

    # 빈도

    단독     253724
    다가구     45013
    Name: class, dtype: int64


    # 비율

    단독     0.849322
    다가구    0.150678
    Name: class, dtype: float64



```python
print("# 단독 평균 거래금액(만원) \n\n",
      df["price"][df["class"]=="단독"].mean())
print("\n")
print("# 다가구 평균 거래금액(만원) \n\n",
      df["price"][df["class"]=="다가구"].mean())
```

    # 단독 평균 거래금액(만원)

     34621.44012391417


    # 다가구 평균 거래금액(만원)

     71254.98980294581


#### Class(단독, 다가구) 별 평균 거래금액에 큰 차이가 있으므로 데이터프레임 분리하여 분석할 필요가 있음
- 단독평균 : 3억 4천 만원
- 다가구평균 : 7억 1천 만원


```python
# "df_single" 단독 주택 데이터프레임 및 "df_multi" 다가구 주택 데이터프레임 생성
df_single = df[df["class"]=="단독"]
df_multi = df[df["class"]=="다가구"]
```


```python
df_single.head()
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
      <th>add_1</th>
      <th>add_do</th>
      <th>add_si</th>
      <th>road_name</th>
      <th>class</th>
      <th>road_condition</th>
      <th>year_month</th>
      <th>day</th>
      <th>GFA</th>
      <th>land_area</th>
      <th>built_year</th>
      <th>price</th>
      <th>FAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>강원도 강릉시 교동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>화부산로117번길</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>201512</td>
      <td>11~20</td>
      <td>122.52</td>
      <td>121.0</td>
      <td>1985</td>
      <td>9000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강원도 강릉시 교동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>율곡로</td>
      <td>단독</td>
      <td>25m이상</td>
      <td>201512</td>
      <td>1~10</td>
      <td>193.29</td>
      <td>156.0</td>
      <td>1978</td>
      <td>11500</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강원도 강릉시 교동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>원대로20번길</td>
      <td>단독</td>
      <td>12m미만</td>
      <td>201512</td>
      <td>21~31</td>
      <td>88.62</td>
      <td>178.2</td>
      <td>1976</td>
      <td>11000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>강원도 강릉시 교동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>강릉대로211번길</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>201512</td>
      <td>1~10</td>
      <td>81.67</td>
      <td>185.0</td>
      <td>1982</td>
      <td>9700</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>강원도 강릉시 교동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>명주로77번길</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>201512</td>
      <td>21~31</td>
      <td>169.20</td>
      <td>205.0</td>
      <td>1989</td>
      <td>15500</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_multi.head()
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
      <th>add_1</th>
      <th>add_do</th>
      <th>add_si</th>
      <th>road_name</th>
      <th>class</th>
      <th>road_condition</th>
      <th>year_month</th>
      <th>day</th>
      <th>GFA</th>
      <th>land_area</th>
      <th>built_year</th>
      <th>price</th>
      <th>FAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강원도 강릉시 교동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>하슬라로206번길</td>
      <td>다가구</td>
      <td>12m미만</td>
      <td>201512</td>
      <td>1~10</td>
      <td>371.58</td>
      <td>219.3</td>
      <td>2003</td>
      <td>63000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>강원도 강릉시 내곡동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>관대길36번길</td>
      <td>다가구</td>
      <td>12m미만</td>
      <td>201512</td>
      <td>21~31</td>
      <td>404.75</td>
      <td>254.3</td>
      <td>1989</td>
      <td>56000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>36</th>
      <td>강원도 강릉시 지변동</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>지변길18번길</td>
      <td>다가구</td>
      <td>8m미만</td>
      <td>201512</td>
      <td>11~20</td>
      <td>453.42</td>
      <td>294.0</td>
      <td>2001</td>
      <td>48500</td>
      <td>1</td>
    </tr>
    <tr>
      <th>57</th>
      <td>강원도 동해시 구미동</td>
      <td>강원도</td>
      <td>동해시</td>
      <td>구미3길</td>
      <td>다가구</td>
      <td>12m미만</td>
      <td>201512</td>
      <td>11~20</td>
      <td>657.70</td>
      <td>351.0</td>
      <td>2014</td>
      <td>87000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>67</th>
      <td>강원도 동해시 천곡동</td>
      <td>강원도</td>
      <td>동해시</td>
      <td>동굴로</td>
      <td>다가구</td>
      <td>25m미만</td>
      <td>201512</td>
      <td>21~31</td>
      <td>488.52</td>
      <td>264.0</td>
      <td>2015</td>
      <td>79000</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



## 시군구, 클래스 별 거래금액평균 비교


```python
# 거래금액평균 내림차순 정렬 (단독/다가구)
round(df.groupby(["add_do","add_si","class"])[["price"]].mean(), 0).sort_values(by=['price'], axis=0, ascending=False)
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
      <th></th>
      <th></th>
      <th>price</th>
    </tr>
    <tr>
      <th>add_do</th>
      <th>add_si</th>
      <th>class</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">서울특별시</th>
      <th rowspan="2" valign="top">강남구</th>
      <th>단독</th>
      <td>317941.0</td>
    </tr>
    <tr>
      <th>다가구</th>
      <td>294355.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">서초구</th>
      <th>다가구</th>
      <td>226453.0</td>
    </tr>
    <tr>
      <th>단독</th>
      <td>207172.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">경기도</th>
      <th rowspan="2" valign="top">성남분당구</th>
      <th>단독</th>
      <td>173115.0</td>
    </tr>
    <tr>
      <th>다가구</th>
      <td>153936.0</td>
    </tr>
    <tr>
      <th>과천시</th>
      <th>다가구</th>
      <td>146758.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>송파구</th>
      <th>다가구</th>
      <td>134909.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">경기도</th>
      <th>과천시</th>
      <th>단독</th>
      <td>133695.0</td>
    </tr>
    <tr>
      <th>하남시</th>
      <th>다가구</th>
      <td>127870.0</td>
    </tr>
    <tr>
      <th>용인수지구</th>
      <th>다가구</th>
      <td>120629.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>용산구</th>
      <th>다가구</th>
      <td>116521.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>고양덕양구</th>
      <th>다가구</th>
      <td>115320.0</td>
    </tr>
    <tr>
      <th>남양주시</th>
      <th>다가구</th>
      <td>113539.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">서울특별시</th>
      <th>강동구</th>
      <th>다가구</th>
      <td>113335.0</td>
    </tr>
    <tr>
      <th>마포구</th>
      <th>다가구</th>
      <td>112298.0</td>
    </tr>
    <tr>
      <th>용산구</th>
      <th>단독</th>
      <td>111059.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">경기도</th>
      <th>김포시</th>
      <th>다가구</th>
      <td>109106.0</td>
    </tr>
    <tr>
      <th>용인기흥구</th>
      <th>다가구</th>
      <td>108474.0</td>
    </tr>
    <tr>
      <th>수원영통구</th>
      <th>다가구</th>
      <td>107613.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">서울특별시</th>
      <th>광진구</th>
      <th>다가구</th>
      <td>105084.0</td>
    </tr>
    <tr>
      <th>송파구</th>
      <th>단독</th>
      <td>102833.0</td>
    </tr>
    <tr>
      <th>경기도</th>
      <th>오산시</th>
      <th>다가구</th>
      <td>102121.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">서울특별시</th>
      <th>강동구</th>
      <th>단독</th>
      <td>100410.0</td>
    </tr>
    <tr>
      <th>광진구</th>
      <th>단독</th>
      <td>100049.0</td>
    </tr>
    <tr>
      <th>경기도</th>
      <th>화성시</th>
      <th>다가구</th>
      <td>99659.0</td>
    </tr>
    <tr>
      <th>충청남도</th>
      <th>천안서북구</th>
      <th>다가구</th>
      <td>98935.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>마포구</th>
      <th>단독</th>
      <td>98672.0</td>
    </tr>
    <tr>
      <th>세종특별자치시</th>
      <th>부강면</th>
      <th>다가구</th>
      <td>97485.0</td>
    </tr>
    <tr>
      <th>제주특별자치도</th>
      <th>서귀포시</th>
      <th>다가구</th>
      <td>96926.0</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>강원도</th>
      <th>태백시</th>
      <th>단독</th>
      <td>8693.0</td>
    </tr>
    <tr>
      <th>충청남도</th>
      <th>청양군</th>
      <th>단독</th>
      <td>8670.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>김제시</th>
      <th>단독</th>
      <td>8604.0</td>
    </tr>
    <tr>
      <th>경상북도</th>
      <th>봉화군</th>
      <th>단독</th>
      <td>8563.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">전라북도</th>
      <th>무주군</th>
      <th>단독</th>
      <td>8474.0</td>
    </tr>
    <tr>
      <th>부안군</th>
      <th>단독</th>
      <td>8390.0</td>
    </tr>
    <tr>
      <th>경상남도</th>
      <th>의령군</th>
      <th>단독</th>
      <td>8378.0</td>
    </tr>
    <tr>
      <th>충청북도</th>
      <th>보은군</th>
      <th>단독</th>
      <td>8049.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>임실군</th>
      <th>단독</th>
      <td>7885.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>곡성군</th>
      <th>단독</th>
      <td>7794.0</td>
    </tr>
    <tr>
      <th>충청남도</th>
      <th>서천군</th>
      <th>단독</th>
      <td>7665.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경상북도</th>
      <th>의성군</th>
      <th>단독</th>
      <td>7646.0</td>
    </tr>
    <tr>
      <th>청송군</th>
      <th>단독</th>
      <td>7642.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>영광군</th>
      <th>단독</th>
      <td>7267.0</td>
    </tr>
    <tr>
      <th>경상남도</th>
      <th>합천군</th>
      <th>단독</th>
      <td>7025.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">전라북도</th>
      <th>진안군</th>
      <th>단독</th>
      <td>6938.0</td>
    </tr>
    <tr>
      <th>장수군</th>
      <th>단독</th>
      <td>6933.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">전라남도</th>
      <th>해남군</th>
      <th>단독</th>
      <td>6920.0</td>
    </tr>
    <tr>
      <th>완도군</th>
      <th>단독</th>
      <td>6879.0</td>
    </tr>
    <tr>
      <th>함평군</th>
      <th>단독</th>
      <td>6877.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>순창군</th>
      <th>단독</th>
      <td>6858.0</td>
    </tr>
    <tr>
      <th>경상북도</th>
      <th>영양군</th>
      <th>단독</th>
      <td>6796.0</td>
    </tr>
    <tr>
      <th rowspan="8" valign="top">전라남도</th>
      <th>영암군</th>
      <th>단독</th>
      <td>6543.0</td>
    </tr>
    <tr>
      <th>장흥군</th>
      <th>단독</th>
      <td>6385.0</td>
    </tr>
    <tr>
      <th>보성군</th>
      <th>단독</th>
      <td>6346.0</td>
    </tr>
    <tr>
      <th>진도군</th>
      <th>단독</th>
      <td>5140.0</td>
    </tr>
    <tr>
      <th>보성군</th>
      <th>다가구</th>
      <td>5000.0</td>
    </tr>
    <tr>
      <th>강진군</th>
      <th>단독</th>
      <td>4990.0</td>
    </tr>
    <tr>
      <th>고흥군</th>
      <th>단독</th>
      <td>4762.0</td>
    </tr>
    <tr>
      <th>신안군</th>
      <th>단독</th>
      <td>4044.0</td>
    </tr>
  </tbody>
</table>
<p>510 rows × 1 columns</p>
</div>




```python
# 거래금액평균 내림차순 정렬 (전체)
round(df.groupby(["add_do","add_si"])[["price"]].mean(), 0).sort_values(by=['price'], axis=0, ascending=False)
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
      <th></th>
      <th>price</th>
    </tr>
    <tr>
      <th>add_do</th>
      <th>add_si</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">서울특별시</th>
      <th>강남구</th>
      <td>309302.0</td>
    </tr>
    <tr>
      <th>서초구</th>
      <td>212412.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>성남분당구</th>
      <td>164492.0</td>
    </tr>
    <tr>
      <th>과천시</th>
      <td>137597.0</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">서울특별시</th>
      <th>용산구</th>
      <td>112151.0</td>
    </tr>
    <tr>
      <th>송파구</th>
      <td>110368.0</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>104023.0</td>
    </tr>
    <tr>
      <th>마포구</th>
      <td>102899.0</td>
    </tr>
    <tr>
      <th>광진구</th>
      <td>101403.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>용인수지구</th>
      <td>91711.0</td>
    </tr>
    <tr>
      <th>수원영통구</th>
      <td>91016.0</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">서울특별시</th>
      <th>동작구</th>
      <td>90298.0</td>
    </tr>
    <tr>
      <th>성동구</th>
      <td>89452.0</td>
    </tr>
    <tr>
      <th>중구</th>
      <td>89364.0</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>89104.0</td>
    </tr>
    <tr>
      <th>종로구</th>
      <td>87357.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>용인기흥구</th>
      <td>84976.0</td>
    </tr>
    <tr>
      <th>안산상록구</th>
      <td>83380.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>강서구</th>
      <td>80783.0</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">경기도</th>
      <th>고양일산동구</th>
      <td>79757.0</td>
    </tr>
    <tr>
      <th>의왕시</th>
      <td>78809.0</td>
    </tr>
    <tr>
      <th>하남시</th>
      <td>76667.0</td>
    </tr>
    <tr>
      <th>고양덕양구</th>
      <td>73917.0</td>
    </tr>
    <tr>
      <th>오산시</th>
      <td>73492.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>서대문구</th>
      <td>71637.0</td>
    </tr>
    <tr>
      <th>경기도</th>
      <th>고양일산서구</th>
      <td>70598.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">서울특별시</th>
      <th>영등포구</th>
      <td>70223.0</td>
    </tr>
    <tr>
      <th>양천구</th>
      <td>69779.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>안산단원구</th>
      <td>69717.0</td>
    </tr>
    <tr>
      <th>군포시</th>
      <td>69367.0</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>고창군</th>
      <td>9678.0</td>
    </tr>
    <tr>
      <th>경상남도</th>
      <th>의령군</th>
      <td>9640.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">충청남도</th>
      <th>부여군</th>
      <td>9112.0</td>
    </tr>
    <tr>
      <th>청양군</th>
      <td>9095.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>부안군</th>
      <td>9022.0</td>
    </tr>
    <tr>
      <th>충청북도</th>
      <th>보은군</th>
      <td>8986.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>무주군</th>
      <td>8901.0</td>
    </tr>
    <tr>
      <th>강원도</th>
      <th>태백시</th>
      <td>8734.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>임실군</th>
      <td>8731.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경상북도</th>
      <th>청송군</th>
      <td>8663.0</td>
    </tr>
    <tr>
      <th>봉화군</th>
      <td>8587.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>곡성군</th>
      <td>8321.0</td>
    </tr>
    <tr>
      <th>경상북도</th>
      <th>의성군</th>
      <td>8180.0</td>
    </tr>
    <tr>
      <th>충청남도</th>
      <th>서천군</th>
      <td>8175.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">전라남도</th>
      <th>영광군</th>
      <td>7374.0</td>
    </tr>
    <tr>
      <th>해남군</th>
      <td>7346.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>진안군</th>
      <td>7214.0</td>
    </tr>
    <tr>
      <th>경상남도</th>
      <th>합천군</th>
      <td>7160.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>함평군</th>
      <td>6972.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>장수군</th>
      <td>6933.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>완도군</th>
      <td>6926.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>순창군</th>
      <td>6858.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>영암군</th>
      <td>6851.0</td>
    </tr>
    <tr>
      <th>경상북도</th>
      <th>영양군</th>
      <td>6796.0</td>
    </tr>
    <tr>
      <th rowspan="6" valign="top">전라남도</th>
      <th>장흥군</th>
      <td>6411.0</td>
    </tr>
    <tr>
      <th>보성군</th>
      <td>6343.0</td>
    </tr>
    <tr>
      <th>진도군</th>
      <td>5398.0</td>
    </tr>
    <tr>
      <th>강진군</th>
      <td>5010.0</td>
    </tr>
    <tr>
      <th>고흥군</th>
      <td>4842.0</td>
    </tr>
    <tr>
      <th>신안군</th>
      <td>4097.0</td>
    </tr>
  </tbody>
</table>
<p>260 rows × 1 columns</p>
</div>




```python
# 거래금액평균 내림차순 정렬 (단독)
round(df_single.groupby(["add_do","add_si","class"])[["price"]].mean(), 0).sort_values(by=['price'], axis=0, ascending=False)
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
      <th></th>
      <th></th>
      <th>price</th>
    </tr>
    <tr>
      <th>add_do</th>
      <th>add_si</th>
      <th>class</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">서울특별시</th>
      <th>강남구</th>
      <th>단독</th>
      <td>317941.0</td>
    </tr>
    <tr>
      <th>서초구</th>
      <th>단독</th>
      <td>207172.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>성남분당구</th>
      <th>단독</th>
      <td>173115.0</td>
    </tr>
    <tr>
      <th>과천시</th>
      <th>단독</th>
      <td>133695.0</td>
    </tr>
    <tr>
      <th rowspan="10" valign="top">서울특별시</th>
      <th>용산구</th>
      <th>단독</th>
      <td>111059.0</td>
    </tr>
    <tr>
      <th>송파구</th>
      <th>단독</th>
      <td>102833.0</td>
    </tr>
    <tr>
      <th>강동구</th>
      <th>단독</th>
      <td>100410.0</td>
    </tr>
    <tr>
      <th>광진구</th>
      <th>단독</th>
      <td>100049.0</td>
    </tr>
    <tr>
      <th>마포구</th>
      <th>단독</th>
      <td>98672.0</td>
    </tr>
    <tr>
      <th>동작구</th>
      <th>단독</th>
      <td>88672.0</td>
    </tr>
    <tr>
      <th>중구</th>
      <th>단독</th>
      <td>88288.0</td>
    </tr>
    <tr>
      <th>관악구</th>
      <th>단독</th>
      <td>88074.0</td>
    </tr>
    <tr>
      <th>종로구</th>
      <th>단독</th>
      <td>87847.0</td>
    </tr>
    <tr>
      <th>성동구</th>
      <th>단독</th>
      <td>86840.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>안산상록구</th>
      <th>단독</th>
      <td>82359.0</td>
    </tr>
    <tr>
      <th>용인수지구</th>
      <th>단독</th>
      <td>81783.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>강서구</th>
      <th>단독</th>
      <td>81684.0</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">경기도</th>
      <th>고양일산동구</th>
      <th>단독</th>
      <td>78866.0</td>
    </tr>
    <tr>
      <th>의왕시</th>
      <th>단독</th>
      <td>78606.0</td>
    </tr>
    <tr>
      <th>수원영통구</th>
      <th>단독</th>
      <td>76703.0</td>
    </tr>
    <tr>
      <th>용인기흥구</th>
      <th>단독</th>
      <td>74131.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">서울특별시</th>
      <th>양천구</th>
      <th>단독</th>
      <td>70935.0</td>
    </tr>
    <tr>
      <th>서대문구</th>
      <th>단독</th>
      <td>70834.0</td>
    </tr>
    <tr>
      <th>영등포구</th>
      <th>단독</th>
      <td>69970.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>하남시</th>
      <th>단독</th>
      <td>69817.0</td>
    </tr>
    <tr>
      <th>군포시</th>
      <th>단독</th>
      <td>69101.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">서울특별시</th>
      <th>금천구</th>
      <th>단독</th>
      <td>68877.0</td>
    </tr>
    <tr>
      <th>은평구</th>
      <th>단독</th>
      <td>67615.0</td>
    </tr>
    <tr>
      <th>경기도</th>
      <th>오산시</th>
      <th>단독</th>
      <td>64096.0</td>
    </tr>
    <tr>
      <th>세종특별자치시</th>
      <th>고운동</th>
      <th>단독</th>
      <td>63000.0</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>충청남도</th>
      <th>부여군</th>
      <th>단독</th>
      <td>8807.0</td>
    </tr>
    <tr>
      <th>강원도</th>
      <th>태백시</th>
      <th>단독</th>
      <td>8693.0</td>
    </tr>
    <tr>
      <th>충청남도</th>
      <th>청양군</th>
      <th>단독</th>
      <td>8670.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>김제시</th>
      <th>단독</th>
      <td>8604.0</td>
    </tr>
    <tr>
      <th>경상북도</th>
      <th>봉화군</th>
      <th>단독</th>
      <td>8563.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">전라북도</th>
      <th>무주군</th>
      <th>단독</th>
      <td>8474.0</td>
    </tr>
    <tr>
      <th>부안군</th>
      <th>단독</th>
      <td>8390.0</td>
    </tr>
    <tr>
      <th>경상남도</th>
      <th>의령군</th>
      <th>단독</th>
      <td>8378.0</td>
    </tr>
    <tr>
      <th>충청북도</th>
      <th>보은군</th>
      <th>단독</th>
      <td>8049.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>임실군</th>
      <th>단독</th>
      <td>7885.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>곡성군</th>
      <th>단독</th>
      <td>7794.0</td>
    </tr>
    <tr>
      <th>충청남도</th>
      <th>서천군</th>
      <th>단독</th>
      <td>7665.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경상북도</th>
      <th>의성군</th>
      <th>단독</th>
      <td>7646.0</td>
    </tr>
    <tr>
      <th>청송군</th>
      <th>단독</th>
      <td>7642.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>영광군</th>
      <th>단독</th>
      <td>7267.0</td>
    </tr>
    <tr>
      <th>경상남도</th>
      <th>합천군</th>
      <th>단독</th>
      <td>7025.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">전라북도</th>
      <th>진안군</th>
      <th>단독</th>
      <td>6938.0</td>
    </tr>
    <tr>
      <th>장수군</th>
      <th>단독</th>
      <td>6933.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">전라남도</th>
      <th>해남군</th>
      <th>단독</th>
      <td>6920.0</td>
    </tr>
    <tr>
      <th>완도군</th>
      <th>단독</th>
      <td>6879.0</td>
    </tr>
    <tr>
      <th>함평군</th>
      <th>단독</th>
      <td>6877.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>순창군</th>
      <th>단독</th>
      <td>6858.0</td>
    </tr>
    <tr>
      <th>경상북도</th>
      <th>영양군</th>
      <th>단독</th>
      <td>6796.0</td>
    </tr>
    <tr>
      <th rowspan="7" valign="top">전라남도</th>
      <th>영암군</th>
      <th>단독</th>
      <td>6543.0</td>
    </tr>
    <tr>
      <th>장흥군</th>
      <th>단독</th>
      <td>6385.0</td>
    </tr>
    <tr>
      <th>보성군</th>
      <th>단독</th>
      <td>6346.0</td>
    </tr>
    <tr>
      <th>진도군</th>
      <th>단독</th>
      <td>5140.0</td>
    </tr>
    <tr>
      <th>강진군</th>
      <th>단독</th>
      <td>4990.0</td>
    </tr>
    <tr>
      <th>고흥군</th>
      <th>단독</th>
      <td>4762.0</td>
    </tr>
    <tr>
      <th>신안군</th>
      <th>단독</th>
      <td>4044.0</td>
    </tr>
  </tbody>
</table>
<p>260 rows × 1 columns</p>
</div>




```python
# 거래금액평균 내림차순 정렬 (다가구)
round(df_multi.groupby(["add_do","add_si","class"])[["price"]].mean(), 0).sort_values(by=['price'], axis=0, ascending=False)
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
      <th></th>
      <th></th>
      <th>price</th>
    </tr>
    <tr>
      <th>add_do</th>
      <th>add_si</th>
      <th>class</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">서울특별시</th>
      <th>강남구</th>
      <th>다가구</th>
      <td>294355.0</td>
    </tr>
    <tr>
      <th>서초구</th>
      <th>다가구</th>
      <td>226453.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>성남분당구</th>
      <th>다가구</th>
      <td>153936.0</td>
    </tr>
    <tr>
      <th>과천시</th>
      <th>다가구</th>
      <td>146758.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>송파구</th>
      <th>다가구</th>
      <td>134909.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>하남시</th>
      <th>다가구</th>
      <td>127870.0</td>
    </tr>
    <tr>
      <th>용인수지구</th>
      <th>다가구</th>
      <td>120629.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>용산구</th>
      <th>다가구</th>
      <td>116521.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>고양덕양구</th>
      <th>다가구</th>
      <td>115320.0</td>
    </tr>
    <tr>
      <th>남양주시</th>
      <th>다가구</th>
      <td>113539.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">서울특별시</th>
      <th>강동구</th>
      <th>다가구</th>
      <td>113335.0</td>
    </tr>
    <tr>
      <th>마포구</th>
      <th>다가구</th>
      <td>112298.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">경기도</th>
      <th>김포시</th>
      <th>다가구</th>
      <td>109106.0</td>
    </tr>
    <tr>
      <th>용인기흥구</th>
      <th>다가구</th>
      <td>108474.0</td>
    </tr>
    <tr>
      <th>수원영통구</th>
      <th>다가구</th>
      <td>107613.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>광진구</th>
      <th>다가구</th>
      <td>105084.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기도</th>
      <th>오산시</th>
      <th>다가구</th>
      <td>102121.0</td>
    </tr>
    <tr>
      <th>화성시</th>
      <th>다가구</th>
      <td>99659.0</td>
    </tr>
    <tr>
      <th>충청남도</th>
      <th>천안서북구</th>
      <th>다가구</th>
      <td>98935.0</td>
    </tr>
    <tr>
      <th>세종특별자치시</th>
      <th>부강면</th>
      <th>다가구</th>
      <td>97485.0</td>
    </tr>
    <tr>
      <th>제주특별자치도</th>
      <th>서귀포시</th>
      <th>다가구</th>
      <td>96926.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">서울특별시</th>
      <th>동작구</th>
      <th>다가구</th>
      <td>95264.0</td>
    </tr>
    <tr>
      <th>성동구</th>
      <th>다가구</th>
      <td>95195.0</td>
    </tr>
    <tr>
      <th>충청남도</th>
      <th>천안동남구</th>
      <th>다가구</th>
      <td>95066.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>중구</th>
      <th>다가구</th>
      <td>94118.0</td>
    </tr>
    <tr>
      <th>경상남도</th>
      <th>의령군</th>
      <th>다가구</th>
      <td>93583.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>관악구</th>
      <th>다가구</th>
      <td>93011.0</td>
    </tr>
    <tr>
      <th>경기도</th>
      <th>평택시</th>
      <th>다가구</th>
      <td>92190.0</td>
    </tr>
    <tr>
      <th>제주특별자치도</th>
      <th>제주시</th>
      <th>다가구</th>
      <td>89380.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">경기도</th>
      <th>안양동안구</th>
      <th>다가구</th>
      <td>88558.0</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>동두천시</th>
      <th>다가구</th>
      <td>34641.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">강원도</th>
      <th>정선군</th>
      <th>다가구</th>
      <td>34250.0</td>
    </tr>
    <tr>
      <th>양양군</th>
      <th>다가구</th>
      <td>33928.0</td>
    </tr>
    <tr>
      <th>홍천군</th>
      <th>다가구</th>
      <td>33551.0</td>
    </tr>
    <tr>
      <th>충청남도</th>
      <th>청양군</th>
      <th>다가구</th>
      <td>33500.0</td>
    </tr>
    <tr>
      <th>강원도</th>
      <th>횡성군</th>
      <th>다가구</th>
      <td>32311.0</td>
    </tr>
    <tr>
      <th>부산광역시</th>
      <th>중구</th>
      <th>다가구</th>
      <td>32099.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>영암군</th>
      <th>다가구</th>
      <td>31833.0</td>
    </tr>
    <tr>
      <th>부산광역시</th>
      <th>사하구</th>
      <th>다가구</th>
      <td>30995.0</td>
    </tr>
    <tr>
      <th>인천광역시</th>
      <th>강화군</th>
      <th>다가구</th>
      <td>30137.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>진도군</th>
      <th>다가구</th>
      <td>30100.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">강원도</th>
      <th>철원군</th>
      <th>다가구</th>
      <td>28500.0</td>
    </tr>
    <tr>
      <th>평창군</th>
      <th>다가구</th>
      <td>28215.0</td>
    </tr>
    <tr>
      <th>부산광역시</th>
      <th>해운대구</th>
      <th>다가구</th>
      <td>26762.0</td>
    </tr>
    <tr>
      <th>경상북도</th>
      <th>고령군</th>
      <th>다가구</th>
      <td>23688.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>함평군</th>
      <th>다가구</th>
      <td>22250.0</td>
    </tr>
    <tr>
      <th>부산광역시</th>
      <th>서구</th>
      <th>다가구</th>
      <td>22211.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>신안군</th>
      <th>다가구</th>
      <td>22000.0</td>
    </tr>
    <tr>
      <th>부산광역시</th>
      <th>영도구</th>
      <th>다가구</th>
      <td>21945.0</td>
    </tr>
    <tr>
      <th>인천광역시</th>
      <th>동구</th>
      <th>다가구</th>
      <td>21527.0</td>
    </tr>
    <tr>
      <th>부산광역시</th>
      <th>동구</th>
      <th>다가구</th>
      <td>20593.0</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">전라남도</th>
      <th>장흥군</th>
      <th>다가구</th>
      <td>20000.0</td>
    </tr>
    <tr>
      <th>담양군</th>
      <th>다가구</th>
      <td>19000.0</td>
    </tr>
    <tr>
      <th>완도군</th>
      <th>다가구</th>
      <td>18319.0</td>
    </tr>
    <tr>
      <th>화순군</th>
      <th>다가구</th>
      <td>17983.0</td>
    </tr>
    <tr>
      <th>강원도</th>
      <th>태백시</th>
      <th>다가구</th>
      <td>17000.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>강진군</th>
      <th>다가구</th>
      <td>16000.0</td>
    </tr>
    <tr>
      <th>강원도</th>
      <th>양구군</th>
      <th>다가구</th>
      <td>14750.0</td>
    </tr>
    <tr>
      <th>경상북도</th>
      <th>봉화군</th>
      <th>다가구</th>
      <td>11100.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>보성군</th>
      <th>다가구</th>
      <td>5000.0</td>
    </tr>
  </tbody>
</table>
<p>250 rows × 1 columns</p>
</div>



### 도로명 기준


```python
# 거래금액평균 내림차순 정렬
round(df.groupby(["add_do","add_si","road_name"])[["price"]].mean(), 0).sort_values(by=['price'], axis=0, ascending=False)
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
      <th></th>
      <th></th>
      <th>price</th>
    </tr>
    <tr>
      <th>add_do</th>
      <th>add_si</th>
      <th>road_name</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>경기도</th>
      <th>용인처인구</th>
      <th>죽양대로904번길</th>
      <td>4021645.0</td>
    </tr>
    <tr>
      <th>울산광역시</th>
      <th>남구</th>
      <th>수암로128번길</th>
      <td>1612846.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">서울특별시</th>
      <th rowspan="2" valign="top">강남구</th>
      <th>도곡로</th>
      <td>1428750.0</td>
    </tr>
    <tr>
      <th>도산대로61길</th>
      <td>1408400.0</td>
    </tr>
    <tr>
      <th>광주광역시</th>
      <th>서구</th>
      <th>상무중앙로</th>
      <td>1390000.0</td>
    </tr>
    <tr>
      <th rowspan="16" valign="top">서울특별시</th>
      <th>강남구</th>
      <th>도산대로55길</th>
      <td>1385300.0</td>
    </tr>
    <tr>
      <th>용산구</th>
      <th>이태원로55라길</th>
      <td>1356366.0</td>
    </tr>
    <tr>
      <th>강남구</th>
      <th>선릉로157길</th>
      <td>1210000.0</td>
    </tr>
    <tr>
      <th>종로구</th>
      <th>종로62길</th>
      <td>1160200.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">강남구</th>
      <th>도산대로13길</th>
      <td>1117500.0</td>
    </tr>
    <tr>
      <th>선릉로64길</th>
      <td>1110000.0</td>
    </tr>
    <tr>
      <th>용산구</th>
      <th>회나무로44길</th>
      <td>1000000.0</td>
    </tr>
    <tr>
      <th>강남구</th>
      <th>언주로168길</th>
      <td>1000000.0</td>
    </tr>
    <tr>
      <th>마포구</th>
      <th>성암로13길</th>
      <td>940000.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">강남구</th>
      <th>논현로150길</th>
      <td>937000.0</td>
    </tr>
    <tr>
      <th>압구정로12길</th>
      <td>914856.0</td>
    </tr>
    <tr>
      <th>용산구</th>
      <th>이태원로55길</th>
      <td>913000.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">강남구</th>
      <th>선릉로132길</th>
      <td>900000.0</td>
    </tr>
    <tr>
      <th>영동대로122길</th>
      <td>857193.0</td>
    </tr>
    <tr>
      <th>성북구</th>
      <th>대사관로13길</th>
      <td>810000.0</td>
    </tr>
    <tr>
      <th>강남구</th>
      <th>테헤란로25길</th>
      <td>800000.0</td>
    </tr>
    <tr>
      <th>대구광역시</th>
      <th>달서구</th>
      <th>와룡로33길</th>
      <td>800000.0</td>
    </tr>
    <tr>
      <th>서울특별시</th>
      <th>강남구</th>
      <th>삼성로112길</th>
      <td>785000.0</td>
    </tr>
    <tr>
      <th>충청남도</th>
      <th>천안동남구</th>
      <th>발산4길</th>
      <td>779700.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">서울특별시</th>
      <th>용산구</th>
      <th>이태원로55가길</th>
      <td>777214.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">강남구</th>
      <th>언주로170길</th>
      <td>770000.0</td>
    </tr>
    <tr>
      <th>테헤란로6길</th>
      <td>749000.0</td>
    </tr>
    <tr>
      <th>경기도</th>
      <th>부천시</th>
      <th>신흥로469번길</th>
      <td>730200.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">서울특별시</th>
      <th>강남구</th>
      <th>역삼로</th>
      <td>730000.0</td>
    </tr>
    <tr>
      <th>마포구</th>
      <th>어울마당로</th>
      <td>712500.0</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>경상남도</th>
      <th>거창군</th>
      <th>내춘길</th>
      <td>330.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">전라남도</th>
      <th>해남군</th>
      <th>신창안길</th>
      <td>327.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">장흥군</th>
      <th>효자2길</th>
      <td>315.0</td>
    </tr>
    <tr>
      <th>농안길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>정읍시</th>
      <th>상신양길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">전라남도</th>
      <th>영암군</th>
      <th>장선길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th>고흥군</th>
      <th>도촌1길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th>완도군</th>
      <th>생일로171번길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th>고흥군</th>
      <th>충무사길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>부안군</th>
      <th>성황3길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>장흥군</th>
      <th>부평길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th>강원도</th>
      <th>태백시</th>
      <th>계산2길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">전라남도</th>
      <th rowspan="2" valign="top">고흥군</th>
      <th>유동길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th>당곤길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th>경상남도</th>
      <th>거창군</th>
      <th>가현길</th>
      <td>300.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>고흥군</th>
      <th>쇠머리길</th>
      <td>299.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>정읍시</th>
      <th>신영길</th>
      <td>297.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">전라남도</th>
      <th>무안군</th>
      <th>연동길</th>
      <td>285.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">고흥군</th>
      <th>원등길</th>
      <td>280.0</td>
    </tr>
    <tr>
      <th>두원동촌1길</th>
      <td>279.0</td>
    </tr>
    <tr>
      <th>전라북도</th>
      <th>김제시</th>
      <th>종덕1길</th>
      <td>270.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">전라남도</th>
      <th rowspan="2" valign="top">고흥군</th>
      <th>방란길</th>
      <td>261.0</td>
    </tr>
    <tr>
      <th>금산신흥길</th>
      <td>250.0</td>
    </tr>
    <tr>
      <th>완도군</th>
      <th>생일로579번길</th>
      <td>250.0</td>
    </tr>
    <tr>
      <th>충청북도</th>
      <th>영동군</th>
      <th>굴우길</th>
      <td>250.0</td>
    </tr>
    <tr>
      <th>경상북도</th>
      <th>청송군</th>
      <th>황목길</th>
      <td>215.0</td>
    </tr>
    <tr>
      <th>전라남도</th>
      <th>고흥군</th>
      <th>풍류2길</th>
      <td>210.0</td>
    </tr>
    <tr>
      <th>경상북도</th>
      <th>울진군</th>
      <th>남산동길</th>
      <td>200.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">전라남도</th>
      <th>곡성군</th>
      <th>제월평촌길</th>
      <td>165.0</td>
    </tr>
    <tr>
      <th>진도군</th>
      <th>의신송정길</th>
      <td>150.0</td>
    </tr>
  </tbody>
</table>
<p>85633 rows × 1 columns</p>
</div>



#### Outliers?


```python
df[df["road_name"]=="죽양대로904번길"]
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
      <th>add_1</th>
      <th>add_do</th>
      <th>add_si</th>
      <th>road_name</th>
      <th>class</th>
      <th>road_condition</th>
      <th>year_month</th>
      <th>day</th>
      <th>GFA</th>
      <th>land_area</th>
      <th>built_year</th>
      <th>price</th>
      <th>FAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>309457</th>
      <td>경기도 용인처인구 백암면 백봉리</td>
      <td>경기도</td>
      <td>용인처인구</td>
      <td>죽양대로904번길</td>
      <td>단독</td>
      <td>8m미만</td>
      <td>201809</td>
      <td>11~20</td>
      <td>31165.14</td>
      <td>22552.0</td>
      <td>2014</td>
      <td>4021645</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[df["road_name"]=="수암로128번길"]
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
      <th>add_1</th>
      <th>add_do</th>
      <th>add_si</th>
      <th>road_name</th>
      <th>class</th>
      <th>road_condition</th>
      <th>year_month</th>
      <th>day</th>
      <th>GFA</th>
      <th>land_area</th>
      <th>built_year</th>
      <th>price</th>
      <th>FAR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>232617</th>
      <td>울산광역시 남구 야음동</td>
      <td>울산광역시</td>
      <td>남구</td>
      <td>수암로128번길</td>
      <td>단독</td>
      <td>25m미만</td>
      <td>201711</td>
      <td>1~10</td>
      <td>2317.57</td>
      <td>6706.0</td>
      <td>1989</td>
      <td>1612846</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## Visualization


```python
# 그래프를 노트북 안에 그리기 위해 설정
%matplotlib inline

# 필요한 패키지와 라이브러리를 가져옴
import matplotlib as mpl
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

# 그래프에서 마이너스 폰트 깨지는 문제에 대한 대처
mpl.rcParams['axes.unicode_minus'] = False

# 폰트 설정
font_location = 'C:/Windows/Fonts/malgunbd.ttf'
font_name = fm.FontProperties(fname = font_location).get_name()
mpl.rc('font', family = font_name)
```

### Bar-plot


```python
data=df.groupby(["add_do"])[["price"]].mean().sort_values(by=['price'], axis=0, ascending=False)

plt.figure(1, figsize=(10, 6))
plt.title("지역 별 주택(단독, 다가구) 평균 거래 금액(단위:만원)",size=20)
plt.xlabel("지역", size=16)
plt.ylabel("평균거래금액(만원)", size=16)
plt.bar(data.index,data["price"],facecolor="red", alpha=0.5)
plt.xticks(rotation=45)
plt.grid(True)
```


![png](/assets/img/post_img/2018-12-28-public_data_eda_files/2018-12-28-public_data_eda_60_0.png)



```python
data=df_single.groupby(["add_do"])[["price"]].mean().sort_values(by=['price'], axis=0, ascending=False)

plt.figure(1, figsize=(10, 6))
plt.title("지역 별 단독주택 평균 거래 금액(단위:만원)",size=20)
plt.xlabel("지역", size=16)
plt.ylabel("평균거래금액(만원)", size=16)
plt.bar(data.index,data["price"],facecolor="orange", alpha=0.5)
plt.xticks(rotation=45)
plt.grid(True)
```


![png](/assets/img/post_img/2018-12-28-public_data_eda_files/2018-12-28-public_data_eda_61_0.png)



```python
data=df_multi.groupby(["add_do"])[["price"]].mean().sort_values(by=['price'], axis=0, ascending=False)

plt.figure(1, figsize=(10, 6))
plt.title("지역 별 다가구주택 평균 거래 금액(단위:만원)",size=20)
plt.xlabel("지역", size=16)
plt.ylabel("평균거래금액(만원)", size=16)
plt.bar(data.index,data["price"],facecolor="blue", alpha=0.5)
plt.xticks(rotation=45)
plt.grid(True)
```


![png](/assets/img/post_img/2018-12-28-public_data_eda_files/2018-12-28-public_data_eda_62_0.png)



```python
data=df.groupby(["FAR"])[["price"]].mean().sort_values(by=['price'], axis=0, ascending=False)

plt.figure(1, figsize=(10, 6))
plt.title("FAR 별 주택(단독, 다가구) 평균 거래 금액(단위:만원)",size=20)
plt.xlabel("FAR", size=16)
plt.ylabel("평균거래금액(만원)", size=16)
plt.bar(data.index,data["price"],facecolor="red", alpha=0.5, width=0.2)
plt.xticks(rotation=45)
plt.grid(True)
```


![png](/assets/img/post_img/2018-12-28-public_data_eda_files/2018-12-28-public_data_eda_63_0.png)



```python
data=df_single.groupby(["FAR"])[["price"]].mean().sort_values(by=['price'], axis=0, ascending=False)

plt.figure(1, figsize=(10, 6))
plt.title("FAR 별 단독주택 평균 거래 금액(단위:만원)",size=20)
plt.xlabel("FAR", size=16)
plt.ylabel("평균거래금액(만원)", size=16)
plt.bar(data.index,data["price"],facecolor="orange", alpha=0.5, width=0.2)
plt.xticks(rotation=45)
plt.grid(True)
```


![png](/assets/img/post_img/2018-12-28-public_data_eda_files/2018-12-28-public_data_eda_64_0.png)



```python
data=df_multi.groupby(["FAR"])[["price"]].mean().sort_values(by=['price'], axis=0, ascending=False)

plt.figure(1, figsize=(10, 6))
plt.title("FAR 별 다가구주택 평균 거래 금액(단위:만원)",size=20)
plt.xlabel("FAR", size=16)
plt.ylabel("평균거래금액(만원)", size=16)
plt.bar(data.index,data["price"],facecolor="blue", alpha=0.5, width=0.2)
plt.xticks(rotation=45)
plt.grid(True)
```


![png](/assets/img/post_img/2018-12-28-public_data_eda_files/2018-12-28-public_data_eda_65_0.png)


### Scatter-plot


```python
data=df
plt.scatter(data["GFA"], data["price"], facecolor="orange", alpha=0.3)
plt.xscale("log")
```


![png](/assets/img/post_img/2018-12-28-public_data_eda_files/2018-12-28-public_data_eda_67_0.png)



```python
data=df
plt.scatter(data["land_area"], data["price"],facecolor="orange", alpha=0.3)
plt.xscale("log")
```


![png](/assets/img/post_img/2018-12-28-public_data_eda_files/2018-12-28-public_data_eda_68_0.png)



```python
data=df
plt.scatter(data["GFA"]/data["land_area"], data["price"],facecolor="orange", alpha=0.3)
plt.xscale("log")
```


![png](/assets/img/post_img/2018-12-28-public_data_eda_files/2018-12-28-public_data_eda_69_0.png)


## year_month


```python
data=df.groupby(["year_month"])[["price"]].mean().sort_values(by=['year_month'], axis=0, ascending=True)
```


```python
plt.figure(1, figsize=(10, 6))
plt.title("계약년월 별 주택 평균 거래 금액(단위:만원)",size=20)
plt.xlabel("계약년월", size=16)
plt.ylabel("평균거래금액(만원)", size=16)
plt.bar(data.index,data["price"],facecolor="blue", alpha=0.5)
plt.xticks(rotation=45)
plt.grid(True)
```


![png](/assets/img/post_img/2018-12-28-public_data_eda_files/2018-12-28-public_data_eda_72_0.png)



```python
data.head()
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
      <th>price</th>
    </tr>
    <tr>
      <th>year_month</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>201512</th>
      <td>36976.200000</td>
    </tr>
    <tr>
      <th>201601</th>
      <td>35814.201696</td>
    </tr>
    <tr>
      <th>201602</th>
      <td>37197.702386</td>
    </tr>
    <tr>
      <th>201603</th>
      <td>36187.884465</td>
    </tr>
    <tr>
      <th>201604</th>
      <td>37489.123676</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python

```


```python

```
