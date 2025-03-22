---
{"dg-publish":true,"permalink":"/= Study/Algorithm/Probabilistic Algorithm./","created":"2023-12-07T05:49:34.000+09:00","updated":"2025-01-14T15:33:43.000+09:00"}
---


# Deterministic Algorithm
동일한 입력에 대하여 매번 동일한 결과를 출력하는 알고리즘.
즉, 확률과 무작위성이 개입하지 않는다. 우리가 지금까지 배워온 형태.

# Randomized Algorithm
확률과 무작위성의 개입으로, 동일한 입력에 대하여 다른 결과가 나타날 수도 있는 알고리즘.

## Miller-Rabin 소수 판별법
일반적인 소수 판별 알고리즘의 속도는 $O(\sqrt n)$ 이다.
하지만, 무작위 알고리즘을 이용하면, 정확도를 포기하는 대신, 훨씬 빠르게 판별할 수 있다.

Flash Back!

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/study/algorithm/encryption-algorithm/#c-euler-theorem" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



### (c) Euler Theorem
자연수 $a$와 $n$이  서로소라면 $a^{\Phi(n)}$을 $n$으로 나눈 나머지는 1이다.
$a^{\Phi(n)} \equiv 1 \text { (mod n)}$ 

----
(a)의 정의로부터, 임의의 소수 $p$에 대해, $\Phi(p) = p - 1$ 임이 자명하며, 그 역도 성립한다. 따라서 이는 소수만이 갖는 성질로서, **소수를 수학적으로 표현**한 것이나 마찬가지이다.

소수를 수학적으로 표현할 수많은 방법 중 왜 하필 이거냐? 하면,
바로 (b)-2 와 같은 성질을 통해 소수를 활용할 수 있기 때문일 것이다.

물론 (b)-1이나, (c)의 성질 들을 볼 때, 서로소 또한 굉장히 중요하게 사용된다. #Q 솔직히 이 이상은 잘 모르겠다.


</div></div>

암호화 알고리즘에서 수학적으로 소수를 표현하는 방법에 대해 배웠었다.
이것을 이용하자.

$\large N = \text{소수} \iff \Phi(N) = N-1$
$\large\hspace{2.7cm} \implies a^{N-1} = a^{2^sd} \equiv 1 \pmod{N}$
$\large\hspace{2.6cm} \iff a^{2^sd} - 1 \equiv 0 \pmod{N}$
$\large\hspace{2.6cm} \iff (a^{2^{s-1}d} + 1)(a^{2^{s-1}d} - 1) \equiv 0 \pmod{N}$
$\large\hspace{2.6cm} \iff (a^{2^{s-1}d} + 1)(a^{2^{s-2}d} + 1)(a^{2^{s-2}d} - 1) \equiv 0 \pmod{N}$
$\large\hspace{2.6cm} \iff (a^{2^{s-1}d} + 1)(a^{2^{s-2}d} + 1)...(a^{d} + 1)(a^{d} - 1) \equiv 0 \pmod{N}$

따라서 만약 N이 소수라면, s+1개의 항들 중 최소한 한개는 N의 배수일 것이다.
그러나 이것이 **항들 중 최소한 한개가 N의 배수인게, N이 소수임을 보장하지는 않는다.**

그치만!! 보장은 안하지만!! 저 조건이 무의미하지는 않다.
왜냐하면, 저 조건을 만족하지 않는 경우에는 합성수임이 확실히 보장된다. 이러한 부분이 꽤 크기 때문에, **남아있는 수 들은 소수일 확률이 꽤 높다.**


## Pollard-Rho.
[[= Study/Algorithm/Encryption Algorithm.#RSA Cryptosystem\|RSA]] 암호화는 소인수분해 문제의 난해함에 기반한다.
#Q ㅅㅂ 이해가안간다고요

# 생일 문제
23명이 모이면 적어도 같은 생일인 사람이 있을 확률 50%
70명이 모이면 99.9%
=> 교훈. 틀릴 가능성이 높아도, 반복적으로 시도한다면 신뢰성이 높아진다.

## 무작위화를 이용한 문제 해결
틀릴확률이 높은 무작위화 알고리즘도 적당한 횟수만큼 반복하여, 틀릴 확률을 낮힐 수 있다.
