---
{"dg-publish":true,"permalink":"/== Study/Logic Design/Mealy and Moore models/","created":"2023-12-18T03:21:06.000+09:00","updated":"2025-01-14T15:33:45.000+09:00"}
---


[[== Study/Logic Design/Sequential Logic\|Sequential Logic]]은 과거의 input을 저장했다가 사용할 수 있는 로직을 의미했다. 그 말 뜻은 **과거의 input이 현재의 상태를 규정**한다는 말과도 동일하다. Finite State Machine에서 State란, 어떤 input이 들어올 때, 그 input에 어떻게 반응(output)하는 지를 나타내는 것이다.

아래의 두 모델은 모두 Sequential Loigc에 속하며, output이 input에 영향을 받는 타이밍만이 다를 뿐이다. 실질적으로 두 모델은 언제나 동치관계로 전환될 수 있다.

## Mealy model
output이 present state와 input에 의해서 결정되는 로직.

- Inputs must be synchronized with the clock // 입력은 언제나 클럭 엣지에 들어온다는 의미.
- Outputs must be sampled at the clock edge // 출력은 언제나 클럭 엣지일 때, 샘플링된 값에 종속된다는 의미.

## Moore model
output이 present state에 의해서만 결정되는 로직.
밀리 모델의 모든 state에서 각 input에 대해 output이 동일하다면, 무어 모델이 된다고 볼 수 있다.

- Outputs are synchronized with the clock // 클럭에 동기화된 때에 출력된다. 