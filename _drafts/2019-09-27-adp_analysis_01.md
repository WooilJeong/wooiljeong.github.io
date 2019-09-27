---
title: "ADP필기 R을 활용한 데이터 분석 01"
date: 2019-09-27 00:00:00 -0000
categories: R
tags: R ADP
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

> ## 제1장 : R 기초와 데이터 마트

먼저 데이터 분석을 수행할 환경의 경로를 설정합니다.


```R
setwd('C:/Users/wooil/Documents/GitHub/ADP')
getwd()
```


'C:/Users/wooil/Documents/GitHub/ADP'


## 제1절 R 기초

**R의 데이터 구조**  

1) 벡터


```R
x = c(1, 10, 24, 40)                # 숫자형 벡터
y = c("사과", "바나나", "오렌지")   # 문자형 벡터
z = c(TRUE, FALSE, TRUE)            # 논리 연산자 벡터

x <- c(1, 10, 24, 40)                # 숫자형 벡터
y <- c("사과", "바나나", "오렌지")   # 문자형 벡터
z <- c(TRUE, FALSE, TRUE)            # 논리 연산자 벡터

# (참고) '=', '<-' : 우측 값을 좌측 변수에 할당

x
y
z
```


<ol class=list-inline>
	<li>1</li>
	<li>10</li>
	<li>24</li>
	<li>40</li>
</ol>




<ol class=list-inline>
	<li>'사과'</li>
	<li>'바나나'</li>
	<li>'오렌지'</li>
</ol>




<ol class=list-inline>
	<li>TRUE</li>
	<li>FALSE</li>
	<li>TRUE</li>
</ol>




```R
xy = c(x,y)        # 벡터와 벡터 결합하여 새로운 벡터 생성

xy
```


<ol class=list-inline>
	<li>'1'</li>
	<li>'10'</li>
	<li>'24'</li>
	<li>'40'</li>
	<li>'사과'</li>
	<li>'바나나'</li>
	<li>'오렌지'</li>
</ol>



2) 행렬


```R
mx = matrix(c(1,2,3,4,5,6), ncol=2)                 # 컬럼이 3개인 행렬(디폴트 열기준)
mx2 = matrix(c(1,2,3,4,5,6), ncol=2, byrow=TRUE)    # 컬럼이 3개인 행렬(행기준)

mx
mx2
```


<table>
<tbody>
	<tr><td>1</td><td>4</td></tr>
	<tr><td>2</td><td>5</td></tr>
	<tr><td>3</td><td>6</td></tr>
</tbody>
</table>




<table>
<tbody>
	<tr><td>1</td><td>2</td></tr>
	<tr><td>3</td><td>4</td></tr>
	<tr><td>5</td><td>6</td></tr>
</tbody>
</table>



rbind : 행 추가, cbind : 열 추가


```R
r1 = c(10, 10)
c1 = c(20, 20, 20)

rbind(mx, r1)
cbind(mx2, c1)
```


<table>
<tbody>
	<tr><th scope=row></th><td> 1</td><td> 4</td></tr>
	<tr><th scope=row></th><td> 2</td><td> 5</td></tr>
	<tr><th scope=row></th><td> 3</td><td> 6</td></tr>
	<tr><th scope=row> r1</th><td> 10</td><td> 10</td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col></th><th scope=col></th><th scope=col>c1</th></tr></thead>
<tbody>
	<tr><td>1 </td><td>2 </td><td>20</td></tr>
	<tr><td>3 </td><td>4 </td><td>20</td></tr>
	<tr><td>5 </td><td>6 </td><td>20</td></tr>
</tbody>
</table>



3) 데이터프레임


```R
income = c(100, 200, 150, 300, 900)
car = c("kia", "hyundai", "kia", "toyota", "lexus")
marriage = c(FALSE, FALSE, FALSE, TRUE, TRUE)

df = data.frame(income, car, marriage)
df
```


