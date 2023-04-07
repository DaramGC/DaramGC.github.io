---
layout: post
title: 베타 분포와 디리클레 분포 하이퍼파라미터값 (Beta distribution and Dirichlet distribution hyperparameter)
parent: 확률과 통계
---
### 베타 분포(Beta distribution) 확률 밀도 함수 (PDF)는 다음과 같이 표현된다 <br><br>
$$ 
    f(x)=\frac{1}{B(\alpha,\beta)}x^{\alpha-1}(1-x)^{\beta-1}
$$
- - -
### 디리클레 분포(Dirichlet distribution) 확률 밀도 함수(PDF)는 다음과 같이 표현된다 <br><br> 
$$ 
    f(x)=\frac{1}{B(\alpha)}\Pi_{i=1}^{k}x_i^{\alpha_i-1}
$$
- - -
중요한건, 베타분포의 하이퍼파라미터인 $$\alpha$$와 $$\beta$$값은 실제 A와 B가 일어난 횟수+1 이라는 부분이다. 예를 들면 A라는 사건이 10번 일어났고, B라는 사건이 7번 일어났다면, $$\alpha$$와 $$\beta$$는 각각 11과 8이 된다. <br>

> $$\alpha - 1 = (사건 A가 일어난 횟수) = 10$$ <br>
> $$\beta - 1 = (사건 B가 일어난 횟수) = 7$$ <br>

$$\alpha=11$$이고 $$\beta=8$$일때 위 식을 만족한다 <br> 
- - -
디리클레 분포도 위의 베타 분포와 마찬가지로 하이퍼파라미터 $$\alpha_i$$의 값은 사건 $$X_i$$가 일어난 횟수+1 이다. <br>

> $$\alpha_i - 1 = (사건 X_i가 일어난 횟수)$$ <br>

|$$X_i, (k=3)$$|$$\alpha_i, (k=3)$$|
|:---:|:---:|:---:|
|$$X_1 = 4$$|$$\alpha_1 = 5$$|
|$$X_2 = 7$$|$$\alpha_2 = 8$$|
|$$X_3 = 3$$|$$\alpha_3 = 4$$|

- - -
베타분포의 경우 하이퍼파라미터에 따라 그려지는 그래프 모양이 다른데 헷갈리지 않도록 하자! (물론 디리클레 분포도 하이퍼파라미터에 따라 그려지는 그래프 모양이 다릅니다~)


