# Route 53

: EC2, ELB, S3, CloudFront와 연동 가능한 `DNS 서비스`입니다.

기존 DNS 서비스는 도메인 하나를 쿼리하더라도 각 상황에 따라 다른 IP 주소를 알려줍니다.   
Route 53의 기능들을 사용하면 도메인 하나를 쿼리하더라도 각 상황에 따라 다른 IP 주소를 알려줍니다.

## Route 53 기능

AWS를 사용하여 대규모 글로벌 서비스를 할 때 유용합니다.   
AWS 리소스들과 연동할 수 있는 것과 더불어 글로벌 서비스에 꼭 필요한 [라우팅 정책](https://docs.aws.amazon.com/ko_kr/Route53/latest/DeveloperGuide/routing-policy.html?icmpid=docs_console_unmapped)들을 제공해줍니다.

* `지연 시간` : 여러 리전에 리소스가 있고 현재 위치에서 `지연 시간`이 가장 낮은 리전에 속한 리소스의 IP 주소를 알려줍니다. 
* `가중치 기반` : 서버 IP 주소 또는, 도메인(ELB) 마다 가중치를 부여하여 트래픽을 조절하는 기능입니다.
* `장애 조치` : 장애가 발생한 서버의 IP 주소 또는 도메인(ELB)으로는 트래픽을 막는 기능입니다.
* `지리적 위치` :사용자나 리소스의 위치에 기반해서 트래픽을 라우팅하는 기능입니다. 지역별로 다른 IP 주소를 알려줍니다.
* `다중값 응답` : 하나의 도메인에 여러 개의 값(IP 주소)이 연결됩니다.

## Route 53에서 도메인 구매

[Route 53 콘솔] > [등록된 도메인] > [도메인 등록]   
![image](https://user-images.githubusercontent.com/43658658/146714064-65a0bc25-d5ac-435e-9399-a8cc8b381c83.png)

원하는 도메인 이름을 쓰고 [확인] 버튼을 누르면 가용성과 관련 도메인을 추천해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/146714143-19deff8b-f6ef-47fa-ac73-4ba88af65764.png)

다음 페이지에서 도메인에 대한 연락처 세부 정보를 입력합니다.

![image](https://user-images.githubusercontent.com/43658658/146714598-a976edce-894b-48a8-9219-e82858634a97.png)   
* 1년이 지났을 때 해당 도메인을 자동으로 갱신할 것인지 활성화/비활성화 여부를 체크합니다.
* 약관에 동의하고 [주문 완료]를 누릅니다.

## 호스팅 영역

[Route 53 콘솔] > [호스팅 영역] > [호스팅 영역 생성]   
![image](https://user-images.githubusercontent.com/43658658/146712399-207197c7-58a7-4e16-9f19-9ea83220b501.png)

구매한 도메인을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146712501-bcef8df5-1abd-4c42-a3ee-8e94a12a2a2e.png)
* `퍼블릭 호스팅 영역` : 트래픽을 인터넷에서 라우팅합니다.
* `프라이빗 호스팅 영역` : 트래픽을 VPC에서 라우팅합니다.

도메인을 생성하면 트래픽 라우팅 대상에 네임 서버가 4개 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146712792-9aab76a0-ba58-444a-a2af-983097edc72a.png)

도메인을 구입한 사이트의 `도메인 관리`로 들어가서 `네임 서버`를 편집합니다.   
![image](https://user-images.githubusercontent.com/43658658/146712953-6636da49-c3a0-4f3f-9beb-ef7e1ee77ef4.png)

`네임 서버`를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146713265-cb83b96b-c6ad-4416-88e7-27d6904295b5.png)

## A 레코드 설정(EC2 인스턴스와 연동하기)

: DNS 서버에서 IP 주소를 알려주도록 설정하는 기능입니다.

실습 이전에 먼저 EC2 인스턴스를 생성하고 탄력적 IP를 할당해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/146734857-6cae3aef-acd1-494c-9855-d50edcc89ae9.png)

