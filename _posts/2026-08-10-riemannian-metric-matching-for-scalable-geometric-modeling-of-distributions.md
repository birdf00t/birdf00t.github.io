---
layout: post
title: "Riemannian Metric Matching for Scalable Geometric Modeling of Distributions"
date: 2026-08-10 08:58:14 +0900
series: Short Paper
tags: [ICML2026, Manifold, Riemannian Geomtry]
---

### BackGround
1) _**manifold**_ : 전체적으로는 휘어져있지만 확대해서보면 국소적으로 유클리드 공간, 이에 나온 가설로 고차원 데이터도 실제로는 훨씬 낮은 차원의 매니폴드 위에 분포한다.
2) _**Riemannian Metric**_ : 매니폴드의 각 점에서 접공간에 정의되는 내적이다. 이 메트릭으로 리만 매니폴드를 만들어서 거리, 각도, 곡률, 측지선 등을 계산할 수 있다.
3) _**denoising diffusion**_ : 데이터에 점진적으로 노이즈를 추가하는 forward process와, 그 노이즈를 단계적으로 제거하며 데이터 분포를 복원하는 reverse process를 학습하는 생성모델 프레임워크
4) _**CDC 연산자**_ : 데이터 매니폴드 위에서 함수가 얼마나 빨리 변하는지(기울기의 크기)를 알려주는 연산자

### Why
기존 그래프로 거리를 계산하면 데이터 규모가 커질수록 계산량이 폭증하고 고차원에서는 거리의 변별력이 떨여져 매니폴드 구조를 제대로 파악하지 못 하는 한계가 있다. 그래서 그래프 구축 없이 신경망을 통해 직적 학습하는 프레임워크를 제안한다.


### How
수학적으로 매니폴드 위에서 CDC 연산자는 리만 메트릭을 근사할 수 있다. 복잡한 기존 거리 함수 대신 CDC 연산자를 학습하게 한다. 

_**CDC 연산자 학습 방법**_
denoising diffusion을 사용한다.
노이즈를 추가해서 확산한 데이터를 복원하면서 기하학적 구조를 학습하게 한다.


### Result
_**연산 효율성 측면**_
![](https://velog.velcdn.com/images/birdf00t/post/928c1623-2157-4269-9bd7-85c99c161721/image.png)


_**매니폴드 위에서의 경로 탐색 최적성**_
![](https://velog.velcdn.com/images/birdf00t/post/bed6cfd2-fb87-404a-a588-9bb4394daced/image.png)



### Inf
[paper link](openreview.net/pdf?id=KVzXnWPLgX)


