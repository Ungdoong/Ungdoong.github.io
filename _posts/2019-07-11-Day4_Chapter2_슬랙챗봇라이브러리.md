# 190711(Thu)\_Day4_Chapter2_슬랙챗봇라이브러리

## 이론

### 글씨 꾸미기

![1562862484396](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562862484396.png)

### 링크 만들기

![1562862532631](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562862532631.png)

### 다른 사람을 멘션하기

- 슬랙 앱이 멘션을 하려면 '유저의 ID 코드'를 알아야 한다

![1562862580886](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562862580886.png)

### ID 코드 구하기

![1562862609097](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562862609097.png)

### 섹션 블록

![1562862705543](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562862705543.png)

### 이미지 블록

![1562862761395](C:\Users\student\AppData\Roaming\Typora\typora-user-images\1562862761395.png)

### 블록 메시지 보내기

```python
my_blocks = [block1, block2, block3, ...]

slack_web_client.chat_postMessage(
	channel=channel,
	blocks=extract_json(my_blocks)
)
```



## 실습

### 1. 메시지 꾸미기

```python
# 글씨 모양 꾸미기
slack_web_client.chat_postMessage(
    channel="#채널명",
    text="*진한 글씨* _비스듬한 글씨_ ~취소선~ `코드`"
)

# 링크 만들기
slack_web_client.chat_postMessage(
    channel="#채널명",
    text="<https://ssafy.elice.io|엘리스>는 정말 최고야!"
)

# 특정 유저 멘션하기
# 유저 ID를 구하는 방법은 실습 3을 참고하세요.
slack_web_client.chat_postMessage(
    channel="#채널명",
    text="<@U12345678> 님이 나를 보셨어!\n날 발할라로 데려가실 거야!"
)
```

### 2. 블록으로 메시지 만들기

```python
# 블록 클래스를 사용하는 데 필요한 import 문입니다.
from slack.web.classes import extract_json
from slack.web.classes.blocks import *


# 섹션 블록: 한 덩어리의 텍스트를 표시합니다..
block1 = SectionBlock(
    text="텍스트"
)

# 섹션 블록: 짧은 문구 여러 개를 2줄로 표시합니다 (최대 10개).
block2 = SectionBlock(
    fields=["text1", "text2", ...]
)

# 이미지 블록: 큰 이미지 하나를 표시합니다..
block3 = ImageBlock(
    image_url="이미지의 URL",
    alt_text="이미지가 안 보일 때 대신 표시할 텍스트"
)

# 여러 개의 블록을 하나의 메시지로 묶어 보냅니다.
my_blocks = [block1, block2, block3]
slack_web_client.chat_postMessage(
    channel="#채널명",
    blocks=extract_json(my_blocks)
)
```

### 3. Slack Web API 활용하기

```python
# 특정 채널에 메시지 보내기
# 참고: https://api.slack.com/methods/chat.postMessage
slack_web_client.chat_postMessage(channel="#채널명", text="Hello, world!")

# 다른 사람이 메시지를 올렸을 때, 그 사람에게만 답장하기
# 참고: https://api.slack.com/methods/chat.postEphemeral
slack_web_client.chat_postEphemeral(
    channel=event_data["event"]["channel"],
    user=event_data["event"]["user"],
    text="조용히 하세요!"
)

# 유저를 이름으로 검색하여 ID 코드를 구하기
# 참고: https://api.slack.com/methods/users.list
response = slack_web_client.users_list()
for user in response["members"]:
    if user.name == "홍길동":
        user_id = user.id
        break
        
# 특정한 유저 한 사람에게만 보이는 메시지를 보내기
# 유저 ID 코드를 구하는 법은 위의 코드를 참고하세요.
# 참고: https://api.slack.com/methods/chat.postEphemeral
slack_web_client.chat_postEphemeral(
    channel="#채널명", user=user_id, text="뭐하고 있니?"
)

# 특정 채널에 파일 업로드하기
# 참고: https://api.slack.com/methods/files.upload
file_name = "hello.txt"
result = slack_web_client.files_upload(channels="#채널명", file=file_name)
if not result["ok"]:
    print(file_name + " 업로드에 실패했습니다.")
    print("원인: " + result["error"])

# 알람 메시지를 설정하기
# 시간을 입력하는 형식은 아래 링크를 참고하세요:
# https://get.slack.help/hc/en-us/articles/208423427#format-a-reminder
# 참고: https://api.slack.com/methods/reminders.add
slack_web_client.reminders_add(text="오늘 수업 끝!", time="18:00")

# 다른 사람이 올린 메시지에 이모티콘 달기
# 참고: https://api.slack.com/methods/reactions.add
slack_web_client.reactions_add(
  name="thumbsup",
  channel=event_data["event"]["channel"],
  timestamp=event_data["event"]["event_ts"]
)
```

### 4. 가격비교 봇

