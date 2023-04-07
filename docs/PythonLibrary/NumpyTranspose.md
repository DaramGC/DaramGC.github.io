---
layout: default
title: '[Numpy] 파이썬 numpy.transpose() 함수'
parent: Python 라이브러리
---
### 파이썬 numpy.transpose() 함수를 알아보자
<br>

일반적으로 아래와 같이 사용할 수 있다
```python
import numpy as np

# np.ones([a,b,c, ...]) 함수는 1로이루어진 [a,b,c, ...]크기의 배열을 만든다
x1 = np.ones([2,3,4,5])
x1 = x1.transpose(2,3,0,1)
# or
x2 = np.transpose(np.ones([2,3,4,5]), (2,3,0,1))

# .shape을 하면 배열이 몇곱하기 몇곱하기 몇인지 알 수 있다
print(x1.shape) # --> (4, 5, 2, 3)
print(x2.shape) # --> (4, 5, 2, 3)
```
numpy.transpose() 함수는 행렬의 Transpose(전치)를 구할때 쓰는 함수지만, 주로 다차원 행렬의 순서를 바꾸는데 많이 사용한다. ~~(아마도...)~~

사용법을 쉽게 설명하자면 위에 x1과 x2는 모두 (2,3,4,5) 크기의 행렬을 가지고 있다. 여기서 나는 (2,3,4,5)를 (**4**,**5**,**2**,**3**)순서의 행렬로 바꾸고 싶다면 다음과 같이 하면 된다.

* 기존의 배열 (x1이랑 x2)의 크기는 앞에서 부터 0,1,2,3의 index를 갖고 있다고 생각한다
* 그러면 index0 &rarr; **2**, index1 &rarr; **3**, index2 &rarr; **4**, index3 &rarr; **5** 가된다
* 다음으로 바꾸고 싶은 배열의 순서(여기서는 (**4**,**5**,**2**,**3**))에 따라 인덱스를 나열해 주면 된다
* (**4**,**5**,**2**,**3**) &rarr; (index2, index3, index0, index1) &rarr; (2,3,0,1)
* (2,3,0,1)을 numpy.transpose() 함수에 인자로 넣어주면 된다

이러한 과정을 거치면 다차원 배열을 여러분의 맘대로 순서를 바꿀 수 있다!

마지막으로 numpy.transpose()함수가 전치행렬을 뽑아내는 모습을 보고 마무리 하자.
```python
import numpy as np

x = np.array([[1  ,2  ],
              [10 ,20 ],
              [100,200]]) # x는 3x2 행렬이다. print(x.shape)을 하면 (3, 2)가 나온다.

x_transpose = x.transpose() # 여기서 x.transpose((1,0))을 사용해도 결과는 같다
                            # transpose()함수의 기본 정의 값은 (n, n-1, ... 1, 0) 값이기 때문이다
                            # n = (배열 x의 차원의 수 -1). 여기서는 n = 2 - 1 = 1

print(x_transpose) # --> [[1, 10, 100],
                   #      [2, 20, 200]]
                   # (2, 3) 행렬이 되어버렸다! 
```

가장 쉽게 이해하는 방법은 파이썬에서 transpose() 함수로 이것저것 해보는 것이다.