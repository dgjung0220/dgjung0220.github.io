---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Time Conversion'
date: 2017.08.21
category: 'hackerrank'
---

### solution

```javascript
function timeConversion (s) {
    // Complete this function
  var time = s.slice(0, 8)
  var word = s.slice(8, 10)

  var arr = time.split(':')

  if (word == 'AM') {
    if (arr[0] == '12') {
      arr[0] = '00'
    }

    return arr.join(':')
  }

  if (arr[0] !== '12') {
    arr[0] = parseInt(arr[0]) + 12
  }

  return arr.join(':')
}

function main () {
  var s = readLine()
  var result = timeConversion(s)
  process.stdout.write('' + result + '\n')
}
```
