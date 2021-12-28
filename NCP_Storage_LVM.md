# NCP에서 스토리지 LVM 구성하기

## 서버 구축

Klaytn EN 노드를 위한 서버를 구축합니다.   

```
OS : ubuntu-18.04
CPU: 8 Core
RAM: 32G
HDD: SSD 3TB
Network: 1Gbps
```

## LVM 구성

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
(parted) set 1 lvm on           // 방금 생성한 첫 번째 파티션(1)을 LVM 사용 가능하도록 설정합니다.
(parted) p                      // p(print)로 파티션 설정을 확인합니다.
(parted) quit
partprobe /dev/xvdb1            // 파티션 설정을 갱신합니다.
```

```
parted /dev/xvdc                // `parted /디스크이름`
(parted) mklabel gpt            // 파티션을 GPT 방식(2TB 이상으로 나눌 때 사용)으로 설정합니다.
(parted) mkpart primary 0 100%  // 디스크 전체를 하나의 파티션으로 만듭니다.
(parted) set 1 lvm on           // 방금 생성한 첫 번째 파티션(1)을 LVM 사용 가능하도록 설정합니다.
(parted) p                      // p(print)로 파티션 설정을 확인합니다.
(parted) quit
partprobe /dev/xvdc1            // 파티션 설정을 갱신합니다.
```

```
parted /dev/xvdd                // `parted /디스크이름`
(parted) mklabel gpt            // 파티션을 GPT 방식(2TB 이상으로 나눌 때 사용)으로 설정합니다.
(parted) mkpart primary 0 100%  // 디스크 전체를 하나의 파티션으로 만듭니다.
(parted) set 1 lvm on           // 방금 생성한 첫 번째 파티션(1)을 LVM 사용 가능하도록 설정합니다.
(parted) p                      // p(print)로 파티션 설정을 확인합니다.
(parted) quit
partprobe /dev/xvdd1            // 파티션 설정을 갱신합니다.
```

`fdisk -l` 명령을 통해 파티션이 정상적으로 나누어졌는지 확인합니다.

> <h3>Physical Volumn 생성</h3>

```
pvcreate /dev/xvdb1   // `pvcreate /파티션이름` : 파티션을 pv(Physical Volumn)로 생성합니다.
pvcreate /dev/xvdc1
pvcreate /dev/xvdd1
pvdisplay             // pv들의 정보를 보여줍니다.
```

> <h3>Volumn Group 생성</h3>

```
vgcreate klaytn-data /dev/xvdb1 /dev/xvdc1 /dev/xvdd1 // `vgcreate vg이름 /파티션이름1, /파티션이름2, ...` : [vg이름]으로 [파티션]들을 모두 vg으로 묶습니다.
vgdisplay // vg들의 정보를 보여줍니다.
```

> <h3>Logical Volumn 생성</h3>

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
lsblk --fs    // UUID를 확인합니다. fstab을 설정할 때, 
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

## LVM 용량 추가 후 파티션 추가하기

아래와 같이 `/dev/xvdb` 디스크의 용량을 2TB로 확장합니다.   
![image](https://user-images.githubusercontent.com/43658658/147557714-b345affd-77a9-4743-a7a1-450648e63226.png)

그리고 서버로 접속해 `fdisk -l`를 입력하면 오류가 발생합니다.   
![image](https://user-images.githubusercontent.com/43658658/147557756-4e8a392d-dfe1-479d-93ea-14e15040f0a3.png)

오류의 해결방법은 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/147557802-0840012c-c849-4c0e-b1c9-4f97d5072bcf.png)

그 후에 아래와 같이 파티션을 나눠줍니다.   
![image](https://user-images.githubusercontent.com/43658658/147557833-bd87fde3-8007-483e-8ba9-d5ca67fac5da.png)





