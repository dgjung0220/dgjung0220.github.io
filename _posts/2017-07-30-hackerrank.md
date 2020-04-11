---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Grading Students'
date: 2017.07.30
category: 'hackerrank'
---

### solution

```javascript

function solve (grades) {
    // Complete this function
  var n = grades.length
  var board = []
  for (var i = 0; i < n; i++) {
    var tmp = grades[i]
    var grade = rescore(grades[i])
    grade >= 40 ? board.push(grade) : board.push(tmp)
  }

  return board
}

function rescore (grade) {
  var i = grade % 10
  var a = i - 5 < 0 ? 5 : 10
  if (a - i < 3) {
    grade = grade - i + a
  }

  return grade
}

function main () {
  var n = parseInt(readLine())
  var grades = []
  for (var grades_i = 0; grades_i < n; grades_i++) {
    grades[grades_i] = parseInt(readLine())
  }
  var result = solve(grades)
  console.log(result.join('\n'))
}
```
