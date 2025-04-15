---
{"dg-publish":true,"permalink":"/== Study/Algorithm/Sort/","created":"2023-12-04T23:03:42.000+09:00","updated":"2025-01-14T15:33:43.000+09:00"}
---

- Bubble Sort
- Insertion Sort
- Selection Sort
- Heap Sort
- Radix Sort
---
무작위 수열에서 정렬을 수행하기 위한 가장 기본적인 알고리즘은 [[== Study/Algorithm/Bubble Sort\|Bubble Sort]], [[== Study/Algorithm/Insertion Sort\|Insertion Sort]], [[== Study/Algorithm/Selection Sort\|Selection Sort]] 이 3가지로, 이들은 모두 최악 시간복잡도가 O($n^2$)이다.
하지만 Heap을 이용하면 O($n$log$n$)에 정렬할 수 있다. 그것이 [[== Study/Algorithm/Heap Sort\|Heap Sort]]이다.
위 4개를 두 원소간의 비교를 통해 정렬을 한다 하여, **비교정렬**이라 하며, 비교정렬의 최악시간복잡도 하한은 O($n$log$n$)보다 낮아질 수 없다.
뿐만아니라 특수한 상황(예컨대 최대 자릿수를 아는 경우)에는 [[== Study/Algorithm/Radix Sort\|Radix Sort]]를 사용해 O(n)만에 정렬할 수도 있다.

>분할정복에 포함된 sort
>- [[== Study/Algorithm/Merge Sort.\|Merge Sort.]]
>- [[== Study/Algorithm/Quick Sort.\|Quick Sort.]]

