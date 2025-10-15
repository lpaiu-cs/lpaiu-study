---
{"dg-publish":true,"permalink":"/== Study/Algorithm/Machine Learning Algorithm_/","created":"2024-04-22T02:22:52.000+09:00","updated":"2025-03-07T22:57:35.000+09:00"}
---


![Pasted image 20231207033500.png](/img/user/z-Attached%20Files/Pasted%20image%2020231207033500.png)
# 기계학습이란?
컴퓨터가 데이터로부터 학습하여 패턴을 인식하고, 이를 통해 예측이나 결정을 자동으로 수행할 수 있게 하는 것.

# 연관분석
대규모의 데이터셋으로부터, 아이템들간의 흥미로운 관계, 즉 **연관규칙**을 찾아내는 것을 의미한다.

연관 규칙을 평가하기 위한 지표로써 다음의 3가지가 존재한다.
### (1) 지지도Support
전체 데이터셋 대비 특정 아이템셋이 나타나는 비율을 의미한다.
$$\text{Support}(A \cap B) = \frac{\text{Number of transactions containing both A and B}}{\text{Total number of transactions}}$$
만약, 아이템셋이 일정 비중 아래라면, 의미없는 값으로 치부할 수 있다. (일종의 threshold)

### (2) 신뢰도Confidence
P(B | A)와 유사하다. A를 포함하는사례 중 B 또한 포함하는 사례가 많다면, **A에 대한 B의 신뢰도가 높다**라고 말할 수 있다.
$$\text{Confidence}(A \Rightarrow B) = \frac{\text{Support}(A \cap B)}{\text{Support}(A)}$$
유의할 점은, 높은 신뢰도를 보인다 해서 항상 유의미한 것은 아니다. 만약 B가 너무 흔하다면, 무의미하다.

### (3) 향상도Lift
신뢰도가 우연에 의한 것인지 아닌지를 나타내는 지표.
1이면 독립을 의미하고,  1보다 크면 두 아이템이 양의 상관 관계, 작으면 음의 상관 관계를 가지고 있다고 해석한다.
$$\text{Lift}(A \Rightarrow B) = \frac{\text{Confidence}(A \Rightarrow B)}{\text{Support}(B)}$$
## Apriori Algorithm
최소 지지도를 설정하고, 최소 지지도 이상을 갖는 모든 아이템셋을 알아내는 알고리즘.

확장과 가지 치기를 반복하여 효율 도모.
(**확장**) 크기 1인 아이템 셋들을 모음 -> (**가지치기**) 최소 지지도 미만 배제 -> (**확장**) 살아남은 아이템 셋끼리의 조합으로 크기 2인 아이템 셋 형성 -> (**가지치기**) -> (**확장**) -> ... 반복

최종적으로 얻은 아이템셋은 크기와 무관하게 합친다.
>확장할 때마다, **데이터베이스를 매번 참조**해야 하는 **한계**가 있다. 그러니까 브루트포스보다는 빠른데, 아직도 존나 느리다.

## FP-Growth Algorithm.
해답은 자료구조에 있다.
FP-[[== Study/Data Structure/Binary Tree\|Tree]]라는 자료구조를 정의하고, 규칙에 따라 데이터를 연결해 나가는 알고리즘이다.

# 의사결정트리Decision [[== Study/Data Structure/Binary Tree\|Tree]]
각 데이터들이 가진 속성들로부터 패턴을 인식하여 분류 과제를 수행하는 모델.

분류를 잘 하기 위해 필요한 것은 뭘까?
바로, **좋은 질문**이다.
그리고 좋은 질문이란, 데이터의 순도를 잘 높이는 분류 기준을 의미한다.

데이터셋으로부터 두 가지 레이블, A집단과 B집단으로 분류할 때,
처음에 A와 B가 뒤죽박죽 섞여 있는 상태를 **불순도**가 높다고 한다.
#play [의사결정 나무(Decision Tree) 쉽게 이해하기](https://hleecaster.com/ml-decision-tree-concept/)
### 불순도Impurity
불순도를 정량적으로 표현하는 두가지 지표가 있다.
#### Gini Index
- $1 - \sum \text {(각 레이블이 차지하는 비율)}^2$
- 레이블이 두 가지라면, 값의 범위는 0~0.5 이다. 만약, 분류가 끝났다면, 0이 된다.
#### Entropy
- $-\sum p_i\ log_2 p_i$
- 레이블이 두 가지라면, 값의 범위는 0~1 이다.

>이것을 통해, 좋은 질문과 나쁜 질문을 판단할 수 있다. 레이블 즉, 정답을 통해 불순도를 급격히 낮출 수 있는 좋은 질문들을 찾는 과정이 기계학습에 속한다. 그 중, 정답을 통해 학습시키므로 지도학습이라 할 수 있다.

# k-NN에 대한 기하학적 접근
k-Nearest Neighbor 알고리즘은 유유상종 알고리즘이라 불리는 거리기반 분류 과제 수행 모델이다.
말 그대로, k개의 **가까운 이웃**의 속성에 따라 분류한다.

그런데, '가깝다'는 것은 거리 척도(distance metric)의 정의에 따라 달라진다.

## 거리 척도Distance Metric


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/study/data-processing-and-analysis/text-classification/#euclidean-distance" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### Euclidean distance
$d_{euc}(\vec{a}, \vec{b}) = \sqrt{\sum_{i=1}^{|V|} (a_i - b_i)^2}$

</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/study/data-processing-and-analysis/text-classification/#manhattan-distance" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### Manhattan distance
$d_{mht}(\vec{a}, \vec{b}) = \sum_{i=1}^{|V|} |a_i - b_i|$

</div></div>


어떤 거리 척도(distance metric)을 선택할지는 매우 흥미로운 주제이다.
왜냐하면 거리 척도에 따라 공간의 기하학적 구조 자체가 근본적으로 다르기 때문이다.
예컨대 아래 그림에서 왼쪽의 사각형은 **L1 distance 관점에서는 원과 같다.** 중심으로부터의 거리가 모두 동일한 점들의 집합이기 때문이다. L2 distance에서는 동일한 집합의 표현이 우리가 아는 원일 것이다.

![스크린샷 2025-03-07 오후 10.46.07.png](/img/user/z-Attached%20Files/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202025-03-07%20%EC%98%A4%ED%9B%84%2010.46.07.png)

L1 distance는 좌표계의 영향을 크게 받는다. 예컨대 좌표계를 회전시킨다면, 모양이 변한다. 반면, L2 distance는 별다른 영향이 없다.

## Voronoi Diagram
평면 상에 여러 점들이 있을 때, 최근접 점이 동일한 영역을 묶어서 구획화한 것이다.
즉, 가장 가까운 1개의 이웃(점)을 기준으로 결정을 내리는 것이므로, k = 1인 경우의 k-NN에서 쓰일 수 있다.

## Delaunay Triangulation
평면 상에 여러 점들이 있을 때, 이 점들을 연결하여 삼각분할을 만드는 다양한 방법이 있다.
들로네 삼각분할은 이러한 분할 중에서 각각의 삼각형들이 **최대한 정삼각형에 가까운 꼴**인 것을 의미한다.

**인접한 보로노이 셀에 속한 점들끼리 이으면**, 점들에 대한 삼각분할이 완성되는데 이는 들로네 삼각분할과 동치이다. (쌍대성duality)

[[Topology Algorithm.\|Topology Algorithm.]]
