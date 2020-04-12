---
tags: ["프로그래머스 코딩테스트 고득점 Kit"]
title: '[프로그래머스] 해시 문제 연습 (초급) '
date: 2020.04.12
category: 'programmers'
---





### 해시 문제 연습하기



> *출처: 프로그래머스 코딩 테스트 연습,*
>
> https://programmers.co.kr/learn/courses/30/lessons/42576



초급 문제이고 문제도 쉽다. js의 indexOf, splice 등을 이용하면 정답은 간단히 맞출 수는 있는데.... 효율성 테스트에서 빵점을 맞고만다;; 해시로 풀면 간단하다는 프로그래머스의 힌트처럼 해시맵을 이용해서 체크하면 쉽게 풀 수 있다.



#### solution

```javascript
function solution(participant, completion) {
    var answer ='';
    let com_map = new Map()
    
    for (let i in completion) {
        if (!com_map.get(completion[i])) {
            com_map.set(completion[i], 1)
        } else {
            com_map.set(completion[i], com_map.get(completion[i]) + 1)
        }
    }
    
    for (let i in participant) {
        if (!com_map.get(participatn[i] || com_map.get(participant[i]) === 0)) {
            answer = participant[i]
            return answer
        } else {
            com_map.set(participant[i], com_map.get(participant[i]) - 1)
        }
    }
}
```



#### 처음 접근했던 방법

indexOf, splice를 이용한 간단한 방법인데 각 함수들이 내부적으로 for문을 도는지 효율성이 극악으로 나왔다.

```javascript
function solution(participant, completion) {
    var answer = ''
    
    for (let i in completion) {
        let index = participant.indexOf(completion[i])
        
        if (index !== -1) {
            participant.splice(index, 1)
        }
    }
    answer = participant[0]
    
    return answer
}
```
