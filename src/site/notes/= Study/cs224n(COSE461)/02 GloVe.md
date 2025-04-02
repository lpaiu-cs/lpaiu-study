---
{"dg-publish":true,"permalink":"/= Study/cs224n(COSE461)/02 GloVe/","created":"2025-03-18T12:21:20.891+09:00","updated":"2025-04-02T19:35:00.911+09:00"}
---

Lecture 2: Word Vectors, Word Senses, and Neural Network Classifiers

Key Goal: To be able to read word embeddings papers by the end of class

이전 강의: [[= Study/cs224n(COSE461)/01 Word Vectors\|01 Word Vectors]]
다음 강의: 

---

# Bag of words model and computations 9p
Bag of words: 앞뒤 구분없는 계산

UV 나눠서 계산 후 평균합산

# 벡터 분해 특성
linear meaning component
king - man + woman = queen

# stochastic gradient
시간이 없었는지 강의상에서 다뤄지지는 않았다.
그러나 실제 학습에서 쓰이는 것은 GD가 아닌 SGD 이다.

![Screenshot 2025-03-17 at 6.34.27 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-17%20at%206.34.27%20PM.png)

# skip-gram model with negative sampling
skip-gram 모델을 더 적은 리소스로 구현하기 위한 시도이다.

![Screenshot 2025-03-25 at 9.27.30 AM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-25%20at%209.27.30%20AM.png)
확률 계산 부분에서 매 연산마다 모든 단어들(아마 40만개정도)에 대해 내적 및 덧셈을 하려니, 계산량이 너무 많았다.
이 문제를 완화하기 위해 negative sampling 기법이 등장했다.
깊게 들어가기 앞서, [[= Study/Deep Learning/로짓과 로지스틱 함수 logits and logistic function\|로짓과 로지스틱 함수 logits and logistic function]]의 내용을 복습하자.


**Main idea: true pair(분자)와 noise pairs(분모)를 독립적으로 이진 로지스틱 회귀 학습을 진행한다.**

skip-gram 모델은 $U^Tv_c$연산의 모든 로짓들을 한번에 softmax를 적용했었다.
하지만 negative sampling은 품질을 유지하는 동시에 일부 로짓만 사용하여 단순화한다.

그러기 위해 true pair와 noise pairs를 먼저 독립적으로 로지스틱 함수를 이용해 확률을 만든다.
$$
\begin{align*}
\text{true pair }\quad p_t &= \sigma(u_o^T v_c)  \\
\text{noise pairs}\quad p_k &= \sigma(u_k^T v_c) = 1 - \sigma(- u_k^T v_c), \quad \forall k \in K
\end{align*}
$$
noise pairs는 계산의 단순성을 위해 K개(대략 5~20개)의 단어만 샘플링한다. 기법의 이름인 negative sampling 또한 이 noise pairs를 뽑는 방법에 기인한다.

따라서 $p_k$대신 $q_k$를 사용하면 목적을 확률의 최대화로 바꿀 수 있다.
$\implies q_k = \sigma(-u_k^T v_c)$


## Likelihood, Objective Ftn
이제 확률을 바탕으로 likelihood를 구할 것이다.
로지스틱 함수의 $L = \log\frac{p}{(1-p)} = \log p - \log (1-p)$이다.

따라서 이렇게 정의할 수 있다.
$$L = \log \frac{p_t}{1-p_t} - \log \frac{p_{k=1}}{1-p_{k=1}} - \log \frac{p_{k=2}}{1-p_{k=2}} - \space ... $$
이때 로지스틱 함수의 특성상,
$\sigma(x) = 1 - \sigma(-x)$ 이므로, $p = \sigma (l) \rightarrow 1-p = \sigma (-l)$이 성립한다.
따라서 $\log p = \log (1-p)$ 가 성립하여 다음처럼 식이 간단해진다.
$$L = 2\left\{ \log p_t - \log p_{k=1} - \log p_{k=2} - \space ...\right\} $$
따라서 목적함수가 아래와 같이 정의된다.
$$
J_{neg-sample}(u_o, v_c, U) = - \left\{ \log \sigma(u_o^T v_c) - \sum_{k \in \{K \space sampled \space indices\}} \log \sigma(u_k^T v_c) \right\}
$$
---


# Co-occurrence matrix with SVD
시간없어서 넘김.

# Windows vs Full document

> Window
> 단어 사이의 문법적, 의미적 정보의 학습 -> "word space"

 > Full document
 > 일반적인 토픽이 주는 연관성 학습 -> "document space"

# Predict-based vs Count-based
왜 직접 카운팅해서 벡터를 구성하는 방법을 쓰지 않고, 예측기반으로 워드벡터를 구성할까?
둘 사이의 차이점은 무엇일까?

