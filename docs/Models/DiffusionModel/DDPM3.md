---
layout: post
title: '[DDPM] Diffusion model에 대한 이해 3편'
parent: Diffusion Model
grand_parent: 딥러닝 모델
nav_order: 2
---
### DDPM Loss에 관한 개인적인 이해
<br>
저번시간에 우리가 최소화 시켜야 하는 식을 구했다. 원래의 목적 함수(object function)는 $$-\log(p_\theta(x_0))$$이지만, 이는 우리가 직접 계산할 수 없는 식이여서 2편에서는 이 식의 upper bound를 구하고 우리가 계산 가능한 식의 형태로 바꿨다. 따라서 최종적으로 나온 식은 아래와 같다.

$$
\begin{align}
    -\log(p_\theta(x_0)) \leq \mathbb{E}_{x_{1:T} \sim q(x_{1:T}|x_0)} \left[ \log \frac{q(x_{1:T}|x_0)}{p_\theta(x_{0:T})} \right]
\end{align}
$$

논문에서는 이 식 양변에 $$\mathbb{E}_{x_0 \sim q(x_0)}$$를 해줘서 더 모양이 예쁜 식을 만든다. $$q(x_0)$$의 분포를 따르는 평균을 각각 계산하는 것이라고 볼 수 있다.

$$
\begin{align}
    \mathbb{E}_{x_0 \sim q(x_0)} \left[ -\log(p_\theta(x_0)) \right] &\leq  \mathbb{E}_{x_0 \sim q(x_0)} \left[ \mathbb{E}_{x_{1:T} \sim q(x_{1:T}|x_0)} \left[ \log \frac{q(x_{1:T}|x_0)}{p_\theta(x_{0:T})} \right] \right] \\
    &= \mathbb{E}_{x_{0:T} \sim q(x_{0:T}|x_0)} \left[ \log \frac{q(x_{1:T}|x_0)}{p_\theta(x_{0:T})} \right] \\
    &= \mathbb{E}_{q} \left[ \log \frac{q(x_{1:T}|x_0)}{p_\theta(x_{0:T})} \right]
\end{align}
$$

이처럼 양변에 같은 평균을 걸워줘도 부등호는 유지가 된다. 간단히 설명하면, 확률 계산식을 생각하면 (확률)X(값)의 합인데, 확률은 항상 양수가 나오니까 결국 양변에 같은 분포의 $$\mathbb{E}$$를 씌우는 것은 양변에 양수를 곱했다고 생각하면 된다. 마지막식은 편의를 위해 $$x_{0:T} \sim q(x_{0:T} \mid x_0) \rightarrow q$$로 나타내었다.

 다시 돌아와서, 우리가 object function의 upper bound를 구한 이유는 object function을 최소화 하는 과정을 object function의 upper bound를 최소화 하는 과정으로 대체해서 모델을 학습시키기 위해서이다. 모델의 object function을 직접 최소화 시키면서 학습하는것이 가장 좋지만, 우리는 $$\mathbb{E}_{x_0 \sim q(x_0)} \left[ -\log(p_\theta(x_0)) \right]$$를 직접 계산하지 못하기 때문에 계산가능한 upper bound를 최소화 시키는 것이라고 이해하면 된다.

지금부터는 object function의 upper bound 식을 예쁘게 분해해서 간단한 식으로 바꿀 것이다. 일단 수식을 전개하고 저번처럼 하나하나 설명을 하겠다.

