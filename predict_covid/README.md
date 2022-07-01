## Feature Engineering을 통한 코로나 확진자 수 예측

[과제1] 2022.06

아래의 공공데이터 포털에 공개된 코로나관련 데이터를 판다스를 활용하여 Feature Engineering을 수행

*조건사항*

(1) 데이터 - https://drive.google.com/file/d/1F5UAuqEruAlsma4LtjWkRbJXr2cYJzvV/view?usp=sharing

(2) 데이터 정보 - https://www.data.go.kr/tcs/dss/selectApiDataDetailView.do?publicDataPk=15043378

(3) 2021년 2월 1일 부터 2022년 4월 17일까지의 코로나 데이터를 참고

(4) 지역 별로 구분된 코로나 확진자수,사망자 수 등의 데이터 중에, gubunEn 컬럼 값이 Total 인것을 활용(즉, 전국 합계만 활용)

(5) 일자별 코로나 확진자 수를 예측하는 기계학습 알고리즘 개발

(6) 기계학습 알고리즘은 sklearn의 linear model 중 Ridge Regression을 사용

(7) 다양한 Feature Engineering 및 Feature Selection 기법을 활용하여, 최대한 모델의 test_score를 향상하기 바랍니다.

(8) training set은 2021년 2월 1일 부터 2022년 1월 31일까지의 데이터를 활용하고, test set은 2022년 2월 1일 부터 2022년 4월 17일까지의 데이터를 활용하여 가장 높은 정확도를 얻어 보세요.


```
# 기계학습 코드 샘플
from sklearn.linear_model import Ridge
import numpy as np
import pandas as pd
df = pd.read_csv('.....')


X_train, Y_train, = df [ .... ]    # 제시된 훈련 데이터를 선별
X_test, Y_test = df [ ........ ]   # 제시된 테스트 데이터 선별
model = Ridge()
score = model.fit(X_train, Y_train).score(X_test, Y_test)
print("Test Score : {:.4f}".format(score))
```


### 1. Data Processing

* createDt : 등록 날짜
* deathCnt : 사망자 수
* defCnt : 확진자 수
* gubunEn : 시도명(영어)
* incDec : 전일대비 증감 수
* localOccCnt : 지역발생 수
* overFlowCnt : 해외유입 수
* qurRate : 10만명당 발생률
* seq : 게시글번호
* stdDay : 기준일시
* updateDt : 수정일시분초

![image](https://user-images.githubusercontent.com/87981867/176192146-e63188c6-9200-46bd-955e-95cf12ca3eb7.png)

- 의미없는 변수 제거
- gubunEn 변수가 Total인 데이터만 추출
- 날짜 데이터(createDt) datetime 변환

![image](https://user-images.githubusercontent.com/87981867/176192773-bcd10715-a717-4349-a1c0-ba1c673da778.png)

### 2. Correlation Analysis

#### 상관분석(Correlation Analysis)
: 두 변수간에 어떤 선형적 관계를 가지는지 분석하는 기법으로 상관계수를 이용하여 측정

#### 상관계수(Correaltion Coefficient)

- 상관계수 r = X 와 Y가 함께 변하는 정도 또는 X와 Y가 각각 변하는 정도
- 상관계수(r)은 1 또는 -1에 가까울 수록 두 변수가 매우 상관이 있음
- 상관계수가 0이면 상관이 없다는 것보단 선형의 상관관계가 아니다는 것이 정확

![image](https://user-images.githubusercontent.com/87981867/176677825-c37ec6dd-1ad4-40bb-a6e9-c837d9704786.png)

![image](https://user-images.githubusercontent.com/87981867/176678343-3c47e421-412e-4fd8-8a8d-440248aafcf2.png)

- 확진자 수(defCnt)와 가장 상관있는 변수는 deathCnt, incDec, localOccCnt 이다. 


### 3. Multi Regression Analysis

#### 다중회귀분석(Multi Regression Analysis)

- 두 개 이상의 독립변수들과 하나의 종속변수의 관계를 분석하는 기법
- 독립변수 : 영향을 미칠 것으로 생각되는 변수 -> 확진자 수(defCnt)를 제외한 모든 변수
- 종속변수 : 영향을 받을 것으로 생각되는 변수 -> 확진자 수(defCnt)

![image](https://user-images.githubusercontent.com/87981867/176905942-79acad11-e212-4afa-801e-d2122981ad32.png)

![image](https://user-images.githubusercontent.com/87981867/176905996-2a4e8b58-f3a7-45c7-a25b-62da4b703a31.png)

- R-squared or Adj.R-squared : 데이터를 통해 현재 모델이 얼마나 잘 설명하고 있는지 나타내는 지수(보통 1에 가까울 수록 높은 설명력)
- Prob(F-statistic) : 모델에 대한 p-value(보통 0.05 이하인 경우 통계적으로 유의)
- P>[t] : 각 독립변수에 대한 p-value(보통 0.05 이하인 경우 통계적으로 유의)
- R-squared : 1.00 이므로 독립변수들의 설명력 충분
- 하지만 qurRate 변수를 제외하고는 p-value가 통계적의로 유의하지 않음
