# 190529(WED)_VMware RAID 구성

### 하드디스크 및 파티션 추가

##### Virtual Machine Setting 에서 디스크 추가

##### 부팅 후 장치 확인

```
#ls -l /dev/sd*
```

##### 파티션 생성

```
#fdisk /dev/sdb
Command : n
Select : p
Partition number : 1
First Sector : Enter
Last Sector : Enter
Command : t
Hex Code : fd
Command : w
```

##### mdadm 설치

```
#apt-get -y install mdadm
```

### RAID 구축

##### RAID 생성

```
#fdisk -l /dev/sdb ; fdist -l /dev/sdc (상태확인)
#mdadm --create /dev/md9 --level=linear --raid-devices=2 /dev/sdb1 /dev/sdc1
#mkfs.ext4 /dev/md9
#mkdir /raidLinear
#mount /dev/md9 /raidLinear
#gedit /etc/fstab
 추가) /dev/md9 /raidLinear ext4 defaults 0 0
```

##### mdadm의 버그에 대한 설정

```
#mdadm --detail --scan >> /etc/mdadm/mdadm.conf
 (name 삭제)
#update-initramfs -u
```

### RAID 복구

##### 고장난 raid가 linear or raid0 일 경우 

→ /etc/fstab 의 해당부분 주석처리

```
#mdadm --stop /dev/md9(RAIDLinear)
#mdadm --stop /dev/md0(RAID0)
```



##### md0 / md9

```
#mdadm --stop /dev/md9
#mdadm --create /dev/md9 --level=linear --raid-devices=2 /dev/sdb1 /dev/sdc1
```

##### md1 / md5

```
#mdadm /dev/md1 --add /dev/sdg1
```

