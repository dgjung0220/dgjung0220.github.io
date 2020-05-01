---
tags: ["프로그래머스 코딩테스트 고득점 Kit"]
title: '[프로그래머스] 해시 문제 연습 (중급) '
date: 2020.04.12
category: '코딩테스트'
---





### 해시 문제 연습하기



> *출처: 프로그래머스 코딩 테스트 연습,*
>
> https://programmers.co.kr/learn/courses/30/lessons/42578



고득점 키트 해시 문제의 중급 난이도. 이미 해시를 통해 풀면 쉽다는 것을 알고 있었기 때문에 큰 어려움없이 풀 수 있었다. 주어지는 input 을 해시맵으로 변환하고, 수학의 조합을 적용하면 쉽게 풀린다.



#### solution

```javascript
function solution(participant, completion) {
    var answer =1;
    let clothes_map = new Map()
    
    for (let i in clothes) {
        if (clothes_map.get(clothes[i][1])) {
            clothes_map.set(clothes[i][1], clothes_map.get(clothes[i][1]) + 1)
        } else {
            clothes_map.set(clothes[i][1], 1)
        }
    }
    
    const c_ary = Array.from(clothes_map.values())
    for (let i in c_ary) {
        answer *= (c_ary[i] + 1)
    }
    return answer - 1
}
```



#### 삽질

해시맵으로 변환은 잘 하였으나, 여러 경우의 수를 계산하고자 수학의 조합으로 생각하지 않고, 부분집합을 만들어 하나 하나 곱해보려고 했다. 부분집합을 구하기 위해서 재귀함수를 사용하였는데, 언제 또 부분집합을 구할 일이 있을까... 해서 기록한다.

```javascript
comp = [["yellow_hat", "headgear"], ["blue_sunglasses", "eyewear"], ["green_turban", "headgear"]]

let clothes_map = new Map()
for (i in comp) {
    if(clothes_map.get(comp[i][1])) {
        clothes_map.set(comp[i][1], clothes_map.get(comp[i][1]) + 1)
    } else {
        clothes_map.set(comp[i][1] , 1)
    }
}

let ary = new Array(clothes_map.size)
const value_map = Array.from(clothes_map.values())

get_subsets = (n, depth) => {
    if (n === depth) {
        for (i in ary) {
            console.log(ary)
        }
    } else {
        ary[n] = 1
        get_subsets(n+1, depth)
        ary[n] = 0
        get_subsets(n+1, depth)
    }
}

get_subsets(0, clothes_map.size)
```



#### + 파이썬은 더 쉽게...

파이썬에서는 주어진 배열에 대해 해시맵을 추출하는 것이 더 간단하다.  요즘 알고리즘 대회에서 라운드를 진행할수록 파이썬의 인기가 올라가고 있는데, 그 이유를 알 것도 같다.

```python
from collections import Counter

def solution(clothes) :
    answer = 1
    
    c = Counter(x[1] for x in clothes)
    for v in c.values():
        answer *= (v+1)
        
    answer -= 1
    returnr answer
```

