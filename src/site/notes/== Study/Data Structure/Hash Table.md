---
{"dg-publish":true,"permalink":"/== Study/Data Structure/Hash Table/","created":"2023-12-04T23:01:45.000+09:00","updated":"2023-12-04T23:01:45.000+09:00"}
---

# Hash Table
데이터 값과 해쉬 함수에 의해 삽입 위치가 결정되는 자료구조

- 충돌의 경우를 무시한다면 삽입, 삭제, 검색에 O(1)만이 소요됨

>충돌 회피 방법
>- Chaining: 충돌 시 연결 리스트 등을 이용하여 동일한 위치에 데이터를 연달아 삽입
>- Linear Probing: 충돌 시 다음 빈칸 등에 대신 적재. (선형적으로 문제를 해결)

