---
tags: ["파이썬","벡터"]
title: '파이썬으로 보는 벡터의 이해'
date: 2020.05.09
category: '파이썬'
---

벡터의 원리를 파이썬으로 이해하기 위한 기능 구현. 다만 성능 자체는 좋지 않기 때문에 실제 벡터 계산시에는 numpy를 사용한다.



##### 벡터의 표현

파이썬에서 벡터 자료형이 없기 때문에 벡터를 float 객체를 갖고 있는 리스트인 Vector라는 타입으로 명시한다.

```python
from typing import List
Vector = List[float]
```



##### 벡터의 덧셈

```python
def vector_add(v:Vector, w:Vector) -> Vector :
    """Vector addition"""
    assert len(v) == len(w), "Vector must be the same length"
    
    return [v_i + w_i for v_i, w_i in zip(v, w)]
```

```python
assert vector_add([1,2,3],[4,5,6]) == [5,7,9]
```



##### 벡터의 뺄셈

```python
def vector_subtract(v:Vector, w:Vector) -> Vector :
    """Vector subtraction"""
    assert len(v) == len(w), "Vector must be the same length"
    
    return [v_i - w_i for v_i, w_i in zip(v,w)]
```

```python
assert vector_subtract([1,2,3],[4,5,6]) == [-3, -3, -3]
```



##### 여러 개의 벡터 각 성분의 덧셈

```python
def vector_sum(vectors:List[Vector]) -> Vector :
    """Sum of Vectors"""
    
    # 비어있는지 확인
    assert vectors, "no vectors provided."
    
    num_elements = len(vectors[0])
    assert all(len(v) == num_elements for v in vectors), "Vector must be the same length"
    
    return [sum(vector[i] for vector in vectors) for i in range(num_elements)]
```

```python
assert vector_sum([[1,2], [3,4], [5,6], [7,8]]) == [16, 20]
```



##### 벡터 스칼라 곱셈

```python
def scalar_multiply(c : float, v : Vector) -> Vector :
    return [c * v_i for v_i in v]
```

```python
assert scalar_multiply(3, [1,2,3]) == [3,6,9]
```



##### 여러 개의 벡터 각 성분의 평균

```python
def vector_mean(vectors : List[Vector]) -> Vector :
    n = len(vectors)
    return scalar_multiply(1/n, vector_sum(vectors))
```

```python
assert vector_mean([1,2], [3,4], [5,6]) == [3,4]
```



##### 벡터의 내적(dot-product)

```python
def vector_dot(v: Vector, w: Vector) -> Vector :
    """Dot product"""
    assert len(v) == len(w), "Vector must be the same length"
    
    return sum(v_i * w_i for v_i, w_i in zip(v, w))
```

```python
assert vector_dot([1,2,3], [4,5,6]) == 32
```



##### 백터의 크기

```python
def sum_of_squares(v: Vector) -> float :
    return vector_dot(v, v)

import math

def magnitude(v : Vector) -> float :
    return math.sqrt(sum_of_squares(v))
```

```python
assert magnitude([3,4]) == 5
```



##### 두 벡터간의 거리

```python
def sqaured_distance(v: Vector, w: Vector) -> float :
    return sum_of_squares(vector_subtract(v,w))

import math

def distance(v: Vector, w: Vector) -> float :
    return math.sqrt(squared_distance(v,w))
```

다른 방법으로는,

```python
def distance(v: Vector, w: Vector) -> float :
    return magnitude(vector_subtract(v,w))
```
