## USA Residential Building Energy Consumption
![image](https://github.com/DianaKang0123/machine_learning_project/assets/156397873/499e8e6f-3f96-4b51-bd61-84fea2751689)

- 미국 주거 에너지 소비 조사(RECS)
- Features : 759
- TARGET : TOTALDAL(총 사용 에너지 소비 금액)

---

## 🤓 목차
1. 데이터 전처리
2. 연속형 피처 분석
3. 범주형 피쳐 합산 분석
4. 이상치 제거
5. 차원 축소
6. 파이프 라인을 통한 학습
7. 다항 회귀 분석
8. 최적의 분석방법 도출
9. 성능 검증
10. 결론

---

### 1. 데이터 전처리
- 데이터 결측치가 있는 'NGXBTU' 피처 삭제
- 타겟 데이터 분포 확인
<img width="431" alt="스크린샷 2024-05-15 172339" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/f4f16249-1214-4865-b568-1a591eafd2a0">

<br>
<br>
<br>

### 2. 연속형 피처 분석
- Linear Regression 을 통한 학습 결과
> test = MSE: 0.5563, RMSE: 0.7458, **R2: 1.0000**
> 
> val = MSE: 0.0000, RMSE: 0.0041, **R2: 1.0000**

- 파이토치를 통한 확인
<img width="562" alt="스크린샷 2024-05-15 172607" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/09f9dffa-4121-4da7-ac94-c5c2dc50d0f0">

- R2 점수가 1.0으로 완벽한 예측을 하고 있으나 추가적인 전처리 과정을 통해 수치 조정예정

<br>
<br>
<br>

### 3. 범주형 피쳐 합산 분석
- 총 4개의 범주형 피처를 라벨인코딩과 함수를 통한 변환을 하여 연속형으로 변경 후 분석 진행
- Linear Regression 을 통한 학습 결과
>  test = MSE: 0.0000, RMSE: 0.0039, **R2: 1.0000**
> 
> val = MSE: 0.3530, RMSE: 0.5941, **R2: 1.0000**

- 연속형 피쳐만 분석했던 것과 동일하게 R2점수가 1.0으로 나타남

<br>
<br>
<br>

### 4. 이상치 제거
- standard scaler 를 통하여 총 5,686개의 데이터에서 5,113개로 스케일링을 진행
- Linear Regression 을 통한 학습 결과
>  test = MSE: 0.0000, RMSE: 0.0039, **R2: 1.0000**
> 
> val = MSE: 0.3530, RMSE: 0.5941, **R2: 1.0000**

- 위와 동일하게 R2점수가 1.0으로 나타남

<br>
<br>
<br>

### 5. 차원 축소
- 피처의 갯수가 많으므로 차원 축소를 통하여 분석을 진행
- PCA를 통하여 5개의 차원으로 축소
- 차원 축소를 통한 보존률 : 0.6107
- 각 피처 1,2 / 피처 3,4 / 피처 1,5 별 분포 그래프 확인
<img width="825" alt="스크린샷 2024-05-15 173422" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/1a03a8e2-1015-4071-ba76-90983585c8a7">

<br>
<br>
<br>

### 6. 파이프 라인을 통한 학습
- standard scaler, PCA, Linear Regression을 통한 학습
- 이때, 차원 축소의 n_conponents 의 값은 5
>  test = MSE: 403886.0377, RMSE: 635.5203, **R2: 0.5318**
> 
> val = MSE: 473603.4948, RMSE: 688.1886, **R2: 0.4814**

- 스케일링을 진행하고 차원을 축소한 결과 R2 점수가 확연하게 감소하여  스케일링을 제외 후 재 학습

- PCA, Linear Regression을 통한 학습
- 이때, 차원 축소의 n_conponents 의 값은 5
>  test = MSE: 187731.8207, RMSE: 433.2803, **R2: 0.7824**
> 
> val = MSE: 186798.8171, RMSE: 432.2023, **R2: 0.7955**

- 스케일링을 제외하고 학습한 결과 R2점수가 상승
- 다항 회귀를 통한 학습 후 결과를 보기로 함

<br>
<br>
<br>

### 7. 다항 회귀를 통한 학습
- Polynomial를 적용한 PCA, Linear Regression을 통한 학습
>  test = MSE: 274151.0699, RMSE: 523.5944, **R2: 0.6822**
> 
> val = MSE: 306689.6564, RMSE: 553.7957, **R2: 0.6642**

- 다항 회귀를 통한 학습 시 R2점수가 줄어드는 것을 보아 해당 데이터는 선형 분석이 적합하다고 판단

<br>
<br>
<br>

### 8. 최적의 분석방법 도출
- PCA, Linear Regression을 통한 분석, 이때 차수를 늘리는 방향으로 시도
- `n_components = 15`
>  test = MSE: 172932.6324, RMSE: 415.8517, **R2: 0.7995**
> 
> val = MSE: 164965.5654, RMSE: 406.1595, **R2: 0.8194**

- 15로 차수를 늘렸을 때 적정한 수치라고 판단, pytorch를 통한 과적합 확인
<img width="565" alt="스크린샷 2024-05-15 174331" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/6dda543d-1535-40a2-a66a-da33ad4291c3">

- 각 Cycle의 R2 결과 시각화
- Test

<img width="218" alt="test table" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/4a56fe0a-045f-42df-9de4-c573dfbb8ea8">

<img width="560" alt="test " src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/785512db-9354-4a45-ae4e-0a2725f4fc93">


- Validation

<img width="215" alt="val table" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/118728a6-c903-4928-82a0-3bc6ddf1e96a">

<img width="563" alt="val" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/e2941977-f8a7-49a8-a63f-3e7b304af178">

<br>
<br>
<br>

### 9. 학습 성능 시각화
- train
<img width="461" alt="train" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/f5cb45bb-981d-4a82-8dbb-888decbeabe2">

- test
<img width="446" alt="test" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/9af5340b-90bf-4421-8e99-50c3f86d4d3e">

- validation
<img width="449" alt="validation" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/f929dc76-866f-4021-bc97-b48eaa37edbe">

<br>
<br>
<br>

### 10. 결론

**최종 점수**

<img width="253" alt="final" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/53014b23-4978-435c-ad21-dff692e43cd6">

<img width="558" alt="fianl--" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/7717cffd-d3a3-4f81-aa2a-0ccf4f1851d0">
