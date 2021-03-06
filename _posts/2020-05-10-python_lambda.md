---
tags: ["파이썬","람다"]
title: '파이썬 람다'
date: 2020.05.10
category: '파이썬'
---

파이썬 람다 함수는 단일문으로 표현되는 익명 함수이다.

람다 함수를 이해하기 위해 먼저 아래와 같이 code의 묶음과 특정 함수를 받아 출력하는 코드를 작성한다.

```python
def edit_code(codes, func):
    for code in codes:
        print(func(code))
```

codes에 들어갈 리스트를 만들기 위해 다음과 같이 정의한다. 파이썬에서 사용되는 예약어들 중 일부이다.

```python
reserved_word = ['False', 'None', 'assert', 'break', 'with', 'return', 'yield']
```

이제 이 예약어들은 변수로 사용할 수 없다고 알려주는 함수를 만들어준다.

```python
def warning_python(word):
    return word + ' is a reserved_word!'
```

edit_code에 적용하여 결과를 보면 다음과 같다.

```shell
>>> edit_code(reserved_word, warning_python)
```

```shell
False is a reserved_word!
None is a reserved_word!
assert is a reserved_word!
break is a reserved_word!
with is a reserved_word!
return is a reserved_word!
yield is a reserved_word!
```

위의 내용을 람다를 이용하면 비교적 간단하게 바꿀 수 있다.

```shell
>>> edit_code(reserved_word, lambda word : word + 'is reserved_word!')
```

대부분의 경우, 실제 함수를 구현하여 표현하는 것이 람다를 사용하는 것보다 훨씬 더 명확하다. 람다는 다양한 작은 크기의 함수를 정의하고, 이들을 호출해서 얻은 모든 결과값을 저장해야 하는 경우에 유용하다. (데이터 사이언스에 많이 사용된다.) 
