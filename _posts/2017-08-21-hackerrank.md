---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Kangaroo'
date: 2017.08.21
category: 'hackerrank'
---

### solution

```javascript
function kangaroo (x1, v1, x2, v2) {
    // Complete this function
  var aws = (x2 - x1) / (v1 - v2)
  if (aws > 0 && aws == parseInt(aws)) {
    return 'YES'
  }
  return 'NO'
}

function main () {
  var x1_temp = readLine().split(' ')
  var x1 = parseInt(x1_temp[0])
  var v1 = parseInt(x1_temp[1])
  var x2 = parseInt(x1_temp[2])
  var v2 = parseInt(x1_temp[3])
  var result = kangaroo(x1, v1, x2, v2)
  process.stdout.write('' + result + '\n')
}
```
