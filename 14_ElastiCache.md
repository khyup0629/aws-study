# ElastiCache

: 분산 `인 메모리 캐시(In-Memory Cache)`를 손쉽게 생성하고 확장할 수 있는 서비스입니다.

## 인 메모리 캐시

: 모든 데이터를 메모리(RAM)에만 올려놓고 사용하는 데이터베이스의 일종입니다.   

일반적인 데이터베이스(RDBMS)는 디스크(HDD, SSD)에 데이터를 영구적으로 저장해놓고, 필요한 데이터만 메모리에 읽어서 사용합니다.

## ElastiCache 사용 이유

* `분산 캐시 환경 구축`에 필요한 `비용을 절감`합니다.   
* 읽기 중심의 서비스(소셜 네트워크, 게임, 추천 엔진 등)를 제공해야 하는 환경, `고속으로 데이터를 분석해야 하는 환경`에 적합합니다.

## ElastiCache 장단점

인 메모리 캐시는 모든 데이터를 메모리에 올려놓는 것이 특징입니다. 디스크에 접근하지 않고 `메모리로만` 모든 처리를 하기 때문에 데이터 저장 및 검색 속도가 `매우 빠릅니다.` 

데이터는 딱 `메모리 크기`까지만 저장할 수 있습니다. 또한, 메모리에만 저장되어 있기 때문에 서버의 전원 공급이 중단되면 `데이터는 소멸`됩니다.

## ElastiCache Memcached 클러스터 생성

> <h3>Memcached 캐시 엔진</h3>

ElastiCache는 두 가지 캐시 엔진을 지원합니다.

두 가지중 하나가 `Memcached`입니다.

