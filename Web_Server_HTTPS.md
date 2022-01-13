# 웹서버 HTTPS 변경

## Tomcat 설치

아파치 톰캣(Apache Tomcat)은 아파치 소프트웨어 재단에서 개발한 서블릿 컨테이너(또는 웹 컨테이너)만 있는 `웹 애플리케이션 서버(WAS)`입니다.

톰캣은 웹 서버와 연동하여 실행할 수 있는 자바 환경을 제공하여 자바서버 페이지(JSP)와 자바 서블릿이 실행할 수 있는 환경을 제공하고 있다.

1. tomcat 실행 파일(.exe) 다운로드

![image](https://user-images.githubusercontent.com/43658658/149269846-d52333f8-82e8-4ac8-8578-444d67e89c0f.png)

2. 다운 받은 실행 파일을 실행해서 설치를 진행합니다.

![image](https://user-images.githubusercontent.com/43658658/149269891-f1385a5b-32df-4499-85b8-75486da6819e.png)

Tomcat 밑의 `Service Startup`, `native`를 체크하여, tomcat을 서비스로 실행할 수 있도록 합니다.   
![image](https://user-images.githubusercontent.com/43658658/149270038-843b56bd-4335-4fcb-9f44-d15cac0fb05e.png)



1. tomcat 압축 파일 다운로드

아래의 아파치 사이트에 접속 후 8버전 Tomcat의 `.zip(윈도우)` 파일을 다운로드 합니다.   
=> tomcat.apache.org/download-80.cgi

![image](https://user-images.githubusercontent.com/43658658/149268432-4744ff7d-1af3-4919-aafa-1c46ff4e3215.png)

2. C 드라이브에 압축을 해제합니다.

![image](https://user-images.githubusercontent.com/43658658/149268616-0800761e-2cf1-46bc-ad4a-a049a01a207b.png)

3. tomcat 설치

`관리자 권한`으로 `cmd`를 실행합니다.   
tomcat의 압축을 푼 폴더의 `bin` 폴더로 들어가 명령을 실행합니다.

```
C:\apache-tomcat-8.5.73\bin> service.bat install
```

![image](https://user-images.githubusercontent.com/43658658/149269064-26d6d986-eb0e-4a54-8f47-bd18dee294d5.png)

4. tomcat 실행

```
C:\apache-tomcat-8.5.73\bin> startup.bat
```

5. 오류 해결

`startup.bat` 명령을 통해 `startup.bat` 파일을 실행하면 tomcat이 실행되어야 하는데, 아래의 메시지가 나오며 실행되지 않는 경우가 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/149271814-2a7afe63-5330-4a60-9d6b-13ce234c59cb.png)   
* JAVA_HOME 이 설정 안되서 발생한 문제라는 걸 알 수 있습니다.

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




```
JAVA_HOME
C:\Program Files\Java\jdk1.8.0_91
```



6. 오류 해결 2

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









