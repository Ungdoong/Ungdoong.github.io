# 190529(WED)_DISK CONTROL

### SCSI

- 장착된 물리적 하드디스크 - /dev/sda, sdb, sdc ...
- sda 하드디스크에 대한 파티션 - /dev/sda1, sda2, sda3 ...

### 파티션 분할 후 마운팅

##### 파티션 분할 명령

```
$fdisk
```

##### 파일시스템 설정

```
$mkfs
```

##### 디스크 마운트

```
$mount
```

##### 마운트 저장

```
$gedit /etc/fstab
/dev/sdb1 /mydata ext4 defaults 0 0 아래에 추가
```

- /etc/fstab - 리눅스가 부팅시 자동으로 읽는 중요 파일

  ​                     마운트정보가 수록되어 있으며 글자가 틀리면 부팅X될 수 있음

  → 장치이름 / 마운트될 디렉터리 / 파일시스템 / 속성 / dump사용여부

### RAID

여러 개의 디스크를 하나의 디스크처럼 사용하는 방식

비용 절감 + 신뢰성 향상 + 성능 향상

- 하드웨어 RAID

- 소프트웨어 RAID - 운영체제에서 지원

  ```
  $mdadm (RAID 구성)
  ```

  

### RAID 방식 비교

![1559098144643](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1559098144643.png)

##### Linear RAID

- 100% 공간효율성

##### RAID0

- 모든 디스크 동시저장(왔다갔다 저장)
- 100% 공간효율성
- 빠른성능을 요구하되, 손실의 문제가 없는 자료

##### RAID1 = Mirroring

- 같은 정보를 두 군데에서 기록
- 결함 허용(Fault-tolerance) 제공 = 신뢰성 높음
- 중요한 데이터 저장에 적합
- 공간효율 나쁨

##### RAID5

- 3개 이상
- RAID1의 데이터 안정성 + RAID0의 공간 효율성
- 디스크 중 하나를 패리티비트 저장용으로 사용
- 디스크가 2개이상 결함이면 복원 힘듬

##### RAID6

- 패리티비트를 두 군데에 저장