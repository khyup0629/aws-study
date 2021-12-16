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
![image](https://user-images.githubusercontent.com/43658658/146338404-9b7f3118-cc97-4cef-b8c0-8a9aecce99bb.png)   
* `퍼블릭 액세스` : VPC 내부에서만 DB 인스턴스에 접속 가능하게 할 것인지 여부. 이후 실습에서 외부에서 접속하게 해줘야 하므로 `YES`를 선택합니다.
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
* `DB 파라미터 그룹` : DB의 세부적인 설정(컴퓨팅 리소스) 관련 파라미터들이 모인 그룹입니다. 로그와 관련된 설정을 할 때도 사용합니다.
* [옵션 그룹](https://docs.aws.amazon.com/ko_kr/ko_kr/AmazonRDS/latest/UserGuide/USER_WorkingWithOptionGroups.html) : DB를 관리하는데 필요한 추가 옵션을 설정한 그룹입니다.

자동 백업을 활성화하면 아래와 같이 백업과 관련된 설정을 할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146322652-a13881ee-9d7c-46ee-b971-6ecb6f1ec5be.png)

`Enhanced 모니터링 활성화`를 체크하면 아래와 같이 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/146323407-9d71708b-625f-4342-a7fe-709ef465f901.png)   
* [Enhanced Monitoring](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/USER_Monitoring.OS.overview.html#USER_Monitoring.OS.cost)을 통해 DB 인스턴스의 운영 체제를 실시간으로 모니터링할 수 있습니다.
* `역할 모니터링`에서 `IAM 역할`을 선택합니다(`IAM 역할`을 통해 `Enhanced 모니터링`에 `권한을 부여`해야 합니다).

CloudWatch Logs에서 어떤 로그 유형을 받을지 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146324615-664715b5-0621-42c4-bc7e-efac0c87fcd5.png)   
* 감사 로그 : 시간, 사용자 그리고 객체에 대한 모든 액세스 형태를 로그로 남깁니다.
* 에러 로그 : 시작, 종료 및 오류 발생 시에만 로그를 남깁니다.
* 일반 로그 : 쿼리가 실행되면 로그를 남깁니다.
* 느린 쿼리 로그 : 쿼리가 실행되는게 설정한 시간보다 늦어지면 로그를 남깁니다.

