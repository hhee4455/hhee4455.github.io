---
layout: default
title: airflow dag 작성 기초
nav_order: 1
has_children: false
permalink: /docs/DE/airflow/airflw_dags
parent: airflow
grand_parent: 데이터 엔지니어
date : 2024-07-24
comments: true
---

# Apache Airflow 기초 및 DAG 작성 요령

## 1. Airflow 소개
Apache Airflow는 워크플로우를 작성, 스케줄링 및 모니터링하기 위한 플랫폼입니다. 주로 데이터 파이프라인을 자동화하고 관리하는 데 사용됩니다.

## 2. 기본 개념

### 2.1 DAG (Directed Acyclic Graph)
DAG는 작업(task)들의 모음으로, 각 작업은 특정한 순서에 따라 실행됩니다. DAG는 순환이 없어야 하며, 각 작업은 의존성을 가질 수 있습니다.

### 2.2 Task
Task는 DAG의 단위 작업을 의미합니다. Airflow에서는 다양한 종류의 Operator를 사용하여 Task를 정의할 수 있습니다.

### 2.3 Operator
Operator는 실제 작업의 단위입니다. PythonOperator, BashOperator, EmailOperator 등 다양한 Operator가 있습니다.

### 2.4 Sensor
Sensor는 특정 조건이 충족될 때까지 대기하는 특수한 유형의 Operator입니다. 예를 들어, 특정 파일이 존재하는지 확인하는 FileSensor가 있습니다.

## 3. Airflow 설치 및 설정

### 3.1 설치
```
pip install apache-airflow
```

### 3.2 기본 설정
Airflow는 설정 파일 `airflow.cfg`를 통해 다양한 설정을 관리합니다. 기본 설정 파일은 Airflow 설치 디렉토리 내에 있습니다.

### 3.3 데이터베이스 초기화
Airflow는 내부적으로 메타데이터를 관리하기 위해 데이터베이스를 사용합니다. 초기 설정 후 데이터베이스를 초기화해야 합니다.
```
airflow db init
```

### 3.4 사용자 생성
웹 UI에 접근하기 위해서는 사용자를 생성해야 합니다.
```
airflow users create --username admin --password admin --firstname Admin --lastname User --role Admin --email admin@example.com
```

## 4. DAG 작성 - Hello World 예제

### 4.1 기본 PythonOperator 사용
```
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime

def print_hello():
    print("Hello World")

def print_goodbye():
    print("Goodbye")

default_args = {
    'owner': 'airflow',
    'start_date': datetime(2023, 7, 1),
    'retries': 1
}

dag = DAG(
    dag_id='hello_world',
    default_args=default_args,
    schedule_interval='@daily',
    catchup=False
)

hello_task = PythonOperator(
    task_id='print_hello',
    python_callable=print_hello,
    dag=dag
)

goodbye_task = PythonOperator(
    task_id='print_goodbye',
    python_callable=print_goodbye,
    dag=dag
)

hello_task >> goodbye_task
```

### 4.2 Task Decorator 사용
```
from airflow import DAG
from airflow.decorators import task
from datetime import datetime

default_args = {
    'owner': 'airflow',
    'start_date': datetime(2023, 7, 1),
    'retries': 1
}

dag = DAG(
    dag_id='hello_world_v2',
    default_args=default_args,
    schedule_interval='@daily',
    catchup=False
)

@task
def print_hello():
    print("Hello World")
    return "Hello World"

@task
def print_goodbye():
    print("Goodbye")
    return "Goodbye"

with dag:
    hello = print_hello()
    goodbye = print_goodbye()
    hello >> goodbye
```

### 4.3 BashOperator 사용
```
from airflow import DAG
from airflow.operators.bash import BashOperator
from datetime import datetime

default_args = {
    'owner': 'airflow',
    'start_date': datetime(2023, 7, 1),
    'retries': 1
}

dag = DAG(
    dag_id='bash_example',
    default_args=default_args,
    schedule_interval='@daily',
    catchup=False
)

bash_task = BashOperator(
    task_id='bash_hello',
    bash_command='echo "Hello World"',
    dag=dag
)
```

