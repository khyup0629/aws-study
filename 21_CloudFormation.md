# CloudFormation

: 사용자가 구성한 EC2 인스턴스, EBS 볼륨, S3 버킷, RDS 인스턴스 등의 시스템 구조를 템플릿을 만들고 필요할 때 자동 생성하는 기능입니다.

CloudFormation을 활용하면, 복잡하고 반복적인 작업을 간단하게 처리할 수 있습니다.   
템플릿 파일은 `JSON 형식`의 텍스트 파일입니다. 

## CloudFormation 지원 리소스

* Auto Scaling	
* RDS	
* CloudFront
* Redshift	
* CloudWatch
* Route 53
* DynamoDB
* S3	
* EC2
* SimpleDB	
* ElastiCache	
* SNS
* Elastic Beanstalk	
* SQS	
* Elastic Load Balancing
* VPC	
* IAM

## 소프트웨어 설치 자동화

CloudFormation 템플릿(JSON 형식)은 AWS 리소스 생성 및 설정뿐만 아니라 EC2 인스턴스에 `소프트웨어를 자동으로 설치하고 설정`할 수 있습니다.   

소프트웨어 설치 자동화에 Chef, Puppet, cloud-init을 지원합니다.   
* `Chef`, `Puppet` : 루비 작성된 오픈소스 자동화 솔루션입니다. 소프트웨어 설치, 설정, 빌드, 배포를 자동화합니다.
* `cloud-init` : 클라우드 인스턴스의 초기화를 위한 스크립트입니다.   
![image](https://user-images.githubusercontent.com/43658658/147716917-9a89bfa8-31b3-46d0-9014-d9ae6d8c9dff.png)

## CloudFormation 템플릿 기본 구조

CloudFormation 템플릿은 `JSON 형태`로 작성합니다. 다음은 CloudFormation 템플릿의 가장 기본적인 형태입니다.

`CloudFormation 기본 템플릿`   
```
{
  "Description" : "A text description for the template usage",
  "Parameters": {
    // A set of inputs used to customize the template per deployment
  },
  "Resources" : {
    // The set of AWS resources and relationships between them
  },
  "Outputs" : {
    // A set of values to be made visible to the stack creator
  },
  "AWSTemplateFormatVersion" : "2010-09-09"
}
```

* Description: 템플릿의 설명입니다.
* Parameters: 템플릿으로 스택을 생성할 때 사용자가 입력할 파라미터 목록입니다. 숫자와 문자열 형식을 사용할 수 있습니다.
* Resources: AWS 리소스 종류와 옵션, AWS 리소스간의 관계를 정의합니다.
* Outputs: 스택을 생성한 뒤 출력할 값입니다.
* AWSTemplateFormatVersion: 현재 템플릿 구조의 버전입니다.

## CloudFormation 스택 생성

AWS 리소스 조합을 CloudFormation `스택`이라고 합니다. 이 스택을 삭제하면 관련된 AWS 리소스도 모두 삭제됩니다.

이제 CloudFormation 스택을 생성해보겠습니다.   

[CloudFormation 콘솔] > [스택] > [스택 생성]   
![image](https://user-images.githubusercontent.com/43658658/147717615-787f5f11-5a98-45c9-b41b-e039bf65241a.png)

1단계에선 템플릿을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147717649-7ccb3e77-34f9-4b32-847b-a76bafa9d392.png)   
* LAMP Stack : Linux, Apache, MySQL, PHP가 EC2 인스턴스에 자동으로 구성되는 샘플 템플릿입니다.

2단계에선 먼저 스택 이름을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147852166-abde1364-ffa9-4d90-b2e7-5c2493cb2a87.png)

인스턴스에 대한 여러 파라미터들을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147852143-5ce5a4c9-f4fc-4f79-a54d-183bfc4f27a2.png)   
* MySQL에 접근하기 위한 파라미터들과 인스턴스의 스펙, 키 파일과 보안규칙을 설정합니다.

3단계로 넘어와서 먼저 IAM 역할을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147852208-428ed363-e33f-4da1-b5a4-4074ec4eaeea.png)   
* AWS CloudFormation에서 스택 리소스를 생성, 업데이트 또는 삭제하도록 허용하는 IAM 역할을 지정할 수 있습니다.

스택으로 리소스를 호출할 때 실패한다면 어떻게 할지를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147852234-ae47d449-a785-4312-8e14-e00ee85ed8d8.png)   
* `모든 스택 리소스 롤백` : 하나라도 실패하면 모든 리소스를 롤백합니다.
* `성공적으로 프로비저닝된 리소스 보존` : 성공적으로 호출된 리소스는 보존합니다.

![image](https://user-images.githubusercontent.com/43658658/147852265-f15bfb83-4a87-4012-a545-9c73a7b3b899.png)   
* `스택 정책` : 특정 리소스가 의도치 않게 업데이트 되는 것을 방지하도록 JSON 형식으로 정책을 설정할 수 있습니다.
* `롤백 구성` : CloudWatch에 경보가 구성되어 있다면 해당 경보를 이용해서 CloudFormation 리소스를 모니터링 할 수 있습니다.

![image](https://user-images.githubusercontent.com/43658658/147852336-44f5aa16-0eeb-4de0-a495-f0e640ebd0a2.png)   
* `알림 옵션` : SNS 주제를 선택할 수 있습니다.
* `스택 생성 옵션` : 스택과 관련한 설정을 할 수 있습니다.
  - `제한 시간` : 스택이 생성되기까지 제한 시간을 설정합니다.
  - `종료 방지` : 스택이 실수로 지워지지 않게 하고 싶다면 활성화합니다.

4단계에서 설정 사항들을 검토하고 스택을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147852428-a7479d6f-9179-4f05-a37b-cf2037e6d0f1.png)