<table>
<thead><tr><th scope=col>income</th><th scope=col>car</th><th scope=col>marriage</th></tr></thead>
<tbody>
	<tr><td>100    </td><td>kia    </td><td>FALSE  </td></tr>
	<tr><td>200    </td><td>hyundai</td><td>FALSE  </td></tr>
	<tr><td>150    </td><td>kia    </td><td>FALSE  </td></tr>
	<tr><td>300    </td><td>toyota </td><td> TRUE  </td></tr>
	<tr><td>900    </td><td>lexus  </td><td> TRUE  </td></tr>
</tbody>
</table>



**외부 데이터 불러오기**

1) CSV파일 불러오기

```dataset.csv``` : 엑셀 등으로 임의로 만든 데이터 파일


```R
data1 <- read.table("dataset.csv", header = TRUE, sep = ",")
data2 <- read.csv("dataset.csv", header = TRUE)
```


```R
data1
```


<table>
<thead><tr><th scope=col>no</th><th scope=col>income</th></tr></thead>
<tbody>
	<tr><td> 1      </td><td>50000000</td></tr>
	<tr><td> 2      </td><td>45000000</td></tr>
	<tr><td> 3      </td><td>40000000</td></tr>
	<tr><td> 4      </td><td>35000000</td></tr>
	<tr><td> 5      </td><td>30000000</td></tr>
	<tr><td> 6      </td><td>25000000</td></tr>
	<tr><td> 7      </td><td>20000000</td></tr>
	<tr><td> 8      </td><td>15000000</td></tr>
	<tr><td> 9      </td><td>10000000</td></tr>
	<tr><td>10      </td><td> 5000000</td></tr>
</tbody>
</table>




```R
data2
```


<table>
<thead><tr><th scope=col>no</th><th scope=col>income</th></tr></thead>
<tbody>
	<tr><td> 1      </td><td>50000000</td></tr>
	<tr><td> 2      </td><td>45000000</td></tr>
	<tr><td> 3      </td><td>40000000</td></tr>
	<tr><td> 4      </td><td>35000000</td></tr>
	<tr><td> 5      </td><td>30000000</td></tr>
	<tr><td> 6      </td><td>25000000</td></tr>
	<tr><td> 7      </td><td>20000000</td></tr>
	<tr><td> 8      </td><td>15000000</td></tr>
	<tr><td> 9      </td><td>10000000</td></tr>
	<tr><td>10      </td><td> 5000000</td></tr>
</tbody>
</table>



**R의 기초 함수**

1) 수열 생성하기


```R
# 1을 3회 반복
rep(1, 3)

# 1에서 3까지 연속된 수
seq(1, 3)

# 1에서 3까지 연속된 수
1:3

# 1에서 11까지 공차가 2인 등차수열
seq(1, 11, by=2)

# 1에서 11까지 항이 6개인 등차수열
seq(1, 11, length=6)

# 1에서 11까지 항이 8개인 등차수열
seq(1, 11, length=8)

# 2에서 5까지 연속된 수를 각 3회 반복
rep(2:5, 3)
```


<ol class=list-inline>
	<li>1</li>
	<li>1</li>
	<li>1</li>
</ol>




<ol class=list-inline>
	<li>1</li>
	<li>2</li>
	<li>3</li>
</ol>




<ol class=list-inline>
	<li>1</li>
	<li>2</li>
	<li>3</li>
</ol>




<ol class=list-inline>
	<li>1</li>
	<li>3</li>
	<li>5</li>
	<li>7</li>
	<li>9</li>
	<li>11</li>
</ol>




<ol class=list-inline>
	<li>1</li>
	<li>3</li>
	<li>5</li>
	<li>7</li>
	<li>9</li>
	<li>11</li>
</ol>




<ol class=list-inline>
	<li>1</li>
	<li>2.42857142857143</li>
	<li>3.85714285714286</li>
	<li>5.28571428571429</li>
	<li>6.71428571428571</li>
	<li>8.14285714285714</li>
	<li>9.57142857142857</li>
	<li>11</li>
</ol>




<ol class=list-inline>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
</ol>



2) 기초적인 수치 계산


```R
a = 1:10
a
a + a
a - a
a * a
a / a
```


<ol class=list-inline>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
	<li>7</li>
	<li>8</li>
	<li>9</li>
	<li>10</li>
</ol>




