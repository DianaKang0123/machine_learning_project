## 파키스탄 중고차 가격 예측
![image](https://github.com/DianaKang0123/machine_learning_project/assets/156397873/f9a5a149-4229-4248-a84d-9f46996441dd)

- features: 14
- target: price

---

### 🤓 목차

1. 데이터 전처리
2. 연속형 피쳐 분석
3. 다항 회귀, Tree를 통한 비선형 회귀 분석 (적정모델 선정)
4. 범주형 피처 포함 분석
5. 타겟 분포 조정
6. 이상치 제거
7. 차원 축소
8. 최종 모델 선정
9. pytorch를 통한 과적합 확인
10. 성능 검증
11. 결론

---
<br>
<br>
<br>

### 1. 데이터 전처리
- 'assembly' 피처의 값이 하나이므로 결측치는 다른 문자열로 대체 (domestic)
- 이 외 항목은 `dropna`로 처리

<br>
<br>
<br>
 
### 2. 연속형 피쳐 분석
- 연속형 피처를 추출하여 학습 진행

<img width="557" alt="스크린샷 2024-05-15 223917" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/15c22c7f-5f97-414d-9e88-b55a4cb9676c">

- Linear Regression을 통한 학습
> test : MSE: 14646732672425.7812, RMSE: 3827104.9989, R2: 0.4568
>
> validation : MSE: 15715581258704.0664, RMSE: 3964288.2411, R2: 0.4496

- 선형 회귀에 대하여 낮은 R2 점수를 보이므로 다항 회귀 진행

<br>
<br>
<br>

### 3. 다항 회귀, Tree를 통한 비선형 회귀 분석 (적정모델 선정)
- Polynomial feature을 통하여 선형 회귀 학습 

> test : MSE: 6295554347490.7139, RMSE: 2509094.3281, R2: 0.7489
>
> validation : MSE: 9535269733684.9258, RMSE: 3087923.2072, R2: 0.6161

- 다항 회귀 분석 시 R2점수가 소폭 상승
- 다양한 모델을 통하여 비선형 회귀 학습 진행
> DecisionTreeRegressor => R2: 0.5554
> RandomForestRegressor => R2: 0.7455
> GradientBoostingRegressor => R2: 0.7142
> XGBRegressor => R2: 0.6690
> LGBMRegressor => R2: 0.7289
- Random Forest Regressor이 가장 높은 R2점수를 보임

<br>
<br>
<br>

### 4. 범주형 피처 포함 분석
- 'body' 항목은 항목 내 값을 크기별로 분류 후 5개의 등급으로 인코딩
- 외 범주 항목은 라벨인코더를 통한 인코딩 진행

- 다양한 모델을 통하여 비선형 회귀 학습 진행
> DecisionTreeRegressor => R2: 0.5874
> RandomForestRegressor => R2: 0.7721
> GradientBoostingRegressor => R2: 0.7184
> XGBRegressor => R2: 0.7317
> LGBMRegressor => R2: 0.7548

- 전체 모델의 R2 점수가 소폭 상승, Random Forest Regressor가 여전히 가장 높은 점수를 보임
- 따라서 해당 모델을 채택하여 학습을 진행한 결과
> test : MSE: 6144498918054.2695, RMSE: 2478809.9802, R2: 0.7721
>
> validation : MSE: 4795831010683.0967, RMSE: 2189938.5861, R2: 0.8320

<br>
<br>
<br>

### 5. 타겟 분포 조정
- 타겟 분포 조정을 통하여 정확도가 높아지도록 전처리
- log를 통한 데이터 분포 조정

<img width="555" alt="스크린샷 2024-05-15 224024" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/c541fc71-4f61-433a-ad1d-8932e338dd3e">

- 분포 조정 후 Random Forest Regressor 학습 결과
> test : MSE: 0.0451, RMSE: 0.2125, R2: 0.9326
>
> validation : MSE: 0.0447, RMSE: 0.2115, R2: 0.9337

- 결과가 눈에 띄게 좋아졌으므로, 분포를 조정하여 학습 진행

<br>
<br>
<br>

### 6. 이상치 제거
- 이상치를 제거하여 학습
- 전체 63,370개의 데이터에서 48,772개로 스케일링

<img width="555" alt="std" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/02e85ffe-57a5-40cb-a1cd-8b443027dc7a">

- Random Forest Regressor 학습 결과
> test : MSE: 0.0378, RMSE: 0.1944, R2: 0.9221
>
> validation : MSE: 0.0380, RMSE: 0.1950, R2: 0.9205

- R2 점수는 이상치 제거 전보다 낮아졌지만, 여전히 높은 점수를 보이므로 신뢰도가 더 높은 이상치 제거 상태를 유지하고 학습 진행

<br>
<br>
<br>


### 7. 차원 축소
- 차수를 2차원으로 축소하여 학습을 진행
- 보존율 : 0.9999

<img width="370" alt="스크린샷 2024-05-15 224144" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/b62f7de0-a810-40e8-9cdf-c70ccb9b4100">

-  파이프 라인을 통하여 차원 축소 및 Random Forest Regressor 학습
> test : MSE: 0.6966, RMSE: 0.8346, R2: -0.0408
>
> validation : MSE: 0.7021, RMSE: 0.8379, R2: -0.0417

- 차원을 축소한 결과 보존율이 높음에도 불구하고 R2점수가 마이너스를 띄므로 차원을 축소하지 않는 모델을 채택한다.

<br>
<br>
<br>

### 8. 최종 모델 선정

- test
<img width="283" alt="test_mode" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/166c1fe8-5438-4b6a-8424-aac16083b7d8">

<img width="556" alt="test_visual" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/b09cfa97-e2b5-4e22-9a8f-ce284e7f1a25">

- validation

<img width="280" alt="val_mode" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/055fb8ec-fd20-4fdf-850c-cff88806d5e7">

<img width="559" alt="val_visual" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/a13a2045-2726-4dcf-a405-90eb2c4b1487">

- 분포 조정 후 Random Forest Regressor을 통한 학습을 최종 모델로 채택
- 과적합의 우려가 있으므로 pytorch를 통하여 확인

<img width="571" alt="스크린샷 2024-05-15 224202" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/9e84bc48-cb15-44c2-a0d0-bf03e36163d7">

- 과적합이 없다고 판단하여,  성능 검증을 진행

<br>
<br>
<br>

### 9. 성능 검증
- train

<img width="432" alt="train" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/1f22a0ad-4790-4c10-a060-434236647702">

- test

<img width="425" alt="test" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/c2b46101-6849-42c7-8ebf-cfe0e4614c51">

- validation

<img width="430" alt="val" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/bd9da302-9619-4186-a7f5-5bc40d030a42">

<br>
<br>
<br>

### 10. 결론

<img width="217" alt="result" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/52a1ee7b-1ab1-48f0-b908-002eb4799c01">

<img width="559" alt="result_vi" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/d9b8c5bd-4b7b-4e9a-9962-666a046a41af">
