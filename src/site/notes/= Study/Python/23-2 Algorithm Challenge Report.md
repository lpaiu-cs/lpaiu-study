---
{"dg-publish":true,"permalink":"/= Study/Python/23-2 Algorithm Challenge Report/","created":"2023-12-26T19:47:48.000+09:00","updated":"2025-01-14T15:33:46.000+09:00"}
---

# 개요
### Input
- 첫 번째 줄에 𝑁가지 다각형의 이름이 주어집니다. 𝑁은 2 이상 10 미만의 자연수입니다. 
- 두 번째 줄에 첫 번째 다각형을 구성하는 꼭짓점들의 𝑥좌표들이 한 칸간격으로 주어집니다. 세 번째 줄에 첫 번째 다각형을 구성하는 꼭짓점들의 𝑦좌표가 주어집니다. 이러한 방식으로 𝑁번째 다각형의 𝑥좌표는 2N번째 줄에, 𝑦좌표는 2N+1번째 줄에 주어집니다.
- 좌표는 반드시 점 순서대로 주어지며, 이때 점 순서는 다각형을 그리는 순서와 같습니다.
- 좌표로 주어지는 값의 범위는 -100 이상 100 미만의 정수입니다. 
### Output
 주어진 모든 다각형들이 겹친 부분의 넓이를 정수단위로 반올림하여 출력.

## Ex
![Pasted image 20231226194745.png](/img/user/z-Attached%20Files/Pasted%20image%2020231226194745.png)
>X Y Z
 -2 -5 -1 4 4
 6 1 -1 -1 3
 -1 -4 -4 2 3
 3 -1 -4 -4 5
 1 -3 2 6
 2 -2 -3 -1

 \>>> 8

