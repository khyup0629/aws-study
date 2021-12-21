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

## IAM 사용자 비밀번호 설정

IAM 사용자가 콘솔에 로그인할 수 있도록 하기 위해 비밀번호를 설정할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146906119-6cd8a191-aec7-4c83-b740-3f924e0c1a08.png)

`콘솔 액세스`를 `활성화`하고 비밀번호를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/146906406-72aefc28-f0ed-4a8b-9767-49a6e262561c.png)

IAM 사용자가 콘솔에 로그인하기 위해서 [대시보드] > [이 계정의 IAM 사용자를 위한 로그인 URL]로 접속합니다.   
![image](https://user-images.githubusercontent.com/43658658/146906705-d12386eb-a1ce-4818-9481-d7baa710e02e.png)

생성한 사용자로 로그인합니다.   
![image](https://user-images.githubusercontent.com/43658658/146906952-1592ed24-f996-472b-87ec-4cf9a90ff159.png)

이 사용자의 경우 권한 설정에 의해 EC2 인스턴스와 S3는 제어가 가능하지만 RDS는 제어가 불가능합니다.   
![image](https://user-images.githubusercontent.com/43658658/146907850-08e55bba-c10a-4164-b668-06f6a5c063b1.png)   
![image](https://user-images.githubusercontent.com/43658658/146907914-54a4feaf-8cf0-49b5-8449-308eea58644a.png)   
![image](https://user-images.githubusercontent.com/43658658/146908016-a8d15da6-2870-456e-8ed1-ed7cc8ea1d2d.png)

## IAM 역할 생성

EC2 인스턴스 전용으로 S3로의 접근만 허용하는 IAM 역할을 생성해보겠습니다.

IAM 역할을 사용하면 EC2 인스턴스 생성 즉시 API로 AWS 리소스에 접근할 수 있습니다.   
Auto Scaling 기능으로 EC2 인스턴스를 자동으로 늘려나갈 때 액세스 키와 시크릿 키를 일일이 설정해 줄 필요가 없어집니다.

[IAM 콘솔] > [역할] > [역할 만들기]   
![image](https://user-images.githubusercontent.com/43658658/146908727-2e012c13-1a9f-47b1-9d6d-a62280989297.png)

EC2를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/146908863-9efe167c-6453-431f-beba-f3bae499d917.png)

S3로의 접근만 허용해보겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/146908982-d3087d2a-3311-401a-b7f6-231ac19d42ad.png)

역할 이름을 입력하고 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146909080-768f82e4-50c1-4b09-a696-7cac94915c52.png)

## EC2 인스턴스에서 IAM 역할 사용

기존에 만들어진 EC2 인스턴스는 IAM 역할을 사용할 수 없습니다.   

인스턴스를 시작할 때 IAM 역할을 사용해야 합니다.

인스턴스 시작의 3번째 단계에서 `IAM 역할`을 선택할 수 있습니다.
![image](https://user-images.githubusercontent.com/43658658/146909864-905999d1-3b44-495e-8f29-a9008d4f268a.png)

인스턴스를 생성하고 SSH로 접근해서 `AWS CLI`를 통해 S3 버킷의 파일 목록을 보겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/146910326-d88f585b-bede-4566-b753-1ee65ec751d3.png)

정상적으로 확인되는 것을 확인할 수 있습니다.

다음으로 DynamoDB에 접근해보겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/146910444-c5f35b71-f0e4-472b-8e46-12b8b2a338bd.png)

IAM 역할에 의해 S3를 제외한 나머지 서비스에는 접근이 되지 않는 것을 확인할 수 있습니다.













