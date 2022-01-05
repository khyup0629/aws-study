# 단일 스토리지와 LVM의 성능 차이 실험

`Ubuntu 18.04-LTS`를 설치합니다.

스토리지는 단일 스토리지(2TB)와 LVM(1TB+1TB)를 구성합니다.

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

=> [각 항목의 의미](https://jcil.co.kr/29#:~:text=%EC%82%AC%EC%9D%B4%ED%8A%B8%EB%A5%BC%20%EC%B0%B8%EA%B3%A0%ED%95%98%EC%8B%9C%EA%B2%8C%20%ED%8E%B8%ED%95%A9%EB%8B%88%EB%8B%A4.-,fio%20menual,-linux.die.net)

`filename`에 작성된 경로를 통해 디스크 성능 테스트를 실시하므로 `테스트를 실시할 디스크경로`로 바꿔줍니다.   
(이때 마운트한 경로가 아닌 원래의 경로를 작성해주어야 합니다)

마지막으로 fio를 실행합니다.

```
sudo fio disktest
```

`disktest`에서 설정한 `runtime` 시간(초) 뒤에 결과가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/148196815-7ebd4488-1c70-4a6b-a736-af46b1ba333c.png)


LVM을 구성해줍니다.   
=> [LVM 구성 방법](https://github.com/khyup0629/aws-study/blob/main/NCP_ENN_Duplication.md#lvm-%EA%B5%AC%EC%84%B1)

마찬가지로 `disktest`의 `filename`의 경로를 수정해주고 `fio` 명령을 실행합니다.   
(`filename=/dev/mapper/vg이름-lv이름`)   

```
sudo fio disktest
```












LVM을 사용하는 가장 큰 이유는 확장성 때문입니다.
하지만 안정성이 상당히 떨어지기 때문에(디스크 손상 시 복구가 상당히 어렵습니다) 잘 사용하지 않습니다.


