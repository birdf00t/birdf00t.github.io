---
title: "K nearest neighbors regression"
layout: post
categories: "혼공머신"
---

## 지도 학습 알고리즘

**분류** : 샘플을 몇 개의 클래스 중 하나로 분류하는 문제  
**회귀** : 정해진 클래스가 아닌 임의의 수치를 예측하는 문제

## k-최근접 이웃

**분류** : 가까운 k개 중에 제일 많은 클래스가 샘플의 클래스가 된다  
**회귀** : 가까운 k개의 평균이 샘플의 수치가 된다

## '회귀'의 탄생

**WHO?** 19세기 통계학자이자 사회학자인 프랜시스 골턴  
**HOW?** 그는 키가 큰 사람의 아이가 부모보다 더 크지 않다는 사실을 관찰하고 이를 '평균으로 회귀한다'라고 표현했다. 그 후 두 변수 사이의 상관관계를 분석하는 방법을 회귀라고 불렀다.

## k-최근접 이웃 회귀 모델

**데이터준비**

```python
import numpy as np
perch_length = np.array([8.4, 13.7, 15.0, 16.2, 17.4, 18.0, 18.7, 19.0,
       19.6, 20.0, 21.0, 21.0, 21.0, 21.3, 22.0, 22.0, 22.0, 22.0, 22.0,
       22.5, 22.5, 22.7, 23.0, 23.5, 24.0, 24.0, 24.6, 25.0, 25.6, 26.5,
       27.3, 27.5, 27.5, 27.5, 28.0, 28.7, 30.0, 32.8, 34.5, 35.0, 36.5,
       36.0, 37.0, 37.0, 39.0, 39.0, 39.0, 40.0, 40.0, 40.0, 40.0, 42.0,
       43.0, 43.0, 43.5, 44.0])
perch_weight = np.array([5.9, 32.0, 40.0, 51.5, 70.0, 100.0, 78.0, 80.0,
       85.0, 85.0, 110.0, 115.0, 125.0, 130.0, 120.0, 120.0, 130.0,
       135.0, 110.0, 130.0, 150.0, 145.0, 150.0, 170.0, 225.0, 145.0,
       188.0, 180.0, 197.0, 218.0, 300.0, 260.0, 265.0, 250.0, 250.0,
       300.0, 320.0, 514.0, 556.0, 840.0, 685.0, 700.0, 700.0, 690.0,
       900.0, 650.0, 820.0, 850.0, 900.0, 1015.0, 820.0, 1100.0, 1000.0,
       1100.0, 1000.0, 1000.0])
```

**데이터의 형태 파악을 위한 시각화 (x축: 특성 데이터인 길이, y축: 타깃 데이터인 무게)**

농어 길이와 무게가 비례하는 것을 확인할 수 있다

```python
import matplotlib.pyplot as plt
plt.scatter(perch_length, perch_weight)
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
```

결과 >> ![](https://velog.velcdn.com/images/koeunbi093/post/6c52f094-cae1-40c9-b657-e08d46442b92/image.png)

<hr/>

**데이터를 모델에 사용하기 전에 테스트와 훈련 세트 나누기**

```python
from sklearn.model_selection import train_test_split
train_input, test_input, train_target, test_target
= train_test_split(perch_length, perch_weight, random_state=42)
```

perch_length가 1차원 배열이기 때문에 이를 나눈 train_input과 test_input도 1차원이다

```python
print(train_input.shape)
```

결과 >> (42,)

<hr/>

사이킷런에서 훈련을 하기 위해 reshape 함수를 사용해서 2차원으로 바꾸기  
ex) [1,2,3] -> [ [1],[2],[3] ] , 크기 (3, ) -> 크기(3, 1)

reshape() 함수는 지정한 크기가 원소 개수와 다르면 에러가 발생한다

```python
train_input.reshape(2,3)
```

결과 >> <span style="color: red">cannot reshape array of size 42 into shape (2,3)</span>

<hr/>

크기에 -1을 지정하면 나머지 원소 개수로 모두 채워준다

```python
# train_input.reshape(42,1)도 가능
train_input = train_input.reshape(-1,1)
test_input=test_input.reshape(-1,1)
print(train_input.shape)
```

결과 >> (42, 1)

<hr/>

객체 생성과 회귀 모델 훈련하기

k-최근접 이웃 분류에서 사용한 클래스 KNeighborsClassfier과 비슷한 KNeighborsRegressor을 k-최근접 이웃 회귀에서 사용한다

```python
from sklearn.neighbors import KNeighborsRegressor
knr = KNeighborsRegressor()
knr.fit(train_input, train_target)
```

테스트 세트의 테스트 점수 확인

분류: 정확도 (샘플을 정확하게 분류한 개수의 비율)  
회귀: 결정계수 (정확하게 맞히는 것은 불가능하기에 정확도가 아닌 결정계수)

결정계수 = 1- (타깃-예측)^2의 합/ (타깃- 평균)^2의 합

분자와 분모가 비슷해지면(예측이 타깃의 평균가 비슷해지면) 결정계수는 0에 가까워지고
타깃과 예측이 비슷해지면 결정계수는 1에 가까운 값이 된다

```python
print(knr.score(test_input, test_target))
```

결과 >> 0.992809406101064

<hr/>

타깃과 예측의 절댓값 오차 평균

이 외에도 타깃과 예측한 값 사이를 구해서 예측이 어느정도 벗어났는지 확인할 수 있다

예측값이 평균적으로 19g 정도 타깃값과 다르다는 것을 알 수 있다

```python
from sklearn.metrics import mean_absolute_error
test_prediction = knr.predict(test_input)
mae = mean_absolute_error(test_target, test_prediction)
print(mae)
```

결과 >> 19.157142857142862

<hr/>

훈련 세트의 테스트 점수 확인

훈련세트로 훈련을 했기에 훈련 점수가 더 높아야 함에도 불구하고 테스트 세트의 점수가 더 높다.  
이는 과소적합 됐다고 볼 수 있다

과대적합: 훈련세트 점수는 좋지만 테스트 세트의 점수가 굉장히 안 좋다  
과소적합: 테스트 세트의 점수가 더 좋거나 두 점수 모두 너무 낮다

과소 적합은 훈련, 테스트 세트의 크기가 매우 작거나 모델이 단순해서 훈련이 제대로 되지 않았을 때 일어난다

```python
print(knr.score(train_input,train_target))
```

결과 >>0.9698823289099254

<hr/>

과소 적합을 해결하기 위해 모델을 조금 더 복잡하게 만들겠다

k-최근접 이웃 알고리즘 모델은 k의 개수를 줄이면 더 복잡하게 만들 수 있다  
이웃의 개수를 줄이면 훈련 세트에 잇는 국지적인 패턴에 민감해지고 이웃의 개수를 늘리면 데이터 전반의 일반적인 패턴을 따를 것이다

```python
knr.n_neighbors=3

knr.fit(train_input, train_target)
```

테스트와 훈련 세트의 점수 확인

테스트 세트의 점수가 더 낮아졌으니 과소적합 문제가 해결됐다

```python
print(knr.score(train_input, train_target))
print(knr.score(test_input, test_target))
```

결과 >> 0.9804899950518966
0.9746459963987609
