---
{"dg-publish":true,"permalink":"/== Study/cs224n(COSE461)/01 Word Vectors/","created":"2025-03-13T21:14:27.138+09:00","updated":"2025-04-04T09:08:17.870+09:00"}
---

Lecture 1: Introduction and Word Vectors
Human language and word meaning
Word2vec introduction
Word2vec objective function gradients
Optimization basics
다음 강의: [[== Study/cs224n(COSE461)/02 GloVe\|02 GloVe]]

---

언어란 단순한 커뮤니케이션이 아니다.
더 높은 수준의 사고를 가능케 하는 도구이다.

# Represent the meaning of a word

컴퓨터가 자연어를 어떻게 처리하는가에 앞서, 먼저 인간이 어떻게 언어를 다루는지에 대해 알아보자.

우리는 단어의 의미를 어떻게 표현할 수 있을까?
의미에 대한 가장 일반적인 사고방식은 아래의 그림처럼 tree 라는 단어는 가지각색의 실재 형상들에 대한 표현이라고 보는 것이다.

![Screenshot 2025-03-14 at 7.38.22 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-14%20at%207.38.22%20PM.png)

([[= My Brain/본연의 사건/단어와 일반화\|단어와 일반화]]에서 더 깊게 다루어 보았고, 더 발전된 형태에 대한 초기 아이디어를 적어놓았지만 차치하고..)

이처럼 외연적 의미론에 따르면 언어는 심볼과 의미의 결합으로 이루어지며, 단어란 어떤 심볼을 가리킨다.
당연하게도 컴퓨터가 단어를 다루기 위해서는 심볼이 계산가능한 숫자로 표현되어야 한다.
따라서 one-hot vector가 초기에 사용되었다.
![Screenshot 2025-03-14 at 9.21.24 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-14%20at%209.21.24%20PM.png)

이런 경우 Vector의 차원은 사전에 존재하는 단어의 수와 같다. 대략, 500,000개 이상이다.
문제는 이러한 표현이 단어의 의미를 전혀 담아내지 못한다는 것이다. motel과 hotel은 의미적으로 매우 유사하지만, 수학적으로 두 벡터는 직교orthogonal이다. 완전히 독립적인 이산 기호로서의 역할밖에 못한다는 것이다.

따라서 의미를 담아낼 다른 방법을 고안하게 되었다.
[[== Study/Data Processing and Analysis/Vector Semantics\|Vector Semantics]]에서 여러가지 형태의 공기어 벡터를 다루어 보았지만, 무언가를 하나 깊게 다뤄보진 않았었다. 지금부터 조금 더 깊게 다뤄볼 예정이다.

# Word vectors
*"You shall know a word by the company it keeps"*, J.R. Firth 1957:11.
퍼스에 따르면 단어의 의미는 독립적으로 존재하는 것이 아니라 주변에 함께 쓰이는 단어들과의 관계(context) 속에서 결정된다.

이러한 아이디어에 착안해서 비슷한 맥락에서 사용되는 단어는 서로 유사성을 띠도록 단어의 체계를 구성하였다.
간단한 예시로 아래의 그림처럼 두 개의 단어 banking과 monetary를 이처럼 표현할 수 있다. 이 경우에 두 벡터의 내적을 통해 유의성이 표현된다. 각 차원의 부호가 같은 경우 유사성이 커지는 식으로 말이다.
![Screenshot 2025-03-14 at 9.35.49 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-14%20at%209.35.49%20PM.png)

이제 문제는 각 벡터의 차원값, 즉 파라미터들을 어떻게 학습할 수 있는가? 이다.

# Word2vec: skip-gram
Word2vec은 단어를 벡터화시키기 위한 학습 프레임 워크이다.
Skip-gram은 Word2vec 기법 중 하나로, 주어진 중심단어(center word)로부터 주변부 단어(outside word, context word)를 예측하는 신경망 기반 학습 모델이다.
이 모델은 맥락을 학습하기 위함이다. 비슷한 맥락을 가진 단어는 함께 등장할 것이라는 의미이다.

