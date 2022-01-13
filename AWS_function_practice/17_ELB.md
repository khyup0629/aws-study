# ELB

서버의 부하 분산과 고가용성을 제공하는 서비스입니다.

하나의 ELB에 집중되는 HTTP, TCP, SSL 트래픽을 ELB와 연결된 여러 EC2 인스턴스로 분산합니다.   
그리고 서버가 정상적으로 가동 중인지 확인(Health Check)하여 일부 인스턴스가 중단되더라도 트래픽을 정상 인스턴스로만 보냅니다.

`리전별`로 생성해야 하고, 여러 `가용 영역`에서 실행되는 인스턴스로 부하를 분산시킬 수 있습니다.

## ELB 개념

* L4 : OSI 레이어에서 4번째 전송 계층을 뜻합니다. TCP, UDP 등의 프로토콜이 대표적이며 포트 번호로 구분합니다. 
  - 보통 OSI 레이어에서 3번째 네트워크 계층의 IP와 묶어서 처리합니다. 
  - `L4 로드밸런싱`이라고 하면 IP 주소와 포트 번호를 기준으로 트래픽을 분배합니다.
* L7 : OSI 레이어에서 7번째 애플리케이션 계층을 뜻합니다. HTTP 프로토콜이 대표적입니다. 
  - `L7 로드밸런싱`이라고 하면 HTTP 헤더의 내용을 기준으로 트래픽을 분배합니다.
* 헬스 체크(Health Check) : EC2 인스턴스가 정상적으로 가동 중인지 확인하는 기능입니다.
* 라운드 로빈(Round Robin) : ELB 알고리즘으로 우선 순위를 두지 않고 순서대로 분배하는 방식입니다.
* Connection Draining : `Auto Scaling`이 사용자의 요청을 처리 중인 EC2 인스턴스를 바로 삭제하지 못하도록 방지하는 기능입니다.
* Sticky Sessions : 사용자의 세션을 확인하여 적절한 EC2 인스턴스로 트래픽을 분배하는 기능입니다. L7 로드밸런싱의 기능입니다.
  - 예를 들어 동일한 사용자가 서비스에 계속 접속할 때 처음 접속했던 EC2 인스턴스에 연결시켜 줍니다. 
  - 이 기능을 사용하지 않으면 라운드 로빈 알고리즘에 따라 매번 다른 EC2 인스턴스에 연결됩니다.
* 지연 시간(Latency) : ELB와 EC2 인스턴스 간의 지연시간입니다.
* 서지 큐 길이(Surge Queue Length) : ELB 로드 밸런서에서 EC2 인스턴스로 전달되지 못하고 큐에 남아있는 요청의 개수입니다.
* Spillover Count : `서지 큐`가 꽉 차서 ELB 로드 밸런서가 거부한 요청의 개수입니다.

## ELB 생성

