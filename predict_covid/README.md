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
