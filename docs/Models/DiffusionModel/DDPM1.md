---
layout: post
title: '[DDPM] Diffusion model에 대한 이해 1편'
parent: Diffusion Model
grand_parent: 딥러닝 모델
nav_order: 0
---
### DDPM에 관한 개인적인 이해
<br>
DDPM모델에 대해 간단히 설명하자면, 원본 이미지에 조금씩 노이즈를 줘서 결국 Normal Distribution을 만들고, 만들어진 Normal Distribution에 노이즈를 주었던 방식을 참고하여 다시 원래 이미지로 되돌리는 것이 기본 개념이다. 수식으로 설명하면 다음과 같다. <br>

* $$ q(x_t|x_{t-1}) : \text{ Forward diffusion process} $$ 
<br>
* $$ p(x_{t-1}|x_t) : \text{ Reverse diffusion process} $$
<br>
* $$ x_0 : \text{ 원본 이미지}$$
<br>
* $$ x_t : \text{ 노이즈를 t번 줬을때 이미지}$$
<br>
* $$ t : \text{ 노이즈를 준 횟수, Time step이라고도 한다}$$
<br>
* $$ T : \text{ 노이즈를 주는 총 횟수, T=1000이면 노이즈를 1000번 반복해서 준다는 의미이다}$$
<br>

따라서 **Forward diffusion process**의 의미는 $$x_{t-1}$$ 이미지일때 $$x_t$$ 이미지의 분포이다. 쉽게 말하면 전 단계의 이미지$$x_{t-1}$$로부터 노이즈를 줘서 $$x_t$$를 만드는 과정이라고 생각하면 된다. \| 기호가 확률에서 조건을 나타낼때 쓰는데, 결국 $$x_{t-1}$$일때 $$x_t$$일 분포라고도 볼 수 있다. **Reverse diffusion process**는 반대 과정이기 때문에 $$x_t$$와 $$x_{t-1}$$의 자리만 바뀐 것이다. 말로 풀어서 해석하면 $$x_t$$일때 $$x_{t-1}$$의 분포를 의미한다.
<br><br>
여기서 계속 **이미지의 분포** 라는 말을 사용하는데, 직관적인 이해를 돕자면 이미지 데이터는 보통 (채널수, 이미지 가로 크기, 이미지 세로 크기)의 벡터로 이루어진다. 보통 채널수는 컬러 이미지의 경우 RGB로 이루어져 있기 때문에 3이다. 따라서 32x32크기의 컬러 이미지는 (3,32,32)크기의 벡터 정보를 가지고 있고, 결과적으로 3x32x32=3072 개의 숫자값을 가지고 있을것이다. 바로 이 숫자값들의 분포를 **이미지의 분포** 라고 하는것이다. 
<br><br>
그렇다면 여기서 "그냥 이미지의 벡터값들을 정규화 해버리면(평균빼고 표준편차로 나눠버리면) Normal Distribution이 되는거 아닌가"라는 의문이 들 수 있다. 그러면 원본 이미지 $$x_0$$를 한번에 Normal Distribution으로 바꿀 수 있고, 평균이랑 표준편차만 학습하면 Normail Distribution에서 랜덤 샘플링을 하고 $$x_0 = m + z\sigma,\quad z\sim N(0,1)$$를 적용하면 $$x_0$$을 구할 수 있다.
<br><br>
그러면 굳이 왜 여러번 나눠서 (논문의 경우 t=1000) $$x_0$$를 정규화 하는 것일까? 이 질문의 대한 대답은 DDPM 논문 그 자체이다. 이사람들이 여러번 나눠서 정규화를 해봤는데, 많이 나눌수록 이미지를 생성하는 성능이 좋다고 했다. ~~(수학적인 이유는 나도 잘 모르겠다)~~
- - -
그러면 Forward diffusion process에서 어떤 과정을 거쳐서 $$N(0,1)$$인 Normal Distribution을 만드는지 알아보자. 여기서 중요한건 $$x_0$$를 어떤 방식으로든 Normal Distribution으로 만들면 된다는 것이다. 논문을 보면 아래의 수식을 여러번 적용해서 이미지 $$x_0$$를 Normal Distribution으로 만든다. 아래 수식들은 수학적 베이스에 의해 만들어진 것이겠지만, 일단은 이렇게 가정을 해놨다고만 생각하면 편하다.<br>

