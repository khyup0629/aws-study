# 단일 스토리지와 LVM의 성능 차이 실험

## 디스크 성능 테스트 시 고려할 항목

* IOPS : 컴퓨터 저장 장치를 벤치마크(일반적으로 컴퓨터, 스마트폰 등 전자기기의 연산성능을 시험하여 수치화하는 것을 말하는 단어)하는데 사용되는 초당 입출력 측정 단위로 `디스크의 속도`와 관련됩니다.
* BandWidth(bw) : 대역폭으로 주어진 시간 내에 얼마나 많은 정보가 데이터 연결을 통과할 수 있는지를 나타냅니다.

## 성능 차이 실험

`Ubuntu 18.04-LTS`를 설치합니다.

스토리지는 단일 스토리지(3TB)와 LVM(2TB+1TB)를 구성합니다.

먼저 fio를 설치합니다.

```
sudo apt update
sudo apt install fio -y
```

먼저 단일 스토리지를 포맷합니다.

```
mkfs.ext4 /dev/단일스토리지     // ext4 파일 시스템으로 포맷.
```

홈 경로에 `disktest`라는 파일을 생성하고 아래의 내용을 입력합니다.   

```
[global]
ioengine=libaio
filename=/dev/디스크이름
group_reporting=1
direct=1
verify=0
norandommap=1
size=100%
time_based=1
runtime=300s
ramp_time=10
randrepeat=0
refill_buffers
log_avg_msec=1000
log_max_value=1
unified_rw_reporting=1
percentile_list=50:99:99.9:99.99:99.999

[4k_randwrite_qd16_4w]
stonewall
bs=4k
rw=randwrite
iodepth=4
numjobs=4

[4k_randread_qd16_4w]
stonewall
bs=4k
rw=randread
iodepth=4
numjobs=4
```

=> [각 항목의 의미](https://linux.die.net/man/1/fio)

```
[global]
# 테스트에서 사용할 IO엔진 : libaio - 리눅스용 엔진
ioengine=libaio
# 테스트할 디스크
filename=/dev/디스크이름
# job별 리포팅/그룹별 리포팅
group_reporting=1
# direct I/O | buffered I/O
direct=1
verify=0
norandommap=1
# 디스크 전체 파일 생성
size=100%
time_based=1
runtime=300s
ramp_time=10
randrepeat=0
refill_buffers
log_avg_msec=1000
log_max_value=1
unified_rw_reporting=1
percentile_list=50:99:99.9:99.99:99.999

[4k_randwrite_qd16_4w]
# 그룹 구분
stonewall
# 블럭 사이즈
bs=4k
rw=randwrite
iodepth=4
# 4번의 테스트 진행
numjobs=4

[4k_randread_qd16_4w]
stonewall
bs=4k
rw=randread
iodepth=4
numjobs=4
```

`filename`에 작성된 경로를 통해 디스크 성능 테스트를 실시하므로 `테스트를 실시할 디스크경로`로 바꿔줍니다.   
(이때 마운트한 경로가 아닌 원래의 경로를 작성해주어야 합니다)

마지막으로 fio를 실행합니다.

```
sudo fio disktest
```

`disktest`에서 설정한 `runtime` 시간(초) 뒤에 결과가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/148199256-3069b7e6-7dab-4fcf-b9a5-f671b7473202.png)   
* group 0 : Read / group 1 : Write 를 의미합니다.

LVM을 구성해줍니다.   
=> [LVM 구성 방법](https://github.com/khyup0629/aws-study/blob/main/NCP_ENN_Duplication.md#lvm-%EA%B5%AC%EC%84%B1)

마찬가지로 `disktest`의 `filename`의 경로를 수정해주고 `fio` 명령을 실행합니다.   
(`filename=/dev/mapper/vg이름-lv이름`)   

```
sudo fio disktest
```

![image](https://user-images.githubusercontent.com/43658658/148198813-df3b0bbe-15f0-4545-8db5-e888b672785e.png)

LVM을 사용하는 가장 큰 이유는 확장성 때문입니다.   
하지만 안정성이 상당히 떨어지기 때문에(디스크 손상 시 복구가 상당히 어렵습니다) 잘 사용하지 않습니다.