$$
\begin{align}
    & \mathbb{E}_{q} \left[ \log \frac{q(x_{1:T}|x_0)}{p_\theta(x_{0:T})} \right] \\ 
    &= \mathbb{E}_{q} \left[ \log \frac{\Pi_{t=1}^{T}q(x_t|x_{t-1})}{p_\theta(x_T)\Pi_{t=1}^{T}p_\theta(x_{t-1}|x_{t})} \right] &\quad \cdots \quad (1) \\
    &= \mathbb{E}_{q} \left[ -\log p_\theta(x_T) + \Sigma_{t=1}^{T} \log \frac{q(x_t|x_{t-1})}{p_\theta(x_{t-1}|x_{t})} \right] &\quad \cdots \quad (2) \\
    &= \mathbb{E}_{q} \left[ -\log p_\theta(x_T) + \Sigma_{t=2}^{T} \log \frac{q(x_t|x_{t-1})}{p_\theta(x_{t-1}|x_{t})} + \log \frac{q(x_1|x_0)}{p_\theta(x_0|x_1)} \right] &\quad \cdots \quad (3) \\
    &= \mathbb{E}_{q} \left[ -\log p_\theta(x_T) + \Sigma_{t=2}^{T} \log \frac{q(x_{t-1}|x_t, x_0)}{p_\theta(x_{t-1}|x_{t})} \cdot \frac{q(x_t|x_0)}{q(x_{t-1}|x_0)} + \log \frac{q(x_1|x_0)}{p_\theta(x_0|x_1)} \right] &\quad \cdots \quad (4) \\
    &= \mathbb{E}_{q} \left[ -\log p_\theta(x_T) + \Sigma_{t=2}^{T} \log \frac{q(x_{t-1}|x_t, x_0)}{p_\theta(x_{t-1}|x_{t})} + \Sigma_{t=2}^{T} \log \frac{q(x_t|x_0)}{q(x_{t-1}|x_0)} + \log \frac{q(x_1|x_0)}{p_\theta(x_0|x_1)} \right] &\quad \cdots \quad (5) \\
    &= \mathbb{E}_{q} \left[ -\log p_\theta(x_T) + \Sigma_{t=2}^{T} \log \frac{q(x_{t-1}|x_t, x_0)}{p_\theta(x_{t-1}|x_{t})} + \log \frac{q(x_T|x_0)}{q(x_1|x_0)} + \log \frac{q(x_1|x_0)}{p_\theta(x_0|x_1)} \right] &\quad \cdots \quad (6) \\
    &= \mathbb{E}_{q} \left[ \log \frac{q(x_T|x_0)}{p_\theta(x_T)} + \Sigma_{t=2}^{T} \log \frac{q(x_{t-1}|x_t, x_0)}{p_\theta(x_{t-1}|x_{t})} -\log p_\theta(x_0|x_1) \right] &\quad \cdots \quad (7) \\
    &= \mathbb{E}_{q} \left[ D_{KL}(q(x_T|x_0) || p_\theta(x_T)) + \Sigma_{t=2}^{T} D_{KL}(q(x_{t-1}|x_t, x_0) || p_\theta(x_{t-1}|x_{t})) -\log p_\theta(x_0|x_1) \right] &\quad \cdots \quad (8)
\end{align}
$$

* (1) : $$q \text{와} \ p_\theta$$는 1편에서 현재 분포가 바로 이전 분포에 의해 결정되는 분포라고 했다. 이러한 분포를 Markov Chain이라고 따로 부르기도 하는데, Markov Chain은 다음과 같은 성질을 가지고 있다. <br><br>
$$
\begin{align}
    &q(x_t|x_{t-1}) = q(x_t|x_{t-1}, x_{t-2}) = q(x_t|x_{t-1}, x_{t-2}, \ ... \ x_0) \\
    &p_\theta(x_0|x_1) = p_\theta(x_0|x_1, x_2) = p_\theta(x_0|x_1, x_2, \ ... \ x_t)
\end{align}
$$ <br><br>
쉽게 설명하자면, Markov Chain을 만족하는 분포는 항상 이전의 값에 의해서만 결정되니까 어떤 다른 조건들을 넣어도 결과값에는 변화가 없다고 생각하면 된다. 이러한 성질을 이용하면 다음과 같은 식전개가 가능하다. <br><br>
$$
\begin{align}
    &q(x_t|x_{t-1}) = q(x_t|x_{t-1}, x_{t-2}) = q(x_t|x_{t-1}, x_{t-2}, \ ... \ x_0) \\
    &p_\theta(x_0|x_1) = p_\theta(x_0|x_1, x_2) = p_\theta(x_0|x_1, x_2, \ ... \ x_t)
\end{align}
$$ <br><br>
