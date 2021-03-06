# DynamoDB

: `NoSQL 데이터베이스`를 제공하는 서비스입니다.   
사용자가 따로 설치, 운영, 확장하지 않아도 원할 때 바로 사용할 수 있도록 만든 서비스입니다.

읽기와 쓰기가 매우 빈번하고 처리 속도가 빨라야 하는 환경, 작은 용량의 데이터가 매우 많을 때, 고가용성의 분산 데이터베이스를 자체적으로 운영하기에는 부담될 때 적합합니다.   
따라서 `모바일 게임`과 `소셜 네트워크`와 같은 서비스에 유용합니다.   

관계형 데이터베이스가 아니기 때문에 복잡한 쿼리가 필요한 환경에는 적합하지 않습니다.

## NoSQL vs 관계형

> <h3>관계형 데이터베이스</h3>

![image](https://user-images.githubusercontent.com/43658658/146698193-9a0e4b84-5d4a-4c95-8623-5725f07cbc32.png)   
관계형 데이터베이스는 테이블에 데이터를 저장하기 전에, `테이블 구조`를 미리 만들어 놓아야 합니다.   
또한, `테이블간의 관계`를 주 키(Primary Key)와 외래 키(Foreign Key)를 이용하여 정의해야 합니다.   
이것을 데이터베이스 `스키마(Schema)`라고 합니다.   
그래서 관계형 데이터베이스는 미리 정해진 스키마에 맞게 데이터를 추가해야 하고, 스키마에 맞지 않으면 데이터를 추가할 수 없습니다.

> <h3>NoSQL 데이터베이스</h3>

![image](https://user-images.githubusercontent.com/43658658/146698258-59928340-415d-428e-bac1-d388ca06a412.png)   
NoSQL 데이터베이스는 기존의 관계형 데이터베이스와는 달리 `스키마`가 정해져 있지 않습니다.   
따라서 `비정형 데이터`를 저장하는데도 유용합니다. 정형화된 데이터를 저장해도 됩니다.

## NoSQL의 특징

> <h3>고가용성</h3>

리전별로 생성할 수 있으며 성능과 가용성을 위해 데이터를 3곳의 가용 영역에 복제하여 저장합니다.   
사용자가 따로 데이터를 백업할 필요가 없습니다.

> <h3>용량 제한 없음</h3>

저장할 수 있는 데이터 용량에 제한이 없습니다.   
데이터 용량이 증가하면 DynamoDB가 알아서 스토리지를 늘리고 클러스터를 확장하여 데이터를 분산시킵니다.

> <h3>자동 샤딩</h3>

기존 데이터베이스는 테이블의 용량이 커지면 `샤딩(Sharding)`이라는 방법을 통해 테이블의 데이터를 여러 서버에 분산해서 저장하였습니다.   
DynamoDB는 `샤딩`을 `자동으로 처리`해주기 때문에 데이터가 늘어나는 것에 대해 신경을 쓸 필요가 없습니다.

## DynamoDB 개념

> <h3>데이터 저장 구조</h3>

![image](https://user-images.githubusercontent.com/43658658/146698258-59928340-415d-428e-bac1-d388ca06a412.png)   
* `테이블(Table)` : 테이블은 아이템들의 모음입니다. 들어갈 수 있는 아이템의 개수는 제한이 없습니다. 그리고 테이블은 `기본 키(Primary Key)`를 반드시 지정해야 합니다. 리전당 생성할 수 있는 테이블의 최대 개수는 256개이며 AWS에 요청하여 더 늘릴 수 있습니다.
* `항목(Item)` : 항목은 `속성(Attribute)`들의 모음입니다. 들어갈 수 있는 속성의 개수는 제한이 없습니다. 단 항목의 크기는 속성 이름과 값을 포함하여 64KB까지입니다. 기본 키는 필수로 포함하고 있어야 하며 복합키와 기타 속성을 가집니다.
* `속성(Attribute)` : 속성은 `키-값(Key-Value)` 방식입니다. 키는 문자열이라야 합니다.
  - 속성의 값에 NULL이나 빈 문자열은 저장할 수 없습니다. 

> <h3>데이터 형식</h3>

* 스칼라 데이터 형식(Scalar data types)
  - 숫자: `정수와 실수`를 지원합니다. 숫자는 0을 트림(Trim)합니다. 예) 0.3 → .3, 00300 → 300
  - 문자열: `UTF8` 형식이며 대소를 비교할 때에는 `아스키(ASCII)` 코드 기준으로 합니다.
  - 바이너리: 바이너리 데이터는 BASE64 형식으로 인코딩하여 저장하면 됩니다. 대소를 비교할 때에는 각 바이트를 부호 없는(unsigned) 정수로 취급합니다.
* 다중 값 형식(Multi-valued types): 스칼라 데이터 형식의 `배열 형태`입니다. 숫자 세트(Number Set), 문자열 세트(String Set), 바이너리 세트(Binary Set)가 있습니다. 
  - 다중 값 형식에 들어가는 값은 중복될 수 없으며 값이 하나라도 들어가 있어야 합니다. 
  - 값들은 정렬되지 않으며 정렬 순서도 저장되지 않습니다. 
  - 다중 값 형식은 기본 키로 사용할 수 없습니다.

> <h3>기본 키 형식</h3>

DynamoDB에서 검색을 하려면 기본 키로 인덱스를 생성해야 합니다. 그리고 이 기본 키는 테이블을 생성할 때 반드시 지정해야 하며 이 기본 키로 생성되는 인덱스를 `테이블 인덱스`라고 합니다.

* `파티션 키` 형식 기본 키: 속성 `한 개`를 기본 키로 사용합니다. 기본 키의 값은 스칼라 데이터 형식만 가능합니다. 다중 값 형식은 지원하지 않습니다.
  - `일치 방식` 검색만 지원합니다. 
* `파티션 키와 정렬 키` 형식 기본 키: 속성 `두 개`를 기본 키로 사용합니다. 첫 번째 속성은 파티션 기본 키로 사용하고 두 번째 속성은 정렬 기본 키로 사용하여 두 가지를 복합적으로 사용하는 방식입니다.
  - `일치, 부등호, 포함, ~로 시작` 등의 검색을 지원합니다.

기본 키로 생성하는 테이블 인덱스 이외에도 `보조 인덱스`를 생성할 수 있습니다. 기본 키로 생성한 인덱스 하나만으로는 검색 기능이 부족하기 때문에 사용자가 원하는 속성으로 보조 인덱스를 생성하여 `검색에 활용`할 수 있습니다. 이 보조 인덱스는 사용이 빈번하기 때문에 성능을 위해 `읽기/쓰기 용량 유닛을 따로 설정`할 수 있습니다.

* `로컬 보조 인덱스(Local Secondary Index)` : 파티션 키는 테이블 인덱스의 파티션 기본 키와 같고, 정렬 키는 다르게 설정한 것입니다. 테이블당 5개까지 생성할 수 있습니다.
* `글로벌 보조 인덱스(Global Secondary Index)` : 파티션 키와 정렬 키 모두 테이블 인덱스와 다르게 설정한 것입니다. 정렬 키는 생략할 수 있습니다. 테이블당 5개까지 생성할 수 있습니다.

> <h3>데이터 읽기</h3>

* `Eventually Consistent Read(기본)` : `읽기 처리량`을 최대화합니다. 쓰기가 데이터의 모든 복사본에 반영되는 것은 1초내에 이루어집니다. 최신 데이터를 얻으려면 짧은 시간 내에 읽기를 반복해야 합니다.
  - 읽은 데이터가 최근 완료된 쓰기 결과를 반영하지 못했을 수도 있습니다.  
* `Strongly Consistent Read` : 최근 완료된 쓰기 결과가 모두 반영된 데이터를 가져옵니다.

> <h3>데이터 처리량</h3>

사용자가 원하는 수치를 지정하면 DynamoDB가 알아서 지정된 수치만큼 처리량을 제공해주는 것을 말합니다.

* `필요한 읽기 용량 유닛(Read Capacity Units)` : 초당 읽은 아이템 수 x KB 단위 아이템 크기(근사치 반올림) 
  - (`Eventually Consistent Read`를 사용하는 경우 초당 읽은 아이템 용량은 두 배가 됩니다)
* `필요한 쓰기 용량 유닛(Write Capacity Units)` : 초당 쓴 아이템 수 x KB 단위 아이템 크기(근사치 반올림)

> <h3>데이터 조회 방법</h3>

한 번에 조회할 수 있는 용량은 `1MB`입니다.

* `Scan` : 조건 없이 모든 데이터를 가져옵니다.
* `Query` : 해시(기본)키에 특정 값을 지정하고, 범위(기본)키에 조건을 지정하여 원하는 데이터를 가져옵니다. 범위(기본)키에 조건을 지정하는 것은 생략할 수 있습니다.

## 데이터 설계

게임에서 `유저 순위 테이블`과 `친구 순위 테이블` 구조를 설계하고 이후에 DynamoDB에 테이블로 생성해보겠습니다.

먼저, `유저 순위 테이블(UsersLeaderboard)`의 구조를 설계해보겠습니다.   
* Id: 숫자 형식이며 유저를 구분하는 유일한 값입니다.
* Name: 문자열 형식이며 유저의 이름을 저장합니다.
* TopScore: 숫자 형식이며 유저의 주간 최고점수를 저장합니다.
* Week: 문자열 형식이며 한 주를 저장합니다. 한 주의 시작 날짜와 마지막 날짜를 `,`를 기준으로 적습니다.

![image](https://user-images.githubusercontent.com/43658658/146720246-74d919e7-2624-4660-b35b-81c158be1139.png)   
* `Id`를 `파티션 기본 키`, `Week`를 `정렬 기본 키`로 설정한 `테이블 인덱스`로 `개인 최근 점수 목록`을 조회할 수 있습니다.
  - Week를 9999-12-31,9999-12-31보다 작은 `less than` 조건으로 `내림차순`으로 정렬하면 됩니다.
* `Id`를 `파티션 키`, `TopScore`를 `정렬 키`로 설정한 `로컬 보조 인덱스`로 `개인 최고기록`을 조회할 수 있습니다. 
  - TopScore를 99999보다 작은 `less then` 조건으로 `내림차순` 정렬하면 됩니다.
* `Week`를 `파티션 키`, `TopScore`를 `정렬 키`로 설정한 `글로벌 보조 인덱스`로 `주간 전체순위`를 산출할 수 있습니다.
  - Week와 매칭되는 `TopScore`를 내림차순 정렬하면 됩니다. 

다음으로, 친구 순위 테이블(FriendsLeaderboard)의 구조를 설계해보겠습니다.   
* Id : 숫자 형식이며 유저를 구분하는 유일한 값입니다.
* Name : 문자열 형식이며 유저의 이름을 저장합니다.
* Score : 숫자 형식이며 현재 유저가 획득한 점수를 저장합니다.
* FriendIdAndWeek : 문자열 형식이며 현재 유저의 친구 Id와 한 주를 함께 저장합니다. 친구 Id가 2라면 2,2014-05-09,2014-05-15와 같은 형식입니다. 
  - 현재 유저가 게임을 플레이 할 때마다, 유저의 모든 친구 Id와 week를 이런 방식으로 점수와 함께 저장해야 합니다. 
  - 또한, 자신의 Id가 1이라면 1,2014-05-09,2014-05-15와 같이 자기자신의 데이터도 저장합니다.

![image](https://user-images.githubusercontent.com/43658658/146720866-715e6011-fc47-43d0-9c67-591f8f2079db.png)
* `글로벌 보조 인덱스`로 `주간 친구순위`를 산출할 수 있습니다. 
  - 조회하고 싶은 Id가 `2`라면 FriendIdAndWeek를 2,2014-05-09,2014-05-15와 `같은` 조건에 Score를 99999보다 `less than` 조건으로 `내림차순` 정렬하면 됩니다.

이런 방법으로 DynamoDB에 점수 데이터를 저장하면 늘어나는 유저의 숫자와 상관없이 순위를 산출할 수 있습니다.   
데이터가 늘어나면 대응을 자동으로 해주기 때문에 용량에 대해 신경 쓸 필요가 없고 쿼리 속도 또한, 빨라서 상당히 편리합니다.

## 테이블 생성

앞서 설계한 데이터를 테이블로 2개 생성해보겠습니다.

먼저 첫 번째 테이블(UsersLeaderboard)을 생성합니다.   

[DynamoDB 콘솔] > [테이블] > [테이블 생성]   
![image](https://user-images.githubusercontent.com/43658658/146716842-1e10d8d1-75ba-4572-a308-d478809407f8.png)

테이블 이름과 파티션 키(해시 기본 키)와 정렬 키(범위 기본 키)를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146717619-a0195c6e-86f4-4a54-a82b-8fb45375428c.png)

`설정 사용자 지정`을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146718041-38e33ac6-1f28-46c7-806c-2622a15142a6.png)

읽기/쓰기 용량을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/146719235-967b0eba-b215-4e8b-8826-d7888f3f02ed.png)

`글로벌 보조 인덱스`를 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/146718118-af98e3c8-d1d4-4c12-a8db-9929f473a13d.png)

파티션 키와 정렬 키를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146718244-e5e91c9b-0a7c-4645-b0d9-4c8bbce0634b.png)

