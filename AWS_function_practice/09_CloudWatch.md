# CloudWatch

AWS 리소스 상태를 모니터링 하는 서비스입니다.

모니터링 뿐만 아니라 `측정치와 연계`해서 `다양한 액션(알림, Auto Scaling)`을 수행할 수도 있습니다.

## AWS 리소스에 따른 CloudWatch 활용법

1. EC2 인스턴스: CPU 사용률, 데이터 전송량, 디스크 사용량 등을 모니터링할 수 있습니다. 

2. EBS 볼륨: 읽기/쓰기 사용량, 지연 시간 등을 모니터링할 수 있습니다. 

3. ELB(Elastic Load Balancing): 요청 수 및 지연 시간 등을 모니터링할 수 있습니다. 

4. RDS(Relational Database Service): CPU 사용률, DB 연결 수, 사용 가능한 메모리 및 스토리지 공간, 읽기/쓰기 지연 시간 등을 모니터링할 수 있습니다. 

5. DynamoDB: 테이블 인덱스와 글로벌, 로컬 보조 인덱스에서 소모한 읽기/쓰기 용량 유닛, 스캔, 쿼리, 아이템 추가 및 수정(PutItem), 아이템 삭제(Delete)를 모니터링 할 수 있습니다. 

6. ElastiCache: CPU 사용률, 데이터 읽기/쓰기, 네트워크 사용량, 캐시 엔진의 각 명령어 사용량 등을 모니터링 할 수 있습니다. 

7. SNS(Simple Notification Service): 게시(Published) 및 전송(Delivered) 메시지 수 등을 모니터링할 수 있습니다. 

8. SQS(Simple Queue Service): 전송(Send) 및 수신(Received) 된 메시지 수 등을 모니터링할 수 있습니다.

## CloudWatch 알람 생성

인스턴스의 CPU 사용률을 모니터링하고 설정한 측정치에 도달하면 알림 메일을 전송하도록 알람을 생성해보겠습니다.   

1. 인스턴스를 하나 선택하고, [우클릭] > [모니터링 및 문제 해결] > [세부 모니터링 관리]   
![image](https://user-images.githubusercontent.com/43658658/145750688-e7105956-cedd-443d-b153-6cecd15a6ec3.png)

2. 세부 모니터링을 활성화 해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/145750747-7f758d50-bc29-47f7-a738-a3aba300d49c.png)   
* 세부 모니터링 활성화 : 기본 5분 간격에서 1분 간격으로 모니터링을 할 수 있습니다.

3. 콘솔에서 `Cloudwatch`를 검색합니다.   
![image](https://user-images.githubusercontent.com/43658658/145750808-c6a9817d-5d80-4a98-8345-516828ea41a1.png)

4. [경보] > [경보 생성]   
![image](https://user-images.githubusercontent.com/43658658/145750951-3a3e13c2-9b37-4e4a-a101-42c45e52a1bb.png)

5. `단계 1`에서 [지표 선택]을 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/145751380-643e0d03-d6ba-4661-8514-73588f5999ab.png)
* 인스턴스 ID를 통해 검색해서 [인스턴스별 지표]를 누릅니다.

6. `지표 이름`이 `CPUUtilization`인 것을 선택하고 [지표 선택]을 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/145751572-47b17abb-7f60-41f8-a7eb-f87b399e5eba.png)

7. 통계 방법과 모니터링 간격을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/145751667-167b88d7-e2c2-4a73-902d-251081c2fdf2.png)

8. 알람 기준을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/146382585-148ba912-5402-45d8-8ae7-8658c693ddb6.png)

(참고) `임계값 유형`을 `이상 탐지`로 선택하면 임계값으로 표준 편차를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/145752061-e07d4e1b-d8fd-441e-b5c0-08d4af121fa1.png)   
* 편차 : 관측값 - 평균
* 분산 : 편차의 평균
* 표준 편차 : 분산의 제곱근. 정규 분포에서 평균값에서 얼마나 떨어져 있는지 정도를 나타냅니다.

즉 표준 편차는 아래 그래프에서 회색 영역의 범위를 설정하는 것이 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/145752537-2cc03b5d-ef3f-43d2-91a5-b5081d2271e7.png)

9. `추가 구성` 탭으로 들어갑니다.   
![image](https://user-images.githubusercontent.com/43658658/145753040-bfe47056-7c0b-4ecd-92e5-fcc45b1be56a.png)   
* `경보를 알릴 데이터 포인트` : `데이터포인트수/평가기간`를 설정합니다.
  - 앞서 설정한 `기간(5분)` * `평가기간` 동안 `데이터포인트 수` 횟수 만큼 알람 조건을 만족하면 알람이 울리도록 설정.
  - 예를 들어, `4/5`이라면 총 25분 동안 4번의 알람 조건을 만족하면 알람이 발생합니다.   
  ![image](https://user-images.githubusercontent.com/43658658/145753306-5beb6b0a-cac1-4615-87fc-d5544b32336d.png)

10. `단계 2`에서 다음을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/146383594-3f5c4306-77ca-42f6-8100-25e5de299df2.png)   
* `경보 상태 트리거` : CPU가 정해진 임계값을 벗어나면 알람이 발생.
* `SNS 주제 선택` : 알람을 수신할 SNS를 선택합니다.
  - `주제 이름`과 `알림을 수신한 이메일`을 작성합니다.   
  - 작성을 완료하고 [주제 생성] 버튼을 누르면 아래와 같이 나타납니다.   
  ![image](https://user-images.githubusercontent.com/43658658/145754116-b1819541-2f43-4c0d-be90-56480388b9f3.png)    
  - 이때 작성한 이메일로 들어가 `confirm subscription`을 진행해주어야 합니다.   
  ![image](https://user-images.githubusercontent.com/43658658/145755153-001be282-2761-44b5-81cf-637ff53678d3.png)   
* `알림 추가` : 여러 개의 알림을 추가로 설정하고 싶다면 클릭합니다.

11. `단계 3`에서 `알람 이름`을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/145754204-6d8fc8f1-1825-4c12-84ed-43f8bb743bb3.png)

12. `단계 4`에서 전체 조건을 확인하고 [경보 생성] 버튼을 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/145754279-09452a5c-14c8-4bc2-95f1-adfd0fe6dead.png)

13. 검색 상태를 `모든 상태`로 선택하면 생성한 알람이 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/145754386-2b1bb012-3e1e-4e78-87d5-4a4e240bc689.png)

> <h3>알람 테스트</h3>

CPU 사용률을 강제로 올려서 알람이 제대로 발생하는지 테스트 해보겠습니다.

해당 인스턴스로 접속해 아래의 명령을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/145754519-65213826-2a3e-48ba-9074-01a7cf26e5b1.png)

몇 분 기다리면 아래와 같이 상태가 변경됩니다.   
![image](https://user-images.githubusercontent.com/43658658/145754894-32ebc9ee-c199-4427-b1e8-fb08994605c5.png)

클릭해서 보면 그래프의 CPU가 임계값을 넘어섰음을 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145754973-68bd0a7c-a09a-4c08-9726-907193f28033.png)

아래와 같이 SNS 엔드포인트로 설정한 이메일로도 알림이 오는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145756452-6ec3e882-5c5f-4e1a-be74-633aa41ea5e4.png)














