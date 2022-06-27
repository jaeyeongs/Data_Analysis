# KMAC 빅데이터 실전 프로젝트

*빅데이터 분석 Case Study & 실전프로젝트*

*: 2021.10.16 ~ 2021.11.20*

- 수료증

![Inked143247983-7fef2a1a-86ed-4da2-8567-e2df493901a2](https://user-images.githubusercontent.com/87981867/143873259-3c5a411f-6085-4d93-8924-965122f87363.jpg)


## 구매 기록 기반 상품 추천하기

### <STEP1. 탐색적 분석>

: 유저별 구매 패턴 분석

[UK Retail 데이터셋]

- InvoiceNo : 거래 고유 번호
- StockCode : 상품 고유 번호
- Description : 상품명
- Quantity : 거래 수량
- InvoiceDate : 거래 일시
- UnitPrice : 상품 단가
- CustomerID : 구매자 고유 번호
- Country : 구매 국가

### <STEP2. 예측 분석>

: SVD를 활용한 상품 구매예측

#### (1) 알고리즘 선택 : SVD

*surprise : 파이썬 기반의 추천 시스템 구축을 위한 전용 패키지*

[Surprise 추천 알고리즘]

![image](https://user-images.githubusercontent.com/87981867/143381333-e027446d-6c8f-47ff-aa4d-67f2e67aa98d.png)

#### (2) SVD 모델 학습

![image](https://user-images.githubusercontent.com/87981867/143381647-c56a311d-fa5e-4e11-8e50-62bcd251b1a1.png)

[SVD 모델 파라미터 설정]

- n_factors=8
- lr_all=0.005
- reg_all=0.02
- n_epochs=200

[모델 성능 평가]

- RMSE : 0.338
- 0에 가까울 수록 좋으나, 평균과 같이 고려해야 함

### <STEP3. 예측 평가하기>

#### (1) 상품 추천 시뮬레이션

*1. 이전에 구매하지 않았던 상품 추천 : anti_build_testset()을 사용*

*2. 이전에 구매했던 상품 다시 추천 : build_testset()을 사용*

*3. 모든 상품을 대상으로 하여 상품 추천*

#### (2) 상품 추천 결과

![image](https://user-images.githubusercontent.com/87981867/143382309-c122564a-18eb-4f9e-81e5-cfd0117e7b3c.png)

- 고객에 따라 실제 주문과 5개의 상품 추천결과를 비교해서 나타냄
- 제대로 상품을 추천했는지에 대해 재현도를 평가 점수로 나타냄
- 추천한 상품에 대해 평가 점수가 매우 좋지 않음
- 모델 성능 개선이 
