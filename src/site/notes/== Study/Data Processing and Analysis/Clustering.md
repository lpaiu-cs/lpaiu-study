---
{"dg-publish":true,"permalink":"/== Study/Data Processing and Analysis/Clustering/","created":"2023-12-19T15:35:12.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
---


군집화

# Topic Modeling
토픽 모델링은 군집화를 통해 분포를 확인하며 [[== Study/Algorithm/Machine Learning Algorithm.#k-NN에 대한 기하학적 접근\|k-NN]]과 마찬가지로 비지도 기계학습에 속한다.

### 모델의 역할
- input:
	text collection
- output:
	발견된 토픽의 집합(원소-단어-를 나열하여 표현)들

### 사람의 역할
출력된 토픽 집합들 중에서 토픽을 결정하고, 해당 집합의 이름을 직접 생각해서 이름 붙여야 한다.


## 알고리즘
- LSI
- LDA

### LDA (Latent Dirichlet Allogcation)

#### 원리:
여기에는 중요한 2가지 전제가 있다.
1. 문서는 하나 이상의 토픽을 가지고 있다.
2. 문서와 단어 사이엔 latent topic layer(잠재된 토픽층)가 존재한다.

여기서 토픽이란 하나 이상의 핵심 keyword의 집합으로 구성된 것을 의미한다.
문서 내 단어들이 잠재적 토픽들과 연결되는 학습 과정을 갖는다.

