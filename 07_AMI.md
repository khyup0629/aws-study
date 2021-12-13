# AMI

인스턴스를 생성하기 위한 기본 이미지 파일입니다.   
단순히 OS만 들어있는 것이 아닌, 애플리케이션, DB, 방화벽 등도 포함될 수 있습니다.

모든 설치와 설정이 완료된 AMI를 이용해서 `Auto Scaling` 기능을 구현할 수도 있습니다.

## AMI 사용 이유

1. 모든 설치와 설정이 완료된 인스턴스 신속하게 생성
2. Auto Scaling
3. 인스턴스를 다른 리전으로 이전
4. 상용 솔루션 사용

## AWS 마켓플레이스

AWS에는 AMI를 사고 팔 수 있는 플랫폼인 `AWS Marketplace`가 있습니다.   

AWS 콘솔에서 `AWS Marketplace`를 검색해서 `[제품 검색]`으로 들어가서 볼 수 있는 방법이 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145740267-c710dbce-cedf-4407-987c-231a6513862d.png)

`인스턴스 시작`의 1단계에서도 `AWS Marketplace`를 찾아볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145740352-88caa333-bb0c-43d7-8b83-398a0f0b4573.png)

마켓플레이스에는 `보안, 네트워킹, 스토리지, 머신 러닝, 비즈니스 인텔리전스, 데이터베이스` 등 다양한 범주의 소프트웨어 제품이 포함되어 있습니다.   
유연한 요금 옵션(무료 평가판, 시간당, 월별, 연간, 다년)과 다양한 배포 방법을 통해 소프트웨어 라이선스 취득 및 구매를 간소화 해놓았습니다.

## AMI 생성하기

> <h3>EC2 인스턴스로 AMI 생성</h3>

1. 이미지를 생성하고 싶은 인스턴스 위에서 [우클릭] > [이미지 및 템플릿] > [이미지 생성]   
![image](https://user-images.githubusercontent.com/43658658/145740809-7f0a8b55-3a89-4ff2-bb54-9255d33d3121.png)

2. AMI 파일에 대해 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/145740966-6ae2e9aa-2664-4e3d-ba4f-63e261bf5e1b.png)   
* `재부팅 안 함` : `활성화`에 체크하면 인스턴스를 재부팅하지 않고 AMI를 생성합니다. 이렇게 AMI를 생성하면 파일시스템의 무결성이 보장되지 않습니다.
  - `파일시스템 무결성` : 데이터베이스에 저장된 데이터 값과 그것이 표현하는 현실 세계의 실제값이 일치하는 정확성을 의미.

3. 아래와 같이 이미지가 생성되고, 이 이미지를 통해 인스턴스를 시작할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145741454-89140096-feef-4fea-ae0a-07c0e1d7bd31.png)

4. 인스턴스 생성 과정은 동일합니다.    
![image](https://user-images.githubusercontent.com/43658658/145741498-46e3795a-2699-448d-ba38-f6de0688350b.png)

(참고) AMI를 생성하면 스냅샷도 같이 생성됩니다.
![image](https://user-images.githubusercontent.com/43658658/145741989-5de8076d-b8dd-40e3-bce0-303a791190d0.png)

## AMI 복사

> <h3>다른 리전으로 AMI 복사</h3>

1. 생성한 AMI를 선택하고 [우클릭] > [AMI 복사]   
![image](https://user-images.githubusercontent.com/43658658/145741684-51d9b505-7aec-4c0a-a298-dfe5058d1edf.png)

2. 대상 리전을 선택하고 [AMI 복사] 버튼을 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/145741816-12532011-b661-4056-b762-48cfed263c10.png)

3. 대상 리전에 들어가면 AMI가 생성된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145742084-164976b8-0263-45f8-9bbb-06475133ac3e.png)






