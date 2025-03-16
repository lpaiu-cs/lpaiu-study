---
{"dg-publish":true,"permalink":"/= Study/cs231n/01 Image Classification/","created":"2025-03-07T21:12:21.000+09:00","updated":"2025-03-17T00:57:42.513+09:00"}
---

Lecture 2: Summary
- The data-driven approach   
- K-nearest neighbor   
- Linear classification I

---

# Image Classification
이미지 분류는 컴퓨터 비전에서의 핵심 테스크이다.
이는 근본적 문제와 여러 어려움에 직면한다.

근본적으로, 인간과 컴퓨터의 정보 인지에는 차이가 존재한다.

![스크린샷 2025-03-07 오후 9.24.34.png](/img/user/z-Attached%20Files/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202025-03-07%20%EC%98%A4%ED%9B%84%209.24.34.png)

그리고 몇가지 어려움으로는 광원의 변화, 변형, 가려짐, 혼잡한 배경, 클래스 내부 변동성 등이 있다.

![스크린샷 2025-03-07 오후 9.27.16.png](/img/user/z-Attached%20Files/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202025-03-07%20%EC%98%A4%ED%9B%84%209.27.16.png)

![스크린샷 2025-03-07 오후 9.29.16.png](/img/user/z-Attached%20Files/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202025-03-07%20%EC%98%A4%ED%9B%84%209.29.16.png)

![스크린샷 2025-03-07 오후 9.32.22.png](/img/user/z-Attached%20Files/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202025-03-07%20%EC%98%A4%ED%9B%84%209.32.22.png)

![스크린샷 2025-03-07 오후 9.32.34.png](/img/user/z-Attached%20Files/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202025-03-07%20%EC%98%A4%ED%9B%84%209.32.34.png)

![스크린샷 2025-03-07 오후 9.32.41.png](/img/user/z-Attached%20Files/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202025-03-07%20%EC%98%A4%ED%9B%84%209.32.41.png)


이러한 어려움들을 어떻게 해결할 수 있을까?
먼저 생각하기 쉬운 예로는 윤곽선을 뽑아내어 그림을 간단화 하는 것이 있다.

이러한 정보를 바탕으로 데이터를 학습시키기 위해 선형 분류기를 이용할 수 있다.

