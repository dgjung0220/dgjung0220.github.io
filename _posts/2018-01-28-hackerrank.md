---
tags: ["hackerrank solution"]
title: '[hackerrank solution] Super Reduced String'
date: 2018.01.28
category: 'hackerrank'
---

### solution

```javascript
function super_reduced_string (s) {
    // Complete this function
  let chars = s.split('')
  let diff = null
  while (diff !== 0) {
    let beforeLength = chars.length

    let newChars = []
    for (let idx = 0; idx <= chars.length; idx++) {
      if (chars[idx] === chars[idx + 1]) {
        idx++
      } else {
        newChars.push(chars[idx])
      }
    }

    chars = newChars

    let afterLength = chars.length

    diff = beforeLength - afterLength
  }

  return chars.join('') || 'Empty String'
}
```
