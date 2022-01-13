# 키 페어

AWS에서는 EC2 인스턴스에 접속하기 위해 `키 페어(공개키/프라이빗키)` 접속 방식을 사용하고 있습니다.

암호화 방식은 공개 키 방식을 이용합니다.   
![image](https://user-images.githubusercontent.com/43658658/145737658-51cb5cee-71e9-4212-9e49-20eeedab5e31.png)   
암호화하는 키(공개 키)와 복호화하는 키(프라이빗 키)를 다르게 설계했고, 외부에 노출되어도 안전합니다.   

ID와 비밀번호를 접속하는 방식에 비해 `훨씬 안전`하고, ID/비밀번호를 입력하지 않아도 되기 때문에 `자동화에도 유리`합니다.

키 페어를 생성하는 방법은 앞서 `EC2 인스턴스 시작` 과정에서 [사용 설정 단계](https://github.com/khyup0629/aws-study/blob/main/03_EC2.md#%ED%82%A4-%ED%8E%98%EC%96%B4-%EC%83%9D%EC%84%B1)에서 다루었기 때문에 넘어가겠습니다.

> <h3>이미 생성된 EC2 인스턴스에서 공개 키 바꾸기</h3>

1. 바꾸려는 `키 파일`의 공개 키를 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/145738658-6a6104c2-a30c-4778-97ff-71b694398438.png)
![image](https://user-images.githubusercontent.com/43658658/145738904-7f51992d-e8da-4fa7-a615-5f4991e74cc6.png)

2. 인스턴스의 `/home/ec2-user/.ssh/authorized_keys` 경로로 접속합니다.
![image](https://user-images.githubusercontent.com/43658658/145738965-e91cec0d-c296-41d7-9d63-161e3893d53a.png)

3. `authorized_keys` 파일의 내용 밑에 바꿀 공개 키를 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/145739200-d9c820dd-8c52-407f-a6e9-6fc0c6c61de8.png)   
* 형식(`ssh-rsa <공개 키> <코멘트>`)을 맞춰서 입력합니다.

4. 추가한 키 파일을 이용해 인스턴스 접속을 시도합니다.   
![image](https://user-images.githubusercontent.com/43658658/145739381-b28c5a80-d3d1-4a6d-b9a7-319afee8e596.png)

5. 정상적으로 접속이 되는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145739411-f6d6caf9-14aa-41f5-9302-496475aed8a3.png)