# K-Nearest Neighbor Classifier
예전에 알고리즘 수업에서 [[Machine Learning Algorithm.#k-NN에 대한 기하학적 접근\|k-NN]]에 대해 배웠었다. 이는 어떤 데이터의 속성을 k개의 가까운 이웃의 속성을 보고 다수에 속하는 속성으로 분류하는 비지도 학습 알고리즘이다.

그때 나왔던 토픽 중에 거리 척도에 대한 다음과 같은 내용이 있다.

![[Machine Learning Algorithm.#k-NN에 대한 기하학적 접근\|Machine Learning Algorithm.#k-NN에 대한 기하학적 접근#거리 척도Distance Metric]]

따라서 특징 벡터의 각 요소들이 개별적 의미를 담고 있다면 L1 distance가 더 적합할 것이다. 반면, 각 요소의 실질적 의미가 모호한 경우에는 L2 distance가 더 적합할 수 있다.

## kNN on images never used
이러한 kNN이 실제로 이미지에 대해 사용되지는 않는다.
이미지에서 유사성을 판별하기 위한 특성 벡터는 무엇일까?
바로 픽셀이다. 예컨대 32x32 픽셀의 두 그림간 유사성은 각 픽셀 위치와 RGB채널 값을 차원으로 하는 내적을 통해 이뤄질 수 있다. 이 경우 차원의 크기는 32x32x3 = 3072 이다.
e.g. R(1,1), G(1,1), B(1,1), ... , B(32,32).
이미지가 커질 수록 차원의 크기도 기하급수적으로 커지며, 이는 성능상 매우 나쁘다.

![Screenshot 2025-03-13 at 11.24.20 AM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-13%20at%2011.24.20%20AM.png)

뿐만 아니라, kNN은 학습이 존재하지 않기 때문에 각각의 이미지 판단마다 모든 훈련 데이터를 처음부터 참조하여 유사성을 계산해야 한다. 어떤 이미지가 주어졌을때 비행기인지 사람인지를 구분해내기 위해 비행기 세트와 사람 세트로 구분된 이미지들간의 유사성을 각각 판단해야 하는 것이다. 이것은 매우 비효율적이다.

마지막으로 픽셀들의 위치에 따른 distance metric이 공간적 위치 정보를 반영하지 못한다. 각각의 특성 벡터가 완전히 독립적으로 간주되어 나열되기 때문에 실제 공간상 거리와 방향은 물론 약간의 변형에도 취약해진다.

![Screenshot 2025-03-13 at 11.47.11 AM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-13%20at%2011.47.11%20AM.png)


# Linear Classification
선형 분류기는 Neural Network의 핵심이다. NN는 선형 분류기를 벽돌처럼 쌓아 만드는 것이기 때문이다.

CIFAR10 데이터 세트를 바탕으로 분류기를 학습 시켜보자. 이 이미지들은 비행기, 오토바이, 새, 고양이 등 10개의 클래스로 구성되어 있다.
![Screenshot 2025-03-13 at 12.07.04 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-13%20at%2012.07.04%20PM.png)

선형 분류기의 구조는 간단하게 아래 그림처럼 나타낼 수 있다.
이미지가 들어오면, 이 이미지를 분류기 f가 파라미터를 통해 계산하고 결과를 내놓는다.

![Screenshot 2025-03-13 at 12.13.30 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-13%20at%2012.13.30%20PM.png)

f는 단지 이미지 x와 파라미터 W간의 매트릭스 연산을 의미한다.
$$f(x, \text{W}) = \text{W}x$$
그렇다면 그 결과 10개의 차원을 가진 벡터가 각 차원에 10개의 분류에 해당하는 점수를 산출하게 된다.
여기에 바이어스 b는 결과 차원과 동일한 값을 더해주게 된다.

![Screenshot 2025-03-13 at 12.18.49 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-13%20at%2012.18.49%20PM.png)

이때 차원을 맞춰주는 것이 매우 중요하다. 이를 이해하기 위해서는 [[= Study/Linear Algebra/00 Linear Algebra INDEX_\|선형대수학]]의 기초가 필요하다.

간단한 상황으로 예시를 들어보자
다음은 x = 1x4 인 4픽셀 1채널 이미지이다. 분류 클래스는 3이다.
따라서 학습이 끝난 파라미터 W = 3x4 매트릭스 이다.

![Screenshot 2025-03-13 at 3.50.19 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-13%20at%203.50.19%20PM.png)

그 결과 스코어가 가장 높은 것은 Dog이므로, 이 이미지는 Dog로 분류된다. 잘못 분류되었지만 우리의 분류기는 그 결과로부터 무언가를 수정하고 배울 것이다.

### Templete

이 외에도 수많은 데이터를 통해 학습이 완료된 분류기, 즉 파라미터 W의 각 행은 해당 분류를 위한 무언가 기준이 되는 표본으로서의 이미지를 띠게될 것이다. 이를 **템플릿**이라고 하는데 이를 이미지화한 템플릿을 관찰하면 분류기의 내적에서 어떤일이 일어나는지 대략 짐작해볼 수 있다.

![Screenshot 2025-03-13 at 3.58.03 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-13%20at%203.58.03%20PM.png)

위 그림 하단에 보면 10개의 클래스가 갖는 각각의 템플릿들을 확인할 수 있다.
하지만 보이듯 각 템플릿들은 굉장히 이상하다. 예컨대 horse 클래스의 경우 말 비스무리하게 생겼지만, 머리가 양쪽에 모두 달려있는 것 같다. 왜냐하면 어떤 이미지의 경우 왼쪽에 머리가 있고 또 어떤 경우에는 오른쪽에 머리가 있기 때문이다. 이처럼 **문제는 각 클래스에 대해 단 하나의 템플릿만을 학습**하기 때문에 한 클래스 내에 존재하는 다양한 특징들을 평균화 시켜서 갖게 된다는 것이다.

추후 배울 Neural Network와 같이 더 복잡한 모델에서 이를 해결할 것이다.

## Interpreting a Linear Classifier

여러 차원으로 이루어진 각 이미지를 고차원 공간의 한점으로 생각해보자.
그러면, Linear Classifier는 각 클래스를 구분하는 선형경계를 학습하여 긋는 역할을 한다

![Screenshot 2025-03-13 at 4.34.45 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-13%20at%204.34.45%20PM.png)

이러한 Linear Classification이 직면할 수 있는 아래와 같은 문제들이 있다는 것도 참고하자.

![Screenshot 2025-03-13 at 4.37.50 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-13%20at%204.37.50%20PM.png)


# Training without Overfitting
그런데 파라미터 학습 시에 데이터를 무작정 학습할 시에는 오버피팅이 발생할 수 있다.  
만약, 아래 그림의 Idea 1처럼 데이터 세트를 모두 train set으로 사용하고, 그 정확도로 성능을 평가한다면 학습 데이터가 아닌 데이터에 대해서는 좋은 성능을 보일 수 없게된다. 그냥 해당 데이터에 오버피팅된 파라미터값이 되어, 한 번도 본적없는 데이터에 대한 유의미한 예측값을 나타내지 못한다.

따라서 Idea 2처럼 일부 데이터는 학습에 사용하지 않고 test를 통한 성능 척도로만 사용하는 시도를 할 수 있다. 이 경우 train 데이터 자체를 나타내는 학습이 발생하지는 않겠지만, 여러 하이퍼파라미터 중 test 데이터에서만 높은 성능을 나타내는 하이퍼파라미터로 오버피팅이 발생하게 된다. 이것은 정확한 성능을 표현하지 못한다.

그렇기 때문에 validation set을 추가로 할당하여 이를 통해 검증하고, test는 완전히 간접적으로나마 터치되지 않은 데이터로 놔두어 검증할 수 있게 된다.

![스크린샷 2025-03-12 오후 3.49.19.png](/img/user/z-Attached%20Files/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202025-03-12%20%EC%98%A4%ED%9B%84%203.49.19.png)

즉, 파라미터에 대한 오버피팅을 피하기 위해 하나. -validation
하이퍼파라미터에 대한 오버피팅을 피하기 위해 또 하나를 두는 셈이다. -test

또 다른 방법 중에 하나로, Cross-Validation이라는 방법이 존재한다.
데이터가 적은 경우, train / valid 로 지정해 버리는 대신, 번갈아가며 일부를 valid set으로 사용한다.

![Screenshot 2025-03-12 at 4.16.01 PM.png](/img/user/z-Attached%20Files/Screenshot%202025-03-12%20at%204.16.01%20PM.png)
