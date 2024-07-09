---
layout: default
title: Redshift deepdive
nav_order: 4
has_children: false
permalink: /docs/DE/DW/Redshift_advanced
parent: DW
grand_parent: 데이터 엔지니어
date : 2024-07-06
comments: true
---

# AWS Redshift: deep dive into aws redshift(예정)

---

## Redshift 개요 및 아키텍처

### Redshift의 기본 개념 및 데이터 웨어하우스의 역할 이해
- 데이터 웨어하우스란 무엇인가?
- Redshift가 제공하는 데이터 웨어하우스 솔루션의 특징

### Redshift 클러스터 아키텍처: Leader Node, Compute Node
- Leader Node와 Compute Node의 역할
- 클러스터 내부에서의 데이터 처리 및 쿼리 실행 방식

### 분산 컴퓨팅 및 MPP(Massively Parallel Processing)
- MPP의 개념과 Redshift에서의 구현
- 분산 컴퓨팅을 통한 성능 향상 방법

## Redshift 클러스터 구성 및 관리

### 클러스터 생성 및 설정
- 클러스터 생성 방법 및 초기 설정
- 각 설정의 의미와 중요성

### 클러스터 스냅샷 및 백업 관리
- 스냅샷 생성 및 복구 방법
- 백업 전략 및 모범 사례

### 노드 유형 및 크기 선택
- 노드 유형의 종류 및 선택 기준
- 클러스터 크기 조정 방법

### 자동화된 스냅샷 및 복구
- 자동 스냅샷 설정 방법
- 스냅샷을 통한 데이터 복구 절차

## 데이터 로딩 및 언로드

### COPY 명령어를 사용한 데이터 로딩
- COPY 명령어 사용법 및 최적화 방법
- 데이터 로딩 시 고려 사항

### S3, DynamoDB, EMR, EC2, Data Pipeline 등을 통한 데이터 통합
- 각 데이터 소스와의 통합 방법
- 데이터 통합의 모범 사례

### UNLOAD 명령어를 사용한 데이터 언로드
- UNLOAD 명령어 사용법 및 최적화 방법
- 데이터 언로드 시 고려 사항

## 데이터 모델링 및 테이블 디자인

### 분포 키와 소트 키의 선택과 사용
- 분포 키 및 소트 키의 역할
- 최적의 키 선택 방법

### 테이블 스키마 설계 및 성능 최적화
- 테이블 스키마 설계 원칙
- 성능을 고려한 최적화 기법

### 압축 알고리즘 사용 (Column Encoding)
- 데이터 압축의 중요성
- Column Encoding 사용 방법 및 이점

### 데이터 파티셔닝
- 데이터 파티셔닝의 개념과 이점
- 파티셔닝 전략 및 구현 방법

## 쿼리 성능 최적화

### 쿼리 실행 계획 분석 (EXPLAIN)
- EXPLAIN 명령어를 통한 쿼리 실행 계획 분석
- 쿼리 성능 개선을 위한 분석 방법

### 쿼리 성능 향상을 위한 최적화 기법
- 쿼리 최적화 기법 소개
- 성능 향상을 위한 모범 사례

### Workload Management (WLM) 설정
- WLM의 개념과 설정 방법
- 워크로드 관리 전략

### Materialized Views 활용
- Materialized Views의 개념과 사용 방법
- 성능 향상을 위한 Materialized Views 활용법

## 보안 및 접근 관리

### IAM 역할 및 정책 설정
- IAM 역할과 정책의 개념
- Redshift에서의 보안 설정 방법

### 네트워크 보안 (VPC, 서브넷, 보안 그룹)
- 네트워크 보안 구성 요소
- 안전한 네트워크 환경 설정 방법

### 데이터 암호화 (전송 중 및 저장 시)
- 데이터 암호화의 필요성
- 전송 중 및 저장 시 데이터 암호화 방법

## 모니터링 및 로깅

### CloudWatch를 통한 모니터링
- CloudWatch를 사용한 모니터링 설정
- 주요 메트릭 및 알림 설정

### Amazon Redshift Spectrum을 사용한 외부 테이블 쿼리
- Redshift Spectrum의 개념과 사용 사례
- 외부 테이블 쿼리 방법

### Performance Insights 및 STL, SVL 뷰 사용
- Performance Insights 사용 방법
- STL, SVL 뷰를 통한 성능 모니터링 및 분석

## 비용 관리

### 요금 모델 이해 (온디맨드, 예약 인스턴스)
- Redshift의 요금 모델 설명
- 각 모델의 장단점

### 비용 최적화 방법 (클러스터 크기 조정, 클러스터 일시 중지/재개)
- 비용 절감을 위한 클러스터 관리 방법
- 클러스터 크기 조정 및 일시 중지/재개 방법

<script src="https://utteranc.es/client.js"
        repo="hhee4455/hhee4455.github.io"
        issue-term="pathname"
        label="comments"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>