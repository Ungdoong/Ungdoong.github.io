# 190711(Thu)\_Day3_Mission

### 1. Bugs 차트 Top10 곡 제목

```python
# -*- coding: utf-8 -*-
import re
import urllib.request

from bs4 import BeautifulSoup

from flask import Flask
from slack import WebClient
from slackeventsapi import SlackEventAdapter

SLACK_TOKEN = 'xoxb-688411139621-678345631491-Zu9tWUbq79hqAmxha044CLwN'
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
    elif 'bugs' in text or '벅스' in text:
        url = 'https://music.bugs.co.kr/'
        tag_music = "p"
        class_music = "title"
        tag_artist = "p"
        class_artist = "artist"
    else:
        url = 'http://www.daum.net'
        tag_name = "a"
        class_name = "link_issue"

    source_code = urllib.request.urlopen(url).read()
    soup = BeautifulSoup(source_code, "html.parser")

    # 여기에 함수를 구현해봅시다.
    keywords = []
    if not 'bugs' in text and not '벅스' in text:
        for tag in soup.find_all(tag_name, class_=class_name):
            word = tag.get_text().strip()
            if not word in keywords:
                keywords.append(word)
    else:
        soup = soup.find("table", class_="list trackList")
        musics = soup.find_all(tag_music, class_= class_music)
        artists = soup.find_all(tag_artist, class_=class_artist)
        for i in range(10):
            if i == 0:
                keywords.append("Bugs 실시간 음악 차트 Top 10\n")
            artist = artists[i].get_text().strip()
            if '\n' in artist:
                artist = artist.split('\n')[3]
            keywords.append(str(i+1) + '위 : ' + musics[i].get_text().strip() + ' / ' + artist)


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

@slack_events_adaptor.on("message")
def message(event_data):
    channel = event_data["event"]["channel"]
    text = event_data["event"]["text"]



# / 로 접속하면 서버가 준비되었다고 알려줍니다.
@app.route("/", methods=["GET"])
def index():
    return "<h1>Server is ready.</h1>"


if __name__ == '__main__':
    app.run('0.0.0.0', port=8080)
```

