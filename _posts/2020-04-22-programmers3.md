---
tags: ["프로그래머스 코딩테스트 고득점 Kit"]
title: '[프로그래머스] 스택 문제 연습_1 (중급) '
date: 2020.04.22
category: '코딩테스트'
---



### 스택 문제연습



> *출처: 프로그래머스 코딩 테스트 연습,*
>
> https://programmers.co.kr/learn/courses/30/lessons/42588



스택 문제 첫번째. 이 것도 이미 스택을 사용해야 쉽다는 것을 알고 있기 때문에 쉽게 풀 수 있다.  처음에 Stack 클래스를 밖에다 만들었는데, 프로그래머스 환경에서는 구현이 안 된다. 그래서 Solution 펑션 안에 class를 만들어줬다.



#### solution

```javascript
function solution(heights) {
    class Stack {
        constructor() {
            this._arr = []
        }
        push(item) { this._arr.push(item) }
        pop(item) { return this._arr.pop() }
        peek() { return this._arr[this._arr.length - 1] }
        getAry() { return this._arr}
    }
    
    var answer = [];
    const stack = new Stack()
    
    for (let i in heights) {
        stack.push(heights[i])
    }
    
    while(true) {
        let target = stack.pop()
        if (stack.peek() === undefined) {
            answer.push(0)
            
            return answer.reverse()
        }
        
        let stack_ary = stack.getAry()
        let value = 0
        for (let i in stack_ary) {
            if (target < stack_ary[stack_ary.length-1-i]) {
                value = stack_ary.length-i
                break
            }
        }
        answer.push(value)
    }
}
```