<ol class=list-inline>
	<li>2</li>
	<li>4</li>
	<li>6</li>
	<li>8</li>
	<li>10</li>
	<li>12</li>
	<li>14</li>
	<li>16</li>
	<li>18</li>
	<li>20</li>
</ol>




<ol class=list-inline>
	<li>0</li>
	<li>0</li>
	<li>0</li>
	<li>0</li>
	<li>0</li>
	<li>0</li>
	<li>0</li>
	<li>0</li>
	<li>0</li>
	<li>0</li>
</ol>




<ol class=list-inline>
	<li>1</li>
	<li>4</li>
	<li>9</li>
	<li>16</li>
	<li>25</li>
	<li>36</li>
	<li>49</li>
	<li>64</li>
	<li>81</li>
	<li>100</li>
</ol>




<ol class=list-inline>
	<li>1</li>
	<li>1</li>
	<li>1</li>
	<li>1</li>
	<li>1</li>
	<li>1</li>
	<li>1</li>
	<li>1</li>
	<li>1</li>
	<li>1</li>
</ol>




```R
a = c(2,7,3)    # 열벡터
b = t(a)        # 행벡터

a
b
```


<ol class=list-inline>
	<li>2</li>
	<li>7</li>
	<li>3</li>
</ol>




<table>
<tbody>
	<tr><td>2</td><td>7</td><td>3</td></tr>
</tbody>
</table>




```R
a * b           # 스칼라 곱
a %*% b         # 행렬 곱
```


<table>
<tbody>
	<tr><td>4 </td><td>49</td><td>9 </td></tr>
</tbody>
</table>




<table>
<tbody>
	<tr><td> 4</td><td>14</td><td> 6</td></tr>
	<tr><td>14</td><td>49</td><td>21</td></tr>
	<tr><td> 6</td><td>21</td><td> 9</td></tr>
</tbody>
</table>




```R
mx = matrix(c(1,2,3,4), ncol=2)
mx_inv = solve(mx)              # mx의 역행렬

mx
mx_inv
```


<table>
<tbody>
	<tr><td>1</td><td>3</td></tr>
	<tr><td>2</td><td>4</td></tr>
</tbody>
</table>




<table>
<tbody>
	<tr><td>-2  </td><td> 1.5</td></tr>
	<tr><td> 1  </td><td>-0.5</td></tr>
</tbody>
</table>




```R
mx %*% mx_inv                   # 결과 : 단위행렬
```


<table>
<tbody>
	<tr><td>1</td><td>0</td></tr>
	<tr><td>0</td><td>1</td></tr>
</tbody>
</table>




```R
a = 1:10

# a의 평균
mean(a)

# a의 분산
var(a)

# a의 표준편차
sd(a)

# a의 합
sum(a)

# a의 중앙값
median(a)

# a의 로그값
log(a)

# a와 a의 공분산
cov(a, a)

# a와 log(a)의 공분산
cov(a, log(a))

# a와 a의 상관계수
cor(a, a)

# a와 log(a)의 상관계수
cor(a, log(a))
```


5.5



9.16666666666667



3.02765035409749



55



5.5



<ol class=list-inline>
	<li>0</li>
	<li>0.693147180559945</li>
	<li>1.09861228866811</li>
	<li>1.38629436111989</li>
	<li>1.6094379124341</li>
	<li>1.79175946922805</li>
	<li>1.94591014905531</li>
	<li>2.07944154167984</li>
	<li>2.19722457733622</li>
	<li>2.30258509299405</li>
</ol>




9.16666666666667



2.11206237777995



1



0.951662390427133



```R
# a의 요약 통계량 (최솟값/1사분위수/중앙값/평균값/3사분위수/최댓값)
summary(a)
```


       Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
       1.00    3.25    5.50    5.50    7.75   10.00


**R 데이터 핸들링**

1) 벡터형 변수


```R
b = c("a", "b", "c", "d", "e")
b[1]
b[-2]
b[c(-2,-3)]
b[c(2,3)]
```


'a'



<ol class=list-inline>
	<li>'a'</li>
	<li>'c'</li>
	<li>'d'</li>
	<li>'e'</li>
</ol>




<ol class=list-inline>
	<li>'a'</li>
	<li>'d'</li>
	<li>'e'</li>
