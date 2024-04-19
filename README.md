# LG AIMERS 4기 해커톤 본선
### 교육명 : LG Aimers / Data Intelligence
### 주제 : MQL 데이터 기반 B2B 영업 기회 창출 예측 모델 개발

![image](https://github.com/JHZLO/lgAimers/assets/105791673/94e731b5-f790-4951-ab82-8dd99ac7d734)
- public score 14등

![image](https://github.com/JHZLO/lgAimers/assets/105791673/539e8783-f5e0-4e5b-8afd-44e7066537e3)
- private score 7등

- 노션 템플릿을 이용하여 협업

  참고: https://www.notion.so/24-LG-Aimers-LG-640b68d15392436aa57a15bed1f00195?pvs=4

### 성능을 올리는 데에 있어서 핵심
- 점수가 크게 오른 feature에 대한 전처리

  - expected_budget: 문자열 -> 숫자로 바꾸는 전처리
  
  - lead_date : 년/월/일 파생변수 추가
  
  - 결측치 도메인 지식을 바탕으로 적합한 방식으로 채워주기
  
- 중요도가 낮은 feature에 대해서는 과감한 drop

- catboost stratified k fold 사용

### 아쉬웠던 점
- transformer를 사용한 데이터 클리닝,  
  feature selection을 통한 파생변수 생성,  
  EDA 분석을 바탕으로 진행한 스케일링 및 이상치 제거,  
  도메인 지식을 바탕으로 진행한 파생변수 혹은 상위(하위) 변수 생성,  
  soft voting / hard voting과 같은 앙상블 기법을 적용  
  와 같은 다양한 방식으로 전처리를 시도하였고, 이론상으로는 충분히 성능 향상을 이끌 것 같았지만 적용 후 막상 점수가 낮게 나와 사용하지 못한 점이 정말 아쉽다..  
  결국 대회 막바지에는 이러한 옵션들을 하나 하나 적용하는 방식으로 계속 테스트하고 점수 측정하는 방식으로 진행하여 점수가 가장 잘 나오는 경우의 수를 추려냈다.

- 1등팀의 전처리의 핵심은 도메인 지식의 활용과 feature importance를 바탕으로 중요한 feature에 대한 파생변수를 다양하게 생성했다는 점이였는데, 우리팀도 마찬가지로 중요 feature를 바탕으로 파생변수를 생성하였지만
  결정적인 차이점이 있다면 1등팀의 파생변수 생성은 통계적인 기법을 활용했다는 점이고 우리팀은 단순하게 단어를 조합시켰다는 점이다.  
  ex. 1등팀 : lead_owner_job : lead_owner가 데이터셋 상 "얼마나 많이" 등장한 숙련된 담당자인지
      우리팀 : lead_owner_idx : lead_owner + customer_idx
  1등팀은 이러한 "얼마나 많이"와 같은 느낌으로 통계적으로 접근하여 생성한 파생변수들이 다양했다.

- 예선 코드를 참고해보면, 하드코딩을 굉장히 많이 진행하였지만 본선 때는 하드코딩이 실격의 사유가 될 수 있어서 하드코딩이 진행된 부분은 전부 제거하였다.

- 가중치를 부여하는 column에 대해서는 과감하게 drop하였지만, 상위팀들은 어느 변수에 가중치를 부여하는지 파악하고, 해당 변수들과 다른 변수들간의 차별성을 두었다

- feature importance가 너무 한쪽에 몰리는 것이 아닌 여러 파생변수를 생성하여 고루 분포하게 만들어 모델을 robust하게 만드는 것이 중요한 것 같다

- label의 불균형을 해소하기 위해 catboost의 하이퍼파라미터 class_weight를 사용하여 모델 학습을 진행하였지만, 1등팀처럼 k fold를 진행할 때 label의 불균형의 비율에 맞게 fold를 진행하여 학습을 하는 것도 모델을 보다 더 robust하게 만드는 데에 효율적일 것 같다.
  
