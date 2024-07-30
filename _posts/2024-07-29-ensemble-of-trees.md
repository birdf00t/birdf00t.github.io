---
title: "ensemble of trees"
layout: post
categories: "혼공머신"
---

### ensemble learning(앙상블 학습)

더 좋은 예측 결과를 만들기 위해 여러 개의 모델을 훈련하는 머신러닝 알고리즘
정형 데이터를 다루는데 가장 뛰어난 성과를 나타내는 알고리즘으로 대부분 결정 트리 기반으로 만들어짐.

### 사이킷런의 앙상블 학습 알고리즘

**랜덤 포레스트**

대표적
인 결정 트리 기반의 앙상블 학습 방법으로 안정적인 성능 덕분에 많이 사용되고 있어서 앙상블 학습을 적용할 때 가장 먼저 시도해보길 권한다.

랜덤 포레스트는 결정 트리를 랜덤하게 만들어 결정 트리(나무)의 숲을 만든다. 결정 트리의 예측을 사용해 최종 예측을 만든다.

랜덤 포레스트는 각 트리를 훈련하기 위한 데이터를 랜덤하게 만드는데 입력한 훈련 데이터에서 랜덤하게 샘플을 추출하여 만든다. (중복된 샘플이 추출될 수 있다) 이렇게 만들어진 샘플을 부트스트랩 샘플이라고 하며 기본적으로 훈련 세트의 크기와 같게 만든다.

부트스트랩이란 데이터 세트에서 중복을 허용하여 데이터를 샘플링하는 방식을 의미한다.

또한 노드 분할시 전체 특성 중 일부 특성을 무작위로 골라 최선의 분할을 찾는다.

랜덤 포레스트 분류 모델은 전체 특성 개수의 제곱근만큼의 특성을 선택한다. (4개의 특성이 있다면 노드마다 2개를 랜덤하게 선택하여 사용)
랜덤포레스트 회귀 모델은 전체 특성을 사용한다

사이킷런의 랜덤 포레스트는 100개의 결정 트리를 이런 방식으로 훈련하고 분류일 때는 각 트리의 클래스별 확률을 평균하여 가장 높은 확률을 가진 클래스를 예측으로 회귀는 각 트리의 예측을 평균한다.

랜덤하게 선택한 샘플과 특성을 사용해서 훈련 세트 과대적합을 막아주고 안정적인 성능을 얻을 수 있어 기본 매개변수 설정만으로 좋은 결과를 내기도 한다

와인 데이터를 가져와서 화이트 와인을 분류하는 문제

```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split

wine = pd.read_csv('https://bit.ly/wine_csv_data')
data = wine[['alcohol','sugar','pH']].to_numpy()
target = wine['class'].to_numpy()
train_input, test_input, train_target,test_target = train_test_split(data, target, test_size=0.2,random_state=42)
```

100개의 결정 트리를 사용하기 때문에 모든 cpu 코어를 사용하는 것이 좋다

n_jobs = -1 : 모든 cpu 코어를 사용
return_train_score = True : 검증 점수와 훈련 세트 점수 같이 반환 (기본값은 False)

결과를 보면 훈련 세트에 과대 적합된 것을 확인할 수 있다

```python
from sklearn.model_selection import cross_validate
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier(n_jobs=-1,random_state=42)
scores = cross_validate(rf, train_input, train_target,return_train_score=True,n_jobs=-1)
print(np.mean(scores['train_score']), np.mean(scores['test_score']))
```

-> 0.9973541965122431 0.8905151032797809

feature_importance : 랜덤포레스트 모델을 훈련 후 특성 중요도  
각 [알코올 도수, 당도, pH] 이다.

```python
rf.fit(train_input,train_target)
print(rf.feature_importances_)
```

-> [0.23167441 0.50039841 0.26792718]

oob_score: 부트스트랩 샘플에 포함되지 않고 남는 샘플(OOB)로 모델을 평가한 점수

```python
rf=RandomForestClassifier(oob_score=True, n_jobs=-1,random_state=42)
rf.fit(train_input,train_target)
print(rf.oob_score_)
```

-> 0.8934000384837406

**엑스트라 트리**

랜덤 포레스트와 비슷하게 결정 트리를 사용하여 앙상블 모델을 만들지만 부트스트랩 샘플을 사용하지 않고 전체 훈련 세트를 사용하지만 대신 랜덤하게 노드를 분할해 성능은 낮지만 과대적합을 감소시키고 검증 세트의 점수를 높인다. 엑스트라 트리는 무작위성이 더 커서 랜덤 포레스트보다 더 많은 결정 트리를 훈련 시켜야 하지만 랜덤 노드 분할로 인해 빠른 계산 속도를 갖는다.

교차 검증 점수를 확인해본 결과 아까 랜덤 포레스트와 비슷한 결과를 얻었다. 특성이 많지 않아서 두 모델의 차이가 크지 않다.

```python
from sklearn.ensemble import ExtraTreesClassifier

et = ExtraTreesClassifier(n_jobs=-1,random_state=42)
scores = cross_validate(et,train_input, train_target,return_train_score=True,n_jobs=-1)
print(np.mean(scores['train_score']),np.mean(scores['test_score']))
```

-> 0.9974503966084433 0.8887848893166506

엑스트라 트리 모델에서의 특성 중요도

```python
et.fit(train_input,train_target)
print(et.feature_importances_)
```

-> [0.20183568 0.52242907 0.27573525]

**그레이디언트 부스팅**

깊이가 얕은 결정 트리를 사용하여 이전 트리의 오차를 보완하는 방식으로 앙상블하는 방법  
깊이가 얕은 결정 트리를 사용하기 때문에 과대적합에 강하고 일반적으로 높은 일반화 성능 기대할 수 있지만 순서대로 트리를 추가하기 때문에 속도가 느리다