</ol>




<ol class=list-inline>
	<li>'b'</li>
	<li>'c'</li>
</ol>



2) 행렬/데이터 프레임 형태의 변수


```R
income = c(100, 200, 150, 300, 900)
car = c("kia", "hyundai", "kia", "toyota", "lexus")
marriage = c(FALSE, FALSE, FALSE, TRUE, TRUE)

df = data.frame(income, car, marriage)
df
```


<table>
<thead><tr><th scope=col>income</th><th scope=col>car</th><th scope=col>marriage</th></tr></thead>
<tbody>
	<tr><td>100    </td><td>kia    </td><td>FALSE  </td></tr>
	<tr><td>200    </td><td>hyundai</td><td>FALSE  </td></tr>
	<tr><td>150    </td><td>kia    </td><td>FALSE  </td></tr>
	<tr><td>300    </td><td>toyota </td><td> TRUE  </td></tr>
	<tr><td>900    </td><td>lexus  </td><td> TRUE  </td></tr>
</tbody>
</table>




```R
df[2,2]
df[-1]
df[-2]
df[-3]
df[-1,-1]
df[2,]
df[,2]
```


hyundai
<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'hyundai'</li>
		<li>'kia'</li>
		<li>'lexus'</li>
		<li>'toyota'</li>
	</ol>
</details>



<table>
<thead><tr><th scope=col>car</th><th scope=col>marriage</th></tr></thead>
<tbody>
	<tr><td>kia    </td><td>FALSE  </td></tr>
	<tr><td>hyundai</td><td>FALSE  </td></tr>
	<tr><td>kia    </td><td>FALSE  </td></tr>
	<tr><td>toyota </td><td> TRUE  </td></tr>
	<tr><td>lexus  </td><td> TRUE  </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>income</th><th scope=col>marriage</th></tr></thead>
<tbody>
	<tr><td>100  </td><td>FALSE</td></tr>
	<tr><td>200  </td><td>FALSE</td></tr>
	<tr><td>150  </td><td>FALSE</td></tr>
	<tr><td>300  </td><td> TRUE</td></tr>
	<tr><td>900  </td><td> TRUE</td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>income</th><th scope=col>car</th></tr></thead>
<tbody>
	<tr><td>100    </td><td>kia    </td></tr>
	<tr><td>200    </td><td>hyundai</td></tr>
	<tr><td>150    </td><td>kia    </td></tr>
	<tr><td>300    </td><td>toyota </td></tr>
	<tr><td>900    </td><td>lexus  </td></tr>
</tbody>
</table>




<table>
<thead><tr><th></th><th scope=col>car</th><th scope=col>marriage</th></tr></thead>
<tbody>
	<tr><th scope=row>2</th><td>hyundai</td><td>FALSE  </td></tr>
	<tr><th scope=row>3</th><td>kia    </td><td>FALSE  </td></tr>
	<tr><th scope=row>4</th><td>toyota </td><td> TRUE  </td></tr>
	<tr><th scope=row>5</th><td>lexus  </td><td> TRUE  </td></tr>
</tbody>
</table>




<table>
<thead><tr><th></th><th scope=col>income</th><th scope=col>car</th><th scope=col>marriage</th></tr></thead>
<tbody>
	<tr><th scope=row>2</th><td>200    </td><td>hyundai</td><td>FALSE  </td></tr>
</tbody>
</table>




<ol class=list-inline>
	<li>kia</li>
	<li>hyundai</li>
	<li>kia</li>
	<li>toyota</li>
	<li>lexus</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'hyundai'</li>
		<li>'kia'</li>
		<li>'lexus'</li>
		<li>'toyota'</li>
	</ol>
</details>


**반복 구문과 조건문**

1) for 반복 구문


```R
a = c()
for (i in 1:9){
  a[i] = i * i
}

a
```


<ol class=list-inline>
	<li>1</li>
	<li>4</li>
	<li>9</li>
	<li>16</li>
	<li>25</li>
	<li>36</li>
	<li>49</li>
	<li>64</li>
	<li>81</li>
</ol>




