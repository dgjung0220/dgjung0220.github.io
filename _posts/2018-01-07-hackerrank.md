---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Divisible Sum Pairs'
date: 2018.01.07
category: 'hackerrank'
---

### solution

```javascript
function divisibleSumPairs (n, k, ar) {
    // Complete this function
  let result = 0
  while (ar.length !== 0) {
    let i = ar.shift()
    for (let j of ar) {
      if (!((i + j) % k)) {
        result++
      }
    }
  }

  return result
}

function main () {
  var n_temp = readLine().split(' ')
  var n = parseInt(n_temp[0])
  var k = parseInt(n_temp[1])
  ar = readLine().split(' ')
  ar = ar.map(Number)
  var result = divisibleSumPairs(n, k, ar)
  process.stdout.write('' + result + '\n')
}
```
