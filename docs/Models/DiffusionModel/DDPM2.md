---
layout: post
title: '[DDPM] Diffusion model에 대한 이해 2편'
parent: Diffusion Model
grand_parent: 딥러닝 모델
nav_order: 1
---
### DDPM Loss에 관한 개인적인 이해
<br>
저번 시간에 이어서 Diffusion Model의 Loss 값에 대해 알아보자. 대부분 딥러닝 모델들은 각각의 모델에서 최소화 또는 최대화시키고 싶어하는 object function이 존재한다. DDPM에 경우 $$p_\theta(x_0)$$를 구하는 것이 최종 목표이다. 저 수식의 의미는 $$x_0$$, 즉 원본데이터를 가장 잘 나타내는 distribution을 구하고 싶다는 것이다. 여기서 $$p_\theta(x)$$는 데이터 $$x$$들에 대한 확률밀도함수라고 생각하면 된다. MLE(최대가능도추정)의 방법을 생각해보면, 데이터를 가장 잘 나타내는 분포는 각각의 데이터 값들의 likelihood(가능도)를 모두 곱하였을때, 가장 큰 값이 나오는 분포가 그 데이터를 가장 잘 나타낸다고 한다. 이와 비슷한 논리로 $$p_\theta(x)$$에 데이터 $$x_0$$를 넣었을때, 그 값이 가장 크다면 우리가 학습시에 넣은 $$x_0$$의 분포를 잘 나타냈다고 말할 수 있다. 조금 더 직관적으로 말하면, $$x_0$$의 RGB 벡터 값들을 계산해서 가우시안분포를 구할 수 있는데, $$p_\theta(x)$$가 $$x_0$$의 가우시안 분포와 비슷한 평균과 분산을 가지고 있는 가우시안 분포라면, 각각 RGB벡터들을 $$p_\theta(x)$$에 넣어서 나온 값들의 곱이 최대치와 비슷하게 된다는 것이다. (최대치가 되려면 $$x_0$$의 가우시안 분포와 완전히 같은 평균과 분산을 가지면 된다). 
- - -
이제 우리는 $$p_\theta(x_0)$$를 매 학습마다 최대화 해주는 방향으로 diffusion model을 학습해야 한다는 사실을 알았다. 하지만 실제 모델에서는 앞에 $$-\log$$를 붙여서 $$-\log(p_\theta(x_0))$$를 최소화 하는 방향으로 모델을 설계한다. ($$-\log(x)$$는 감소함수이기 때문에 최소화 할수록 $$x$$의 값은 커진다). 이제 아래 식을 살펴보자.

$$
\begin{align}
    -\log (p_\theta(x_0)) &\leq -\log (p_\theta(x_0)) + D_{KL}(q(x_{1:T}|x_0)||p_\theta(x_{1:T}|x_0)) &\quad \cdots \quad (1) \\
    &= -\log (p_\theta(x_0)) + \mathbb{E}_{x_{1:T} \sim q(x_{1:T}|x_0)} \left[ \log \frac{q(x_{1:T}|x_0)}{p_\theta(x_{1:T}|x_0)} \right] &\quad \cdots \quad (2) \\
    &= -\log (p_\theta(x_0)) + \mathbb{E}_{x_{1:T} \sim q(x_{1:T}|x_0)} \left[ \log \frac{q(x_{1:T}|x_0)}{p_\theta(x_{1:T}, x_0)/p_\theta(x_0)} \right] &\quad \cdots \quad (3) \\
    &= -\log (p_\theta(x_0)) + \mathbb{E}_{x_{1:T} \sim q(x_{1:T}|x_0)} \left[ \log \frac{q(x_{1:T}|x_0)}{p_\theta(x_{0:T})} + \log (p_\theta(x_0)) \right] &\quad \cdots \quad (4) \\
    &= -\log (p_\theta(x_0)) + \mathbb{E}_{x_{1:T} \sim q(x_{1:T}|x_0)} \left[ \log \frac{q(x_{1:T}|x_0)}{p_\theta(x_{0:T})} \right] + \log (p_\theta(x_0)) &\quad \cdots \quad (5) \\
    &= \mathbb{E}_{x_{1:T} \sim q(x_{1:T}|x_0)} \left[ \log \frac{q(x_{1:T}|x_0)}{p_\theta(x_{0:T})} \right] &\quad \cdots \quad (6)
\end{align}
$$