```R
isum = 0
for (i in 1:100){
  isum = isum + i
}
cat("1부터 100까지의 합 =", isum, "입니다.", "\n")
```

    1부터 100까지의 합 = 5050 입니다.


2) while 반복 구문


```R
x=0
while (x<5){
  x=x+1
  print(x)
}
```

    [1] 1
    [1] 2
    [1] 3
    [1] 4
    [1] 5


3) if~else 조건문


```R
StatScore = c(88, 90, 87, 92, 75, 65, 93, 54, 85, 90)
over90 = rep(0, length(StatScore))

for (i in 1:length(StatScore)){

  if (StatScore[i] >= 90) over90[i] = 1
  else over90[i] = 0

}

over90
cat("90점을 넘은 사람의 수는 ", sum(over90), "명 입니다.", sep="")
```


<ol class=list-inline>
	<li>0</li>
	<li>1</li>
	<li>0</li>
	<li>1</li>
	<li>0</li>
	<li>0</li>
	<li>1</li>
	<li>0</li>
	<li>0</li>
	<li>1</li>
</ol>



    90점을 넘은 사람의 수는 4명 입니다.

**사용자 정의 함수**


```R
addto = function (a) {

  isum=0
  for (i in 1:a){
    isum = isum + i
  }
  print(isum)
}

addto(10)
addto(100)
```

    [1] 55
    [1] 5050


**기타 유용한 기능들**

1) paste


```R
number = 1:10
alphabet = c("a", "b", "c")

paste(number, alphabet)
paste(number, alphabet, sep=" to the ")
```


<ol class=list-inline>
	<li>'1 a'</li>
	<li>'2 b'</li>
	<li>'3 c'</li>
	<li>'4 a'</li>
	<li>'5 b'</li>
	<li>'6 c'</li>
	<li>'7 a'</li>
	<li>'8 b'</li>
	<li>'9 c'</li>
	<li>'10 a'</li>
</ol>




<ol class=list-inline>
	<li>'1 to the a'</li>
	<li>'2 to the b'</li>
	<li>'3 to the c'</li>
	<li>'4 to the a'</li>
	<li>'5 to the b'</li>
	<li>'6 to the c'</li>
	<li>'7 to the a'</li>
	<li>'8 to the b'</li>
	<li>'9 to the c'</li>
	<li>'10 to the a'</li>
</ol>



2) substr


```R
substr("BigDataAnalysis", 1, 4)

country = c("Korea", "Japan", "China", "Singapore", "Russia")
substr(country, 1, 3)
```


'BigD'



<ol class=list-inline>
	<li>'Kor'</li>
	<li>'Jap'</li>
	<li>'Chi'</li>
	<li>'Sin'</li>
	<li>'Rus'</li>
</ol>



3) 자료형 데이터 구조 변환


```R
x = matrix(c(1,2,3,4,5,6), ncol=2)

as.data.frame(x)  # 데이터프레임
as.list(x)        # 리스트
as.matrix(x)      # 행렬
as.vector(x)      # 벡터
as.factor(x)      # 팩터
```


<table>
<thead><tr><th scope=col>V1</th><th scope=col>V2</th></tr></thead>
<tbody>
	<tr><td>1</td><td>4</td></tr>
	<tr><td>2</td><td>5</td></tr>
	<tr><td>3</td><td>6</td></tr>
</tbody>
</table>




<ol>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
</ol>




<table>
<tbody>
	<tr><td>1</td><td>4</td></tr>
	<tr><td>2</td><td>5</td></tr>
	<tr><td>3</td><td>6</td></tr>
</tbody>
</table>




<ol class=list-inline>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
</ol>




<ol class=list-inline>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'1'</li>
		<li>'2'</li>
		<li>'3'</li>
		<li>'4'</li>
		<li>'5'</li>
		<li>'6'</li>
	</ol>
</details>



```R
as.integer(3.14)
as.numeric('foo')
as.character(101)
as.numeric(FALSE)
as.logical(0.35)
as.logical(0)
```


3


    Warning message in eval(expr, envir, enclos):
    "강제형변환에 의해 생성된 NA 입니다"


&lt;NA&gt;



'101'



0



TRUE



FALSE



