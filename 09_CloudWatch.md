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