* $$ 0.0001 \leq \beta_t \leq 0.02, \quad (1\leq t\leq T, \: \text{here } T=1000), \quad  \beta_1 < \beta_2 < .. < \beta_T \text{ and Uniform Distribution}  $$ 
<br>
* $$ \alpha_t = 1 - \beta_t $$
<br>
* $$ \bar \alpha_t = \Pi_{i=1}^t \alpha_i, \quad \bar a_t = a_1a_2a_3...a_t$$
<br>
* $$ q(x_t|x_{t-1}) = N(x_t; \sqrt{\alpha_t}x_{t-1}, (1-\alpha_t)I)$$
<br>
* $$ x_t = \sqrt{\alpha_t}x_{t-1} + \sqrt{(1-\alpha_t)} \epsilon, \quad \epsilon \sim N(0,1)$$
<br>
* $$x_t \text{는} x_{t-1} \text{에 의해 구해지고, 이러한 연속성을 바탕으로 아래의 식이 도출된다}$$
<br>
* $$x_t = \sqrt{\bar \alpha_t}x_0 + \sqrt{(1-\bar \alpha_t)} \epsilon, \quad \epsilon \sim N(0,1)$$
<br>

결론적으로 $$x_t$$의 이미지 분포인 $$q(x_t \mid x_0)$$을 마지막 식에 의해 한번에 구할 수 있게 됬다. 자세한 증명은 직접 해보면 된다. 증명에서 조심해야 할 것은 $$\epsilon$$있는 항끼리의 덧셈인데 이건 가우시안의 PDF의 덧셈이 아니기때문에 "정규분포의 합"을 검색해서 나오는 방식으로 계산을 해야한다. $$A\epsilon + B\epsilon$$를 $$(A+B) \epsilon \text{로 계산하면 안된다는 말이다.}$$

그러면 결국 우리는 위에 과정을 거치면 $$x_0$$가 Normal Distribution이 되는지 알아볼 필요가 있다. 위에 식에 의해 마지막 이미지 $$x_T = \sqrt{\bar \alpha_T}x_0 + \sqrt{(1-\bar \alpha_T)} \epsilon$$이고 $$x_T$$의 분포는 $$N(x_T, \sqrt{\bar \alpha_T}x_0, (1-\bar \alpha_T)I)$$라고 볼 수 있다. 아래는 $$\bar \alpha_t$$의 값을 그린 코드와 그래프이다. 여기서 T는 1000이다.

```python
import numpy as np

T = 1000
beta_t = np.linspace(0.0001, 0.02, T)   # 주어진 간격을 T개로 나눈다
alpha_t = 1 - beta_t                    
alpha_bar_t = np.cumprod(alpha_t)       # 각각의 원소들의 누적 곱

print(alpha_bar_t[-1])                  # -> 4.035829765375676e-05

plt.plot(alpha_bar_t)
plt.title("alpha_bar_t")
plt.show()
```
![screenshot](..\images\153343.png)

$$\bar \alpha_t$$의 값이 1에서 0으로 가는 모습을 볼 수 있다. 그렇다면 결국 마지막 이미지 분포인 $$x_T$$는 $$N(0, I)$$이라고 볼 수 있다. (T=1000에서 $$\bar \alpha_t = 0$$이기 때문!). 하지만 $$\bar \alpha_T$$는 0에 가까운 수이기 때문에 완전한 Normal Distribution이라고는 볼 수 없다.

결론적으로 위에 제시된 방법으로 $$x_0$$에 노이즈를 더해주면, Normal Distribution인 $$x_T$$가 된다는 것을 알았다. 한번에 정규화를 하지 않고 재귀적인 방법으로 정규화를 하는 방법을 알아버린 것이다! 다음 편에서는 diffusion model이 어떤 것을 목표로 학습하는지 알아보고, 그에 따른 loss를 구해볼 것이다.