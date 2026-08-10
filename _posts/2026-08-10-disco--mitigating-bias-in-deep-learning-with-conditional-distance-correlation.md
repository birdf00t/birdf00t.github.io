---
layout: post
title: "DISCO : Mitigating Bias in Deep Learning with Conditional Distance Correlation"
date: 2026-08-10 09:41:24 +0900
series: Causal AI
tags: [ICML2026]
---

### BackGround
인과적 안정성 : 모델의 예측값이 편향(bias) 속성의 반사실적(counterfactual) 변화에 영향을 받지 않고 오직 작업과 인과적으로 연결된 경로만 따를 때를 말한다.

### Why
기존의 bias 제거 기법들은 특정 데이터 타입에만 제한적이거나, 인과 구조에 대한 고려 없이 통계적 독립성만 강제하여 태스크와 관련된 핵심 신호까지 함께 삭제하는 Over-debiasing 문제가 발생했다. 또한, 기존의 조건부 독립성 계산법은 연산 복잡도가 너무 높아 실제 딥러닝 환경에서 사용이 불가능했다.

### How
기존 조건부 거리 상관관계의 높은 계산 복잡도를 대수적 분해(Algebraic Factorization)를 통해 계산 복잡도를 최적화한 sDISCO 추정기를 제안하여, 딥러닝 모델이 인과적으로 안정적인 예측 수행하도록 한다

1)	구조화 : SAM(Standard Anti-Causal Model)을 통해 태스크 관련 경로와 편향 경로를 분리
  ![](https://velog.velcdn.com/images/birdf00t/post/b48979c9-7b27-4352-899c-136bfa44c233/image.png)
Z (Backdoor variables): confounder(교란 요인)나 collider(충돌 요인) 경로를 가짜 상관관계를 만드는 변수들
![](https://velog.velcdn.com/images/birdf00t/post/a15d5488-e994-442c-a82b-0f613eebd95c/image.png)
W (Mediator variables): 원치 않는 지름길(shortcut)로 작용할 수 있는 중간 변수들
![](https://velog.velcdn.com/images/birdf00t/post/c9aa8255-227b-4d65-abf8-d9cf440a90d8/image.png)
2) 효율화 : 기존의 높은 계산 복잡도를 가지는 조건부 거리 상관관계 계산식을 대수적 분해(Algebraic Factorization)를 통해 계산 복잡도를 낮추어 딥러닝 훈련에 통합 가능한 sDISCO와 DISCOm을 설계
![](https://velog.velcdn.com/images/birdf00t/post/a67b7cd7-3118-4474-aa5c-a674e220cbf2/image.png)

### Result
유연성 비교
![](https://velog.velcdn.com/images/birdf00t/post/1588d96a-2958-4c8a-9448-59338ec54a9f/image.png)
정확도 비교
![](https://velog.velcdn.com/images/birdf00t/post/5035dc21-3e48-46c1-b576-66da4161783b/image.png)

### Info
[paper link](https://openreview.net/pdf?id=TYjTWxDOOC)

