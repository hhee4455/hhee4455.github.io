---
layout: default
title: 데이터 엔지니어 aws 아키텍처
nav_order: 2
has_children: false
permalink: /docs/backend/aws/DE_aws_architecture
parent: aws
grand_parent: 백엔드
date : 2024-07-19
comments: true
---

# 프로젝트 아키텍쳐

## 아키텍쳐

![ETL Process](/assets/images/BE/architecture.png)

## 문제점??
- lambda의 위치가 private인지 public인지? private kafka에 붙을거라면 lambda 위치가 좀 애매하다
- private subnet에서 s3로 바로 붙으려면 다른 endpoint가 필요하지 않을까? 저 상태에서 바로 s3로 통신이 가능한지 확인이 필요할것같아요
- 웹서버가 public보다는 private에 있고, 유저는 LB를 타고 들어가는 형태는 어떨까? 웹서버나 DB 인스턴스가 private에 있는게 더 좋겠단 생각이 든다.

## 답변
- 
- 
- 


<script src="https://utteranc.es/client.js"
        repo="hhee4455/hhee4455.github.io"
        issue-term="pathname"
        label="comments"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>