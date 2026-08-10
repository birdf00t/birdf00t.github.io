---
layout: post
title: "Learning to Theorize the World from Observation"
date: 2026-08-10 08:56:35 +0900
series: WorldModel
tags: [ICML2026]
---

### Why
현재의 월드 모델들은 단순히 다음 관찰 예측에 최적화되어 있어, 데이터가 조금만 바뀌어도 무너지는 경향이 있으며 구조적 이해가 부족하다. 발달 심리학의 이론에 따르면, 인간은 단순히 데이터를 외우는 기계가 아니라 세상을 설명하는 내적 이론을 구축하는 과학자이다. 지능 시스템이 미지의 상황에서도 처음 보는 현상을 이론적 조합을 통해 일반화 수 있는 능력을 갖추게 해야 한다. 재사용 가능한 기본 원리와 이를 조합한 명시적 프로그램을 유도하여 세상을 이해하는 학습 패러다임인 L2T(Learning-to-Theorize) 와 이를 구현한 모델 NEO를 제안한다.

### How
_**L2T(Learning-to-Theorize) 프레임워크**_
![](https://velog.velcdn.com/images/birdf00t/post/b7426f47-b3e1-4a6e-9ad3-b202fe29084d/image.png)

_**NEO(Neural Theorizer) 모델**_
![](https://velog.velcdn.com/images/birdf00t/post/5689ce31-8e06-44e5-aec3-adee745c41a8/image.png)

1)	_**Theory Programmer (잠재적 이론 구축)**_ : 현재 상태 s_k와 목표 관찰값 y를 입력받아, 다음 단계로 이동하기 위한 최적의 기본 연산자(primitive) z를 선택한다. 모델이 스스로 세상의 변화 규칙을 학습하는 과정
2)	_**Program Execution & Shared Transition Model**_: 선택된 연산자 z를 바탕으로 현재 상태 s _k를 다음 상태 s_{k+1}로 전이시킨다. 전이 모델(Transition Model)은 모든 단계에서 공유(Shared)되므로, 모델은 하나의 연산자가 다양한 상황에서 어떻게 작용하는지 범용적인 규칙을 학습하게 된다.
3)	_**MDL(Minimum Description Length principle, 최소 기술 길이 원칙)**_: 녹색 박스는 MDL 원칙이다. 복잡한 현상을 가장 짧으면서 정확한 설명을 유도하는 기준이다. 불필요하게 긴 프로그램은 과적합으로 간주하고 재사용 가능한 핵심 연산자만 남긴다.
4) _**State Grounding**_: 중간 단계의 잠재 상태들이 붕괴하거나 의미 없이 변질되지 않도록 실제 관찰 가능한 데이터의 잠재 공간(manifold) 위에 머물도록 강제한다. 수학적인 지름길만 찾아 정답을 내는 것을 막고, 매 단계가 실제 물리적 변화의 의미를 갖게 한다.

### Result
_**OTIB(Observation-to-Theory Induction Benchmark) 벤치마크**_
![](https://velog.velcdn.com/images/birdf00t/post/a288c63c-30d0-4b9d-a0d0-8a90fff02a5c/image.png)
_**GridWorld OTIB**_
![](https://velog.velcdn.com/images/birdf00t/post/3a5be54b-140c-45d5-ab8e-6ff248dc83f1/image.png)
**_Arithmetic Factorization Reasoning OTIB_**
![](https://velog.velcdn.com/images/birdf00t/post/47219ae5-d8dc-4d1c-83dc-cb10e62de1ed/image.png)
**_이미지 편집 작업 결과 비교_**
![](https://velog.velcdn.com/images/birdf00t/post/f04e38a3-de53-4cec-a037-cdc3d8fd09b8/image.png)


### Info
[paper link](https://openreview.net/pdf?id=wsA8LgHU5U)
