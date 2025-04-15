---
{"dg-publish":true,"permalink":"/== Study/Data Structure/Queue/","created":"2023-12-04T23:02:26.000+09:00","updated":"2023-12-04T23:02:26.000+09:00"}
---

# Queue
First In First Out 선입선출의 형태.
**대기열**이라는 의미를 가진다.
자료구조의 최후방에 새로운 데이터가 삽입되고, 최전방의 데이터가 제거된다.
각각의 위치를 **Rear**와 **Front**라 부른다.
### enqueue
rear에 새로운 데이터를 삽입

### dequeue
front에 위치한 데이터를 제거


## Circular Queue

- 선형이 아닌 환형 대기열의 형태.
- 일반적으로 **크기를 고정**

>rear와 front는 각각 삽입 및 제거시에 이동한다.
>- rear는 삽입 시, 삽입된 데이터와 동일한 위치를 가리킴.
>- front는 제거시, 제거되는 데이터의 바로 뒤 데이터를 가리킴.


## Double-Ended Queue (Deque)

- front와 rear 모두에서 삽입 및 제거가 자유롭다.
