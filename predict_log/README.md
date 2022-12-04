## 접근 방식

- 7을 구별하기 위해서 비지도 학습으로 접근을 시도했고 오토인코더, one-class-svm, isolated-randomforest 등의 비지도 학습 방법을 적용해 보았으나 기존과 단어 몇개만 다른 경우들이 7 특성 떄문인지 성능이 좋지 않음
- 그 중에서 오토인코더가 성능이 좋았음(참고 : https://dacon.io/competitions/official/235717/codeshare/2677?page=1&dtype=recent)

- 로그 데이터의 전처리를 진행하고 Baseline에서 Validation을 통해 등급별로 threshold를 상세 설정한 점수가 가장 높음
- Threshold 7을 잡았지만, Train 문장과 완전히 동일한 문장이 존재하는지 여부를 체크해주는 컬럼을 통해 문장 전체가 완전히 동일한 경우에는 Threshold 에 걸리지 않도록 함

- 마지막으로 점수를 조금 향상하기 위해서 7 등급 이외의 등급은 전처리가 덜 진행된 데이터를 활용하여 베이스라인 지도학습을 다시 진행(지나친 전처리는 0~6등급 지도 학습 분류에 악영향이라고 판단)

실제 서비스에서 Line by Line으로 들어왔을 때의 분류방법:

    1. 로그 문장 데이터 전처리 진행 (Cleaning)
    2. train 데이터와 완전히 동일한 문장이 있는지 확인하여 서로 다른 모델 적용(A방법, B방법)
    3. 동일한 문장이 있다면 0~6 등급 지도 학습된 모델로 Predict(Threshold 적용 X) (A방법)
    4. 동일한 문장이 없다면 0~6 등급 지도 학습된 모델로 Predict 이후에 설정된 Threshold 를 통해 7은 따로 분류 (B방법)
    5. 4번에서 Threshold를 통해 걸러지지 않는다면 3번과 동일한 모델로 predict 적용 (A방법)
    
[참고]
https://dacon.io/competitions/official/235717/codeshare/2536?page=1&dtype=recent
https://dacon.io/competitions/official/235717/codeshare/2539?page=1&dtype=recent 
https://dacon.io/competitions/official/235717/support/403102?page=1&dtype=recent 

