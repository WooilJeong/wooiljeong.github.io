---
title: "Python 뉴스레터 서비스 만들기"
categories: python
tags: newsletter
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## 파이썬 뉴스레터 서비스

Python으로 정해진 시간에 자동으로 뉴스레터 컨텐츠를 만들어 메일로 발송하는 서비스를 만들어보자. 관심 키워드를 몇 가지 정해서 각각에 대한 네이버 뉴스 검색 결과를 정리해서 때마다 메일로 보내는 기능을 구현해볼 것이다. 이메일만 자동으로 보내는 것이 아니라 뉴스레터 컨텐츠도 때마다 새로 만들어야 하기 때문에 약간의 웹 크롤링 과정도 포함된다. 

<br>

## 뉴스레터 예시

네이버, 카카오, 라인, 쿠팡 그리고 배달의민족을 관심 키워드로 정하고, 각각에 대한 네이버 뉴스 검색 결과를 뉴스레터로 만들어 보낸 결과는 다음과 같다.

![PNG](/assets/img/post_img/2021-12/newsletter.png){: .align-center}

<br>

## 프로젝트 구조

일단 아래와 같은 폴더 구조를 하나 만든다.

- template
    - newsletter.html: 뉴스레터 컨텐츠가 담긴 HTML 양식 파일
- src
    - newsletter.py: 컨텐츠를 만들고 이메일을 자동 발송하는 기능이 포함된 모듈
- config.py: G메일 계정 정보가 담길 파일 (*외부에 노출되지 않도록 유의)
- main.py: 뉴스레터 발송 일정 설정과 발송하는 기능이 담긴 실행 파일

```bash
project/
├── template/
│      └── newsletter.html
├── src/
│      └── newsletter.py
├── config.py
└── main.py
```

<br>

## 개발환경(Python 3.8.8)

가상환경을 만들어 뉴스레터 서비스 개발에 필요한 파이썬 패키지만 설치해보자. 각 패키지에 대한 설명은 생략한다.

- 웹 크롤링 관련 패키지
    - requests
    - bs4
- 테이블 형태의 컨텐츠 생성 관련 패키지
    - pandas
    - pretty-html-table
- 스케줄링 관련 패키지
    - schedule

```bash
# 가상환경 생성
python -m venv venv

# (Linux) 가상환경 활성화
. venv/bin/activate
# (Windows) 가상환경 활성화
. venv/Scripts/activate

# pip 업그레이드
python -m pip install --upgrade pip

# 패키지 설치
pip install pandas schedule requests bs4 pretty-html-table
```

<br>

## 1. template/newsletter.html 작성하기

template 경로의 newsletter.html 문서는 뉴스레터의 양식이라고 생각하면 된다. 원하는 양식대로 수정해서 사용하면 된다. 아래 HTML 문서에는 {sort_selected}와 {contents}가 포함되어 있는데, 이는 파이썬으로 정의한 변수이고, 전자는 조회기준을 후자는 네이버 뉴스 크롤링 결과를 테이블로 정리한 컨텐츠를 뜻한다.