> Predict-based (e.g. word2vec)
> 1) 밀집(dense) 벡터이고, 저차원임.
> 2) 미묘한 의미까지 학습 가능.

> Count-based (e.g. co-occurrence matrix)
> 1) 희소(sparse) 벡터이고, 고차원임. -> 계산상 비효율 + 일반화 어려움.
> 2) raw counts의 경우 의미적 부정확성 및 다의 구분 어려움.

하지만 카운팅 기반의 단점인 고차원성과 의미적 부정확성은 각각 SVD와 LSA(Latent Semantic Analysis)로 해결할 수 있음.
ex) GloVe

# COALS
Co-Occurrence Analysis using Latent Semantic Indexing - Douglas L. Rohde

기존의 LSA를 확장하여, 의미적 유사성과 관련성을 더 잘 반영.
SVD를 사용하여 count-based의 약점인 고차원성을 해결.

# GloVe
Global Vectors for Word Representation

COALS에서 착안한 count-based 방식의 일종으로, **동시 발생 확률의 비율**이 의미 구성 요소를 형성할 수 있다면, 그리고 벡터 공간에서 선형적인 것으로 만들 수 있다면, word2vec이나 COALS와 같은 결과를 얻을 수 있다.

아래 그림은 co-occurrence matrix에 기반한, 동시 발생 확률을 표로 나타낸 것이다.

![Screenshot 2025-04-01 at 2.52.23 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-01%20at%202.52.23%20PM.png)

ice와 steam 두 단어를 중심 단어로 두고 확률을 살폈을 때, 각각 속성으로 갖는 단어들에 대해 높은 확률을 지녔다. 
>$ice$: $solid$, $water$ -> 높은 확률
>$steam$: $gas$, $water$ -> 높은 확률
>$fashion$과 같이 무관한 단어(random 단어) -> 두 단어 모두에서 비슷하게 낮은 확률

이때 두 확률을 나누었더니, $ice$와 $steam$간의 의미 차이가 각각 $solid$, $gas$에서 두드러지고, $water$, $fashion$ 등 의미 차이가(비슷하게 의미를 갖거나 비슷하게 없거나) 거의 없는 부분에서는 1에 가까운 값으로 드러나는 것을 발견할 수 있었다.
결론적으로, **의미 차이는 확률의 비율로 드러난다**는 중요한 통찰을 얻게 된다.

이 의미 차이를 선형 구성요소로 바꾸는 것이 GloVe의 핵심 아이디어이다.

## GloVe: Encoding meaning components in vector differences

![Screenshot 2025-04-01 at 3.15.49 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-01%20at%203.15.49%20PM.png)


# How to evaluate word vectors?
워드벡터의 성능을 어떻게 평가할 수 있을까는 중요한 요소이다. 채점을 할 수 있어야 학습이 의미있기 때문이다.
여기에는 두 가지 방법이 존재한다.

>Intrinsic:
>내재적 평가란 매우 구체적인 하위 작업을 수행하고 그것을 통해 평가를 매기는 것이다.
>따라서 일반적으로 계산이 빠르고, 시스템을 직접적으로 이해하는데 도움이 되지만, 다운스트림 작업과거리가 멀고, 실제로 도움이 되는지와 다른 문제일 수 있다.

>Extrinsic:
>외재적 평가는 질문에 답하거나 문서 요약을 하거나 기계번역을 하는 등 실제적인 작업을 하는 경우를 말한다.
>전체 시스템을 실행하고 다운스트림 정확도를 계산한 후 파악해야 하므로 오래 걸리고, 간접적이다.

## Intrinsic word vector evaluation
벡터 자체의 의미정보를 평가한다.
-단어 유사도 판단: 두 단어 벡터간 코사인 유사도를 사람이 판단한 후 비교
-단어 유추(analogy): man: woman :: king : ? 와 같은 연산이 얼마나 잘 맞는지
-군집 시각화: 벡터 시각화를 통해 유사 단어가 근처에 있는지 확인

## Extrinsic word vector evaluation
실제 NLP 다운스트림 작업에서의 성능을 평가한다.
대표적인 다운스트림 작업:
-문서분류: 벡터로 문서를 표현하여 분류 정확도 측정
-개체명 인식(NER): 단어 벡터를 입력으로 사용하여 성능 확인
-감정 분석: 문장 벡터 구성 후 긍/부정 판단
-기계번역 / 질의응답: 문맥 벡터 성능 간접 평가

# Word senses and word sense ambiguity
단어에는 여러 의미를 가진 단어가 흔히 존재한다. 그러한 다의어 및 동음이의어는 의미를 구분하기 어렵다.

![Screenshot 2025-04-01 at 3.52.02 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-01%20at%203.52.02%20PM.png)

