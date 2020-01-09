# 190611(Tus)_DockerRedis

### Image

- 파일시스템과 CMD로 이루어짐

```
FROM ~
CP ~
CMD ~
```

### Redis

```
#docker run redis
#docker exec -it <ID> redis-cli
```

##### Dockerfile

```dockerfile
FROM alpine

RUN apk add --update redis

CMD ["redis-server"]
```

```
#docker build .
#docker build -t ungdoong/redis-server:1.0 .
#docker build -t ungdoong/redis-server:latest .
#docker run dungdoong/redis-server
```

##### client

```
#docker exec -it <ID> redis-cli
```

### Ubuntu:16.04

```
#docker run -it ubuntu:16.04 sh
```

### NVM

```
#curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
#nvm install 10.14.2
#mkdir simpleweb
#cd simpleweb/
#npm init
```

##### exprees 모듈 설치

```
#npm install --save express
```

- package.jon 파일 확인 및 수정

  ```json
  {
    "name": "simpleweb",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "start": "node app.js"
    },
    "author": "",
    "license": "ISC",
    "dependencies": {
      "express": "^4.17.1"
    }
  }
  ```

- app.js 파일 추가

  ```json
  const express = require('express');
  
  const app = express();
  
  app.get('/', (req, res) => {
    res.send('Hi there');
  });
  
  app.listen(8080, () => {
    console.log('Listening at 8080');
  })
  ```

- node 서버 동작 확인

  ```
  #node app.js or #npm start
  ```

##### Docker container에서 실행

- Dockerfile 생성

  ```dockerfile
  # Specify a base image
  FROM node:10-alpine
  
  # Install some dependencies
  COPY . .
  RUN npm Install
  
  # Default command
  CMD ["npm", "start"]
  ```

- docker build

  ```
  #docker build -t ungdoong/node_server:1.0 .
  ```

- docker run

  ```
  #docker run <Image ID>
  ```

  

