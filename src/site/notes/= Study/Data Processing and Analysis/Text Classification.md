---
{"dg-publish":true,"permalink":"/study/data-processing-and-analysis/text-classification/","created":"2023-12-19T19:11:46.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
---

# Text Classification
문서를 미리 정한 카테고리로 분류하는 작업을 의미한다.

<해결하고 싶은 문제들>
- 주제 찾는 문제
- 스팸 판단 문제
- 저자 판별 문제
- 저자성별 판단 문제
- 어조 긍/부정 판단 문제

이러한 문제들은 결국, 텍스트를 어디로 분류할건지에 관한 문제다.

> Input:
> - a document $d$
> - a fixed set of classes $C$ = {$c_1, c_2,..., c_j$}

> Output:
> - a predicted class $c \in C$

# Classification Methods: [[Machine Learning Algorithm.\|Supervised Machine Learning]]

- NaÏve Bayes
- Logistic regression
- Support-vector machines
- k-Nearest Neighbors

## NaÏve Bayes


# Aurthorship Attribution (저자판별) - 기계학습 기반

### 전산 문체론:
저자의 문체에 계량적이고 식별 가능한 특성이 존재할 것이다.

## Feature Vector

텍스트 -> 자질(feature)를 추출하여 벡터로 표현 -> 학습데이터 #Q 자질이라는 용어가 공식적인가? 왜 특징이나 특성이라고 안하지.
분류기 학습

### feature set
- 문자(음절)
- 형태소
- 기능 형태소
- 품사
- 어절

각 feature들은 각각 분류된 후, n-gram으로 필요한만큼 붙여 사용하며, 각 요소들은 feature space의 차원이 된다.

### Feautre Selection
학습 데이터에 overfiting되는 것을 막기 위해, 불필요한 feature들을 제거할 필요가 있다. (Curse of dimensionality 방지)
여러가지 방법이 있겠지만 본지에선 **빈도**를 사용한다.
임계값threshold를 설정하고 cutoff 한다. 이 방법은 가장 효과적이면서 단순한 방법이라고 할 수 있다.

### SVM (Support Vector Macine)
가장 높은 성능을 보이는 기계학습 모델
선형 분류기의 일종: 입력 예제로부터 maximum-margin hyperplane을 찾는다. 즉, 가장 구분이 잘되는 지점을 찾아서, 명확한 구분선을 제시한다.

즉, input으로 문서들을 받고, 각 문서의 feature 벡터를 만들고, 이 벡터를 바탕으로 SVM을 하여, 분류한다.

## 문체 특징 학습
단일 feature 벡터를 이용하여 SVM을 시행하여 정확도를 측정.
의미 있는 feature를 지정하고. 이후, 여러종의 feature 벡터를 조합해가며, 더 정확한 조합을 찾는다. 이후 선택한 조합을 바탕으로, 저자의 문체를 특정한다. 즉, 대표되는 feature 벡터를 선택하는 것.

### 오류분석
정확한 조합을 찾는 과정에서, 혼동행렬 (confusion matrix)를 만들면, 자주 틀리는 지점을 발견하는 등 문제점을 분석하기 쉽다.

### MDS (Multi Dimentional Scaling) 다차원 척도법
저자/소설 분포 등을 수치상으로 보는 것은 정확하고 연구 데이터로서 가치가 있지만, 시인성이 떨어진다. 시각화만큼 설명하기 쉬운건 없다. 하지만 우리가 다루는 벡터의 차원은 수천 수만, 어쩌면 그 이상으로 무수히 많을 것이다. 이러한 벡터들 간의 관계를 어떻게 시각화 할 수 있을까?
이에 대한 해답이 바로 이것이다. 기본적인 원리는 유사한 객체들을 가깝게 배치하여 나타낸다. 물론, 뭉게지는 면은 피할 수 없다.


## 저자식별
문체 특징을 학습할 때 선택한 것과 동일한 조합으로 저자를 식별하고 싶은 대상 문서의 feature vector를 형성한다. 이후 이 벡터와 각각의 저자 문체 feature vector와의 유사도를 계산하여 가장 유사한 저자를 선택한다.
이때, **유사도**를 어떻게 판단할 것인가에 대해서는 다양한 옵션이 있다.

## 차원의 값
지금까지 feature vector의 형성은 절대빈도를 차원의 값으로 하여 설명했지만, 실제로는 다양한 방법들이 있다.

- #### raw frequency(절대빈도):
말 그대로 절대빈도이다. 문서 내에 등장한 횟수를 센다.
$$\text{freq}_{\text{abs}}(f_i) = \text{freq}(f_i)$$
- #### relative frequency(상대빈도):
일종의 통계적 확률. 대상 차원의 절대빈도를 모든 차원의 절대빈도 합으로 나눈다.
$$\text{freq}_{\text{rel}}(f_i) = \frac{\text{freq}(f_i)}{\sum_{k} \text{freq}(f_k)}$$
- #### z-score:
우리가 잘아는 정규분포의 그 값.
$$z(f_i) = \frac{\text{freq}(f_i) - \mu_i}{\sigma_i}$$
- #### Standard deviation-normalized frequency:
평균을 0으로 맞춰주지 않은걸 제외하면 z-score와 동일.
$$\text{norm}_{\text{std}}(f_i) = \frac{\text{freq}(f_i)}{\sigma_i}$$

## 유사도 척도
임의의 두 벡터가 유사하다는 것을 나타내는 방법은 다양하다.
- Euclidean distance
- Manhattan distance
- Cosine similarity
- Pearson correlation coefficient

#### Euclidean distance
$$d_{euc}(\vec{a}, \vec{b}) = \sqrt{\sum_{i=1}^{|V|} (a_i - b_i)^2}$$
#### Manhattan distance
$$d_{mht}(\vec{a}, \vec{b}) = \sum_{i=1}^{|V|} |a_i - b_i|$$
#### Cosine similarity
$$\cos(\vec{a}, \vec{b}) = \frac{\vec{a} \cdot \vec{b}}{\|\vec{a}\| \|\vec{b}\|} = \frac{\sum_{i=1}^{|V|} a_i b_i}{\sqrt{\sum_{i=1}^{|V|} a_i^2} \sqrt{\sum_{i=1}^{|V|} b_i^2}}$$

#### Pearson correlation coefficient:
두 변수 사이의 선형관계를 나타내는 값으로, -1 ~ 1 사이의 값을 갖는다.
	+1에 가까움: 양적 선형 관계
	-1 에 가까움: 음적 선형 관계
	0에 가까움: 독립에 가까움.
$$r = \frac{\sum_{i=1}^{|V|} (a_i - \overline{a})(b_i - \overline{b})}{\sqrt{\sum_{i=1}^{|V|} (a_i - \overline{a})^2} \sqrt{\sum_{i=1}^{|V|} (b_i - \overline{b})^2}}$$
이때, 거리 척도는 1/(1+거리)로 유사도로 변환한다.
