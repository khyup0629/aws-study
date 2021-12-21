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

> <h3>생성 과정</h3>

[ElastiCache 콘솔] > [Memcached] > [생성]   
![image](https://user-images.githubusercontent.com/43658658/146865319-fed53fe9-8011-408f-9e7f-6eaf12e623a0.png)

![image](https://user-images.githubusercontent.com/43658658/146864792-80ad9725-21f5-4b6c-a5f6-aa488c8657eb.png)   
![image](https://user-images.githubusercontent.com/43658658/146865054-f61eed98-7da7-4c1d-9c8b-29bd09eb471c.png)   
* 프리티어 버전인 `t2.micro` 버전을 이용합니다.
* 보안 그룹은 이후에 클러스터 전용으로 따로 생성해주어야 합니다.

생성 버튼을 누르면 생성되는데 `5분` 정도 소요됩니다.

생성이 완료되면 클러스터의 엔드포인트 주소를 통해 `캐시 노드`로 접속할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146865573-1b4ceb4a-fe21-4406-823d-5382e820c1b7.png)













