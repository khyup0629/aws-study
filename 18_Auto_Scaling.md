# Auto Scaling

: 트래픽에 따라 자동으로 EC2 인스턴스를 추가/삭제해 서비스를 유동적으로 관리하고 가용성을 높이는 기능입니다.

`Auto Scaling`은 `ELB`와 함께 사용합니다. 새로 생성된 EC2 인스턴스는 ELB 로드 밸런서에 연결되고, ELB 로드 밸런서는 새로 생성된 EC2 인스턴스에 트래픽을 분산합니다.

`CloudWatch`와 연동하여 EC2 인스턴스의 CPU 사용률, 네트워크 사용량 등 `매트릭`이 늘어났을 때 EC2 인스턴스를 `생성`하고, 줄어들면 EC2 인스턴스를 `삭제`합니다.


## AMI 생성(Auto Scaling용)

Auto Scaling으로 인스턴스를 생성할 때 AMI를 이용해서 편리하게 생성할 수 있습니다.

먼저 EC2 인스턴스 `2개`(서로 다른 가용영역)와 `ELB`를 구성합니다.   
=> [ELB 생성 방법](https://github.com/khyup0629/aws-study/blob/main/17_ELB.md#elb-%EC%83%9D%EC%84%B1)   
- 인스턴스 2개   
![image](https://user-images.githubusercontent.com/43658658/147436546-2fffb3a7-39d9-4ebf-adc0-a757bdbfc5b3.png)   
- ELB   
![image](https://user-images.githubusercontent.com/43658658/147436809-e1c7ea00-45aa-46c4-a224-9da7d6ddd639.png)

[인스턴스 선택] > [우클릭] > [이미지 및 템플릿] > [이미지 생성]   
![image](https://user-images.githubusercontent.com/43658658/147436865-ac9f157b-e6d7-4aef-b93f-1a84902f48ce.png)

이미지를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147436899-7e72f7f6-87c8-4cc0-8154-8aa05e3bae56.png)

## Auto Scaling 그룹 생성

[EC2 콘솔] > [Auto Scaling 그룹] > [Auto Scaling 그룹 생성]   
![image](https://user-images.githubusercontent.com/43658658/147437289-9ff862d4-a12c-46f4-838f-6f9d3096e00f.png)

1단계에서 그룹 이름과 `시작 템플릿`을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/147437418-2b4c58fa-fce7-4ee4-b8bd-fe42766e063f.png)

템플릿 이름과 AMI를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147437636-e947a839-d32d-4646-858c-5c84fe655725.png)   
* AMI는 이전에 생성한 AMI의 ID를 적습니다.

인스턴스 유형, 키 페어, 보안 그룹을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147437682-e5324b64-056f-468a-9c09-15228c596a1e.png)

사용자 데이터에 아래와 같이 입력합니다.   
``` bash
#!/bin/bash
node /home/ec2-user/example/app.js &
```

![image](https://user-images.githubusercontent.com/43658658/147446925-59d4eced-3d0c-4eb1-a140-4f910d2e60d6.png)   
* 이 부분은 EC2 인스턴스가 생성되었을 때 실행될 스크립트입니다.

다시 Auto Scaling 그룹 생성 페이지로 돌아옵니다.   

2단계에서 인스턴스가 생성될 VPC와 서브넷을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147438563-aa396f52-6eeb-4b8d-9954-8c88334df21b.png)

3단계에서 기존에 생성되어 있는 로드 밸런서와 연결합니다.   
![image](https://user-images.githubusercontent.com/43658658/147438673-64c99d52-b62b-41c6-82e9-b65c776eaecf.png)

4단계에서 Auto Scaling으로 생성 또는 삭제되는 인스턴스의 개수 범위를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147439455-e7bf50a6-329f-4f7b-a0b1-4ecd7e3703f5.png)   
* 원하는 용량 : 최소~최대 용량 범위 내에서만 설정 가능하며, 용량 범위 내에서 동적으로 자동 조정됩니다. 원하는 용량을 기준으로 인스턴스가 `생성/삭제` 됩니다.

Auto Scaling 그룹 내의 인스턴스들의 상태를 어떤 조건으로 유지할 것인가에 대해 `조정 정책`을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147439375-f2728ba4-c75b-4a42-8e5b-64533ab8ca37.png)   
* 인스턴스 축소 보호 : 인스턴스가 삭제되는 것을 방지합니다.

5단계에서 SNS와 연동해 특정 이벤트에 대해 이메일 알람을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147439502-70b46155-0719-4aaf-86ff-1ee23fbf8e82.png)

SNS 주제를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147439542-f5603603-cf0b-490c-81b4-beaf1968675b.png)

마지막으로 검토한 후 `Auto Scaling 그룹`을 생성합니다.   

생성된 `Auto Scaling 그룹`을 선택하고 [인스턴스 관리] 탭을 보면 Auto Scaling으로 생성된 인스턴스가 있는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/147439864-a1681972-040a-45e1-b9eb-a58a1538c80b.png)

## Auto Scaling 테스트

이제 Auto Scaling이 생성한 인스턴스에 SSH로 접속해 부하를 걸어줍니다.   
`yes > /dev/null`   
![image](https://user-images.githubusercontent.com/43658658/147440235-e6bb7593-7747-4991-8125-1fad3dbb56fe.png)

잠시 기다리면 `Auto Scaling 그룹`에 인스턴스가 하나 추가됩니다.   
![image](https://user-images.githubusercontent.com/43658658/147440487-78bf5a29-771b-4ea0-b699-112739e46ab8.png)

다시 부하를 멈춰보겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/147440522-1d83f311-38d7-4342-9e86-f02794834ce4.png)

잠시 기다리면 생성되었던 인스턴스가 다시 삭제된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/147443932-89a5c135-723d-4dd5-a3ad-df57092b6dad.png)

## 접속 테스트

Auto Scaling을 통해 생성된 인스턴스가 정상적으로 접속되는지 살펴봅시다.

ELB `DNS 이름`을 통해서 접속할 수 있습니다.

기존에 있었던 인스턴스 2개와 Auto Scaling을 통해 생성된 인스턴스 1개를 더해 총 3개의 인스턴스에 트래픽이 라운드 로빈 방식으로 분배됩니다.   
![image](https://user-images.githubusercontent.com/43658658/147446836-682604eb-8f04-42bd-8b93-a195032e7543.png)
![image](https://user-images.githubusercontent.com/43658658/147446863-e2cbb20d-54da-43c7-9d25-77ab87c63270.png)
![image](https://user-images.githubusercontent.com/43658658/147446842-53a6527e-5013-43fd-bf0e-957773fe492d.png)




















