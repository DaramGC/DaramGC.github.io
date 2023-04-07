---
layout: post
title: '[Python] 파이썬에서 String을 인자로 넘길때 발생한 문제'
parent: 제목미지정
---
### 파이썬에서 String Argument를 넘길때 생긴 일
<br>
주피터 노트북(jupyter notebook)에서 CIFAR-10 데이터 셋을 불러올때 생긴 문제이다.
아래 **def unpickle(file):** 함수는 String문자열의 경로를 받아 컴퓨터에 저장된 data파일을 코드로 불러오는 함수이다. 

```python
def unpickle(file):
    with open(file, 'rb') as f:
        dict = pickle.load(f, encoding='bytes')
        data = dict[b'data'].reshape((-1, 3, 32, 32)).transpose(0, 2, 3, 1).astype(np.float64) / 255.
        print(file.split("\\")[-1] + f': {data.shape}') #어떤 파일이 저장되었는지 출력
    return data

train1 = unpickle("Data\cifar-10-batches-py\data_batch_1")
train2 = unpickle("Data\cifar-10-batches-py\data_batch_2")
train3 = unpickle("Data\cifar-10-batches-py\data_batch_3")
train4 = unpickle("Data\cifar-10-batches-py\data_batch_4")
train5 = unpickle("Data\cifar-10-batches-py\data_batch_5")
test = unpickle("Data\cifar-10-batches-py\test_batch")
```

위의 코드를 실행시키면 아래와 같이 나온다. "Data\\\cifar-10-batches-py\test_batch"라는 경로에 파일이 없다고 한다. 

```
OSError                                   Traceback (most recent call last)
Input In [65], in <cell line: 13>()
     11 train4 = unpickle("Data\cifar-10-batches-py\data_batch_4")
     12 train5 = unpickle("Data\cifar-10-batches-py\data_batch_5")
---> 13 test = unpickle("Data\cifar-10-batches-py\test_batch")

Input In [65], in unpickle(file)
      1 def unpickle(file):
----> 2     with open(file, 'rb') as f:
      3         dict = pickle.load(f, encoding='bytes')
      4         data = dict[b'data'].reshape((-1, 3, 32, 32)).transpose(0, 2, 3, 1).astype(np.float64) / 255.

OSError: [Errno 22] Invalid argument: 'Data\\cifar-10-batches-py\test_batch'
```

경로도 내가쓴 경로가 아니고 조금 다른 String이 경로로 나와서 print() 함수로 한번 출력해보았다

```python
print("Data\cifar-10-batches-py\test_batch")
--> Data\cifar-10-batches-py	est_batch
```

중간에 ...-py\test_... 부분에서 \t를 [TAB]으로 인식하는 것 같다. ~~(레전드 ㄷㄷ)~~ 주피터 노트북이 이상한건지 내가 설치한 파이썬이 이상한건지 모르겠는데 경로 앞에 escape문자 \를 하나 더 붙였다.

```python
print("Data\cifar-10-batches-py\\test_batch")
--> Data\cifar-10-batches-py\test_batch
```

이걸로 고쳐서 코드를 돌리니까 잘 됬다.

```python
def unpickle(file):
    with open(file, 'rb') as f:
        dict = pickle.load(f, encoding='bytes')
        data = dict[b'data'].reshape((-1, 3, 32, 32)).transpose(0, 2, 3, 1).astype(np.float64) / 255.
        print(file.split("\\")[-1] + f': {data.shape}') #어떤 파일이 저장되었는지 출력
    return data

train1 = unpickle("Data\cifar-10-batches-py\data_batch_1")
train2 = unpickle("Data\cifar-10-batches-py\data_batch_2")
train3 = unpickle("Data\cifar-10-batches-py\data_batch_3")
train4 = unpickle("Data\cifar-10-batches-py\data_batch_4")
train5 = unpickle("Data\cifar-10-batches-py\data_batch_5")
test = unpickle("Data\cifar-10-batches-py\\test_batch")
```

결과

```
data_batch_1: (10000, 32, 32, 3)
data_batch_2: (10000, 32, 32, 3)
data_batch_3: (10000, 32, 32, 3)
data_batch_4: (10000, 32, 32, 3)
data_batch_5: (10000, 32, 32, 3)
test_batch: (10000, 32, 32, 3)
```

다른 파이썬 실행환경에서도 이런 현상이 발생하는지는 나중에 알아봐야겠다.