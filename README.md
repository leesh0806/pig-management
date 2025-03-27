# eda-repo-4 : 양돈 관리 서비스 (Team 김이박최)

## Introduction

### 주제
| 주제 | 양돈 관리 서비스 |
|:---|:---|
| 서비스 명 | Pig Scouter |
| 배경 | 돼지고기는 국내 소비량이 높은 축산물 중 하나로, **양돈 산업은 국내 축산산업에서 중요한 역할**<br/>하지만, 사료비 상승, 질병 문제, 가격 변동 등으로 인해 **농가의 운영이 점점 어려워짐**.<br/>문제 개선을 위해, 데이터 기반 솔루션을 필요. <br/> -> 돼지 수와 농장 규모를 입력하면 <u>**사료비와 운영비, 수익을 같이 예측해주는 서비스가 있다면?**</u> <br/>사료비, 운영비, 구축비를 계산해줌으로써 운영 전략을 제공하는 서비스 계획
| 서비스 | 농장의 전체 상태를 파악하는 **대시보드** <br/> 농장 관리에 필요한 **핵심지표(PSY, MSY, 분만율, 표준실산) 제공** <br/> 농장 규모 입력시 **예상 사료비 계산** <br/> 농장 규모 입력시 **예상 운영비 계산** <br/> 농장 규모 및 지역 입력 시 **예상 인프라 구축비 계산**|

### 기술 스택
|분류|기술|
|---|---|
|개발환경|<img src="https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=white"/> <img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=Ubuntu&logoColor=white"/>|
|언어|<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=Python&logoColor=white"/> 
|데이터프레임|<img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=Pandas&logoColor=white"/> <img src="https://img.shields.io/badge/Numpy-013243?style=for-the-badge&logo=numpy&logoColor=white"/>|
|웹크롤링| <img src="https://img.Shields.io/badge/selenium-43B02A?style=for-the-badge&logo=selenium&logoColor=white"/>|
|시각화| <img src="https://img.shields.io/badge/Matplotlib-301D81?style=for-the-badge&logo=Python&logoColor=white"/> <img src="https://img.shields.io/badge/Seaborn-50BFDE?style=for-the-badge&logo=Python&logoColor=white"/>|
|데이터베이스|<img src="https://img.shields.io/badge/MYSQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white"/>|
|협업 툴|<img src="https://img.shields.io/badge/confluence-172B4D?style=for-the-badge&logo=confluence&logoColor=white"/> <img src="https://img.shields.io/badge/slack-4A154B?style=for-the-badge&logo=slack&logoColor=white"/> |



### 팀명 : 김이박최
| 이름 | 주요 역할 | 상세 |
|:---:|---|---|
| 김지연 (팀장) | **핵심지표 기능 구현**  | 핵심지표 산출식 정리 |
| 이승훈 (팀원) | **DB 구축, 크롤링 및 분석** | 데이터 수집, 시각화, 분석|
| 박성윤 (팀원) | **통계자료 서칭, 자료 정리**   | 출처 조사 및 전체 자료 정리 |
| 최원호 (팀원) | **프로젝트 관리**  | 일정 관리, 스토리보드 및 발표 자료 제작 |

## 0. Research Background

### 0-1. 질병 문제로 농장주는 어려움을 겪었을까?

사회적인 변화, 그 중에서 아프리카 돼지 열병과, 구제역과 같은 축산 질병은 농장주에게 큰 피해를 주었다. <br/>
축산 질병은 농장주에게 어느정도의 피해를 준 것일까? <br/>

| **이미지** | 농장 순수익과 질병의 반비례 관계  그래프 |

출처 : 농림축산식품부 가축질병 발생현황, KOSIS 통계

이미지를 보면 농장의 순수익이 2019년과 2023년에 낮아진 것을 확인할 수 있다.<br/>
하지만 그 해의 질병 그래프는 높아진 것을 볼 수 있다. <br/>
즉 질병 유행 수준과 농장 순수익은 반비례라는 것을 확인 할 수 있는데, <br/>
이러한 결과는 축산 질병의 농장에 아주 크게 악영향을 준다고 볼 수 있다.

