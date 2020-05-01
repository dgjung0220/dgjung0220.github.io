---
tags: ["해커랭크"]
title: '[Hackerrank] 코딩 테스트 (warm up)'
date: 2020.04.30
category: '코딩테스트'
---

##### Warm up (1)

>  https://www.hackerrank.com/challenges/sock-merchant/problem
>
>  Difficulty : Easy

```javascript
// Test Case
const n = 9
const ar = [10, 20, 20, 10, 10, 30, 50, 10, 20]
const socks_map = new Map()
// Test Case end

let count = 0

for (var i in ar) {
    if (socks_map.get(ar[i])) {
        socks_map.set(ar[i], socks_map.get(ar[i]) + 1)
    } else {
        socks_map.set(ar[i], 1)
    }
}

for (let [key, val] of socks_map.entries()) {
    if (val >= 2) {
        count += Math.floor(val/2)
    }
}

console.log(count)
```





##### Warm up (2)

> https://www.hackerrank.com/challenges/counting-valleys/problem
>
> Difficulty : Easy

​	문제가 무슨 말인지 좀 이해하기 어려웠다;; 어떤 데이터 구조를 사용하면 쉽게 풀 지 떠오르지 않아 그냥 생각나는 대로 코딩했다. 문제 설명에 비해 문제 풀이는 굉장히 쉬운 문제.

```javascript
(() => {
	// Test Case
    //n = 8
    //s = 'UDDDUDUU'
    n = 12
    s = 'DDUUDDUDUUUD'
    
    
    let sea_level = 0
    let start_flag = false
    let vally_num = 0

    for (let i in s) {
        
        if (s[i] === 'U') {
            sea_level += 1
        } else {
            sea_level -= 1
        }

        if (sea_level === -1) {
            start_flag = true
        } else if (res === 0 && start_flag === true) {
            vally_num += 1
            start_flag = false
        }
    }

    console.log(vally_num)
})()
```



##### Warm up (3)

> https://www.hackerrank.com/challenges/jumping-on-the-clouds/problem
>
> Difficulty : Easy

​	Greedy 알고리즘 방식으로 풀었다. 적은 점프를 하기 위해 무조건 2번 점프하되, 2번 점프시 번개 구름이 있을 경우 1번 점프했다. 문제에 무조건 이길 수 있다는 가정이 있기 때문에 큰 고려없이 이렇게 풀어도 답은 잘 나온다.

```javascript
(() => {
    const c = [0,0,1,0,0,0,0,1,0,0]
    const thunder_indices = []
    let pos = 0,
        jumping_count = 0

    c.forEach(function(v, i) {
        if (v === 1) {
            thunder_indices.push(i)
        }
    })

    while(true) {        
        let temp_pos = pos + 2
        if (thunder_indices.indexOf(temp_pos) !== -1) {
            temp_pos = pos + 1
        }
        pos = temp_pos
        jumping_count += 1

        if (pos >= c.length-1) {
            break;
        }
    }

    console.log(jumping_count)

})()
```



##### Warm up (4)

> https://www.hackerrank.com/challenges/repeated-string/problem
>
> Difficulty : Easy

​	

​	문제 푸는 방법은 쉽지만, 특이 케이스때문에 계산이 많아질 수 있다.

```javascript
(() => {
    const s = 'aba'
    const n = 10
    
    let result = 0
    
    let s_length = s.length
    let count = s.split('').filter(word => word==='a').length

    result += Math.floor(n / s_length) * count
    let rest = n % s_length

    for (let i = 0; i < rest; i++) {
        if (s[i] === 'a') {
            result += 1
        }
    }

    console.log(result)
})()
```

###### 케이스 통과 못 하는 경우

​	쓸 데 없이, repeated_string을 굳이 만들어서 filter를 쓰니 N이 클수록 계산이 복잡해진다.

```javascript
(() => {
    const S = 'aba'
    const n = 10

    const repeated_string = []

    for(let i = 0; i < n; i++) {
        repeated_string.push(S[i%3])
    }

    console.log(repeated_string.filter(word => word === 'a').length)
    
})()
```

