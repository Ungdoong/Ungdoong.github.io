# 190710(Wed)\_Day3\_Chapter5\_크롤링챗봇만들기

## 이론

### 새로운 기능이 추가된 챗봇을 사용하려면

- Request URL을 변경해 주어야 한다

### 다음/네이버 인기검색어 크롤링

- ```python
  keywords = []
  	if "naver" in url :         
     		for data in (soup.find_all("span", class_="ah_k")):
               #10위까지만 크롤링하겠습니다.
              if not data.get_text() in keywords :
                  if len(keywords) >= 10 : 
                      break                 
                  keywords.append(data.get_text())
  	elif "daum" in url :    
          for data in soup.find_all("a", class_="link_issue"):
  			if not data.get_text() in keywords :
                  keywords.append(data.get_text()) 
  ```

  



## 실습

### 1. 슬랙 네이버 인기검색어 챗봇

```python
# -*- coding: utf-8 -*-
import re
import urllib.request

from bs4 import BeautifulSoup

from flask import Flask
from slack import WebClient
from slackeventsapi import SlackEventAdapter


SLACK_TOKEN = 'xoxb-688411139621-678345631491-lbwE2WBQRPTczuBqaLbQ4zhK'
SLACK_SIGNING_SECRET = 'ed9bcc54ef15b26a9d129c6b23a451f7'


app = Flask(__name__)
# /listening 으로 슬랙 이벤트를 받습니다.
slack_events_adaptor = SlackEventAdapter(SLACK_SIGNING_SECRET, "/listening", app)
slack_web_client = WebClient(token=SLACK_TOKEN)


# 크롤링 함수 구현하기
def _crawl_naver_keywords(text):
    # 여기에 함수를 구현해봅시다.
    url = 'http://www.naver.com'
    source_code = urllib.request.urlopen(url).read()
    soup = BeautifulSoup(source_code, "html.parser")
    
    keywords = []
    for span_tag in soup.find_all("span", class_= "ah_k"):
        word = span_tag.get_text().strip()
        keywords.append(word)
    

    # 키워드 리스트를 문자열로 만듭니다.
    return '\n'.join(keywords)


# 챗봇이 멘션을 받았을 경우
@slack_events_adaptor.on("app_mention")
def app_mentioned(event_data):
    channel = event_data['event']['channel']
    text = event_data['event']['text']

    keywords = _crawl_naver_keywords(text)
    slack_web_client.chat_postMessage(
        channel=channel,
        text=keywords
    )


# / 로 접속하면 서버가 준비되었다고 알려줍니다.
@app.route("/", methods=["GET"])
def index():
    return "<h1>Server is ready.</h1>"


if __name__ == '__main__':
    app.run('0.0.0.0', port=8080)
```



### 2. 여러 포털 사이트의 인기 검색어 (다음)

```python
# -*- coding: utf-8 -*-
import re
import urllib.request

from bs4 import BeautifulSoup

from flask import Flask
from slack import WebClient
from slackeventsapi import SlackEventAdapter


SLACK_TOKEN = 'xoxb-688411139621-678345631491-lbwE2WBQRPTczuBqaLbQ4zhK'
SLACK_SIGNING_SECRET = 'ed9bcc54ef15b26a9d129c6b23a451f7'


app = Flask(__name__)
# /listening 으로 슬랙 이벤트를 받습니다.
slack_events_adaptor = SlackEventAdapter(SLACK_SIGNING_SECRET, "/listening", app)
slack_web_client = WebClient(token=SLACK_TOKEN)


# 크롤링 함수 구현하기
def _crawl_portal_keywords(text):
    # url_match = re.search(r'<(http.*?)(\|.*?)?>', text)
    # if not url_match:
    #     return '올바른 URL을 입력해주세요.'

    # url = url_match.group(1)
    # source_code = urllib.request.urlopen(url).read()
    # soup = BeautifulSoup(source_code, "html.parser")
    
    if 'naver' in text or '네이버' in text:
        url = 'http://www.naver.com'
        tag_name = "span"
        class_name = "ah_k"
    else:
        url = 'http://www.daum.net'
        tag_name = "a"
        class_name = "link_issue"
        
    source_code = urllib.request.urlopen(url).read()
    soup = BeautifulSoup(source_code, "html.parser")

    # 여기에 함수를 구현해봅시다.
    keywords = []
    for tag in soup.find_all(tag_name, class_=class_name):
        word = tag.get_text().strip()
        if not word in keywords:
            keywords.append(word)
        
    # 키워드 리스트를 문자열로 만듭니다.
    return '\n'.join(keywords)


# 챗봇이 멘션을 받았을 경우
@slack_events_adaptor.on("app_mention")
def app_mentioned(event_data):
    channel = event_data["event"]["channel"]
    text = event_data["event"]["text"]

    keywords = _crawl_portal_keywords(text)
    slack_web_client.chat_postMessage(
        channel=channel,
        text=keywords
    )


# / 로 접속하면 서버가 준비되었다고 알려줍니다.
@app.route("/", methods=["GET"])
def index():
    return "<h1>Server is ready.</h1>"


if __name__ == '__main__':
    app.run('0.0.0.0', port=8080)
```

