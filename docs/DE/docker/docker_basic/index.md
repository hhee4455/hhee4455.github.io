---
layout: default
title: Docker 맛보기
nav_order: 1
has_children: false
permalink: /docs/DE/docker/docker_basic
parent: docker
grand_parent: 데이터 엔지니어
date: 2024-07-09
comments: true
---

# Docker 맛보기

## Docker 소개
Docker는 컨테이너 기반의 오픈 소스 가상화 플랫폼으로, 애플리케이션을 빠르고 효율적으로 개발, 배포, 실행할 수 있도록 도와줍니다. 이 문서는 Docker의 주요 기능, 설치 과정, 초기 설정 및 컨테이너화된 애플리케이션 배포에 대한 개요를 제공합니다.

---

## Docker의 주요 기능

### 일반 특징
- **컨테이너 기반**: 경량화된 가상 환경을 제공하여 애플리케이션 실행.
- **신속한 배포**: 이미지를 통한 빠른 배포 및 확장 가능.
- **이식성**: 컨테이너 이미지를 통해 어디서나 일관된 실행 환경 제공.
- **보안성**: 프로세스와 네트워크 격리를 통해 보안 강화.

### 개발 및 테스트
- **일관된 환경 제공**: 모든 환경에서 동일하게 동작하는 컨테이너.
- **빠른 피드백 루프**: 코드 변경 후 즉각적인 테스트 가능.
- **버전 관리 용이**: Docker 이미지를 통해 애플리케이션 버전 관리가 용이.
- **효율적인 협업**: 개발, 테스트, 배포 환경을 통일하여 팀 간 협업 향상.

### 배포 및 확장
- **이식성**: 어디서나 실행 가능한 컨테이너 이미지.
- **확장성**: 컨테이너를 쉽게 확장하여 로드 밸런싱 가능.
- **자동화된 배포**: CI/CD 파이프라인과 통합하여 자동화된 배포 가능.
- **무중단 배포**: 컨테이너 오케스트레이션 도구를 사용하여 무중단 배포 가능.

### 자원 효율성
- **경량화**: 기존 VM보다 적은 자원 사용.
- **빠른 시작**: 컨테이너의 빠른 시작 시간.
- **고밀도 배포**: 하나의 호스트에 다수의 컨테이너 배포 가능.
- **자원 격리**: 각 컨테이너가 독립된 자원(CPU, 메모리)을 사용하도록 격리.

---

## Docker 설치

### 설치 과정
- **다양한 플랫폼 지원**: Windows, macOS, Linux 등 다양한 OS에서 지원.
- **설치 방법**: Docker 공식 문서에 따라 설치 절차 진행.

```
# Ubuntu 예제
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

- **버전 확인**: 설치된 Docker 버전을 확인하여 올바르게 설치되었는지 확인.

``` 
docker --version
``` 

---

## Docker 초기 설정

### 기본 설정
- **설정 파일**: Docker 데몬 설정을 위한 기본 설정 파일 편집.
- **사용자 그룹 설정**: Docker 명령어를 sudo 없이 사용하도록 설정.

``` 
sudo usermod -aG docker ${USER}
``` 

- **데몬 설정 변경**: 필요에 따라 Docker 데몬 설정 파일을 수정하여 추가적인 설정을 적용.

---

## 컨테이너 관리

### Docker 이미지 빌드
- **Dockerfile 작성**: 애플리케이션을 컨테이너로 빌드하기 위한 Dockerfile 작성.

``` 
FROM node:14
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "app.js"]
``` 

- **이미지 빌드**: Dockerfile을 기반으로 이미지를 빌드.

``` 
docker build -t my-node-app .
``` 

- **이미지 최적화**: 이미지 크기를 줄이기 위한 최적화 기법 적용(예: 멀티 스테이지 빌드).

### 컨테이너 실행
- **컨테이너 실행**: 빌드한 이미지를 사용하여 컨테이너를 실행.

``` 
docker run -d -p 3000:3000 my-node-app
``` 

- **포트 포워딩 설정**: 로컬 머신과 컨테이너 간의 네트워크 포트를 연결.
- **환경 변수 설정**: 컨테이너 실행 시 필요한 환경 변수를 설정하여 유연한 실행 환경 제공.

### 컨테이너 관리
- **컨테이너 상태 확인**: 실행 중인 컨테이너의 상태를 확인하고 로그를 모니터링.

``` 
docker ps
docker logs [컨테이너 ID]
``` 

- **컨테이너 중지 및 제거**: 더 이상 필요하지 않은 컨테이너를 중지하고 제거.

``` 
docker stop [컨테이너 ID]
docker rm [컨테이너 ID]
``` 

---

## Docker-Compose 사용

### Docker-Compose 개요
- **다중 컨테이너 애플리케이션 관리**: 여러 컨테이너를 정의하고 동시에 실행.
- **YAML 파일 사용**: docker-compose.yml 파일로 서비스 정의.

### 설정 및 실행
- **설치**: Docker-Compose 설치 방법.

``` 
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
``` 

- **docker-compose.yml 작성**: 여러 서비스를 정의하는 예제 파일 작성.

``` 
version: '3'
services:
  web:
    image: my-node-app
    ports:
      - "3000:3000"
  redis:
    image: "redis:alpine"
``` 

- **Compose 파일 실행**: 정의된 서비스를 기반으로 컨테이너를 실행.

``` 
docker-compose up -d
``` 

- **Compose 파일 관리**: 서비스 상태 확인, 중지, 로그 확인 등.

``` 
docker-compose ps
docker-compose logs
docker-compose down
``` 

---

## Container Orchestration이란?

### 컨테이너 오케스트레이션 개념 설명
- **효율적 관리**: 다수의 컨테이너를 효율적으로 배포, 관리, 확장하는 기술.
- **주요 도구**: Kubernetes, Docker Swarm, Apache Mesos.

### Kubernetes 소개 및 기본 개념
- **구성 요소**: Pod, Node, Cluster, Deployment, Service.
- **작동 원리**: 클러스터 내에서 컨테이너 관리 및 스케일링.
- **자동화된 배포**: 컨테이너 배포 및 스케일링의 자동화.
- **자체 치유**: 실패한 컨테이너를 자동으로 재시작하거나 교체.

### Kubernetes 클러스터 설정 및 관리 방법
- **Minikube 사용**: 로컬에서 Kubernetes 클러스터 설정.
- **kubectl 명령어**: Kubernetes 리소스를 관리하기 위한 기본 명령어 사용법.
- **배포와 스케일링**: Deployment를 사용하여 애플리케이션 배포 및 스케일링.
- **네트워킹**: 서비스 디스커버리와 로드 밸런싱을 통해 네트워크 관리.

---

## 결론
Docker와 Kubernetes는 현대 소프트웨어 개발 및 배포에서 중요한 도구로 자리 잡았습니다. Docker를 사용하면 개발 환경과 프로덕션 환경의 일관성을 유지하고 애플리케이션을 경량의 컨테이너로 패키징하여 어디서나 일관되게 실행할 수 있습니다. Kubernetes는 이러한 컨테이너들을 효율적으로 관리하고 확장할 수 있는 강력한 오케스트레이션 도구입니다. 이 강의를 통해 Docker와 Kubernetes의 기본 개념부터 실습까지 체계적으로 학습함으로써, 실제 프로젝트에서 이를 활용할 수 있는 역량을 갖추게 될 것입니다.

<script src="https://utteranc.es/client.js"
        repo="hhee4455/hhee4455.github.io"
        issue-term="pathname"
        label="comments"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>