위의 식은 diffusion model의 object function인 $$-\log(p_\theta(x_0))$$를 정리한 식이다. 
* (1) : 뒤에 $$D_{KL}$$항이 붙고 부등호가 생겼다. 여기서 $$D_{KL}$$은 **쿨백-라이블러 발산(Kullback–Leibler divergence)**인데, 정말 간단히 설명하자면 두 분포의 엔트로피 차이라고 생각하면 된다. 또한 $$D_{KL}$$은 항상 0보다 큰 특징을 가지고 있다. 따라서 식의 부등호가 성립하는 것이다. (쿨백-라이블러 발산을 구글에 검색해서 알아보면 더 재밌어요). 그렇다면 갑자기 $$D_{KL}$$를 더한 이유는 우리가 계산가능한 식을 만들기 위해 식 변형을 했다고 생각하면된다. 어차피 $$-\log(p_\theta(x_0))$$보다 크거나 같은 값을 최소화하는 것은 $$-\log(p_\theta(x_0))$$를 최소화 하는 것과 같기 때문이다. 한마디로 하자면 upper bound이기 때문이다.

* (2) : 쿨백-라이블러 발산식은 $$D_{KL}(q \mid p_{\theta}) = \int_{-\infty}^{+\infty} q(x_{1:T} \mid x_0) \log \frac{q(x_{1:T} \mid x_0)}{p_\theta(x_{1:T} \mid x_0)} \; dx_{1:T}$$로 풀어쓸 수 있는데, 이걸 다시 평균의 관점에서 식을 정리한 것이다. 확률 분포가 $$x_{1:T} \sim q(x_{1:T} \mid x_0)$$를 따를때, $$\log \frac{q(x_{1:T} \mid x_0)}{p_\theta(x_{1:T} \mid x_0)}$$의 기댓값을 구한 것이다.

* (3) : $$P(A \mid B) = P(A,B)/P(B)$$ 공식을 이용해서 $$p_\theta(x_{1:T} \mid x_0)$$를 분리한 것이다

* (4) : $$P(x_{1:T})$$의 정의는 $$P(x_1,x_2, \dots, x_T)$$이기 때문에 $$P(x_{1:T}, x_0)=P(x_{0:T})$$이 성립한다. (아마 논문에서 만든 정의로 기억한다. 아닐수도 있어요~). 뒤에 $$\log(p_{\theta}(x_0))$$는 분모에 있던 항을 분리한 것이다.

* (5) : 뒤에 있던 $$\log(p_{\theta}(x_0))$$항이 $$\mathbb{E}$$를 벗고 나와있다. 왜냐하면 $$\log(p_{\theta}(x_0))$$는 상수이기 때문이다. 우리가 아직 $$p_{\theta}(x)$$가 어떤 확률 밀도 함수인지는 모르지만, 여기에 $$x_0$$를 넣으면 likelihood가 나온다는 사실은 이미 알고있다.($$x_0$$는 처음 넣었던 이미지의 벡터값이다.) 따라서 상수의 평균은 확률 분포에 상관없이 상수값이 나오니까 $$\log(p_{\theta}(x_0))$$를 그대로 뺄 수 있다.

* (6) : 앞뒤에 $$\log(p_{\theta}(x_0))$$를 제거하면 우리가 계산할 수 있는 항이 남는다!

혹시나 해서 말하지만, 1편에서 나온 $$q$$와 $$p$$의 함수는 위의 식에서 $$q$$와 $$p_\theta$$와 같다. $$\theta$$는 우리가 찾아야 할 파라미터라는 의미로 $$p$$에 붙인것이다. $$q$$는 forward diffusion process이고 $$p_\theta$$는 reverse diffusion process라는 점을 잊지 말자.~~(수식이 길어서 저는 가끔 까먹습니다).~~
- - -
여기까지 우리는 diffusion model의 object function을 찾고, object function자체를 직접 계산할 수 없어서 특별한 변형을 거처 upper bound인 식 $$\mathbb{E}_{x_{1:T} \sim q(x_{1:T}|x_0)} \left[ \log \frac{q(x_{1:T}|x_0)}{p_\theta(x_{0:T})} \right]$$를 찾아냈다. 이 식은 당연히 계산이 되니까 구했을거고, 다음 편에서 어떻게 계산하는지 알아볼 것이다. 추가적으로 $$p_\theta(x_0)$$가 계산이 안되는 이유는 우리는 $$p(x_0 \mid x_t)$$나 $$q(x_t \mid x_0)$$ 같은 식들은 1편에서 정의했던 식들로 계산이 되지만, $$p(x_0)$$ 같이 조건이 없는 식은 우리가 구하는 것이 불가능 하다. 왜냐하면 정의한 식으로부터 유도가 불가능 하기 때문이다. 예를 들어 $$q(x_t \mid x_0)$$ 식은 우리가 1편에서 이쁘게 정리해서 $$x_t = \sqrt{\bar \alpha_t}x_0 + \sqrt{(1-\bar \alpha_t)} \epsilon, \quad \epsilon \sim N(0,1)$$라는 것을 알았지만 여기서 $$q(x_0)$$이나 $$q(x_t)$$를 구할 수는 없다. 식 자체가 conditional한 성격을 가지기 때문이다.(현재값을 구하기 위해 과거의 출력값을 사용하는 성격). 그럼 이만~

