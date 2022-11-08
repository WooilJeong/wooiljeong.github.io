---
title: "퀀트용어 - Benchmark(벤치마크)와 Alpha(알파)"
categories: quant
tags: quant
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## Benchmark, Alpha

## Benchmark

벤치마크는 퀀트 전략을 이용해 만든 모델의 수익률이 좋은지 나쁜지를 비교할 기준이라고 보면 된다. 2001년 ~ 2021년 동안 코스피 200에 투자하는 것을 벤치마크로 설정해보자.


```python
import matplotlib.pyplot as plt
import FinanceDataReader as fdr
import pandas as pd

# 코스피200
df = fdr.DataReader("KS200", "2010", "2022")

plt.figure(figsize=(10,6))
plt.title("코스피 200 (2010~2021년)", size=20, weight='bold')
plt.plot(df['Close'], color='tomato', label='KOSPI 200')
plt.legend()
plt.xlabel("년도", size=20)
plt.ylabel("가격", size=20)
plt.show()
```
  

![PNG](/assets/img/post_img/2022-08-02-benchmark-alpha/1.png){: .align-center}

<br>

벤치마크의 CAGR은 4.84%이고, MDD는 41.19%이다. 


```python
def CAGR(df, n):
    total_profit = df['Close'][-1] / df['Close'][0]
    cagr = (total_profit ** (1 / n)) - 1
    return cagr

def MDD(df):
    price = np.array(df['Close'])
    peak_lower = np.argmax(np.maximum.accumulate(price) - price)
    peak_upper = np.argmax(price[:peak_lower])
    mdd = ((price[peak_upper] - price[peak_lower]) / price[peak_upper])
    return peak_lower, peak_upper, mdd
```


```python
# CAGR 계산
cagr = CAGR(df, 12)

# MDD 계산
peak_lower, peak_upper, mdd = MDD(df)

print(f"""
CAGR: {cagr*100:.2f}%
MDD: {mdd*100:.2f}%
""")
```

    
    CAGR: 4.84%
    MDD: 41.19%
    
    
<br>

다시 차트를 살펴보면, 17년 11월 ~ 20년 3월 까지 최대 낙폭 즉, MDD가 41.19% 인 것을 확인할 수 있다.


```python
plt.figure(figsize=(10,6))
plt.title(f"코스피 200 (2010~2021년) - CAGR: {cagr*100:.2f}%, MDD: {mdd*100:.2f}%", size=20, weight='bold')

plt.plot(df['Close'], color='tomato', label='KOSPI 200')
plt.axvspan(df.index[peak_lower], df.index[peak_upper], facecolor='tomato', alpha=0.3)
plt.annotate('',
             xy = (df.index[peak_lower], df['Close'][peak_lower]), 
             xytext = (df.index[peak_upper], df['Close'][peak_upper]), 
             arrowprops = dict(facecolor="grey"))

plt.legend()
plt.xlabel("년도", size=20)
plt.ylabel("가격", size=20)
plt.show()
```


    
![PNG](/assets/img/post_img/2022-08-02-benchmark-alpha/2.png){: .align-center}

<br>

## Alpha, α

알파는 벤치마크보다 높은 초과수익을 의미한다. 예를 들어, 벤치마크의 수익률이 9%인데, 퀀트 전략을 이용해 만든 모델의 수익율이 12%였다면, 3%의 알파를 창출했다고 볼 수 있다. 위에서 정한 벤치마크인 코스피 200의 수익율과 미국 주식, 장기 국채, 단기 국채 그리고 금에 동일 비중으로 투자한 포트폴리오의 수익율을 비교해보자. 투자 기간은 2010~2021년이라고 가정한다.


```python
import FinanceDataReader as fdr

# 투자기간(년)
n = 12

# 코스피200
ks200 = fdr.DataReader("KS200", "2010", "2022")
ks200['Close'] = ks200['Close'] / ks200['Close'][0]
ks200_cagr = CAGR(ks200, n)
ks200_low_idx, ks200_upper_idx, ks200_mdd = MDD(ks200)

# 미국 전체 주식 - Vanguard Total Stock Market
VTI = fdr.DataReader("VTI", "2010", "2022")
# 만기 20년 이상 미국 장기 국채 - iShares 20+ Year Treasury Bond
TLT = fdr.DataReader("VTI", "2010", "2022")
# 미국 초단기 채권 - SPDR Blmbg Barclays 1-3 Mth T-Bill
BIL = fdr.DataReader("VTI", "2010", "2022")
# 금 현물 - SPDR Gold Trust
GLD = fdr.DataReader("VTI", "2010", "2022")

# 포트폴리오
port = pd.DataFrame(VTI['Close'] + TLT['Close'] + BIL['Close'] + GLD['Close'])
port['Close'] = port['Close'] / port['Close'][0]
port_cagr = CAGR(port, n)
port_low_idx, port_upper_idx, port_mdd = MDD(port)

print(f"""
KS200
> CAGR: {ks200_cagr*100:.2f}%
> MDD: {ks200_mdd*100:.2f}%

포트폴리오
> CAGR: {port_cagr*100:.2f}%
> MDD: {port_mdd*100:.2f}%

Alpha = {(port_cagr-ks200_cagr) * 100:.2f}%
""")
```

    
    KS200
    > CAGR: 4.84%
    > MDD: 41.19%
    
    포트폴리오
    > CAGR: 12.73%
    > MDD: 35.00%
    
    Alpha = 7.89%
    
    
<br>

벤치마크에 비해 포트폴리오가 CAGR이 더 높아 알파가 존재하는 것을 확인할 수 있다. MDD 역시 KS200 보다 작고, 낙폭을 기록하는 기간도 더 짧았다. 이 경우는 리밸런싱을 적용하지 않은 결과인데도 불구하고 성과가 매우 좋아 보인다.


```python
plt.figure(figsize=(10,6))
plt.title(f"코스피 200 vs 포트폴리오(2010~2021년) - Alpha: {(port_cagr-ks200_cagr) * 100:.2f}%", size=20, weight='bold')

plt.plot(ks200['Close'], color='tomato', label='KOSPI 200')
plt.axvspan(ks200.index[ks200_low_idx], ks200.index[ks200_upper_idx], facecolor='tomato', alpha=0.3)

plt.plot(port['Close'], color='dodgerblue', label='portfolio')
plt.axvspan(port.index[port_low_idx], port.index[port_upper_idx], facecolor='dodgerblue', alpha=0.3)

plt.legend()
plt.xlabel("년도", size=20)
plt.ylabel("가격지수", size=20)
plt.show()
```


![PNG](/assets/img/post_img/2022-08-02-benchmark-alpha/3.png){: .align-center}