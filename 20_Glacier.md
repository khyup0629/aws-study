# Glacier

: 대용량 데이터 저장에만 특화된 서비스입니다. 사용 요금도 1GB당 0.01 USB(약 10원)로 매우 저렴합니다.

자주 사용하지 않고 용량이 매우 큰 데이터를 저장하는데 적합합니다.   
* 압축하지 않은 고해상도 동영상의 원본 데이터
* 몇 년치 이메일 데이터
* 의료, 법무, 금융 및 재무 기록
* 연구 및 과학 데이터(제약, 바이오 등 분석 데이터)
* 오래된 디지털 콘텐츠(웹사이트, 소스 코드, 게임 등)

Glacier는 AWS의 다른 리소스와 연동할 수 있습니다.   
S3에서 `수명 주기 규칙`을 통해 데이터를 Glacier로 이전할 수 있고, `IAM`을 이용하여 데이터에 대한 접근제어를 설정할 수 있습니다.   
그리고 `AWS Import/Export`로 인터넷을 통하지 않고 데이터를 저장할 수도 있습니다.   
뿐만 아니라 `Glacier 클라이언트`, `Java SDK`, `.NET SDK`를 통해서 데이터를 저장하고, 꺼내올 수 있습니다.

## Glacier 기본 개념

* 아카이브 : Glacier에 데이터가 저장되는 최소 단위입니다. S3의 객체와 같은 개념입니다.
* 볼트 : 아카이브가 저장되는 최상위 디렉토리입니다. S3의 버킷과 같은 개념입니다.
* 볼트 인벤토리 : 볼트에 저장된 아카이브 목록, 아카이브 크기, 생성 날짜, 아카이브 설명 등의 정보를 뜻합니다.
* 검색(Retrieval) : 아카이브를 다운로드 하기 위해 볼트 인벤토리를 가져오는 작업과 다운로드 할 아카이브를 선택하고 다운로드가 가능하도록 준비하는 작업을 뜻합니다. 
 - 검색 작업은 `3 ~ 5시간` 소요되고, 검색 후 다운로드는 `24시간` 동안 가능합니다.

## 아카이브 다운로드 과정

한 번 저장한 파일을 꺼내오려면   
볼트 인벤토리 검색 요청을 하여 `3 ~ 5시간 뒤`에 아카이브 `목록을 확인`하고,   
다운로드하고 싶은 아카이브에 대해 검색 요청을 하여 `3 ~ 5시간 뒤`에 아카이브를 `다운로드`해야 합니다.   
![image](https://user-images.githubusercontent.com/43658658/147622860-1af18027-afcd-4b4a-a0ef-3064a03443af.png)   

## 볼트 생성

[Glacier 콘솔] > [볼트 생성]   
![image](https://user-images.githubusercontent.com/43658658/147628719-a1bd71ca-505b-4fca-a721-80a3219a673b.png)

볼트 이름을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/147628821-8c438d96-7e20-46eb-a53c-5061ff3d4291.png)

SNS 알림을 활성화 할 것인지를 선택하고 주제를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147628849-4997f6ac-6de7-4abf-9bed-0ab76c1724a9.png)

SNS 주제의 ARN을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/147628942-52ca2234-2cfd-435f-8cc7-fdd61105fd66.png)   
* SNS 주제의 ARN은 [SNS 콘솔] > [주제] > [주제 선택] > [ARN]을 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/147628994-ec348440-e26a-42fb-908e-3620eed58318.png)

볼트를 생성했습니다.   
![image](https://user-images.githubusercontent.com/43658658/147629027-ac385420-0e78-4fc7-9746-9da3e41b6df8.png)




















