---
layout: post
title: "DiScoFormer : Plug-In Density and Score Estimation with Transformers"
date: 2026-08-10 09:42:57 +0900
series: Short Paper
tags: [ICML2026, Transformer]
---


### Why
기존 모델들의 단점
![](https://velog.velcdn.com/images/birdf00t/post/24d3c7f7-630e-4d48-aaf9-be1ca7ec0e7a/image.png)


### How
매번 학습하지 않아도 되는 범용 모델 : 등변성을 위해서 위치 인코딩이 없는 트랜스포머 아키텍처를 아핀 정규화 계층과 결합해서 사용한다.
밀도 출력 : Attention 공식을 KDE로 유도해서 대신 계산
기존 방법들을 합쳐서 한계였던 성능과 범용적 두 마리 토끼를 잡음

### Result
평가 기준
![](https://velog.velcdn.com/images/birdf00t/post/616e47d0-45e2-4e26-9d14-dd262e8ef6a9/image.png)
정확도 ![](https://velog.velcdn.com/images/birdf00t/post/e9f820a9-83ce-4c5d-b0f2-f5d52779eb69/image.png)
확장성 & 일반화 ![](https://velog.velcdn.com/images/birdf00t/post/eb97f3d8-9552-4365-bce8-1c8394c88e4b/image.png)
![](https://velog.velcdn.com/images/birdf00t/post/179c2b70-cee9-44b9-8bf5-39f35b80af2b/image.png)
활용성 ![](https://velog.velcdn.com/images/birdf00t/post/af7bf709-a891-499e-8a3c-5436c2528177/image.png)
![](https://velog.velcdn.com/images/birdf00t/post/d7cb0b9d-1963-494a-9ce6-a9c03a368a12/image.png)

### Info
[paper link](https://openreview.net/pdf?id=gyOWJpP8cQ)

