---
layout: default
title: Redshift
nav_order: 3
has_children: false
permalink: /docs/DE/DW/Redshift_basic
parent: DW
grand_parent: 데이터 엔지니어
date : 2024-07-05
comments: true
---

# AWS Redshift: 소개 및 주요 기능

## Redshift 소개
Amazon Redshift는 AWS에서 제공하는 완전 관리형 클라우드 데이터 웨어하우스 서비스입니다. 이 문서는 Redshift의 주요 기능, 설치 과정, 초기 설정 및 데이터 로딩 기술에 대한 개요를 제공합니다.

---

## Redshift의 주요 기능

### 일반 특징
- **클라우드 기반**: AWS에서 제공하는 데이터 웨어하우스 서비스입니다.
- **대용량 처리**: 최대 2 PB의 데이터를 처리할 수 있습니다. 최소 160GB로 시작하여 점진적으로 용량을 증가시킬 수 있습니다.
- **OLAP(Online Analytical Processing)**: 빠른 응답 속도를 제공하지 않기 때문에 프로덕션 데이터베이스로는 적합하지 않습니다.
- **컬럼 기반 스토리지**: 레코드가 아닌 컬럼별로 데이터를 저장합니다. 컬럼별 압축이 가능하며, 컬럼을 추가하거나 삭제하는 것이 매우 빠릅니다.
- **벌크 업데이트 지원**: S3에 레코드 파일을 복사한 후 COPY 명령어를 사용하여 Redshift로 일괄 복사합니다.
- **고정 용량/비용 SQL 엔진**: 최근에는 가변 비용 옵션인 Redshift Serverless도 제공됩니다.
- **데이터 공유 기능(Datashare)**: 다른 AWS 계정과 특정 데이터를 공유할 수 있습니다.
- **Primary Key Uniqueness 미보장**: 다른 데이터 웨어하우스와 달리 primary key의 고유성을 보장하지 않습니다.

### 데이터베이스 특성
- **SQL 기반 관계형 데이터베이스**: PostgreSQL 8.x와 SQL이 호환됩니다. 하지만 PostgreSQL 8.x의 모든 기능을 지원하지는 않습니다.
- **툴 및 라이브러리 접근성**: PostgreSQL 8.x를 지원하는 툴이나 라이브러리로 접근이 가능합니다(JDBC/ODBC).

### 스케일링
- **스케일 아웃 및 스케일 업**: 용량이 부족할 때 새로운 노드를 추가하거나 기존 노드를 업그레이드하는 방식으로 스케일링합니다. Auto Scaling 옵션을 설정하면 자동으로 리사이징이 가능합니다.
- **Redshift Serverless**: 가변 비용 옵션으로 Pay as You Go 방식으로 사용 가능합니다.

### 최적화
- **복잡한 최적화**: 두 대 이상의 노드로 구성될 때 테이블 최적화가 중요합니다. Distkey, Diststyle, Sortkey의 설정이 필요합니다. 적절한 Sortkey와 Distkey를 선택하여 데이터 분포를 균등하게 하고, 쿼리 성능을 최적화해야 합니다.
- **배치 작업**: 대량의 데이터를 한 번에 로드하는 것이 소량의 데이터를 여러 번 로드하는 것보다 효율적입니다. COPY 명령어를 사용하여 대용량 데이터를 로드하세요.
- **압축 사용**: 컬럼 기반 압축을 사용하면 스토리지 비용을 절감하고 성능을 향상시킬 수 있습니다.

---

## Redshift 설치

### 설치 과정
1. **AWS 계정 생성 및 로그인**: 본인 AWS 계정을 생성하고 로그인합니다.
2. **Redshift Serverless 선택**: Free Trial을 선택하고 진행합니다. 만기되기 전에 꼭 셧다운하고 비용 확인이 필요합니다.

### 외부 연결 열기
- **Public Access 설정**: VPC Security Group에서 Inbound rules에 포트번호 5439를 0.0.0.0/0에 오픈합니다.

---

## Redshift 초기 설정

### 스키마 설정
- **스키마 생성**:
  ```sql
  CREATE SCHEMA raw_data;
  CREATE SCHEMA analytics;
  CREATE SCHEMA adhoc;
  CREATE SCHEMA pii;
  ```

### 사용자 생성
- **사용자 생성**:
  ```sql
  CREATE USER hhee4455 PASSWORD '...';
  ```

### 그룹 생성/설정
- **그룹 생성 및 사용자 추가**:
  ```sql
  CREATE GROUP analytics_users;
  CREATE GROUP analytics_authors;
  CREATE GROUP pii_users;

  ALTER GROUP analytics_authors ADD USER hhee4455;
  ALTER GROUP analytics_users ADD USER hhee4455;
  ALTER GROUP pii_users ADD USER hhee4455;
  ```

### 역할(Role) 생성/설정
- **역할 생성 및 할당**:
  ```sql
  CREATE ROLE staff;
  CREATE ROLE manager;
  CREATE ROLE external;

  GRANT ROLE staff TO hhee4455;    
  GRANT ROLE staff TO ROLE manager;
  ```

---

## Redshift 데이터 로딩

### COPY 명령어 사용
- **COPY 명령어를 사용하여 S3에서 Redshift로 데이터 복사**:
  ```sql
  COPY raw_data.user_session_channel
  FROM 's3://hhee4455-test-bucket/test_data/user_session_channel.csv'
  credentials 'aws_iam_role=arn:aws:iam:xxxxxxx:role/redshift.read.s3'
  delimiter ',' dateformat 'auto' timeformat 'auto' IGNOREHEADER 1 removequotes;
  ```

### 에러 확인
- **로드 에러 확인**:
  ```sql
  SELECT * FROM stl_load_errors ORDER BY starttime DESC;
  ```

---

## 보안 및 접근 제어

### IAM 역할 설정
Redshift 클러스터에 대한 접근을 관리하기 위해 AWS IAM 역할을 설정합니다. 각 역할에 최소 권한 원칙을 적용하여 보안을 강화합니다.

### 데이터 암호화
전송 중 및 저장된 데이터를 암호화하여 보안을 유지합니다. AWS Key Management Service(KMS)를 사용하여 암호화 키를 관리할 수 있습니다.

---

## 비용 관리

### Reserved Instances 활용
장기적으로 사용할 경우 Reserved Instances를 사용하여 비용을 절감할 수 있습니다.

### Auto Scaling 설정
사용량에 따라 자동으로 리소스를 조정하여 비용 효율성을 높입니다.

### 모니터링 및 알림 설정
AWS CloudWatch를 사용하여 리소스 사용량을 모니터링하고, 비용 초과 시 알림을 설정하여 비용을 관리합니다.


---

### 참고 문서
- [AWS Redshift Documentation](https://docs.aws.amazon.com/redshift/)
- [AWS IAM Documentation](https://docs.aws.amazon.com/iam/)
- [AWS S3 Documentation](https://docs.aws.amazon.com/s3/)


<script src="https://utteranc.es/client.js"
        repo="hhee4455/hhee4455.github.io"
        issue-term="pathname"
        label="comments"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>