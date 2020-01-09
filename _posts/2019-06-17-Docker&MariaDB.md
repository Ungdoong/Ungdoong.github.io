# 190617(Mon)_Docker&MariaDB

### MariaDB 구축

##### docker ubuntu 실행

```
#docker run -it ubuntu:16.04
```

##### repository 추가

```
# apt -y install software-properties-common dirmngr
# apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
# add-apt-repository 'deb [arch=amd64] http://mirror.zol.co.zw/mariadb/repo/10.3/ubuntu xenial main'
# apt update
```

##### MariaDB server, client 설치

```
# apt -y install mariadb-server mariadb-client
```

##### Docker Hub 를 통한 Mariadb 설치

```
#docker run --name my-mariadb -e MYSQL_ROOT_PASSWORD=password -d mariadb:10-bionic
```

