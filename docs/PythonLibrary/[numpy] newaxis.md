---
layout: post
title: '[Numpy] 파이썬 넘파이에서 [:, None]식 배열 표현 (numpy.newaxis)'
parent: Python 라이브러리
---
### 파이썬 numpy.newaxis를 알아보자 ([:,None] or [None,:])
<br>
다른 사람들이 짜놓은 코드를 보면 아래와 같이 넘파이 배열에 **None**을 넣는 경우가 있다.

```python
import numpy as np

array = np.array([1,2,3,4])
print(array[:, None])   # --> [[1]
                        #      [2]
                        #      [3]
                        #      [4]]
```

결론만 빠르게 말하면, **None**을 넣은 곳에 새로운 차원 하나가 더 생긴다. 단 새로 생긴 차원의 크기는 항상 1이다. 아래 예시를 보면 한눈에 알 수 있다.

```python
import numpy as np

array = np.array([1,2,3,4])
print(array[:, None].shape)         # --> (4, 1)
print(array[:, None, None].shape)   # --> (4, 1, 1)
print(array[None, 0:2, None].shape) # --> (1, 2, 1)
print(array[None, :, None].shape)   # -->  (1, 4, 1)
```

중간에 **:** 대신 **0:2** 들어간걸 볼 수 있는데, 기존의 문자열을 슬라이싱 해서 넣을 수 있다. 추가적으로 **:** 가 들어갈 수 있는 자리는 최대 기존 배열의 차원 수 만큼 들어갈 수 있다. 예를 들어 3차원 배열이면 **:** 는 3개, 2개, 1개, 0개가 들어갈 수 있다. 아래 기가막힌 예시가 있다.

```python
import numpy as np

array = np.array(np.ones([3,5,7]))      # --> make array with "1"
print(array.shape)                      # --> (3, 5, 7)

print(array[None].shape)                # --> (1, 3, 5, 7)
print(array[None, None].shape)          # --> (1, 1, 3, 5, 7)
print(array[None, :, None, :].shape)    # --> (1, 3, 1, 5, 7)
print(array[:,:,None,:,None].shape)     # --> (3, 5, 1, 7, 1)
print(array[:,:,None,:,None, :].shape)  # --> IndexError: too many indices for array: array is 3-dimensional, but 4 were indexed
```

코드를 보면 남는 차원들( **:** )은 뒤에 자동으로 붙어서 결과가 출력되는 것 같다. 그리고 **:** 의 차원의 크기는 기존 배열(**array**)의 순서를 앞에서부터 따라가는 것을 볼 수 있다. **:** 의 개수가 기존 배열의 차원보다 크다면 **IndexError**를 뱉어버린다!

추가적으로 **None**는 **np.newaxis**와 같은 역할로 쓸 수 있다. 자세히 알아보고 싶다면 구글에 numpy.newaxis를 검색해서 스스로 알아보도록 하자!

```python
import numpy as np

array = np.array([1,2,3])

print(array[None, :].shape)         # --> (1, 3)
print(array[np.newaxis, :].shape)   # --> (1, 3) 
```

하지만 아래 코드를 보면 한가지 의문이 든다...

```python
import numpy as np

array = np.array([1,2,3])

print(array[None, :].shape)         # --> (1, 3)
print(array.reshape(1,-1).shape)    # --> (1, 3)
```

**reshape()** 함수가 건재한데 굳이 **None**을 쓰는 이유가 있나 궁금해진다. 찾아보니까 **reshape()** 함수 안에서 -1을 두번이상 써야 할 상황이 있는데(아마 두개이상의 차원의 크기를 모를때), 이때 **None**을 쓰면 아주 좋다고 한다.~~(무슨 상황에 이런일이 일어나는지는 잘 모르겠...)~~

아래는 **None** 쓰기 좋은 예시를 한번 구현해봤다.
```python
import numpy as np

array = np.array([[1,2,3], [10,20,30]])
print(array.shape)                  # --> (2, 3)

print(array[:,None,:].shape)        # --> (2, 1, 3)
print(array.reshape(-1,1,-1).shape) # --> ValueError: can only specify one unknown dimension
```