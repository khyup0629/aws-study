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
  - 검색 작업은 `12 ~ 48시간` 소요되고, 검색 후 다운로드는 복원 과정에서 설정한 기간 동안 가능합니다.

## 아카이브 다운로드 과정

한 번 저장한 아카이브(파일)을 꺼내오려면   
다운로드하고 싶은 아카이브에 대해 검색 요청을 한 후 완료되면 아카이브를 `다운로드` 할 수 있습니다.   

## S3 버킷을 이용한 Glacier 이용

S3 버킷을 이용해 Glacier를 이용할 수 있습니다.

일반적인 방법으로 `버킷을 생성`합니다.

버킷에 아카이브(파일)을 업로드합니다.   
![image](https://user-images.githubusercontent.com/43658658/147630568-05413799-fd40-4ee6-8f6f-4cb057e3365c.png)

속성 항목에서 스토리지 클래스를 `Glacier Deep Archive`를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147630792-baeda97f-630a-4358-8f51-95af12e0f679.png)

이 방법으로 파일을 업로드하면 버킷을 Glacier의 `볼트`로 보고 업로드한 것과 같게 됩니다. 

## 아카이브 복원

아카이브에 액세스해서 다운로드하고 싶다면 `복원`을 진행해야 합니다.

다운로드 할 아카이브를 선택해 들어갑니다.   
![image](https://user-images.githubusercontent.com/43658658/147630842-7bcfb9da-c792-4ccc-92d0-ddf09e360a88.png)   

[복원 시작]을 눌러 검색 요청을 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/147630899-9e7962ad-2635-4f14-b13f-e20ca763a9cf.png)

검색을 완료한 후에 `다운로드 받을 수 있는 기간`과 `검색 방법`에 대해 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147631114-3c469e72-4217-466c-b0d9-cbc2d0e21c3d.png)

`복원 시작`을 누르면 `복원 진행 중`이라는 메시지가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/147631250-64e874b0-2a2b-4aea-9ffd-988aa8c0634a.png)

약 12시간 정도 기다리면 검색이 완료되고 앞서 설정한 기간만큼 다운로드 받을 수 있는 시간이 주어집니다.   


## 볼트 생성

이번엔 직접 Glacier 콘솔로 접근해 볼트를 생성하는 방법입니다.

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

## Glacier 클라이언트를 이용한 파일 업로드

Glacier 클라이언트의 대표적 프로그램인 `FastGlacier`를 이용해 파일을 업로드해보겠습니다.

[FastGlacier 설치 사이트](https://fastglacier.com/)로 접속해 다운로드 받습니다.   
![image](https://user-images.githubusercontent.com/43658658/147629774-07c9b68b-e095-453e-9142-a08816e45439.png)

계정 이름을 하나 설정하고, AWS `액세스 키/시크릿 키`를 입력하고 접속합니다.   
![image](https://user-images.githubusercontent.com/43658658/147629936-d5f32832-4908-415c-9ebe-9aa43254380e.png)

알맞은 리전을 선택하고 이전에 생성한 볼트로 들어가 파일을 업로드 하면 됩니다.




















