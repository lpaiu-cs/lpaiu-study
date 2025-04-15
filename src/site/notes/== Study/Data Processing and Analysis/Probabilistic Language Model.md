---
{"dg-publish":true,"permalink":"/== Study/Data Processing and Analysis/Probabilistic Language Model/","created":"2023-12-07T04:24:00.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
---

# Probabilistic Language Model
단어, 문장 또는 문서가 생성될 확률을 추정하여 언어의 구조를 모델링한다.

# N-gram 모델
이전 N-1개의 단어로부터 다음 단어를 예측하는 확률 모델.
(N-gram은 길이가 N인 토큰 열을 의미한다)

## 단어 예측
"나는 어제 친구와 영화를" 다음에 나올 법한 단어를 어떻게 예측할까? #Q 순서는 고려되나?
가장 단순한 방법으로 아래와 같은 생각을 할 수 있다.
$$P(\text{봤다 } | \text{ 나는, } \text{ 어제, } \text{ 친구와, } \text{ 영화를}) = \frac{\text{freq}(\text{나는}, \text{ 어제}, \text{ 친구와}, \text{ 영화를}, \text{ 봤다})}{\text{freq}(\text{나는}, \text{ 어제}, \text{ 친구와}, \text{ 영화를})}$$
그러나 이 방식으로 확률을 추정하는 것은 불가능하다.
특히 sequence가 길어질 수록, 분자나 분모가 0이 될 가능성이 높다.

그래서 우리는 마르코프 가정Markov assumption을 이용해야 한다.
### Markov assumption
$$P(\text{봤다 } | \text{ 나는}, \text{ 어제}, \text{ 친구와}, \text{ 영화를}) \approx P(\text{봤다 } | \text{ 영화를})$$
예측 대상 단어와 가까운 단어가 가장 큰 영향을 미친다는 의미이다. 이것은 예측 대상어와 직전 단어 총 2개만으로 확률을 추산하기 때문에 **2-gram 모델**이라 한다.
여기서 모델 사이즈를 키우면, 더 정확해진다. 예컨대,
$$P(\text{봤다 } | \text{ 나는}, \text{ 어제}, \text{ 친구와}, \text{ 영화를}) \approx P(\text{봤다 } | \text{ 친구와 }, \text{ 영화를})$$
이것은 **3-gram 모델**이다.


>이처럼 단어의 생성 확률을 추정하는 방법에 대해 알아보았다.
>마지막으로 마르코프 가정을 응용해서 길이가 n인 **문장**의 확률도 계산해보자. 
> $$P(w_{1,n}) \approx \prod_{i=1}^{n} P(w_i | w_{i-1})$$
