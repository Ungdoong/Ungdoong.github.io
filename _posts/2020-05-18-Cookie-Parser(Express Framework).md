## Cookie-Parser(Express Framework)



## Cookie-Parser란?

______

- 요청된 쿠키를 쉽게 추출할 수 있도록 도와주는 미들웨어
- experss의 request(req) 객체에 cookies 속성이 부여

### 설치

```shell
$ npm i -S cookie-paser
```

### 사용 예제

```js
var express = require('express')
var cookieParser = require('cookie-parser');

var app = express();
app.use(cookieParser());

app.get('/', function(req, res) {
    console.log('Cookies: ', req.cookies)
})
app.listen(3000)
```

