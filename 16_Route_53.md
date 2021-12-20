# Route 53

: EC2, ELB, S3, CloudFront와 연동 가능한 `DNS 서비스`입니다.

기존 DNS 서비스는 도메인 하나를 쿼리하더라도 각 상황에 따라 다른 IP 주소를 알려줍니다.   
Route 53의 기능들을 사용하면 도메인 하나를 쿼리하더라도 각 상황에 따라 다른 IP 주소를 알려줍니다.

## Route 53 기능

AWS를 사용하여 대규모 글로벌 서비스를 할 때 유용합니다.   
AWS 리소스들과 연동할 수 있는 것과 더불어 글로벌 서비스에 꼭 필요한 기능들을 제공해줍니다.

* `지연 시간` : 현재 위치에서 `지연 시간`이 가장 낮은 리전의 IP 주소를 알려줍니다. 
* `가중치 기반` : 서버 IP 주소 또는, 도메인(ELB) 마다 가중치를 부여하여 트래픽을 조절하는 기능입니다.
* `장애 조치` : 장애가 발생한 서버의 IP 주소 또는 도메인(ELB)으로는 트래픽을 막는 기능입니다.
* `지리적 위치` : 지역별로 다른 IP 주소를 알려줍니다.
* `다중값 응답` : 하나의 도메인에 여러 개의 값(IP 주소)이 연결됩니다.

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

## A 레코드 설정(EC2 인스턴스와 연동하기)

: DNS 서버에서 IP 주소를 알려주도록 설정하는 기능입니다.

실습 이전에 먼저 EC2 인스턴스를 생성하고 탄력적 IP를 할당해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/146734857-6cae3aef-acd1-494c-9855-d50edcc89ae9.png)

아파치 웹 서버를 설치해줍니다.   
=> [아파치 웹 서버 설치 과정]()   
![image](https://user-images.githubusercontent.com/43658658/146737430-cc7288fd-a777-47a2-8296-70c331bdd865.png)

다시 [Route 53 콘솔] > [호스팅 영역] > [원하는 도메인] > [도메인 생성]   
![image](https://user-images.githubusercontent.com/43658658/146735154-8a7f8773-6c0c-43cf-b70f-03be0d612453.png)

레코드를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146735368-2c34f045-778f-46c8-a489-50d83e083121.png)   
* `레코드 이름` : 서브 도메인 이름을 설정합니다.
* `레코드 유형` : A 레코드로 DNS가 IPv4를 가리키도록 할 것이므로 `A - IPv4`를 선택합니다.
* `값` : DNS가 가리킬 IP 주소를 입력합니다.
* `TTL` : IP 주소가 갱신되는 주기를 `초`단위로 입력합니다.
  - 예를 들어, 300이라면, 다음에 A 레코드의 IP 주소를 바꿨을 때 5분 뒤에 갱신됩니다.
* `라우팅 정책` : 지연시간, 가중치기반, 지리적위치, 장애조치, 다중값 응답의 기능을 사용할 수 있습니다.

`A 레코드`를 생성하고 A 레코드로 생성한 도메인으로 들어가면 접속이 원활히 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/146737342-35164df0-537a-4717-add2-477f8db993ed.png)

## S3와 연동하기

S3와 Route 53을 연동하기 위해서는 버킷을 2개 생성해야 합니다.   

먼저 정적 웹 호스팅 버킷을 생성하겠습니다.   
Route 53과 연동하는 버킷을 생성할 때는 버킷 이름을 도메인 이름과 같게 해야 합니다.   
![image](https://user-images.githubusercontent.com/43658658/146739067-ec0c65a0-b379-4a83-8b5a-0f7090b5f402.png)

생성한 버킷의 정책을 아래와 같이 편집합니다.   
![image](https://user-images.githubusercontent.com/43658658/146739247-4629213d-7e94-4f89-b661-fe2ea93f006b.png)

[속성] > [정적 웹 사이트 호스팅]을 활성화하고 인덱스 문서를 `index.html`로 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146739418-68e2e386-989e-49d0-85de-fafa116e0860.png)

아래와 같은 내용의 `index.html` 파일을 정적 웹 호스팅 버킷에 업로드합니다.   
``` html
<html>
<head>
    <title>Example CloudFront Distribution</title>
</head>
<body>
    <p>Hello Route 53 - S3</p>
</body>
</html>
```

![image](https://user-images.githubusercontent.com/43658658/146739746-172e9331-55ce-4cda-b551-4912e11ba7c0.png)

이제 두 번째 버킷을 생성합니다.   
버킷 이름은 서브 도메인으로 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146739907-9b518d85-7fe2-465d-b5e6-8da756ce521d.png)

[속성] > [정적 웹 사이트 호스팅]을 활성화하고, `객체에 대한 요청 리디렉션`을 선택하고 `호스트 이름`을 루트 도메인으로 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146740220-cb8191e8-6864-489a-8ca1-ac45276e6f3e.png)

이제 `bllu.` 도메인으로의 트래픽은 모두 루트 도메인으로 리디렉션하게 설정되었습니다.

Route 53 콘솔에서 루트 도메인에 대한 A 레코드를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146741211-cb2ead6e-7eaf-46ef-bb23-0558396142ec.png)   
* `레코드 이름` : 루트 도메인에 대한 A 레코드 생성이므로, 아무것도 작성하지 않습니다.
* `별칭`을 켜주면 `트래픽 라우팅 대상` 항목이 변경됩니다.
* S3 엔드포인트 > 리전 선택 > 루트 도메인 제목으로 된 버킷 선택

다음으로 서브 도메인에 대한 CNAME 레코드를 생성해주겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/146741764-79a73131-8132-4952-9b4b-36efbf70ba64.png)   
* `값` : 서브 도메인 버킷의 정적 웹 사이트 호스팅 엔드포인트 주소를 `http://`를 빼고 입력합니다.
* `CNAME` : 현재 서브 도메인을 다른 도메인으로 연결하는 기능입니다.

이제 `bllu.hongikit.com` 또는 `hongikit.com`으로 







































