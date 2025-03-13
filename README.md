# ICU-LOS-Prediction
중환자실(ICU) 환자의 입실 기간(Length of Stay)을 예측하는 회귀 프로젝트

본 프로젝트는 데이터 애널리틱스 교과목 시간에 진행한 프로젝트입니다. 

### 프로젝트 개요
+ 프로젝트 배경 : 현재 고령화로 인해 중환자실 수요 증가 반면, 공급은 이를 따라가지 못하고 있습니다.
  
+ 프로젝트 목적: ICU(중환자실) 입원 환자들을 질병군별로 분류하여, 입실 후 주요 생체 신호 이벤트를 기반으로 입실 기간(Length of Stay, LOS)을 예측합니다.

+ 프로젝트 기여점: 입실기간을 예측하면 특정 환자의 퇴원 시점을 미리 파악해 병상 가용성을 효율적으로 계획할 수 있습니다. 이를 통해 환자 입원 및 퇴원 관리를 개선하고 적절한 타이밍에 병상을 확보 할 수 있을 것입니다.
  
+ 데이터 셋: [MIMIC-IV (베스 이스라엘 디콘네스 메디컬 센터의 중환자실 입원 EHR 데이터)](https://physionet.org/content/mimiciv/3.1/)
  
### 파이프라인

1. 데이터 추출 : 이벤트 로그 데이터로부터 환자의 입실 후 6시간, 12시간, 18시간에 대한 데이터를 추출합니다. (여러 시간대에 대해 성능을 비교해 보기 위함)
2. 데이터 집계 : 추출한 데이터에 대하여, 특정 생체 신호를 측정하는 이벤트를 필터링 한 후, 한시간 단위로 집계하여 시계열 데이터를 만듭니다.
3. 전처리 : 사람에게 측정될 수 없는 이상치 및 결측치를 처리 한 후 인코딩 및 스케일링 등 전처리를 진행합니다. 이후 슬라이딩 방식으로 데이터셋을 구성합니다. (데이터 부족으로 더 풍부한 데이터셋을 구성하기 위함)
4. 모델링 : 시계열 데이터에 대해 RNN,LSTM,GRU,Transformer 등 시계열 딥러닝 모델을 구축하여 데이터를 학습합니다.
5. XAI : SHAP을 통해, 각 변수에 대한 영향력을 확인하고 모델의 성능에 대한 신뢰도를 상승시킵니다.

### 주요 생체 신호
+ 감염성 질병 환자: 나이, 심박수, 동맥혈압, 호흡수, 혈당, 백혈구 수치, 젖산 수치 등 15가지 지표

+ 심혈관 질병 환자: 나이, 심박수, 동맥혈압(수축기/이완기), 호흡수, 헤드 위치, 통증 평가 등 16가지 지표

### 활용 데이터셋
+ patients.csv: 환자의 기본 정보 (성별, 나이, 인종 등)
+ icustays.csv: ICU 입원 기간 정보
+ chartevents.csv: 환자의 생체 신호 정보
+ diagnoses_icd.csv: 환자의 진단 정보
+ d_icd_diagnoses: 진단 내용 사전
+ d_items.csv: 이벤트 내용 사전

### 프로젝트 한계점
+ 생체 지표를 측정하는 이벤트가 매우 드물게 나타나거나, 불규칙하게 나타남
+ 모델 파트에 대해, 보안이 필요할 듯 함 --> 시간이 될 때 수정 예정
+ 시간 부족으로 인해 시간 구간을 6, 12, 18시간으로 한정 
+ 1번 진단을 기준으로 환자를 그룹화 하였기 때문에, 다중 진단 환자의 복잡성을 충분히 반영하지 못함
  
### 팀 구성 및 담당
+ 팀원 : 윤지형, 노은선, 백지헌, 김수성
+ 담당 : 데이터 추출 및 감염성 질병군 데이터 생성

### 라이선스
+ 이 프로젝트는 MIT 라이선스를 따릅니다.

### 참고자료
+ [MIMIC 4 피처 및 모듈 설명 document](https://mimic.mit.edu/docs/iv/modules/)
+ Los 예측 관련 논문
  + [ICU 입원기간예측에대한응용, 컬럼비아대학](https://ar5iv.labs.arxiv.org/html/2401.00902)
  + [Prediction of Intensive Care Unit Length of Stay in the MIMIC-IV Dataset](https://www.mdpi.com/2076-3417/13/12/6930)
  
### 추가 자료
+ 발표 자료는 24-2 데이터애널리틱스_2조.pdf 파일에서 확인할 수 있습니다.
