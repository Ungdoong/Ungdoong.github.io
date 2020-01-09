# 190710(Wed)\_Day3_Chapter4_슬랙앱시작하기

### 1. 슬랙 앱 생성하기

#### 슬랙 워크 스페이스 가입

#### 슬랙 앱 생성하기 링크로 간다

![1562740256747](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562740256747.png)

![1562740883627](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562740883627.png)

![1562741255922](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562741255922.png)



#### 챗봇의 이름을 설정 후 워크스페이스 선택하여 생성

![1562741275971](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562741275971.png)

#### 슬랙 앱 생성 후, 슬랙 앱 설정

- Add features and functionality
  - 슬랙 앱 기본 기능 메뉴
  - 챗봇 등 다양한 형태의 슬랙 앱을 만들 수 있다.
- Install your app to your workspace
  - 슬랙 앱을 워크스페이스에 설치
- Manage distribution
  - 슬랙 앱 배포 설정을 할수 있음



#### 슬랙 봇 추가

![1562741590470](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562741590470.png)

- Always Show My Bot as Online 활성화 하고 봇 추가



#### 슬랙 앱 권한 설정하기

![1562741697081](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562741697081.png)



![1562741755706](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562741755706.png)

![1562741790881](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562741790881.png)

#### 슬랙 앱 Install 하기

![1562741916150](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562741916150.png)

![1562741933126](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562741933126.png)

![1562741950238](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562741950238.png)

### 슬랙 설정하기

![1562742325120](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562742325120.png)

![1562742352601](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562742352601.png)

#### app_mention을 추가하고 봇을 멘셔닝하면 멘션에 대답함

![1562743715490](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562743715490.png)



## 실습

### 슬랙 챗봇 첫 메세지

- pycharm과 ngrok를 이용하여 로컬에서 실행

```python
# -*- coding: utf-8 -*-

from flask import Flask
from slack import WebClient
from slackeventsapi import SlackEventAdapter


# OAuth & Permissions로 들어가서
# Bot User OAuth Access Token을 복사하여 문자열로 붙여넣습니다
SLACK_TOKEN = 'xoxb-688411139621-678345631491-lbwE2WBQRPTczuBqaLbQ4zhK'
# Basic Information으로 들어가서
# Signing Secret 옆의 Show를 클릭한 다음, 복사하여 문자열로 붙여넣습니다
SLACK_SIGNING_SECRET = 'ed9bcc54ef15b26a9d129c6b23a451f7'


app = Flask(__name__)
# /listening 으로 슬랙 이벤트를 받습니다.
slack_events_adaptor = SlackEventAdapter(SLACK_SIGNING_SECRET, "/listening", app)
slack_web_client = WebClient(token=SLACK_TOKEN)


# 챗봇이 멘션을 받았을 경우
@slack_events_adaptor.on("app_mention")
def app_mentioned(event_data):
    # 슬랙 챗봇이 대답합니다.
    slack_web_client.chat_postMessage(
        channel=event_data["event"]["channel"],
        text="Hello, I am your chatbot!"
    )


# / 로 접속하면 서버가 준비되었다고 알려줍니다.
@app.route("/", methods=["GET"])
def index():
    return "<h1>Server is ready.</h1>"


if __name__ == '__main__':
    app.run(127.0.0.1', port=8080)
```

- 코드 입력 후 Run

- 파이참 내 터미널에서 ngrok.exe가 존재하는 폴더로 이동

- C드라이브폴더내에 ngrok가 존재할 경우

  ```
  C:> ngrok http 8080 #입력
  ```

  

![1562743005177](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562743005177.png)

- Web Interface 부분의 url을 브라우저에서 입력하면

![1562743087980](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562743087980.png)

- Forwading 부분의 http://1c8c33e3.ngrok.io를 브라우저에 입력시

![1562743150399](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562743150399.png)

- Request URL 에 추가

![1562743229484](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562743229484.png)