## EBS(Elastic Block Store)

`EBS` : EC2 인스턴스에 장착하여 사용할 수 있는 `가상 저장 장치`.   
* EC2 인스턴스에서 제공하는 기본 용량(루트)보다 `더 사용해야 할 때` 사용.
* 운영체제를 중단시키지 않고 용량을 자유롭게 늘리고 싶을 때 사용.
* 영구적인 데이터 보관이 필요할 때 사용.
* RAID 등의 고급 기능이 필요할 때 사용.

> <h3>기본 개념</h3>

* 볼륨(Volume): EBS의 가장 기본적인 형태로 OS에서 바로 사용 가능한 형태입니다.
* 이미지(Image): AMI(Amazon Machine Image)를 줄여 부르는 말입니다. 
* 스냅샷(Snapshot): EBS 볼륨의 특정 시점을 그대로 복사하여 저장한 파일을 뜻합니다. 이 스냅샷을 이용하여 `EBS 볼륨`과 `AMI`를 생성할 수 있습니다.
* IOPS(Input/Output Operation Per Second): 저장 장치의 성능 측정 단위입니다. AWS에서는 추가 비용을 지불하고 높은 성능(IOPS)의 EBS를 생성할 수 있습니다. 설정할 수 있는 값은 최소 100 IOPS에서 4000 IOPS까지입니다.
  - IOPS는 16KB 단위로 처리됩니다. 따라서 크기가 작은 파일이 있다면 16KB 단위로 묶어서 처리하면 높은 성능을 낼 수 있습니다.

## EBS 볼륨

> <h3>EBS 볼륨 추가하기</h3>

