---
layout: default
title: aws 서비스
nav_order: 1
has_children: false
permalink: /docs/backend/aws/architecture
parent: aws
grand_parent: backend
date : 2024-07-18
comments: true
---

# AWS ElastiCache, Athena, Glue 소개 및 사용 예시

## AWS ElastiCache

### 개념
AWS ElastiCache는 인메모리 데이터 스토어 및 캐시 서비스로, Redis와 Memcached 두 가지 오픈 소스 기술을 지원합니다. ElastiCache는 데이터베이스 성능을 최적화하고 애플리케이션 응답 시간을 줄이는 데 사용됩니다.

### 기능
- **고성능**: 메모리 기반 데이터 저장소로 매우 빠른 데이터 접근과 읽기/쓰기 속도를 제공합니다.
- **확장성**: 필요에 따라 쉽게 확장할 수 있으며, 노드를 추가하거나 클러스터를 재조정하여 성능을 최적화할 수 있습니다.
- **고가용성**: 여러 가용 영역에 걸쳐 복제를 설정할 수 있으며, 자동 장애 조치(Failover) 기능이 있습니다.
- **관리 용이성**: 자동으로 소프트웨어 패치, 백업 및 복구를 관리합니다.

### 사용 사례
- **데이터베이스 캐싱**: 자주 조회되는 데이터베이스 쿼리 결과를 캐싱하여 데이터베이스 부하를 줄이고 응답 시간을 단축합니다.
- **세션 스토어**: 사용자 세션 데이터를 빠르게 저장하고 접근하여 웹 애플리케이션 성능을 향상시킵니다.
- **리더보드/실시간 분석**: 게임 리더보드나 실시간 데이터 분석에 활용됩니다.

### 사용 예시
``` 
import redis

#Redis 클라이언트 생성
client = redis.StrictRedis(
    host='my-redis-cluster.xxxxxx.0001.use1.cache.amazonaws.com',
    port=6379,
    password='mypassword'
)

#데이터 캐싱 예시
client.set('user_session', 'session_data')
session_data = client.get('user_session')
print(session_data) 
```

## Amazon Athena

### 개념
Amazon Athena는 Amazon S3에 저장된 데이터를 SQL을 사용해 분석할 수 있는 서버리스 대화형 쿼리 서비스입니다. 데이터 레이크의 데이터를 빠르고 쉽게 쿼리할 수 있습니다.

### 기능
- **서버리스**: 인프라 관리가 필요 없으며, 사용한 만큼만 비용을 지불합니다.
- **SQL 지원**: 표준 SQL을 사용해 데이터를 쿼리할 수 있습니다.
- **다양한 데이터 형식 지원**: CSV, JSON, ORC, Avro, Parquet 등 다양한 데이터 형식을 지원합니다.
- **통합**: AWS Glue Data Catalog와 통합되어 메타데이터를 관리하고, 다른 AWS 서비스와 쉽게 연동됩니다.

### 사용 사례
- **로그 분석**: S3에 저장된 로그 데이터를 쿼리하여 분석합니다.
- **데이터 레이크 분석**: 데이터 레이크에 저장된 데이터를 분석하여 인사이트를 도출합니다.
- **애드혹 쿼리**: 빠르게 질문을 던지고 답을 얻을 수 있는 비정형 데이터 분석에 사용됩니다.

### 사용 예시
``` -- S3 버킷에 저장된 데이터를 쿼리하는 예시
SELECT *
FROM "my_database"."my_table"
WHERE year = 2023 AND month = '07' 
```

## AWS Glue

### 개념
AWS Glue는 데이터 준비와 변환을 위한 완전 관리형 ETL(Extract, Transform, Load) 서비스입니다. 데이터 엔지니어들이 데이터를 추출하고 변환하여 분석할 수 있도록 도와줍니다.

### 기능
- **완전 관리형**: 인프라 관리 없이 ETL 작업을 설정하고 실행할 수 있습니다.
- **데이터 카탈로그**: 데이터 소스의 메타데이터를 자동으로 크롤링하고 관리하여, 데이터 검색과 접근을 용이하게 합니다.
- **자동화**: ETL 작업을 자동화하여 데이터 파이프라인을 구축할 수 있습니다.
- **다양한 데이터 소스 지원**: 여러 데이터 소스와 연동되어 데이터를 통합하고 변환할 수 있습니다.

### 사용 사례
- **데이터 정제 및 변환**: 원시 데이터를 정제하고 변환하여 분석 준비를 합니다.
- **데이터 웨어하우스로 데이터 로드**: 변환된 데이터를 데이터 웨어하우스(Redshift 등)에 로드합니다.
- **데이터 파이프라인**: 지속적인 데이터 처리를 위한 데이터 파이프라인을 구축합니다.

### 사용 예시
``` 
import boto3

glue = boto3.client('glue', region_name='us-east-1')

response = glue.start_job_run(
    JobName='my-glue-job',
    Arguments={
        '--source_table': 's3://my-bucket/source-data/',
        '--destination_table': 's3://my-bucket/transformed-data/'
    }
)

print(response['JobRunId']) 
```


## 결론
이 블로그에서는 AWS의 세 가지 주요 서비스인 ElastiCache, Athena, Glue에 대해 알아보았습니다. 각 서비스는 데이터 엔지니어링 파이프라인의 중요한 부분을 담당하며, 다양한 데이터 처리 및 분석 요구사항을 효율적으로 해결할 수 있습니다. 각 서비스의 기능과 사용 사례를 잘 이해하고 활용하면, 데이터 엔지니어링 작업을 크게 효율화할 수 있습니다.


<script src="https://utteranc.es/client.js"
        repo="hhee4455/hhee4455.github.io"
        issue-term="pathname"
        label="comments"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>