---
title: "CSV파일을 MySQL에 적재하는 세 가지 방법 비교"
categories: server
tags: Mysql
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# CSV파일을 MySQL에 적재하는 세 가지 방법 비교

대용량 CSV 파일을 MySQL DB 테이블에 적재하기 전 적재 속도가 빠른 방법을 찾기 위해 세 가지 방법을 적용하여 테스트 진행함. 

<br>

## 1) Pandas `to_sql`를 이용하여 DataFrame을 DB에 적재

```python
import time
import pandas as pd
import pymysql
from sqlalchemy import create_engine
import configparser

# 행: 100,000, 열: 40, 파일 크기: 27.9MB
df = pd.read_csv("./ETL_TEST_DATA.csv", encoding='cp949')

# 설정 파일 경로
config_path = "../setting/setting.ini"

# 설정 파일 읽기
config = configparser.ConfigParser(interpolation = None)
config.read(config_path)

# params
user = config['database']['user']
password = config['database']['pw']
host = config['database']['host']
port = int(config['database']['port'])
database = config['database']['database']

# 실행 시작 시간
start_time = time.time()

# DB 접속 엔진 객체 생성
engine = create_engine(f'mysql+pymysql://{user}:{password}@{host}:{port}/{database}', encoding='utf-8')

# DB 테이블 명
table_name = "etl_test_1"

# DB에 DataFrame 적재
df.to_sql(index = False,
          name = table_name,
          con = engine,
          if_exists = 'append',
          method = 'multi', 
          chunksize = 10000)

# 실행 종료 시간
cost_time_sec = time.time() - start_time
cost_time_min = cost_time_sec / 60
cost_time_hour = cost_time_min / 60

print(f">> 소요시간: {cost_time_sec:,.1f}초 = {cost_time_min:,.1f}분 = {cost_time_hour:,.1f}시간")
```

```markdown
>> 소요시간: 586.2초 = 9.8분 = 0.2시간
```

<br>

## 2) SQL Script를 이용하여 CSV파일을 DB에 적재

```sql
# 적재
LOAD DATA LOCAL INFILE "D:/ETL_TEST_DATA.csv" 
INTO TABLE dbName.tableName FIELDS TERMINATED BY "|"
IGNORE 1 ROWS
(`column1`, `column2`, ...);
```

```markdown
# 적재 소요 시간
505.391초
```

<br>

## 3) SQL Script를 이용하여 CSV파일을 DB에 적재(Index 제거)

```sql
## 인덱스 삭제
ALTER TABLE dbName.tableName DROP INDEX idxName;
## 적재
LOAD DATA LOCAL INFILE "D:/ETL_TEST_DATA.csv" 
INTO TABLE aide.etl_test_3 FIELDS TERMINATED BY "|"
IGNORE 1 ROWS
(`column1`,`column2`,...);
## 인덱스 추가
ALTER TABLE dbName.tableName ADD INDEX idxName(column1);
```

```markdown
# 적재 소요 시간
383.328초

# 인덱스 추가 소요 시간 (인덱스 3개 추가함)
145.234초 + 126.719초 + 131.266초 = 403.219초

# 총 소요 시간
786.547초
```

<br>

## 결론

같은 조건에서 여러번 테스트해봐야 더 정확하겠지만, Pandas의 `to_sql` 메서드로 `DataFrame` 객체를 DB에 적재하는 것이나 SQL Script로 직접 CSV 파일을 DB에 적재하는 것 사이에 큰 차이는 없어보인다. 다만, SQL Script를 사용할 때, 테이블에 미리 생성해둔 인덱스를 삭제하고 적재할 경우 조금 더 속도가 빨라졌다. 그러나 적재 후 인덱스를 다시 추가하는데 소요되는 시간을 고려하면 이 방법도 그리 좋은 방법인지 판단하기 쉽지 않다.  더 효율적인 ETL 방법을 찾아봐야겠다.

<br>

## 참고

[[MySQL] csv 파일을 직접 MySQL 테이블로 Import 하는 방법 (대용량 파일 import 팁)](https://moonlighting.tistory.com/140)