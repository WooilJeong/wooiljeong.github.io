---
title: " Airflow - Connections, Variables 관리"
categories: Server
tags: airflow
header:
  overlay_image: /assets/img/wallpaper.jpg
  overlay_filter: 0.2 # same as adding an opacity of 0.5 to a black background
---

## 환경변수로 Connections, Variables 관리하기

GCP(Google Cloud Platform)의 인증 정보와 기타 환경변수를 Airflow의 UI에서 직접 설정하지 않고, docker-compose.yml에 설정하여 관리하는 방법을 고민해보았다.

<br>

## docker-compose.yml 다운로드

```bash
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.3.0/docker-compose.yaml'
```

- 터미널에서 위 명령을 실행하면 apache airflow 2.3.0 버전의 docker-compose.yml 파일을 다운로드 받을 수 있다.

<br>

## Connections 및 Variables 정보를 입력할 .env 파일 생성

- **.env**

```bash
AIRFLOW_VAR_DOWNLOAD_PATH="/opt/airflow/plugins/download"
AIRFLOW_VAR_CHROMEDRIVER_PATH="/opt/airflow/chromedriver/chromedriver"
AIRFLOW_CONN_GOOGLE_CLOUD_DEFAULT='google-cloud-platform://?extra__google_cloud_platform__key_path=/opt/airflow/dags/config/gcp.json&extra__google_cloud_platform__num_retries=5'
AIRFLOW_CONN_SLACK='{
    "conn_type": "slack_webhook",
    "password": "keys"
}'
```

docker-compose.yml 파일이 위치한 경로에 .env 파일을 만들어 위와 같이 Airflow에서 사용할 Connections와 Variables 정보를 입력하면 된다. Variables의 경우 AIRFLOW_VAR_NAME과 같이 정의하면 되고, Connections의 경우 AIRFLOW_CONN_NAME와 같이 정의하면 된다. 이 때, AIRFLOW_VAR_* 이나 AIRFLOW_CONN_* 모두 대문자로 작성해야한다. DAG에서 이 값들을 참조할 때에는 *부분을 소문자로 바꿔 사용하면 된다. 예를 들어, AIRFLOW_CONN_GOOGLE_CLOUD_DEFAULT는 실제 conn_id로 참조하려면 google_cloud_default가 되는 식이다. 단, 이와 같은 방식으로 환경변수를 설정할 경우 Airflow UI에서는 해당 정보가 보이지 않는다고 한다.

<br>

## (참고) GCP Connections 이슈

참고로 GCP의 GCS나 BigQuery를 Airflow에서 사용하기 위해 Connections에 GCP 인증 정보가 담긴 JSON 파일 경로를 **keyfile_path**에 입력하여 사용하고자 하였다.  [Airflow - Managing Connections](https://airflow.apache.org/docs/apache-airflow/stable/howto/connection.html) 에 나와있듯이 JSON 형식이나 Airflow URI 형식을 사용할 수 있다고 한다. 하지만, 내 경우에는 JSON형태로 GCP Connection 정보를 입력하니 해결하기 어려운 오류가 발생하였다. 따라서 위와 같이 Airflow URI 형식으로 변경하니 정상적으로 DAG에서 GCP Operator가 실행되는 것을 확인하였다. Slack App의 Connection 정보의 경우는 JSON 형식으로 입력해도 오류없이 잘 인식되었다.

<br>

## docker-compose.yml에 .env 파일 설정

```yaml
environment:
    &airflow-common-env
    AIRFLOW__CORE__EXECUTOR: CeleryExecutor
    AIRFLOW__DATABASE__SQL_ALCHEMY_CONN: postgresql+psycopg2://airflow:airflow@postgres/airflow
    AIRFLOW__CORE__SQL_ALCHEMY_CONN: postgresql+psycopg2://airflow:airflow@postgres/airflow
    AIRFLOW__CELERY__RESULT_BACKEND: db+postgresql://airflow:airflow@postgres/airflow
    AIRFLOW__CELERY__BROKER_URL: redis://:@redis:6379/0
    AIRFLOW__CORE__FERNET_KEY: ''
    AIRFLOW__CORE__DAGS_ARE_PAUSED_AT_CREATION: 'true'
    AIRFLOW__CORE__LOAD_EXAMPLES: 'false'
    AIRFLOW__API__AUTH_BACKENDS: 'airflow.api.auth.backend.basic_auth'
    _PIP_ADDITIONAL_REQUIREMENTS: ${_PIP_ADDITIONAL_REQUIREMENTS:-}
```

docker-compose.yml 파일을 살펴보면 위와 같이 환경변수를 설정하는 부분이 존재한다. 처음엔 .env 파일을 만들어 yml의 environments 하위에 있는 값들을 모두 복사하여 env_file 하위에 .env 파일 경로를 설정하여 사용하고자 하였으나 실패했다. .env 파일에 적혀있는 `${}` 과 같은 오퍼레이터를 파싱하는 것이 정상적으로 되지 않았기 때문이다. 따라서 아래와 같이 기존 environment는 그대로 두고 그 아래에 env_file 설정을 추가하여 Connections와 Variables 정보가 담긴 .env 파일 경로를 추가하였다.

```yaml
env_file:
    - .env
```

<br>

## Connections 및 Variables 업데이트

.env 파일에 있는 Connections 및 Variables 정보를 업데이트한 경우 이를 Airflow에 반영하려면 Contanier를 아래와 같이 재실행하면 된다.

```bash
docker-compose restart
```

<br>

## 참고

[Airflow - Managing Connections](https://airflow.apache.org/docs/apache-airflow/stable/howto/connection.html)

[Airflow - Google Cloud Connection](https://airflow.apache.org/docs/apache-airflow-providers-google/stable/connections/gcp.html)