---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Staircase'
date: 2017.08.03
category: 'hackerrank'
---

### solution

```javascript
function main () {
  var n = parseInt(readLine())

  var space = ' '
  var symbol = '#'

  var result = []
  for (var i = 0; i < n; i++) {
    result.push(space.repeat(n - i - 1) + symbol.repeat(i + 1))
  }

  console.log(result.join('\n'))
}
```