![Screenshot 2025-03-14 at 9.59.24 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-14%20at%209.59.24%20PM.png)

중심 단어 $w_t$가 주어졌을 때, 주변부 단어 $w_{t+j}, (j \neq 0)$가 등장하는 빈도에 따라 확률 가중치를 업데이트 하는 것이 핵심이다. 확률 가중치가 정확해질 수록 우리는 어떠한 맥락 속에서 어떠한 단어가 등장해야 하는지를 점점 더 잘 알게된다.

결국 우리는 더 정확한 $P(w_{t+j} \mid w_t)$를 알고 싶은 것이다.

그렇기 위해서는 파라미터를 최대한 정확하게 구해야 한다.
이때 파라미터 학습은 [[== Study/Deep Learning/Gradient Descent\|Gradient Descent]]을 이용한다.

## Gradient Descent
경사하강법을 사용하기 위해서는 우린 예측이 어긋날 경우 비용이 커지게 되는 함수를 정의해줘야 한다.
따라서 cost funtion $J(\theta)$를 정의한 후, 해당 함수의 최솟값을 추정해 나가면 된다.

![Pasted image 20250317165132.png](/img/user/z-Attached%20Files/Pasted%20image%2020250317165132.png)

그라디언트 디센트 알고리즘을 통해 파라미터($\theta$)를 추정하는 골자는 아래와 같다.
이때, $\theta$는 존재하는 모든 파라미터를 가리킨다.
$$\theta \leftarrow \theta - \alpha \nabla J(\theta)$$
그렇다면, $J(\theta)$는 어떻게 정의해야 할까?

## 정합한 문장과 맥락적 의미를 반영하기 위한 Likelihood Ftn
일단 우리가 [[= My Brain/토막생각/글을 읽고 이해한다는 것\|원하는 글]]이 무엇인지에 대해 먼저 생각해야 한다. **우리는 단지 짧은 토막 문장만 만들어내는 모델을 원하는 것이 아니다. 문장이 뭉쳐 유기적인 글을 형성하기 원한다.** 하지만 이를 위해 윈도우 사이즈를 지나치게 크게 한다면, 단어간 연관성이 형성되기 어렵다. 그래서 학습시키는 데이터 셋은 하나의 완성된 글의 형태를 통으로 학습하되 하나의 뭉텅이로 보는 윈도우 사이즈는 작게 만든다.

그래서 완성된 글이 서로 얼마나 유기적인지 그 정도를 나타내는 함수 $L(\theta)$를 다음처럼 정의한다.

![Screenshot 2025-03-17 at 5.52.11 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-17%20at%205.52.11%20PM.png)

