---
{"dg-publish":true,"permalink":"/Insight/Random Walk/","created":"2025-03-17T00:29:51.012+09:00","updated":"2025-03-17T00:52:54.026+09:00"}
---

1/2 확률 동전 토스를 하여 앞면이 나오면 +1,
뒷면이 나오면 -1을 한다.

0에서 부터 시작했을 때, 시행횟수와 최대진폭에는 어떠한 확률적 관계가 있을까?

![Pasted image 20250317004524.png](/img/user/z-Attached%20Files/Pasted%20image%2020250317004524.png)
![Pasted image 20250317004538.png](/img/user/z-Attached%20Files/Pasted%20image%2020250317004538.png)


파이썬 코드로 실험한 결과 위와 같이 시행 횟수가 늘어남에 따른 최대 진폭 그래프를 얻을 수 있었다.

보통 이러한 무작위 걷기(random walk)는 특성상 최대 진폭이 시행 횟수의 제곱근(√n​)과 비례하여 증가하는 경향을 보인다고 한다.

저 뒤가 궁금할 것이다. 조금 더 시행횟수를 늘려보자.

![Pasted image 20250317004913.png](/img/user/z-Attached%20Files/Pasted%20image%2020250317004913.png)
![Pasted image 20250317004955.png](/img/user/z-Attached%20Files/Pasted%20image%2020250317004955.png)


\*코드첨부
```python
import numpy as np
import matplotlib.pyplot as plt

# 실험 설정
np.random.seed(42)
n_trials = 1000

# 동전 토스 실험: 앞면(+1), 뒷면(-1)
coin_flips = np.random.choice([-1, 1], size=n_trials)

# 누적 합 계산
cumulative_sum = np.cumsum(coin_flips)

# 각 시행 횟수마다 최대 진폭 계산
max_deviation = np.array([np.max(np.abs(cumulative_sum[:i])) for i in range(1, n_trials + 1)])

# 결과 시각화
plt.figure(figsize=(10, 6))
plt.plot(range(1, n_trials + 1), max_deviation, lw=1)
plt.title('Maximum Deviation vs Number of Trials')
plt.xlabel('Number of Trials')
plt.ylabel('Maximum Absolute Deviation')
plt.grid(True)
plt.show()
```