# 190711(Thu)\_Day4_Chapter1_슬랙챗봇로컬환경구성

## requirements.txt

- 자신의 프로젝트 폴더 안에  requirements.txt 파일 생성

```
aiohttp==3.5.4
async-timeout==3.0.1
attrs==19.1.0
beautifulsoup4==4.7.1
bs4==0.0.1
certifi==2019.6.16
chardet==3.0.4
Click==7.0
Flask==1.0.3
idna==2.8
itsdangerous==1.1.0
Jinja2==2.10.1
MarkupSafe==1.1.1
multidict==4.5.2
pyee==6.0.0
requests==2.22.0
six==1.12.0
slackclient==2.1.0
slackeventsapi==2.1.0
soupsieve==1.9.2
urllib3==1.25.3
Werkzeug==0.15.4
yarl==1.3.0
```

- 터미널에서 해당 폴더로 이동

- ```
  >pip install -r requirements.txt
  ```

  

## 슬랙 챗봇 API 사용 가이드

### local to remote

- chat_postMessage()

  ```python
  slack_web_client.chat_postMessage(
  	channel=channel,
  	text=keywords
  )
  ```

- JSON data

  ![1562812536976](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562812536976.png)

- URL verification handshaking

  ![1562861557839](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562861557839.png)