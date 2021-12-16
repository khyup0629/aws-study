# RDS

RDS : AWS에서 관계형 데이터베이스를 손쉽게 구축하고 확장할 수 있도록 지원하는 기능

## RDS 사용 목적

고성능, 대규모 DB를 운영하려면 전문적인 DB 운영 인력이 필요합니다.   
하지만 스타트업이나 벤처기업에서는 DB를 운영할 인력, 고성능의 DB 장비를 갖추는 것도 버겁습니다.

RDS를 이용하면 클릭 몇 번 만으로 `손쉽게 DB 인스턴스를 생성`할 수 있고, `사용량이 늘어나면` 스토리지 용량과 IOPS를 증가시켜 `성능 확장이 가능`합니다.
또한, 장애가 발생해도 `Failover` 기능 통해 정상적인 서비스 제공이 가능하도록 구성할 수 있고 `Read Replica`를 이용하여 읽기 성능을 향상시킬 수 있습니다

(참고) EC2 인스턴스에 DB 엔진을 설치해서 DB를 구성할 수도 있지만, 이렇게 되면 서버와 DB를 모두 관리해야하고, Failover, Read Replica 같은 기능들도 직접 구축해야하는 번거로움이 있습니다.

## RDS 인스턴스 생성

=> [RDS 인스턴스 생성 공식 문서](https://docs.aws.amazon.com/ko_kr/ko_kr/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html#:~:text=Amazon%20RDS%20DB%20%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%20%EC%83%9D%EC%84%B1)

RDS 콘솔로 접속해서 [데이터베이스] > [데이터베이스 생성]   
![image](https://user-images.githubusercontent.com/43658658/146317169-ac1e12c5-95dc-429b-a0e6-a5ec76eb0eb2.png)

`표준 생성`, `MySQL`을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146317995-304e3882-65f7-4e25-9ebe-49f0eda652ff.png)

`프리 티어`를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146318152-1b95ca6d-4a45-474f-aae4-34f2b2587ede.png)   
* 프로덕션 : Multi-AZ(이중화), 프로비저닝된 IOPS(고성능 I/O), 삭제 방지 활성화 기능이 들어있는 고가용성 템플릿
* 개발/테스트 : 개발 용도의 템플릿

`DB 식별자`와 `마스터 사용자 설정`을 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/146318829-095ab5fe-5bd0-42c0-a56c-a70f64fca12b.png)

`DB 인스턴스 클래스`를 설정합니다. 프리티어이므로 선택지가 제한됩니다.   
![image](https://user-images.githubusercontent.com/43658658/146318910-54af2887-15d1-4b92-b041-ef3418a2d247.png)   
* `스탠다드 클래스` : vCPU, 메모리, 네트워크 등이 `평균`적인 사양으로 제공됩니다.
* `메모리 최적화 클래스` : 다른 인스턴스 클래스보다 `메모리 용량`이 훨씬 큽니다.
* `버스터블 클래스` : 가격이 가장 싼 인스턴스입니다. `낮은 vCPU 성능`과 `적은 메모리`를 제공합니다. 프리 티어(무료 사용 계정)에서는 이 인스턴스 유형을 무료로 사용할 수 있습니다.

초기 스토리지를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/146319171-b20f5639-4631-418c-ad75-700f39e61989.png)

네트워크를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/146319406-4ec6f024-f139-4891-aa06-47e5c0094a1f.png)   
* `퍼블릭 액세스` : VPC 내부에서만 DB 인스턴스에 접속 가능하게 할 것인지 여부.
* `VPC 보안 그룹` : DB 인스턴스 전용 보안 그룹을 설정해야 합니다. 일단 디폴트로 두고 나중에 생성합니다.
* `가용 영역` : EC2 인스턴스에서 DB에 접속한다면 같은 가용 영역에 있는 것이 좋습니다.

DB에 접근하는 사용자가 인증하는 방식을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/146319853-bd16218c-2150-4aab-83a6-df0bc7955773.png)   
* 암호 인증 : 설정한 암호로 사용자를 인증.
* 암호 및 IAM 데이터베이스 인증 : 암호와 IAM 사용자 및 역할을 통해 인증.
* 암호 및 Kerberos 인증 : 암호와 AWS Directory Service, AWS Managed Microsoft AD를 통해 데이터베이스 사용자 자격 증명을 관리합니다.

`추가 구성` 화살표를 눌러 아래 옵션을 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/146324590-a1b135a8-6c04-4121-ab07-fc31aae24608.png)   
* 초기 데이터베이스 이름 : DB 인스턴스를 생성할 때 RDS가 생성하는 `데이터베이스의 이름`을 지정합니다.
* DB 파라미터 그룹 : 데이터베이스에 할당할 리소스 양을 지정하는 그룹입니다. 로그와 관련된 설정을 할 때도 사용합니다.
* 옵션 그룹 : 비슷한 기능을 하는 데이터베이스 끼리 묶습니다.

자동 백업을 활성화하면 아래와 같이 백업과 관련된 설정을 할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146322652-a13881ee-9d7c-46ee-b971-6ecb6f1ec5be.png)

`Enhanced 모니터링 활성화`를 체크하면 아래와 같이 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/146323407-9d71708b-625f-4342-a7fe-709ef465f901.png)   
* `Enhanced Monitoring`을 통해 DB 인스턴스의 운영 체제를 실시간으로 모니터링할 수 있습니다.
* `역할 모니터링`에서 `IAM 역할`을 선택합니다(`IAM 역할`을 통해 `Enhanced 모니터링`에 `권한을 부여`해야 합니다).

CloudWatch Logs에서 어떤 로그 유형을 받을지 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146324615-664715b5-0621-42c4-bc7e-efac0c87fcd5.png)   
* 감사 로그 : 시간, 사용자 그리고 객체에 대한 모든 액세스 형태를 로그로 남깁니다.
* 에러 로그 : 시작, 종료 및 오류 발생 시에만 로그를 남깁니다.
* 일반 로그 : 
* 느린 쿼리 로그 : 쿼리가 실행되는게 설정한 시간보다 늦어지면 로그를 남깁니다.








