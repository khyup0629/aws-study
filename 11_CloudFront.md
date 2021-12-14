# CloudFront

CloudFront는 전 세계에 파일을 빠른 속도로 배포하는 CDN(Content Distribution Network) 서비스.

## CloudFront 개념

![image](https://user-images.githubusercontent.com/43658658/145957945-bd606b32-79f1-457d-ba75-4f88ce094b16.png)   
EC2 인스턴스, ELB(Elastic Load Balancing), S3 버킷의 내용을 에지 로케이션에 캐싱합니다.   

사용자가 맨 처음 CloudFront 에지 로케이션에 접속했을 때, 원하는 파일이 있으면 에지 로케이션에서 바로 파일을 받게 됩니다.   
하지만 파일이 없으면 에지 로케이션이 원본 파일이 있는 `원본(Origin)`에 접속하여 파일을 가져온 뒤 사용자에게 전달하게 됩니다.   

이때 다른 사용자들도 이 캐시한 파일을 받게 됩니다.   
이후 에지 로케이션이 파일을 캐시하고 있는 동안에는 원본(Origin)에 다시 접속하지 않습니다.

## CloudFront 사용 이유

전 세계를 대상으로 하는 서비스를 준비할 때, 세계 곳곳에 서버를 구축하는 것은 현실적으로 어렵습니다.   
또한, AWS를 사용한다 하더라도 모든 리전에 EC2 인스턴스나 S3 버킷을 생성하는 것은 비효율적이며 비용도 많이 듭니다.

> <h3>전송 속도 향상</h3>

CloudFront를 이용하면 각 사용자들은 가장 가까운 `에지 로케이션`에서 파일을 다운로드 받을 수 있으므로 속도가 빨라집니다.

> <h3>비용 절감</h3>

모든 리전에 EC2 인스턴스나 S3 버킷을 생성할 필요가 없어지므로 `비용이 절감`됩니다.

## CloudFront 특징

> <h3>동적 컨텐츠 전송 지원</h3>

* URL 규칙에 따라 정적 페이지는 캐시하고, 동적 페이지는 바로 EC2 인스턴스로 접속하도록 구성할 수 있습니다.
* HTTP GET 메서드 이외에도 HEAD, POST, PUT, DELETE, OPTIONS, PATCH 메서드를 지원합니다. 특히 POST, PUT, DELETE, OPTIONS, PATCH 메서드는 캐시되지 않고 곧바로 원본(Origin)으로 전달됩니다.

> <h3>사용한 만큼 과금</h3>

선납금이나 최소 금액이 없고, 사용한 만큼 지불하는 방식입니다.

## S3와 CloudFront 연동

> <h3>버킷 생성</h3>

먼저 버킷을 생성하고 아래의 내용을 담은 `index.html` 파일을 업로드합니다.   
![image](https://user-images.githubusercontent.com/43658658/145963118-9328339f-4070-4fcc-a451-6694d9b7b5f5.png)   
![image](https://user-images.githubusercontent.com/43658658/145963074-ef49b42e-cb23-4f69-b942-5a3aa1bd725b.png)

> <h3>CloudFront 배포 생성</h3>

`CloudFront 배포` : CloudFront의 가장 기본 단위입니다. 각 배포마다 독립적인 도메인을 가집니다.

CloudFront 콘솔로 접속해서 [배포] > [배포 생성]   
![image](https://user-images.githubusercontent.com/43658658/145963595-54e274b4-84e7-48b1-8f77-d2a1a8d74184.png)

![image](https://user-images.githubusercontent.com/43658658/145974345-fd419e8f-05db-4948-9eb0-1ed17230e516.png)   
* `원본 도메인` : S3 버킷을 선택합니다.
* `OAI` : 이 단계를 수행하면 사용자가 S3 버킷에서 직접 액세스하지 않고 CloudFront를 통해서만 파일에 액세스할 수 있습니다.
  - OAI는 URL의 형식이고, 이 OAI를 통해서만 버킷에 접근이 가능합니다.

![image](https://user-images.githubusercontent.com/43658658/145976044-272ff614-755f-4f8a-b479-fc55057c0981.png)   
* [origin shield](https://docs.aws.amazon.com/ko_kr/ko_kr/AmazonCloudFront/latest/DeveloperGuide/origin-shield.html) : 에지 로케이션과 오리진 사이에 위치해서 가용성과 캐싱 적중률을 높여줍니다. [Origin Shield 캐싱 적중률 실험](https://linuxer.name/tag/origin-shield/)
* CloudFront가 오리진에 연결을 시도하는 횟수와 `연결 시간 초과로 인식하는 시간`을 세부 설정합니다.

![image](https://user-images.githubusercontent.com/43658658/145977834-263caa81-377f-4462-ac69-e1b8a902e6ce.png)   
* `경로 패턴` : `CloudFront로 파일을 가져올 규칙`을 `정규표현식`에 기반해 작성합니다.
* `자동으로 객체 압축` : 뷰어(사용자)에서 압축된 콘텐츠를 지원하는 경우 CloudFront가 특정 유형의 파일을 자동 압축합니다. 
  - CloudFront가 콘텐츠를 압축하면 파일 크기가 더 작아지므로 `다운로드 속도가 빨라`지고 웹 페이지는 사용자에게 `더 빨리 렌더링`됩니다.
* `뷰어 프로토콜 정책` : 뷰어가 CloudFront 에지 로케이션의 콘텐츠에 액세스하는데 사용하는 `프로토콜 정책`을 선택합니다.
* `허용된 HTTP 메서드` : CloudFront에서 오리진을 처리하고 전달하기 위한 `HTTP 메서드`를 지정합니다.

![image](https://user-images.githubusercontent.com/43658658/145981588-85a8ffe3-029b-4ef4-9aea-87880b4dcd16.png)   
* `캐시 정책` : 캐시 정책을 통해 오리진의 객체가 얼마동안 캐싱되는지를 설정할 수 있습니다.

`CachingOptimized` 캐시 정책은 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/145981789-054892da-ca1c-4447-80fa-b6de5f9ac3e2.png)   
* `TTL` : 캐시에 데이터가 얼마나 머무를지 시간을 설정합니다.
* `Gzip`, `Brotli` : 최종 사용자가 지원할 때 CloudFront에서 `Gzip` 또는 `Brotli` 압축 형식으로 압축된 객체를 요청하고 캐시할 수 있습니다.

![image](https://user-images.githubusercontent.com/43658658/145982824-052eb743-48aa-4642-8c31-6d5e8232fada.png)   
* `가격 분류` : 어느 지역의 에지 로케이션을 이용할 것인지를 선택합니다. 모든 지역은 비용이 조금 더 높습니다.
* `AWS WAF 웹 ACL` : `AWS WAF`를 사용하여 요청을 허용 또는 차단하려는 경우, 이 배포와 연결할 `웹 ACL`을 선택합니다.
  - `AWS WAF` : CloudFront에 전달되는 HTTP 및 HTTPS 요청을 모니터링할 수 있게 해주고 콘텐츠에 대한 액세스를 제어할 수 있게 해주는 `웹 애플리케이션 방화벽`입니다. 
* `CNAME` : CloudFront에서 할당하는 도메인 이름 대신 객체의 URL에 사용할 하나 이상의 도메인 이름을 지정합니다.
* `SSL` : CNAME을 지정한 경우 `SSL 인증서`를 선택해 CNAME에 대한 권한을 검증합니다.
* `HTTP/2` : 최종 사용자가 CloudFront와 통신할 때 배포에서 지원할 HTTP 버전을 선택합니다.
  - 일반적으로 CloudFront가 `HTTP/2`를 사용하는 `최종 사용자`와 통신하도록 구성하면 `지연 시간이 감소`합니다.
* `기본 루트 객체` : 최종 사용자가 배포의 객체가 아닌 배포의 `루트 URL`을 요청할 때 나타날 `객체 파일`을 적습니다.

![image](https://user-images.githubusercontent.com/43658658/145984605-40163fa6-8710-4038-9e11-5f51c61659a3.png)   
* `표준 로깅` : S3 버킷에 CloudFront에서의 객체 요청에 대한 로그를 기록할 것인지 여부입니다.
* `IPv6` : CloudFront에서 `IPv6`의 요청에 응답하도록 하고 싶다면 `켜기`를 선택합니다.


