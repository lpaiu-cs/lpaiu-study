---
{"dg-publish":true,"permalink":"/= Study/cs224n(COSE461)/01 Word Vectors/","created":"2025-03-13T21:14:27.000+09:00","updated":"2025-03-17T00:53:15.729+09:00"}
---

Lecture 1: Introduction and Word Vectors
Human language and word meaning
Word2vec introduction
Word2vec objective function gradients
Optimization basics

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
[[= Study/Data Processing and Analysis/Vector Semantics\|Vector Semantics]]에서 여러가지 형태의 공기어 벡터를 다루어 보았지만, 무언가를 하나 깊게 다뤄보진 않았었다. 지금부터 조금 더 깊게 다뤄볼 예정이다.

# Word vectors
*"You shall know a word by the company it keeps"*, J.R. Firth 1957:11.
퍼스에 따르면 단어의 의미는 독립적으로 존재하는 것이 아니라 주변에 함께 쓰이는 단어들과의 관계(context) 속에서 결정된다.

이러한 아이디어에 착안해서 비슷한 맥락에서 사용되는 단어는 서로 유사성을 띠도록 단어의 체계를 구성하였다.
간단한 예시로 아래의 그림처럼 두 개의 단어 banking과 monetary를 이처럼 표현할 수 있다. 이 경우에 두 벡터의 내적을 통해 유의성이 표현된다. 각 차원의 부호가 같은 경우 유사성이 커지는 식으로 말이다.
![Screenshot 2025-03-14 at 9.35.49 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-14%20at%209.35.49%20PM.png)

이제 문제는 각 벡터의 차원값, 즉 파라미터들을 어떻게 학습할 수 있는가? 이다.

# Word2vec
Word2vec은 단어를 벡터화시키기 위한 학습 프레임 워크이다.

## Skip-gram
Skip-gram은 Word2vec 기법 중 하나로, 주어진 중심단어(center word)로부터 주변부 단어(outside word, context word)를 예측하는 신경망 기반 학습 모델이다.

![Screenshot 2025-03-14 at 9.59.24 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-14%20at%209.59.24%20PM.png)

중심 단어 $w_t$가 주어졌을 때, 주변부 단어 $w_{t+j}, (j \neq 0)$가 등장할 확률을 계산하고, 이러한 확률을 바탕으로 Likelihood를 계산



결국 우리는 $P(w_{t+j} \mid w_t)$를 