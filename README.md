# LEAGUE OF LEGEND : 인게임 데이터분석을 통한 승패예측
## 프로젝트 목표
- 직접 선택한 데이터셋을 사용하여 ML예측모델을 통한 성능 및 인사이트 도출/공유
- 데이터셋 전처리/eda부터 모델을 해석하는 과정을 설명하는 영상 작성

## 프로젝트 절차 
### 1. 데이터 선정 이유 및 문제 정의
- 해당 게임데이터셋은 다른 데이터셋들과 달리 다이아1티어 이상 티어게임의 초반 10분간의 데이터
- League of Legend게임은 초반에 이득을 쌓는 것이 중요한 전략게임이며 상위티어의 게임일수록 그 영향력이 커지기 때문에 유의미한 결과를 얻을 수 있을거라 보여짐
- 승리와 패배(1,0)로 나누어진 타겟을 선택하여 분류문제로 접근
- 데이터 설명
  - gameId : 게임의 고유 ID
  - blueWins : 승리 여부(1,0)
  >- blueWardsPlaced : 설치 와드 수
  >- blueWardsDestroyed : 블루팀이 파괴한 적 와드 수
  >- blueFirstBlood : 첫 킬(블루:1, 레드:0)
  >- blueKills : 블루팀이 죽인 적의 수
  >- blueDeaths : 블루팀이 죽은 수
  >- blueAssists : 블루팀의 어시스트 수
  >- blueEliteMonsters : 블루팀이 죽인 엘리트몬스터 수
  >- blueDragons : 블루팀이 죽인 드래곤 수(1,0)
  >- blueHeralds : 블루팀이 죽인 전령(1,0)
  >- blueTowersDestroyed : 블루팀이 파괴한 타워 수
  >- blueTotalGold : 블루팀 총 골드획득량
  >- blueAvgLevel : 블루팀 평균 챔피언 레벨
  >- blueTotalExperience : 블루팀 총 경험치량
  >- blueTotalMinionsKilled : 블루팀 총 미니언 처치 수
  >- blueTotalJungleMinionsKilled : 블루팀 총 정글몬스터 처치 수
  >- blueGoldDiff : 블루팀과 적팀의 골드 차이
  >- blueExperienceDiff : 블루팀과 적팀의 경험치 차이
  >- blueCSPerMin : 블루팀의 분당 미니언 처치 수
  >- blueGoldPerMin : 블루팀의 분당 팀 골드획득량
  - red도 동일한 19개의 feature 존재
  - 9879 rows × 40 columns
  
### 2. 데이터를 이용한 가설 및 평가지표, 베이스라인 선택
- 타겟데이터인 승,패의 비율이 5:5정도로 보여져 50%의 베이스라인 선택
- 평가지표 : Accuracy

### 3. EDA 및 데이터 전처리
- 이상치, 중복치 제거
- 타겟과 킬,데스,와드 등과의 분포 확인

![image](https://user-images.githubusercontent.com/39218451/221573903-9fd65ddf-67cc-4160-aad3-2c246f969234.png)

- feature engineering을 통해 새로운 지표(kda) 생성 및 중복적인 의미를 가지는 지표들 제거(다중공선성 피하기 위함)

### 4. 머신러닝 방식 적용 및 교차검증
- 로지스틱 회귀, 랜덤 포레스트, xgboost모델 성능비교
- 가장 일반화 됐다고 보여지는 xgboost모델 사용
- 순열중요도를 통해 중요도가 낮은 특성들을 제거한 후 모델 성능 재측정

![image](https://user-images.githubusercontent.com/39218451/221575738-b4419376-d1cb-429a-9831-8e8a37a05599.png)

### 5. 머신러닝 모델 해석
- 훈련 정확도:  0.7390
- 검증 정확도:  0.7281
- 시각화를 통해 특성이 타겟에 미치는 영향 확인

![image](https://user-images.githubusercontent.com/39218451/221576230-25ae68bb-0077-4e9d-9293-6ea484c48486.png)

- 가장 영향력이 컸던 골드차이량과 다른특성들을 비교했을때도 경험치 차이량이 클수록 골드차이량에 긍정적인 영향, 블루팀의 Kill,Death,Assists비율이 클수록 골드차이량에 긍정적인 영향을 줌

### 결론
- 승리(타겟)을 위해 초반 10분간의 게임에서 상대방과의 골드차이량을 많이내야하고 골드차이량을 많이내기위해 경험치, KDA비율도 잘 신경써야할 것으로 보여집니다.
- 인게임 내부적으로 추가적인 데이터(챔피언, 스펠여부) 특성이 더 존재했다면 더 좋은 결과를 낼 수 있을거라 보입니다.