# 코드 및 주석
```python
####### 2023-2 Algoirhm Challenge Assignment ###########################
####### 2022320140 Computer Science & Engineering Kim June Young #######

x = 0
y = 1

# 픽의 정리 근사를 보다 정확하게 하기 위해 사용할 배수이다. time-accurate trade off
scale = 5

x_min = scale * -100
x_max = scale *  100 
y_min = scale * -100
y_max = scale *  100

endValue = x_max
########################## 각각의 도형(shape의 원소)에 point집합으로 정의됨.

# 반직선 AB에 대해 점 C가 위치한 지점 판단하기
def LR(A, B, C):    #AB x AC
    rst = A[x]*B[y] + B[x]*C[y] + C[x]*A[y] - A[x]*C[y] - B[x]*A[y] - C[x]*B[y]
    if rst > 0:
        return 'Left'
    elif rst < 0:
        return 'Right'
    else:
        return 'Stand'
    
# 선분 AB와 선분 CD의 교차여부 판단하기
def is_Cross(A, B, C, D):  
    if 'Stand' in [LR(A, B, C), LR(A, B, D), LR(C, D, A), LR(C, D, B)]:
        return False    # Stand인 경우 완전 배제
    elif LR(A, B, C) != LR(A, B, D) and LR(C, D, A) != LR(C, D, B):
        return True 
    return False

######################################################################
# 점 P가 선분 AB위에 있는지 판단하기
def is_point_on_line(A, B, P):
    # 두 점이 같은 경우, 선분이 아니므로 False 반환
    if A[x] == B[x] and A[y] == B[y]:
        return False

    return LR(A, B, P) == 'Stand' and min(A[x], B[x]) <= P[x] <= max(A[x], B[x]) and min(A[y], B[y]) <= P[y] <= max(A[y], B[y])

######################################################################

# 주어진 점이 도형의 내부에 있는지 판단하기
def is_Inside(point, shape):    # point는 (x, y) 꼴, shape는 [point1, point2, ...] 꼴

    count = 0
    length = len(shape)
    shape = shape.copy()
    shape.append(shape[0])

    for i in range(length):
        if is_point_on_line(shape[i], shape[(i+1 + length)%length], point):
            return "Boundary"
        if shape[i][y] == shape[i + 1][y]:
            count = count   # ㅡ자 선분 무시
        elif shape[i][y] == point[y] and point[x] < shape[i][x]:
            if shape[i + 1][y] > shape[i][y]:
                count += 1  # point의 x와 시점이 동일선상 오른쪽에 있고 올라가는 선분 카운트
        elif shape[i + 1][y] == point[y] and point[x] < shape[i + 1][x]:
            if shape[i][y] > shape[i + 1][y]:
                count += 1  # point의 x와 종점이 동일선상 오른쪽에 있고 내려가는 선분 카운트
        else:
            if is_Cross(point, (endValue, point[y]), shape[i], shape[(i+1 + length)%length]):
                count += 1  # point의 x와 시점 종점 모두 동일선상에 있지 않는 교차 선분 카운트
    if count%2 == 0:
        return "Outside"
    else:
        return "Inside"

######################################################################
######################################################################

shapes = input().split()
shapes_num = len(shapes)
# input shapes 
for i in range(shapes_num):
    temp_x = list(map(int, input().split()))
    temp_y = list(map(int, input().split()))

    temp_x = [_ * scale for _ in temp_x]
    temp_y = [_ * scale for _ in temp_y]
    
    shapes[i] = []
    for j in range(len(temp_x)):
        shapes[i].append((temp_x[j], temp_y[j]))

# 격자 범위 줄이기 (시간복잡도를 확연히 줄이기 위해 필수적인 옵션.)
for shape in shapes:
    # 전략: 각 shape의 최솟
    shape_x_min = min(shape, key= lambda _: _[0])[x]    # 각 shape의 점들 중 최소의 x값 구하기
    shape_x_max = max(shape, key= lambda _: _[0])[x]    # 각 shape의 점들 중 최대의 x값 구하기
    shape_y_min = min(shape, key= lambda _: _[1])[y]    # 각 shape의 점들 중 최소의 y값 구하기
    shape_y_max = max(shape, key= lambda _: _[1])[y]    # 각 shape의 점들 중 최대의 y값 구하기
    
    if x_min < shape_x_min:
        x_min = shape_x_min
    if x_max > shape_x_max:
        x_max = shape_x_max
    if y_min < shape_y_min:
        y_min = shape_y_min
    if y_max > shape_y_max:
        y_max = shape_y_max

# 범위내의 모든 점들이 도형 내에 있는지 스캔하기 주어진 범위는 -100 ~ 100 -> -1000 ~ 1000
# 격자점 스캔: 내부 및 경계에 있는 격자점을 각각 기록하기
internal_points = []
boundary_points = []
for x_ in range(x_min, x_max + 1):
    for y_ in range(y_min, y_max + 1):
        point = (x_, y_)    # 격자점 포인터

        plag = 0
        for shape in shapes:
            if is_Inside(point, shape) == "Outside":
                break
            elif is_Inside(point, shape) == "Boundary":
                plag += 1
        else:
            if plag == 0:
                internal_points.append(point)
            else:
                boundary_points.append(point)

# 픽의 정리를 활용하여, 도형의 넓이 산출
area = round((len(internal_points) + len(boundary_points)/2 - 1) / (scale**2))

print(area)
```
\*코드에 대한 설명은 주석으로 갈음하였다.

# 코드 작성 과정에서 느낀 점 및 소회