![image](https://user-images.githubusercontent.com/43658658/146327984-75f618b5-f754-476b-ae5f-0d2e4115c236.png)   
* `마이너 버전 자동 업그레이드 사용` : 보안 패치나 버그가 수정된 버전을 자동으로 업데이트합니다.
* [유지 관리 기간](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.Maintenance.html#Concepts) : RDS가 DB 리소스를 유지 관리하기 위해서는 기본 하드웨어, OS, DB 엔진 버전에 대한 업데이트가 수반될 수 있는데, 이때 업데이트 이전에 `DB 인스턴스 클래스` 또는 `파라미터 그룹`을 유지 관리 기간 내에 변경하도록 할 수 있습니다.

생성 버튼을 누르면 몇 분 뒤에 `DB 인스턴스` 생성이 완료됩니다.   

> <h3>보안 그룹 생성</h3>

이제 DB 인스턴스용 보안 그룹을 생성하겠습니다.   

[EC2 콘솔] > [보안 그룹] > [보안 그룹 생성]   
![image](https://user-images.githubusercontent.com/43658658/146330774-af3a95d3-d678-454d-898b-ddd9857f7ce3.png)

인바운드 규칙에서 `사용자 지정`에 EC2 인스턴스를 만들면서 지정한 보안 그룹을 지정합니다.   
![image](https://user-images.githubusercontent.com/43658658/146333576-d1c3201b-95bb-4bf9-9ac2-9f3ca72ff7f5.png)   
* 이렇게 보안 그룹을 규칙으로 지정하면, 같은 보안 그룹을 이용하는 인스턴스들 모두에서 접근이 가능하게 됩니다.

[DB 인스턴스] > [수정]   
![image](https://user-images.githubusercontent.com/43658658/146331422-9a33e832-6d58-453d-bb24-4bd65a36713f.png)

기존의 `Default`를 지우고 앞서 생성한 `DB 인스턴스 전용 보안 그룹`을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146331555-ff0a500b-30ad-4fb5-ad7b-7d05091e8c92.png)

아래에 [계속] 버튼을 누르고 `즉시 적용`을 선택합니다.
![image](https://user-images.githubusercontent.com/43658658/146331897-250528c1-483a-4531-8c36-0cfbc629dae2.png)

## DB 인스턴스 접근

실제로 외부에서 DB 인스턴스가 제대로 생성되었는지 접근해보겠습니다.   

[MySQL Workbench 다운로드](https://dev.mysql.com/downloads/workbench/)를 이용해 MySQL Workbench를 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/146334060-d92aa37f-9539-4d8f-9c31-d60d5802fa62.png)

설치가 완료되면 상단의 [Database] > [Manage Connections]   
![image](https://user-images.githubusercontent.com/43658658/146334901-54edab64-4182-45ee-82cc-058fd414ce8d.png)

좌측 하단에 `New`를 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/146339150-ee6b2245-13b9-4c48-b5ee-370d3ef37e56.png)

아래의 설정값들을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146334822-dc3ef873-9f6a-444a-8fe7-64f0b6f2f0c3.png)    
* `Connection Name` : 연결의 이름을 입력합니다.
* `Hostname` : RDS의 엔드포인트를 적어줍니다.
* `Username` : RDS 생성 당시 마스터 사용자를 적어줍니다.
* `Store in Vault` : 눌러서 마스터 사용자의 비밀번호를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146335232-9660b9cc-8396-43c8-87de-11ac598e1ea2.png)

[Test Connection]을 눌렀을 때 아래의 화면이 뜨면 성공적으로 연결이 되는 것입니다.   
![image](https://user-images.githubusercontent.com/43658658/146339241-bcb0e65b-d6a3-431a-aa27-9c1cda92bde0.png)

이제 [Database] > [Connect to Database]로 가서 방금 생성한 `Connection`을 선택하고 RDS 내에 있는 MySQL과 연결합니다.   
![image](https://user-images.githubusercontent.com/43658658/146339397-f2b42c6b-5528-44f0-a09e-d0ef45c8f1be.png)   
![image](https://user-images.githubusercontent.com/43658658/146339486-0327b42d-d74e-4fd5-a785-624f034e9a68.png)

> <h3>새 테이블 생성</h3>

RDS를 생성할 때 초기 DB로 생성한 DB가 있습니다. [Table] > [Create Table]   
![image](https://user-images.githubusercontent.com/43658658/146340095-0fabeda9-e6ad-4206-840e-1324425de91d.png)

![image](https://user-images.githubusercontent.com/43658658/146340598-127205c6-6f1e-4c1e-8932-dc456bb89cd7.png)   
* Table Name: 테이블 이름입니다. ExampleTable을 입력합니다.
* Column Name: id, DataType: INT, PK 체크, NN 체크, AI 체크
* Column Name: name, DataType: VARCHAR(45)
* Column Name: address, DataType: VARCHAR(45)

`Apply`를 클릭해서 RDS에 반영합니다.   
![image](https://user-images.githubusercontent.com/43658658/146340742-990ae7da-bb66-495c-99a2-2d0b83b9819b.png)

`ExampleTable`이 생성되었습니다. [우클릭] > [Select Rows - Limit 1000]   
![image](https://user-images.githubusercontent.com/43658658/146341071-dbfa3b0c-6adc-4b1d-9c16-7e8837db94e2.png)

레코드를 입력하고 적용하기 위해 `Apply`를 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/146341245-d85e1523-7b87-4939-99b0-7831c510bab7.png)

## 스냅샷

> <h3>스냅샷 생성</h3>

DB 인스턴스를 클릭하고 [작업] > [스냅샷 생성]   
![image](https://user-images.githubusercontent.com/43658658/146343551-c741a1b7-7e98-42a1-a410-52ecec9aa85e.png)

이름을 지정해주고 스냅샷을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146343694-479507ca-cfbb-4d4c-af9a-c8fe5e0c88df.png)

생성하기까지 시간이 약간 소요됩니다.   
![image](https://user-images.githubusercontent.com/43658658/146343824-05f0e3ad-a2a1-4a7e-bec0-d373b71bd4cb.png)

스냅샷과 백업의 가장 큰 차이점은 DB 인스턴스를 삭제할 때, 백업 파일은 같이 지워지지만, 스냅샷은 지워지지 않는다는 것입니다.   
스냅샷으로 다른 RDS DB 인스턴스를 만들 수 있습니다.

> <h3>스냅샷으로 RDS DB 인스턴스 생성</h3>

생성한 스냅샷을 클릭하고 [스냅샷 복원]을 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/146344672-683ef2e3-d7e6-489f-ac99-a132067239b6.png)

DB 엔진과 이름을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/146345139-802f07ef-8150-4f91-b3a9-625b638fb197.png)

퍼블릭 액세스를 허용해주고, 보안 그룹을 이전에 생성했던 DB 보안 그룹을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146345363-47b89946-5d87-4eaf-8c83-9d4d3ebdefe1.png)

나머지 구성은 앞선 RDS 생성 설정과 동일하게 선택합니다.   

스냅샷을 통해 DB 인스턴스가 생성되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/146365993-2eb21592-2dfe-45bf-813e-7f36790e1e49.png)

`MySQL Workbench`를 이용해 접속 테스트를 진행합니다.   

접속이 원활하게 수행된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146363558-a38e997d-90f0-4761-a871-7ebbe917e69a.png)

> <h3>다른 리전으로 복사</h3>

DB 인스턴스는 다른 리전으로 복사할 수 없기 때문에 `스냅샷을 통해` 복사합니다.

DB 인스턴스 스냅샷을 선택하고 [작업] > [스냅샷 복사]   
![image](https://user-images.githubusercontent.com/43658658/146364541-279f6608-8f8c-4440-981e-432baa3cb06d.png)

대상 리전과 식별자를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146364945-410ae542-ddf6-4a2e-b0ad-04a7e14fcdc4.png)

다른 리전에 스냅샷이 복사되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/146365730-c3e431da-08a6-49ef-81b1-faab0a4cd8f2.png)

이제 이 스냅샷을 가지고 같은 DB 인스턴스를 생성할 수 있습니다.

## 자동 백업 활용

DB 인스턴스를 생성할 때 `자동 백업 설정`을 했다면 DB의 `특정 시점`을 `인스턴스로 생성`할 수 있습니다.   
이미 있는 DB 인스턴스의 내용을 되돌리는 것이 아니라 특정 시점의 내용을 DB 인스턴스로 `새롭게 생성`하는 방식입니다.

기존에 생성했던 DB 인스턴스에 레코드를 한 줄 더 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/146366537-f9d1dc7c-1822-4cd8-a490-4b49cbcd5564.png)

[RDS 콘솔] > [자동 백업] > [작업] > [특정 시점으로 복원]   
![image](https://user-images.githubusercontent.com/43658658/146366613-196d4a09-005a-4fc3-acc8-058dab558e14.png)

`복원 시점`과 `인스턴스 식별자`, 그리고 나머지 RDS 생성 당시 설정과 동일하게 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/146367682-bb917bd2-9f56-4754-a944-d8cd8e213d41.png)

생성을 진행합니다. 10~15분 정도 소요됩니다.   
![image](https://user-images.githubusercontent.com/43658658/146367392-8cf87438-79e4-4ba5-8e72-5a8df550c634.png)

생성이 완료되면 `MySQL Workbench`를 통해 DB로 접속할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146370551-c339342a-793b-4e86-b824-e357c434ea4f.png)

3번째 레코드를 추가하기 이전 상태로 돌아가서 DB 인스턴스가 생성된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146370447-e55029c0-5fbe-478b-9adf-09ce545d1f2e.png)

## 읽기 전용(Read Replica) 복제본 생성

서비스에서 읽기 위주의 작업이 많을 경우 DB 인스턴스는 `읽기 전용 복제본`을 생성해서 부하를 분산할 수 있습니다.

원본 DB 인스턴스(`마스터`)는 `쓰기` 작업만 처리하고, 읽기 전용 복제본(`슬레이브`)에서 `읽기` 작업을 담당해서 원본 DB 인스턴스(마스터)의 부하를 줄일 수 있습니다.

마스터에 쓰기를 하면 자동으로 슬레이브로 데이터가 복제됩니다.   
단 쓰기 작업을 실시한 즉시 복제되는 것은 아니며 `약간의 시간차`가 있습니다.

[RDS 콘솔] > [작업] > [읽기 전용 복제본 생성]   
![image](https://user-images.githubusercontent.com/43658658/146368885-1141fb36-2bf6-4782-af14-204dc8cf7f95.png)

RDS 생성 설정과 상당히 유사합니다. RDS를 생성할 때처럼 생성해주고 [읽기 전용 복제본 생성]을 눌러줍니다.   
![image](https://user-images.githubusercontent.com/43658658/146369268-f2758708-4f44-4933-a1b6-abe851fc9428.png)   
![image](https://user-images.githubusercontent.com/43658658/146369403-9bbc1136-620a-4b10-b17c-161e4748736d.png)   
![image](https://user-images.githubusercontent.com/43658658/146369314-514963b2-cdfa-4326-8ff5-ab160f5804ad.png)   

마스터 DB 인스턴스도 설정을 해야 하기 때문에 `수정 중`으로 표시가 되고, 읽기 전용 복제본이 생성됩니다.   
![image](https://user-images.githubusercontent.com/43658658/146369693-0addae45-9cfe-44da-9e88-28e9130a3fa6.png)

복제본의 엔드포인트를 통해 DB에 접속해서 값을 추가한 뒤 `Apply`를 누르면 에러가 발생합니다.   
![image](https://user-images.githubusercontent.com/43658658/146371082-b01969c6-7b78-497b-b186-74d3b0163083.png)

읽기 전용 복제본으로 생성된 인스턴스는 읽기만 허용되기에 값을 `추가/수정/삭제`할 수 없습니다.





