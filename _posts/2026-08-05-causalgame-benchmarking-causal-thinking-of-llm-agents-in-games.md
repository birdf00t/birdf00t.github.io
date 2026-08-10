---
layout: post
title: "CausalGame: Benchmarking Causal Thinking of LLM Agents in Games"
date: 2026-08-05 18:01:47 +0900
series: Causal AI
tags: [ICML2026, Agent, LLM, benchmark]
---

### Why
기존의 벤치마크들은 데이터 분석이나 단순 과학적 QA에만 치중되어 실제 과학 현장에서 연구자를 괴롭히는 선택 편향, 측정 오차, 숨겨진 교란 요인을 다루지 못한다는 한계가 있다. ![](https://velog.velcdn.com/images/birdf00t/post/aac4307c-fa3b-4b29-b81c-84d02ecc6993/image.png)
이에 따라 에이전트의 인과적 사고 능력을 측정하는 벤치마크 CausalGame을 제안한다. 현실 과학 연구에서 발생하는 선택 편향, 측정 오차, 숨겨진 교란 요인 등의 난관을 게임 환경으로 구현하여, 단순한 통계적 상관관계를 넘어 실제 인과적 메커니즘을 발견할 수 있는지 평가한다.

### How
![](https://velog.velcdn.com/images/birdf00t/post/b8289c3e-a328-4924-b8e4-815812029167/image.png)

_**Queried History**_ : 에이전트는 과거에 드론들의 생존 기록을 입력받는다. 에이전트는 관측된 데이터에 포함된 선택 편향이나 상관관계에 노출된다.

_**Interact with SCM governed Environment**_ : 
	1)	Deploy Drone: 에이전트는 가설을 검증하기 위해 특정 부품의 DEF(방어력) 값을 설정하여 드론
    을 배치한다. 환경의 법칙(SCM)을 직접 건드리는 '개입(Intervention)' 단계
	2)	Environment: SCM(구조적 인과 모델)은 설정된 변수와 환경 요인(날씨, 적 공격 등)에 따라
    생존 여부와 데미지 상태를 결과로 반환
	3)	Agent : 에이전트는 관측된 결과가 자신의 가설과 맞는지 끊임없이 확인
    
_**Final Report & Reflection**_ : 모든 실험이 종료되면, 에이전트는 "왜 이 디자인이 성공하는지"에
대한 인과적 메커니즘을 보고서로 제출한다. 이때 평가자는 에이전트의 실험 설계 논리와 성찰 능력을 루브릭으로 점수화한다.
![](https://velog.velcdn.com/images/birdf00t/post/a8c1fb9e-21cd-49e8-b855-b6ae9bf448f5/image.png)

### Result
_**기존 벤치마크들과 비교 기준**_ : Automated evaluation(자동 평가), Experiment design(실험 설계), Multi-turn interaction(다중 턴 상호작용), Causal relations(인과 관계), Explanation evaluation(설명 평가), Observational pitfalls(관찰 연구의 함정)

![](https://velog.velcdn.com/images/birdf00t/post/fc59ab22-6e9a-40bd-971d-f45d8628f443/image.png)
기존 나온 LLM들을 CausalGame 벤치마크로 선택 편향, 숨겨진 교란 두 가지 버전 결과
![](https://velog.velcdn.com/images/birdf00t/post/92153dd1-8bd7-4002-b686-dc3822ad2e5a/image.png)

### Info