```html
<div style="width:100%">

    <div style="max-width:600px;margin:0 auto;padding:60px 0 30px 0;font-family:'Roboto',Arial,Helvetica,sans-serif;font-size:16px;line-height:1.5;border:1px solid #e2e2e2">
  
      <div align="center" style="padding-right:0px;padding-left:0px" class="logo-area">
        <div style="font-size:1px;line-height:20px">&nbsp;</div><a href="https://wooiljeong.github.io" style="outline:none" target="_blank"> <img align="center" border="0" src="https://avatars.githubusercontent.com/u/38076110?v=4" alt="Logo" title="Logo" style="text-decoration-line: none; height: auto; border: none; width: 100%; max-width: 143px; display: block;" width="143"></a>
        <div style="font-size:1px;line-height:20px">&nbsp;</div>
      </div>
  
      <hr style="border:0;border-top:solid 1px #e2e2e2;width:90%;margin:30px auto" class="horizontal-line">
  
      <div style="max-width:90%;margin-left:auto;margin-right:auto;margin-top:40px" class="nomal-paragraph">
  
        <div style="margin-top:20px">
          안녕하세요. 관심 키워드별 네이버 뉴스 검색 결과를 정리해주는 뉴스레터입니다.
        </div>
  
      </div>
  
      <div style="max-width:90%;margin-left:auto;margin-right:auto;margin-top:20px" class="bullet-point">
        <ul>
          <li>조회기준: {sort_selected}</li>
        </ul>
      </div>
  
      <div style="max-width:90%;margin-left:auto;margin-right:auto;margin-top:20px" class="bullet-point">
        <ol>
          <!-- <li>순서 리스트</li> -->
        </ol>
      </div>
      
      <!-- 메인 이미지 넣기 -->
      <div align="center" style="padding-right:0px;padding-left:0px;margin-top:40px" class="full-image">
        <!-- <img align="center" border="0" src="" alt="Image" title="Image" style="border: 0px; height: auto; width: 100%; display: block;"> -->
      </div>
  
      <div style="max-width:90%;margin-left:auto;margin-right:auto;margin-top:40px" class="nomal-paragraph">
  
        <div style="margin-top:20px">
          {contents}
        </div>
  
      </div>
  
      <div align="center" style="padding-top:40px;padding-right:10px;padding-bottom:10px;padding-left:10px">
        <a href="https://wooiljeong.github.io" style="text-decoration-line: none; display: inline-block; color: rgb(255, 255, 255); background-color: rgb(0, 0, 0); border-radius: 60px; width: auto; border-width: 1px; border-style: solid; border-color: rgb(0, 0, 0); padding: 10px 25px;" target="_blank">블로그</a>
        <a href="https://github.com/wooiljeong" style="text-decoration-line: none; display: inline-block; color: rgb(255, 255, 255); background-color: rgb(0, 0, 0); border-radius: 60px; width: auto; border-width: 1px; border-style: solid; border-color: rgb(0, 0, 0); padding: 10px 25px;" target="_blank">깃허브</a>
      </div>
  
      <div style="text-align:center;">
        <a style="font-size:12px;color:silver" href="mailto:mcwooil2@gmail.com?subject=Unsubscribe!&amp;body=I&nbsp;don't&nbsp;want&nbsp;to&nbsp;receive&nbsp;an&nbsp;email&nbsp;from&nbsp;your&nbsp;service!" target="_blank">Unsubscribe from emails</a>
      </div>
  
    </div>
  </div>
```

<br>

## 2. src/newsletter.py 작성하기

src 경로의 newsletter.py는 Python Email 전송 클래스와 Pandas DataFrame을 HTML 테이블 태그로 변환하는 함수, 웹 크롤링 함수, 전처리 함수, HTML 뉴스레터 템플릿 적용 함수 그리고 컨텐츠 생성 함수로 구성되어 있다.

