# IAM(Identity and Access Management)

사용자와 그룹을 생성하고 사용자 또는 그룹에게 AWS의 각 리소스에 대해 `접근제어`와 `권한관리`를 제공합니다.

어떤 IAM 사용자는 EC2만 관리할 수 있고, 어떤 IAM 사용자는 S3의 내용을 읽을 수만 있도록 구성할 수 있습니다.   
따라서 전체 권한이 아닌 필요한 권한만 주기 때문에 `보안성`이 높아집니다.

동일한 권한을 여러 사용자에게 부여하기 위해 `그룹`을 이용합니다.

직접적으로 서비스를 제공하지는 않기 때문에 이용 요금은 없습니다.

## IAM 역할

IAM 사용자 또는 그룹에 권한을 주는 것이 아닌 EC2 인스턴스, 다른 AWS 계정, 페이스북 계정, 구글 계정 등에 권한을 줄 수도 있습니다.   
이것을 `IAM 역할`이라고 합니다.

## IAM 그룹 생성

EC2 인스턴스에만 권한을 허용하는 그룹을 생성해보겠습니다.

[IAM 콘솔] > [사용자 그룹] > [그룹 생성]   
![image](https://user-images.githubusercontent.com/43658658/146902739-d66218cb-987a-4d97-922b-947869d64928.png)   

그룹 이름을 지정하고 사용자를 추가하고 싶다면 특정 사용자를 체크해서 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/146903098-bcb91c11-2af1-4bd2-90f2-fd20479ebcab.png)

EC2에 대한 모든 접근을 허용해주기 위해 `권한 정책`에서 `EC2`로 필터링한 후 `AmazonEC2FullAccess`를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146903442-641c80cd-d177-40f5-b9ad-e4e950ce74a8.png)

그룹 생성을 누르면 그룹이 생성됩니다.   
![image](https://user-images.githubusercontent.com/43658658/146903607-5641f85c-a5c4-4a0d-bd7d-22383a993f95.png)

이제 이 그룹에 속한 사용자는 EC2 인스턴스만 제어할 수 있습니다.




