여기서 **단어간의 유기성을 학습하기 위한** 윈도우 사이즈 $m$은 5내외, (아마 문장의 완성을 도울 것이다.)
**문장간의 유기성을 학습하기 위한** 중심단어 위치 $t$의 범위 $T$는 1M~1B이며, 10B이상이기도 한다. (아마 내용이 연결되는 글의 사이즈를 의미하고, 이로써 맥락을 학습할 것이다.)
( #Q 하지만 이게 최선일까?)

## Objective Ftn(= cost or loss ftn)
우리 목표는 우도Likelihood를 최대화 하는것이다.
따라서 $J(\theta)$는 $L(\theta)$에 '-' 를 곱해 최소화 하는 함수로 바꾸고 그라디언트 디센트를 쓴다.

이때 곱연산은 미분하기 어렵기 때문에 로그를 합성하고, $T$사이즈에 따라 무작정 값이 커지는 것을 막기 위해 평균으로 바꿔준다. 그 결과 아래와 같은 식이 된다.

![Screenshot 2025-03-17 at 6.05.25 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-17%20at%206.05.25%20PM.png)

## How to express Probability
그래서 확률 $P(w_{t+j}|w_t)$는 어떻게 표현되는가?

중심 단어(center word)가 주어졌을 때, 다른 모든 단어들이 나오는 대신, 주변부 단어(outside or context word)가 등장할 확률이다.
이때 등장 확률은 유사성이 결정하며, 유사성은 내적으로 구한다. $\implies u^T v$

여기에 소프트맥스softmax 함수를 사용하면 중심단어로부터 다른 모든 단어들과의 거리 중, 주변부 단어와 얼마나 가까운 상태인지를 확률로 표현할 수 있다.

![Screenshot 2025-03-17 at 6.12.56 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-17%20at%206.12.56%20PM.png)

이처럼 어떤 $t$와 $j$에 대해 $P(o|c)$를 정의하였다.

## Calculating Gradient
우리는 다음과 같이 파라미터를 업데이트 하고 싶었다.
$$\theta^{new} = \theta^{old} - \alpha \nabla_{\theta} J(\theta^{old}), \qquad \theta \in \mathbb R^{2 \times D \times Vocab}$$
이제 우리는 $J(\theta)$를 정의했으므로, $\nabla J(\theta)$를 계산할 차례다.
참고로 차원 때문에 이해하는게 꽤 힘들었다.

---
전제:
$J \in \mathbb{R}$
$\forall w \in Vocab$,  $v_w, u_w \in \mathbb{R}^{D}$
$$\begin{align*}
&U = [ u_{w_1} u_{w_2} ... u_{w_{|vocab|}}],\quad
V = [ v_{w_1} v_{w_2} ... v_{w_{|vocab|}}] \quad
\therefore U, V \in \mathbb{R}^{D\times Vocab} \\\\
&\theta = [U \quad V] \quad \therefore \theta \in \mathbb{R}^{2\times D\times Vocab}
\end{align*}$$
---
$$\begin{align*}
\nabla J(\theta) &=\frac{∂J}{∂θ} \\\\
&= \left[ \frac{∂J}{∂U} \quad \frac{∂J}{∂V} \right] \\\\
&= \left[ \frac{∂J}{∂u_o} + \sum_{w \neq o}\frac{∂J}{∂u_w} \quad \frac{∂J}{∂v_c} + \sum_{w \neq c}\frac{∂J}{∂v_w} \right]
\end{align*}$$
---

참고로 위 식의 + 는 브로드캐스팅 합처럼 생각하면 된다.
엄밀하게 쓰기엔 길어서 줄여썼다.

그럼 우린 최종적으로 4가지 식에 대한 계산을 해야 한다.
가장 먼저 아래 식을 이해하는 것이 중요하다. 그러고 나면 나머지는 어렵지 않다.

---
$$\begin{align*}
\frac{\partial}{\partial v_c} \log P(O = o \mid C = c)
&= \frac{\partial}{\partial v_c} \log \frac{\exp(u_o^\top v_c)}{\sum_{w \in V} \exp(u_w^\top v_c)}
\\\\
&= \frac{\partial}{\partial v_c} \log \exp(u_o^\top v_c) - \frac{\partial}{\partial v_c} \log \left\{ \sum_{w \in V} \exp(u_w^\top v_c)\right\}
\\\\
&= u_o - \frac{\partial}{\partial v_c} \log Z \quad (Z\text{ 치환})
\\\\
&= u_o - \frac{\partial}{\partial Z} \log Z \frac{\partial Z}{\partial v_c} = u_o - \frac{1}{Z} \frac{\partial Z}{\partial v_c}
\\\\
&= u_o - \frac{1}{\sum_{w \in V} \exp(u_w^\top v_c)} \frac{\partial}{\partial v_c} \left \{\sum_{x \in V} \exp(u_x^\top v_c) \right\}
\\\\
&= u_o - \frac{1}{\sum_{w \in V} \exp(u_w^\top v_c)} \sum_{x \in V} u_x \exp(u_x^\top v_c)
\\\\
&= u_o - \sum_{x \in V} \frac{\exp(u_x^\top v_c)}{\sum_{w \in V} \exp(u_w^\top v_c)} u_x
= u_o - \sum_{x \in V} P(x \mid c) u_x
\\\\
&= u_o - \mathbb{E}[u_X \mid c] \qquad (\text{단}, X \in V)
\\
&(observed - expected)
\end{align*}$$
이제 나머지 3개에 대해서도 연산하자
$\text{Case 1) }x = o:$
$$\begin{align*}
\frac{\partial}{\partial u_x} \log P(O = o \mid C = c)
&= \frac{\partial}{\partial u_o} \log \frac{\exp(u_o^\top v_c)}{\sum_{w \in V} \exp(u_w^\top v_c)} \\\\
&= \frac{\partial}{\partial u_o} \log \exp(u_o^\top v_c) - \frac{\partial}{\partial u_o} \log \left\{ \sum_{w \in V} \exp(u_w^\top v_c)\right\} \\\\
&= v_c - \frac{\partial}{\partial u_o} \log Z \quad (Z\text{ 치환}) \\\\
&= v_c - \frac{\partial}{\partial Z} \log Z \frac{\partial Z}{\partial u_o} = v_c - \frac{1}{Z} \frac{\partial Z}{\partial u_o} \\\\
&= v_c - \frac{1}{\sum_{w \in V} \exp(u_w^\top v_c)} \space v_c \exp(u_o^T v_c) \\\\
&= v_c \left\{1 - \frac{\exp(u_o^T v_c)}{\sum_{w \in V} \exp(u_w^\top v_c)} \right\} \\\\
&= v_c \left\{1 - P(o \mid c) \right\}
\end{align*}$$
$\text{cf})\quad \frac{\partial Z}{\partial u_o} = \frac{\partial}{\partial u_o} \sum_{w \in V, w \neq o} \exp(u_w^\top v_c) + \frac{\partial}{\partial u_o} \sum_{w \in V,  o} \exp(u_w^\top v_c) = v_c \exp(u_o^T v_c)$

