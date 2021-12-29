# VPC(Virtual Private Cloud)

: 가상 네트워크를 제공합니다. 사용자가 원하는 요구에 따라 `VPC`를 생성하여 여러 가지 형태의 네트워크를 구성할 수 있습니다.

## 서브넷(Subnet)

VPC 안에는 `서브넷`을 여러 개 추가할 수 있습니다.   

서브넷을 여러 개로 나누면 네트워크를 격리할 수 있고, 이 서브넷 간에 접근제어(ACL)를 설정할 수 있습니다.

인터넷에서 접근해야 하는 웹 서버는 Public Subnet에 만들고, 외부에서 접근이 필요 없는 데이터베이스 서버는 Private Subnet에 만들어서 구성할 수 있습니다.

VPC는 `리전별`로 생성하고, 서브넷은 `가용 영역` 별로 생성합니다.

## CIDR(Classless Inter-Domain Routing)

: IP 주소 할당 방법입니다. xxx.xxx.xxx.xxx/yy 형태로 표기하는데 맨 뒤의 yy는 서브넷 마스크를 2진수로 바꾸었을 때 1의 개수입니다.   

예를 들어,   
`192.168.0.0/24`라면 `192.168.0.1 ~ 192.168.0.254` 범위의 IP를 의미합니다(192.168.0.0는 네트워크 192.168.0.255는 브로드캐스트).   
`192.168.0.15/32`라면 `192.168.0.15` 단 한 개를 의미합니다.

## VPC 생성하기

[VPC 콘솔] > [VPC] > [VPC 생성]   
![image](https://user-images.githubusercontent.com/43658658/147619004-6970540c-fbb1-4512-ac54-ab2c58596f0e.png)

이름과 IP 주소, 테넌시를 설정해주고 VPC를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147619143-47deac63-9c55-450a-9257-1f05edc485fb.png)   
* `IPv4 CIDR` : VPC의 IPv4 주소를 입력합니다. CIDR로 입력합니다. 
  - VPC는 전체 네트워크를 설정하는 것입니다. 서브넷 마스크를 16으로 지정해주어 나중에 서브넷을 24로 지정할 수 있도록 해줍니다.
* `테넌시` : VPC 내에서 EC2 인스턴스를 생성 시 전용 테넌시 인스턴스가 되도록 해주는 옵션입니다(기본/전용).

## 서브넷 생성하기

[VPC 콘솔] > [서브넷] > [서브넷 생성]   
![image](https://user-images.githubusercontent.com/43658658/147619368-aedcebcc-0d8e-46f3-8918-cbc828f49c15.png)

서브넷을 생성할 VPC를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147619411-bc29601e-0d6f-4c77-8e53-9f67bb621e61.png)

서브넷 이름, 가용 영역, CIDR를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147619499-931a971f-2b2b-4f2b-ad54-76a257924913.png)   
* 서브넷은 가용 영역 별로 생성할 수 있습니다.
* 서브넷의 CIDR는 VPC의 네트워크 내에 생성되는 것이므로, VPC의 IP 주소를 따라갑니다. 
* 서브넷은 가용 영역 별로 다른 CIDR로 설정해줍니다.
  - 예를 들어, `2a` 영역에 `192.168.0.0/24`로 설정했다면, `2b` 영역은 `192.168.1.0/24`로 설정합니다.

## 인터넷 게이트웨어 생성하기

VPC에 속한 서브넷에서 `외부 인터넷`에 접속하기 위해서는 인터넷 게이트웨이가 필요합니다.

[VPC 콘솔] > [인터넷 게이트웨이] > [인터넷 게이트웨어 생성]   
![image](https://user-images.githubusercontent.com/43658658/147619693-85b11da0-4526-4f5d-9a94-75512ea7903a.png)

인터넷 게이트웨이의 이름만 입력하고 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147619794-590fde62-91e9-42e1-bf0b-cd6a6ab2ef8d.png)

생성된 게이트웨이를 선택하고 [우클릭] > [VPC에 연결]   
![image](https://user-images.githubusercontent.com/43658658/147619837-962a28cd-70b5-4a62-a0b4-bc13952c53f6.png)

인터넷 게이트웨이를 연결할 VPC를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147620017-bb13f82c-fd58-4aa5-896c-98c38f83d65e.png)

보통 계정마다 `디폴트 VPC`와 `인터넷 게이트웨이`가 있기 때문에 이전까지는 이러한 설정이 없었어도 외부 인터넷에 연결이 잘 되었습니다.

하지만 당연한듯이 보였던 외부 인터넷 접속도 사실 `인터넷 게이트웨이`가 있었기 때문에 가능했던 것입니다.

## 라우팅 테이블

서브넷 또는 게이트웨이의 네트워크 트래픽이 전송되는 위치를 결정하는 데 사용되는 라우팅이라는 규칙 집합이 포함되어 있습니다.   

인터넷 게이트웨이와 마찬가지로 외부 인터넷에 접속하기 위해 반드시 필요한 설정이고, 기본적으로 디폴트 라우팅 테이블이 존재합니다.

라우팅 테이블을 생성하는 방법은 아래와 같습니다.

[VPC 콘솔] > [라우팅 테이블] > [라우팅 테이블 생성]   
![image](https://user-images.githubusercontent.com/43658658/147620646-930ae5a0-9be1-4ea4-9e33-915794bc159c.png)

라우팅 테이블 이름과 VPC를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147620582-9510302a-e2c4-4b87-bfb0-8f638ffa7679.png)

생성한 라우팅 테이블에서 [우클릭] > [라우팅 편집]   
![image](https://user-images.githubusercontent.com/43658658/147620633-a060ec83-2237-4c2d-aa53-3818ffb59e7f.png)

라우팅 규칙을 추가해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/147620709-75552b26-9b25-425a-aefe-e9389cfcd593.png)   
* 게이트웨이를 통해 VPC로 들어오는 인바운드 트래픽에 대한 라우팅 규칙을 지정할 수 있습니다.
* 라우팅 목적지는 VPC 내의 모든 IP 주소(0.0.0.0/0)입니다.
* 인터넷 게이트웨이에 라우팅 규칙을 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/147620868-1d0986b2-7762-415a-8076-7f5dddfbeafb.png)   
* 이로써 서브넷이 게이트웨이를 통해 외부 인터넷에 액세스할 수 있습니다.

## VPC 삭제

서브넷과 인터넷 게이트웨이는 VPC 내에 속해있기 때문에 VPC를 삭제하면 그 안에 속해있던 구성요소들도 삭제됩니다.   
![image](https://user-images.githubusercontent.com/43658658/147620194-4a2c1a6e-9cc7-4a52-858c-c05e7e3e747e.png)


























