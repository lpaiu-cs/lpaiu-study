---
{"dg-publish":true,"permalink":"/= Study/Python/numpy/","created":"2024-01-04T20:03:34.000+09:00","updated":"2025-01-14T15:33:46.000+09:00"}
---

numpy는 [[library and module.\|외부 라이브러리]]이다.

따라서 사용하기 위해서는 다음 처럼 나타내야 한다.
```python
import numpy as np
```
이것은 numpy를 np라는 이름으로 가져오라는 뜻이다. 관습적으로 numpy를 np라는 이름으로 많이 사용하는데, 사실 as np 부분은 없어도 된다. 다만 이름이 길어서 불편하기 때문에 사용하지 않는다.
```python
x = np.array([1.0, 2.0, 3.0])

print(x)
# >>> [1. 2. 3.]
print(type(x))
# >>> <class 'numpy.ndarray'>
```
이를 통해numpy 모듈에 존재하는 클래스인 ndarray를 선언했다.
그런데 array가 아닌 이유는 뭘까? 그 이유는 array는 numpy 모듈 내부에 있는 ndarray 선언을 도와주는 함수이기 때문이다. 도움이 필요한 이유는 직접 선언하기에는 좀 어려운 면이 있어서 그렇다.
```python
# array 함수 없이 ndarray 클래스 직접 선언하기 

import array
# array.array를 사용하여 파이썬 리스트를 메모리 버퍼로 변환
buffer = array.array('d', [1.0, 2.0, 3.0])

# memoryview를 사용하여 buffer를 numpy.ndarray의 buffer 매개변수에 전달
x = np.ndarray(shape=(3,), dtype=float, buffer=memoryview(buffer))

print(x)
# >>> [1. 2. 3.]
```
이처럼 직접 선언으로도 동일한 결과를 얻을 수 있다. **하지만 굳이 알고싶지 않다.**

# numpy element-wise calculate
```python
x = np.array([1.0, 2.0, 3.0])
y = np.array([2.0, 4.0, 6.0])

print(x + y)
# >>> [3. 6. 9.]

print(x - y)
# >>> [-1. -2. -3.]

print(x * y)
# >>> [ 2.  8. 18.]

print(x / y)
# >>> [0.5 0.5 0.5]
```
# numpy N-dimension array
```python
A = np.array([1.0, 2.0, 3.0], [2.0, 4.0, 6.0])
print(A)
# >>> [[1. 2. 3.]
#      [2. 4. 6.]]
print(A.shape)
# >>> (2, 3)
print(A.dtype)
# >>> float64
B = np.array([[3, 0, 0], [0, 6, 0]])

print(A + B)
# >>> [[ 4.  2.  3.]
#      [ 2. 10.  6.]]
print(A * B)
# >>> [[ 3.  0.  0.]
#      [ 0. 24.  0.]]
```
주의할 점은 '+'과 '\*' 연산을 할 때, shape가 동일해야 한다.
그리고 '\*'은 두 매트릭스간의 내적 연산이 아니다. 각 원소의 자리별로 계산, 즉 element-wise 계산이다.

