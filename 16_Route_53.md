# Route 53

: EC2, ELB, S3, CloudFront와 연동 가능한 `DNS 서비스`입니다.

기존 DNS 서비스는 도메인 하나를 쿼리하더라도 각 상황에 따라 다른 IP 주소를 알려줍니다.   
Route 53의 기능들을 사용하면 도메인 하나를 쿼리하더라도 각 상황에 따라 다른 IP 주소를 알려줍니다.

## Route 53 기능

AWS를 사용하여 대규모 글로벌 서비스를 할 때 유용합니다.   
AWS 리소스들과 연동할 수 있는 것과 더불어 글로벌 서비스에 꼭 필요한 기능들을 제공해줍니다.

* `Latency Based Routing` : 현재 위치에서 지연 시간Latency이 가장 낮은 리전의 IP 주소를 알려줍니다. 
* `Weighted Round Robin` : 서버 IP 주소 또는, 도메인(ELB) 마다 가중치를 부여하여 트래픽을 조절하는 기능입니다.
* `DNS Failover` : 장애가 발생한 서버의 IP 주소 또는 도메인(ELB)으로는 트래픽을 막는 기능입니다.
* `Geo Routing` : 지역별로 다른 IP 주소를 알려줍니다. 

## Route 53에서 도메인 구매

[Route 53 콘솔] > [등록된 도메인] > [도메인 등록]   
![image](https://user-images.githubusercontent.com/43658658/146714064-65a0bc25-d5ac-435e-9399-a8cc8b381c83.png)

원하는 도메인 이름을 쓰고 [확인] 버튼을 누르면 가용성과 관련 도메인을 추천해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/146714143-19deff8b-f6ef-47fa-ac73-4ba88af65764.png)

다음 페이지에서 도메인에 대한 연락처 세부 정보를 입력합니다.

![image](https://user-images.githubusercontent.com/43658658/146714598-a976edce-894b-48a8-9219-e82858634a97.png)   
* 1년이 지났을 때 해당 도메인을 자동으로 갱신할 것인지 활성화/비활성화 여부를 체크합니다.
* 약관에 동의하고 [주문 완료]를 누릅니다.

## 호스팅 영역

[Route 53 콘솔] > [호스팅 영역] > [호스팅 영역 생성]   
![image](https://user-images.githubusercontent.com/43658658/146712399-207197c7-58a7-4e16-9f19-9ea83220b501.png)

구매한 도메인을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146712501-bcef8df5-1abd-4c42-a3ee-8e94a12a2a2e.png)
* `퍼블릭 호스팅 영역` : 트래픽을 인터넷에서 라우팅합니다.
* `프라이빗 호스팅 영역` : 트래픽을 VPC에서 라우팅합니다.

도메인을 생성하면 트래픽 라우팅 대상에 네임 서버가 4개 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146712792-9aab76a0-ba58-444a-a2af-983097edc72a.png)

도메인을 구입한 사이트의 `도메인 관리`로 들어가서 `네임 서버`를 편집합니다.   
![image](https://user-images.githubusercontent.com/43658658/146712953-6636da49-c3a0-4f3f-9beb-ef7e1ee77ef4.png)

`네임 서버`를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146713265-cb83b96b-c6ad-4416-88e7-27d6904295b5.png)









































