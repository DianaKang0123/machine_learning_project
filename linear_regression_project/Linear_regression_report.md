## fifa 게임 선수캐릭터 가치 예측

![image](https://github.com/DianaKang0123/machine_learning_project/assets/156397873/70ada97d-c9e0-4561-bc0f-c69a498d93c4)

- Features: 89
- target : Value (선수의 금전적 가치)

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

- 결측치 삭제로 결측치 처리
- 결측치 비율이 높고, 연관성이 없는 'Lorned From' 칼럼 삭제
- 학습과 연관없는 'ID','Photo', 'Flag','Club Logo' 칼럼 제거
- weight, height, value, wage, release clause 항목 단위 제거 후 연속형으로 변환


<br>
<br>
<br>

### 2. 연속형 피처 분석
- Linear Regression을 통한 학습 결과
> test : MSE: 61741063549.3715, RMSE: 248477.4910, R2: 0.7999
>
> validation : MSE: 65135924899.3173, RMSE: 255217.4071, R2: 0.8256

- 특별한 전처리 없이 0.8정도의 R2점수를 보임
- 범주형 피처를 합산하여 분석한 후 결과확인

<br>
<br>
<br>

### 3. 범주형 피쳐 합산 분석
- 범주형 데이터 중 'Body Type', 'Work Rate' 는 함수를 통하여 인코딩
- 'Preferred Foot', 'Position'은 라벨 인코딩을 통하여 인코딩
- Linear Regression을 통한 분석결과
> test : MSE: 61771632969.7934, RMSE: 248538.9969, R2: 0.7619
>
> validation : MSE: 61354177368.7408, RMSE: 247697.7541, R2: 0.8025

<img width="500" alt="스크린샷 2024-05-15 220653" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/2ac678b4-e428-4631-b869-97d5bc81cefd">

- OLS를 통한 다중 공선성 제거
- p-value 가 0.05 이상인 'Preferred Foot' 외 14개 항목 삭제
- Linear Regression을 통한 분석결과
> test : MSE: 61672459081.4570, RMSE: 248339.4030, R2: 0.7623
>
> validation : MSE: 61479017844.9167, RMSE: 247949.6276, R2: 0.8021

- 다중 공선성 제거 전과 근소한 차이를 보임

<br>
<br>
<br>
  
### 4. 이상치 제거
- standard scaler를 통하여 16,166개의 데이터를 9,832개로 조정
- 이상치 제거 후 선형 회귀 학습 결과
> test : MSE: 57004456233.2967, RMSE: 238756.0601, R2: 0.1378
>
> validation : MSE: 54955034148.6600, RMSE: 234424.9009, R2: 0.1413


<img width="558" alt="스크린샷 2024-05-15 220825" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/b464fca7-17e6-4abb-b8e1-1a42c2b7bcb7">  


- `describe()`를 통해 이상치가 있는 칼럼만 이상치를 제거
- 'Wage', 'Release Clause' 두개의 피처의 이상치 제거
- 16,636개의 데이터에서 15,943개의 데이터로 조정
- 부분 이상치 제거 후 선형 회귀 학습 결과
> test : MSE: 49708148490.1766, RMSE: 222953.2428, R2: 0.3106
>
> validation : MSE: 51391272484.6929, RMSE: 226696.4324, R2: 0.2690

- 전체 이상치를 제거 한 것 보다는 R2 점수가 상승하였지만, 이상치 제거 전과 비교하여 점수가 확연히 감소

<br>
<br>
<br>

### 6. 차원 축소
- 전처리 후 피처가 36개 이므로 차원 축소를 통한 학습을 진행
- PCA를 통하여 2차원으로 차원 축소
- 보존률 : 0.9999

<img width="490" alt="스크린샷 2024-05-15 220858" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/9a6fb94f-f45b-4227-80d1-63487c177e98">


<br>
<br>
<br>

### 7. 파이프 라인을 통한 학습
- PCA, Linear Regression
- n_components = 2
> test : MSE: 63515486015.1438, RMSE: 252022.7887, R2: 0.7956
>
> validation : MSE: 64981988012.5874, RMSE: 254915.6488, R2: 0.7495

- Standard Scaler, PCA, Linear Regression
- n_components = 2
>  test : MSE: 263973364041.7213, RMSE: 513783.3824, R2: 0.1504
>
> validation : MSE: 218844533892.5767, RMSE: 467808.2234, R2: 0.1565

- standard scaler 를 적용한 결과 R2 점수가 확연하게 감소

<br>
<br>
<br>

### 8. 다항 회귀 분석
- Polynomial features를 적용하여 PCA, Linear Regression 학습
> test : MSE: 100807449242.0323, RMSE: 317501.8886, R2: 0.6114
>
> validation : MSE: 105423802684.5129, RMSE: 324690.3181, R2: 0.6607

- 다항 회귀를 통한 학습을 실행한 결과 R2 점수가 감소

<br>
<br>
<br>

### 9. 최적의 분석방법 도출
- test

<img width="278" alt="test_mode" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/fad72346-2af2-4443-a591-8cf1616b24ce">

<img width="560" alt="test_visual" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/338fc4bd-179c-4710-8275-31bd07ef7463">


- validation

<img width="275" alt="val_mode" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/cb245765-a685-4dca-9a76-cd6f25281461">

<img width="560" alt="val_visual" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/498c06a7-fea2-4144-b5af-d2cea7ef62da">

- 2차수로 차원 축소를 통한 선형 회귀 학습이 가장 효과적이므로 해당 방식을 채택

<br>
<br>
<br>

### 9-2 pytorch를 통한 과적합 방지

- 2차원 차원 축소를 통한 선형회귀 학습의 loss function 그래프

<img width="460" alt="스크린샷 2024-05-15 220918" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/1a8cbb83-f93e-46f5-999f-20291bb45402">

- 과적합이 없다고 판단되어 성능을 검증

<br>
<br>
<br>

### 10. 성능 검증

- train

<img width="466" alt="train" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/96eb83b5-946e-4469-8a6b-019f7ccd209f">

- test
  
<img width="440" alt="test" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/42544251-d126-4305-a83f-fc66405683b5">

- validation

<img width="454" alt="val" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/56478ad6-8c9f-46e5-9f9d-8d246167d5c5">

<br>
<br>
<br>

### 11. 결론

<img width="263" alt="result" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/4aebb75f-9cce-4967-a706-8150fd7d4fa6">  

<img width="560" alt="result_visual" src="https://github.com/DianaKang0123/machine_learning_project/assets/156397873/217b4309-ebc0-4822-bfdd-6dcb5a3c5295">
