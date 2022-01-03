# OpsWorks

: 모든 형태의 애플리케이션 구성과 배포를 자동화해주는 서비스입니다.

Elastic Beanstalk보다 좀더 자유도가 높습니다.   
Elastic Beanstalk은 미리 제공되는 애플리케이션 플랫폼만 사용할 수 있지만 `OpsWorks`는 `Chef`를 이용하여 어떤 애플리케이션이든 구성과 배포를 자유롭게 할 수 있습니다.   
CloudFormation으로 모든 부분을 구현하기는 부담스럽고, Elastic Beanstalk보다는 좀더 세세한 설정을 하고 싶은 사용자에게 적합합니다.

![image](https://user-images.githubusercontent.com/43658658/147897743-151f7c62-0c3d-457f-8e6e-0408fbdf1b64.png)   
* 스택(Stack): OpsWorks에서 최상위 단위입니다. 스택 안에 여러 개의 레이어가 들어갑니다. 또한, MySQL 데이터베이스, 로드밸런서 등도 포함됩니다.
* 레이어(Layer): EC2 인스턴스 생성, Elastic IP 할당, 애플리케이션을 구성하는 템플릿입니다. 
  - Chef 레시피를 이용하여 설치, 업데이트, 배포 작업을 정의합니다. 로드 기반, 시간 기반, Auto Scaling 설정도 포함됩니다.
* 인스턴스(Instance): 레이어에 포함된 EC2 인스턴스입니다. 레이어에 정의한 Chef 레시피대로 애플리케이션이 설치됩니다.
* App: 사용자가 작성한 소스의 배포 단위입니다. S3 버킷, HTTP 및 Subversion, Git 저장소를 사용하여 배포할 수 있습니다.

* `시간 기반(Time-based)` : EC2 인스턴스가 생성, 시작, 정지되는 날짜와 시간을 정의할 수 있습니다.
* `로드 기반(Load-based)` : CPU 사용률, 메모리 사용량 등에 따라 EC2 인스턴스를 생성하거나 삭제합니다.
* `Chef 쿡북(Cookbook)` : 레시피, 속성, 템플릿, 라이브러리 등의 묶음입니다.
* `Chef 레시피(Recipe)` : 애플리케이션 설치 및 업데이트, 소스 배포 방법이 정의된 파일입니다.

## 스택 생성

[OpsWorks 콘솔] > [스택] > [Add your first stack]   
![image](https://user-images.githubusercontent.com/43658658/147899444-712fd5f8-f01b-465c-871a-54a5ea3afba5.png)

Chef에 대한 설정을 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/147899533-f71eb62a-87d0-4865-8570-2b29994d7444.png)   
* `Use custom Chef cookbooks` : OpsWorks에서 제공하는 Chef 쿡북 이외에 인터넷에 공개된 Chef 쿡북이나 사용자가 작성한 Chef 쿡북을 사용하는 옵션입니다.

OpsWorks용 IAM 역할을 만들어서 지정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147900124-2685d910-a426-4a89-adb3-9108724bd6d8.png)

스택을 생성합니다.