```python
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.base import MIMEBase
from email import encoders
import pandas as pd
import datetime
from dateutil.relativedelta import relativedelta
import codecs
import logging
import requests
from bs4 import BeautifulSoup
from pretty_html_table import build_table

class PyMail:
    """
    Python Email 전송 클래스
    """
    def __init__(self, my_email_id, my_email_pw):
        """
        G메일 계정, SMTP 정보 및 세션 초기화
        """
        # 계정 정보 초기화
        self.my_email_id = my_email_id
        self.my_email_pw = my_email_pw
        # G메일 SMTP 호스트, 포트 정보 초기화
        self.smtp_host = 'smtp.gmail.com'
        self.smtp_port = 587
        # 세션 정의
        self.session = smtplib.SMTP(self.smtp_host, self.smtp_port)

    def send_mail(self, target_email_id, title, contents, subtype=None, attachment_path=None):
        """
        이메일 전송 메서드
        - 수신자 이메일, 제목, 내용, 문서타입, 첨부 파일 경로
        """
        # 세션 보안 TLS 시작
        self.session.starttls()
        # 세션 계정 로그인
        self.session.login(self.my_email_id, self.my_email_pw)
        # 제목, 본문 작성
        msg = MIMEMultipart()
        msg['Subject'] = title
        if not subtype:
            msg.attach(MIMEText(contents, 'plain'))
        else:
            msg.attach(MIMEText(contents, subtype))
        # 파일첨부 (파일 미첨부시 생략가능)
        if attachment_path:
            fileName = attachment_path.split("/")[-1]
            attachment = open(attachment_path, 'rb')
            part = MIMEBase('application', 'octet-stream')
            part.set_payload((attachment).read())
            encoders.encode_base64(part)
            part.add_header('Content-Disposition', "attachment; filename= " + fileName)
            msg.attach(part)
        # 메일 전송
        try:
            self.session.sendmail(self.my_email_id, target_email_id, msg.as_string())
            self.session.quit()
        except:
            self.session.quit()

def df_to_html_table(df, index=False):
    """
    Pandas DataFrame을 HTML 테이블 태그로 변환
    """
    return build_table(df, 'blue_light')

def make_contents(search_word_list, sort):
    """
    웹 크롤링 함수
    """
    df = pd.DataFrame()
    for search_word in search_word_list:
        # 해당 url의 html문서를 soup 객체로 저장
        url = f'https://m.search.naver.com/search.naver?where=m_news&sm=mtb_jum&query={search_word}&sort={sort}'
        req = requests.get(url)
        html = req.text
        soup = BeautifulSoup(html, 'html.parser')
        search_result = soup.select_one('#news_result_list')
        news_links = search_result.select('.bx > .news_wrap > a')
        times = search_result.select("#news_result_list > li > div > div.news_info > div.info_group > span:nth-child(2)")
        source = search_result.select("#news_result_list > li > div.news_wrap > div.news_info > div.info_group > a")
        title_list = list(map(lambda x: x.text, news_links))
        link_list = list(map(lambda x: x.attrs['href'], news_links))
        times_list = list(map(lambda x: x.text, times))
        source_list = list(map(lambda x: x.text, source))
        source_link_list = list(map(lambda x: x.attrs['href'], source))
        tmp = pd.DataFrame({"Title": title_list, "Times": times_list, "Source": source_list, "Link": link_list, "SourceLink": source_link_list})
        tmp['Keyword'] = search_word
        df = df.append(tmp.head(3))
    df = df[['Keyword', 'Title', 'Times', 'Source', 'Link', 'SourceLink']]
    df.index = range(len(df))
    return df

def preprocessing(df):
    """
    전처리 함수
    """
    new_title_list = []
    new_source_list = []
    for idx, row in df.iterrows():
        title = row['Title']
        link = row['Link']
        source = row['Source']
        source_link = row['SourceLink']
        new_title = f"""<a href="{link}">{title}</a>"""
        new_source = f"""<a href="{source_link}">{source}</a>"""
        new_title_list.append(new_title)
        new_source_list.append(new_source)
    df['Title_Link'] = new_title_list
    df['Source_Link'] = new_source_list

    # 시점 계산
    now = datetime.datetime.now()
    df.loc[df['Times'].str.contains("일 전"), 'Times_'] = df.loc[df['Times'].str.contains("일 전")]['Times'].apply(lambda x: now-relativedelta(days=int(x.split("일")[0])))
    df.loc[df['Times'].str.contains("시간 전"), 'Times_'] = df.loc[df['Times'].str.contains("시간 전")]['Times'].apply(lambda x: now-relativedelta(hours=int(x.split("시간")[0])))
    df.loc[df['Times'].str.contains("분 전"), 'Times_'] = df.loc[df['Times'].str.contains("분 전")]['Times'].apply(lambda x: now-relativedelta(minutes=int(x.split("분")[0])))
    df.loc[df['Times'].str.contains("\."), 'Times_'] = df.loc[df['Times'].str.contains("\.")]['Times'].apply(lambda x: datetime.datetime.strptime(x, "%Y.%m.%d."))
    df['Times_'] = pd.to_datetime(df['Times_']).apply(lambda x: datetime.datetime.strftime(x, "%Y-%m-%d"))

    # 결과
    df_cls = df[['Keyword','Title_Link','Times_','Source_Link']]
    colDict = {"Keyword": "주제",
               "Title_Link": "제목",
               "Times_": "날짜",
               "Source_Link": "채널"}
    df_cls = df_cls.rename(columns=colDict)
    return df_cls

def merge_with_html_template(contents, sort):
    """
    HTML 뉴스레터 템플릿 적용
    """
    sort_dict = {0: "관련도순", 1: "최신순", 2: "오래된순"}
    sort_selected = sort_dict[sort]
    f=codecs.open("./template/newsletter.html", 'r', 'utf-8')
    html = f.read().format(sort_selected=sort_selected, contents=contents)
    return html

def make_final_contents(search_word_list, sort=1):
    """
    컨텐츠 생성
    sort: 정렬 기준 - 0: 관련도순, 1: 최신순, 2: 오래된순
    """
    # 컨텐츠 생성
    df = make_contents(search_word_list, sort)
    # 전처리
    df_cls = preprocessing(df)
    # HTML로 변환하기
    html = df_to_html_table(df_cls)
    # HTML Contents
    contents_ = html.replace("&lt;","<").replace("&gt;",">")
    # 뉴스레터 HTML 템플릿 적용
    contents = merge_with_html_template(contents_, sort)
    return contents
```

