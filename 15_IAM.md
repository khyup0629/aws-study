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

## IAM 사용자 생성

[IAM 콘솔] > [사용자] > [사용자 추가]   
![image](https://user-images.githubusercontent.com/43658658/146903826-3ef884fc-2f1e-4e2d-b1c8-530be566e5fb.png)

사용자 이름과 자격 증명 유형을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146904341-4c22350c-ab35-4772-8d18-f661cabfc441.png)   
* 액세스 키 : 액세스 키 ID와 시크릿 액세스 키를 활성화합니다.
* 암호 : 사용자가 로그인할 수 있도록 하는 비밀번호를 활성화합니다.

이전에 생성한 `EC2Admin` 그룹에 추가합니다.
![image](https://user-images.githubusercontent.com/43658658/146904879-c22b1bc1-da94-4eb5-be1f-c01a6dd40dbb.png)

태그 단계와 검토 단계를 거쳐서 완료 단계로 오면 액세스 키와 시크릿 키를 다운로드 받을 수 있습니다.   
(이 과정에서 다운로드 받지 못했다면 이후에 액세스 키를 폐기하고 새로 생성해야 합니다)   
![image](https://user-images.githubusercontent.com/43658658/146905291-d511553e-cfa8-4afc-b36c-8f612220c957.png)

이제 이 생성한 사용자에게 S3에 접근할 수 있는 권한을 부여해보도록 하겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/146905483-d5f60797-a1d5-4d87-81a0-cb60e79411fe.png)   

사용자 이름을 클릭해서 `권한 추가`를 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/146905589-a52e16cb-92ce-4f00-ab03-56cc7b3daf8b.png)

`AmazonS3FullAccess` 정책을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146905809-b19b03a4-8a54-476a-a14e-c560d9d1a5cc.png)

그룹 권한과 사용자 권한이 모두 설정되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/146905896-e7a8e7ec-d3e6-4aba-bd36-0f04398c8ed9.png)

