처음엔 쉽게 생각했었던 문제가 풀면 풀수록 까다롭게 느껴졌다. 조건으로 주어진 다각형이라는 표현도, 볼록다각형이 전제되어있지 않기 때문에 발생하는 문제들을 해결하는 과정에서 여러 조건들에 대해 생각하게 만들었다.
문제의 의도는 픽의 정리를 사용하라는 것일 것 같다. 그런데, 한가지 전제인, 모든 꼭짓점이 격자위에 존재해야 한다는 것을 맞추기가 어려웠다. 그래서 생각한 것이 scaling이다. 격자의 스케일을 변경하면 모든 점들이 격자에 있는것과 비슷한 효과를 낼 수 있다. 이 효과는 반대로 도형의 크기를 증가시킴으로써 쉽게 반영할 수 있다. 예컨대 (1,1)에 위치한 점은 (10, 10)에 위치한것과 동일하게 파악하는 것이다. 이것은 도형의 스케일을 길이로는 10배, 넓이로는 100배 증가시키는 효과를 발생시킨다.
이러한 픽의 정리를 가장 잘 활용하려면, 도형을 정하고 도형 내의 점들을 찾는게 아닌, 격자점들 각각이 도형내부에 존재하는지를 판단하는 것이 가장 좋다. 그렇게 하기 위해, 범위내의 모든 점인 -100~100의 가로세로 점들 총 4만여개의 점들에 대해 판별하기로 하였다. 그러나 이것은 너무 많은 시간복잡도를 발생시켰다. 만약 여기서 도형의 스케일을 10배로 키운다면, 범위는 -1000~1000으로 4백만개의 점들을 판별해야 되게 된다. 이는 끔찍한 효율을 만들어낸다. 실제로 이렇게 예시 입출력을 시행해 보았을 때, 시간이 대략 5분정도 걸렸다. 이를 해결하기 위해 추가한 코드 블럭이 이것이다.
```python
# 격자 범위 줄이기 (시간복잡도를 확연히 줄이기 위해 필수적인 옵션.)
for shape in shapes:
    # 전략: 각 shape의 최솟
    shape_x_min = min(shape, key= lambda _: _[0])[x]    # 각 shape의 점들 중 최소의 x값 구하기
    shape_x_max = max(shape, key= lambda _: _[0])[x]    # 각 shape의 점들 중 최대의 x값 구하기
    shape_y_min = min(shape, key= lambda _: _[1])[y]    # 각 shape의 점들 중 최소의 y값 구하기
    shape_y_max = max(shape, key= lambda _: _[1])[y]    # 각 shape의 점들 중 최대의 y값 구하기
    
    if x_min < shape_x_min:
        x_min = shape_x_min
    if x_max > shape_x_max:
        x_max = shape_x_max
    if y_min < shape_y_min:
        y_min = shape_y_min
    if y_max > shape_y_max:
        y_max = shape_y_max
```
이를 통해 판별 대상이 되는 격자점을 4만개에서 25개(예시입출력 기준)로 줄일 수 있었다. 이는 1600배나 빠른 실행을 만들어냈다. 스케일이 10배로 증가한 상태에서도 마찬가지이다. 

---
이번 첼린지 과제를 하면서 기하 알고리즘에 대해 정말 심도있게 고민해볼 수 있는 시간이었다. 처음으로 마주한 기하 알고리즘을 정말 재밌게 풀어볼 수 있었고, 또 수업 중에 만들어봤던 함수들을 직접 활용하여 문제를 푸는 과정이 소스코드를 이용한 작업효율성 제고에 대해 깊게 생각해볼 수 있었다. 실제로 코드에 있는 주석은 과제를 위한 주석이 아닌, 시인성을 높이기 위해 스스로 과제를 푸는 과정에서 사용한 주석이다. 이러한 점에 대해 깊은 생각과 깨달음을 얻을 수 있는 기회가 되어 정말 좋았다.

# 실행 결과 캡쳐 이미지

예시 입출력은 3장의 실행 결과에서 모두 동일하며, scale 변수만 조정하였다.
![Pasted image 20231226015332.png](/img/user/z-Attached%20Files/Pasted%20image%2020231226015332.png)
1) scale 변수의 값이 1인 상황이다.
이 경우에는 boundary_points의 len( )이 보다시피 7로 경계점 7개를 잡아내었다.
또한 internal_pointns의 크기는 5였다.
이를 픽의 정리를 통해 계산하면, area = 5 + 7/2 -1 = 7.5 이고, 반올림하여 8이 출력된다.

![Pasted image 20231226015720.png](/img/user/z-Attached%20Files/Pasted%20image%2020231226015720.png)
2) scale 변수의 값이 10인 상황이다.
이 경우에는 len(boundary_points) = 69, len(internal_points) = 771로 픽의 정리에 따르면 넓이는 = 771 + 69/2 - 1 = 809.5가 된다. scale이 10배인 상황이므로 넓이는 100배이다. 따라서 8.095가 되며, 이를 반올림하면 8이 된다.

![Pasted image 20231226020409.png](/img/user/z-Attached%20Files/Pasted%20image%2020231226020409.png)
3) scale = 100인 경우이다.
len(boundary_points) = 688, len(internal_points) = 80177이다.
픽의 정리에 따르면 scale 넓이 = 80177 + 688/2 - 1 = 80520 이고
원래 scale에서의 넓이는 8.0520이 된다. 따라서 반올림하면 8이다.


이로써 scale을 높일 수록 정확한 값에 근사해 가는 모습을 볼 수 있었다. 픽의 정리에서 각 꼭짓점이 격자에 있어야 한다는 가정은, 각 꼭짓점의 개수만큼의 점을 무시할 수 있을 정도로 scale을 높이면 근사적으로 해결되는 문제임을 알 수 있다. 따라서 나의 해결방법은 time-accurate를 trade off할 수 있는 근사문제 해결법이라 할 수 있다.

#topology_algorithm
#brute_force