1. EC2 콘솔에서 [볼륨] > [볼륨 생성]   
![image](https://user-images.githubusercontent.com/43658658/145525116-3ec3287b-7653-4599-8358-e68000ea0fe2.png)

2. 볼륨에 대한 설정   
![image](https://user-images.githubusercontent.com/43658658/145525368-55c0ae27-3afc-4cbe-84e6-41cd4ff2f69f.png)   
* 가용 영역 : 볼륨을 추가할 인스턴스의 가용 영역과 같아야 합니다.
* 스냅샷 ID : EBS 스냅샷이 있다면 여기서 선택합니다.

> <h3>인스턴스에 볼륨 장착하기</h3>

1. 생성한 볼륨 위에서 [우클릭] > [볼륨 연결]   
![image](https://user-images.githubusercontent.com/43658658/145526357-7a55964a-fa70-48a1-840c-0b463e6ba576.png)   

2. 인스턴스의 ID를 확인하고 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/145526485-a3d7f9c0-cb63-4be3-9ef9-bde9addb9129.png)   
![image](https://user-images.githubusercontent.com/43658658/145526535-fc3c7724-fae6-470c-a245-0375c5c6a580.png)

3. `볼륨 연결`을 클릭합니다.   

4. 인스턴스의 [스토리지] 탭을 보면 2개의 볼륨이 있는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145526721-2faa5bfe-21da-456c-96ce-bdc6418e7dde.png)

> <h3>EC2 인스턴스에서 새로 생성한 EBS 볼륨 포맷하기</h3>

새로 생성한 `EBS 볼륨`를 OS에서 사용하려면 알맞은 파일시스템으로 포맷을 해주어야 합니다.   
이전에 생성한 EC2 인스턴스가 `Linux`이므로 `Ext4 파일시스템`을 사용하겠습니다.

1. EC2 인스턴스에 장착된 EBS 볼륨의 `장치명`을 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/145527266-f46f6876-c1ae-42d3-ba87-3be739db50f5.png)

2. SSH로 EC2 인스턴스에 접속한 뒤 아래와 같이 `sudo mkfs -t ext4 /dev/sdf`를 입력하여 `EBS 볼륨을 포맷`합니다.   
![image](https://user-images.githubusercontent.com/43658658/145527132-44bb0afc-572d-4f1d-98b1-de7074dd797b.png)

> <h3>EBS 볼륨 마운트하기</h3>

포맷까지 완료했으므로 EC2 인스턴스에 마운트만 하면 해당 볼륨을 사용할 수 있습니다.

1. `ls /dev/sdf -al` 명령을 입력하여 /dev/sdf 장치가 있는지 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/145527829-92ee80c3-52ce-4d92-93c0-dfe490295c85.png)   
* /dev/sdf가 /dev/xvdf에 심볼릭 링크로 연결되어 있습니다.

2. `sudo mount /dev/sdf /mnt`를 입력하여 저장 장치를 마운트합니다.   
![image](https://user-images.githubusercontent.com/43658658/145527883-c16fca6b-ad85-435e-b591-ca1e99df9d16.png)   
* /dev/sdf를 /mnt에 마운트(/mnt말고 다른 디렉토리를 지정해줘도 상관 없습니다).
* /dev/sdf 대신 /dev/xvdf를 써도 무관합니다(심볼릭 링크).

3. `df -h`를 통해 디스크를 확인해보면 /mnt에 /dev/xvdf와 /dev/sdf가 마운트된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145528427-a8cca0fb-ce89-4e2d-ab47-a13357902ea9.png)

이제 이곳에 파일을 저장할 수 있습니다.

> <h3>EBS 볼륨 제거하기</h3>

EC2 인스턴스에서 사용하지 않는 EBS 볼륨을 제거해보겠습니다. 

1. `sudo umount /mnt` 명령을 입력하여 장치를 언마운트 합니다.   
![image](https://user-images.githubusercontent.com/43658658/145528967-0e02ddef-f432-4042-a0c5-8f4ea3891b61.png)   
* `df -h`를 통해 보면 `/mnt`가 사라졌음을 볼 수 있습니다.

2. 사용하지 않는 볼륨을 우클릭하고 `[볼륨 분리]`를 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/145529168-10708fa3-829a-4477-b88d-ae09e066b2a9.png)   
![image](https://user-images.githubusercontent.com/43658658/145529256-be06d663-cf44-4c2f-a5cd-1cd915ec35e6.png)

3. 볼륨 상태가 `사용 가능`으로 변경되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/145529333-aeebf710-1716-44f8-805e-1ac483451252.png)

이제 볼륨이 인스턴스로부터 분리되었습니다.   
앞으로 볼륨을 사용할 계획이 없다면 `볼륨 삭제`를 통해 완전히 삭제할 수 있습니다.

## EBS 스냅샷

스냅샷 : EBS 볼륨의 전체 내용 중 특정 시점을 파일로 저장한 형태입니다. 다양한 용도로 사용이 가능합니다.   
* EBS 볼륨 생성(다른 가용 영역에 생성 가능)
* AMI 생성
* 스냅샷을 다른 리전으로 복사

> <h3>EBS 스냅샷 생성하기</h3>

1. [볼륨] > [볼륨 선택] > [우클릭] > [스냅샷 생성]   
![image](https://user-images.githubusercontent.com/43658658/145529927-aa349d9b-55ff-40df-8eae-56fba567457b.png)

2. 그대로 스냅샷을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/145530011-6e1e8e98-5afe-49a8-ac9f-df38c03d3a12.png)

3. [스냅샷] 탭으로 넘어가보면 스냅샷이 생성되고 있는 것을 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145530125-baeadc09-9fd8-43ea-97a1-30595ca72712.png)

4. 스냅샷 생성이 완료되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/145530315-bf37a947-019e-4bb7-a212-7ed80686abf5.png)

> <h3>스냅샷으로 볼륨 생성하기</h3>

1. 생성한 스냅샷 위에서 [우클릭] > [스냅샷에서 볼륨 생성]   
![image](https://user-images.githubusercontent.com/43658658/145530486-45d6f224-4c37-4d97-bc2d-2480812ba62d.png)

2. 볼륨을 생성할 때와 비슷한 설정 창이 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/145530810-c752b962-a64c-48ca-a840-48e2056c5d0b.png)   
* 여기서 가용 영역을 다른 곳으로 선택하여 볼륨을 다른 가용 영역으로 옮길 수 있습니다.