![image](https://user-images.githubusercontent.com/43658658/146862631-b0ed9e2a-8fd5-42e8-9586-2558ad6e6eb7.png)   
Memcached를 이용하면 클러스터를 손쉽게 생성할 수 있고, 클러스터는 리전의 가용 영역 별로 생성할 수 있습니다.   
클러스터 내에 Memcached 노드들을 여러 개 추가할 수 있습니다. 노드를 추가할 수록 더 많은 데이터를 저장할 수 있습니다.

하지만 Memcached는 스냅샷 생성과 `읽기 전용 복제본`을 지원하지 않습니다.

> <h3>클러스터 생성</h3>

[ElastiCache 콘솔] > [Memcached] > [생성]   
![image](https://user-images.githubusercontent.com/43658658/146865319-fed53fe9-8011-408f-9e7f-6eaf12e623a0.png)

![image](https://user-images.githubusercontent.com/43658658/146864792-80ad9725-21f5-4b6c-a5f6-aa488c8657eb.png)   
![image](https://user-images.githubusercontent.com/43658658/146865054-f61eed98-7da7-4c1d-9c8b-29bd09eb471c.png)   
* 프리티어 버전인 `t2.micro` 버전을 이용합니다.
* 보안 그룹은 이후에 클러스터 전용으로 따로 생성해주어야 합니다.

생성 버튼을 누르면 생성되는데 `5분` 정도 소요됩니다.

생성이 완료되면 클러스터의 엔드포인트 주소를 통해 `캐시 노드`로 접속할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146882206-300dae2b-afb5-4846-84d8-9787b92bc7f6.png)   
![image](https://user-images.githubusercontent.com/43658658/146873390-0fb15dbb-62e5-43b7-ae99-86ac12fe0758.png)

> <h3>보안 그룹 설정</h3>

클러스터와 캐시 노드를 생성해도 엔드포인트로 접속하면 접속되지 않습니다.   
보안 그룹을 지정해서 `11211` 포트를 열어주어야 합니다.

[EC2 콘솔] > [보안 그룹] > [보안 그룹 생성]에서 11211 포트의 인바운드 규칙을 허용하는 정책을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/146873087-1e02d95a-1368-444a-84fa-94075341e172.png)   
* VPC는 나중에 인스턴스를 통한 텔넷 접속을 위해 인스턴스의 VPC와 같아야합니다.

[ElastiCache 콘솔] > [Memcached] > [클러스터 선택] > [작업] > [수정]에서 보안 그룹을 방금 생성한 것으로 수정합니다.   
![image](https://user-images.githubusercontent.com/43658658/146873251-169a355f-12d2-4d84-83ce-63bf54b6f32e.png)

생성한 클러스터를 클릭해서 들어가면 `캐시 노드`들을 볼 수 있습니다. 여기서 엔드포인트 주소를 알 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146882176-6217a2c1-96d4-4dea-806a-fe78b50db946.png)   
![image](https://user-images.githubusercontent.com/43658658/146873390-0fb15dbb-62e5-43b7-ae99-86ac12fe0758.png)

`Memcached`는 `같은 VPC`에 있는 인스턴스를 통해 `텔넷`으로 접속할 수 있습니다.   

인스턴스로 접속해서 `sudo yum -y install telnet` 명령으로 텔넷을 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/146873960-4b81cb68-159f-4faa-9ddc-1d1395d824b7.png)

인스턴스를 같은 VPC로 생성한 후 `telnet <엔드포인트 주소> 11211`을 명령해서 접속합니다.   
![image](https://user-images.githubusercontent.com/43658658/146874073-95d902f1-74a6-4dfa-a66b-f3e6a4fc39ab.png)

텔넷 접속 후 `stats`를 명령했을 때 아래와 같이 리스트가 나타나면 성공적으로 접속된 것입니다.   
![image](https://user-images.githubusercontent.com/43658658/146874155-f6a996f0-d679-4f8f-b871-a7d9566e33af.png)

## Memcached 클러스터에 캐시 노드 추가

캐시 노드를 추가해서 사용할 수 있는 메모리 용량을 확장할 수 있습니다.

`캐시 노드`를 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/146882176-6217a2c1-96d4-4dea-806a-fe78b50db946.png)   
![image](https://user-images.githubusercontent.com/43658658/146882282-e6ba3a32-52b4-4015-98d2-cb4c5dae5f4a.png)

추가할 노드 개수와 가용 영역, 즉시 추가할 것인지 여부를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146883095-6c9f0892-e638-46f0-9b17-da1a600d9c77.png)   
* `즉시 적용` : `아니요`로 할 시 유지 관리 기간에 추가됩니다.

`약 5분`정도 기다리면 캐시 노드가 완전히 추가됩니다.   
![image](https://user-images.githubusercontent.com/43658658/146883563-f8b910c1-2c0e-4ee5-8112-4915c9a525b1.png)

엔드포인트로 인스턴스를 통해 텔넷 접속을 시도해봅니다.   
![image](https://user-images.githubusercontent.com/43658658/146883823-24f6b608-99ce-4d87-b825-b24e3bd73813.png)

## Redis 클러스터 생성

> <h3>Redis 캐시 엔진</h3>

ElastiCache에서 사용하는 두 가지 엔진 중 또 다른 하나는 `Redis`입니다.

![image](https://user-images.githubusercontent.com/43658658/146884301-2b698e57-ba6d-4cc1-95ec-73078b611678.png)   
Redis는 Memcached와 달리 용량을 추가하며 클러스터를 구성할 수 없습니다. 따라서 전체 메모리 용량이 늘어나지는 않습니다.   
그리고 데이터를 저장할 수 있는 메모리 용량은 각 캐시 노드가 제공하는 메모리 용량에 한합니다.

`스냅샷 생성`과 `읽기 전용 복제본(Read Replica)`를 지원합니다. 특히 Read Replica는 `마스터 캐시 노드`에 장애가 발생하면 자동으로 Read Replica가 마스터 캐시 노드로 승격되는 `Failover 기능`이 작동하게 됩니다. 이후 새 마스터 캐시 노드에서 쓰기 작업이 진행되게 됩니다.   
Read Replica는 Replication Group이라는 논리적인 그룹 안에 위치합니다. 그리고 Read Replica는 한 리전 안에서 여러 가용 영역에 생성할 수 있습니다.

> <h3>Redis 클러스터 생성</h3>

Redis가 구성하는 클러스터는 마스터 캐시 노드(쓰기)와 읽기 전용 복제본들의 집합을 의미합니다.

[ElastiCache 콘솔] > [Redis] > [생성]에서 클러스터를 생성합니다.   

아래와 같이 Redis를 설정하고 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146884954-613599db-bdfe-46dc-bde0-e6037e42030d.png)
* 프리티어에서 사용가능한 노드 유형을 선택합니다.
* `복제본 개수` : 생성할 읽기 전용 복제본의 개수를 입력합니다.
* `다중 AZ` : 페일오버 기능을 활성화 시킬지 여부를 선택합니다.

생성 버튼을 누르면 `약 5분` 후 클러스터가 생성됩니다.   

Memcached와 마찬가지로 클러스터를 눌러서 들어가면 캐시 노드를 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146886267-cdba7e18-4bf0-4ecc-892e-860c98bfaba7.png)

캐시 노드의 엔드포인트를 통해 캐시 노드로의 접속이 가능합니다.   
![image](https://user-images.githubusercontent.com/43658658/146886304-65917551-767f-4991-989d-104721692372.png)   
* 마스터 캐시 노드와 읽기 전용 복제본(Read Replica) 2개가 생성되어 총 3개의 캐시 노드가 있습니다.

> <h3>보안 그룹 생성</h3>

[EC2 콘솔] > [보안 그룹] > [보안 그룹 생성]을 통해 인바운드 규칙으로 `6379` 포트를 모두에게 허용한 정책을 생성해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/146886200-3f2c55e7-6bf9-4a77-b2ff-40cb40f86237.png)

[ElastiCache 콘솔] > [Redis] > [클러스터 선택] > [수정]을 통해 방금 생성한 보안 규칙을 선택하고 적용합니다.   
![image](https://user-images.githubusercontent.com/43658658/146886683-8b4e7014-3b1f-4d63-b3ad-454d29c1205c.png)

마스터 캐시 노드의 엔드포인트를 확인하고 EC2 인스턴스를 통해 `telnet <엔드포인트 주소> 6379` 명령을 통해 텔넷 접속합니다.   
![image](https://user-images.githubusercontent.com/43658658/146887159-679bc9a7-25ab-49fa-954b-5498e06701dd.png)   