<br>

## 3. config.py 작성하기

config.py에는 G메일 계정 정보를 입력해둔다. 외부에 노출되지 않도록 유의해야한다. 메일 발송에 사용할 G메일 계정의 앱 비밀번호를 발급받으려면 [https://myaccount.google.com/security](https://myaccount.google.com/security)에 접속하여 Google에 로그인 2단계 인증을 설정한 뒤 앱 비밀번호를 발급받으면 된다. 아래와 같이 G메일 계정의 이메일 주소와 앱 비밀번호를 입력하면 된다.

```python
from dataclasses import dataclass

@dataclass
class Config:

    GMAIL_ACCOUNT = {
        "address": "<<이메일 주소>>",
        "password": "<<이메일 앱 비밀번호>>",
    }
```

<br>

## 4. main.py 작성하기

main.py는 위에서 만든 메일 발송 관련 클래스와 함수 그리고 G메일 계정 연동 정보를 불러와 실제로 뉴스레터 컨텐츠를 이메일로 발송하는 파이썬 실행 파일이다. schedule 모듈을 이용해 아래와 같이 매 60분 마다 뉴스레터 컨텐츠를 생성하여 자동으로 메일을 보낼 수 있다. 아래 target_email_id에 받는이 이메일 주소를 할당하고 실행하면 된다.

```python
import datetime
import schedule
import time

from src.newsletter import PyMail
from src.newsletter import make_final_contents
from config import Config

# G메일 계정 정보 초기화
c = Config()
address = c.GMAIL_ACCOUNT['address']
password = c.GMAIL_ACCOUNT['password']

# 뉴스 검색 키워드 정의
search_word_list = ['네이버','카카오','라인','쿠팡','배달의민족']

def send_mail_func():
    """
    컨텐츠 생성 및 이메일 발송 기능 호출 함수
    """
    # 컨텐츠 생성 (sort -> 0: "관련도순", 1: "최신순", 2: "오래된순")
    contents = make_final_contents(search_word_list, sort=0)
    # 타이틀 및 컨텐츠 작성
    date_str = datetime.datetime.strftime(datetime.datetime.now(),'%Y년 %m월 %d일')
    title = f"""📢 키워드별 뉴스검색 결과 ({date_str})"""
    contents=f'''{contents}'''
    # # 첨부파일 경로 설정
    # attachment_path = f"D:/Task.txt"
    # 수신자 정보 설정
    target_email_id = "<<받는이 이메일 주소>>"
    # 문서 타입 설정 - plain, html 등
    subtype = 'html'
    # 세션 설정
    PM = PyMail(address, password)
    # 메일 발송
    PM.send_mail(target_email_id, title, contents, subtype)
    print("발송 완료")

# 스케줄 등록
schedule.every(60).minutes.do(send_mail_func)
# schedule.every().day.at("09:00").do(send_mail_func)

while True:
    schedule.run_pending()
    time.sleep(1)
```

<br>

## 뉴스레터 서비스 시작하기

터미널에서 아래 명령어를 입력하면 정해진 시간에 맞게 뉴스레터 컨텐츠를 생성하고 자동으로 메일을 발송한다.

```bash
python main.py
```

<br>

## github repository

아래 깃허브 저장소에서 전체 코드를 참조할 수 있다.

[news-letter-bot](https://github.com/WooilJeong/news-letter-bot)