**결론 : 질병 문제는 농장주에게 큰 혼란을 야기한다**

### 0-2. 옥수수 값의 변화가 농장주에게 끼치는 영향이 있을까?

다른 사회적인 요인으로는 곡물 가격의 변동이 있다. <br/>
특히 옥수수는 돼지 시장에 영향을 많이 끼치는데, 돼지 사료의 90프로 이상은 옥수수이기 때문이다. <br/>
그럼 주 사료 재료인 옥수수 값의 변화는 농장주에게 어떠한 영향을 미칠까?

| **이미지** | 옥수수 값과, 도매가 그래프

사진은 옥수수 가격 그래프와, 도매가 가격 그래프이다.<br/>
옥수수의 가격이 오르면, 얼마 뒤에 도매가 가격이 오르는(후반영)되는 모습을 보이고 있다. <br/>
즉 옥수수 값의 상승은 돼지고기 값 상승에도 영향을 미친다는 것이다. <br/>
따라서 농장주는 높아진 사료값을 매꾸기 위해 심리적인 압박을 받을 수 있다.

**결론 : 옥수수 값 변동은 농가 운영에 압박을 줄 수 있다.**

### 0-3. 기후 문제가 돼지 농장에 끼치는 영향이 있을까?
내용 내용

| 이미지 | 내용 |

내용 내용 내용 <br/>
내용 내용 내용

### 0-결론
따라서 사회적인 변화(곡물 값 상승, 질병 문제)는 농가 운영에 어려움을 야기할 수 있습니다. <br/>
이러한 외부적인 요소로부터 최대한 농장주들을 보호하고, <br/>
결과를 예상할 수 있도록 솔루션이 필요합니다. <br/>

데이터 기반 솔루션으로 '**양돈 관리 서비스:PIG SCOUTER**'를 제안합니다. <br/>
양돈을 위한 주요 핵심지표(PSY, MSY, 분만율, 평균실상)

## 서비스를 위한 필요 데이터 수집

### USER REQUIREMENTS
| No. | 요구사항 |
|:---:|---|
|UR_01|아래와 같은 농장 정보를 한눈에 모니터링 할 수 있게 해주세요. <br/>- 현재 총 돼지 수 <br/> - 월별 사료 급여량 <br/> - 월별 농장 운영비용 <br/> - 월별 수익 예측 <br/> - 출하시기 추천|
|UR_02| 1년동안 엄마돼지 한마리가 출산하여 출하시킨 돼지 평균 마리 수를 알고 싶어요 <br/> (= MSY를 알고 싶어요) |
|UR_03|1년동안 엄마돼지 한마리가 출산하여 젖을 떼기까지 이어진 돼지 평균 마리 수를 알고 싶어요 <br/> (= PSY를 알고 싶어요)|
|UR_04|돼지가 교배후 분만으로 얼마나 이어졌는지 평균을 알고 싶어요. <br/> (= 분만율을 알고 싶어요)|
|UR_05|엄마돼지가 분만시 아기 돼지가 몇마리나 살아남았는지 평균을 알고 싶어요. <br/> (= 평균 실산을 알고 싶어요)|
|UR_06|우리 농장의 예상 사료 가격을 알려줬으면 좋겠어요.|
|UR_07|우리 농장의 예상 운영비를 알려줬으면 좋겠어요. <br/> 운영비에는 아래의 항목이 포함되었으면 좋겠어요. <br/> - 수도광열비, 농구비, 영농시설비, 기타재료비, 차입금이자, 토지임차료, 고용노동비, 분뇨처리비, 생산관리비, 기타비용, 총 합계|
|UR_08|우리 농장(혹은 앞으로 설립할 농장)이 월 얼마정도 수익이 나올지 계산해주었으면 좋겠어요. |