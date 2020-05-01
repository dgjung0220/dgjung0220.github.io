---
tags: ["해커랭크"]
title: '[Hackerrank] 코딩 테스트 (Arrays)'
date: 2020.04.30
category: '코딩테스트'
---

##### Arrays (1)

>  https://www.hackerrank.com/challenges/2d-array/problem
>
>  Difficulty : Easy

```javascript
(() => {
        let array = [1,1,1,0,0,0,0,1,0,0,0,0,1,1,1,0,0,0,0,0,2,4,4,0,0,0,0,2,0,0,0,0,1,2,4,0]
        let arr = []
        let arr_temp = []
    	
        //2차원 배열 생성
        for(i of array) {
             arr_temp.push(i)
             if (arr_temp.length === 6) {
                 arr.push(arr_temp)    
                 arr_temp = []
             }
        }

        console.log(arr)

        maxX = 3;
        maxY = 3;
        total = -63;  
   
        for (let y = 0; y <= maxY; y++) {
            for (let x = 0; x <= maxX; x++) {
                let sum = arr[y][x] + arr[y][x+1] + arr[y][x+2];
                sum += arr[y+1][x+1];
                sum += arr[y+2][x] + arr[y+2][x+1] + arr[y+2][x+2]
            	
                // sum이 더 작으면 굳이 대입하지 않는다.
                if (total < sum)
                     total = sum;
            }
        }

        console.log(total)
})()
```



##### Arrays (2)

> https://www.hackerrank.com/challenges/ctci-array-left-rotation/problem
>
> Difficulty : Easy

​	js 에서 배열 shift 함수쓰면 쉽다.

```javascript
function rotLeft(a, d) {
	// a : 배열
    // d : left rotation 반복 횟수
    
    for (let i = 0; i < d; i++) {
        let temp = a.shift()
        a.push(temp)
    }

    return a
}
```

​	3초컷이니 어렵게 풀어본다. 근데 풀어보니 쉽긴 매한가지다.

```javascript
function rotLeft(a, d) {
    let temp = a[0]
    
    for (let i = 0; i < d; i++) {
        for (let j = 0; j < d; j++) {
            a[j] = a[j+1]
        }
        a[a.length-1] = temp
    }
    
    return a
}
```



