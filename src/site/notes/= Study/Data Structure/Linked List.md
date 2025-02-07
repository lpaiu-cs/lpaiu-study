---
{"dg-publish":true,"permalink":"/study/data-structure/linked-list/","created":"2023-12-04T23:01:35.000+09:00","updated":"2023-12-04T23:01:35.000+09:00"}
---

# Linked List

- 각각의 요소가 다음 요소를 가리키는 형태
- 각 요소를 노드Node 라고 부른다; 각 Node는 data와 next를 갖는다.

>**연결리스트에서 데이터 접근**
>
>- 접근: O(n)
>- 삽입: O(1) << 바로 앞 Node를 알고 있다는 전제 필요.
>- 삭제: O(1) << 대상 직전 Node를 알고 있다는 전제 필요.

이처럼 삽입과 삭제는 빠르지만 삽입 삭제를 수행하기 위해서는 느린 접근을 해야하기 때문에 단점이 있다.

## Doubly Linked List

- 각각의 요소가 다음과 이전 요소를 가리키는 형태


## Circular Linked List

- 제일 앞 요소(head)와 제일 뒤 요소(last)를 연결한 연결리스트

## Doubly Circular Linked List

* 원형 연결리스트를 양방향으로 연결한 것.

