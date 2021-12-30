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

AWS 리소스 조합을 CloudFormation `스택`이라고 합니다. 이 스택을 삭제하면 관련된 AWS 리소스도 모두 삭제됩니다.

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

