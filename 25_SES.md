# SES

: 대량의 이메일을 전송할 수 있는 서비스입니다.

마케팅을 위해 대량의 이메일을 보내야 하는 조직에 적합합니다.   
메일 서버 구축에 드는 비용을 절감할 수 있고, 스팸 필터 및 IP 차단 등은 신경 쓰지 않아도 됩니다.

## 이메일 주소 인증하기

SES는 이메일 주소를 인증해야만 정상적으로 메일을 이용할 수 있습니다.

[SES 콘솔] > [Verifeid identities] > [Create identity]   
![image](https://user-images.githubusercontent.com/43658658/147996654-86a83a1f-3b6e-4db1-9f33-85a0b47e6fa4.png)

이메일에 대해 인증하기 위해 이메일 주소를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/147996862-f94f4713-002f-499c-a092-d2db088b4083.png)   
* `Assign a default configuration set` : 구성 세트를 설정합니다.
  - 구성 세트 : 이메일에 규칙을 적용할 수 있습니다. 
  - 각 이메일에 대한 전송, 배달, 확인, 클릭, 반송 및 불만 제기 이벤트 수를 추적하기 위한 설정.
  - IP 풀이라는 주소 그룹 생성.
  
![image](https://user-images.githubusercontent.com/43658658/147997329-5d589594-5e95-4d30-bb56-9f4ccbd785d4.png)

identity를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147997405-e8e039e7-4202-4ef7-9471-a929c864fcf0.png)   
* 해당 이메일을 인증 완료하기 전까진 상태가 `Unverified`로 나타납니다.   

해당 이메일에 SES로부터 메일이 와있음을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/147997519-dd60cf72-d182-4da8-a78f-b63e9f10f1b7.png)

메일에 적힌 확인 URL로 접속해 인증을 완료합니다.   
![image](https://user-images.githubusercontent.com/43658658/147997769-e7a93798-8b4e-4488-8bec-95546e9c3ee2.png)   
* 위의 페이지가 나타나는데, 접속만 하면 인증이 완료되어 있습니다.

인증이 완료되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/147997807-d5561bed-6d10-488a-a2bb-5de88f1759e6.png)

아직 프로덕션 인증을 거치지 않았기 때문에 외부와 메일을 주고받을 수 없고, 인증된 메일끼리만 주고 받을 수 있습니다.

## 도메인 인증하기

[SES 콘솔] > [Verifeid identities] > [Create identity]   
![image](https://user-images.githubusercontent.com/43658658/147996654-86a83a1f-3b6e-4db1-9f33-85a0b47e6fa4.png)

이번에는 도메인을 인증하기 위해 `Identity type`을 `Domain`으로 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/148001741-0d4c0204-a9f5-404e-8fcf-0298d143ae88.png)   
* Use a custom MAIL FROM domain(사용자 지정 MAIL FROM 도메인을 사용) : `MAIL FROM 도메인`을 사용자 지정 도메인으로 사용할 것인지 여부입니다.
  - 사용자 지정 MAIL FROM 도메인을 설정하면 DMARC 검증을 통해 발신자의 도메인에서 발송된 이메일이 하나 이상의 인증 시스템에 의해 보호되고 있음을 나타낼 수 있습니다.
  - 메일서버는 `MAIL FROM` 주소를 사용해 반송 메시지 및 기타 오류 알림을 반환합니다.
  - MAIL FROM 도메인을 설정하려면 DNS 구성에 MX 레코드를 추가 해야 합니다.

DKIM에 관한 설정을 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/147999677-fdd9868b-ea5a-40c4-8e68-38842a841352.png)   
* DKIM : 스팸 메일을 차단하기 위해 개발된 기술이며 공개 키와 사설 키를 기반으로 하여 이메일을 인증합니다.
* Provide DKIM authentication token : 키 페어를 통해 DKIM 설정을 진행합니다.
* DKIM signing key length : 키의 길이를 2048/1024 비트 중에 선택합니다. 비트 길이가 길수록 보안이 강화됩니다.
* Publish DNS records to Route53 : Route53에 도메인이 등록되어 있다면 CNAME 레코드를 자동으로 생성합니다.
* DKIM signatures : 메시지가 중간에 위조되거나 변경되었는지를 검증할 수 있게 해줍니다.

도메인 identity를 생성합니다. 도메인 identity의 authentication 탭을 보면 DKIM 정보가 나와있습니다.   
![image](https://user-images.githubusercontent.com/43658658/148002640-a2c8f287-f52d-4b1c-81d8-9bc610a74b4c.png)

Route 53에 해당 도메인의 레코드를 보면 DKIM과 관련된 CNAME이 3개가 추가된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/148002401-20117ba4-82ad-4a6f-9011-8513408e1cf0.png)

잠시 기다리면 도메인이 인증 완료된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/148002426-3763b8b8-5188-4df2-9a1e-629250985eef.png)

## 프로덕션 액세스 권한 얻기

[SES 콘솔] > [Account Dashboard] > [Request production access]   
![image](https://user-images.githubusercontent.com/43658658/148008212-31cc5a18-5589-4703-94cf-cff926fb31c9.png)

프로덕션 액세스 권한을 얻기 위해 이메일을 보낼 계획을 작성합니다.   
![image](https://user-images.githubusercontent.com/43658658/148008702-38e81712-b7ba-424f-8147-5a6479df1377.png)

요청을 제출하면, 24시간 내에 수락 여부를 알려줍니다. 대부분 1 ~ 2시간 후에 승인 판정이 납니다.   
![image](https://user-images.githubusercontent.com/43658658/148008794-e73e31b1-5458-4258-8baf-f35c31697e30.png)

승인이 되면 이메일로 메일이 수신됩니다.   

프로덕션 액세스 권한을 얻으면, SES 계정이 하루에 보낼 수 있는 메일 개수가 늘어납니다.   
(이전)   
![image](https://user-images.githubusercontent.com/43658658/148009012-1f998ae7-f27c-46af-9943-48678a057d0d.png)   
(이후)   























