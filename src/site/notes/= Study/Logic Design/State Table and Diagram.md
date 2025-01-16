---
{"dg-publish":true,"permalink":"/= Study/Logic Design/State Table and Diagram/","created":"2023-12-04T22:55:46.000+09:00","updated":"2025-01-14T15:33:45.000+09:00"}
---


여태껏 배운 logic들을 이용해서 circuit을 그릴 수 있다.
이렇게 그려진 circuit을 보기좋게 분석할 수 있어야 한다.
State Table과 State Diagram은 그러한 분석의 도구라고 할 수 있다.

### Circuit Diagram
전선, 저항기, 콘덴서 등 전기 회로의 물리적 구성을 표현하는 그림.
** 우리의 수업에서는 해당 내용에 대해 배우지 않는다.

# Analysis & Design Procedure
Logic Diagram에서 아래로 내려가면, Analysis.
State Diagram에서 위로 올라가면 Design Procedure라고 할 수 있다.
## a) Logic Diagram
AND, OR, NOT과 같은 논리 게이트를 이용하여, 디지털 회로의 논리적 작동 방식을 나타낸 그림.

## b) Equation
Analysis나 Design procedure를 수행하기 위해서는, 특정한 input이나 output에 대한 equation을 작성하면 편리하다.
State Table이 주어진 경우, 카르노 맵을 통해 최적화된 equation을 구하여 압축된 로직을 짠다.

## c) State Table
어떤 상태에서 어떤 input을 받으면 어떤 output을 내는지에 대해 표로 나타낸 것.
- Present State
- Input
- Next State
- Output
주로 이렇게 4가지를 표현함. (\*만약 없으면 안씀)

## d) State Diagram
하나 이상의 상태를 갖고, 각 상태에서 어떻게 동작하는 지를 나타낸 그림.
동작은, 특정한 입력 조건에서 특정한 output을 내거나, 다른 상태로 전이하는 것을 의미한다.

# Analysis: 이것은 어떻게 작동하는가?
Logic diagram을 보고 Analysis(분석)하라고 하는 것은 
State Diagram까지 그리라는 의미이다. (Sequential logic일 경우)
State Diagram을 그리기 위해서는 State Table이 필요하다. 이 또한 분석의 결과라 볼 수 있다.

# Design Procedure: 이것을 어떻게 만들 것인가?
State Diagram 또는 State table은 분석의 최종 결과라면,
반대로 이것을 어떻게 구현할지에 대해서 묻는 것이 바로 Design Proccedure이다.
따라서 Analysis의 역 비스무리하다.
1. State Diagram을 보고, State Table을 만든 후, 필요하다면 reduction을 수행한다.
2. 그 다음 각 State에 binary code를 할당하고,
3. 적절한 flip-flop을 선택한 다음,
4. 각 input에 해당하는 최적화된 equation을 구한다.
5. 이에 따라 Logic Diagram을 만든다.

## Synthesis using flip-flops
Design Procedure의 중요한 부분으로, 적절한 flip-flop을 선정하여 Present State와 Input을 통해 Next State를 도출하는 equation을 만든 후, Logic Diagram을 만든다.