---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Day of the Programmer'
date: 2018.01.07
category: 'hackerrank'
---

### solution

```javascript
function divisible (i, j) {
  return !(i % j)
}

function solve (year) {
    // Complete this function
  let date = [13, '09', year]

  if (year === 1918) {
    date[0] = 26
  } else if (year > 1918) {
    if (divisible(year, 400) || (divisible(year, 4) && !divisible(year, 100))) {
      date[0]--
    }
  } else if (divisible(year, 4)) {
    date[0]--
  }
  return date.join('.')
}

function main () {
  var year = parseInt(readLine())
  var result = solve(year)
  process.stdout.write('' + result + '\n')
}

```
