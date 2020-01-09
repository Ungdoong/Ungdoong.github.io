# 190617(Mon)_Docker&Server

### 컨테이너 telnet설치

```
#docker run -it ubuntu:16.04
컨테이너#apt-get update
컨테이너#apt-get install net-tools
컨테이너#dpkg -l xinetd telnetd
컨테이너#cd /etc/xinetd.d
컨테이너#touch telnet
컨테이너#vim telnet
```

```
service telnet
{
	disable = no
	flags = REUSE
	socket_type = stream
	wait = no
	user = root
	server = /usr/sbin/in.telnetd
	log_on_failure += USERID
}
```

```
컨테이너#service xinetd restart
```



### 로컬(Ubuntu(Server))에서 컨테이너 telnet 접속

```
컨테이너#adduser teluser
#telnet 172.17.0.2<컨테이너IP>
```

### SSH 접속

```
컨테이너#apt-get install -y openssh-server
컨테이너#service ssh restart
#ssh teluser@172.17.0.2
```

### VNC 서버

##### 새로운 컨테이너 생성 및 초기환경 세팅

```
#docker run -it ubuntu:16.04
컨테이너#apt-get update
컨테이너#apt-get -y install gnome-panel gnome-settings-daemon metacity vnc4server
```

##### VNC 설정

```
컨테이너#vncserver
컨테이너#vncserver -kill :1
컨테이너#vim /root/.vnc/xstartup
<<Append>>
gnome-panel &
gnome-settings-daemon &
metacity &
nautilus &
컨테이너#vncserver :1
(필요할 경우)#ufw allow 5901/tcp
```

### 클라이언트에서 접속

##### VNC 클라이언트 프로그램 설치

```
#apt-get -y install xtightvncviewer
#vncviewer <서버IP>:디스플레이번호
```

### Teamviewer

##### Ubuntu

```
#wget https://download.teamviewer.com/download/linux/teamviewer_amd64.deb
#apt install gdebi-core
#gdebi teamviewer_amd64.deb
```

