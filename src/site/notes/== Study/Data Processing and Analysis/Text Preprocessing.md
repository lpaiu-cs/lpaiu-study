---
{"dg-publish":true,"permalink":"/== Study/Data Processing and Analysis/Text Preprocessing/","created":"2023-12-21T18:08:30.000+09:00","updated":"2025-01-14T15:33:44.000+09:00"}
---

# Text Preprocessing
전처리는 주어진 문서집합을 분석에 용이한 형태로 가공하는 단계이다.
문서들에는 여러 문장들이 막 섞여있을 것이다. 우린 이제부터 이걸 가공해야한다.
가장 먼저, 문서들을 **문장**들로 파싱한다.
그 다음, 문장을 구성하는 기본 요소, 예컨대 **단어**와 같은 형태들로 나눈다.
마지막으로, 단어들을 **정규화**한다.

# 1. Segmenting Sentence
주어진 문서를 그대로 다룰 수 없기 때문에, 먼저 문장이라는 더 작은 단위들로 바꾼다.

>물음표(?)나 느낌표(!)는 비교적 확실하지만, 마침표(.)와 같은 경우는 U.S.A나 Ph.D 등 애매한 경우들이 있기 때문에 나눌 때 주의를 요한다.

# 2. Tokenization
토큰Token은 정보를 나타내는 가장 작은 단위이다.
분석의 목적에 따라 어떻게 토큰화할지 세세한 내용은 달라질 수 있다.
우리는 가장 많이 쓰이는 Word Tokenization을 사용한다.

문장으로부터 쉼표나 따옴표같은 불필요한 문장성분을 제거하고, 단어들만을 뽑아낸다.

>이 때 어퍼스트로피 같은 경우 잘못 제거할 수 있기 때문에 주의를 요한다.

>[!info]
>assignment01이 BPE token learner를 만들어서 직접 토큰화를 해보는 것이었다.

# 3. Normalizing
정규화는 토큰들을 일관된 형식으로 변환하는 것이다.
예컨대, U.S.A 와 USA는 다른 토큰이지만, 동일한 의미를 갖는다. 따라서, 동일한 토큰으로써 사용하기 위해 정규화를 시켜준다.

정규화 예시
- 동치인 단어 정의: Lematization
- 하이픈 제거
- 악센트 제거

## Lematization
단어의 기본형태Lemma를 정의하고 치환한다.

ex)
- am, are, is -> be
- car, cars, car's, cars' -> car

Lemmatization의 간단한 ver으로 **Stemming**이 있다.

