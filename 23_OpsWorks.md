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

## OpsWorks for Chef Automate

: 완전 관리형 Chef 서버와 자동화 도구 세트로 사용자 워크플로에 지속적 배포 자동화, 규정 준수 및 보안 자동 테스트 및 노드와 상태를 볼 수 있는 사용자 인터페이스를 제공합니다.

> <h3>Chef Automate</h3>

: Chef Software, Inc.에서 제공하는 오픈 소스 프레임워크로서, 코드를 사용하여 애플리케이션이 구성, 배포 및 관리되는 방법을 자동화합니다.

## Chef Automate 서버 생성

![image](https://user-images.githubusercontent.com/43658658/147901747-774bd549-a8e6-4424-bf22-a99366c82cb0.png)

1단계에서 서버 이름, 리전, 인스턴스 타입을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/147901864-849e2816-5ed8-415f-ad8f-7923917be72f.png)

2단계에서 SSH 접근 EC2 키 페어와 서버 엔드포인트를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147901950-7dbaf6f4-ef25-49ca-b614-d2b775077d49.png)   
* `Endpoint` : `Use a custom domain`을 선택해서 사용자 지정 도메인을 선택할 수 있습니다.

3단계에서 네트워크를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147902114-01253d93-3934-4eef-8bdc-315cbf178ffc.png)   
* VPC에는 하나 이상의 퍼블릭 서브넷이 있어야 합니다.
* 퍼블릭 IP 자동 할당이 활성화되어야 합니다.
* 보안 그룹, 서비스 역할 및 인스턴스 프로파일이 아직 없으면 AWS OpsWorks에서 해당 항목을 자동으로 생성할 수 있습니다. 









