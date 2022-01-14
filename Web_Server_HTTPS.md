# 웹서버 HTTPS 변경

## Tomcat 설치

아파치 톰캣(Apache Tomcat)은 아파치 소프트웨어 재단에서 개발한 서블릿 컨테이너(또는 웹 컨테이너)만 있는 `웹 애플리케이션 서버(WAS)`입니다.

톰캣은 웹 서버와 연동하여 실행할 수 있는 자바 환경을 제공하여 자바서버 페이지(JSP)와 자바 서블릿이 실행할 수 있는 환경을 제공하고 있다.

> <h3>1. tomcat 압축 파일 다운로드</h3>

아래의 아파치 사이트에 접속 후 8버전 Tomcat의 `.zip(윈도우)` 파일을 다운로드 합니다.   
=> tomcat.apache.org/download-80.cgi

![image](https://user-images.githubusercontent.com/43658658/149268432-4744ff7d-1af3-4919-aafa-1c46ff4e3215.png)

> <h3>2. C 드라이브에 압축을 해제합니다.</h3>

![image](https://user-images.githubusercontent.com/43658658/149268616-0800761e-2cf1-46bc-ad4a-a049a01a207b.png)

> <h3>3. tomcat 설치</h3>

`관리자 권한`으로 `cmd`를 실행합니다.   
tomcat의 압축을 푼 폴더의 `bin` 폴더로 들어가 명령을 실행합니다.

```
C:\apache-tomcat-8.5.73\bin> service.bat install
```

![image](https://user-images.githubusercontent.com/43658658/149269064-26d6d986-eb0e-4a54-8f47-bd18dee294d5.png)

> <h3>4. tomcat 실행</h3>

tomcat이 설치된 경로의 `bin` 폴더로 들어가서 `tomcat8w.exe` 파일을 실행합니다.   
![image](https://user-images.githubusercontent.com/43658658/149280341-e0d4f5e5-b42f-43a3-9be3-14167e9c3378.png)

`start` 버튼을 누르면, `Service Status` 상태가 `Started`로 변경됩니다.   
![image](https://user-images.githubusercontent.com/43658658/149280795-7d302a84-ee89-4959-ab7c-8069ee952e22.png)   
![image](https://user-images.githubusercontent.com/43658658/149280769-11472062-272f-4fc0-b919-022e63c5781e.png)

웹 브라우저를 열고 `127.0.0.1:8080`으로 접속하면 아래와 같은 화면이 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/149281483-4bb4ca86-efe0-4835-bba9-253031da4764.png)

> <h3>5. 웹 루트 디렉토리 확인</h3>

tomcat의 웹 루트 디렉토리는 `tomcat 디렉토리\webapps\ROOT` 내에 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/149282368-5981a749-d096-47ed-8993-053424a59e2a.png)

경로 내에 `test.jsp` 파일을 아래와 같은 내용으로 만들고 웹 브라우저로 접속해봅니다.   
```
<%
	out.println("Tomcat Install");
%>
```

정상적으로 나타나는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/149282750-41f2d5cf-34ee-4095-9c3e-098995097d69.png)

> <h3>tomcat이 실행되지 않는 오류</h3>

tomcat을 실행시켜도 시작되지 않는 오류가 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/149281535-c1d45176-74aa-466d-9e74-9654a16f82ba.png)

cmd를 `관리자 권한`으로 열고 `startup.bat` 명령을 통해 tomcat을 실행하면 아래의 메시지가 나옵니다.   
![image](https://user-images.githubusercontent.com/43658658/149271814-2a7afe63-5330-4a60-9d6b-13ce234c59cb.png)   
* 환경 변수 `JAVA_HOME`이 설정되지 않아 발생한 문제라는 걸 알 수 있습니다.

`JDK 파일`을 설치해 주어야 합니다.   
JDK 파일은 자바 파일이며, tomcat은 자바 환경으로 실행됩니다.

아래의 경로로 접속해 JDK 파일을 설치합니다.   
=> https://www.oracle.com/java/technologies/downloads/#jdk17-windows   
![image](https://user-images.githubusercontent.com/43658658/149274418-ae020737-f012-4148-98e9-84412599524d.png)

실행 파일을 열고 설치를 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/149274625-df256b16-4d9d-4034-bfe1-659db06843ad.png)

기본적으로 경로는 `C:\Program Files\JAVA\jdk-버전`에 설치됩니다.   
![image](https://user-images.githubusercontent.com/43658658/149274770-ee0eafa7-5822-48db-bdb0-d3b43109ba80.png)

이제 윈도우 환경 변수에 `JAVA_HOME`를 추가합니다.

먼저 `시스템 정보 > 소프트웨어 환경 > 환경 변수`에서 `JAVA_HOME`에 대한 정보를 찾아봅니다.   
![image](https://user-images.githubusercontent.com/43658658/149275806-4e406a4f-b0a4-4c80-aaa7-e7ff7fcf303c.png)

정보가 없다면, `제어판(Control Panel) > 시스템 및 보안 > 시스템 > 설정 변경 > 고급 > 환경변수`에서 `JAVA_HOME`에 대한 환경 변수를 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/149275588-2c717e08-dc90-4f97-8c9e-4166fba0d2e6.png)

`시스템 변수`를 새로 만듭니다.   
![image](https://user-images.githubusercontent.com/43658658/149277895-272daa9b-d245-4459-8d76-b8769151e957.png)

환경 변수명과 `디렉터리 찾아보기` 버튼을 클릭해 JDK 파일 경로(PATH)를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/149280150-d4e76b8b-fc7c-44fe-a62b-9e88a99216a8.png)   

![image](https://user-images.githubusercontent.com/43658658/149278112-3091d3de-649c-4c06-b402-d1400e49b5c5.png)

`시스템 변수`의 `Path` 변수를 선택하고 편집합니다.   
![image](https://user-images.githubusercontent.com/43658658/149278786-574b21e5-1d6d-4e15-8796-35c8c408e701.png)

`JAVA_HOME`이 추가되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/149278969-ffd7585a-af11-4bfa-b513-54774e79618d.png)

```
(깨알 팁!)

환경 변수가 적용이 안될 때,

1. 재부팅
2. cmd에서 `taskkill /f /im /explorer.exe` 후 `explorer.exe`
```

> <h3>인터넷 익스플로러 호환성 문제 & 크롬 .exe 파일 다운 차단 해결</h3>

`Windows 2016 Server`는 설치될 때 `인터넷 익스플로러`만 설치되어 있습니다.   

인터넷 익스플로러를 통해서 `오라클 홈페이지`에 접속하면 호환성 문제로 화면이 제대로 출력되지 않습니다.   
![image](https://user-images.githubusercontent.com/43658658/149273351-75357ae3-2658-44c2-a890-a2ee1b149763.png)

그래서 `Chrome 브라우저`를 설치합니다.

그런데 다운로드 과정에서 `현재의 보안 설정 때문에 이 파일을 다운로드 할 수 없습니다.`라는 경고가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/149273608-5c8f3fb1-1a10-49da-af7a-0fd18fd9baf7.png)

해결 과정은 아래와 같습니다.

먼저, 브라우저 우측 상단에 `톱니바퀴 > 인터넷 옵션`을 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/149273750-a54b6e72-139c-43e7-99c2-f1dc556b1054.png)   
![image](https://user-images.githubusercontent.com/43658658/149273799-ad8df85f-1f72-4ac6-bc40-69c4a8244559.png)

[보안] 탭에서 [인터넷] > [사용자 지정 수준] > [파일 다운로드]를 `사용`으로 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/149273924-61072867-a9d1-45ea-b6d5-202419992dc5.png)

이렇게 하면 정상적으로 파일이 다운로드 됩니다.   

크롬을 통해 `오라클 홈페이지`에 접속하면 정상적으로 화면이 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/149274000-1304f19e-f61c-46ce-b48f-6aaada6904f0.png)

## 인증서 발급

사이트에 SSL을 적용하려면 인증기관으로부터 인증서를 발급받아 tomcat에 설치해야 합니다.

> <h3>1. openssl 설치</h3>

무료로 인증서를 발급 받을 수 있는 `openssl`을 설치해 보겠습니다.   

아래의 사이트에 접속해서 `openssl-0.9.8k_X64.zip` 파일을 다운로드 받습니다.   
=> https://code.google.com/archive/p/openssl-for-windows/downloads   
![image](https://user-images.githubusercontent.com/43658658/149285498-2b9bc134-a4e7-42ed-b391-9dbb2a959cfb.png)

C 드라이브에 압축을 풉니다.   
![image](https://user-images.githubusercontent.com/43658658/149285711-919f9fbc-d08a-409a-be48-e51a5996532e.png)

[제어판] > [시스템 및 보안] > [시스템] > [설정 변경] > [고급] > [환경 변수] > path를 편집합니다.   
![image](https://user-images.githubusercontent.com/43658658/149287155-d265810d-5ee6-4e3b-bcd5-ec6036688597.png)
* 이 설정을 완료하면 cmd에서 openssl 명령을 사용할 수 있습니다.

> <h3>2. CA가 사용할 RSA 개인 키 만들기</h3>

cmd를 `관리자 권한`으로 실행합니다.

`C:`에 `cert` 폴더를 만들고 안에서 작업합니다.   
`C:\cert>`

먼저, 2048bit `개인 키`를 생성하는데, 분실에 대비해서 AES256으로 암호화 합니다.   
```
openssl genrsa -aes256 -out rootca_private.key 2048
```   
![image](https://user-images.githubusercontent.com/43658658/149290044-996c1abf-5413-45a4-b599-60bfcec1caf9.png)
* 적당한 비밀번호를 입력합니다.

`C:\cert\rootca_private.key`가 생성되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/149290207-f71292f2-f77e-44ec-baeb-946c027028dd.png)

> <h3>3. .csr 파일 만들기</h3>

인증서를 발급받기 위해서는 나의 공개 키와 도메인 정보를 담은 `.csr 파일`을 만들어서 인증기관에 보냅니다.   

현재는 자신이 인증기관이 되어 인증서를 발급하므로 인증기관의 공개 키를 `자신의 개인 키`로 서명하여 만들게 됩니다.   
```
openssl req -new -key rootca_private.key -out rootca.csr -config C:\openssl-0.9.8k_X64\openssl.cnf
```   
![image](https://user-images.githubusercontent.com/43658658/149291112-977f1ef9-d635-4ad3-b311-274051106237.png)
* Enter pass phrase for rootca_private.key: 개인키 비밀번호를 입력합니다.
* 인적 사항들을 입력합니다.
* Email Address[]:, A challenge password[]:, An optional company name []: 엔터만 치고 넘어갑니다.

rootca.csr 파일이 만들어졌습니다.   
![image](https://user-images.githubusercontent.com/43658658/149292034-106cabc7-21f1-4264-a388-ab4a20f81ef6.png)

> <h3>4. 10년짜리 self-signed 인증서 만들기</h3>

자신이 인증기관이므로 `.csr 파일`을 자신의 인증기관 `개인 키`로 `서명`합니다.   
```
openssl x509 -req -days 3650 -extensions v3_ca -set_serial 1 -in rootca.csr -signkey rootca_private.key -out rootca.crt
```   
![image](https://user-images.githubusercontent.com/43658658/149291859-4331200b-6c1a-4d2f-8ac6-7f0971f51f96.png)

rootca 인증서(`인증기관의 인증서`)가 만들어졌습니다.   
![image](https://user-images.githubusercontent.com/43658658/149292088-7b5fe19b-37df-4608-a346-2bf8fa95ed11.png)

`openssl x509 -text -in rootca.crt` 명령으로 인증서 내용 확인 가능.   

> <h3>5. 웹 서버가 사용할 .csr 파일 만들기</h3>

이제 웹서버에 필요한 키와 .csr 파일을 생성합니다.   
생성 방법은 위에서 작업한 것과 거의 비슷한 과정을 거치게 됩니다.

```
openssl genrsa -aes256 -out localhost_private.key 2048
```   
![image](https://user-images.githubusercontent.com/43658658/149292760-859d49c0-ec9b-4ad2-bf7a-763bf2ead111.png)

`웹 서버 개인 키` 생성.   
![image](https://user-images.githubusercontent.com/43658658/149292849-b1935caf-530f-4f9f-af13-a5548baa6604.png)

```
openssl req -new -key localhost_private.key -out localhost.csr -config C:\openssl-0.9.8k_X64\openssl.cnf
```   
![image](https://user-images.githubusercontent.com/43658658/149293363-609b3b10-7ce0-48cf-a78d-367161362225.png)

`웹 서버 .csr 파일` 생성.   
![image](https://user-images.githubusercontent.com/43658658/149293435-14a50398-aaa5-4337-8d31-790ae625f9f8.png)

> <h3>6. 5년짜리 localhost용 SSL 인증서 발급하기(CA 개인키로 서명)</h3>

localhost용 인증서를 발급합니다. 이전에 생성한 인증기관(`rootca`)의 인증서와 개인키로 서명합니다.   
```
openssl x509 -req -days 1825 -extensions v3_user -in localhost.csr -CA rootca.crt -CAcreateserial -CAkey rootca_private.key -out localhost.crt
```   
![image](https://user-images.githubusercontent.com/43658658/149293899-613da3e2-bd88-4681-bbbd-2d7d1b18796a.png)

드디어 웹서버를 HTTPS로 사용할 수 있는 SSL 인증서를 발급 받았습니다.   
![image](https://user-images.githubusercontent.com/43658658/149294232-832a7e46-693a-4f80-af22-e0c39140bfb4.png)

## 인증서 적용

> <h3>1. tomcat용 인증서 파일(keystore) 생성</h3>

웹 서버용 인증서와 개인 키를 이용해서 `Tomcat`용 인증서 파일인 `keystore`를 생성합니다.   
```
openssl pkcs12 -export -in localhost.crt -inkey localhost_private.key -out keystore -name "localhost cert"
```   
![image](https://user-images.githubusercontent.com/43658658/149294721-9a83ca9d-2fe1-447d-8b9e-e8ececad8d53.png)   
- `-name` 옵션은 `keystore`의 `alias`
- Enter pass phrase for localhost_private.key: 웹 서버용 개인 키 비밀번호를 입력합니다.
- Enter Export Password: 키스토어 비밀번호를 입력합니다.
- Verifying - Enter Export Password: 키스토어 비밀번호를 확인합니다.

`keystore` 파일이 생성되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/149294940-ca595e11-d18f-49f7-aae0-cb1ac9451da2.png)

> <h3>2. Tomcat 설정</h3>

이전에 설치한 tomcat의 디렉토리 경로(`C:\apache-tomcat-8.5.73\conf`) 안의 `server.xml` 파일에 다음 내용을 추가합니다.   
```
<Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol"
           maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
           keystoreFile="keystore 파일 경로" keystorePass="keystore 파일 비밀번호"
           clientAuth="false" sslProtocol="TLS" />
```   
![image](https://user-images.githubusercontent.com/43658658/149298609-88e51b71-12d1-40af-9449-353c8d5d9f6e.png)   
* keystoreFile="keystore 파일 경로" 
* keystorePass="keystore 파일 비밀번호"

tomcat을 `재실행`하고 `8443 포트`로 접속합니다.   
![image](https://user-images.githubusercontent.com/43658658/149309882-c60a6a48-bcd5-41f2-85e0-fd2fb87cf0ac.png)

웹서버로 부터 받은 인증서를 보증 해줄 인증기관의 인증서가 웹브라우저에는 없으므로 신뢰할 수 없다고 나옵니다.

> <h3>3. 사설 root 인증서 설치하기</h3>

웹 브라우저에서 `도구 -> 인터넷 옵션 -> 내용 -> 인증서`를 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/149300992-43207df4-9e15-48eb-9ab0-9befde9326be.png)

`신뢰할 수 있는 루트 인증기관` 탭에서 "가져오기" 를 실행하여 앞에서 만든 `rootca.crt`를 가져옵니다.
![image](https://user-images.githubusercontent.com/43658658/149301086-763522b4-ff7f-45bb-8792-6034cd08ed23.png)   
![image](https://user-images.githubusercontent.com/43658658/149301159-31144e46-3059-452a-ae2b-ed410408d6ed.png)

`신뢰할 수 있는 루트 인증기관`에 사설 root 인증서(`rootca`)가 등록되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/149301409-46c4f98b-50aa-410b-976b-058894e561bd.png)

이제 `https://localhost:8443`으로 접속하면 HTTPS 접속이 되는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/149309727-eb146cea-8327-40a2-aa95-28473caa1e00.png)

> <h3>IE는 되고 Chrome은 안되는 이유</h3>

`.csr`을 생성할 때 무결성을 입증하는 서명 알고리즘인 `SHA256`를 옵션으로 주지 않아서 인 것 같습니다.

=> https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=oxcow119&logNo=220449926380   
위 사이트를 참조해보면, 보안의 취약점이 발견되어 키를 생성할 때는 기존 2048에서 `4096`으로, 무결성 알고리즘은 `SHA2 그룹`을 써야한다고 되어 있습니다.

`크롬`의 경우 IE보다 훨씬 엄격한 보안 준수를 요구하기 때문에, `SHA256` 알고리즘을 적용하지 않고 생성한 인증서에 대해서는 올바른 보안인증서로 취급하지 않습니다.
