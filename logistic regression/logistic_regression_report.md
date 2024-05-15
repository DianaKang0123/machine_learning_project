## 명일 강수여부 예측 (이진분류)

- features
- targets

---
### 🤓 목차
1. 데이터 전처리
2. 연속형 피처 분석
3. 범주형 피처 합산 분석
4. 타겟 분포 조정
5. 다중 공선성 제거
6. 차원 축소
7. 각 모델 별 점수 확인
8. 최적의 모델 찾기
9. 교차검증
10. 임계치 확인, ROC CURVE 확인
11. 결론

---

### 1. 데이터 전처리
- 결측치 비율 확인

<img width="260" alt="isna" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/7b420ae6-432a-461b-9b44-65ec95d56b2f">

- 비율이 10% 이상인 경우 중앙값으로 대체
- 비율이 10% 미만의 경우 `dropna`로 처리

### 2. 연속형 피처 분석
- 데이터 프레임에서 연속형 피처만 추출하여 학습 진행
- Logistic Regression
- 이때 `solver = 'liblinear', L2 패널티, C = 1`
- test
  
<img width="557" alt="num test" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/91ad8b1a-806f-4a68-9681-34a799bd3e7d">

- validation

<img width="567" alt="num_val" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/420860e4-ff52-41b0-b6a0-aa47e94cbd03">

### 3. 범주형 피처 합산 분석
- 범주형 피처의 라벨 인코딩
- Logistic Regression
- 이때 `solver = 'liblinear', L2 패널티, C = 1`
- test

<img width="560" alt="obj_val" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/997947e8-7f49-472e-98c6-3b023a55872f">

- validation

<img width="557" alt="0bj_test" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/e53e7060-24ba-4834-9c97-375eabbca72c">

### 4. 타겟 분포 조정
- 타겟 분포의 불균형이 원인이라고 파악

<img width="451" alt="스크린샷 2024-05-15 222356" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/da186c3b-3e03-4c2c-9186-cd3a1d17d7ca">


- 언더샘플링을 통한 타겟 분포 조정
- Logistic Regression통한 학습
- 이때 `solver = 'liblinear', L2 패널티, C = 1`
- test

<img width="565" alt="under_test" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/f8bb71a2-73a1-46b0-ba0e-cb8c71cee862">

- validation

<img width="565" alt="under_val" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/6d3caa7f-5d35-497e-ae6d-85abe3dff394">

### 5. 다중 공선성 제거
- OLS 수치 확인, VIF 수치 확인 후 다중 공선성 제거
- 총 6개의 피처 삭제
> MinTemp   
> MaxTemp   
> Rainfall  
> Cloud9am  
> Temp3pm
> Pressure3pm

- Logistic Regression을 통한 학습
- 이때 `solver = 'liblinear', L2 패널티, C = 1`
- test
  
<img width="560" alt="cln_test" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/be5a1faa-6a42-4f90-a18a-7c06a2fdf96e">

- validation

<img width="559" alt="cln_val" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/5a8f682b-6ed5-4c05-b64c-1937c1338210">

### 6. 차원 축소
- LDA를 통한 차원 축소 후 학습 진행
- 이중 분류이므로 차원은 1차수로 축소
- 보존율 : 1.0

### 7. 각 모델 별 점수 확인
- 차원 축소의 보존율이 높으므로, 차원 축소와 각 모델의 파이프라인 구축 후 학습
- RandomForest Classifier

<img width="560" alt="rfc" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/bd62127d-dd2c-438e-bfda-a64986be718d">

- DecisionTree Classifier 

<img width="558" alt="dtct " src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/6215a89f-dddb-44c0-bb63-a0cd9e283950">

- K Neighbors Classifier

<img width="557" alt="knnt" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/8c81dfd3-bfbd-42cb-8c9e-7bebecb97ea3">

- Hard Voting (DTC, KNN)

<img width="557" alt="hardt" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/e3b890f5-d702-4aea-aeea-1b0e5e49b1dd">

- Soft Voting (DTC, KNN)

<img width="557" alt="softt" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/d4480769-86e2-406f-ad42-7e832656ec35">

- AdaBoost Classifier

<img width="559" alt="adat" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/2f44f6ad-0955-42e1-bd64-007a242e4165">

- GradientBoosting Classifier

<img width="566" alt="gbct" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/28a8547e-3ed8-4e5b-b080-da9c2ee64672">

- XGB Classifier

<img width="556" alt="xgbt" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/a9c26c29-b364-4d3f-968d-7a89fffc31e8">

-Light GBM Classifier

<img width="557" alt="lgbmt" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/1193cb83-f9a8-4b45-a321-432ea2b04fd8">

- Logistic Regression

<img width="557" alt="lrt" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/8e0d4358-bd2f-4616-b4d7-b68acc01e33c">

### 8. 최적의 모델 찾기
- 전체 모델 학습 결과 Logistic Regression이 근소하지만 전반적으로 높은 수치를 보임
- test

![다운로드](https://github.com/DianaKang0123/machine_learning_project/assets/156397873/22286319-f0cc-4886-b54d-a6b5352c2fc2)

- validation

![다운로드 (1)](https://github.com/DianaKang0123/machine_learning_project/assets/156397873/72a1ff90-9485-450c-9609-742b68ddc69f)

### 9. 교차검증
- 해당 모델의 과적합을 확인하기 위해 K-Fold를 통하여 교차검증
- 교차 검증: 0.7854
- 실제 예측 정확도: 0.7887

### 10. 결론
- 임계치 확인을 위하연 시각화를 진행, 약 0.5에서 정확도와 재현율이 교차하는 것을 확인
- 해당 모델은 정밀도와 재현율 모두 중요하므로 임계치를 따로 조정하지 않음

<img width="529" alt="스크린샷 2024-05-15 223521" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/483d17b3-4f45-4d7d-8f6d-540e2a2c7c22">

- ROC Curve

<img width="461" alt="스크린샷 2024-05-15 223544" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/7acf6cca-4d88-4dac-8878-a4f1d17663e0">
