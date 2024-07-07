---
title: "필요한 도구들"
layout: post
categories: "이미지처리바이블"
---

### 1. OpenCV
컴퓨터로 이미지나 영상을 읽고, 이미지의 사이즈 변환이나 회전, 선분 및 도형 그리기, 채널 분리 등의 연산을 처리할 수 있도록 만들어진 오픈 소스 라이브러리로 이미지 처리 분야에서 가장 많이 사용됨.

OpenCV 라이브러리는 구글 코랩이 아닌 로컬 컴퓨터 기준으로 만들어져서 구글 코랩에서는 모든 기능을 사용할 수 없다. 코랩에서는 'cv2.imshow'를 사용할 수 없고 'cv2_imshow'를 대신 사용한다

파이썬 기본 자료형 리스트는 연산할 때 느리다는 단점이 있어서 이미지와 같이 연산량이 많아진다면 리스트가 아닌 numpy를 사용하면 속도가 빨라지고 더 적은 메모리 공간을 차지한다.
~~~python
import cv2 #OpenCV 사용하기 위해
from google.colab.patches import cv2_imshow #이미지 출력을 위해
import numpy as np #바이트를 넘파이로 변환해주기 위해
import urllib.request #url에서 이미지 불러오기

resp = urllib.request.urlopen('https://raw.githubusercontent.com/Cobslab/imageBible/main/image/like_lenna224.png')
image = np.asarray(bytearray(resp.read()), dtype='uint8')
image = cv2.imdecode(image, cv2.IMREAD_COLOR)

cv2_imshow(image) #이미지 출력
~~~
![](/img/pasted_image_20240706013412.png)

각 픽셀 수로 가로 224, 세로 224, 3차원
~~~python
print(f"이미지 배열 형태: {image.shape}")
~~~
-> 이미지 배열 형태: (224, 224, 3)

**이미지 사이즈 변환**  
cv2.resize 함수로 이미지의 사이즈 가로, 세로 100, 100으로 수정했다
~~~python
image_small = cv2.resize(image,(100,100))
cv2_imshow(image_small)
~~~
![](/img/pasted_image_20240706014013.png)

이미지의 배율을 변환한다
~~~python
image_big = cv2.resize(image, dsize=None, fx=2,fy=1)
cv2_imshow(image_big)
~~~
![](/img/pasted_image_20240706014246.png)

**대칭 변환**  
cv2.flip 함수를 사용하여 이미지 대칭, 변환할 수 있다
0 : 수평축 반전, 1 : 세로축 반전
~~~python
image_fliped = cv2.flip(image,0)
cv2_imshow(image_fliped)
~~~
![](/img/pasted_image_20240706015140.png)
~~~python
image_fliped = cv2.flip(image,1)
cv2_imshow(image_fliped)
~~~
![](/img/pasted_image_20240706015150.png)

**회전 변환**  
cv2.warpAffine 함수는 이미지를 원하는 각도로 회전한다
~~~python
height, width = image.shape[:2]
matrix = cv2.getRotationMatrix2D((width/2,height/2),90,1)
result = cv2.warpAffine(image,matrix,(width,height))
cv2_imshow(result)
~~~
![](/img/pasted_image_20240706020244.png)
~~~python
matrix = cv2.getRotationMatrix2D((width/2,height/2),30,1)
result = cv2.warpAffine(image,matrix,(width,height),borderValue=200)
cv2_imshow(result)
~~~
![](/img/pasted_image_20240706020311.png)

**자르기**  
슬라이싱은 원본 객체의 값을 그대로 참조해서 자른 이미지에 다른 값을 할당시키면 원본 사진 자체가 변한 것을 확인할 수 있다.
~~~python
croped_image = image[50:150,50:150]
croped_image[:]=200
cv2_imshow(image)
~~~
![](pasted_image_20240706021126.png)
원본 이미지에 영향을 미치고 싶지 않을 때는 깊은 복사를 사용해야 한다 copy 메서드 사용
~~~python
resp = urllib.request.urlopen('https://raw.githubusercontent.com/Cobslab/imageBible/main/image/like_lenna224.png')
image = np.asarray(bytearray(resp.read()), dtype='uint8')
image = cv2.imdecode(image, cv2.IMREAD_COLOR)

croped_image = image[50:150, 50:150].copy()
croped_image[:]=200
cv2_imshow(image)
~~~
![](pasted_image_20240706021739.png)

**도형 그리기**  
OpenCV는 이미지에 도형을 그릴 수 있는 기능을 제공한다
- 선 그리기 : cv2.line
- 원 그리기 : cv2.circle
- 직사각형 그리기 : cv2.rectangle
- 타원 그리기 : cv2.ellipse
- 다각형 그리기 : cv2.polylines, cv2.fillPoly
### 2. TensorFlow

구글 브레인 팀에서 개발되어 공개된 오픈 소스 머신 러닝 라이브러리로 다차원 배열을 기반으로 하는 연산을 수행하며 병렬 처리와 지연 실행을 쉽게 수행할 수 있다.

**편의성**  

고수준 API 지원, 사전 빌드된 층 및 모델 제공, 즉시 실행 모드 등  
고수준 API에 Keras(케라스)가 있다. 케라스 API는 모든 표준 모델을 정의해서 선형 회귀부터 복잡한 심층 신경망까지 몇 줄의 코드만으로 모델을 설정하고 컴파일할 수 있다. 또한 모델 아키텍처를 자유롭게 정의할 수 있는 기능도 제공한다. Model 클래스 API를 사용하면 층 그래프를 정의하는데 사용돼서 다중 출력 모델, 방향성 비순환 그래프, 공유 층이 있는 모델을 만들 때 유용하다.

**확장성**
- 행렬 및 벡터 연산에 더 효율적인 GPU와 TPU에서 모델을 훈련할 수 있기 때문에 CPU만 사용할 때보다 더 복잡한 모델 빠르게 처리 가능
- 분산 컴퓨팅도 지원해서 여러 머신에서 동시에 모델을 훈련할 수 있다. 
- 모델 학습 후 대규모 모델 배포 도구 제공

**유연성**
- 모델 아키텍처 설계를 넘어 데이터 전처리부터 배포까지
- 텐서플로 저수준 API는 특정 문제에 따라 고유한 층을 제작할 수 있는 기능 제공
- 맞춤형 손실 함수 생성 도구 제공
- tf.data API는 효율적인 데이터 파이프라인을 구축 제공, tf.data는 모든 데이터를 처리할 수 있을 만큼 다용도로 사용할 수 있다

