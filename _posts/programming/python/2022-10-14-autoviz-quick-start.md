---
title: Python AutoViz - 데이터 자동으로 시각화하기
categories: python
tags: autoviz
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

AutoViz를 이용하면 데이터 전처리 및 시각화를 자동화할 수 있다. 


## AutoViz

0.1.50 이후 버전의 AutoViz를 사용하면 자동으로 데이터를 분석해주고, 데이터 정제 방법도 제안해준다. 누락값, 적은 수의 카테고리, 무한한 값 그리고 혼합 데이터 유형 등을 찾아주는 작업을 수행한다. 이러한 기능을 활용하면 데이터 정제 작업에 소요되는 시간을 줄일 수 있다. 

<br>

## AutoViz 설치

다음과 같이 Shell 명령을 통해 최신 버전의 AutoViz를 설치한다.

```bash
pip install autoviz --upgrade
```

<br>

## 예제 데이터

유명한 아이리스 데이터를 판다스 데이터프레임으로 불러온다.

```python
import pandas as pd

# 예제 데이터 URL
url = 'https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data'

# 예제 데이터를 데이터프레임으로 읽기
df = pd.read_csv(url, names=['sepal length','sepal width','petal length','petal width','target'])
```

<br>

## AutoViz 임포트하기

이제 AutoViz 라이브러리를 임포트한다. `AutoViz_Class` 를 이용해 AutoViz 인스턴스를 생성한다.

```python
from autoviz.AutoViz_Class import AutoViz_Class

# AutoViz 인스턴스 생성
AV = AutoViz_Class()
```

<br>

## 시각화 실행하기

`AV.AutoViz()` 이용 시 아래와 같이 옵션을 지정하면 다양한 형태로 시각화 결과를 출력할 수 있다.

- `chart_format='bokeh`: bokeh 대시보드를 주피터 노트북에서 실행
- `chart_format='server`: 웹 브라우저에서 실행
- `chart_format='html`: 로컬 경로에 동적 HTML 파일 저장
- `chart_format='png`, `chart_format='svg`, `chart_format='jpg`: Matplotlib 차트 인라인 실행
    - `verbose=0`: 주피터 노트북에 표시 (최소 정보)
    - `verbose=1`: 주피터 노트북에 표시
    - `verbose=2`: 로컬에 저장


```python
%matplotlib inline

# 시각화 결과 저장 경로
save_plot_dir = "./result"

# 자동 시각화 실행
dft = AV.AutoViz(
    filename="",
    sep=",",
    depVar="target",
    dfte=df,
    header=0,
    verbose=2,
    lowess=False,
    chart_format="png",
    max_rows_analyzed=150000,
    max_cols_analyzed=30,
    save_plot_dir=save_plot_dir
)
```

    Shape of your Data Set loaded: (150, 5)
    #######################################################################################
    ######################## C L A S S I F Y I N G  V A R I A B L E S  ####################
    #######################################################################################
    Classifying variables in data set...
    Data cleaning improvement suggestions. Complete them before proceeding to ML modeling.
    


