---
layout: post
title: "A Recursive Decomposition Framework for Causal Structure Learning in the Presence of Latent Variables"
date: 2026-08-10 08:08:20 +0900
series: Causal AI
tags: [ICML2026, Graph]
---

### BackGroud
1)	_**Markov 성질**_
확률론에서 "미래는 과거와 무관하게 현재 상태에 의해서만 결정된다"는 기억이 없는(memoryless) 특성. 통계학적으로는 특정 정보를 알면 다른 정보가 불필요해지는 조건부 독립의 상태

2)	_**CI Test(Conditional Independence, 조건부 독립성 검정)**_
변수 X와 Y가, 다른 변수 집합 Z의 값을 알고 나면 서로 관련이 없어지는가? X ⊥ Y | Z(Z를 조건으로 줬을 때 X와 Y가 독립이다)

3)	_**인과 발견 알고리즘 종류**_
제약 기반(Constraint-based) : 변수들 간의 조건부 독립성 검증을 통해 그래프 구조를 찾아낸다. 검정 오류가 누적되면 구조가 틀어질 수 있고, 방향까진 완전히 정하지 못 한다.
점수 기반(Score-based) : 가능한 그래프 구조들에 대해 데이터를 가장 잘 설명하는 구조를 탐색해서 찾는다. 모든 구조를 다 볼 수 없어 탐색 공간이 매우 크고, 지역 최적해에 빠질 위험이 있다.
함수 기반(Function-based) : 변수들 간의 관계를 수학적 방정식(노이즈 포함)으로 가정해서, 앞의 두 방법과 달리 인과 방향까지 완전히 결정할 수 있는 경우가 많다. 함수/노이즈 형태에 대한 가정이 틀리면 결과가 부정확해질 수 있다.

4)	_**인과 관계를 나타내는 그래프 종류**_ 
 ![](https://velog.velcdn.com/images/birdf00t/post/e4933f67-c6dc-497d-bd0d-20d3a86aa337/image.png)

5)	_**분할정복(Divide-and-Conquer)**_
분할(Divide): 큰 문제를 더 작은 여러 개의 하위 문제(subproblem)로 쪼갠다
정복(Conquer): 각 하위 문제를 (보통 같은 방법으로) 개별적으로 푼다

### Why
기존의 제약 기반 인과 발견 알고리즘은 고차원 데이터에서 조건부 독립성(CI) 검정을 반복적으로 수행해야 하므로 계산 비용이 높다. 기존의 분할정복 기법들은 잠재 변수가 없는 환경(DAG)을 가정하고 있어, 잠재 변수가 빈번한 실제 시스템에 적용하기 어렵다. 잠재변수가 존재하는 환경 PAG를 복원하는 인과 구조 학습의 계산 효율성을 높이기 위한 재귀적 분할정복 프레임워크인 DiCoLa를 제안한다.


### How
![](https://velog.velcdn.com/images/birdf00t/post/76bfdb9b-0d1a-49cb-af69-5b8098021220/image.png)

_**Decomposition**_ : 전체 변수 집합을 UIG(Undirected Indepencence Graph)로 변환한 뒤, Markov Blanket 학습을 통해 인과적으로 독립적인 부분들로 쪼갠다
_**Local Learning**_ : 쪼개진 작은 조각들은 크기가 작아 연산 부담이 적으므로, FCI와 같은 기존 알고리즘을 사용하여 각각 독립적으로 구조를 빠르게 학습
_**Recostruction**_ : 부분 구조들을 합칠 때, 분리 집합 내의 연결은 양쪽 하위 문제 모두에서 증거가 확인된 경우에만 최종 구조로 확정하여 오류를 방지



### Result
Number of CI Tests : 계산복잡도(CI 검정 횟수)
ln(Time) : 계산효율성(알고리즘 실행시간)

전체적으로 DiCoLa를 사용한 알고리즘들이 계산복잡도가 낮으면서 높은 정확도를 달성하는 것을 확인할 수 있다.
![](https://velog.velcdn.com/images/birdf00t/post/b13be2eb-d1da-4ffd-a4c3-c1a9f4021d8d/image.png)


### Info
[paper link](https://openreview.net/pdf?id=jdbQzlnYll)
[code link](https://github.com/zhengli0060/DiCoLa-ICML2026)
