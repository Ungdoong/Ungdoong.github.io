# 190613(Thu)_DockerVolume

## Volume

- 컨테이너 밖에 데이터 유지

### 볼륨을 사용한 mysql container(바인딩 방식)

- 로컬의 /var/mysql_dir로 컨테이너 안의 /var/lib/mysql을 바인딩

```
#docker run -d --name mysqldb -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=testdb -v /var/mysql_dir:/var/lib/mysql mysql
```

- 볼륨 데이터 확인(mysqldb 컨테이너)

  ```
  #sudo docker exec -it mysqldb /bin/bash
  #cd /var/lib/mysql
  #touch test1234
  ```

##### 볼륨 데이터 공유

```
#docker run exec -it --name ub -v /var/mysql_dir:/var/mysql_dir ubuntu:16.04
```

### 도커 볼륨 (도커볼륨방식)

- 도커가 관리하는 디렉토리에 볼륨을 생성해서 관리

##### 도커 볼륨 생성

```
#docker volume create myvolume01
```

##### 도커 볼륨 리스트

```
#docker volume ls
#docker volume list
```

##### 도커 볼륨 사용

- myvolume01은 컨테이너 내에 위치

```
#docker run -it -v myvolume01:/var/myvolume01 ubuntu:16.04 /bin/bash
```

##### 호스트에서 볼륨 디렉토리 위치 확인

```
#docker inspect myvolume01 
[
    {
        "CreatedAt": "2019-06-12T03:03:15-07:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/myvolume01/_data",
        "Name": "myvolume01",
        "Options": {},
        "Scope": "local"
    }
]
```

##### 호스트에서 볼륨 데이터 확인

```
#ls /var/lib/docker/volumes/myvolume01/_data
```

##### 다른 컨테이너에서 볼륨 데이터 접근

```
<Container 1> - myvolume03 생성
#docker run -it -v myvolume03:/var/myvolume ubuntu:16.04 /bin/bash
<Container 2> - myvolume03 연결
#docker run -it -v myvolum03:/var/myvolume ubuntu:16.04 /bin/bash
```

##### 안쓰는 볼륨 제거

```
#docker volume prune
```

