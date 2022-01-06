# ENN 노드 이중화(with NCP)

## 아키텍쳐

=> [AWS 아키텍처 툴 draw.io](https://app.diagrams.net/?splash=0&libs=aws4)   
![ENN_Duplication_Architecture](https://user-images.githubusercontent.com/43658658/147739874-ba642877-2212-41f7-a01c-a7e671824a1a.png)

## 서버 생성

Klaytn EN 노드를 위한 서버를 생성합니다.   

```
OS : ubuntu-18.04
CPU: 8 Core
RAM: 32G
HDD: SSD 3TB
Network: 1Gbps
```

방화벽 인바운드 규칙은 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/147733319-d7bd362f-b08b-4155-873f-d4bb2711d2b5.png)

방화벽 아웃바운드 규칙은 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/147733363-232439dc-2a2c-4990-a15c-78f660798027.png)


## LVM 구성

`LVM(Logical Volumn Manager)` : 물리 디스크를 볼륨 그룹으로 묶고 이것을 논리 볼륨으로 분할하여 관리하는 기술입니다.

먼저 1TB 스토리지 `3개`를 생성하고 EN 노드 서버와 연결합니다.   
![image](https://user-images.githubusercontent.com/43658658/147555586-ca63398f-b716-4c9e-975e-501c79a04ffa.png)

EN 노드 서버로 PuTTy를 이용해 SSH 접속합니다.   
(접속 `ID/PW`는 [EN 노드 서버 선택] > [서버 관리 및 설정 변경] > [관리자 비밀번호 확인]에서 `.pem` 키 파일을 넣고 확인합니다)

> <h3>파티션 나누기</h3>

3개의 스토리지를 파티션으로 나눕니다.      
(/dev/xvdb -> /dev/xvdc -> /dev/xvdd)

```
parted /dev/xvdb                // `parted /디스크이름`
(parted) mklabel gpt            // 파티션을 GPT 방식(2TB 이상으로 나눌 때 사용)으로 설정합니다.
(parted) mkpart primary 0 100%  // 디스크 전체를 하나의 파티션으로 만듭니다.
(parted) set 1 lvm on           // 방금 생성한 첫 번째 파티션(1)을 LVM에서 사용 가능하도록 설정합니다.
(parted) p                      // p(print)로 파티션 설정을 확인합니다.
(parted) quit
partprobe /dev/xvdb1            // 파티션 설정을 갱신합니다.
```

```
parted /dev/xvdc                // `parted /디스크이름`
(parted) mklabel gpt            // 파티션을 GPT 방식(2TB 이상으로 나눌 때 사용)으로 설정합니다.
(parted) mkpart primary 0 100%  // 디스크 전체를 하나의 파티션으로 만듭니다.
(parted) set 1 lvm on           // 방금 생성한 첫 번째 파티션(1)을 LVM에서 사용 가능하도록 설정합니다.
(parted) p                      // p(print)로 파티션 설정을 확인합니다.
(parted) quit
partprobe /dev/xvdc1            // 파티션 설정을 갱신합니다.
```

```
parted /dev/xvdd                // `parted /디스크이름`
(parted) mklabel gpt            // 파티션을 GPT 방식(2TB 이상으로 나눌 때 사용)으로 설정합니다.
(parted) mkpart primary 0 100%  // 디스크 전체를 하나의 파티션으로 만듭니다.
(parted) set 1 lvm on           // 방금 생성한 첫 번째 파티션(1)을 LVM에서 사용 가능하도록 설정합니다.
(parted) p                      // p(print)로 파티션 설정을 확인합니다.
(parted) quit
partprobe /dev/xvdd1            // 파티션 설정을 갱신합니다.
```

`fdisk -l` 명령을 통해 파티션이 정상적으로 나누어졌는지 확인합니다.

> <h3>Physical Volumn(PV) 생성</h3>

각 파타션을 `LVM에서 사용할 수 있도록` 하기 위해서 PV로 만들어주어야 합니다.

PV는 일정한 크기의 PE(Physical Extent)들로 구성됩니다.   
![image](https://user-images.githubusercontent.com/43658658/148312332-3bb01238-f077-4928-a10d-7288b812935b.png)   
PV를 구성하는 일정한 크기의 블록으로 LV(Logical Volume)의 `LE`(Logical Extent)들과 1:1로 대응됩니다.

```
pvcreate /dev/xvdb1   // `pvcreate /파티션이름` : 파티션을 pv(Physical Volumn)로 생성합니다.
pvcreate /dev/xvdc1
pvcreate /dev/xvdd1
pvdisplay             // pv들의 정보를 보여줍니다.
```

> <h3>Volumn Group 생성</h3>

다음으로 PV들로 초기화된 장치들을 `VG`로 통합합니다.

VG는 `PV들의 집합`으로 `LV를 할당할 수 있는 공간`입니다.   
사용자는 VG안에서 원하는대로 공간을 쪼개서 LV로 만들 수 있습니다.

```
vgcreate klaytn-data /dev/xvdb1 /dev/xvdc1 /dev/xvdd1 // `vgcreate vg이름 /파티션이름1, /파티션이름2, ...` : [vg이름]으로 [파티션]들을 모두 vg으로 묶습니다.
vgdisplay // vg들의 정보를 보여줍니다.
```

> <h3>Logical Volumn 생성</h3>

LV는 사용자가 최종적으로 다루게 되는 논리적인 스토리지입니다.

LV는 일정한 크기의 LE(Logical Extent)들로 구성됩니다.   
![image](https://user-images.githubusercontent.com/43658658/148312664-ccf73d87-11c3-4eee-a7ee-05910a10eecd.png)      
`LE`(Logical Extent)는 PE들과 1:1로 맵핑됩니다.

```
lvcreate -l 100%FREE -n klaytn-data klaytn-data // `lvcreate -l 100%FREE -n lvm이름 vg이름` : [vg이름]의 전체 남은 용량(100%FREE)을 [lvm이름]의 LVM으로 생성합니다.
lvdisplay // lvm들의 정보를 보여줍니다.
```

LVM 정보는 `/dev/klaytn-data/klaytn-data`와 `/dev/mapper/klaytn--data-klaytn--data` 두 곳에 생성됩니다.

> <h3>포맷</h3>

```
mkfs.ext4 /dev/klaytn-data/klaytn-data // ext4 파일시스템으로 LVM을 포맷합니다.
```

> <h3>마운트</h3>

```
lsblk --fs    // UUID를 확인합니다. fstab을 설정할 때, LVM 디렉토리 경로나 UUID를 사용할 수 있습니다.
mkdir /klay-data  // LVM을 마운트할 klay-data 디렉토리를 만듭니다.
```

`vim /etc/fstab` // 마운트 설정을 편집합니다.   
![image](https://user-images.githubusercontent.com/43658658/147557286-f5462ff5-3c9e-4f05-8c4d-8508d3116e62.png)   
```
/dev/klaytn-data/klaytn-data /klay-data ext4 defaults 0 0
```

```
mount -a // fstab 파일에 설정된대로 마운트를 진행합니다.
```

## 용량 추가 후 LVM에 추가하기

(별도의 환경에 테스트 서버를 구축해서 테스트했습니다)

먼저 1TB + 1TB의 LVM을 구성하고 hello가 적힌 `test.txt`라는 파일을 저장합니다.

볼륨 하나의 용량을 2TB로 확장합니다.

아래와 같이 `/dev/xvdb` 디스크의 용량을 2TB로 확장합니다.   
![image](https://user-images.githubusercontent.com/43658658/147557714-b345affd-77a9-4743-a7a1-450648e63226.png)

그리고 서버로 접속해 `fdisk -l`를 입력하면 오류가 발생합니다.   
![image](https://user-images.githubusercontent.com/43658658/147557756-4e8a392d-dfe1-479d-93ea-14e15040f0a3.png)

오류의 해결방법은 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/147557802-0840012c-c849-4c0e-b1c9-4f97d5072bcf.png)

그 후에 아래와 같이 파티션을 나눠줍니다.   
![image](https://user-images.githubusercontent.com/43658658/147557833-bd87fde3-8007-483e-8ba9-d5ca67fac5da.png)

# ENN 노드 서버 구축

```
# mkdir /klay             // Binary 를 위한 Directory 생성
# mkdir /klay-data        // Klaytn Data 를 위한 Directory 생성
# cd /klay
# wget https://packages.klaytn.net/klaytn/v1.7.3/ken-v1.7.3-0-linux-amd64.tar.gz      // https://docs.klaytn.com/node/download/ -> 왼쪽 사이트로 접속해서 가장 최신 버전의 링크를 사용
# tar zxvf ken-v1.7.3-0-linux-amd64.tar.gz  
# mv ./ ken-v1.7.3-0-linux-amd64/* ./
# vi conf/kend.conf
```

<kend.conf>   
![image](https://user-images.githubusercontent.com/43658658/147734066-5b72aa03-fe3e-4bee-80f5-e8f942b46625.png)

```
# cd /klay-data
// https://packages.klaytn.net/cypress/chaindata/ -> 왼쪽 사이트로 접속해서 가장 최신 버전의 링크를 사용
# wget https://s3.ap-northeast-2.amazonaws.com/klaytn-chaindata/cypress/klaytn-cypress-chaindata-20211227011211.tar.gz  // NCP 기준 약 2시간 30분 소요
# tar zxvf  klaytn-cypress-chaindata-20211227011211.tar.gz            // 압축 해제 약 2시간 30분 소요
# rm  klaytn-cypress-chaindata-20211227011211.tar.gz      // 불필요한 Disk 소비를 제거하기 위해 삭제
```

앞서 구축한 서버의 이미지 파일을 생성하고 LVM, ENN 노드 설정이 구성된 같은 서버를 하나 더 생성합니다.   
(같은 작업을 반복해도 됩니다)   
![image](https://user-images.githubusercontent.com/43658658/147734989-4d601ba8-4f74-42aa-87e7-1fcf19438676.png)

# nmon 설정

: 리눅스 기반 시스템의 현재 리소스를 모니터링 할 수 있는 도구입니다.

![image](https://user-images.githubusercontent.com/43658658/147639229-7bbd458e-b0e9-4ba4-bd56-0df645056dec.png)

```
apt -y install nmon       // nmon 설치
nmon                      // nmon이 잘 설치되었는지 확인합니다.
```

`nmon` 명령어로 아래와 같은 화면(대화식)이 나타나면 잘 설치되었다는 의미입니다.   
![image](https://user-images.githubusercontent.com/43658658/147638477-6af82ce6-8447-4a2b-aad2-35d8f9535b5a.png)

![image](https://user-images.githubusercontent.com/43658658/147639250-7de3c704-ba8c-4d3e-a97b-84a3351f4df7.png)

```
mkdir /var/log/nmon                         // nmon의 매트릭이 저장될 디렉토리 경로를 생성합니다.
nmon -f -c 20160 -s 30 -m /var/log/nmon     // 아래 설명 확인
```

=> [nmon 명령어 옵션](https://www.ibm.com/docs/ko/aix/7.2?topic=n-nmon-command)   
`nmon -f` : 기록 모드로 nmon 실행
`nmon -c 20160 -s 30` : 매트릭을 생성하는 횟수(-c), 매트릭을 생성하는 간격(-s)을 의미합니다.   
  - 둘을 곱하면 총 604,800초로 7일(1주일)을 나타냅니다. 즉 1주일 동안 매트릭을 하나의 nmon 파일에 저장합니다.

`nmon -m /var/log/nmon` : `/var/log/nmon` 경로에 nmon 파일을 저장합니다.

![image](https://user-images.githubusercontent.com/43658658/147639261-39711490-70b5-4547-8dd7-42993a95a8a6.png)

```
ps aux | grep nmon      // nmon이 백그라운드로 잘 실행되고 있는지 체크합니다.
cd /var/log/nmon
ls                      // /var/log/nmon 경로에 nmon 파일이 정상적으로 생성되는지 체크합니다.
```

(번외)

현재 서버의 디스크 자원을 `웹`에서 편리하게 모니터링 할 수 있도록 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147639311-ac2a8dd8-1382-4a65-a239-bec6991051c0.png)

```
mkdir /script
cd script
vim check.sh
chmod 755 check.sh
```

<check. sh>   
![image](https://user-images.githubusercontent.com/43658658/147639510-1fa02b5c-eadb-4d4b-be9a-dfbe42a930b0.png)

웹 서버를 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/147639723-3639a3c9-ad48-4e45-b28e-766950988dc2.png)

웹 서버를 실행합니다.   
![image](https://user-images.githubusercontent.com/43658658/147639753-0e821362-b516-46be-860a-8c79a0ffc780.png)

`/var/www/html/Share` 경로를 만듭니다.   
![image](https://user-images.githubusercontent.com/43658658/147639778-0773845e-a81f-44c9-8463-87babb2e3c03.png)

`check.sh` 스크립트 파일을 실행해서 `/var/www/html/Share` 경로에 `checkDisk.txt` 파일이 생성되도록 합니다.   
![image](https://user-images.githubusercontent.com/43658658/147639639-7e9d4d5b-367f-446e-85b8-e3a8681cc341.png)

`IP주소/Share`로 웹 브라우저를 통해 접속하면 아래와 같이 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/147639896-ad429a8e-90ba-409f-b9cf-dcafb660ba77.png)   

파일을 열어보면 아래와 같이 디스크 정보가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/147639928-aac4eaa6-c135-4619-9754-88ea2a1bb995.png)

```
crontab -e        // crontab에 nmon 명령어와 check.sh 실행 주기를 등록합니다.
```

=> [crontab 사용법](https://jdm.kr/blog/2)   

nmon 파일이 월요일을 시작으로 1주일마다 하나씩 만들어지도록 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147640145-fedaafa0-dccb-4d05-aa87-d8031043fe44.png)   
* 순서대로 `분-시간-일-월-요일` 순입니다. 괄호 안의 숫자 범위 내로 별 대신 입력 할 수 있습니다.
  - 요일에서 0과 7은 일요일입니다. 1부터 월요일이고 6이 토요일입니다.

# 프로메테우스 에이전트 설치

먼저 아래의 사이트에 접속해 `node_exporter` 압축 파일의 링크를 복사합니다.
=> [node exporter 압축 파일 링크 복사](https://prometheus.io/download/)   
![image](https://user-images.githubusercontent.com/43658658/147640815-7a932841-84bc-4fe7-960a-e3955d7bc552.png)

`cd /home` 경로로 들어가서 복사한 링크를 붙여넣기해서 압축 파일을 다운 받습니다.   
![image](https://user-images.githubusercontent.com/43658658/147640911-f2f3890e-52b2-4950-b168-6b6b8d0d3bf6.png)

압축을 해제합니다.   
![image](https://user-images.githubusercontent.com/43658658/147640923-fc8c88db-00f0-4e41-8f11-ed636c392720.png)

`/home/node_exporter-1.3.1.linux-amd64` 경로에 `node_exporter`가 생성되어 있음을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/147641013-31f4b984-960a-413e-b9e1-b94df70a8e50.png)   

`./node_exporter`로 node exporter를 실행하면, node_exporter라는 프로메테우스 에이전트가 현재 ENN 서버에서 매트릭 값을 가져옵니다.   
![image](https://user-images.githubusercontent.com/43658658/147642572-508e75f4-b739-490a-827a-fd109be4cebc.png)

하지만 node_exporter를 항상 실행한 채로 둘 수는 없기 때문에 node_exporter를 데몬에 서비스로 등록하겠습니다.

```
mv /home/node_exporter-1.3.1.linux-amd64 /home/node_exporter    // 디렉토리 이름을 변경합니다.
vim /etc/systemd/system/node_exporter.service                   // node_exporter를 서비스로 등록하는 설정을 진행합니다.
```

<node_exporter.service>   
![image](https://user-images.githubusercontent.com/43658658/147642620-ea3398a3-660b-4767-a866-52ce3c7edb26.png)

데몬을 재시작해주면 설정이 완료됩니다.

```
systemctl daemon-reload
```

마지막으로 등록한 `node_exporter.service` 서비스를 실행합니다.

```
systemctl enable node_exporter.service  // 서비스 상시 가동
systemctl start node_exporter.service   // 서비스 시작
systemctl status node_exporter.service  // 서비스 가동 상태 확인
```

![image](https://user-images.githubusercontent.com/43658658/147714364-b4982b6f-4577-41d6-b6c7-e9c0faa1d4a8.png)

## Load Balacing 구축

NCP는 로드 밸런서를 구축하기 위해서 사전에 `로드 밸런서 전용 서브넷`을 구성해야합니다.   
![image](https://user-images.githubusercontent.com/43658658/147736622-1936d8eb-d6d3-431a-988c-392e04d3578a.png)

아래와 같이 TCP 프로토콜로 5개의 포트에 대한 `타겟 그룹`을 구성합니다.   
타겟은 이전에 생성한 2개의 서버입니다.   
![image](https://user-images.githubusercontent.com/43658658/147734713-52cb974f-8f40-4b5e-9187-d66cbe359f03.png)

`네트워크 로드 밸런서(NLB)`를 생성합니다. 아래와 같이 리스너를 구성합니다.   
각 리스너는 포트번호에 맞는 타겟 그룹으로 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/147734914-da57acfb-3eff-45f8-998b-bf12433575ea.png)   
* NCP에서 로드 밸런서를 생성할 시, 1개의 리스너와 1개의 타겟 그룹만 설정이 가능하고, 생성이 완료된 후에 `리스너 설정 변경`을 통해 리스너와 타겟 그룹을 추가해야 합니다. 

`NLB`를 구성합니다.
![image](https://user-images.githubusercontent.com/43658658/147734968-a6cefcfb-1a2f-470b-bcd0-2bc4df5e403e.png)

두 개의 서버로 SSH 접속해 ENN 노드를 실행합니다.   
```
# /klay/bin/kend start & // ENN 노드 실행
```

ENN 노드를 실행하면 Klaytn 포트가 열립니다. 5개의 포트가 정상적으로 열렸는지 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/147735136-b25c6b83-0098-4824-ae83-bae3c316dc66.png)

## 부하 테스트 진행




