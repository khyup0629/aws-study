# SNS(Simple Notification Service)

: iPhone, iPad, Android, Kindle Fire와 같은 모바일 장치에 푸시 알림을 보낼 수 있는 서비스입니다.   
이메일과 SMS 문자 메시지, SQS 큐 메시지도 보낼 수 있습니다.

SNS를 이용하면, 메시지 전송 구현에 드는 노력과 시간을 아낄 수 있고, 서버 구축과 운영 비용을 절감하며, 고가용성을 제공합니다.

![image](https://user-images.githubusercontent.com/43658658/147905787-f7629c0f-6642-4e5e-9c7c-cde048caa0d3.png)   
AWS의 여러 리소스에서 푸시 알림을 SNS 주제에 보내면 SNS 주제가 `주제`를 `구독`한 모바일 기기, 이메일, HTTP, SQS로 푸시 알림을 보냅니다.   
* 사용자가 구현한 서버 애플리케이션(EC2 인스턴스)에서 모바일 장치로 푸시 알림을 보낼 수 있습니다.   
* CloudWatch로 EC2 인스턴스를 감시하다가 장애가 발생하면 이메일 또는 아이폰, 안드로이드폰의 푸시 알림으로도 받아볼 수 있습니다.

## SNS 관련 용어

* 애플리케이션(Application) : APNS, APNS_SANDBOX, GCM, ADM 별로 생성하는 설정입니다. 
  - GCM : API 키 필요
  - APNS : 인증서, 개인 키 필요
  - ADM : 클라이언트 ID, Secret 키 필요
* 프로토콜(Protocol) : 푸시 알림 메시지를 보내는 방식입니다. HTTP, HTTPS, Email, Email-JSON, SQS, Application(APNS, GCM, ADM 등)이 있습니다.
* 엔드포인트 : 푸시 알림을 받을 장치의 정보입니다. 
  - HTTP와 HTTPS는 URL, Email과 Email-JSON은 이메일 주소, SQS는 SQS 큐 이름입니다. 
  - Application(APNS, GCM, ADM 등)은 Device Token, Registration ID 등을 설정해야 하며 각 장치마다 다른 값을 가지고 있습니다.
* 주제(Topic): 여러 개의 엔드포인트를 그룹으로 만든 것입니다. SNS 주제를 통해 푸시 알림 요청을 하면 주제에 속한 모든 엔드포인트로 알림을 보냅니다.
* 구독(Subscription): 토픽에 속한 엔드포인트입니다.

## 이메일 주제/구독 생성

[SNS 콘솔] > [주제] > [주제 생성]   
![image](https://user-images.githubusercontent.com/43658658/147908676-69e0908b-e135-47d4-9b1a-80704c5ddfbe.png)

주제 유형과 이름을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147908799-f315b88c-7108-4268-8d8f-2dcfba095a19.png)   
* FIFO : 메시지 순서 지정 및 중복 제거가 필요한 사례
* 표준 : 더 높은 메시지 게시 및 전송 처리 속도를 요구하는 사례

서버 측 암호화(SSE) 여부를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147908907-cb0c3e8b-8330-4391-99e2-a1a1d634e1ca.png)   
* 주제는 메시지를 수신하는 즉시 암호화하고, 엔드포인트로 전송하기 직전에 해제합니다.
* 서버 측 암호화(SSE)를 사용하면 주제에 민감한 데이터를 저장할 수 있습니다. 
* `SSE`는 `AWS Key Management Service(AWS KMS)`에서 관리되는 키를 사용하여 `SNS 주제`의 메시지 내용을 `보호`합니다.

주제에 메시지를 보내거나 주제로부터 메시지를 받는 사용자를 정책을 통해 정의할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/147909069-25f2c23a-a890-4d8c-a97c-d328022ff0f7.png)   
* 기본 : 선택한 옵션을 토대로 JSON이 자동으로 작성됩니다.
* 고급 : JSON를 직접 작성합니다.

전송 재시도 정책을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147909171-9fbf31a3-410f-4ece-93bd-ae45a35641c9.png)

어떤 프로토콜에 대한 알림 메시지의 전송 상태를 로깅하길 원하는지 선택합니다.
![image](https://user-images.githubusercontent.com/43658658/147909557-8b0de0bb-b04c-45d8-8ca9-b3c390dfa31e.png)
* IAM 역할을 통해 전송에 성공/실패했을 때의 역할을 각각 설정합니다.

주제를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147909803-bd0076ce-d1a6-4436-a6bc-a72a5bd2d1e0.png)

주제에 대한 엔드포인트(구독)을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147909850-42806fa1-8465-470a-b7ed-25092e9ce89c.png)

`이메일` 프로토콜과 `이메일 주소`(엔드포인트)를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/147909949-053e1048-8d5f-45a5-9df1-d201133c88a0.png)

구독 필터 정책과 재드라이브 정책을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147910002-8f7c6934-10ec-4212-9223-f05ef3b8d3e0.png)   
* 구독 필터 정책 : 구독자가 받는 메시지를 필터링하는 정책입니다.
* 재드라이브 정책(배달 못한 편지 대기열) : 구독자에게 도달하지 못한 메시지는 [배달 못한 편지 대기열(DLQ)](https://docs.aws.amazon.com/ko_kr/sns/latest/dg/sns-dead-letter-queues.html)에 저장됩니다.
  - 어떤 메시지가 전송에 실패했는지를 알 수 있는 용도로 사용할 수 있습니다.
  - 이후 Lambda, SQS API, SDK, CLI를 사용해서 DLQ 내의 메시지를 다시 처리하거나 삭제할 수 있습니다. 

엔드포인트로 입력한 메일 주소로 접속하면 AWS에서 온 `인증 확인 메일`이 수신된 것을 확인할 수 있습니다.   
(간혹 스팸메일함에 도착했을 수도 있습니다)   
![image](https://user-images.githubusercontent.com/43658658/147910510-2cf5edd2-034a-40f1-8ed8-bc8eabc24455.png)   
![image](https://user-images.githubusercontent.com/43658658/147910530-13e66fd0-60b5-4cb2-a6c9-6a0cbf1ad9f6.png)

## SNS 주제에 메시지 게시

[주제 선택] > [메시지 게시]   
![image](https://user-images.githubusercontent.com/43658658/147910596-4b6f1a24-9308-486d-8151-a07ba4a78a40.png)

메시지 제목과 내용을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/147910758-9caad858-d9ac-495e-9e0b-cd965b086dea.png)   
* TTL : 푸시 알림 서비스가 메시지를 엔드포인트로 전송해야하는 시간(초)입니다.
  - 모바일 애플리케이션 엔드포인트에만 적용됩니다.

메시지에 대한 메타데이터를 입력할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/147910962-696fa1d0-e4e8-417c-af9b-62c15fb94c33.png)   
* 키-값 형식으로 속성을 추가합니다.

모든 설정을 완료하고 `메시지를 게시`합니다.   
메시지를 게시하면 해당 주제를 구독한 엔드포인트들로 메시지가 전달됩니다.

엔드포인트 이메일에 `hello`라는 제목의 이메일이 온 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/147911029-ec545162-153e-4f2b-b358-3f5afa211cb0.png)



