$\text{Case 2) }x \neq\ o$:
$$\begin{align*}
\frac{\partial}{\partial u_{x \neq o}} \log P(O = o \mid C = c)
&= \frac{\partial}{\partial u_{x \neq o}} \log \frac{\exp(u_o^\top v_c)}{\sum_{w \in V} \exp(u_w^\top v_c)} \\\\
&= \frac{\partial}{\partial u_{x \neq o}} \log \exp(u_o^\top v_c) - \frac{\partial}{\partial u_{x \neq o}} \log \left\{ \sum_{w \in V} \exp(u_w^\top v_c)\right\} \\\\
&= 0 - \frac{\partial}{\partial u_{x \neq o}} \log Z \quad (Z\text{ 치환}) \\\\
&= - \frac{\partial}{\partial Z} \log Z \frac{\partial Z}{\partial u_{x \neq o}} = - \frac{1}{Z} \frac{\partial Z}{\partial u_{x \neq o}} \\\\
&= - \frac{1}{\sum_{w \in V} \exp(u_w^\top v_c)} \space \frac{\partial}{\partial u_{x\neq o}} \left \{ \sum_{w \in V, w \neq o} \exp(u_w^\top v_c) \right \} \\\\
&= - \frac{\exp(u_x^T v_c)}{\sum_{w \in V} \exp(u_w^\top v_c)} v_c \\\\
&= - P(x \mid c) v_c
\end{align*}$$
그리고 나머지 케이스인 $\sum_{x \neq c}\frac{∂J}{∂v_x}$는 0이고 계산은 생략한다.

계산이 길었는데, 이 값들이 J에 대한 그라디언트인건 아니므로 바로 써서는 안됨에 유의한다.

##
왜 두개의 벡터 UV를 쓰는지에 대한 설명추가

![Screenshot 2025-03-20 at 6.43.35 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-20%20at%206.43.35%20PM.png)
