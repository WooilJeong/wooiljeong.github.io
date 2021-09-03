---
title: "도커(Docker)에서 Apache Airflow 실행하기"
categories: Server
tags: Docker
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

# Running Airflow in Docker

*Apache Airflow 공식 문서 퀵 스타트에 소개된 [Running Airflow in Docker](https://airflow.apache.org/docs/apache-airflow/stable/start/docker.html#docker-compose-yaml) 내용을 정리함.

Airflow를 시작하는 방법이 다양하지만, 공식문서에 따르면 아래에 소개할 방법이 Airflow를 이용해볼 수 있는 가장 빠른 방법이라고 한다. 이 방법을 요약하면 **Docker에서 CeleryExecutor로 Airflow를 시작하는 방법**이다. [CeleryExecutor](https://airflow.apache.org/docs/apache-airflow/stable/executor/celery.html)가 무엇인지 잘 모르지만, Airflow를 설치하고 실행해보는데 문제되지 않았다. Ubuntu 20.04 LTS 환경을 기준으로 작성하였다.

<br>

## 필수 도구

시작하기전에 다음 도구들이 설치되어 있어야 한다. Docker Compose는 yaml 파일 기반으로 여러 컨테이너를 관리할 수 있는 클라이언트다. 설치 방법은 생략한다. 

1. Docker
2. Docker Compose (v1.27.0 이상 설치 권장)

(*[Docker, Docker-compose 차이](https://gahui-developer123.tistory.com/99))

<br>

## 디렉토리 생성

`/home/airflow` 디렉토리를 생성하고, 해당 디렉토리로 이동한다.

```bash
mkdir /home/airflow
```

<br>

## docker-compose.yaml

[docker-compose.yaml](https://airflow.apache.org/docs/apache-airflow/stable/docker-compose.yaml) 파일을 다운로드한다.

```bash
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.1.3/docker-compose.yaml'
```

**yaml 파일에 포함된 여러 서비스**

- airflow-scheduler
스케줄러는 모든 작업과 DAG을 모니터링한 다음 종속성이 완료되면 작업 인스턴스를 트리거
- airflow-webserver
[http://localhost:8080](http://localhost:8080)
- airflow-worker
워커는 스케줄러에 의해 주어진 작업을 실행
- airflow-init
초기화 서비스
- flower
Celery 클러스터를 모니터링하고 관리하기 위한 웹 기반 도구
[http://localhost:5555](http://localhost:5555/)
- postgres
데이터베이스
- redis
데이터베이스, 캐시 및 메시지 브로커로 사용되는 오픈 소스(BSD 라이선스), 인메모리 데이터 구조 저장소

이러한 모든 서비스를 통해 [CeleryExecutor](https://airflow.apache.org/docs/apache-airflow/stable/executor/celery.html)로 Airflow를 실행할 수 있다고 한다. 자세한 내용은 [아키텍처 개요](https://airflow.apache.org/docs/apache-airflow/stable/concepts/overview.html)를 참조하면된다고 한다. 설치 후에 찾아보기로 한다.

호스트**와 컨테이너 사이의 디렉토리 마운팅**

- ./dags - DAG 파일을 저장한다.
- ./logs - 작업 실행 및 스케줄러의 로그를 저장한다.
- ./plugins - 사용자 정의 플러그인을 저장한다.

컨테이너의 일부 디렉토리가 마운트되어 해당 컨텐츠가 호스트와 컨테이너 간에 동기화된다. 간단히 말해서 호스트의 특정 디렉토리를 컨테이너 안에서도 사용할 수 있다는 이야기다.

`docker-compose.yaml` 파일은 최신 Airflow 도커 이미지([apache/airflow](https://hub.docker.com/r/apache/airflow))를 사용한다. 새 Python 라이브러리 또는 시스템 라이브러리를 설치해야 하는 경우 [이미지를 빌드](https://airflow.apache.org/docs/docker-stack/index.html)할 수 있다. 커스텀 도커 이미지 사용관련 내용은 생략한다.

<br>

## 환경설정

Airflow를 최초로 실행하기 전 환경설정이 필요하다. 호스트와 컨테이너 사이에 마운팅될 디렉토리를 호스트에 만들어둔다. 파일 권한 관련 설정도 필요하다.

```bash
# 컨테이너와 마운팅될 디렉토리 생성
mkdir -p ./dags ./logs ./plugins

# 호스트, 컨테이너 파일 권한 설정
echo -e "AIRFLOW_UID=$(id -u)\nAIRFLOW_GID=0" > .env
```

다음 명령으로 DB 마이그레이션과 계정을 생성한다. 

```bash
docker-compose up airflow-init
```

아래와 같이 출력이 되면 정상적으로 초기화된 것이라고 한다. 내 경우에는 마지막에 `TypeError: **init**() got an unexpected keyword argument 'disable_buffering'`  에러가 같이 출력되기는 했지만 결과적으로 Airflow를 실행하는데는 문제 없었다. 구글링해도 비슷한 사례가 없어서 원인은 알 수 없다. (아시는 분은 댓글 부탁드립니다.)

```bash
airflow-init_1       | Upgrades done
airflow-init_1       | Admin user airflow created
airflow-init_1       | 2.1.3
start_airflow-init_1 exited with code 0
```

최초 계정의 id는 `airflow` 이고, password도 `airflow` 이다.

<br>

## 환경 초기화

공식문서에 따르면 이렇게 docker를 이용해 airflow를 설치하는 과정은 가장 빠른 방법일 뿐, 프로덕션 환경에서 사용하기 위한 것이 아니라고 경고하고 있다. 뭔가 문제가 생겼을 경우 다 지우고 처음부터 다시 시작하길 권장하고 있다. 아래는 다 지우는 방법이다. 

```bash
# docker-compose.yaml 파일이 있는 경로에서 다음 실행
docker-compose down --volumes --remove-orphans

# docker-compose.yaml 파일 위치한 경로 제거
rm -rf <DIRECTORY>
```

환경 초기화 후에는 다시  `docker-compose.yaml` 을 다운받는 과정부터 시작하면 된다.

<br>

## Airflow 시작하기

```bash
docker-compose up -d # -d: 백그라운드 실행
```

아래와 같이 컨테이너들이 잘 실행된 것을 확인할 수 있다.

```bash
docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED        STATUS                   PORTS                                                  NAMES
a17017bbd550   apache/airflow:2.1.3   "/usr/bin/dumb-init …"   2 hours ago    Up 2 hours (unhealthy)   8080/tcp                                               airflow_airflow-worker_1
b6869440964d   apache/airflow:2.1.3   "/usr/bin/dumb-init …"   2 hours ago    Up 2 hours (unhealthy)   8080/tcp                                               airflow_airflow-scheduler_1
9a4c899739c8   apache/airflow:2.1.3   "/usr/bin/dumb-init …"   2 hours ago    Up 2 hours (healthy)     0.0.0.0:5555->5555/tcp, :::5555->5555/tcp, 8080/tcp    airflow_flower_1
24c3823cbcb4   apache/airflow:2.1.3   "/usr/bin/dumb-init …"   2 hours ago    Up 2 hours (healthy)     0.0.0.0:8080->8080/tcp, :::8080->8080/tcp              airflow_airflow-webserver_1
3162f333503e   postgres:13            "docker-entrypoint.s…"   2 hours ago    Up 2 hours (healthy)     5432/tcp                                               airflow_postgres_1
2ca33663a87d   redis:latest           "docker-entrypoint.s…"   2 hours ago    Up 2 hours (healthy)     0.0.0.0:6379->6379/tcp, :::6379->6379/tcp              airflow_redis_1
```

<br>

## 환경접근

Airflow 시작 후 세 가지 방법으로 환경에 접근할 수 있다.

- CLI명령
- 웹 인터페이스
- REST API

1. **CLI명령**

```bash
# 직접 명령
docker-compose run airflow-worker airflow info

# 간단한 명령 (래퍼 스크립트 이용)
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.1.3/airflow.sh'
chmod +x airflow.sh
./airflow.sh info
```

2. **웹 인터페이스**

[http://localhost:8080/](http://172.30.1.7:8080/)

- id: airflow
- password: airflow

3. **REST API**

```bash
ENDPOINT_URL="http://localhost:8080/"
curl -X GET  \
    --user "airflow:airflow" \
    "${ENDPOINT_URL}/api/v1/pools"
```

<br>

## 전체 초기화

- 컨테이너 중지, 삭제
- 데이터베이스 데이터 볼륨 및 다운로드 이미지 삭제

```bash
docker-compose down --volumes --rmi all
```