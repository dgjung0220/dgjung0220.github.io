---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Apple and Orange'
date: 2018.01.01
category: 'hackerrank'
---

### solution

```javascript

function appleAndOrange (s, t, a, b, apple, orange) {
    // Complete this function
  let result = [0, 0]
  for (let item1 of apple) {
    let point1 = item1 + a
    if (s <= point1 && point1 <= t) {
      result[0]++
    }
  }

  for (let item2 of orange) {
    let point2 = item2 + b
    if (s <= point2 && point2 <= t) {
      result[1]++
    }
  }

  return result
}

function main () {
  var s_temp = readLine().split(' ')
  var s = parseInt(s_temp[0])
  var t = parseInt(s_temp[1])
  var a_temp = readLine().split(' ')
  var a = parseInt(a_temp[0])
  var b = parseInt(a_temp[1])
  var m_temp = readLine().split(' ')
  var m = parseInt(m_temp[0])
  var n = parseInt(m_temp[1])
  apple = readLine().split(' ')
  apple = apple.map(Number)
  orange = readLine().split(' ')
  orange = orange.map(Number)
  var result = appleAndOrange(s, t, a, b, apple, orange)
  console.log(result.join('\n'))
}
```
