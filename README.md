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
| 최원호 (팀원) | **프로젝트 관리**  | 전체 일정 관리, 스토리보드 |

## 0. Research Background

### 0-1. 질병 문제로 농장주는 어려움을 겪었을까?

사회적인 변화, 그 중에서 아프리카 돼지 열병과, 구제역과 같은 축산 질병은 농장주에게 큰 피해를 주었습니다. <br/>
축산 질병은 과연 농장주에게 어느정도의 피해를 준 것일까요? <br/>
<br/>
알아보기 위해 데이터 분석을 해보았습니다. 

![도축가와 전염병 관계](https://private-user-images.githubusercontent.com/189529881/427342270-dde7e128-b6ac-48db-80b9-727ba8965990.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDMwNTExNTEsIm5iZiI6MTc0MzA1MDg1MSwicGF0aCI6Ii8xODk1Mjk4ODEvNDI3MzQyMjcwLWRkZTdlMTI4LWI2YWMtNDhkYi04MGI5LTcyN2JhODk2NTk5MC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMzI3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDMyN1QwNDQ3MzFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yMjU5ZTU0OTVjOGY1MjcxNjRjNjQ5NWI4NTlmYmFkNTQwOWVlYWUxOTBlMjc4ODA0MTc0YzI1ZWIyOWZkNGIyJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.-epcxsZGTwBRejofGtkL8rUDwN3YaU6EXTTRKdUcPTQ)

출처 : 농림축산식품부 가축질병 발생현황, KOSIS 통계

위 그래프는 전염병과 도축가 추이를 나타낸 그래프입니다. <br/>
전염병이 높을 땐 도축가가 낮아지고, 전염병이 낮을 땐 도축가가 높아지는 양상을 보입니다. <br/>
즉 전염병과 도축가는 반비례 관계를 갖고 있다는 것을 확인했습니다. <br/>

![수익과 전염병 관계](https://private-user-images.githubusercontent.com/204112513/427348744-74efc992-bc7b-4e14-b88f-6f597ca62973.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDMwNTE2NjAsIm5iZiI6MTc0MzA1MTM2MCwicGF0aCI6Ii8yMDQxMTI1MTMvNDI3MzQ4NzQ0LTc0ZWZjOTkyLWJjN2ItNGUxNC1iODhmLTZmNTk3Y2E2Mjk3My5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMzI3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDMyN1QwNDU2MDBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03NDQ4Njk4YWQwNmRjZDM2NmE5MDdhMGNlOTM2NDkwM2I3ZDBlZjNjYjIyM2E3Y2VjNGIwNzBlMTY1YmU2ZjAwJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.OTllCDqTAJr1ZUGC_QVM1gK_KEEqh-UT1zEP2HTC3B4)

위 그래프는 전염병(아프리카 돼지 열병, 구제역) 발생 건수와 순수익 추이를 나타낸 그래프입니다. <br/>
그래프를 보면 농장의 순수익이 2019년과 2023년에 낮아진 것을 확인할 수 있습니다.<br/>
하지만 그 해의 질병 그래프는 높아진 것을 확인했습니다. <br/>
<br/>
즉 질병 유행 수준과 농장 순수익은 반비례라는 것을 확인 할 수 있는데, <br/>
이러한 결과는 축산 질병이 농장에게 악영향을 크게 준다고 볼 수 있습니다.

**결론 : 질병 문제는 농장주에게 피해를 야기함**

### 0-2. 옥수수 값의 변화가 농장주에게 끼치는 영향이 있을까?

다른 사회적인 요인으로는 곡물 가격의 변동이 있습니다. <br/>
특히 옥수수는 양돈 축산 시장에 영향을 많이 끼치는데, 돼지 사료의 90프로 이상은 옥수수이기 때문입니다. <br/>
그럼 주 사료 재료인 옥수수 값의 변화는 농장주에게 과연 얼마나 영향을 미칠까요?<br/>
<br/>
데이터 분석을 통해서 알아보겠습니다.

![옥수수와 사료비 관계](https://private-user-images.githubusercontent.com/189529881/427342267-d2810be5-3aae-45a6-84a7-6b47f5839444.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDMwNTA2MzEsIm5iZiI6MTc0MzA1MDMzMSwicGF0aCI6Ii8xODk1Mjk4ODEvNDI3MzQyMjY3LWQyODEwYmU1LTNhYWUtNDVhNi04NGE3LTZiNDdmNTgzOTQ0NC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMzI3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDMyN1QwNDM4NTFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02NTU3NDc1OWZjZDY1NjZiOTFhNTM4MmU2YTdmZGE0ODA2NWEyMDU0YjQwNjBjNDg4Y2VkOWQzZDQ1ZGE2ZmJkJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.6EFiKPTS4hgRaweuA-VyUWC-FfBJNSxGHqWV23JiC74)

그래프는 옥수수와 사료값 관계를 나타낸 그래프입니다. <br/>
옥수수 값과 사료값은 서로 비례관계를 나타내고 있습니다.

![도축가와 사료비 추이](https://private-user-images.githubusercontent.com/189529881/427320829-f70c13cb-42ea-48e2-9fca-d5a02ff677b6.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDMwNTA2MzEsIm5iZiI6MTc0MzA1MDMzMSwicGF0aCI6Ii8xODk1Mjk4ODEvNDI3MzIwODI5LWY3MGMxM2NiLTQyZWEtNDhlMi05ZmNhLWQ1YTAyZmY2NzdiNi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMzI3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDMyN1QwNDM4NTFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1jMGU1ZWJlYjIyODA1M2U0OTdhYWE2OGFlYmZlZDRiNzljMTA2ZjM3NDZhMjVlMTg4NDZhOGQwYTY4NjBlYWUzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.Y4D6sN6JFrYm8otLF0LAXdeMfL9kunmv-hyEnCQy6Wc)


이번 그래프는 사료비와 도축가 추이를 나타낸 그래프입니다.<br/>
사료비 그래프와 도축가 그래프가 상당히 비슷한 양상을 나타내는 것을 볼 수 있습니다. <br/>
<br/>
즉 옥수수 값의 상승은 사료비 상승과 연관되어 있고, 돼지고기 값 상승에도 영향을 미치는 것을 확인했습니다. <br/>
<br/>
따라서 농장주는 높아진 옥수수 값으로 인해 농장 운영에 압박을 받을 수 있습니다.

**결론 : 옥수수 값 변동은 농장 운영에 압박을 줄 수 있음.**

### 0-3. 생각해보기
내용 내용

| 이미지 | 내용 |

내용 내용 내용 <br/>
내용 내용 내용

### 0-결론
따라서 사회적인 변화(질병 문제, 곡물 값 상승)는 농가 운영에 어려움을 야기할 수 있다는 사실을 확인했습니다. <br/>
이러한 외부적인 요소로부터 최대한 농장주들을 보호하고, <br/>
결과를 예상할 수 있도록 솔루션이 필요로 합니다. <br/>

우리는 데이터 기반 솔루션으로 '**양돈 관리 서비스:PIG SCOUTER**'를 제안하고자 합니다. <br/>
양돈 관리를 위한 주요 핵심지표(PSY, MSY, 분만율, 평균실상)를 제공하고, <br/>
사료값과 운영비, 수익 예상치를 계산해주는 서비스입니다.

## 1. 서비스를 위한 필요 데이터 수집

### 1-1. 사용자 요구사항 정의 (USER REQUIREMENTS)
우리는 배경조사를 통해 농장주(사용자)의 상황을 알게 되었습니다. <br/>
농장주의 현 상황을 바탕으로 요구사항을 정의해보았습니다. 
| No. | 요구사항 |
|:---:|---|
|UR_01|아래와 같은 농장 정보를 한눈에 모니터링 할 수 있게 해주세요. <br/>- 현재 총 돼지 수 <br/> - 월별 사료 급여량 <br/> - 월별 농장 운영비용 <br/> - 월별 수익 예측  |
|UR_02| 1년동안 엄마돼지 한마리가 출산하여 출하시킨 돼지 평균 마리 수를 알고 싶어요 <br/> (= MSY를 알고 싶어요) |
|UR_03|1년동안 엄마돼지 한마리가 출산하여 젖을 떼기까지 이어진 돼지 평균 마리 수를 알고 싶어요 <br/> (= PSY를 알고 싶어요)|
|UR_04|돼지가 교배후 분만으로 얼마나 이어졌는지 평균을 알고 싶어요. <br/> (= 분만율을 알고 싶어요)|
|UR_05|엄마돼지가 분만시 아기 돼지가 몇마리나 살아남았는지 평균을 알고 싶어요. <br/> (= 평균 실산을 알고 싶어요)|
|UR_06|우리 농장의 예상 사료 가격을 알려줬으면 좋겠어요.|
|UR_07|우리 농장의 예상 운영비를 알려줬으면 좋겠어요. <br/> 운영비에는 아래의 항목이 포함되었으면 좋겠어요. <br/> - 수도광열비, 농구비, 영농시설비, 기타재료비, 차입금이자, 토지임차료, 고용노동비, 분뇨처리비, 생산관리비, 기타비용, 총 합계|
|UR_08|우리 농장(혹은 앞으로 설립할 농장)이 월 얼마정도 수익이 나올지 계산해주었으면 좋겠어요. |

### 1-2. 시스템 요구사항 정의 (SYSTEM REQUIREMENTS)

시용자 요구사항을 바탕으로 시스템 요구사항을 정의해보았습니다.

| No. | 기능 이름 | 기능 설명 |
|:---:|---|---|
|SR_01|농장정보 <br/> 시각화 기능 <br> (대쉬보드) | 농장 정보를 직관적으로 파악할 수 있도록 시각화하여 제공 <br/> - 현재 총 돼지 수 <br/> - 월별 농장 운영비용 <br/> - 월별 수익 예측 <br/> - 월별 사료 급여량 <br/> - 출하 시기 추천
|SR_02|핵심 지표 <br/> 시각화 기능|농장 관리에 필요한 필수 지표 제공 <br/> - MSY : 엄마돼지 한마리가 출산하여 출하시킨 돼지 평균 마리 수 <br/> - PSY : 엄마돼지 한마리가 출산하여 젖을 떼기까지 이어진 돼지 평균 마리 수 <br/> - 분만율 : 돼지가 교배 후 분만으로 이어지는 비율 <br/> - 평균실산 : 돼지가 분만시 아기 돼지가 몇마리나 살아남았는지 평균|
|SR_03|사료비 <br/> 계산기| 농장규모에 따른 사료 비용 예측 |
|SR_04|운영비 <br/> 계산기|농장규모에 따른 운영 비용 예측 <br/> 운영 비용 목록 <br/> - 수도광열비(전기세), 영농시설비, 기타재료비, 차입금이자, 토지임차료, <br/> 고용노동비, 분뇨처리비, 생산관리비, 기타비용, 총 합계|
|SR_05|수익 <br/> 계산기|농장규모와 돼지 평균 무게에 따른 수익 예측|

### 1-3. 수집 데이터 정리 (COLLECT)
시스템 요구사항을 바탕으로, 저희가 수집한 데이터와 저장된 DB 테이블명을 정리했습니다.
| SR No. | 기능 | 데이터 | DB 테이블 or 파일명 |
|:---:|---|---|---|
|SR_02_01| MSY 출력 | | |
|SR_02_02| PSY 출력 | 이유 두수 | avg_iyu |
|        |         | 모돈 회전율 | predicted_sow_turnover |
|SR_02_03| 분만율 출력 | 전국 평균 분만율 | pig_db_monthly |
|SR_02_04| 평균실산 출력 | 실 산자 수 | avg_san |
|SR_03_01| 사료비 계산 | 두 당 사료비 | feed |
|SR_04_01| 운영비 계산 | 두 당 운영비 | operating_price |
|SR_05_01| 수익 계산 | 두 당 돼지 도매가 | 파일 : pig_cost.ipynb |
|        |         | 두 당 돼지 순 이익 | profit_per_pig |


## 2. 서비스 기획 (STORYBOARD)

수집된 데이터를 바탕으로 기획된 서비스입니다.

[스토리보드 링크](https://www.figma.com/design/hiTLHm7gLKHKqU7hOyTuaZ/PIG-SCOUTER?node-id=62-2613&t=yw4IvjsdOEWmJBgv-1)

![명함](https://private-user-images.githubusercontent.com/204112513/427340978-dd20d22f-1109-4083-9e20-52f593233fd2.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDMwNDkzNjgsIm5iZiI6MTc0MzA0OTA2OCwicGF0aCI6Ii8yMDQxMTI1MTMvNDI3MzQwOTc4LWRkMjBkMjJmLTExMDktNDA4My05ZTIwLTUyZjU5MzIzM2ZkMi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMzI3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDMyN1QwNDE3NDhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02MjI3MjI0ZDQ5MjRiY2Q5MTQ1MzE3YmNjNTNlYjE2YzljZjViZWU0N2FlZTc3OTU3NTg3NjBkYWFmNTk1OTYxJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.b42eqkJ58GSzxknfaWKObcegxXW1PUXHn2JY2oosUtU)
**명함 이미지** <br/>
메인 컬러를 주황색으로 하였는데, 돼지 농장의 따뜻하고 생동감 있는 분위기를 표현하면서도 <br/>
흙과 자연을 연상시키는 색감으로 농장환경과 잘 어우러지게 구성하였습니다.<br/>
돼지의 건강한 성장과, 농장의 활력을 동시에 상징하며, 친근하면서 신뢰감을 주는 색상입니다.

![page1](https://private-user-images.githubusercontent.com/204112513/427340005-a4ddff3d-833e-4855-931b-f6faea058066.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDMwNDkxMTMsIm5iZiI6MTc0MzA0ODgxMywicGF0aCI6Ii8yMDQxMTI1MTMvNDI3MzQwMDA1LWE0ZGRmZjNkLTgzM2UtNDg1NS05MzFiLWY2ZmFlYTA1ODA2Ni5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMzI3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDMyN1QwNDEzMzNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0xODAzYWY0MTkzNzc4ODEzZmFjZjBjY2U4OWZlZjRmOGI4OTQ5ZTdmODZmZWU1MTJjZTE4NjhhMTFlMTAwYzM5JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9._ggaBUcyDPG1e-cIOojPf7KO0L-GOfFbamifNd4Li88)
**대시보드**<br/>
농장 정보를 한눈에 볼 수 있는 대시보드입니다. <br/>
총 돼지 수를 입력하면,월별 사료 필요량과 운영비용, 예상 수익을 계산하여 줍니다.<br/>

![page2](https://private-user-images.githubusercontent.com/204112513/427340002-6bf03cd5-4866-419f-89f4-4e65ae909e01.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDMwNDkxMTMsIm5iZiI6MTc0MzA0ODgxMywicGF0aCI6Ii8yMDQxMTI1MTMvNDI3MzQwMDAyLTZiZjAzY2Q1LTQ4NjYtNDE5Zi04OWY0LTRlNjVhZTkwOWUwMS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMzI3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDMyN1QwNDEzMzNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT00NWU4ZjViOWVjZDc2YzA5N2Q3ZTQyNWMzNmM1Mzc1MmJlMjViZGFmYzZkMDNkNDU3YzFmMjU4ZGFiMTI3Y2U4JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.tp-Jxgl5zwNkrLPOXzH8oD_K6_lRhPfx5r5o_-SuGoE)
**핵심지표 산출기**<br/>
농장 관리에 필요한 핵심 지표, MSY, PSY, 분만율, 평균실산을 계산하여 줍니다.<br/>
전체 농장 기준으로도 평균치를 나타내주고 <br/>
요구 정보를 입력시 개인 농장에도 맞게 지표를 산출하여줍니다.

![page3](https://private-user-images.githubusercontent.com/204112513/427340000-5655c240-87d9-4377-b083-d08f15764646.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDMwNDkxMTMsIm5iZiI6MTc0MzA0ODgxMywicGF0aCI6Ii8yMDQxMTI1MTMvNDI3MzQwMDAwLTU2NTVjMjQwLTg3ZDktNDM3Ny1iMDgzLWQwOGYxNTc2NDY0Ni5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMzI3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDMyN1QwNDEzMzNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1mZTUyZWNkMzVkNWVkMjg4M2RlYzM4Njc1ZmMxZGNlYWNhZGU4MzJjNmI5YWQwMjY1ZDMxZWQ5NzU3NGU0NDI3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.N9U3IzutAbfocM2UcJAKbVSIXGoMvDRBFe1CORWEyHU)
**사료비 계산기**<br/>
돼지 수(농장규모)를 입력시 예상 사료비용을 계산하여 줍니다. 

![page4](https://private-user-images.githubusercontent.com/204112513/427340009-f28f4ccc-722f-4bd5-92bf-60affe0a74af.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDMwNDkxMTMsIm5iZiI6MTc0MzA0ODgxMywicGF0aCI6Ii8yMDQxMTI1MTMvNDI3MzQwMDA5LWYyOGY0Y2NjLTcyMmYtNGJkNS05MmJmLTYwYWZmZTBhNzRhZi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMzI3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDMyN1QwNDEzMzNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1jYzRhYTQ4NDA5Y2U0MzVlNzI3NzRmMDA0NTc4MGQ2Yjk1OTFiNDcwZTE5ZDI1NmNhZGQ2Y2FmODZlYmYyYWQzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.c0vRR-PNmPuFpr7uSr4s5jWAIrXR4ma16OvVRT-VAzA)
**운영비 계산기** <br/>
돼지 수(농장규모)를 입력시 예상 운영비용을 계산하여 줍니다.<br/>
수도광열비, 농구비, 영농시설비, 기타재료비, 차입금이자, 토지임차료, 고용노동비, 분뇨처리비, 생산관리비, 기타비용 을 계산해주고, <br/>
마지막으로 합계 정보를 계산하여 줍니다.

![page5](https://private-user-images.githubusercontent.com/204112513/427340004-a808f08f-e393-40d6-be88-2efa27be9978.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDMwNDkxMTMsIm5iZiI6MTc0MzA0ODgxMywicGF0aCI6Ii8yMDQxMTI1MTMvNDI3MzQwMDA0LWE4MDhmMDhmLWUzOTMtNDBkNi1iZTg4LTJlZmEyN2JlOTk3OC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMzI3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDMyN1QwNDEzMzNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03MWI4NDQ4Zjg5ZTBhNWY5ZjQ5MWNkOWZiYmM5N2IyMDI5Nzc1N2RiYTAwNjlkOGY4M2IzYjA1OGY2YTc3OTEzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.Sr0BfYyR-7U-egwi1IboWJJwU4bU6TLcoKDb7mxzjtE)
**수익 계산기** <br/>
돼지 수(농장규모)를 입력시 예상 수익을 계산하여 줍니다.