```python
# -*- coding: utf-8 -*-
import json
import re
import requests
import urllib.request
import urllib.parse

from bs4 import BeautifulSoup
from flask import Flask, request
from slack import WebClient
from slack.web.classes import extract_json
from slack.web.classes.blocks import *
from slack.web.classes.elements import *
from slack.web.classes.interactions import MessageInteractiveEvent
from slackeventsapi import SlackEventAdapter


SLACK_TOKEN = ?
SLACK_SIGNING_SECRET = ?


app = Flask(__name__)
# /listening 으로 슬랙 이벤트를 받습니다.
slack_events_adaptor = SlackEventAdapter(SLACK_SIGNING_SECRET, "/listening", app)
slack_web_client = WebClient(token=SLACK_TOKEN)


# 키워드로 중고거래 사이트를 크롤링하여 가격대가 비슷한 상품을 찾은 다음,
# 메시지 블록을 만들어주는 함수입니다
def make_sale_message_blocks(keyword, price):
    # 주어진 키워드로 중고거래 사이트를 크롤링합니다
    query_text = urllib.parse.quote_plus(keyword, encoding="unicode-escape")
    query_text = query_text.replace("%5C", "%")
    search_url = "http://corners.auction.co.kr/corner/UsedMarketList.aspx?keyword=" + query_text
    print('Searching : ' + search_url)
    source_code = urllib.request.urlopen(search_url).read()
    soup = BeautifulSoup(source_code, "html.parser")

    # 페이지에서 각 매물의 정보를 추출합니다.
    items = []
    for item_div in ?:

        items.append({
            "title": title,
            "link": link,
            "image": image,
            "price": item_price,
            "seller": seller
        })

    # 각 매물을 원하는 가격에 가까운 순서대로 정렬합니다.
    items.sort(key=?)

    # 메시지를 꾸밉니다
    # 처음 섹션에는 제목과 첫 번째 상품의 사진을 넣습니다
    first_item_image = ImageElement(
        image_url=items[0]["image"],
        alt_text=keyword
    )
    head_section = SectionBlock(
        text="*\"" + keyword + "\", " + str(price) + "원으로 검색한 결과입니다.*",
        accessory=first_item_image
    )

    # 두 번째 섹션에는 처음 10개의 상품을 제목 링크/내용으로 넣습니다
    item_fields = []
    for item in items[:10]:
        # 첫 줄은 제목 링크, 두 번째 줄은 게시일과 가격을 표시합니다.
        text = "*<" + item["link"] + "|" + item["title"] + ">*"
        text += "\n" + str(item["price"]) + "원 / " + item["seller"]
        item_fields.append(text)
    link_section = SectionBlock(fields=item_fields)

    # 마지막 섹션에는 가격대를 바꾸는 버튼을 추가합니다
    # 여러 개의 버튼을 넣을 땐 ActionsBlock을 사용합니다 (버튼 5개까지 가능)
    button_actions = ActionsBlock(
        block_id=keyword,
        elements=[
            ButtonElement(
                text="1만원 올리기",
                action_id="price_up_1", value=str(price + 10000)
            ),
            ButtonElement(
                text="5만원 올리기", style="danger",
                action_id="price_up_5", value=str(price + 50000)
            ),
            ButtonElement(
                text="1만원 낮추기",
                action_id="price_down_1", value=str(price - 10000)
            ),
            ButtonElement(
                text="5만원 낮추기", style="primary",
                action_id="price_down_5", value=str(price - 50000)
            ),
        ]
    )

    # 각 섹션을 list로 묶어 전달합니다
    return [head_section, link_section, button_actions]


# 챗봇이 멘션을 받으면 중고거래 사이트를 검색합니다
@slack_events_adaptor.on("app_mention")
def app_mentioned(event_data):
    channel = event_data["event"]["channel"]
    text = event_data["event"]["text"]

    # 입력한 텍스트에서 검색 키워드와 가격대를 뽑아냅니다.
    matches = re.search(r"<@U\w+>\s+(.+)\s+(\d+)원", text)
    if not matches:
        # 유저에게 사용법을 알려줍니다
        print(text)
        slack_web_client.chat_postMessage(
            channel=channel,
            text="사용하려면 `@중고거래봇 <키워드> 10000원`과 같이 멘션하세요"
        )
        return

    keyword = matches.group(1)
    price = int(matches.group(2))

    # 중고거래 사이트를 크롤링하여 가격대가 비슷한 상품을 찾아옵니다.
    message_blocks = make_sale_message_blocks(keyword, price)

    # 메시지를 채널에 올립니다
    slack_web_client.chat_postMessage(
        channel=channel,
        blocks=extract_json(message_blocks)
    )


# 사용자가 버튼을 클릭한 결과는 /click 으로 받습니다
# 이 기능을 사용하려면 앱 설정 페이지의 "Interactive Components"에서
# /click 이 포함된 링크를 입력해야 합니다.
@app.route("/click", methods=["GET", "POST"])
def on_button_click():
    # 버튼 클릭은 SlackEventsApi에서 처리해주지 않으므로 직접 처리합니다
    payload = request.values["payload"]
    click_event = MessageInteractiveEvent(json.loads(payload))

    keyword = click_event.block_id
    new_price = int(click_event.value)

    # 다른 가격대로 다시 크롤링합니다.
    message_blocks = make_sale_message_blocks(keyword, new_price)

    # 메시지를 채널에 올립니다
    slack_web_client.chat_postMessage(
        channel=click_event.channel.id,
        blocks=extract_json(message_blocks)
    )

    # Slack에게 클릭 이벤트를 확인했다고 알려줍니다
    return "OK", 200


# / 로 접속하면 서버가 준비되었다고 알려줍니다.
@app.route("/", methods=["GET"])
def index():
    return "<h1>Server is ready.</h1>"


if __name__ == '__main__':
    app.run('0.0.0.0', port=8080)
```



## 참고사항

### 인코딩 형식 변환

```python
def make_sale_message_blocks(keyword, price):
	query_text = urllib.parse.quote_plus(keyword, encoding="unicode-escape")
	query_text = query_text.replace("%5C", "%")
	search_url = "http://~"
```

