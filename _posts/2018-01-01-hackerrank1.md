---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Breaking the Records'
date: 2018.01.01
category: 'hackerrank'
---

### solution

```javascript
function breakingRecords (score) {
  let lowest, highest
  let result = [0, 0]
  lowest = highest = score.shift()
  for (let record of score) {
    if (record < lowest) {
      lowest = record
      result[1]++
    } else if (record > highest) {
      highest = record
      result[0]++
    }
  }

  return result
}

function main () {
  var n = parseInt(readLine())
  score = readLine().split(' ')
  score = score.map(Number)
  var result = breakingRecords(score)
  console.log(result.join(' '))
}
```
