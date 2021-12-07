# AWS 모던 데이터 플랫폼

## 데이터로 판단 가능한 것   

1. 비즈니스 의사 결정
2. 신규사업 분석
3. 보안 분석
4. 서비스 분석

## 모던 데이터 플랫폼 도입 배경

> <h3>데이터 분석 이슈</h3>

기존 데이터베이스에서 데이터를 분석하는데는 현실적으로 어려움이 많습니다. 데이터가 너무 많은 장소에 분산되어 있고, 데이터의 양 또한 너무 많습니다.   

데이터를 효율적으로 분석하기 위해서 아래와 같은 고민을 하게 됩니다.

1. 데이터를 한 곳에서 한 번에 볼 수 없을까?
2. 다양한 소스에서 발생하는 스트림 데이터를 어떻게 신속하게 수집하고 효율적으로 관리할 수 있을까?
3. 여러가지 종류의 분석 및 처리 도구를 하나로 연결해 적용할 수 있는 방법은 없을까?
4. 대부분 저장소와 컴퓨팅 자원은 하나로 결합되어 있어 저장 공간을 늘리기 위해서는 컴퓨팅 자원도 함께 늘려야 하는데, 저장 공간만 따로 확장할 수 없을까?

> <h3>기존 데이터 분석 아키텍처</h3>

![image](https://user-images.githubusercontent.com/43658658/142590175-f4baec5d-03f3-476b-b1d4-09b412d97de7.png)

기업 중앙에 데이터 웨어하우스 장비를 도입하고 다양한 업무 데이터베이스(OLTP, ERP, CRM, LOB)의 데이터를 데이터 웨어하우스에 모으게 됩니다.
데이터 웨어하우스 구축 전에 분석에 필요한 요건들을 취합하고 취합된 요건을 데이터 웨어하우스 설계와 구축에 반영하게 됩니다.
데이터가 데이터 웨어하우스의 정해진 틀 안에 채워지면, 틀 안에서 데이터를 조회하거나 보고서를 생성합니다.
따라서 설계 시에 존재 했던 분석 요건이 아닌 새로운 분석 요건이 발생하면 기존의 설계나 데이터를 틀 안에 채우는 프로그램을 수정하거나 새롭게 만들어야 합니다.
이러면 반영 때마다 최적화가 필요할 수 있고, 그 변화가 기존의 분석에 영향을 미칠 우려가 생깁니다.

> <h3>데이터 Silo 문제</h3>

silo : 각 저장소가 서로 단절된 상태.

![image](https://user-images.githubusercontent.com/43658658/142591309-fc149b89-d616-4232-a996-7286df77f91d.png)   

각 데이터가 서로 격리되어 있다보니 다른 데이터를 함께 확인하기 힘들고
데이터 웨어하우스로 취합하거나 타 데이터베이스로 연동할 수 있는 환경을 조성한다고 해도
동일한 데이터가 여러 번 복제되면서 값이 오염되는 경우도 있고,
데이터 자체가 다른 코드 체계를 사용하거나 단위가 다르거나 하는 등의 문제로 흩어져 있는 데이터를 단일 뷰로 보여주기가 굉장히 어렵습니다.

## 모던 데이터 플랫폼

데이터 분석을 효과적으로 할 수 있는 아키텍쳐의 필요성이 증가하면서 모던 데이터 플랫폼이 등장하게 되었습니다.

> <h3>모던 데이터 플랫폼의 조건</h3>

1. 모든 소스의 데이터를 통합해서 한 곳에 저장하고 분석할 수 있어야 합니다.
2. 저장 시엔 정의된 특정 스키마를 강제하지 않지만, 조회시에는 스키마를 적용할 수 있어야 합니다.
* 스키마 : 데이터베이스에서 데이터의 구조, 데이터의 표현 방법, 데이터 간의 관계를 전반적으로 정의한 것.
* 데이터가 변하는 경우에도 유연한 대응이 가능하고, 새로운 데이터 요건에도 손쉽게 대응이 가능.
3. Data Lake는 쓰기가 아닌 읽기에 스키마를 적용해서 ad-hoc(에드혹) 분석이 가능해야 합니다.
* ad-hoc(에드혹) 분석 : 실시간 스트림 데이터를 분석해서 데이터를 심층 분석해 더 깊고 정확하고 종합적인 방식으로 데이터를 이해하고, 데이터의 가치를 높이는 분석법.
4. 저장소와 컴퓨팅 리소스를 분리해서 각 구성요소를 별도로 확장할 수 있어야 합니다.
* 저장 공간이 필요한 경우 저장 공간만 확장하고, 빠른 데이터 처리가 필요할 경우 컴퓨팅 자원만 늘릴 수 있다면, 자원 사용이 효율적이고 비용도 최소화.

> <h3>완전 관리형 분석 서비스</h3>

완전 관리형 분석 서비스 : 설치, 운영, 성능, 유지보수는 신경 쓸 필요 없이 플랫폼이 제공하는 필요한 기능만 골라서 사용할 수 있는 서비스   
![image](https://user-images.githubusercontent.com/43658658/142595991-c69e2bed-4338-461f-b1ff-d3c98a38930a.png)
* `AWS Managed Services`(AWS 완전 관리형 서비스)를 활용하면, 인력이나 시스템 관리 측면의 어려움 없이 빠르게 모던 데이터 플랫폼을 도입할 수 있음.   

![image](https://user-images.githubusercontent.com/43658658/142596444-d56719b5-b526-4e5b-996b-94b5a6e383df.png)   
직접 하기 어려운 설치나 모니터링, 백업, 패치 등의 작업을 자동으로, 또는 손쉽게 수행할 수 있고, 
확장이나 업그레이드 역시 간단한 설정 변경이나, 클릭 몇 번으로 수행할 수 있습니다.