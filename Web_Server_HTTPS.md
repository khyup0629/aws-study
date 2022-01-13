# 웹서버 HTTPS 변경

## Tomcat 설치

아파치 톰캣(Apache Tomcat)은 아파치 소프트웨어 재단에서 개발한 서블릿 컨테이너(또는 웹 컨테이너)만 있는 `웹 애플리케이션 서버(WAS)`입니다.

톰캣은 웹 서버와 연동하여 실행할 수 있는 자바 환경을 제공하여 자바서버 페이지(JSP)와 자바 서블릿이 실행할 수 있는 환경을 제공하고 있다.

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










