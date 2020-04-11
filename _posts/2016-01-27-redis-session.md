---
tags: ["nodejs","redis"]
title: "[nodejs - redis] redis - session 레디스 세션 이용하기"
date: 2016.01.27
category: 'redis'
---

### redis를 session서버로 사용하는 방법

* TOC
{:toc}

session의 값을 server와 분리 시킴으로서 안정적으로 저장하고 여러개의 서버의 세션을 하나의 redis서버로 통합하여 사용 할 수 있습니다. session의 특성상 입출력이 빈번하기 때문에 redis의 성격과도 잘 맞다고 생각됩니다.  

우선 사용 모듈은  

> npm install express-session  
> npm install redis  
> npm install connect-redis  
> npm install redis-session  

입니다.  

express와 레디스 설치 방법은 생략합니다.

```javascript
//redis session require
var session = require('express-session');
var redis = require('redis');
var redisStore = require('connect-redis')(session);
var client = redis.createClient();

app.use(session(
    {
        secret: 'secret_key',
        store: new redisStore({
            host: "127.0.0.1",
            port: 6379,
            client: client,
            prefix : "session:",
            db : 0
        }),
        saveUninitialized: false, // don't create session until something stored,
        resave: true // don't save session if unmodified
    }
));

//....아래쪽...

//redis session input output
var router = express.Router();
router.get('/session/set/:value', function(req, res) {
    req.session.redSession = req.params.value;
    res.send('session written in Redis successfully');
});

app.get('/session/get/', function(req, res) {
    if(req.session.redSession)
        res.send('the session value stored in Redis is: ' + req.session.redSess);
    else
        res.send("no session value stored in Redis ");
});
```

를 app.js에 작성하면 설정이 끝나게 됩니다.  

new redisStore 부분에 자신의  reids설정을 작성하고 db의 파라미터를 통하여 session store로 사용할 db index를 설정합니다.  

### 결과

prefix를 통하여 키값의 앞에 붙일 값들을 정합니다. redis는 : <- 를 통하여 키값들을 grouping하게 됩니다. 즉, 위와 같이 해놓게 된다면 session에 해당하는 key값들은  
아래 결과의 사진을 보게 됬을때 정리가 깔금하게 됩니다.  

session의 사용방법은 [여기](http://mythinkg.blogspot.kr/2015_01_01_archive.html)를 참고해주시면 됩니다.  
단순히 redis에 값을 저장하는 것이기 때문에 사용방법은 express-session과 같고 결과는 아래와 같이 나오는 걸 확인하여 session값이 저장됨을 볼 수있습니다.  

![](/images/redis-session-result.png?style=centerme)  
