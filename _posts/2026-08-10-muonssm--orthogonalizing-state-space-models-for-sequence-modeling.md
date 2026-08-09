---
layout: post
title: "MuonSSM : Orthogonalizing State Space Models for Sequence Modeling"
series: SSM
tags: [ICML2026, Muon, Optimizer]
---

### BackGround
1) _**Muon**_ : 2024년에 제안된 옵티마이저, Adam은 파라미터를 원소 단위로 취급해 학습률을 조정한다. Muon은 가중치 행렬 전체를 하나의 기하학적 대상으로 보고, 모멘텀 행렬을 직교화, 신경망의 2차원 파라미터 (임베딩/출력층 등은 여전히 Adam/AdamW 사용)
2) _**SSM(State Space Model)**_ : 긴 시퀀스 모델링에서 어텐션(attention)을 대체할 수 있는 효율적인 선형 시간(linear-time) 대안으로 등장한 모델이다. Transformer처럼 시퀀스를 처리하지만, 계산 복잡도가 시퀀스 길이에 비례해서 효율적

### Why
기존 SSM은 입력 의존적인 1차 업데이트 방식을 쓰는데, 시간이 지나면서 상태 전이 행렬의 특이값(singular value) 분포가 무너지는 스펙트럼 붕괴가 발생한다. 이로 인해 정보가 방향마다 극단적으로 증폭되거나 소멸해서, 장기 의존성 학습과 기울기 흐름이 불안정해지는 한계가 있었다. 이를 해결하기 위해, 원래 파라미터 그래디언트에 작용하던 Muon 옵티마이저의 핵심 아이디어(직교화를 통한 기하학적 조정)를, 옵티마이저가 아니라 SSM의 상태 업데이트 메커니즘 자체에 이식하는 MuonSSM을 제안한다. 이를 통해 업데이트의 특이값을 유계(bounded)로 유지해서 매끄럽고 균일한(near-isometric) 상태 업데이트를 만들고, 결과적으로 병렬 스캔의 효율성은 유지하면서도 장기 안정성을 확보한다.

### How
![](https://velog.velcdn.com/images/birdf00t/post/6bbde057-83f1-4d5d-ba02-36fb9a8be66a/image.png)
_**정규화**_: 입력에 따른 저계수(Low-rank) 주입 시, 가벼운 Newton–Schulz 반복법을 적용하여 업데이트의 특이값을 유계(Bounded)로 유지하고 최적화의 기하학적 조건을 개선한다. 이 과정은 표준적인 병렬 스캔 연산 복잡도를 그대로 유지한다.
_**모멘텀 경로(Momentum pathway)**_: 상태 업데이트에 시간적 관성(Temporal inertia)을 부여하여 장기적인 정보 전파와 안정적인 기울기 흐름을 확보한다.

### Result
언어 작업
![](https://velog.velcdn.com/images/birdf00t/post/010c1ec1-b3db-4fc8-b1a9-0c25e6c35bf5/image.png)

비전 작업 
![](https://velog.velcdn.com/images/birdf00t/post/de1623e0-b483-4171-a2b8-d1ac7c6e04c4/image.png)


시계열 작업
![](https://velog.velcdn.com/images/birdf00t/post/39597bfd-9000-433d-ac3c-a300ca884233/image.png)

### Inf
[paper link](https://openreview.net/pdf?id=GmP3VcfHi0)
