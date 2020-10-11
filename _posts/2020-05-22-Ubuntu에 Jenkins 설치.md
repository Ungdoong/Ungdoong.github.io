# Ubuntu에 Jenkins 설치



### 자바가 필요하므로 자바 설치

```shell
$ sudo apt-get install openjdk-8-jdk
```

### Jenkins 설치를 위한 repository key를 추가

```shell
$ wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
```

추가가 되면 `OK` 출력

### repository를 추가

```shell
$ echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
```

### apt-get 업데이트 후 jenkins 설치

```shell
$ sudo apt-get update
$ sudo apt-get install jenkins
```

### 설치가 완료되면 Jenkins를 실행하기 전에 포트를 변경

 포트변경은 /etc/default/jenkins에서

```shell
$ sudo vim /etc/default/jenkins
```

### jenkins 실행

```shell
$ sudo systemctl start jenkins
```

### status로 상태 확인

```shell
$ sudo systemctl status jenkins
● jenkins.service - LSB: Start Jenkins at boot time
   Loaded: loaded (/etc/init.d/jenkins; bad; vendor preset: enabled)
   Active: active (exited) since Wed 2017-12-27 23:23:36 KST; 16min ago
     Docs: man:systemd-sysv-generator(8)
```

### 브라우저를 열고 http://<서버 IP>:8080을 확인

![](https://user-images.githubusercontent.com/41600558/82640926-6edf6780-9c46-11ea-8cd0-9cf11670907c.png)

### Administrator password에 제시된 경로에서 확인된 패스워드 입력