### 4.4 BranchPythonOperator 사용
```
from airflow import DAG
from airflow.operators.python import BranchPythonOperator
from airflow.operators.dummy import DummyOperator
from datetime import datetime

def choose_branch():
    return 'branch_a' if datetime.now().second % 2 == 0 else 'branch_b'

default_args = {
    'owner': 'airflow',
    'start_date': datetime(2023, 7, 1),
    'retries': 1
}

dag = DAG(
    dag_id='branch_example',
    default_args=default_args,
    schedule_interval='@daily',
    catchup=False
)

branch_task = BranchPythonOperator(
    task_id='branch_task',
    python_callable=choose_branch,
    dag=dag
)

branch_a = DummyOperator(
    task_id='branch_a',
    dag=dag
)

branch_b = DummyOperator(
    task_id='branch_b',
    dag=dag
)

branch_task >> [branch_a, branch_b]
```

## 5. 백필 이해하기

### 5.1 백필 개념
백필(Backfill)은 과거의 특정 기간 동안 실패하거나 누락된 작업을 다시 실행하는 과정입니다. 주로 데이터 누락을 방지하고 데이터 일관성을 유지하기 위해 사용됩니다.

### 5.2 백필 예제
```
from airflow import DAG
from airflow.operators.dummy_operator import DummyOperator
from datetime import datetime, timedelta

default_args = {
    'owner': 'airflow',
    'start_date': datetime(2023, 1, 1),
    'retries': 1
}

dag = DAG(
    dag_id='backfill_example',
    default_args=default_args,
    schedule_interval='@daily',
    catchup=True
)

start = DummyOperator(
    task_id='start',
    dag=dag
)

end = DummyOperator(
    task_id='end',
    dag=dag
)

start >> end
```

### 5.3 수동 백필 실행
특정 날짜 범위에 대해 백필을 실행하려면 다음 명령어를 사용합니다.
```
airflow dags backfill -s 2023-01-01 -e 2023-01-07 backfill_example
```

## 6. 기타 유용한 정보

### 6.1 Variable 사용
Airflow Variables를 사용하여 전역 변수로 사용할 수 있습니다.
```
from airflow.models import Variable

my_var = Variable.get("my_variable")
```

### 6.2 Connection 설정
Airflow Connections를 사용하여 데이터베이스나 API 등의 연결 정보를 설정할 수 있습니다. 이는 Airflow 웹 UI의 Admin -> Connections에서 설정할 수 있습니다.

### 6.3 SLA Misses
SLA Miss는 특정 Task가 설정된 시간 내에 완료되지 않았을 때 발생합니다.
```
from airflow import DAG
from airflow.operators.dummy import DummyOperator
from datetime import datetime, timedelta

default_args = {
    'owner': 'airflow',
    'start_date': datetime(2023, 7, 1),
    'sla': timedelta(minutes=30)
}

dag = DAG(
    dag_id='sla_example',
    default_args=default_args,
    schedule_interval='@daily',
    catchup=False
)

sla_task = DummyOperator(
    task_id='sla_task',
    dag=dag
)
```

### 6.4 XCom 사용
XCom은 Task 간에 데이터를 공유하는 방법입니다.
```
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime

def push_function(**kwargs):
    kwargs['ti'].xcom_push(key='my_key', value='my_value')

def pull_function(**kwargs):
    value = kwargs['ti'].xcom_pull(key='my_key', task_ids='push_task')
    print(f"Pushed value was: {value}")

default_args = {
    'owner': 'airflow',
    'start_date': datetime(2023, 7, 1),
    'retries': 1
}

dag = DAG(
    dag_id='xcom_example',
    default_args=default_args,
    schedule_interval='@daily',
    catchup=False
)

push_task = PythonOperator(
    task_id='push_task',
    python_callable=push_function,
    provide_context=True,
    dag=dag
)

pull_task = PythonOperator(
    task_id='pull_task',
    python_callable=pull_function,
    provide_context=True,
    dag=dag
)

push_task >> pull_task
```
