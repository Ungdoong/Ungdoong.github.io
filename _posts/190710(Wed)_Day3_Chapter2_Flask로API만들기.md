# 190710(Wed)\_Day3\_Chapter2\_Flask로API만들기

### API란?

- 일반적으로 API는 웹 API를 지칭
- 웹 API는 제공된 정보나 기능을 웹으로 연결된 다른 프로그램으로 조정할 수 있게 함

### Flask란?

- 파이썬 웹 어플리케이션을 만드는 마이크로프레임워크(microframework)

- 가벼우면서도 핵심적인 기능 보유

### Flask vs Django

- Flask
  - 높은 자유도
  - 가볍고 심플
  - LinkedIn
  - Pinterest
- Django
  - 풍부한 기능
  - 성숙한 커뮤니티
  - Instagram

### REST API 구현

- UR 요청에 따른 응답(Response)에 JSON 데이터를 전달
- 실제 production에서는 DB와 연동

## 실습

### 1. Hello, World!

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'
    
# 아래 코드는 수정하지 마세요.
app.run('0.0.0.0', port=8080)
```

### 2. 라우팅

```python
from flask import Flask
app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'Hello, World!'


# 경로가 rabbit인 경우 'Hello, Rabbit!'을 띄우는 코드를 작성하세요. 
@app.route('/rabbit')
def hello_rabbit():
    return 'Hello, Rabbit!'
    
@app.route('/cat')
def hello_cat():
    return 'Hello, Kitty!'

# 아래 코드는 수정하지 마세요.
app.run('0.0.0.0', port=8080)
```

### 3. URI에 변수 입력하기

```python
from flask import Flask
app = Flask (__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

# 변수 값을 받는 함수를 작성하세요. 
@app.rout('/rabbit/<name>')
def show_rabbit(name):
    return 'Is your name' + name + '?'

# 아래 코드는 수정하지 마세요.
app.run('0.0.0.0', port=8080)
```

### 4. URI에 int 형 변수 입력하기

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'


# int형 변수를 받는 함수를 작성하세요. 
@app.route(/rabbit/<int:count>)
def hello_rabbit(count):
    return '%d마리의 토끼가 있습니다'%(count * 100)

# 아래 코드는 수정하지 마세요.
app.run('0.0.0.0', port=8080)
```

### 5. REST API 구현

```python
from flask import Flask
from flask import jsonify
app = Flask(__name__)

@app.route('/')
def hello_world():
    data = {'animal':'rabbit', 'fruit':'apple'}
    return jsonify(data)

@app.route('/elice_info')
def hello_rabbit():
    data = {'rabbit':'white', 'character':'elice'}
    return jsonify(data)
    
# 아래 코드는 수정하지 마세요.
app.run('0.0.0.0', port=8080)
```

### 6. 에러 핸들링

```python
from flask import Flask

app = Flask(__name__)


@app.errorhandler(404)
def page_not_found(error):
    return "<h1>404 Error - Page Not Found</h1>", 404

@app.route('/')
def hello_world():
    return 'Hello, World!'
    
# 아래 코드는 수정하지 마세요.
app.run('0.0.0.0', port=8080)
```

