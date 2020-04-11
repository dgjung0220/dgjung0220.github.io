---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Migratory Birds'
date: 2018.01.07
category: 'hackerrank'
---

### solution

```javascript
function migratoryBirds (n, ar) {
    // Complete this function
  let most = 0
  let map = ar.reduce(function (ac, v, index) {
    ac[v]++
    return ac
  }, {
    1: 0, 2: 0, 3: 0, 4: 0, 5: 0
  })

  let resultSet = [5, 4, 3, 2, 1].reduce(function (ac, v, index) {
    if (map[v] >= ac.frequency) {
      ac.result = v
      ac.frequency = map[v]
    }

    return ac
  }, {
    result: 5,
    frequency: map[5]
  })

  return resultSet.result
}

function main () {
  var n = parseInt(readLine())
  ar = readLine().split(' ')
  ar = ar.map(Number)
  var result = migratoryBirds(n, ar)
  process.stdout.write('' + result + '\n')
}
```
