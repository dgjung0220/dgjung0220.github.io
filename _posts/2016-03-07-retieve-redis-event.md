---
tags: ["nodejs","redis"]
title: "[redis - node] key event 받아보기"
date: 2016.03.07
category: 'redis'
---


### redis key event 받아보기

* TOC
{:toc}

관련페이지 : [http://redis.io/topics/notifications](http://redis.io/topics/notifications)

* redis keyspace notification은 2.8버전부터 지원하며 버전 확인은  
-> info server  를 통해 확인 가능합니다.  

레디스에서는 키 이벤트에 대한 것들의 설정을 하게 되면 pub/sub 패턴을 이용하여 메세지(이벤트)를 전달합니다. 키 이벤트가 발생을 하게 되면 redis서버에서는 keyspace,keyevent의 포맷으로 메세지를 전달합니다.

>
> CONFIG SET nitifi-keyspace-events [option]
>

를 통해 설정하게 됩니다. options은 위 링크에 나와있습니다. 간단하게  
**keyspace에 관한 설정은 K**  
**keyevent에 관한 설정은 E**  
그 뒤의 옵션으로 g, $, l, s, h, z, x, e등이 있으며 A를 통하여 앞의 모든것들 설정가능합니다.각각 옵션에 대한것은 위의 본문 링크를 들어가서 확인하시면 됩니다.  

예.. 설명이 빈약합니다. ㅠ  
keyspace에 대한 설정을 하게 되면 **__keyspace@<db>__[key]** 라는 채널로 이벤트가 message로 발행이되고  
keyevent에 대한 설정을 하게 되면 **__keyevent@<db>__[event]** 라는 채널로 key가 message로 발행이됩니다.  

받아볼 이벤트의 종류는 **g, $, l, s, h, z, x, e, A** 등을 통하여 설정가능합니다.  
예로  
>
> CONFIG SET notifiy-keyspace-events KEA
>

라고 설정을 하게 된다면, 모든 이벤트에 대하여 keyspace,keyevent 채널로 발행이 될것입니다.  
제가 맨처음 필요로 했었던 expire에 대한 이벤트의 기준으로만 받아보고 싶다면  

> CONFIG SET notifiy-keyspace-events Ex  

이렇게 설정하면 됩니다.  
결과로는  

> __keyevent@<db>__:expired  

의 채널로 키값이 message로 얻어지게 될것입니다.  


간단히 redis에서의 pub/sub패턴을 소개하자면 publish,subscript, 즉, 발행, 구독!
레디스에서는 pub/sub를 패턴,채널,메세지의 요소로 나눠 이용합니다.
publisher가 메세지를 해당 채널에 메세지를 전달하게 되면,  패턴에 일치하는 subscriber들이 메세지를 받아보게 됩니다.

### 예제

pub/sub예제이자 redis notification예제입니다. node입니다.

```javascript
var redis = require("redis")
  , subscriber = redis.createClient()
  , red  = redis.createClient();

red.config("SET","notify-keyspace-events", "Ex");

subscriber.on("pmessage", function (pattern, channel, message) {
    console.log("pattern : "+pattern+" channel: "+channel+" message : "+message);
});
subscriber.psubscribe("__key*__:*");

red.setex("string key", 10,"string val",redis.print);
```

결과로는
> ``pattern : __key*__:* channel: __keyevent@0__:expired message : string key``

Ex로 설정하였기 때문에 해당 키가 expired된다면 해당 key값을 얻을 수 있습니다.

만약 KEA로 설정을 한다면

``pattern : __key*__:* channel: __keyspace@0__:string key message : set``  
``pattern : __key*__:* channel: __keyevent@0__:set message : string key``  
``pattern : __key*__:* channel: __keyspace@0__:string key message : expire``  
``pattern : __key*__:* channel: __keyevent@0__:expire message : string key``  
---------------------------------------  
``pattern : __key*__:* channel: __keyspace@0__:string key message : expired``  
``pattern : __key*__:* channel: __keyevent@0__:expired message : string key``  
이렇게 나옵니다. ㅎ