`로컬 보조 인덱스`를 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/146718484-f09a577f-c85d-4df2-9c6e-fba064ba7ca8.png)

정렬 키를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146718456-79e3c301-8d60-4914-a46f-4fe0a9eb109e.png)

DynamoDB가 AWS KMS 키를 소유하고 관리하도록 해서 비용을 절감시킵니다.   
![image](https://user-images.githubusercontent.com/43658658/146718852-022b3bd5-f4f8-4f29-916c-fd49ea3fa3e5.png)

첫 번째 테이블(UsersLeaderboard)을 생성합니다.

두 번째 테이블(FriendsLeaderboard)을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146719527-045690d2-9132-4040-b82c-4da6bfac3846.png)

`글로벌 보조 인덱스`를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146719637-f0a940a9-cc7f-487e-8b9a-57537b45ce14.png)   
![image](https://user-images.githubusercontent.com/43658658/146719655-c4d180c1-8728-4c07-a99d-c911bd95c5d4.png)

나머지 설정은 동일하게 해서 테이블을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146721207-24bc3c29-0e18-4bd0-b98d-117ebdde16b1.png)

## 항목 생성

테이블 내에 항목을 생성해서 데이터를 추가해보겠습니다.

[DynamoDB 콘솔] > [항목] > [테이블 선택] > [항목 생성]   
![image](https://user-images.githubusercontent.com/43658658/146721993-290f1120-7ba5-4420-9bc5-03b76b9fad73.png)

아래와 같이 `속성`을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146722980-5aef9bf1-081c-4cf1-b1a6-a4dbfe013a61.png)