```R
income = c(100, 200, 150, 300, 900)
car = c("kia", "hyundai", "kia", "toyota", "lexus")
marriage = c(FALSE, FALSE, FALSE, TRUE, TRUE)

df = data.frame(income, car, marriage)

df              # income 숫자형 벡터, car 문자형 벡터, marriage 논리형 벡터
as.matrix(df)   # matrix 변환 후 모든 컬럼 문자형 벡터화
```


<table>
<thead><tr><th scope=col>income</th><th scope=col>car</th><th scope=col>marriage</th></tr></thead>
<tbody>
	<tr><td>100    </td><td>kia    </td><td>FALSE  </td></tr>
	<tr><td>200    </td><td>hyundai</td><td>FALSE  </td></tr>
	<tr><td>150    </td><td>kia    </td><td>FALSE  </td></tr>
	<tr><td>300    </td><td>toyota </td><td> TRUE  </td></tr>
	<tr><td>900    </td><td>lexus  </td><td> TRUE  </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>income</th><th scope=col>car</th><th scope=col>marriage</th></tr></thead>
<tbody>
	<tr><td>100    </td><td>kia    </td><td>FALSE  </td></tr>
	<tr><td>200    </td><td>hyundai</td><td>FALSE  </td></tr>
	<tr><td>150    </td><td>kia    </td><td>FALSE  </td></tr>
	<tr><td>300    </td><td>toyota </td><td>TRUE   </td></tr>
	<tr><td>900    </td><td>lexus  </td><td>TRUE   </td></tr>
</tbody>
</table>



4) 문자열을 날짜로 변환


```R
Sys.Date()     # 오늘 날짜
a = '2019-07-08'
b = '07/08/2019'
as.Date(a)
as.Date(b, format = "%m/%d/%Y")
```


<time datetime="2019-09-27">2019-09-27</time>



<time datetime="2019-07-08">2019-07-08</time>



<time datetime="2019-07-08">2019-07-08</time>


5) 날짜를 문자열로 변환


```R
format(Sys.Date())
as.character(Sys.Date())
format(Sys.Date(), format="%m/%d/%Y")
```


'2019-09-27'



'2019-09-27'



'09/27/2019'


**format 함수로 날자 정보 추출**


```R
format(Sys.Date(), '%a')             # 요일
format(Sys.Date(), '%b')             # 월
format(Sys.Date(), '%m')             # 두자리 숫자로 월
format(Sys.Date(), '%d')             # 두자리 숫자로 일
format(Sys.Date(), '%y')             # 두자리 숫자로 연도
format(Sys.Date(), '%Y')             # 네자리 숫자로 연도
```


'금'



'9'



'09'



'27'



'19'



'2019'


**R 그래픽 기능**


```R
df = iris            # iris 샘플 데이터 (Sepal:꽃받침, Petal:꽃잎)
```

1) 산점도 그래프(Scatter Plot)


```R
plot(df$Sepal.Length, df$Sepal.Width)  # 꽃받침 길이와 꽃받침 너비 산점도
plot(df$Sepal.Length, df$Petal.Length) # 꽃받침 길이와 꽃잎 길이 산점도
```


![png](/assets/img/post_img/2019-09-27-adp_analysis_01/output_58_0.png)




![png](/assets/img/post_img/2019-09-27-adp_analysis_01/output_58_1.png)


2) 산점도 행렬(Scatter Plot Matrix)


```R
pairs(df[1:4],
      main = "Anderson's Iris Data -- 3 species",
      pch = 21,
      bg = c("red", "green3", "blue")[unclass(df$Species)])
```


![png](/assets/img/post_img/2019-09-27-adp_analysis_01/output_60_0.png)


3) 히스토그램과 상자그림(Histogram and Box-Plot)


```R
StatScore = c(88, 90, 87, 92, 75, 65, 93, 54, 85, 90)
hist(StatScore, prob=T)
boxplot(StatScore)
```


![png](/assets/img/post_img/2019-09-27-adp_analysis_01/output_62_0.png)



![png](/assets/img/post_img/2019-09-27-adp_analysis_01/output_62_1.png)


## Reference

[데이터 분석 전문가 가이드](http://www.yes24.com/Product/Goods/29430751)
