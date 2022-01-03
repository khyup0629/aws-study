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

## 구글 안드로이드에 푸시 알람 보내기

=> [안드로이드에 푸시 알람 보내기 메뉴얼](https://aws.amazon.com/ko/premiumsupport/knowledge-center/create-android-push-messaging-sns/)

SNS에서 Android 플랫폼 애플리케이션을 생성하려면 Firebase Cloud Messaging(FCM)의 자격 증명이 필요합니다.

Firebase 웹사이트의 [1단계: Firebase 프로젝트 생성 지침](https://firebase.google.com/docs/web/setup/#create-firebase-project)을 따릅니다.

먼저 아래의 사이트로 접속해서 프로젝트를 만듭니다.   
=> https://console.firebase.google.com/u/0/?pli=1   
![image](https://user-images.githubusercontent.com/43658658/147915412-f44b5a4f-be51-4292-ad83-cf0c38359370.png)

프로젝트 이름을 지정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147915458-6c1f55ca-dc8e-4786-a852-6acf616a8bf3.png)

대한민국으로 위치를 지정하고 프로젝트를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147915488-129d37a1-36ae-4d2f-80c2-1a265fe00104.png)

생성한 프로젝트에서 [톱니바퀴] > [프로젝트 설정]   
![image](https://user-images.githubusercontent.com/43658658/147915958-a9393ef6-b8ff-4fe7-928a-7c542235d688.png)

[클라우드 메시징] > [서버 키]에서 서버 키를 얻습니다.   
![image](https://user-images.githubusercontent.com/43658658/147916000-221971d7-cfad-4c3f-8347-1d32e0d9296e.png)

다시 SNS 콘솔로 돌아와 [푸시 알림] > [플랫폼 애플리케이션 생성]   
![image](https://user-images.githubusercontent.com/43658658/147913847-8f9765f3-f00d-4714-af40-6efa41030655.png)

애플리케이션 이름, 푸시 알림 플랫폼, API 키, 클라이언트 암호를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/147916086-1b266ee7-9a42-4f26-a384-7b1a01fd467d.png)

아래 4개 이벤트에 대한 메시지를 수신한 SNS 주제를 입력할 수도 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/147914063-efea82a2-060f-4b3a-a7fb-77dfc966b054.png)

`플랫폼 애플리케이션`을 생성합니다.
![image](https://user-images.githubusercontent.com/43658658/147916234-4c86289d-e7b3-4c53-b944-047b472cd84e.png)   

이제 `애플리케이션 엔드포인트`를 생성해야 합니다.

생성한 `플랫폼 애플리케이션`을 선택하고 `애플리케이션 엔드포인트 생성`을 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/147916659-ff64396e-6fbd-4393-a391-4411b0648bd6.png)

디바이스 토큰을 입력합니다. FCM의 경우 `등록 ID`를 입력합니다.    
![image](https://user-images.githubusercontent.com/43658658/147939131-9960fd16-a637-4e28-8968-239ee426d6a6.png)   

`등록 ID`를 알기 위해서는 임의의 안드로이드 애플리케이션을 만들고, 등록 ID를 알아야 합니다.   

다시 `Firebase`로 돌아가서 안드로이드 버전으로 `앱 추가하여 시작하기`를 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/147933569-606d4145-1d86-45e6-8efe-342730810d85.png)

앱의 패키지 이름을 입력하고 다음 단계로 모두 넘어가서 앱을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147939720-df3a851b-7a7e-45cb-85e0-8511b309ee04.png)   
* 중간의 과정들은 `개발`의 영역입니다.

그럼 아래의 화면으로 넘어옵니다. `앱 ID`를 복사해서 애플리케이션 엔드포인트의 `디바이스 토큰`에 붙여넣습니다.   
![image](https://user-images.githubusercontent.com/43658658/147939517-15cc7981-f957-4204-8807-5a3e4434d23c.png)

다시 AWS로 돌아와 `애플리케이션 엔드포인트`를 생성하면 애플리케이션 엔드포인트가 생성됩니다.   
![image](https://user-images.githubusercontent.com/43658658/147939583-1477fec4-91e4-40d3-8ba3-b67afaac75fa.png)

이제 `플랫폼 애플리케이션`을 선택하고 엔드포인트를 선택한 뒤, [메시지 게시]를 통해 메시지를 보낼 수 있습니다.
![image](https://user-images.githubusercontent.com/43658658/147939891-a9552be1-a9c8-4315-b308-6e95af5d58b2.png)

메시지 내용을 입력하고 게시합니다.   
![image](https://user-images.githubusercontent.com/43658658/147940158-a2aaf28e-1d92-413a-b6c4-cafbc46ea455.png)

실제로 애플리케이션에서 `개발`을 통해 메시지를 수신하는 서비스를 구현해야 확인이 가능합니다.   
=> [참고 자료](https://maejing.tistory.com/entry/Android-FCM%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%B4-Push-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)

`플랫폼 애플리케이션`을 통해서는 하나의 엔드포인트에 하나의 메시지만 전달할 수 있습니다.   
여러 개의 애플리케이션 엔드포인트로 같은 메시지를 보내고 싶다면, 주제에 원하는 갯수 만큼 `애플리케이션 엔드포인트`를 구독해야 합니다.   
![image](https://user-images.githubusercontent.com/43658658/147940872-99201801-8fbe-4d86-8728-8c982d9dcbc0.png)
* `플랫폼 애플리케이션`의 ARN이 아닌 `애플리케이션 엔드포인트`의 ARN을 입력 해야 합니다.   
![image](https://user-images.githubusercontent.com/43658658/147941128-da14212c-05fe-4571-aee2-006868c9e12d.png)



































