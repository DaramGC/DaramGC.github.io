---
layout: post
title: '[Pytorch] 파이토치에서 datasets 다운로드와 transforms 인자'
parent: Pytorch 파이토치
---
### 파이토치 datasets 다운로드 후 transform 인자 적용
<br>
파이토치에서 CIFAR10 데이터셋을 받고 -1 ~ 1 사이로 normalize 하려고 했을 때 생긴 일이다. 나는 인터넷을 참고하여 아래와 같이 코드를 짰다.

```python
import torch
import torch.utils.data
import torchvision
import torchvision.transforms as transforms
import torchvision.transforms.functional

# -1 ~ 1 사이로 normalize 하기 위한 transforms 생성
transform = transforms.Compose([transforms.ToTensor(), 
                                transforms.Normalize((0.5,0.5,0.5), (0.5,0.5,0.5))])

# training data set down
train_data = torchvision.datasets.CIFAR10(root='./data', train=True,
                                         download=True, transform=transform)
train_loader = torch.utils.data.DataLoader(train_data, batch_size=128,
                                          shuffle=True, num_workers=2)

# test data set down
test_data = torchvision.datasets.CIFAR10(root='./data', train=False,
                                         download=True, transform=transform)
test_loader = torch.utils.data.DataLoader(test_data, batch_size=128,
                                          shuffle=False, num_workers=2)
```

그리고 평소와 같이 데이터 셋의 크기를 확인하고, -1 ~ 1 사이로 잘 normalize 되었는지 확인했다.

```python
print(train_data.data.shape)            # --> (50000, 32, 32, 3)
print(test_data.data.shape)             # --> (10000, 32, 32, 3)

print(train_data.data[125].max())       # --> 255
print(train_data.data[125].min())       # --> 4
```

여기서 부터 뭔가 이상했다. **train_data**를 선언한 줄에서 인자로 **transform**을 넣어줘서, 당연히 **train_data**가 -1 ~ 1 사이의 값으로 나올 줄 알았다. 근데 기존 0 ~ 255 사이의 값이 나와버린 것이다! 고민하다가 "아 혹시 **train_loader**선언 할 때 적용되나?" 라는 생각이 들어서 아래의 코드를 짜봤다.

```python
print(train_loader.dataset.data.shape)      # --> (50000, 32, 32, 3)

print(train_loader.dataset.data[125].max()) # --> 255
print(train_loader.dataset.data[125].min()) # --> 4
```

**train_loader**도 -1 ~ 1 이 아닌 0 ~ 255 의 값을 뱉는다. 뭔가 이상하다~~(이상해이상해살려줘)~~. 내가 인터넷에서 잘못된 코드를 본 줄 알고 구글에 "파이토치 CIFAR10 데이터셋 다운로드"를 계속 검색해 봤는데 다 비슷하게 구현해 놓았다. 결국에는 구글 "pytorch transform did not work"를 검색해 봤다. 근데 나같은 사람이 한둘이 아니였는지 외국 사이트에 비슷한 질문이 있어 들어가봤다. 결론은 **train_loader**를 iterator로 부를 때만 데이터 셋에 **transform**이 적용된다고 한다. 바로 해보았다.

```python
i = iter(train_loader)  # --> iterator 가져오기
x, label = next(i)      # --> iterator의 next 출력

#위에 DataLoader에서 batch_size를 128로 해서, 128크기의 Tesor들이 나온다!
print(x.shape)          # --> torch.Size([128, 3, 32, 32])
print(label.shape)      # --> torch.Size([128])

print(x.max())          # --> tensor(1.)
print(x.min())          # --> tensor(-1.)
```

오우 잘나온다. 이래서 학습할때 enumerator()만 쓰는 이유를 알았다. 나는 평상시에 "그냥 **train_loader.dataset.data**를 for문 뺑뺑이 돌리면 안되나?" 라는 궁금증이 있었는데 이번에 왜 그런지 알아서 기분이 좋다. 기분이 좋아진 김에 **train_data**도 iterator로 호출하면 되는지 실험하기로 했다.

```python
i = iter(train_data)    # --> iterator 가져오기
x, label = next(i)      # --> iterator의 next 출력

print(x.shape)          # --> torch.Size([3, 32, 32])
print(label)            # --> 6

print(x.max())          # --> tensor(1.)
print(x.min())          # --> tensor(-1.)
```
어...음...진짜 될줄은 몰랐는데... 아무튼 **transform**이나 다른 인자들은 iterator로 호출시에만 적용된다는게 중요하다. 잊지 말도록 하자! 