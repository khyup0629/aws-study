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
* `Assign a default configuration set` : 이메일에 대해 인증하기 위해서 아래와 같은 `사용자 지정 확인 이메일 템플릿`(인증 확인 메일)을 보내게 되는데 이때 사용하는 템플릿을 지정합니다.   
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



