아파치 웹 서버를 설치해줍니다.   
=> [아파치 웹 서버 설치 과정](https://ejko0911.medium.com/centos%EC%97%90-%EC%9B%B9%EC%84%9C%EB%B2%84-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0-227a0fd6e30c)   
![image](https://user-images.githubusercontent.com/43658658/146737430-cc7288fd-a777-47a2-8296-70c331bdd865.png)

다시 [Route 53 콘솔] > [호스팅 영역] > [원하는 도메인] > [도메인 생성]   
![image](https://user-images.githubusercontent.com/43658658/146735154-8a7f8773-6c0c-43cf-b70f-03be0d612453.png)

레코드를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146735368-2c34f045-778f-46c8-a489-50d83e083121.png)   
* `레코드 이름` : 서브 도메인 이름을 설정합니다.
* `레코드 유형` : A 레코드로 DNS가 IPv4를 가리키도록 할 것이므로 `A - IPv4`를 선택합니다.
* `값` : DNS가 가리킬 IP 주소를 입력합니다.
* `TTL` : IP 주소가 갱신되는 주기를 `초`단위로 입력합니다.
  - 예를 들어, 300이라면, 다음에 A 레코드의 IP 주소를 바꿨을 때 5분 뒤에 갱신됩니다.
* `라우팅 정책` : 지연시간, 가중치기반, 지리적위치, 장애조치, 다중값 응답의 기능을 사용할 수 있습니다.

`A 레코드`를 생성하고 A 레코드로 생성한 도메인으로 들어가면 접속이 원활히 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/146737342-35164df0-537a-4717-add2-477f8db993ed.png)

## S3와 연동하기

S3와 Route 53을 연동하기 위해서는 버킷을 2개 생성해야 합니다.   

먼저 정적 웹 호스팅 버킷을 생성하겠습니다.   
버킷 이름은 `서브 도메인`으로 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146739907-9b518d85-7fe2-465d-b5e6-8da756ce521d.png)

[속성] > [정적 웹 사이트 호스팅]을 활성화하고 인덱스 문서를 `index.html`로 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146739418-68e2e386-989e-49d0-85de-fafa116e0860.png)

아래와 같은 내용의 `index.html` 파일을 정적 웹 호스팅 버킷에 업로드합니다.   
``` html
<html>
<head>
    <title>Example CloudFront Distribution</title>
</head>
<body>
    <p>Hello Route 53 - S3</p>
</body>
</html>
```

![image](https://user-images.githubusercontent.com/43658658/146739746-172e9331-55ce-4cda-b551-4912e11ba7c0.png)

다음으로 서브 도메인에 대한 A 레코드를 생성해주겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/146745503-4e50ff3c-56a9-4e01-9cbf-60b62dd3504f.png)   
* `별칭`을 켜주면 `트래픽 라우팅 대상` 항목이 변경됩니다.
* S3 엔드포인트 > 리전 선택 > 루트 도메인 제목으로 된 버킷 선택

이제 `bllu.hongikit.com`로 접속하면 S3 버킷으로 접속됩니다.   
![image](https://user-images.githubusercontent.com/43658658/146745636-027f379f-a72c-424a-bb3a-b60eb3461641.png)

![image](https://user-images.githubusercontent.com/43658658/146741764-79a73131-8132-4952-9b4b-36efbf70ba64.png)   
* `값` : 서브 도메인 버킷의 정적 웹 사이트 호스팅 엔드포인트 주소를 `http://`를 빼고 입력합니다.
* `CNAME` : 현재 서브 도메인을 다른 도메인으로 연결하는 기능입니다.

CNAME으로 설정해도 접속이 잘 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/146745636-027f379f-a72c-424a-bb3a-b60eb3461641.png)

## CloudFront와 연동

먼저 아래 내용의 `index.html`이 업로드 된 버킷을 하나 생성하고, 이 버킷과 CloudFront를 연동합니다.

[CloudFront 콘솔] > [배포] > [배포 생성]   

`CNAME`과 `SSL 인증서`를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/146853270-a831f7fc-9119-45c0-bcec-b0ddae0f6531.png)

나머지 설정은 앞서 배운 [CloudFront 배포 생성](https://github.com/khyup0629/aws-study/blob/main/11_CloudFront.md#s3%EC%99%80-cloudfront-%EC%97%B0%EB%8F%99)을 참고해서 생성합니다.

배포 생성을 마치고 배포가 완료될 때까지 대기합니다.
![image](https://user-images.githubusercontent.com/43658658/146854516-f268f5a9-2e47-4e35-9a18-cb261eba1c83.png)

[Route 53 콘솔] > [호스팅 영역] > [도메인 선택] > [레코드 생성]을 통해 CNAME 레코드를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146854539-9a3f3350-6f8b-4231-87d3-d0dca61db6e5.png)   
* `값`에는 CloudFront 배포의 도메인을 `http://`를 제외하고 작성합니다.

생성을 완료한 후 서브 도메인을 통해 접속하면 접속이 되는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146854462-21f3db32-e490-4398-94e6-f368163429b9.png)

