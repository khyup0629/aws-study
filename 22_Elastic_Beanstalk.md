# Elastic Beanstalk

: 애플리케이션을 실행하는 인프라에 대해 자세히 알지 못해도 AWS 클라우드에서 애플리케이션을 신속하게 배포하고 관리할 수 있습니다.

Go, Java, .NET, Node.js, PHP, Python 및 Ruby에서 개발된 애플리케이션을 지원합니다.

AWS 리소스를 조합하여 완성된 애플리케이션 플랫폼을 제공합니다.   
알아서 EC2 인스턴스 유형을 변경하고, Auto Scaling으로 EC2 인스턴스를 늘리고, ELB로 부하를 분산하며 애플리케이션 배포까지 자동으로 해줍니다.   
애플리케이션을 업로드하기만 하면 `Elastic Beanstalk`에서 용량 프로비저닝, 로드 밸런싱, 조정, 애플리케이션 상태 모니터링에 대한 세부 정보를 자동으로 처리합니다.

![image](https://user-images.githubusercontent.com/43658658/147871125-fdd62cf7-faca-44a9-94f5-3ccc6ced1ef9.png)   
`Elastic Beanstalk`은 `복잡한 CloudFormation 템플릿`을 작성(서버 구성, 설치, 배포 설정)하지 않고 간단하게 사용할 수 있도록 만든 서비스입니다.

## Elastic Beanstalk 실습

=> [Elastic Beanstalk 실습 메뉴얼](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/GettingStarted.html)

Elastic Beanstalk 콘솔에서 사용자가 애플리케이션을 업로드하는 공간을 `애플리케이션`이라고 합니다.   
애플리케이션을 실행하는 플랫폼을 `환경`이라고 합니다.

[Beanstalk 콘솔] > [환경] > [새 환경 생성]   
![image](https://user-images.githubusercontent.com/43658658/147894057-54574d02-2f5c-4384-847a-4e66f8729fef.png)

환경의 종류를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147894167-a10d07a7-12d8-4fbb-856a-b3f9d9c80609.png)   
* `웹 서버` : 일반적인 HTTP 프로토콜로 통신하는 웹 애플리케이션 용도입니다.
* `작업자` : SQS의 메시지를 중심으로 작업을 수행하는 애플리케이션 용도입니다.

애플리케이션의 이름을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/147894114-dc5f7f90-678d-41a7-a6ec-9bd3d2ff135e.png)

환경에 대한 정보를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/147894191-8a30e87c-f0d4-4f1c-995a-aa7eff3808e1.png)   
* 도메인 : 환경의 URL입니다.

플랫폼(개발 언어, OS)에 대한 정보를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/147894289-b8eb4178-0e29-4f06-88d0-20ca178b927b.png)   

업로드할 코드를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147894326-99fbf7c3-c970-4762-9fb7-8b365659d401.png)   
* `샘플 애플리케이션` : Elastic Beanstalk에서 제공하는 예제 소스를 사용합니다.
* `코드 업로드` : 컴퓨터나 S3에 저장된 소스를 사용합니다.

맨 아래에 [추가 옵션 구성]을 선택하면 환경에 속해 있는 서비스에 대한 세부적인 옵션을 설정할 수 있습니다.
![image](https://user-images.githubusercontent.com/43658658/147894492-6ee58ea2-859b-4b03-8e07-778985c36dc0.png)   
* 아래의 세부적인 옵션을 설정하고 싶다면 `사용자 지정 구성`을 선택하고 설정합니다.

환경을 생성합니다.   
애플리케이션과 환경 생성이 시작되고 약 5분 기다리면 생성이 완료됩니다.   
![image](https://user-images.githubusercontent.com/43658658/147894664-20e6ebc7-b230-4dee-9741-a7e7d43e8fa5.png)

웹 브라우저에서 `Elastic Beanstalk URL`로 접속하면 아래와 같은 화면이 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/147894668-815462d7-5a9f-4d74-89bf-16461ad66dc5.png)