<style type="text/css">
#T_8a9f4_row0_col0, #T_8a9f4_row0_col4 {
  background-color: #67000d;
  color: #f1f1f1;
  font-family: Segoe UI;
}
#T_8a9f4_row0_col1, #T_8a9f4_row0_col6, #T_8a9f4_row1_col1, #T_8a9f4_row1_col6, #T_8a9f4_row2_col1, #T_8a9f4_row2_col6, #T_8a9f4_row3_col1, #T_8a9f4_row3_col6 {
  font-family: Segoe UI;
}
#T_8a9f4_row0_col2, #T_8a9f4_row0_col3, #T_8a9f4_row0_col5, #T_8a9f4_row1_col2, #T_8a9f4_row1_col3, #T_8a9f4_row1_col5, #T_8a9f4_row2_col2, #T_8a9f4_row2_col3, #T_8a9f4_row2_col5, #T_8a9f4_row3_col0, #T_8a9f4_row3_col2, #T_8a9f4_row3_col3, #T_8a9f4_row3_col4, #T_8a9f4_row3_col5 {
  background-color: #fff5f0;
  color: #000000;
  font-family: Segoe UI;
}
#T_8a9f4_row1_col0, #T_8a9f4_row1_col4 {
  background-color: #f03d2d;
  color: #f1f1f1;
  font-family: Segoe UI;
}
#T_8a9f4_row2_col0, #T_8a9f4_row2_col4 {
  background-color: #ffede5;
  color: #000000;
  font-family: Segoe UI;
}
</style>
<table id="T_8a9f4">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_8a9f4_level0_col0" class="col_heading level0 col0" >Nuniques</th>
      <th id="T_8a9f4_level0_col1" class="col_heading level0 col1" >dtype</th>
      <th id="T_8a9f4_level0_col2" class="col_heading level0 col2" >Nulls</th>
      <th id="T_8a9f4_level0_col3" class="col_heading level0 col3" >Nullpercent</th>
      <th id="T_8a9f4_level0_col4" class="col_heading level0 col4" >NuniquePercent</th>
      <th id="T_8a9f4_level0_col5" class="col_heading level0 col5" >Value counts Min</th>
      <th id="T_8a9f4_level0_col6" class="col_heading level0 col6" >Data cleaning improvement suggestions</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_8a9f4_level0_row0" class="row_heading level0 row0" >petal length</th>
      <td id="T_8a9f4_row0_col0" class="data row0 col0" >43</td>
      <td id="T_8a9f4_row0_col1" class="data row0 col1" >float64</td>
      <td id="T_8a9f4_row0_col2" class="data row0 col2" >0</td>
      <td id="T_8a9f4_row0_col3" class="data row0 col3" >0.000000</td>
      <td id="T_8a9f4_row0_col4" class="data row0 col4" >28.666667</td>
      <td id="T_8a9f4_row0_col5" class="data row0 col5" >0</td>
      <td id="T_8a9f4_row0_col6" class="data row0 col6" ></td>
    </tr>
    <tr>
      <th id="T_8a9f4_level0_row1" class="row_heading level0 row1" >sepal length</th>
      <td id="T_8a9f4_row1_col0" class="data row1 col0" >35</td>
      <td id="T_8a9f4_row1_col1" class="data row1 col1" >float64</td>
      <td id="T_8a9f4_row1_col2" class="data row1 col2" >0</td>
      <td id="T_8a9f4_row1_col3" class="data row1 col3" >0.000000</td>
      <td id="T_8a9f4_row1_col4" class="data row1 col4" >23.333333</td>
      <td id="T_8a9f4_row1_col5" class="data row1 col5" >0</td>
      <td id="T_8a9f4_row1_col6" class="data row1 col6" ></td>
    </tr>
    <tr>
      <th id="T_8a9f4_level0_row2" class="row_heading level0 row2" >sepal width</th>
      <td id="T_8a9f4_row2_col0" class="data row2 col0" >23</td>
      <td id="T_8a9f4_row2_col1" class="data row2 col1" >float64</td>
      <td id="T_8a9f4_row2_col2" class="data row2 col2" >0</td>
      <td id="T_8a9f4_row2_col3" class="data row2 col3" >0.000000</td>
      <td id="T_8a9f4_row2_col4" class="data row2 col4" >15.333333</td>
      <td id="T_8a9f4_row2_col5" class="data row2 col5" >0</td>
      <td id="T_8a9f4_row2_col6" class="data row2 col6" ></td>
    </tr>
    <tr>
      <th id="T_8a9f4_level0_row3" class="row_heading level0 row3" >petal width</th>
      <td id="T_8a9f4_row3_col0" class="data row3 col0" >22</td>
      <td id="T_8a9f4_row3_col1" class="data row3 col1" >float64</td>
      <td id="T_8a9f4_row3_col2" class="data row3 col2" >0</td>
      <td id="T_8a9f4_row3_col3" class="data row3 col3" >0.000000</td>
      <td id="T_8a9f4_row3_col4" class="data row3 col4" >14.666667</td>
      <td id="T_8a9f4_row3_col5" class="data row3 col5" >0</td>
      <td id="T_8a9f4_row3_col6" class="data row3 col6" ></td>
    </tr>
  </tbody>
</table>



      Printing upto 30 columns max in each category:
        Numeric Columns : ['sepal length', 'sepal width', 'petal length', 'petal width']
        Integer-Categorical Columns: []
        String-Categorical Columns: []
        Factor-Categorical Columns: []
        String-Boolean Columns: []
        Numeric-Boolean Columns: []
        Discrete String Columns: []
        NLP text Columns: []
        Date Time Columns: []
        ID Columns: []
        Columns that will not be considered in modeling: []
        4 Predictors classified...
            No variables removed since no ID or low-information variables found in data set
    
    ################ Multi_Classification problem #####################
       Columns to delete:
    '   []'
       Boolean variables %s 
    '   []'
       Categorical variables %s 
    '   []'
       Continuous variables %s 
    "   ['sepal length', 'sepal width', 'petal length', 'petal width']"
       Discrete string variables %s 
    '   []'
       Date and time variables %s 
    '   []'
       ID variables %s 
    '   []'
       Target variable %s 
    '   target'
    Total Number of Scatter Plots = 10
    No categorical or boolean vars in data set. Hence no pivot plots...
    No categorical or numeric vars in data set. Hence no bar charts.
    All Plots are saved in ./result\target
    Time to run AutoViz = 2 seconds 
    


    
![PNG](/assets/img/post_img/2022-10-14-autoviz-quick-start/1.png){: .align-center}

![PNG](/assets/img/post_img/2022-10-14-autoviz-quick-start/2.png){: .align-center}

![PNG](/assets/img/post_img/2022-10-14-autoviz-quick-start/3.png){: .align-center}

![PNG](/assets/img/post_img/2022-10-14-autoviz-quick-start/4.png){: .align-center}

![PNG](/assets/img/post_img/2022-10-14-autoviz-quick-start/5.png){: .align-center}

![PNG](/assets/img/post_img/2022-10-14-autoviz-quick-start/6.png){: .align-center}

<br>

## 참고

- [AutoViz GitHub Repository](https://github.com/AutoViML/AutoViz)