서브 도메인 > CloudFront > S3 버킷

## 장애 조치 기능 사용

인스턴스 `2개`를 생성하고 Route 53의 `장애 조치(Failover) 기능`을 이용해 하나를 Primary, 다른 하나를 Secondary로 지정한 후, Primary에 장애가 발생했을 시에 Secondary로 접속되도록 설정해봅시다.

인스턴스를 2개 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146856261-ff16850c-f6bb-4b25-962c-b8b43d4e4da8.png)

첫 번째 인스턴스에 아래와 같은 내용의 `app.js` 파일을 만들고 웹 서버를 시작합니다.   
=> [node.js 웹 서버 설치 방법](https://github.com/khyup0629/aws-study/blob/main/11_CloudFront.md#ec2%EC%99%80-cloudfront-%EC%97%B0%EB%8F%99)   

``` javascript
var express = require('express');
var app = express();

app.get(['/', '/index.html'], function (req, res) {
  res.send('EC2 Primary');
});

app.listen(80);
```

두 번째 인스턴스에 아래와 같은 내용의 `app.js` 파일을 만들고 웹 서버를 시작합니다.   

``` javascript
var express = require('express');
var app = express();

app.get(['/', '/index.html'], function (req, res) {
  res.send('EC2 Secondary');
});

app.listen(80);
```

두 개의 인스턴스에 웹 서버가 정상적으로 설치되었는지 웹 브라우저로 IP 주소로 접속해서 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/146856332-fc34ad5b-9c05-41fd-9762-476dc30da7f0.png)   
![image](https://user-images.githubusercontent.com/43658658/146856352-83a93ef0-3c40-4e8c-bf0c-197dfe6ece7f.png)

이제 Route 53 콘솔로 접속해서 [상태 검사] > [상태 검사 생성]   
![image](https://user-images.githubusercontent.com/43658658/146856201-6bee437c-b42b-4353-8d6b-d3f4d9849d42.png)

![image](https://user-images.githubusercontent.com/43658658/146856709-29cad1bf-8dea-4ca1-83b7-3e01e4789b69.png)   
* `모니터링 대상` : EC2 인스턴스의 상태를 체크하기 위해 `엔드포인트`를 선택합니다.
* `IP 주소` : Primary로 지정할 인스턴스의 IP 주소를 입력합니다.

![image](https://user-images.githubusercontent.com/43658658/146856777-69e14b75-7a7c-4429-b77a-80c67f2ba243.png)   
* `요청 간격`
  - `표준(30초)` : 2 ~ 3 초에 1회씩 요청
  - `빠름(10초)` : 1초에 1회씩 요청

`Primary`에 대한 상태 검사를 생성한 후 [호스팅 영역] > [도메인 선택] > [레코드 생성]을 통해 `Primary 서버`를 위한 `A 레코드`를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146857341-3fc50e20-80ae-4faf-9978-879af4652e79.png)   
* `라우팅 정책` : 장애 조치
* `장애 조치 레코드 유형` : Primary이므로 `기본` 선택
* `상태 검사` : 방금 생성한 `상태 검사` 선택
* `레코드 ID` : 레코드끼리 서로 구분하는 ID 값으로 임의로 입력합니다.

`Secondary` 서버를 위한 `A 레코드`를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/146857637-9e30f674-0e1d-4ef1-97cd-871989fd9e34.png)   
* `레코드 이름` : 서브 도메인은 Primary의 A 레코드와 동일하게 입력
* `값` : Secondary 인스턴스의 IP 주소 입력
* `장애 조치 레코드 유형` : Secondary 레코드 생성이므로 `보조` 선택
* `상태 검사` : Secondary 유형이므로 상태 검사는 필요 없습니다.

이제 장애 조치 기능을 테스트해보겠습니다. 

설정한 서브 도메인으로 접속합니다. Primary 인스턴스로 접속되는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146857803-b2aa0429-ba8f-4131-828f-d71938c4691f.png)

이제 Primary 인스턴스를 `중지`하고 `약 1분` 정도 후에 다시 같은 서브 도메인으로 접속해보겠습니다.   

Secondary 인스턴스로 접속되는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/146858370-4bd021b3-0267-4bc9-a2fd-f716358a776d.png)

이로써 `장애 조치` 기능이 정상적으로 잘 작동하는 것을 확인할 수 있습니다.



































