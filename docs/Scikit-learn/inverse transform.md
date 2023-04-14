---
layout: post
title: '[Scikit-learn] 사이킷런에서 inverse_transform() 함수'
parent: Scikit-learn 사이킷런
---
### 사이킷런 inverse_transform() 함수에 대해 알아보자
<br>
WGAN을 구현하는 도중에 이미지 데이터를 Normal Distribution으로 정규화 할 일이 생겼다. 아래와 같이 StandardScaler를 사용하여 이미지 데이터를 Normalization했다.

```python
from sklearn.preprocessing import StandardScaler

print(train.shape)      # --> (50000, 32, 32, 3)
print(train.max())      # --> 1.0
print(train.min())      # --> 0.0

ss = StandardScaler()
# 여기서 reshape으로 데이터를 폈다가 접은 이유는 fit_transform 함수를 사용하기 위해서다
train_set = ss.fit_transform(train.reshape(-1,1)).reshape(-1,32,32,3)

print(train_set.shape)  # --> (50000, 32, 32, 3)
print(train_set.max())  # --> 2.0934103819959557
print(train_set.min())  # --> -1.8816433721538957
```

그러다가 문득 Normal Distribution의 데이터를 다시 Image데이터로 바꾸는 방법이 궁금해졌다. 왜냐하면 Input이 Normal Distribution이면 모델의 Output도 Normal Distribution일 텐데, 결국 우리가 출력할 수 있는 이미지 벡터인 0 ~ 1이나 0 ~ 255로 만들어야 하기 때문이다. 그래서 Normalize를 할때 사용하는 정규화 식을 거꾸로 이용해서 한번 시도해 보았다.

```python
# Z = (X-m)/a --> Z는 정규분포, X는 기존분포, m은 X의 평균, a는 X의 표준편차
# X = m + Z*a

# ss.mean_과 ss.var_은 StandardScaler가 Normalization을 할때 사용한 평균과 분산값이다
new_image = ss.mean_ + train_set*(ss.var_**0.5)
print(new_image.max())      # --> 1.0
print(new_image.min())      # --> 0.0
```

잘 되는 것 같다. 조금 더 찾아보니까 이 과정을 한번에 해주는 StandardScaler 내장 함수를 찾게 되었다. 바로 **inverse_transform()** 함수인데 하는 역할은 위에서 내가 코드로 구현한 역할과 같다.

```python
# 여기서도 reshape으로 데이터를 폈다가 접은 이유는 inverse_transform 함수를 사용하기 위해서다
new_image2 = ss.inverse_transform(train_set.reshape(-1,1)).reshape(-1,32,32,3)
print(new_image2.max())      # --> 1.0
print(new_image2.min())      # --> 0.0
```

지금까지 **new_image**와 **new_image1**의 max(), min() 값으로 잘 변형됬는지 확인했는데, 실제로 이미지도 잘 복원됬는지 확인해보도록 하겠다.

```python
import matplotlib.pyplot as plt

fig = plt.figure()
ax1 = fig.add_subplot(1,3,1)
ax1.imshow(train[0])
ax1.set_title("train[0]")
ax1.axis("off")

ax2 = fig.add_subplot(1,3,2)
ax2.imshow(new_image[0])
ax2.set_title("new_image[0]")
ax2.axis("off")

ax3 = fig.add_subplot(1,3,3)
ax3.imshow(new_image2[0])
ax3.set_title("new_image2[0]")
ax3.axis("off")
```

아래에 귀여운 개구리들을 볼 수 있다!

![screenshot](..\images\화면 캡처 2023-04-14 115209.png)