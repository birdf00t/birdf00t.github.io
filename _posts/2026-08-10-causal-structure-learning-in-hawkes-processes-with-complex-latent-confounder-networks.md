---
layout: post
title: "CAUSAL STRUCTURE LEARNING IN HAWKES PROCESSES WITH COMPLEX LATENT CONFOUNDER NETWORKS"
date: 2026-08-10 09:38:05 +0900
series: Causal AI
tags: [ICLR2026, Graph]
---

### BackGroud
### Why
1)	인과적 충족성 오류
대부분의 기존 연구는 모든 이벤트 과정을 관측할 수 있다고 가정하지만, 실제 시스템에서는 상당 부분이 측정되지 않는다. 이를 무시하면 존재하지 않는 가짜 인과 관계(Spurious Causal Edges)가 도출되는 치명적인 오류가 발생한다.
2)	사전 정보 없는 잠재 변수 추론
잠재 변수의 개수나 위치를 미리 알아야 했던 기존 방식들과 달리, 이 논문은 사전 지식 없이 데이터의 통계적 패턴(랭크 제약 조건)만으로 잠재 교란 요인을 스스로 찾아낸다.
3)	시간 연속 데이터의 이산화 분석
연속적인 Hawkes 과정 데이터를 다루기 쉬운 이산 시간 모델로 변환하여, 복잡한 인과 구조를 수학적으로 명확하게 식별하고 복구할 수 있는 근거를 마련했습니다.
측정되지 않은 숨겨진 요인(잠재 변수)이 있는 복잡한 시스템의 인과 구조를 스스로 찾아내는 프레임워크를 제안한다.

### How
선형화(Hawkes -> AR): Hawkes 과정을 이산시간 선형 자기회귀 구조모델(structural linear model, window causal graph)로 근사한다. 시간간격 Δ→0일 때 원래 Hawkes 과정에 수렴한다는 걸 증명을 통해 ![](https://velog.velcdn.com/images/birdf00t/post/4629ffe4-3e7c-4df8-ab14-12ed24d1f347/image.png)
랭크 제약 조건(Rank Constraints): 관측된 변수들 간 교차공분산(cross-covariance) 행렬의 랭크가 조건화(conditioning)한 변수 집합의 개수와 같아지면, 그 집합이 바로 parent-cause set(= 진짜 원인)이라는 것 
만약 관측된 원인들만으로 설명되는 것보다 랭크가 1만큼 더 높게 나오면, 그 초과분이 바로 "숨겨진 공통 원인(latent confounder)"이 존재한다는 신호가 된다. 랭크 결핍(rank deficiency) 패턴 자체가 두 종류의 정보(누가 원인인지 / 안 보이는 원인이 몇 개 있는지)를 동시에 알려준다.
![](https://velog.velcdn.com/images/birdf00t/post/4f28e7ac-1a8b-4deb-9fb3-6e698de7f9dc/image.png)
-	Phase I 수행: 현재 정보로 찾을 수 있는 엣지 발견.
-	Phase II 수행: 여전히 관계가 설명 안 되는 놈들 사이에서 '잠재 변수'를 찾아 추가.
-	반복: 잠재 변수가 추가되었으니, 다시 Phase I으로 돌아가서 새로 추가된 잠재 변수와 기존 노드들 간의 엣지를 다시 정밀하게 분석.
-	종료: 더 이상의 잠재 변수나 엣지 변화가 없으면 최종 그래프 반환.


### Result
합성데이터 : case 숫자가 커질수록 잠재변수 증가
![](https://velog.velcdn.com/images/birdf00t/post/1fb0fce9-404d-4eb0-a1b1-20d87257e191/image.png)
실제데이터 : 통신망 알람 데이터(55개 기기, 18종 알람, 약 3.5만 이벤트) 중 device_id=8의 5개 알람 서브그래프를 쓰고, Alarm_id=7을 인위적으로 숨긴 뒤 그걸 잠재변수로 성공적으로 복원했다는 것, 그리고 정답 그래프와 비교해 엣지 1개(Alarm1→Alarm3)만 놓쳤다
![](https://velog.velcdn.com/images/birdf00t/post/d0a04bad-5bb7-49e2-a51d-34c272ea4e82/image.png)

### Info
[paper link](https://openreview.net/pdf?id=mA78uXqcnl)