그러한 것을 구분할 수 있게 해주는 sparse coding이라는 기법이 있다.
자세한 내용은 설명되지 않는다.


# Deep Learning Classification: NER
Named Entity Recognition으로 단어에 클래스를 레이블로 지정하는 작업이다.

예를들어, 아래 그림처럼 문장 내에서 '사람', '위치', '날짜' 로 단어들을 분류하는 것이다.
![Screenshot 2025-04-01 at 3.56.22 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-01%20at%203.56.22%20PM.png)

이러한 분류는 어떻게 수행할 수 있을까?
예컨대 Paris라는 단어는 맥락상 '사람'의 이름일 수도 있고, '위치'의 이름일 수도 있다.

I love **Paris** Hiton greatly . -> 사람
the museums in **Paris** are amazing to see . -> 위치

이처럼 문맥은 단어의 개체명 인식과 관련성이 크다.
따라서, NER의 핵심 아이디어는 context window classification 이다.

아래 슬라이드는 로지스틱 분류를 활용한 NER의 아주 간단한 이진 개체명 분류 예시이다.
![Screenshot 2025-04-01 at 4.11.23 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-01%20at%204.11.23%20PM.png)

## Neural clasification
일반적으로 분류기의 경우 지도학습을 통해 학습한다.
이전에 우리가 배워온 전통적 기계학습 분류기는 선형 분류기이다.
>x: input
>y: label
>W: parameter

입력에 대한 결과와 레이블간의 차이를 이용하여, 파라미터 W를 학습하는 것이다.

![Screenshot 2025-04-01 at 4.23.20 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-04-01%20at%204.23.20%20PM.png)

이러한 분류기는 선형 경계를 갖기 때문에 유연하지 못하고, 한계를 갖는다.
하지만, 신경망 분류기는 다르다.

![Pasted image 20250401162702.png](/img/user/z-Attached%20Files/Pasted%20image%2020250401162702.png)

먼저, 파라미터 W 뿐만 아니라, 입력값인 word vectors $\mathbf x$ 또한 함께 학습한다.
그 말은 $\mathbf x$가 one-hot vector $y$ 에서 중간 계층 벡터 공간으로 이동한다는 뜻이며, 그 결과 linear softmax classifier로 더 쉽게 분류할 수 있게 한다. 이러한 과정은 개념적으로 임베딩 벡터 $\mathbf x = \mathbf V\mathbf y$를 얻는 것이다. (그림에는 x = Le)

즉, $\mathbf x$는 히든레이어 계층으로 이동한 후 비선형 활성화 함수 $f$ 를 통과함으로써 비선형성을 얻는다

![Pasted image 20250402000447.png](/img/user/z-Attached%20Files/Pasted%20image%2020250402000447.png)

# Training with "cross entropy loss" for use in PyTorch
cross entropy loss는 딥러닝에서 흔히 쓰이는 손실 함수중 하나이다.

>정답 클래스의 확률 최대화 $\iff$ cross entropy loss의 최소화

cross entropy loss는 다음과 같다.
$p_c$: 정답 분포, $q_c$: 모델이 예측한 확률 분포, $C$: 클래스 개수
$$
H(p, q) = -\sum_{c=1}^C p(c) \log q(c)$$
워드 벡터에 대해 표현해보자.
먼저 정답 분포는 분류 문제에서 주로 one-hot 벡터로 표현된다. 이를  $\mathbf y$ 라 하자. ( $\mathbf y \in \mathbb{R}^{Vocab}$ )
그렇다면 예측 분포를 가리키는 예측 벡터 $\hat{\mathbf y}$ 으로 표현할 수 있다. ( $\hat{\mathbf y} \in \mathbb{R}^{Vocab}$ )
따라서 $y_w, \hat y_w$ 이 각각 $\mathbf y, \hat{\mathbf y}$ 의 element 일 때, 다음이 성립한다.
$$\begin{align*}
H(p, q) 
&= -\sum_{c=1}^C p(c) \log q(c) \\\\
&= -\sum_{w\in Vocab} y_w \log \hat{y}_w
\end{align*}$$
직전 수업에 등장한 skip-gram 모델은 중심 단어 $c$ 가 주어졌을 때, 주변부 단어 $o$ 를 예측하는 모델이었다.
이 경우 모델의 예측 확률 $P(O = o \mid C = c)$는 $\hat y_o$ 이다.
따라서 이경우 다음이 성립하게된다.
$$-\sum_{w\in Vocab} y_w \log \hat{y}_w = - \log \hat y_o$$
이로써 정답 클래스 확률과 cross entropy loss의 동등성이 성립한다.

# Neural Computation
유명하고 반복되서 생략