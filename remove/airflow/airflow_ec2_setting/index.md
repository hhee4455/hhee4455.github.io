---
layout: default
title: ec2안에서 externally-managed-environment 에러
nav_order: 2
has_children: false
permalink: /docs/DE/airflow/airflow_ec2_setting
parent: airflow
grand_parent: 데이터 엔지니어
date : 2024-07-21
comments: true
---

## 문제 사진
![ec2_airflow_error](/assets/images/DE/airflow_ec2_error.png)

## 해결 방안 1
``` 
sudo rm /usr/lib/python3.11/EXTERNALLY-MANAGED
```
-- 해결 완료!

<script src="https://utteranc.es/client.js"
        repo="hhee4455/hhee4455.github.io"
        issue-term="pathname"
        label="comments"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>