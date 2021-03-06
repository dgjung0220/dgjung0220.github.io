---
tags: ["ECMAScript"]
title: '[JS] Map & Set'
date: 2020.04.30
category: '자바스크립트'
---

#### Map과  Set 기능 추가

​	JS 표준에는 기존에 다른 언어에서 많이 사용하고 있던 데이터 구조도  포함되었다. JS에서 이러한 기능들은 객체와 배열의 조합으로 해결 가능하지만, Key, Value 값을 찾는데 별도의 추가 구현들이 필요했다. 이런 점들을 개선하고 기본으로 제공하기 위한 Map, Set 데이터 구조가 추가되었다.



#### Map 기본 사용 방법

``` javascript
(() => {   
    // 맵 생성 및 추가
    var map = new Map(),
        key = {name: "Language"}
    
    map.set('Name', 'DONGGOO JUNG')
        .set('Score', 'A+')
        .set(key.name, 'Computer Vision')
        
    // 맵 조회
    for (let [key, value] of map.entries()) {
        console.log(key + " : " + value)
    }
    // Name : DONGGOO JUNG
    // Score : A+
    // Language : Computer Vision
})()
```

​	재미있는 것은 Map에서 Key 값으로 Object도 가능하다.

```javascript
var map = new Map(),
    key = {name: "Language"}

map.set(key.name, 'Computer Vision')

// 맵 조회 (Record)
for (let [key, value] of map.entries()) {
    console.log(key + " : " + value)
}
// [object Object] : Computer Vision
```



#### Set 기본 사용 방법

```javascript
(() => {
    // 셋 생성 및 추가
    var set = new Set()
    set.add('C#').add('Python').add('Lua').add('Lua').add('Lua')
    
    // 조회
    console.log(set.has('C#'))        // True
    console.log(set.has('C++'))       // False
    
    // 셋 조회
    for (let key of set.values()) {
        console.log(key)
    }
    // C#
    // Python
    // Lua
})()
```

​	중복되는 키값을 넣더라도, 알아서 걸러준다.

```javascript
set.add('C#').add('Python').add('Lua').add('Lua').add('Lua') // Lua는 한 번만 들어감
```