방금 생성한 속성을 체크하고 [작업] > [복제]로 항목을 동일한 양식으로 편하게 작성할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146723491-0f73a461-3237-4c9f-a2fe-64a74849914f.png)

아래와 같이 모든 항목을 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/146723569-4785589c-3e5a-491c-8b75-61285200d1c9.png)

다음으로 두 번째 테이블(FriendsLeaderboard)의 항목을 아래와 같이 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146726344-9d9718ee-e8f5-4e18-a107-eb1c6ecf71bb.png)   
* 1 - 1, 2
* 2 - 1, 2, 3
* 3 - 2, 3
* 친구 관계는 자기 자신도 친구로 포함합니다.

## 인덱스를 이용한 쿼리

이제 첫 번째 테이블에서 `글로벌 보조 인덱스`를 통해 `2021-12-13 ~ 2021-12-19` 기간 동안의 `점수 순위`를 `내림 차순`으로 정렬해보겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/146727138-c893ce21-c6eb-4e35-9b74-da8ba175a5f7.png)

한 유저의 개인 최고 기록 순위를 정렬해보겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/146727312-dce6a4e9-6d48-4e8d-a276-e1088c052b54.png)

다음으로 두 번째 테이블로 넘어가서 `글로벌 보조 인덱스`를 통해 `Id=3` 유저의 친구 순위를 정렬해보겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/146727555-ff75b2dc-cd5a-4788-82fb-0adefb72b19a.png)































