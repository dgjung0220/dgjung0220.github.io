---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Diagonal Difference'
date: 2017.08.03
category: 'hackerrank'
---

### solution

```javascript
function main () {
  var n = parseInt(readLine())
  var a = []
  for (a_i = 0; a_i < n; a_i++) {
    a[a_i] = readLine().split(' ')
    a[a_i] = a[a_i].map(Number)
  }

  var pri = a.reduce(function (sum, v, k) {
    return sum + v[k]
  }, 0)
  var sec = a.reduce(function (sum, v, k) {
    return sum + v[n - 1 - k]
  }, 0)

  var result = Math.abs(pri - sec)

  console.log(result)
}
```