ELB를 이용하기 위해선 서로 다른 가용영역에 있는 EC2 인스턴스 2개 이상이 필요합니다.   
![image](https://user-images.githubusercontent.com/43658658/147174975-834b6026-3a2a-4874-9ce1-248c144024ab.png)

각 인스턴스에 웹 서버를 설치하고 `app.js` 파일에 아래와 같은 내용을 추가합니다.   
=> [웹 서버 설치 방법](https://github.com/khyup0629/aws-study/blob/main/11_CloudFront.md#%EC%BB%A4%EC%8A%A4%ED%85%80-%EC%98%A4%EB%A6%AC%EC%A7%84%EA%B3%BC-cloudfront-%EC%97%B0%EB%8F%99)   

``` javascript
var express = require('express');
var app = express();

app.get(['/', '/index.html'], function (req, res) {
  res.send('Hello ELB 1'); // 두 번째 EC2 인스턴스에서는 Hello ELB 2
});

app.listen(80);
```

`sudo node app.js`로 웹 서버를 실행시킵니다.   
![image](https://user-images.githubusercontent.com/43658658/147177461-941c857e-8aac-40dd-ae51-97962a8d2b02.png)   
![image](https://user-images.githubusercontent.com/43658658/147177492-5d20a29d-530b-4380-a16b-4563dde35451.png)

[EC2 콘솔] > [로드밸런서] > [로드밸런서 생성] > [Application Load Balancer 생성]   

![image](https://user-images.githubusercontent.com/43658658/147070904-70285b20-2698-4268-abfc-d321b5fa559d.png)   
* ALB : Nginx, Apache와 같은 HTTP/HTTPS(L7 계층)의 부하분산을 지원합니다.
* NLB : L4 계층의 부하분산을 지원합니다.
* GLB : L3 계층의 부하분산을 지원합니다. 모든 포트에서 모든 IP 패킷을 수신하고 리스너 규칙에 지정된 대상 그룹에 트래픽을 전달합니다.

![image](https://user-images.githubusercontent.com/43658658/147174601-a916511d-65a8-4777-8620-bcfdb2b8ebb7.png)   
* Internet-facing : 퍼블릭 서브넷을 이용해 요청이 인터넷을 거쳐 타겟으로 가는 스키마입니다.
* Internal : 프라이빗 네트워크를 이용해 클라이언트의 요청이 바로 타겟으로 가는 스키마입니다.

인스턴스가 속해있는 `가용영역`을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147174759-43a533b3-dd0e-4925-b003-f56048bd7525.png)   

`ELB`용 보안 그룹을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147174796-8aff75ed-7251-442d-b070-64b48c59bdb2.png)

`HTTP`만 인바운드 허용한 보안 그룹을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147174444-4b39c057-a4e4-4cd7-9332-92c9a50839e3.png)   
![image](https://user-images.githubusercontent.com/43658658/147174886-2ee0f859-9f4f-4779-a419-7f23f1ae15dc.png)

트래픽이 이용할 `프로토콜`과 도착할 `타겟 그룹`을 생성해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/147175774-5f60d90a-0315-4171-9293-2e86238e1765.png)

인스턴스 2개를 타겟으로 설정할 것이므로, 그에 맞게 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147175741-abf808b2-8281-4d25-89ed-769041f8f7ce.png)   
* HTTP 프로토콜로 통신합니다.
* 인스턴스들이 속한 VPC를 선택합니다.

`상태 체크(Health Check)`를 할 프로토콜과 경로, 여러 설정값들을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/147176389-d33a8a6d-09b7-4af0-ae83-155161ee5ccc.png)

다음 페이지로 넘어가서 타겟으로 설정할 `인스턴스 2개를 선택`하고 아래 목록에 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/147176491-c6873b98-d4f9-449a-a38a-e3a9001cc956.png)

`Pending`된 것을 확인하고 타겟 그룹을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147176536-dad94a52-2bda-478b-bbcd-13830019d826.png)

다시 ELB 생성 페이지로 돌아와서 `타겟 그룹`을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147176581-562cea9f-9783-494b-a842-96ea16f9fa24.png)

`ELB`를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147176606-2a702c42-3fc1-4281-84e6-ac47bf0c06aa.png)

## ELB 테스트

이제 ELB의 `DNS 이름`을 통해 2개의 인스턴스에 라운드 로빈 방식으로 접속할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/147177664-ca40de37-7723-4c7c-b152-e4aa9b79bf70.png)

한 번은 ELB 1, 새로 고침하면 ELB 2로 접속됩니다.   
![image](https://user-images.githubusercontent.com/43658658/147177714-f9562a92-63ed-4a67-b80a-b00c8898e888.png)   
![image](https://user-images.githubusercontent.com/43658658/147177727-a7ef342c-4036-4d13-9d48-34c4a146b7e4.png)

이렇게 ELB가 인스턴스로 들어오는 트래픽을 분산시킵니다.

## Sticky session 설정

: 사용자의 쿠키 세션을 기준으로 트래픽을 분배합니다.

[EC2 콘솔] > [대상 그룹] > [타겟 그룹 선택]   
![image](https://user-images.githubusercontent.com/43658658/147178303-2bfe10b9-67c0-4889-831b-4e6601c90511.png)

[속성] > [편집]   
![image](https://user-images.githubusercontent.com/43658658/147178342-91403b41-6839-456d-9be1-cbd1c09fc4d5.png)

`Stickiness`를 활성화합니다.   
![image](https://user-images.githubusercontent.com/43658658/147178474-815ae38f-3edf-449d-9746-d545d68df878.png)   
* `Load Balancer generated cookie` : ELB가 생성한 쿠키를 이용합니다.
* `Application-based cookie` : 서버의 애플리케이션이 생성한 쿠키를 이용합니다.
* `Stickiness duration` : 쿠키의 만료 시간을 의미합니다. 만료 시간이 되기 전까지 ELB는 같은 인스턴스로 연결합니다.

변경사항을 저장하고 다시 `DNS 이름`을 통해 접속해봅니다.   

`Stickiness duration` 시간이 지나면 쿠키가 만료되면서 다른 인스턴스로 접속되는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/147177714-f9562a92-63ed-4a67-b80a-b00c8898e888.png)   
![image](https://user-images.githubusercontent.com/43658658/147177727-a7ef342c-4036-4d13-9d48-34c4a146b7e4.png)


























