---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Mini-Max Sum'
date: 2017.08.03
category: 'hackerrank'
---

### solution

```javascript
function main () {
  arr = readLine().split(' ')
  arr = arr.map(Number)
  arr.sort()

  var sum = arr.reduce(function (sum, num) {
    return sum + num
  }, 0)

  console.log((sum - arr[arr.length - 1]) + ' ' + (sum - arr[0]))
}
```