경사하강법을 사용해서 트리를 앙상블에 추가하고 분류는 로지스틱 손실 함수, 회귀는 평균 제곱 오차 함수 사용

과대적합에 가능해서 결정 트리의 개수를 늘려도 과대적합이 되지 않는다  
트리의 개수를 늘리면 성능이 더 향상될 수 있다

```python
from sklearn.ensemble import GradientBoostingClassifier

gb = GradientBoostingClassifier(random_state=42)
scores=cross_validate(gb, train_input, train_target, return_train_score=True,n_jobs=-1)
print(np.mean(scores['train_score']), np.mean(scores['test_score']))
```

-> 0.8881086892152563 0.8720430147331015

결정 트리 개수를 5배 늘렸지만 과대적합을 잘 억제하고 있다

```python
gb = GradientBoostingClassifier(n_estimators=500, learning_rate=0.2,random_state=42)
scores = cross_validate(gb,train_input,train_target,return_train_score=True,n_jobs=-1)
print(np.mean(scores['train_score']),np.mean(scores['test_score']))
```

-> 0.9464595437171814 0.8780082549788999

특성 중요도를 보면 랜덤 포레스트보다 당도에 더 집중된 것을 확인할 수 있다

```python
gb.fit(train_input,train_target)
print(gb.feature_importances_)
```

-> [0.15872278 0.68010884 0.16116839]

**히스토그램 기반 그레이디언트 부스팅**

정형 데이터를 다루는 머신러닝 알고리즘 중 가장 인기가 높다

입력 특성을 256개 구간으로 나누고 노드를 분할할 때 최적의 분할을 매우 빠르게 찾을 수 있다

과대적합을 잘 억제하면서 그레이디언트 부스팅보다 조금 더 높은 성능을 제공한다

```python
from sklearn.experimental import enable_hist_gradient_boosting
from sklearn.ensemble import HistGradientBoostingClassifier

hgb = HistGradientBoostingClassifier(random_state=42)
scores = cross_validate(hgb,train_input,train_target,return_train_score=True)
print(np.mean(scores['train_score']),np.mean(scores['test_score']))
```

-> 0.9321723946453317 0.8801241948619236

훈련세트의 특성 중요도  
각 순서대로 특성 중요도, 특성 평균, 표준 편차를 담고 있다

그레이디언트와 비슷하게 당도에 집중하고 있다

```python
from sklearn.inspection import permutation_importance

hgb.fit(train_input,train_target)
result = permutation_importance(hgb, train_input, train_target, n_repeats=10,random_state=42,n_jobs=-1)
print(result.importances)
print(result.importances_mean)
print(result.importances_std)
```

-> [[0.08793535 0.08350972 0.08908986 0.08312488 0.09274581 0.08755051 0.08601116 0.09601693 0.09082163 0.09082163]
 [0.22782374 0.23590533 0.23936887 0.23436598 0.23725226 0.23436598 0.23359631 0.23398114 0.23994612 0.22724649]
 [0.08581874 0.08601116 0.08062344 0.07504329 0.08427939 0.07792957 0.07234943 0.07465846 0.08139311 0.08466423]]
[0.08876275 0.23438522 0.08027708]
[0.00382333 0.00401363 0.00477012]

테스트 세트로 성능 점수

약 87퍼의 정확도를 얻은걸 보아 앙상블 모델은 단일 결정 트리보다 더 좋은 결과를 얻을 수 있다

```python
result = permutation_importance(hgb, test_input,test_target,n_repeats=10,random_state=42,n_jobs=-1)
hgb.score(test_input,test_target)
```

-> 0.8723076923076923

### 사이킷런 라이브러리

**RandomForestClassifier(랜덤 포레스트 분류 클래스)**  
n_estimators : 앙상블 구성 트리 개수 기본값 100<br>criterion : 불순도 기본값 'gini'<br>max_depth : 트리 최대 깊이 기본값 None<br>min_sample_split : 노드를 나누기 위한 최소 샘플 개수 기본값 2<br>max_features : 최적희 분할을 위한 탐색할 특성 개수 기본값 auto(특성 개수의 제곱근)<br>bootstrap : 부트스트랩 샘플 사용 여부 기본값 True<br>oob_score : OOB 샘플로 훈련 모델 평가 여부 기본값 False<br>n_jobs : 사용할 CPU 코어 수 기본값 1, -1은 모든 코어 사용

**ExtraTreesClassifier(엑스트라 트리 분류 클래스) : 랜덤 포레스트와 동일**

**GradientBoostingClassifier(그레이디언트 부스팅 분류 클래스)**  
loss : 손실 함수 지정 기본값 'deviance' 로지스틱<br>learning_rate : 트리가 앙상블에 기여하는 정도 기본값 0.1<br>n_estimators : 부스팅 단계를 수행하는 트리의 개수 기본값 100<br>subsample : 훈련 세트의 샘플 비율 기본값 1.0<br>max_depth : 개별 회귀 트리의 최대 깊이 기본값 3

**HistGradientBoostingClassifier(히스토그램 기반 그레이디언트 부스팅 분류 클래스)**  
learning_rate : 학습률 기본값 0.1, 1.0이면 감쇠가 없다<br>max_iter : 부스팅 단계를 수행하는 트리의 개수 기본값 100<br>max_bins : 입력 데이터를 나눌 구간의 개수 기본값 255

사이킷런 말고도 그레이디언트 부스팅 알고리즘을 XGBoost, LightGBM 라이브러리도 사용할 수 있다