3. [볼륨 생성] 버튼을 누르면 볼륨이 생성됩니다.   
![image](https://user-images.githubusercontent.com/43658658/145530957-32e526be-3223-49fb-8099-d6c494e04ceb.png)

> <h3>스냅샷으로 이미지 생성하기</h3>

1. 생성한 스냅샷 위에서 [우클릭] > [스냅샷에서 이미지 생성]   
![image](https://user-images.githubusercontent.com/43658658/145531115-96ed345e-110d-486e-a1b0-a3b3d25f8896.png)

2. AMI 설정을 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/145531494-5bef63ca-22de-45ab-a8de-76088a0c21cd.png)   
* `가상화 유형` : `HVM`과 `PV` 중 선택합니다.   
* `부팅 모드` : `BIOS`와 `UEFI` 중 선택합니다.

3. 기본으로 장착될 볼륨에 대한 설정입니다.   
![image](https://user-images.githubusercontent.com/43658658/145531642-2cba276a-bf95-4d8f-84f1-0130139ea9f4.png)

4. `[이미지 생성]` 버튼을 누르면 이미지가 생성됩니다.   

5. [AMI] 탭으로 가면 스냅샷으로 생성한 이미지가 생성되었음을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145531830-c0ae8fee-ea15-4614-9aec-8af6bb7ec89d.png)

이 AMI 파일을 통해 EC2 인스턴스를 생성할 수 있습니다.

> <h3>스냅샷을 다른 리전으로 복사하기</h3>

볼륨은 `다른 가용 영역과 리전`으로 이전이 `불가능`합니다.   
이를 해결하기 위해서는 `볼륨을 스냅샷으로 생성`한 뒤 `다른 리전으로 복사`해야 합니다.

1. 생성한 스냅샷 위에서 [우클릭] > [스냅샷 복사]   
![image](https://user-images.githubusercontent.com/43658658/145532626-aa80d166-b6c1-4af8-a565-d2b64ccff6d1.png)

2. 원하는 리전을 선택하고 [스냅샷 복사]를 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/145532791-067e3f6c-6b3d-4ede-9100-e3c7d6f7c828.png)

3. 우측 상단에 리전을 방금 선택한 리전으로 바꾸고 [스냅샷] 탭으로 들어가면 복사된 스냅샷이 생성된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/145533025-79eb63b2-4bfd-4f7a-a8c3-995ba53b259b.png)

이제 이 스냅샷으로 오하이오 리전에서 볼륨이나 AMI를 생성하여 사용하면 됩니다.

## 인스턴스 스토리지를 Root 장치로 사용하는 EC2 인스턴스 생성하기

일반적으로 인스턴스를 생성하면 항상 EBS를 Root 장치로 사용합니다.

인스턴스 스토리지를 Root 장치로 사용하는 인스턴스를 생성할 수도 있습니다.

1. [인스턴스 시작] > [1단계] > [커뮤니티 AMI]   
![image](https://user-images.githubusercontent.com/43658658/145541399-85e3b646-fb41-4279-8a7f-98d3dafe4479.png)

2. 아래쪽에 [루트 디바이스] > [인스턴스 스토어]   
![image](https://user-images.githubusercontent.com/43658658/145541480-514a365f-9139-4543-a288-dfe434b0c7c3.png)

3. 가장 최신버전을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/145541586-73d675f1-fe15-4a1c-ac82-295af5d18aaf.png)

4. [4단계 : 스토리지 추가] 탭으로 가면 기본적으로 장착된 루트 장치가 없습니다.   
![image](https://user-images.githubusercontent.com/43658658/145541703-1a2ed6be-2dc1-445e-8954-a9bab6215442.png)

나머지 단계는 모두 앞서 실습한 인스턴스 시작 과정과 같습니다.

인스턴스 스토리지를 루트 장치로 사용하는 인스턴스는 `중지` 기능이 없습니다. `재부팅`이나 `종료`만 할 수 있습니다.




