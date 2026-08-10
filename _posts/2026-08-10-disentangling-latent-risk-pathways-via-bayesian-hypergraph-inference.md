---
layout: post
title: "Disentangling Latent Risk Pathways via Bayesian Hypergraph Inference"
date: 2026-08-10 09:47:44 +0900
series: Short Paper
tags: [ICML2026]
---

### Why
기존 HER 분석 방식은 질병을 개별적으로 다루거나, 블랙박스 모델을 사용하여 질병 간의 공유된 위험 요인을 체계적으로 설명하지 못했다. 특히 희귀 질환의 경우 데이터 부족으로 독립적인 모델링 시 추정치가 불안정하며, 기존 방법론들은 질병 간의 복잡하고 계층적인 인과 구조를 명확히 분리해내지 못하는 문제, 복잡한 질병 데이터와 위험 요인 간의 관계를 규명하기 위해 Bayesian Hypergraph Pathway Inference (BHPI) 프레임워크를 제안


### How
![](https://velog.velcdn.com/images/birdf00t/post/d660ddce-d808-4d0e-83fa-879fcef3f23b/image.png)
Bayesian Hypergraph Pathway Inference (BHPI): 질병을 하이퍼그래프의 노드로, 공유된 위험 패턴을 하이퍼엣지(hyperedge)로 정의하여 질병이 여러 경로에 중첩되어 참여할 수 있는 구조를 설계, Repulsion Prior를 도입하여 모델의 파라미터가 중복되지 않고 희소성(sparsity)을 유지하도록 유도한다.

### Result
분류 작업 성능
![](https://velog.velcdn.com/images/birdf00t/post/c5234261-0371-40ce-85e3-89304244dd84/image.png)
결과
![](https://velog.velcdn.com/images/birdf00t/post/21a4a14c-ebb1-4692-bb88-0c0bbc959195/image.png)

### Info
[paper link](https://openreview.net/pdf?id=vNfbqRzash)
[code link](https://github.com/Naomi-Ding/BHPI)

