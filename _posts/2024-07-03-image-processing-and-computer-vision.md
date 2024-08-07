---
title: "이미지 처리와 컴퓨터 비전"
layout: post
categories: "이미지처리바이블"
---

이미지 처리와 컴퓨터 비전 둘 다 이미지를 다루지만 접근 방식과 목표는 다르다.

### 1. image processing(이미지 처리)

**analog image processing(아날로그 이미지 처리)**  
사진 촬영 시 필름에 다양한 화학 처리를 하거나 카메라로 촬영한 이미지를 조작하거나 편집하는 것  

**digital image processing(디지털 이미지 처리)**  
수학적 알고리즘과 계산 기술에 의존하며 이미지 향상, 이미지 복원, 특징 추출 등이 포함됨. 

**디지털 이미지 처리 단계**
1. **image acquisition(이미지 획득)** : 이미지 캡처 장치를 통해 획득한 이미지를 디지털 형식으로 최대한 품질을 보존하면서 변환하여 컴퓨터에서 처리할 수 있도록 함. 이 과정에서 다양한 이미지 센서와 변환 알고리즘 사용
2. **image enhancement(이미지 개선)** : noise reduction(노이즈 제거), contrast adjustment(명암 조절), color correction(색상 보정) 등으로 이미지의 품질을 향상시키는데 초점을 맞춤. 이 과정에서 다양한 필터링 기법, histogram equalized(히스토그램 평활화), sharpening(샤프닝) 등의 기술 사용
3. **image analysis(이미지 분석)** : feature extraction(특징 추출), pattern recognition(패턴 인식), object detection(객체 감지) 등으로 이미지의 구조와 패턴을 파악하고 중요한 특징을 식별하여 이미지를 분석함. 이 과정에서 edge detection(에지 검출), corner detection(코너 검출), texture analysis(텍스트 분석) 등의 기술 사용. 
4. **image interpretation and understanding(이미지 해석 및 이해)** : image classification(이미지 분류), image retrieval(이미지 검색), image recognition(이미지 인식) 등으로 이미지에서 얻은 데이터를 분석하고 이용하여 이미지 분류, 특정 패턴 인식, 이미지 객체 식별 등 여러 방식으로 데이터를 사용함. 이 과정에서 pattern matching(패턴 매칭), machine learning(머신 러닝), deep learning(딥 러닝) 등의 기술 사용.

### 2. computer vision(컴퓨터 비전)
컴퓨터 비전은 기계가 시각적 데이터를 이해하고 분석하는 능력을 개발하는 과학 분야로 이미지 처리는 주로 디지털 이미지 향상, 복원, 변형에 중점을 두고 컴퓨터 비전은 이미지의 분석과 해석에 초점을 둠. 그래서 컴퓨터 비전은 더 높은 수준의 이해를 필요로 하며 객체 인식, 패턴 분석, 이미지 분류 등의 작업을 포함. 

컴퓨터 비전은 디지털 이미지를 통해 우리가 세상을 인식하고 이해하는 방식을 모방하고 이미지로부터 의미있는 정보를 추출하는 것. 즉, 컴퓨터가 사람처럼 볼 수 있도록 하는 것

**낮은 수준 비전 작업**
- 노이즈 제거
- 대비 향상
- 채도 향상
- 에지 검출

**중간 수준 비전 작업**
- 이미지 영역 분할
- 이미지 객체로 분할
- 이미지 광학 흐름 추정
  
**높은 수준 비전 작업**
- 객체 인식
- 장면 재구성
- 이미지 학습 및 추론

원본 이미지 -> 이미지 처리 -> 컴퓨터 비전
