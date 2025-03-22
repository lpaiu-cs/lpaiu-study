---
{"dg-publish":true,"permalink":"/= Study/Data Processing and Analysis/Vector Semantics/","created":"2023-12-28T19:05:44.000+09:00","updated":"2025-03-13T21:13:35.772+09:00"}
---


어떤 단어를 벡터로 표현할 때, 벡터 자체에 단어의 의미가 담기도록 하면 어떨까? 에 대해 다루는 것이 벡터 의미론이다.

# Lexical Semantics - 어휘 의미론
#pending 


# Vector Semantics - 벡터 의미론

어휘의 의미를 동의어 유의어 반의어 등을 통해 관계를 표현하는 것은 인간에게 친숙하고 편한데 반해 단어의 의미를 수학적으로 벡터 공간에 표현하는 것은 컴퓨터에게 친숙하며 NLP에 매우 용이하다. 이러한 표현 과정을 임베딩embedding이라고 부른다.

그런데 **단어의 의미를 어떻게 벡터 공간에 임베딩**할 수 있을까?
### Embedding Vector
벡터 차원의 크기를 50, 100, 200, 300 등의 고정된 크기로 사용하는 dense-vector의 일종이다.
단어의 의미를 직접적으로 임베딩하는 기법을 사용한다.
ex) Word2Vec, GloVe

### Co-occurrence Vector
벡터 차원의 크기가 굉장히 크고 열려있는 sparse-vector의 일종. 일반적으로 |Vocabulary| 가 차원의 크기가 된다. 단어의 의미를 표현하기 위해, context에 함께 등장한 단어인 공기어co-occurrence의 빈도를 이용한다.
예컨대 apple이라는 단어와 같은 context에 computer가 몇번 나왔는지, tree가 몇번 나왔는지, pear가 몇번나왔는지 등을 기록하는 것이다.

## Term-document matrix
문서에 해당 단어가 얼마나 등장했는가.

| | 문서1 | 문서2 | 문서3 |... |
| - | - | - | - | -|
| 단어1 | 30 | 23 | 12 |...|
| 단어2 | 12 | 0 | 3 |...|
| 단어3 | 1 | 15 | 2 |...|
|...|...|...|...|

이렇게 하면 문서를 벡터로 표현할 수 있다. 각 차원은 사용된 단어이다.
문서1 = <30, 12, 1, ... >
Bag of Words와 같은 원리로 문서간 유사도를 결정하고 **분류**할 수 있다.

반대로, 단어 또한 표현할 수 있다. 각 차원은 문서목록이다. 동일한 문서에 등장한 단어들은 비슷한 맥락임을 전제로 하고 있다.
단어1 = <20, 23, 12, ... >
이 두 벡터표현은 모두 공기어(co-occurrence)에 의한 임베딩 벡터다.

## Term-context matrix
문서보다는 작은 단위인 문맥을 이용하여, 공기어 분석을 한다. 단어를 표현하기에 문서는 너무 크기 때문에, 공기어 분석의 전제인 비슷한 의미를 가졌을 것이다를 충족하기 어렵다. 따라서, 문맥의 단위를 적절히 정의하여, 성능을 높인다.

문맥의 크기를 정의하는 방법
1. Paragraph
2. Window of ± 4 words (혹은 임의의 n words)
학습 데이터를 이용해 문맥 단어들을 count하여 차원 값을 정한다.

윈도우의 크기는 달라질 수 있다. 윈도우의 크기가 줄어들 수록 더 구문적(syntactic)이고, 커질 수록 더 의미적(semantic)인 성향을 띤다고 한다.

## Measuring Similarity
유사도를 어떻게 측정할 것인가?
가장 먼저 내적이 떠오른다. 좋은 방법이지만, 한가지 **문제**가 있다.
단지, 빈번히 사용될 뿐인 단어에 대해서는 실질적 유사성과 무관하게 유사도가 높게 측정될 수 있다.

그래서 내적대신 cosin 각도를 사용한다.

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/study/data-processing-and-analysis/text-classification/#cosine-similarity" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### Cosine similarity
$\cos(\vec{a}, \vec{b}) = \frac{\vec{a} \cdot \vec{b}}{\|\vec{a}\| \|\vec{b}\|} = \frac{\sum_{i=1}^{|V|} a_i b_i}{\sqrt{\sum_{i=1}^{|V|} a_i^2} \sqrt{\sum_{i=1}^{|V|} b_i^2}}$


</div></div>


>[!warning] raw count problem
> 차원의 값을 설정할 때 절대빈도(raw count)를 쓰는 것은 좋은 방법이지만, 최선은 아니다.
> 빈도는 편향적이고, 가장 구별적이지도 않다.

### raw count의 대안
- tf-idf
- PPMI
- t-score

#### tf-idf
문서 내의 단어들이 얼마나 중요한지를 수치적으로 평가한다.
value = tf × idf
tf: term frequency-용어의 빈도에 log를 씌워 완화함 log(1+freq)
idf: inverse document frequency-문서에서 등장 희소성이 얼마나 되는가 log(N/df)

#### PMI (Pointwise Mutual Information)
두 정보가 독립적이지 않고 얼마나 종속적인지 구하는 지표 log(P(w1, w2) / P(w1)P(w2))
＋: 단어간 거리가 가까움
－: 멂
０: 독립
##### PPMI (Positive PMI)
PMI 지수에서 음수는 실제론 해석이 곤란하다. 따라서 음수인 값을 0으로 처리하기도 한다.
##### smoothing
또 다른 문제는, 매우 희귀한 단어에 대해 PMI 지수가 심하게 편향된다는 것이다. 그래서 모든 단어에 공평하게 수를 더하는 등의 방법으로 편향을 방지할 수 있다.

#### t-score
대상어와 어떤 공기어 간의 연관도associativeness를 측정하는 척도
대상어: a, 어떤 공기어: b ( $\in$ B), 전체 등장 단어 수: N, a가 등장한 context들의 전체 단어 수 합: $N_a$

- 예상빈도 $E = \frac{N_a \times f(b)}{N} = N_a \times P(b)$
	즉, context의 단어 하나하나가 b가 등장할 수 있는 잠재적 확률을 가지고 있다고 보는 것. -> a와 b가 독립이라 가정할 때 **예상되는, b가 a의 공기어로 등장하는 빈도 수 합**.
- 관측빈도 $O = f(a, b)$
	실제로 관측되는 a와 b의 공기 빈도 수 합.
$$t = \frac{O - E}{\sqrt O}$$
따라서, 예상치보다 관측치가 클 경우 t-score는 양의 값을 가지며, a에 대한 b의 연관성이 높다고 할 수 있다. 반면, 예상에 못미칠 경우 음의 값을 가지며, a에 대해 b가 음의 연관성을 갖는다고 할 수 있다. 그리고 zero일 경우, a에 대해 b의 연관성이 없다고 할 수 있다.

>[!info]
>assignment06에서 전처리된 문맥 corpus들을 바탕으로, 대상어-공기어 쌍의 t-score를 계산해 보았다.

>[!info]
>assignment07에서 직전 과제에서 만든 t-score 딕셔너리를 바탕으로, 공기어 벡터를 생성해 보았다. 또한, 이를 바탕으로 query에 대해 가장 유사한 30개의 관련어를 보여주는 코드를 만들어 보았다.

