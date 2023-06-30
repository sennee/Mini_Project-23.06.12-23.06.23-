# Mini_Project (23.06.12 ~ 23.06.23)
2023 날씨 빅데이터 콘테스트 - 해양안전


<기상에 따른 선박 닻끌림 예측>


## 🖥️ 프로젝트 주제
날씨마루에서 제공하는 선박데이터와 기상·해양 데이터를 이용하여 기상 상태에 따른 닻끌림 발생 여부 분석
<br>


## 🕰️ 프로젝트 진행 기간
* 23.06.12일 - 23.06.23일


## 🧑‍🤝‍🧑 멤버구성
 - 팀장  : 이승윤 - 해양 조사원 해양 데이터 전처리, PPT제작, 발표
 - 팀원1 : 이지연 - 정박지 데이터 선회반경 구하고, 지도에 좌표 표시하기 
 - 팀원2 : 이창희 - 기상청 기상 데이터 전처리
 - 팀원3 : 이민재 - 선박 데이터 전처리, 앙상블 모델 설계, 하이퍼파라미터
 - 팀원4 : 심세은 - 한국수력원자력 해양 데이터 전처리, 데이터 통합, 랜덤포레스트 모델 설계 


## ⚙️ 분석 환경
- `Python`
- 'Jupyter notebook'


## 📝분석 데이터
- 날씨마루 (http://bd.kma.go.kr/) 분석환경에서 다운로드
- 학습데이터 (2021년 1월 ~ 2022년 6월), 검증데이터(2022년 7월 ~ 2023년 3월)
- 데이터 중 지역이 울산인 데이터만 분석 (차후, 부산 데이터 분석 예정)
1. 선박 데이터: 선박상태 정보(해양경찰청 제공)
2. 기상 데이터: 기상관측자료 (기상청 제공)
3. 해양 데이터: 해양부이관측자료 (해양조사원, 한국수력원자력 제공)

### 데이터 구성
<center><img src="./image/ulsan_ship.png" width = "70%"></center>
<center><img src="./image/ulsan_weather.png" width = "70%"></center>

### 데이터 설명
<center><img src="./image/ship_description.png" width = "70%"></center>
<center><img src="./image/weather_description.png" width = "70%"></center>
<center><img src="./image/ocean_description_1.png" width = "70%"></center>
<center><img src="./image/ocean_description_2.png" width = "70%"></center>

## 📌 프로젝트 진행 과정
### 1. 데이터 전처리
- **울산 데이터만 필터링**
  - 지역에 관한 컬럼이 존재하는 테이블에서 지역을 기준으로 울산 데이터만 필링
    
- **테이블 간 시간컬럼 표기 통일** 
  - 각 테이블마다 시간컬럼 표기가 달라 통일
  - `string('yyyymmddhhmm)` 형태의 시간 데이터 → `year, month, day, hour, min` 컬럼으로 분리
  - `hour`단위로 기록된 데이터를 가진 테이블, `min`단위로 기록된 데이터를 가진 테이블, 그리고 `sec`단위로 기록된 데이터를 가진 테이블이 존재함.
    
- **테이블 간 위경도 표기 통일**
  - `.lat`, `.lon`으로 위경도 컬럼 name 통일
  - `string`형태의 위경도 데이터 → `float`형태로 데이터 타입 변경
    
- **결측치(-99, -99.9, -999) 처리**
  - 선박데이터_cog컬럼: 평균값으로 대체
  - 선박데이터_hdg컬럼: 평균값으로 대체
  - 해양데이터_ws컬럼: 평균값으로 대체
  - 해양데이터_wd컬럼: hour기준 최빈값으로 대체
  - 해양데이터_wh컬럼: 평균값으로 대체
    
- **데이터 통합**
  - 선박데이터의 시간 컬럼을 기준으로 나머지 기상데이터, 해양데이터 통합하여 하나의 테이블로 만듦. → `df_train.csv`, `df_test.csv`
  - 선박데이터의 시간에 대한 기상·해양 데이터가 없는 경우에 대해 2가지 방법으로 처리
    - 기상·해양 데이터를 평균 및 최빈값으로 대체 → `df_train_v2_mean.csv`, `df_test_v2_mean.csv`
    - 해당 row 삭제 → `df_train_v2.csv`, `df_test_v2.csv`
     
- **이상치 처리**
  - 각각의 컬럼에 대해 boxplot을 그려 이상치 확인 후, 이상치 데이터를 가진 row 삭제
    <center><img src="./image/outlier.png" width = "70%"></center>

### 2. 데이터 분포 시각화 - geoplot

### 3. 데이터 특성 간 상관관계 시각화 - Heat map
<center><img src="./image/heatmap.png" width = "50%"></center>

### 4. 모델링
- **사용한 모델**
  - RandomForest
  - XGBoost
  - XGBRF
- **모델별 성능 비교**
  <center><img src="./image/models.png" width = "70%"></center>

- **앙상블**
  <center><img src="./image/Ensemble.png" width = "90%"></center>

### 5. 최종 모델 선정
- XGBRF
- HyperParameter Tunning 사용
- Standardscaler 사용
  
### 6. Feature Inportance 확인
<center><img src="./image/Feature Inportance.png" width = "70%"></center>

