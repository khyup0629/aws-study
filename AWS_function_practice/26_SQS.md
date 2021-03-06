# SQS

: 서버들끼리 사용할 수 있는 메시지 큐를 제공하는 서비스입니다.

메시지 큐는 대형 인터넷 쇼핑몰에서 가장 중요한 기능입니다.   

예를 들면 아마존닷컴은 대규모 서비스이기 때문에 상품을 보여주는 웹 서버, 상품 정보를 저장하는 데이터베이스, 결제를 처리하는 서버, 배송을 처리하는 서버 등 여러 부분으로 분리되어 있습니다. 
사용자가 상품을 주문하고 결제한 뒤 웹 서버가 중단되더라도 결제 정보는 잃어버리지 않고 정확히 처리되어야 합니다.

SQS는 메시지를 손실 없이 정확하게 처리하고, 고가용성을 제공하기 때문에 대규모 서비스를 구축할 때 필수적인 시스템입니다.

## SQS 큐 생성

[SQS 콘솔] > [대기열] > [대기열 생성]   
![image](https://user-images.githubusercontent.com/43658658/148016468-1c1f53d4-84bb-4840-8ce2-78e0c93321d7.png)

대기열의 이름을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/148016682-9ce10632-03df-4429-a853-1c6bbe0f4957.png)   
* 표준 : 매우 높은 처리량이 중요한 경우
* FIFO : 메시지 순서가 중요하거나 중복을 허용할 수 없을 경우

메시지와 관련된 설정들을 지정합니다.   
![image](https://user-images.githubusercontent.com/43658658/148016854-96aa239f-1fb7-4107-9885-fa84d990627a.png)   
* 표시 제한 시간 : 메시지를 받은 뒤 특정 시간 동안 다른 곳에서 동일한 메시지를 다시 꺼내볼 수 없게 하는 기능입니다.
* 메시지 보존 기간 : 삭제되지 않은 메시지를 보관하는 기간입니다.
* 전송 지연 : 특정 시간 동안 메시지를 받지 못하게 하는 기능입니다.
* 메시지 수신 대기 시간 : 메시지를 수신할 수 있을 때까지 대기하는 시간을 설정합니다.

대기열에 액세스하는 정책에 대해 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/148016987-9e3fa109-17bc-402b-9107-ca1022f8f53c.png)

다양한 옵션에 대해 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/148017733-c2dac551-7509-4dd7-97b9-a154c21fe4c9.png)   
* 리드라이브 허용 정책 : 현재 생성하는 대기열을 DLQ(배달 못한 편지 대기열)로 만듭니다.
* 암호화 : 서버 측 암호화를 활성화할 것인지 선택합니다.
* 배달 못한 편지 대기열 : 전송에 실패한 메시지를 DLQ로 전송할 것인지 선택합니다.

대기열(큐)를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/148017896-e9f4c93d-5b03-4fcb-8912-1c269a84a6e2.png)

## 배달 못한 편지 대기열 생성

DLQ(배달 못한 편지 대기열)을 생성하고 이전에 생성한 대기열과 연결시켜보겠습니다.

[SQS 콘솔] > [대기열] > [대기열 생성]   
![image](https://user-images.githubusercontent.com/43658658/148016468-1c1f53d4-84bb-4840-8ce2-78e0c93321d7.png)

리드라이브 정책에서 현재 생성할 대기열을 DLQ로 만들어주기 위해 활성화합니다.   
![image](https://user-images.githubusercontent.com/43658658/148019798-bc39aa5f-50df-49bd-92e8-be691ef27891.png)   
* 소스 대기열 : 소스 대기열을 통해 들어오는 메시지만 허용하겠다는 의미입니다.

이제 DLQ와 연결할 일반 대기열을 선택하고 [편집]을 눌러 `배달 못한 편지 대기열` 부분을 활성화합니다.   
![image](https://user-images.githubusercontent.com/43658658/148019598-eae60391-01ee-4bdf-be48-5ba1d0f4bf9e.png)   
* 대기열은 DLQ를 선택합니다.
* 이 대기열이 수신할 최대 메시지 수를 입력합니다. 이 수를 넘어서는 메시지는 DLQ로 넘어갑니다.

## 메시지 전송

일반 대기열을 선택하고 [메시지 전송 및 수신]   
![image](https://user-images.githubusercontent.com/43658658/148020052-44329391-a300-49a5-b2fb-b6a7a997a055.png)

메시지를 보냅니다.   
![image](https://user-images.githubusercontent.com/43658658/148019996-21b69769-f403-4c29-89ee-2ddec3d8c104.png)

다시 `메시지 전송 및 수신` 버튼을 눌러서 아래쪽에 `메시지 폴링`을 클릭하면 수신한 메시지가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/148020289-07557ba4-d7ff-4be8-9a9f-ffe06a504af7.png)

클릭하면 메시지 내용이 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/148020239-36d7dbf1-5542-4400-ac33-b7121e041f9c.png)

같은 내용의 메시지를 2번 더 전송합니다. 그럼 총 3개의 메시지가 수신되었습니다.   

하지만 최대 수신 메시지 개수는 2개이므로, 남은 1개의 메시지는 DLQ로 넘어갔습니다.   
(일반 대기열 메시지 수신 상태)   
![image](https://user-images.githubusercontent.com/43658658/148020436-c5963d4f-dd4a-46e3-8fba-d35a9e560858.png)   
(DLQ 메시지 수신 상태)   
![image](https://user-images.githubusercontent.com/43658658/148020503-19aeb0ea-8f56-4758-9777-e1c40484ef2a.png)























