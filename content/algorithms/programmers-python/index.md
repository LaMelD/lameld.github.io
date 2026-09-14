---
title: "프로그래머스 Python 풀이 노트"
date: 2025-10-02
weight: 5
tags: [algorithm, python, programmers]
---

[algorithms/notebooks/programmers.ipynb](https://github.com/LaMelD/algorithms/blob/master/notebooks/programmers.ipynb)에 정리한 프로그래머스 Python 풀이를 글로 옮겼다. 문제 제목을 누르면 프로그래머스 문제 페이지로 간다. 뒤쪽에는 [python/programmers](https://github.com/LaMelD/algorithms/tree/master/python/programmers)의 초기 스크립트 풀이 6개를 붙였다.

## 노트북 풀이

### [^2 배열 자르기](https://school.programmers.co.kr/learn/courses/30/lessons/87390)

- 정수 n, left, right가 주어집니다. 다음 과정을 거쳐서 1차원 배열을 만들고자 합니다.

    1. n행 n열 크기의 비어있는 2차원 배열을 만듭니다.

    2. i = 1, 2, 3, ..., n에 대해서, 다음 과정을 반복합니다.

        2.1. 1행 1열부터 i행 i열까지의 영역 내의 모든 빈 칸을 숫자 i로 채웁니다.

        2.2. 1행, 2행, ..., n행을 잘라내어 모두 이어붙인 새로운 1차원 배열을 만듭니다.

        2.3. 새로운 1차원 배열을 arr이라 할 때, arr[left], arr[left+1], ..., arr[right]만 남기고 나머지는 지웁니다.

- 정수 n, left, right가 매개변수로 주어집니다. 주어진 과정대로 만들어진 1차원 배열을 return 하도록 solution 함수를 완성해주세요.
```

```python
def solution(n, left, right):
    answer = []
    for i in range(left, right + 1):
        value = i // n
        remain = i % n
        answer.append(max(value, remain) + 1)
    return answer
```

```python
answer = solution(4, 7, 14)
len(answer)
print(answer)
```

### [롤케이크 자르기](https://school.programmers.co.kr/learn/courses/30/lessons/132265)

철수는 롤케이크를 두 조각으로 잘라서 동생과 한 조각씩 나눠 먹으려고 합니다. 이 롤케이크에는 여러가지 토핑들이 일렬로 올려져 있습니다. 철수와 동생은 롤케이크를 공평하게 나눠먹으려 하는데, 그들은 롤케이크의 크기보다 롤케이크 위에 올려진 토핑들의 종류에 더 관심이 많습니다. 그래서 잘린 조각들의 크기와 올려진 토핑의 개수에 상관없이 각 조각에 동일한 가짓수의 토핑이 올라가면 공평하게 롤케이크가 나누어진 것으로 생각합니다.

예를 들어, 롤케이크에 4가지 종류의 토핑이 올려져 있다고 합시다. 토핑들을 1, 2, 3, 4와 같이 번호로 표시했을 때, 케이크 위에 토핑들이 [1, 2, 1, 3, 1, 4, 1, 2] 순서로 올려져 있습니다. 만약 세 번째 토핑(1)과 네 번째 토핑(3) 사이를 자르면 롤케이크의 토핑은 [1, 2, 1], [3, 1, 4, 1, 2]로 나뉘게 됩니다. 철수가 [1, 2, 1]이 놓인 조각을, 동생이 [3, 1, 4, 1, 2]가 놓인 조각을 먹게 되면 철수는 두 가지 토핑(1, 2)을 맛볼 수 있지만, 동생은 네 가지 토핑(1, 2, 3, 4)을 맛볼 수 있으므로, 이는 공평하게 나누어진 것이 아닙니다. 만약 롤케이크의 네 번째 토핑(3)과 다섯 번째 토핑(1) 사이를 자르면 [1, 2, 1, 3], [1, 4, 1, 2]로 나뉘게 됩니다. 이 경우 철수는 세 가지 토핑(1, 2, 3)을, 동생도 세 가지 토핑(1, 2, 4)을 맛볼 수 있으므로, 이는 공평하게 나누어진 것입니다. 공평하게 롤케이크를 자르는 방법은 여러가지 일 수 있습니다. 위의 롤케이크를 [1, 2, 1, 3, 1], [4, 1, 2]으로 잘라도 공평하게 나뉩니다. 어떤 경우에는 롤케이크를 공평하게 나누지 못할 수도 있습니다.

롤케이크에 올려진 토핑들의 번호를 저장한 정수 배열 topping이 매개변수로 주어질 때, 롤케이크를 공평하게 자르는 방법의 수를 return 하도록 solution 함수를 완성해주세요.

```python
def solution(topping):
    answer = 0
    topping_size = len(topping)
    left = []
    right = []
    left_dict = {}
    right_dict = {}
    for i in range(topping_size - 1):
        left_dict[topping[i]] = True
        left.append(len(left_dict))
        right_dict[topping[topping_size - i - 1]] = True
        right.append(len(right_dict))

    for i in range(0, len(left)):
        if left[i] == right[len(right) - i - 1]:
            answer += 1

    return answer
```

```python
print(solution([1, 2, 1, 3, 1, 4, 1, 2]))
```

### [진수에서 소수 개수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/92335)

양의 정수 n이 주어집니다. 이 숫자를 k진수로 바꿨을 때, 변환된 수 안에 아래 조건에 맞는 소수(Prime number)가 몇 개인지 알아보려 합니다.

0P0처럼 소수 양쪽에 0이 있는 경우
P0처럼 소수 오른쪽에만 0이 있고 왼쪽에는 아무것도 없는 경우
0P처럼 소수 왼쪽에만 0이 있고 오른쪽에는 아무것도 없는 경우
P처럼 소수 양쪽에 아무것도 없는 경우
단, P는 각 자릿수에 0을 포함하지 않는 소수입니다.
예를 들어, 101은 P가 될 수 없습니다.
예를 들어, 437674을 3진수로 바꾸면 211020101011입니다. 여기서 찾을 수 있는 조건에 맞는 소수는 왼쪽부터 순서대로 211, 2, 11이 있으며, 총 3개입니다. (211, 2, 11을 k진법으로 보았을 때가 아닌, 10진법으로 보았을 때 소수여야 한다는 점에 주의합니다.) 211은 P0 형태에서 찾을 수 있으며, 2는 0P0에서, 11은 0P에서 찾을 수 있습니다.

정수 n과 k가 매개변수로 주어집니다. n을 k진수로 바꿨을 때, 변환된 수 안에서 찾을 수 있는 위 조건에 맞는 소수의 개수를 return 하도록 solution 함수를 완성해 주세요.

1 ≤ n ≤ 1,000,000

3 ≤ k ≤ 10

```python
def solution(n, k):
    answer = 0
    k_num = ""
    while n > 0:
        k_num = str(n % k) + k_num
        n = n // k
    k_num_list = k_num.split("0")
    for num in k_num_list:
        if num != "" and is_prime(int(num)):
            answer += 1

    return answer

def is_prime(num):
    if num < 2:
        return False
    for i in range(2, int(num**0.5) + 1):
        if num % i == 0:
            return False
    return True
```

```python
print(solution(10, 7))
```

### [택배상자](https://school.programmers.co.kr/learn/courses/30/lessons/131704)

영재는 택배상자를 트럭에 싣는 일을 합니다. 영재가 실어야 하는 택배상자는 크기가 모두 같으며 1번 상자부터 n번 상자까지 번호가 증가하는 순서대로 컨테이너 벨트에 일렬로 놓여 영재에게 전달됩니다. 컨테이너 벨트는 한 방향으로만 진행이 가능해서 벨트에 놓인 순서대로(1번 상자부터) 상자를 내릴 수 있습니다. 하지만 컨테이너 벨트에 놓인 순서대로 택배상자를 내려 바로 트럭에 싣게 되면 택배 기사님이 배달하는 순서와 택배상자가 실려 있는 순서가 맞지 않아 배달에 차질이 생깁니다. 따라서 택배 기사님이 미리 알려준 순서에 맞게 영재가 택배상자를 실어야 합니다.

만약 컨테이너 벨트의 맨 앞에 놓인 상자가 현재 트럭에 실어야 하는 순서가 아니라면 그 상자를 트럭에 실을 순서가 될 때까지 잠시 다른 곳에 보관해야 합니다. 하지만 고객의 물건을 함부로 땅에 둘 수 없어 보조 컨테이너 벨트를 추가로 설치하였습니다. 보조 컨테이너 벨트는 앞 뒤로 이동이 가능하지만 입구 외에 다른 면이 막혀 있어서 맨 앞의 상자만 뺄 수 있습니다(즉, 가장 마지막에 보조 컨테이너 벨트에 보관한 상자부터 꺼내게 됩니다). 보조 컨테이너 벨트를 이용해도 기사님이 원하는 순서대로 상자를 싣지 못 한다면, 더 이상 상자를 싣지 않습니다.

예를 들어, 영재가 5개의 상자를 실어야 하며, 택배 기사님이 알려준 순서가 기존의 컨테이너 벨트에 네 번째, 세 번째, 첫 번째, 두 번째, 다섯 번째 놓인 택배상자 순서인 경우, 영재는 우선 첫 번째, 두 번째, 세 번째 상자를 보조 컨테이너 벨트에 보관합니다. 그 후 네 번째 상자를 트럭에 싣고 보조 컨테이너 벨트에서 세 번째 상자 빼서 트럭에싣습니다. 다음으로 첫 번째 상자를 실어야 하지만 보조 컨테이너 벨트에서는 두 번째 상자를, 기존의 컨테이너 벨트에는 다섯 번째 상자를 꺼낼 수 있기 때문에 더이상의 상자는 실을 수 없습니다. 따라서 트럭에는 2개의 상자만 실리게 됩니다.

택배 기사님이 원하는 상자 순서를 나타내는 정수 배열 order가 주어졌을 때, 영재가 몇 개의 상자를 실을 수 있는지 return 하는 solution 함수를 완성하세요.

```python
def solution(order):
    answer = 0
    number_range = len(order) + 1
    stack = []
    pos = 0
    for i in range(1, number_range + 1):
        if pos >= len(order):
            break
        if order[pos] == i:
            answer += 1
            pos += 1
        else :
            while stack and stack[-1] == order[pos] and pos < len(order):
                stack.pop()
                answer += 1
                pos += 1
            if pos >= len(order):
                break
            if order[pos] != i:
                stack.append(i)
            else :
                answer += 1
                pos += 1
    while stack and stack[-1] == order[pos] and pos < len(order):
        stack.pop()
        answer += 1
        pos += 1
    return answer
```

```python
print(solution([5,4,3,2,1]))
```

### [주차 요금 계산](https://school.programmers.co.kr/learn/courses/30/lessons/92341)

주차장의 요금표와 차량이 들어오고(입차) 나간(출차) 기록이 주어졌을 때, 차량별로 주차 요금을 계산하려고 합니다. 아래는 하나의 예시를 나타냅니다.

### 요금표

|기본 시간(분)|기본 요금(원)|단위 시간(분)|단위 요금(원)|
|---|---|---|---|
|180|5000|10|600|
 

### 입/출차 기록

|시각(시:분)|차량 번호|내역|
|---|---|---|
|05:34|5961|입차|
|06:00|0000|입차|
|06:34|0000|출차|
|07:59|5961|출차|
|07:59|0148|입차|
|18:59|0000|입차|
|19:09|0148|출차|
|22:59|5961|입차|
|23:00|5961|출차|
 

### 자동차별 주차 요금

|차량 번호|누적 주차 시간(분)|주차 요금(원)|
|---|---|---|
|0000|34 + 300 = 334|5000 + ⌈(334 - 180)⌉ / 10⌉ x 600 = 14600|
|0148|670|5000 +⌈(670 - 180) / 10⌉x 600 = 34400|
|5961|145 + 1 = 146|5000|

- 어떤 차량이 입차된 후에 출차된 내역이 없다면, 23:59에 출차된 것으로 간주합니다.

    - 0000번 차량은 18:59에 입차된 이후, 출차된 내역이 없습니다. 따라서, 23:59에 출차된 것으로 간주합니다.

- 00:00부터 23:59까지의 입/출차 내역을 바탕으로 차량별 누적 주차 시간을 계산하여 요금을 일괄로 정산합니다.

- 누적 주차 시간이 기본 시간이하라면, 기본 요금을 청구합니다.

- 누적 주차 시간이 기본 시간을 초과하면, 기본 요금에 더해서, 초과한 시간에 대해서 단위 시간 마다 단위 요금을 청구합니다.

    - 초과한 시간이 단위 시간으로 나누어 떨어지지 않으면, 올림합니다.

    - `⌈a⌉` : a보다 작지 않은 최소의 정수를 의미합니다. 즉, 올림을 의미합니다.

> 주차 요금을 나타내는 정수 배열 fees, 자동차의 입/출차 내역을 나타내는 문자열 배열 records가 매개변수로 주어집니다.<br>
차량 번호가 작은 자동차부터 청구할 주차 요금을 차례대로 정수 배열에 담아서 return 하도록 solution 함수를 완성해주세요.

```python
def solution(fees, records):
    fees[0] # 기본 시간
    fees[1] # 기본 요금
    fees[2] # 단위 시간
    fees[3] # 단위 요금

    cars = {}
    for record in records:
        time, car, status = record.split(" ")
        h, m = map(int, time.split(":"))
        total_minute = h * 60 + m
        if car not in cars:
            cars[car] = []
        cars[car].append((total_minute, status))
    
    for car in cars:
        if len(cars[car]) % 2 == 1:
            cars[car].append((23 * 60 + 59, "OUT"))

        total_time = 0
        for i in range(0, len(cars[car]), 2):
            in_time, _ = cars[car][i]
            out_time, _ = cars[car][i + 1]
            total_time += out_time - in_time
        add_fee_time = max(0, total_time - fees[0])
        cars[car] = fees[1] + ((add_fee_time + fees[2] - 1) // fees[2]) * fees[3]

    answer = []
    car_list = sorted(cars.keys())
    for car in car_list:
        answer.append(cars[car])
    return answer
```

```python
print(solution([180, 5000, 10, 600], ["05:34 5961 IN", "06:00 0000 IN", "06:34 0000 OUT", "07:59 5961 OUT", "07:59 0148 IN", "18:59 0000 IN", "19:09 0148 OUT", "22:59 5961 IN", "23:00 5961 OUT"]))
```

### [두 큐 합 같게 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/118667)

길이가 같은 두 개의 큐가 주어집니다. 하나의 큐를 골라 원소를 추출(pop)하고, 추출된 원소를 다른 큐에 집어넣는(insert) 작업을 통해 각 큐의 원소 합이 같도록 만들려고 합니다. 이때 필요한 작업의 최소 횟수를 구하고자 합니다. 한 번의 pop과 한 번의 insert를 합쳐서 작업을 1회 수행한 것으로 간주합니다.

큐는 먼저 집어넣은 원소가 먼저 나오는 구조입니다. 이 문제에서는 큐를 배열로 표현하며, 원소가 배열 앞쪽에 있을수록 먼저 집어넣은 원소임을 의미합니다. 즉, pop을 하면 배열의 첫 번째 원소가 추출되며, insert를 하면 배열의 끝에 원소가 추가됩니다. 예를 들어 큐 [1, 2, 3, 4]가 주어졌을 때, pop을 하면 맨 앞에 있는 원소 1이 추출되어 [2, 3, 4]가 되며, 이어서 5를 insert하면 [2, 3, 4, 5]가 됩니다.

다음은 두 큐를 나타내는 예시입니다.

queue1 = [3, 2, 7, 2]

queue2 = [4, 6, 5, 1]


두 큐에 담긴 모든 원소의 합은 30입니다. 따라서, 각 큐의 합을 15로 만들어야 합니다. 예를 들어, 다음과 같이 2가지 방법이 있습니다.

1. queue2의 4, 6, 5를 순서대로 추출하여 queue1에 추가한 뒤, queue1의 3, 2, 7, 2를 순서대로 추출하여 queue2에 추가합니다. 그 결과 queue1은 [4, 6, 5], queue2는 [1, 3, 2, 7, 2]가 되며, 각 큐의 원소 합은 15로 같습니다. 이 방법은 작업을 7번 수행합니다.
2. queue1에서 3을 추출하여 queue2에 추가합니다. 그리고 queue2에서 4를 추출하여 queue1에 추가합니다. 그 결과 queue1은 [2, 7, 2, 4], queue2는 [6, 5, 1, 3]가 되며, 각 큐의 원소 합은 15로 같습니다. 이 방법은 작업을 2번만 수행하며, 이보다 적은 횟수로 목표를 달성할 수 없습니다.
따라서 각 큐의 원소 합을 같게 만들기 위해 필요한 작업의 최소 횟수는 2입니다.

길이가 같은 두 개의 큐를 나타내는 정수 배열 queue1, queue2가 매개변수로 주어집니다. 각 큐의 원소 합을 같게 만들기 위해 필요한 작업의 최소 횟수를 return 하도록 solution 함수를 완성해주세요. 단, 어떤 방법으로도 각 큐의 원소 합을 같게 만들 수 없는 경우, -1을 return 해주세요.

```python
from collections import deque

def solution(queue1, queue2):
    q1_sum = sum(queue1)
    q2_sum = sum(queue2)
    if q1_sum == q2_sum:
        return 0
    total = q1_sum + q2_sum
    if total % 2 != 0:
        return -1
    
    q1 = deque(queue1)
    q2 = deque(queue2)
    
    answer = 0
    while q1_sum != q2_sum:
        if q1_sum < q2_sum:
            value = q2.popleft()
            q1.append(value)
            q1_sum += value
            q2_sum -= value
            answer += 1
        else :
            value = q1.popleft()
            q2.append(value)
            q1_sum -= value
            q2_sum += value
            answer += 1
        if answer > (len(queue1) + len(queue2)) * 2:
            return -1

    return answer
```

```python
print(solution([2, 3, 2], [1, 1, 1]))
```

### [마법의 엘리베이터](https://school.programmers.co.kr/learn/courses/30/lessons/148653)

마법의 세계에 사는 민수는 아주 높은 탑에 살고 있습니다. 탑이 너무 높아서 걸어 다니기 힘든 민수는 마법의 엘리베이터를 만들었습니다. 마법의 엘리베이터의 버튼은 특별합니다. 마법의 엘리베이터에는 -1, +1, -10, +10, -100, +100 등과 같이 절댓값이 10^c (c ≥ 0 인 정수) 형태인 정수들이 적힌 버튼이 있습니다. 마법의 엘리베이터의 버튼을 누르면 현재 층 수에 버튼에 적혀 있는 값을 더한 층으로 이동하게 됩니다. 단, 엘리베이터가 위치해 있는 층과 버튼의 값을 더한 결과가 0보다 작으면 엘리베이터는 움직이지 않습니다. 민수의 세계에서는 0층이 가장 아래층이며 엘리베이터는 현재 민수가 있는 층에 있습니다.

마법의 엘리베이터를 움직이기 위해서 버튼 한 번당 마법의 돌 한 개를 사용하게 됩니다.예를 들어, 16층에 있는 민수가 0층으로 가려면 -1이 적힌 버튼을 6번, -10이 적힌 버튼을 1번 눌러 마법의 돌 7개를 소모하여 0층으로 갈 수 있습니다. 하지만, +1이 적힌 버튼을 4번, -10이 적힌 버튼 2번을 누르면 마법의 돌 6개를 소모하여 0층으로 갈 수 있습니다.

마법의 돌을 아끼기 위해 민수는 항상 최소한의 버튼을 눌러서 이동하려고 합니다. 민수가 어떤 층에서 엘리베이터를 타고 0층으로 내려가는데 필요한 마법의 돌의 최소 개수를 알고 싶습니다. 민수와 마법의 엘리베이터가 있는 층을 나타내는 정수 storey 가 주어졌을 때, 0층으로 가기 위해 필요한 마법의 돌의 최소값을 return 하도록 solution 함수를 완성하세요.

```python
def dfs(storey, mod=1, max=10**8):
    if storey > max:
        return 0
    if storey < mod:
        return 0
    digit = (storey // mod) % 10

    # up
    storey_up = storey + (10 - digit) * mod
    dfs_up = dfs(storey_up, mod * 10) + (10 - digit)
    # down
    storey_down = storey - digit * mod
    dfs_down = dfs(storey_down, mod * 10) + (digit)

    return min(dfs_up, dfs_down)

def solution(storey):
    answer = dfs(storey)
    return answer
```

```python
solution(2554)
```

### [서버 증설 횟수](https://school.programmers.co.kr/learn/courses/30/lessons/389479)

당신은 온라인 게임을 운영하고 있습니다. 같은 시간대에 게임을 이용하는 사람이 m명 늘어날 때마다 서버 1대가 추가로 필요합니다. 어느 시간대의 이용자가 m명 미만이라면, 서버 증설이 필요하지 않습니다. 어느 시간대의 이용자가 n x m명 이상 (n + 1) x m명 미만이라면 최소 n대의 증설된 서버가 운영 중이어야 합니다. 한 번 증설한 서버는 k시간 동안 운영하고 그 이후에는 반납합니다. 예를 들어, k = 5 일 때 10시에 증설한 서버는 10 ~ 15시에만 운영됩니다.

하루 동안 모든 게임 이용자가 게임을 하기 위해 서버를 최소 몇 번 증설해야 하는지 알고 싶습니다. 같은 시간대에 서버를 x대 증설했다면 해당 시간대의 증설 횟수는 x회입니다.

다음은 m = 3, k = 5 일 때의 시간대별 증설된 서버의 수와 증설 횟수 예시입니다.

|시각|게임 이용자의 수|증설된 서버의 수|증설 횟수|
|---|---|---|---|
|0 ~ 1|0|0|0|
|1 ~ 2|2|0|0|
|2 ~ 3|3|1|1|
|3 ~ 4|3|1|0|
|4 ~ 5|1|1|0|
|5 ~ 6|2|1|0|
|6 ~ 7|0|1|0|
|7 ~ 8|0|0|0|
|8 ~ 9|0|0|0|
|9 ~ 10|0|0|0|
|10 ~ 11|4|1|1|
|11 ~ 12|2|1|0|
|12 ~ 13|0|1|0|
|13 ~ 14|6|2|1|
|14 ~ 15|0|2|0|
|15 ~ 16|4|1|0|
|16 ~ 17|2|1|0|
|17 ~ 18|13|4|3|
|18 ~ 19|3|3|0|
|19 ~ 20|5|3|0|
|20 ~ 21|10|3|0|
|21 ~ 22|0|3|0|
|22 ~ 23|1|0|0|
|23 ~ 24|5|1|1|

모든 게임 이용자를 감당하기 위해 최소 7번 서버를 증설해야 하며, 이보다 적은 수의 서버 증설로는 모든 게임 이용자를 감당할 수 없습니다.

0시에서 23시까지의 시간대별 게임 이용자의 수를 나타내는 1차원 정수 배열 players, 서버 한 대로 감당할 수 있는 최대 이용자의 수를 나타내는 정수 m, 서버 한 대가 운영 가능한 시간을 나타내는 정수 k가 주어집니다. 이때, 모든 게임 이용자를 감당하기 위한 최소 서버 증설 횟수를 return 하도록 solution을 완성해 주세요.

```python
def solution(players, m, k):
    answer = 0
    auto_increment = [0 for _ in range(24)]
    for i in range(len(players)):
        if players[i] >= (auto_increment[i] + 1) * m:
            increase = (players[i] - ((auto_increment[i] + 1) * m) + m) // m
            answer += increase
            for j in range(k):
                if i + j < len(auto_increment):
                    auto_increment[i + j] += increase
                else :
                    break
    return answer
```

```python
solution([0, 2, 3, 3, 1, 2, 0, 0, 0, 0, 4, 2, 0, 6, 0, 4, 2, 13, 3, 5, 10, 0, 1, 5], 3, 5)
```

### [숫자 카드 나누기](https://school.programmers.co.kr/learn/courses/30/lessons/135807)

철수와 영희는 선생님으로부터 숫자가 하나씩 적힌 카드들을 절반씩 나눠서 가진 후, 다음 두 조건 중 하나를 만족하는 가장 큰 양의 정수 a의 값을 구하려고 합니다.

1. 철수가 가진 카드들에 적힌 모든 숫자를 나눌 수 있고, 영희가 가진 카드들에 적힌 모든 숫자들 중 하나도 나눌 수 없는 양의 정수 a
2. 영희가 가진 카드들에 적힌 모든 숫자를 나눌 수 있고, 철수가 가진 카드들에 적힌 모든 숫자들 중 하나도 나눌 수 없는 양의 정수 a


예를 들어, 카드들에 10, 5, 20, 17이 적혀 있는 경우에 대해 생각해 봅시다. 만약, 철수가 [10, 17]이 적힌 카드를 갖고, 영희가 [5, 20]이 적힌 카드를 갖는다면 두 조건 중 하나를 만족하는 양의 정수 a는 존재하지 않습니다. 하지만, 철수가 [10, 20]이 적힌 카드를 갖고, 영희가 [5, 17]이 적힌 카드를 갖는다면, 철수가 가진 카드들의 숫자는 모두 10으로 나눌 수 있고, 영희가 가진 카드들의 숫자는 모두 10으로 나눌 수 없습니다. 따라서 철수와 영희는 각각 [10, 20]이 적힌 카드, [5, 17]이 적힌 카드로 나눠 가졌다면 조건에 해당하는 양의 정수 a는 10이 됩니다.


철수가 가진 카드에 적힌 숫자들을 나타내는 정수 배열 arrayA와 영희가 가진 카드에 적힌 숫자들을 나타내는 정수 배열 arrayB가 주어졌을 때, 주어진 조건을 만족하는 가장 큰 양의 정수 a를 return하도록 solution 함수를 완성해 주세요. 만약, 조건을 만족하는 a가 없다면, 0을 return 해 주세요.

```python
# 최대 공약수 구하기
def gcd(array):
    from math import gcd
    from functools import reduce
    return reduce(gcd, array)

# 숫자의 약수 구하기
def get_divisors(n):
    divisors = set()
    for i in range(1, int(n**0.5) + 1):
        if n % i == 0:
            divisors.add(i)
            divisors.add(n // i)
    return divisors

def check_divisor(div_array, undiv_array):
    ret = 0
    # 배열의 최대공약수의 약수들은 조건을 만족한다.
    gcd_value = gcd(div_array)
    if gcd_value < 2:
        return 0
    divisors = get_divisors(gcd_value)
    
    # 약수들 중에서 undiv_array의 어떤 수로도 나누어 떨어지지 않는 가장 큰 수를 찾는다.
    for num in divisors:
        is_dividable = False
        for undiv in undiv_array:
            if undiv % num == 0:
                is_dividable = True
                break
        if not is_dividable:
            ret = max(ret, num)
    return ret

def solution(arrayA, arrayB):
    answer = max(check_divisor(arrayA, arrayB), check_divisor(arrayB, arrayA))
    return answer
```

```python
solution([14, 35, 119], [18, 30, 102])
```

### [메뉴 리뉴얼](https://school.programmers.co.kr/learn/courses/30/lessons/72411)

레스토랑을 운영하던 스카피는 코로나19로 인한 불경기를 극복하고자 메뉴를 새로 구성하려고 고민하고 있습니다.
기존에는 단품으로만 제공하던 메뉴를 조합해서 코스요리 형태로 재구성해서 새로운 메뉴를 제공하기로 결정했습니다. 어떤 단품메뉴들을 조합해서 코스요리 메뉴로 구성하면 좋을 지 고민하던 "스카피"는 이전에 각 손님들이 주문할 때 가장 많이 함께 주문한 단품메뉴들을 코스요리 메뉴로 구성하기로 했습니다.
단, 코스요리 메뉴는 최소 2가지 이상의 단품메뉴로 구성하려고 합니다. 또한, 최소 2명 이상의 손님으로부터 주문된 단품메뉴 조합에 대해서만 코스요리 메뉴 후보에 포함하기로 했습니다.

예를 들어, 손님 6명이 주문한 단품메뉴들의 조합이 다음과 같다면,
(각 손님은 단품메뉴를 2개 이상 주문해야 하며, 각 단품메뉴는 A ~ Z의 알파벳 대문자로 표기합니다.)

|손님|번호|주문한 단품메뉴 조합|
|---|---|---|
|1번|손님|A, B, C, F, G|
|2번|손님|A, C|
|3번|손님|C, D, E|
|4번|손님|A, C, D, E|
|5번|손님|B, C, F, G|
|6번|손님|A, C, D, E, H|

가장 많이 함께 주문된 단품메뉴 조합에 따라 "스카피"가 만들게 될 코스요리 메뉴 구성 후보는 다음과 같습니다.

|코스 종류|메뉴 구성|설명|
|---|---|---|
|요리 2개 코스|A, C|1번, 2번, 4번, 6번 손님으로부터 총 4번 주문됐습니다.|
|요리 3개 코스|C, D, E|3번, 4번, 6번 손님으로부터 총 3번 주문됐습니다.|
|요리 4개 코스|B, C, F, G|1번, 5번 손님으로부터 총 2번 주문됐습니다.|
|요리 4개 코스|A, C, D, E|4번, 6번 손님으로부터 총 2번 주문됐습니다.|

각 손님들이 주문한 단품메뉴들이 문자열 형식으로 담긴 배열 orders, "스카피"가 추가하고 싶어하는 코스요리를 구성하는 단품메뉴들의 갯수가 담긴 배열 course가 매개변수로 주어질 때, "스카피"가 새로 추가하게 될 코스요리의 메뉴 구성을 문자열 형태로 배열에 담아 return 하도록 solution 함수를 완성해 주세요.

```python
from itertools import combinations

ALPHABET = [chr(i) for i in range(ord('A'), ord('Z') + 1)]

def solution(orders, course):
    answer = []
    course_list = {}
    for course_size in course:
        course_list[course_size] = {}

    for order in orders:
        order = ''.join(sorted(order))
        for course_size in course:
            if len(order) < course_size:
                continue
            comb = combinations(order, course_size)
            for c in comb:
                if c in course_list[course_size]:
                    course_list[course_size][c] += 1
                else :
                    course_list[course_size][c] = 1
    
    for course_size in course:
        max_count = 0
        for c in course_list[course_size]:
            max_count = max(max_count, course_list[course_size][c])
        if max_count < 2:
            continue
        for c in course_list[course_size]:
            if course_list[course_size][c] == max_count:
                answer.append("".join(c))

    answer.sort()

    return answer
```

```python
print(solution(["ABCFG", "AC", "CDE", "ACDE", "BCFG", "ACDEH"], [2,3,4]))
# ["AC", "ACDE", "BCFG", "CDE"]
print(solution(["ABCDE", "AB", "CD", "ADE", "XYZ", "XYZ", "ACD"], [2,3,5]))
# ["ACD", "AD", "ADE", "CD", "XYZ"]
print(solution(["XYZ", "XWY", "WXA"], [2,3,4]))
# ["WX", "XY"]
```

### [우박수열 정적분](https://school.programmers.co.kr/learn/courses/30/lessons/134239)

콜라츠 추측이란 로타르 콜라츠(Lothar Collatz)가 1937년에 제기한 추측으로 모든 자연수 k에 대해 다음 작업을 반복하면 항상 1로 만들 수 있다는 추측입니다.

```
    1.1. 입력된 수가 짝수라면 2로 나눕니다.
    1.2. 입력된 수가 홀수라면 3을 곱하고 1을 더합니다.
    2. 결과로 나온 수가 1보다 크다면 1번 작업을 반복합니다.
```

예를 들어 주어진 수가 5 라면 5 ⇒ 16 ⇒ 8 ⇒ 4 ⇒2 ⇒ 1 이되어 총 5번만에 1이 됩니다.

수가 커졌다 작아지기를 반복하는 모습이 비구름에서 빗방울이 오르락내리락하며 우박이 되는 모습과 비슷하다고 하여 우박수 또는 우박수열로 불리기도 합니다. 현재 이 추측이 참인지 거짓인지 증명되지 않았지만 약 1해까지의 수에서 반례가 없음이 밝혀져 있습니다.

은지는 우박수열을 좌표 평면 위에 꺾은선 그래프로 나타내보려고 합니다. 초항이 k인 우박수열이 있다면, x = 0일때 y = k이고 다음 우박수는 x = 1에 표시합니다. 이런 식으로 우박수가 1이 될 때까지 점들을 찍고 인접한 점들끼리 직선으로 연결하면 다음과 같이 꺾은선 그래프를 만들 수 있습니다.

은지는 이렇게 만든 꺾은선 그래프를 정적분 해보고 싶어졌습니다. x에 대한 어떤 범위 [a, b]가 주어진다면 이 범위에 대한 정적분 결과는 꺾은선 그래프와 x = a, x = b, y = 0 으로 둘러 쌓인 공간의 면적과 같습니다. 은지는 이것을 우박수열 정적분이라고 정의하였고 다양한 구간에 대해서 우박수열 정적분을 해보려고 합니다. 0 이상의 수 b에 대해 [a, -b]에 대한 정적분 결과는 x = a, x = n - b, y = 0 으로 둘러 쌓인 공간의 면적으로 정의하며, 이때 n은 k가 초항인 우박수열이 1이 될때 까지의 횟수를 의미합니다.

예를 들어, 5를 초항으로 하는 우박수열은 5 ⇒ 16 ⇒ 8 ⇒ 4 ⇒ 2 ⇒ 1 입니다. 이를 좌표 평면으로 옮기면 (0, 5), (1, 16), (2, 8), (3, 4), (4, 2), (5, 1) 에 점이 찍히고 점들을 연결하면 꺾은선 그래프가 나옵니다. 이를 [0,0] 구간에 대해 정적분 한다면 전체 구간에 대한 정적분이며, [1,-2] 구간에 대해 정적분 한다면 1 ≤ x ≤ 3인 구간에 대한 정적분입니다.

우박수의 초항 k와, 정적분을 구하는 구간들의 목록 ranges가 주어졌을 때 정적분의 결과 목록을 return 하도록 solution을 완성해주세요. 단, 주어진 구간의 시작점이 끝점보다 커서 유효하지 않은 구간이 주어질 수 있으며 이때의 정적분 결과는 -1로 정의합니다.

```python
def solution(k, ranges):
    answer = []

    graph = []
    num = k
    while num > 1:
        graph.append(num)
        if num % 2 == 0:
            num = num // 2
        else :
            num = num * 3 + 1
    graph.append(1)

    n = len(graph) - 1
    for r in ranges:
        start, end = r
        end = n + end
        if start > end:
            answer.append(-1.0)
            continue
        if start == end:
            answer.append(0.0)
            continue
        area = 0.0
        for i in range(start, end):
            rectangle = min(graph[i], graph[i + 1])
            triangle = (max(graph[i], graph[i + 1]) - rectangle) / 2
            area += rectangle + triangle
        answer.append(area)

    return answer
```

```python
solution(5, [[0,0],[0,-1],[2,-3],[3,-3]])
# [33.0,31.5,0.0,-1.0]
```

### [지게차와 크레인](https://school.programmers.co.kr/learn/courses/30/lessons/388353)

A 회사의 물류창고에는 알파벳 대문자로 종류를 구분하는 컨테이너가 세로로 n 줄, 가로로 m줄 총 n x m개 놓여 있습니다. 특정 종류 컨테이너의 출고 요청이 들어올 때마다 지게차로 창고에서 접근이 가능한 해당 종류의 컨테이너를 모두 꺼냅니다. 접근이 가능한 컨테이너란 4면 중 적어도 1면이 창고 외부와 연결된 컨테이너를 말합니다.

최근 이 물류 창고에서 창고 외부와 연결되지 않은 컨테이너도 꺼낼 수 있도록 크레인을 도입했습니다. 크레인을 사용하면 요청된 종류의 모든 컨테이너를 꺼냅니다.

![alt text](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/0e90cfc3-ddd7-4841-9ed8-c420cb6be7a5/%E1%84%86%E1%85%AE%E1%86%AF%E1%84%85%E1%85%B2%E1%84%8E%E1%85%A1%E1%86%BC%E1%84%80%E1%85%A9-1-1.drawio.png)

위 그림처럼 세로로 4줄, 가로로 5줄이 놓인 창고를 예로 들어보겠습니다. 이때 "A", "BB", "A" 순서대로 해당 종류의 컨테이너 출고 요청이 들어왔다고 가정하겠습니다. “A”처럼 알파벳 하나로만 출고 요청이 들어올 경우 지게차를 사용해 출고 요청이 들어온 순간 접근 가능한 컨테이너를 꺼냅니다. "BB"처럼 같은 알파벳이 두 번 반복된 경우는 크레인을 사용해 요청된 종류의 모든 컨테이너를 꺼냅니다.

![alt text](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/e5fac969-705a-41cf-8609-ad41e30ea694/%E1%84%86%E1%85%AE%E1%86%AF%E1%84%85%E1%85%B2%E1%84%8E%E1%85%A1%E1%86%BC%E1%84%80%E1%85%A9-1-2.drawio.png)

위 그림처럼 컨테이너가 꺼내져 3번의 출고 요청 이후 남은 컨테이너는 11개입니다. 두 번째 요청은 크레인을 활용해 모든 B 컨테이너를 꺼냈음을 유의해 주세요. 세 번째 요청이 들어왔을 때 2행 2열의 A 컨테이너만 접근이 가능하고 2행 3열의 A 컨테이너는 접근이 불가능했음을 유의해 주세요.

처음 물류창고에 놓인 컨테이너의 정보를 담은 1차원 문자열 배열 storage와 출고할 컨테이너의 종류와 출고방법을 요청 순서대로 담은 1차원 문자열 배열 requests가 매개변수로 주어집니다. 이때 모든 요청을 순서대로 완료한 후 남은 컨테이너의 수를 return 하도록 solution 함수를 완성해 주세요.

```python
from collections import deque

# bfs 경로 탐색
def go_outside(storage, pos_x, pos_y, n, m):
    queue = deque()
    queue.append((pos_x, pos_y))
    # 반복적으로 초기화되어 경량화 필요
    visited = [[False] * m for _ in range(n)]
    while queue:
        (x, y) = queue.popleft()
        if x == 0 or x == n - 1 or y == 0 or y == m - 1:
            return True
        if storage[x][y] == 'o':
            return True
    
        # up down left right : 방향 튜플 사용
        for dx, dy in ((-1,0),(1,0),(0,-1),(0,1)):
            nx, ny = x + dx, y + dy
            if not visited[nx][ny] and storage[nx][ny] in ('n','o'):
                visited[nx][ny] = True
                queue.append((nx, ny))

    return False

def solution(storage, requests):
    storage = [list(s) for s in storage]
    answer = 0
    # len 호출 간소화
    n, m = len(storage), len(storage[0])
    for request in requests:
        r_stuff = request[0]
        r_type = "one" if len(request) == 1 else "mul"
        remove_pos = []
        for i in range(n):
            for j in range(m):
                if storage[i][j] == r_stuff:
                    if r_type == "mul":
                        storage[i][j] = 'n'
                    elif go_outside(storage, i, j, n, m):
                        remove_pos.append((i, j))
        if len(remove_pos) > 0:
            for pos_x, pos_y in remove_pos:
                storage[pos_x][pos_y] = 'o'
    
    for i in range(n):
        for j in range(m):
            if storage[i][j] not in ('n', 'o'):
                answer += 1
    return answer
```

```python
print(solution(["AZWQY", "CAABX", "BBDDA", "ACACA"], ["A", "BB", "A"]))
# 11
print(solution(["TTTTT", "TKKKT", "TTKTT", "TTATT"], ["A", "K", "K", "K"]))
# 15
print(solution(["CCCCC", "CABAC", "CABAC", "CCABC"], ["BB", "A"]))
# 15
```

### [숫자 블록](https://school.programmers.co.kr/learn/courses/30/lessons/12923)

그렙시에는 숫자 0이 적힌 블록들이 설치된 도로에 다른 숫자가 적힌 블록들을 설치하기로 하였습니다. 숫자 블록을 설치하는 규칙은 다음과 같습니다.

블록에 적힌 번호가 n 일 때, 가장 첫 블록은 n * 2번째 위치에 설치합니다. 그 다음은 n * 3, 그 다음은 n * 4, ...위치에 설치합니다. 기존에 설치된 블록은 빼고 새로운 블록을 집어넣습니다.

블록은 1이 적힌 블록부터 숫자를 1씩 증가시키며 순서대로 설치합니다. 예를 들어 1이 적힌 블록은 2, 3, 4, 5, ... 인 위치에 우선 설치합니다. 그 다음 2가 적힌 블록은 4, 6, 8, 10, ... 인 위치에 설치하고, 3이 적힌 블록은 6, 9, 12... 인 위치에 설치합니다.

이렇게 3이 적힌 블록까지 설치하고 나면 첫 10개의 블록에 적힌 번호는 [0, 1, 1, 2, 1, 3, 1, 2, 3, 2]가 됩니다.

그렙시는 길이가 1,000,000,000인 도로에 1부터 10,000,000까지의 숫자가 적힌 블록들을 이용해 위의 규칙대로 모두 설치 했습니다.

그렙시의 시장님은 특정 구간에 어떤 블록이 깔려 있는지 알고 싶습니다.

구간을 나타내는 두 정수 begin, end 가 매개변수로 주어 질 때, 그 구간에 깔려 있는 블록의 숫자 배열을 return하는 solution 함수를 완성해 주세요.

```python
def get_divisors(n):
    divisors = set()
    for i in range(1, int(n**0.5) + 1):
        if n % i == 0:
            divisors.add(i)
            if i > 1 and n // i > 10000000:
                continue
            divisors.add(n // i)
    return divisors

def solution(begin, end):
    answer = []
    for num in range(begin, end + 1):
        if num == 0: 
            answer.append(0)
            continue
        if num == 1:
            answer.append(0)
            continue
        divisors = get_divisors(num)
        if num in divisors:
            divisors.remove(num)
        answer.append(max(divisors))
    return answer
```

```python
solution(1, 10)
# [0, 1, 1, 2, 1, 3, 1, 4, 3, 5]
```

### [혼자 놀기의 달인](https://school.programmers.co.kr/learn/courses/30/lessons/131130)

혼자서도 잘 노는 범희는 어느 날 방구석에 있는 숫자 카드 더미를 보더니 혼자 할 수 있는 재미있는 게임을 생각해냈습니다.

숫자 카드 더미에는 카드가 총 100장 있으며, 각 카드에는 1부터 100까지 숫자가 하나씩 적혀있습니다. 2 이상 100 이하의 자연수를 하나 정해 그 수보다 작거나 같은 숫자 카드들을 준비하고, 준비한 카드의 수만큼 작은 상자를 준비하면 게임을 시작할 수 있으며 게임 방법은 다음과 같습니다.

준비된 상자에 카드를 한 장씩 넣고, 상자를 무작위로 섞어 일렬로 나열합니다. 상자가 일렬로 나열되면 상자가 나열된 순서에 따라 1번부터 순차적으로 증가하는 번호를 붙입니다.

그 다음 임의의 상자를 하나 선택하여 선택한 상자 안의 숫자 카드를 확인합니다. 다음으로 확인한 카드에 적힌 번호에 해당하는 상자를 열어 안에 담긴 카드에 적힌 숫자를 확인합니다. 마찬가지로 숫자에 해당하는 번호를 가진 상자를 계속해서 열어가며, 열어야 하는 상자가 이미 열려있을 때까지 반복합니다.

이렇게 연 상자들은 1번 상자 그룹입니다. 이제 1번 상자 그룹을 다른 상자들과 섞이지 않도록 따로 둡니다. 만약 1번 상자 그룹을 제외하고 남는 상자가 없으면 그대로 게임이 종료되며, 이때 획득하는 점수는 0점입니다.

그렇지 않다면 남은 상자 중 다시 임의의 상자 하나를 골라 같은 방식으로 이미 열려있는 상자를 만날 때까지 상자를 엽니다. 이렇게 연 상자들은 2번 상자 그룹입니다.

1번 상자 그룹에 속한 상자의 수와 2번 상자 그룹에 속한 상자의 수를 곱한 값이 게임의 점수입니다.

상자 안에 들어있는 카드 번호가 순서대로 담긴 배열 cards가 매개변수로 주어질 때, 범희가 이 게임에서 얻을 수 있는 최고 점수를 구해서 return 하도록 solution 함수를 완성해주세요.


#### 제한사항

- 2 ≤ cards의 길이 ≤ 100

- cards의 원소는 cards의 길이 이하인 임의의 자연수입니다.

- cards에는 중복되는 원소가 존재하지 않습니다.

- cards[i]는 i + 1번 상자에 담긴 카드에 적힌 숫자를 의미합니다.

```python
def make_group(cards, is_opened, pos):
    ret = []
    if is_opened[pos - 1]:
        return ret
    
    while not cards[pos - 1] in ret:
        is_opened[pos - 1] = True
        ret.append(cards[pos - 1])
        pos = cards[pos - 1]
    
    return ret

def solution(cards):
    answer = 0
    groups = []
    is_opened = [False] * len(cards)
    for card in cards:
        if is_opened[card - 1]:
            continue
        group = make_group(cards, is_opened, card)
        if len(group) > 0:
            groups.append(len(group))

    if len(groups) < 2:
        return 0
    
    groups.sort(reverse=True)
    answer = groups[0] * groups[1]
        
    return answer
```

```python
solution([8,6,3,7,2,5,1,4])
```

### [양궁대회](https://school.programmers.co.kr/learn/courses/30/lessons/92342)

카카오배 양궁대회가 열렸습니다.

라이언은 저번 카카오배 양궁대회 우승자이고 이번 대회에도 결승전까지 올라왔습니다. 결승전 상대는 어피치입니다.

카카오배 양궁대회 운영위원회는 한 선수의 연속 우승보다는 다양한 선수들이 양궁대회에서 우승하기를 원합니다. 따라서, 양궁대회 운영위원회는 결승전 규칙을 전 대회 우승자인 라이언에게 불리하게 다음과 같이 정했습니다.

1. 어피치가 화살 n발을 다 쏜 후에 라이언이 화살 n발을 쏩니다.

2. 점수를 계산합니다.

    1. 과녁판은 아래 사진처럼 생겼으며 가장 작은 원의 과녁 점수는 10점이고 가장 큰 원의 바깥쪽은 과녁 점수가 0점입니다.
    
        ![alt text](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/2c73b8f8-c938-4b6e-9bc3-e3a3784d6a41/01_2022_%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A2%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6_%E1%84%8B%E1%85%A3%E1%86%BC%E1%84%80%E1%85%AE%E1%86%BC%E1%84%83%E1%85%A2%E1%84%92%E1%85%AC_01.png)

    2. 만약, k(k는 1~10사이의 자연수)점을 어피치가 a발을 맞혔고 라이언이 b발을 맞혔을 경우 더 많은 화살을 k점에 맞힌 선수가 k 점을 가져갑니다. 단, a = b일 경우는 어피치가 k점을 가져갑니다. k점을 여러 발 맞혀도 k점 보다 많은 점수를 가져가는 게 아니고 k점만 가져가는 것을 유의하세요. 또한 a = b = 0 인 경우, 즉, 라이언과 어피치 모두 k점에 단 하나의 화살도 맞히지 못한 경우는 어느 누구도 k점을 가져가지 않습니다.

        - 예를 들어, 어피치가 10점을 2발 맞혔고 라이언도 10점을 2발 맞혔을 경우 어피치가 10점을 가져갑니다.

        - 다른 예로, 어피치가 10점을 0발 맞혔고 라이언이 10점을 2발 맞혔을 경우 라이언이 10점을 가져갑니다.

    3. 모든 과녁 점수에 대하여 각 선수의 최종 점수를 계산합니다.
3. 최종 점수가 더 높은 선수를 우승자로 결정합니다. 단, 최종 점수가 같을 경우 어피치를 우승자로 결정합니다.

현재 상황은 어피치가 화살 n발을 다 쏜 후이고 라이언이 화살을 쏠 차례입니다.

라이언은 어피치를 가장 큰 점수 차이로 이기기 위해서 n발의 화살을 어떤 과녁 점수에 맞혀야 하는지를 구하려고 합니다.

화살의 개수를 담은 자연수 n, 어피치가 맞힌 과녁 점수의 개수를 10점부터 0점까지 순서대로 담은 정수 배열 info가 매개변수로 주어집니다. 이때, 라이언이 가장 큰 점수 차이로 우승하기 위해 n발의 화살을 어떤 과녁 점수에 맞혀야 하는지를 10점부터 0점까지 순서대로 정수 배열에 담아 return 하도록 solution 함수를 완성해 주세요. 만약, 라이언이 우승할 수 없는 경우(무조건 지거나 비기는 경우)는 [-1]을 return 해주세요.


#### 제한사항
1 ≤ n ≤ 10

info의 길이 = 11

0 ≤ info의 원소 ≤ n

info의 원소 총합 = n

info의 i번째 원소는 과녁의 10 - i 점을 맞힌 화살 개수입니다. ( i는 0~10 사이의 정수입니다.)

라이언이 우승할 방법이 있는 경우, return 할 정수 배열의 길이는 11입니다.

0 ≤ return할 정수 배열의 원소 ≤ n

return할 정수 배열의 원소 총합 = n (꼭 n발을 다 쏴야 합니다.)

return할 정수 배열의 i번째 원소는 과녁의 10 - i 점을 맞힌 화살 개수입니다. ( i는 0~10 사이의 정수입니다.)

라이언이 가장 큰 점수 차이로 우승할 수 있는 방법이 여러 가지 일 경우, 가장 낮은 점수를 더 많이 맞힌 경우를 return 해주세요.

가장 낮은 점수를 맞힌 개수가 같을 경우 계속해서 그다음으로 낮은 점수를 더 많이 맞힌 경우를 return 해주세요.

예를 들어, [2,3,1,0,0,0,0,1,3,0,0]과 [2,1,0,2,0,0,0,2,3,0,0]를 비교하면 [2,1,0,2,0,0,0,2,3,0,0]를 return 해야 합니다.

다른 예로, [0,0,2,3,4,1,0,0,0,0,0]과 [9,0,0,0,0,0,0,0,1,0,0]를 비교하면[9,0,0,0,0,0,0,0,1,0,0]를 return 해야 합니다.

라이언이 우승할 방법이 없는 경우, return 할 정수 배열의 길이는 1입니다.

라이언이 어떻게 화살을 쏘든 라이언의 점수가 어피치의 점수보다 낮거나 같으면 [-1]을 return 해야 합니다.

```python
from itertools import combinations_with_replacement

def get_score(apeach, ryan):
    ryan_score = 0
    apeach_score = 0
    for i in range(11):
        if apeach[i] < ryan[i]:
            ryan_score += (10 - i)
        elif apeach[i] >= ryan[i] and apeach[i] > 0:
            apeach_score += (10 - i)
    return ryan_score, apeach_score

def solution(n, info):
    answer = []
    answer_list = []
    max_diff = -1
    # 중복 조합 
    comb = combinations_with_replacement(range(11), n)
    for c in comb:
        ryan = [0] * 11
        for num in c:
            ryan[10 - num] += 1
        ryan_score, apeach_score = get_score(info, ryan)
        if ryan_score <= apeach_score:
            continue
        if max_diff == ryan_score - apeach_score:
            answer_list.append(ryan)
        elif max_diff < ryan_score - apeach_score:
            max_diff = ryan_score - apeach_score
            answer_list = [ryan]

    if max_diff == -1:
        return [-1]

    answer_list.sort(key=lambda x: x[::-1], reverse=True)
    answer = answer_list[0]

    return answer
```

```python
print("res:", solution(5, [2,1,1,1,0,0,0,0,0,0,0]))
print("exp:", [0,2,2,0,1,0,0,0,0,0,0])
```

### [택배 배달과 수거하기](https://school.programmers.co.kr/learn/courses/30/lessons/150369)

![재할용 택배 상자.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/7ce63a07-3abd-40a1-87cc-c1f664393aa0/%E1%84%8C%E1%85%A2%E1%84%92%E1%85%A1%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC%20%E1%84%90%E1%85%A2%E1%86%A8%E1%84%87%E1%85%A2%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%A1.png)

당신은 일렬로 나열된 n개의 집에 택배를 배달하려 합니다. 배달할 물건은 모두 크기가 같은 재활용 택배 상자에 담아 배달하며, 배달을 다니면서 빈 재활용 택배 상자들을 수거하려 합니다.
배달할 택배들은 모두 재활용 택배 상자에 담겨서 물류창고에 보관되어 있고, i번째 집은 물류창고에서 거리 i만큼 떨어져 있습니다. 또한 i번째 집은 j번째 집과 거리 j - i만큼 떨어져 있습니다. (1 ≤ i ≤ j ≤ n)
트럭에는 재활용 택배 상자를 최대 cap개 실을 수 있습니다. 트럭은 배달할 재활용 택배 상자들을 실어 물류창고에서 출발해 각 집에 배달하면서, 빈 재활용 택배 상자들을 수거해 물류창고에 내립니다. 각 집마다 배달할 재활용 택배 상자의 개수와 수거할 빈 재활용 택배 상자의 개수를 알고 있을 때, 트럭 하나로 모든 배달과 수거를 마치고 물류창고까지 돌아올 수 있는 최소 이동 거리를 구하려 합니다. 각 집에 배달 및 수거할 때, 원하는 개수만큼 택배를 배달 및 수거할 수 있습니다.

다음은 cap=4 일 때, 최소 거리로 이동하면서 5개의 집에 배달 및 수거하는 과정을 나타낸 예시입니다.

배달 및 수거할 재활용 택배 상자 개수

||집 #1|집 #2|집 #3|집 #4|집 #5|
|---|---|---|---|---|---|
|배달|1개|0개|3개|1개|2개|
|수거|0개|3개|0개|4개|0개|


|배달| 및 수거 과정

||집 #1|집 #2|집 #3|집 #4|집 #5|설명|
|---|---|---|---|---|---|---|
|남은 배달/수거|1/0|0/3|3/0|1/4|2/0|물류창고에서 택배 3개를 트럭에 실어 출발합니다.|
|남은 배달/수거|1/0|0/3|3/0|0/4|0/0|물류창고에서 5번째 집까지 이동하면서(거리 5) 4번째 집에 택배 1개를 배달하고, 5번째 집에 택배 2개를 배달합니다.|
|남은 배달/수거|1/0|0/3|3/0|0/0|0/0|5번째 집에서 물류창고까지 이동하면서(거리 5) 4번째 집에서 빈 택배 상자 4개를 수거한 후, 수거한 빈 택배 상자를 물류창고에 내리고 택배 4개를 트럭에 싣습니다.|
|남은 배달/수거|0/0|0/3|0/0|0/0|0/0|물류창고에서 3번째 집까지 이동하면서(거리 3) 1번째 집에 택배 1개를 배달하고, 3번째 집에 택배 3개를 배달합니다.|
|남은 배달/수거|0/0|0/0|0/0|0/0|0/0|3번째 집에서 물류창고까지 이동하면서(거리 3) 2번째 집에서 빈 택배 상자 3개를 수거한 후, 수거한 빈 택배 상자를 물류창고에 내립니다.|

16(=5+5+3+3)의 거리를 이동하면서 모든 배달 및 수거를 마쳤습니다. 같은 거리로 모든 배달 및 수거를 마치는 다른 방법이 있지만, 이보다 짧은 거리로 모든 배달 및 수거를 마치는 방법은 없습니다.

트럭에 실을 수 있는 재활용 택배 상자의 최대 개수를 나타내는 정수 cap, 배달할 집의 개수를 나타내는 정수 n, 각 집에 배달할 재활용 택배 상자의 개수를 담은 1차원 정수 배열 deliveries와 각 집에서 수거할 빈 재활용 택배 상자의 개수를 담은 1차원 정수 배열 pickups가 매개변수로 주어집니다. 이때, 트럭 하나로 모든 배달과 수거를 마치고 물류창고까지 돌아올 수 있는 최소 이동 거리를 return 하도록 solution 함수를 완성해 주세요.


##### 제한사항

1 ≤ cap ≤ 50

1 ≤ n ≤ 100,000

deliveries의 길이 = pickups의 길이 = n

deliveries[i]는 i+1번째 집에 배달할 재활용 택배 상자의 개수를 나타냅니다.

pickups[i]는 i+1번째 집에서 수거할 빈 재활용 택배 상자의 개수를 나타냅니다.

0 ≤ deliveries의 원소 ≤ 50

0 ≤ pickups의 원소 ≤ 50

트럭의 초기 위치는 물류창고입니다.

```python
def solution(cap, n, deliveries, pickups):
    answer = 0
    truck = 0

    check_deliveries = [0] * n
    check_pickups = [0] * n
    for i in range(n - 1, -1, -1):
        if i == n - 1:
            check_deliveries[i] = deliveries[i]
            check_pickups[i] = pickups[i]
        else :
            check_deliveries[i] = check_deliveries[i + 1] + deliveries[i]
            check_pickups[i] = check_pickups[i + 1] + pickups[i]
    
    # 배송과 수거가 모두 끝났을 때 종료한다.
    delivery_flag = True
    pickup_flag = True
    while delivery_flag or pickup_flag:
        # 트럭에는 남은 배송 물건을 cap 만큼 싣고 센터에서 출발한다.
        truck = check_deliveries[0] % cap if check_deliveries[0] % cap != 0 else cap if check_deliveries[0] > 0 else 0
        # 지역을 탐색하여 배송과 수거를 진행한다.
        d_cycle_flag = True
        p_cycle_flag = True
        d_side_move = 0
        p_side_move = 0
        for i in range(n):
            if not d_cycle_flag and not p_cycle_flag:
                break
            if d_cycle_flag:
                d_side_move += 1
            if p_cycle_flag:
                p_side_move += 1
            # 배송 관련 확인
            if d_cycle_flag:
                if check_deliveries[i] > 0:
                    if truck >= check_deliveries[i]:
                        check_deliveries[i] = 0
                    else :
                        check_deliveries[i] -= truck
                else :
                    d_side_move -= 1
                    d_cycle_flag = False

            # 수거 관련 확인
            if p_cycle_flag:
                if check_pickups[i] > 0:
                    if check_pickups[i] <= cap:
                        check_pickups[i] = 0
                    else :
                        check_pickups[i] -= cap
                else :
                    p_side_move -= 1
                    p_cycle_flag = False
        
        # 결산
        answer += max(d_side_move, p_side_move) * 2
        delivery_flag = True if check_deliveries[0] > 0 else False
        pickup_flag = True if check_pickups[0] > 0 else False

    return answer

solution(2, 7, [1, 0, 2, 0, 1, 0, 2], [0, 3, 0, 1, 0, 2, 0])
```

```python
def solution(cap, n, deliveries, pickups):
    answer = 0
    deliver_pos = n - 1
    pickup_pos = n - 1
    while deliver_pos >= 0 or pickup_pos >= 0:
        while deliver_pos >= 0 and deliveries[deliver_pos] == 0:
            deliver_pos -= 1
        while pickup_pos >= 0 and pickups[pickup_pos] == 0:
            pickup_pos -= 1
        if deliver_pos < 0 and pickup_pos < 0:
            break
        answer += (max(deliver_pos, pickup_pos) + 1) * 2
        deliver_cap = cap
        pickup_cap = cap
        while deliver_pos >= 0 and deliver_cap > 0:
            if deliveries[deliver_pos] > deliver_cap:
                deliveries[deliver_pos] -= deliver_cap
                deliver_cap = 0
            else :
                deliver_cap -= deliveries[deliver_pos]
                deliveries[deliver_pos] = 0
                deliver_pos -= 1
        while pickup_pos >= 0 and pickup_cap > 0:
            if pickups[pickup_pos] > pickup_cap:
                pickups[pickup_pos] -= pickup_cap
                pickup_cap = 0
            else :
                pickup_cap -= pickups[pickup_pos]
                pickups[pickup_pos] = 0
                pickup_pos -= 1
    return answer
```

```python
print(solution(4, 5, [1,0,3,1,2], [0,3,0,4,0]))
# 16
print(solution(2, 7, [1, 0, 2, 0, 1, 0, 2], [0, 2, 0, 1, 0, 2, 0]))
# 30
```

### [택배 상자 꺼내기](https://school.programmers.co.kr/learn/courses/30/lessons/389478)

1 ~ n의 번호가 있는 택배 상자가 창고에 있습니다. 당신은 택배 상자들을 다음과 같이 정리했습니다.

왼쪽에서 오른쪽으로 가면서 1번 상자부터 번호 순서대로 택배 상자를 한 개씩 놓습니다. 가로로 택배 상자를 w개 놓았다면 이번에는 오른쪽에서 왼쪽으로 가면서 그 위층에 택배 상자를 한 개씩 놓습니다. 그 층에 상자를 w개 놓아 가장 왼쪽으로 돌아왔다면 또다시 왼쪽에서 오른쪽으로 가면서 그 위층에 상자를 놓습니다. 이러한 방식으로 n개의 택배 상자를 모두 놓을 때까지 한 층에 w개씩 상자를 쌓습니다.

![alt text](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/e06b4c0d-0ce6-4a2d-8ad4-ba20f9398145/ex1-1.png)

위 그림은 w = 6일 때 택배 상자 22개를 쌓은 예시입니다.

다음 날 손님은 자신의 택배를 찾으러 창고에 왔습니다. 당신은 손님이 자신의 택배 상자 번호를 말하면 해당 택배 상자를 꺼내줍니다. 택배 상자 A를 꺼내려면 먼저 A 위에 있는 다른 모든 상자를 꺼내야 A를 꺼낼 수 있습니다. 예를 들어, 위 그림에서 8번 상자를 꺼내려면 먼저 20번, 17번 상자를 꺼내야 합니다.

당신은 꺼내려는 상자 번호가 주어졌을 때, 꺼내려는 상자를 포함해 총 몇 개의 택배 상자를 꺼내야 하는지 알고 싶습니다.

창고에 있는 택배 상자의 개수를 나타내는 정수 n, 가로로 놓는 상자의 개수를 나타내는 정수 w와 꺼내려는 택배 상자의 번호를 나타내는 정수 num이 매개변수로 주어집니다. 이때, 꺼내야 하는 상자의 총개수를 return 하도록 solution 함수를 완성해 주세요.

##### 제한사항
2 ≤ n ≤ 100

1 ≤ w ≤ 10

1 ≤ num ≤ n

```python
def solution(n, w, num):
    answer = 1

    if num % w == 0: # 위
        move = (1, 2 * w - 1)
    elif (num // w) % 2 == 0: # 오른쪽
        move = ((w - (num % w)) * 2 + 1, (num % w) * 2 - 1)
    elif (num // w) % 2 == 1: # 왼쪽 
        move = ((w - (num % w)) * 2 + 1, (num % w) * 2 - 1)

    clock = 0
    upstair = num + move[clock % 2]
    now = num
    while upstair <= n:
        answer += 1
        now = upstair
        clock += 1
        upstair = now + move[clock % 2]

    return answer
```

```python
print(solution(22, 6, 8))
# 3
```

### [교점에서 별 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/87377)

Ax + By + C = 0으로 표현할 수 있는 n개의 직선이 주어질 때, 이 직선의 교점 중 정수 좌표에 별을 그리려 합니다.

예를 들어, 다음과 같은 직선 5개를

2x - y + 4 = 0

-2x - y + 4 = 0

-y + 1 = 0

5x - 8y - 12 = 0

5x + 8y + 12 = 0

좌표 평면 위에 그리면 아래 그림과 같습니다.

![alt text](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d440b8f4-91c3-4272-8a81-876e9aaffb9c/RisingStarGraphBox.jpg)

이때, 모든 교점의 좌표는 (4, 1), (4, -4), (-4, -4), (-4, 1), (0, 4), (1.5, 1.0), (2.1, -0.19), (0, -1.5), (-2.1, -0.19), (-1.5, 1.0)입니다. 이 중 정수로만 표현되는 좌표는 (4, 1), (4, -4), (-4, -4), (-4, 1), (0, 4)입니다.

만약 정수로 표현되는 교점에 별을 그리면 다음과 같습니다.

![alt text](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/15ffe460-62dc-48df-82a2-7d7636809454/RisingStarGraphStar.jpg)

위의 그림을 문자열로 나타낼 때, 별이 그려진 부분은 *, 빈 공간(격자선이 교차하는 지점)은 .으로 표현하면 다음과 같습니다.

```
"..........."  
".....*....."  
"..........."  
"..........."  
".*.......*."  
"..........."  
"..........."  
"..........."  
"..........."  
".*.......*."  
"..........."  
```

이때 격자판은 무한히 넓으니 모든 별을 포함하는 최소한의 크기만 나타내면 됩니다.

따라서 정답은

```
"....*...."  
"........."  
"........."  
"*.......*"  
"........."  
"........."  
"........."  
"........."  
"*.......*"  
```

입니다.

직선 A, B, C에 대한 정보가 담긴 배열 line이 매개변수로 주어집니다. 이때 모든 별을 포함하는 최소 사각형을 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

line의 세로(행) 길이는 2 이상 1,000 이하인 자연수입니다.

line의 가로(열) 길이는 3입니다.

line의 각 원소는 [A, B, C] 형태입니다.

A, B, C는 -100,000 이상 100,000 이하인 정수입니다.

무수히 많은 교점이 생기는 직선 쌍은 주어지지 않습니다.

A = 0이면서 B = 0인 경우는 주어지지 않습니다.

정답은 1,000 * 1,000 크기 이내에서 표현됩니다.

별이 한 개 이상 그려지는 입력만 주어집니다.

```python
def find_cross_point(line1, line2):
    A1x, B1y, C1 = line1
    A2x, B2y, C2 = line2
    denominator = A1x * B2y - A2x * B1y
    if denominator == 0:
        return None  # 평행하거나 일치하는 직선

    x_numerator = B1y * C2 - B2y * C1
    y_numerator = C1 * A2x - C2 * A1x

    if x_numerator % denominator != 0 or y_numerator % denominator != 0:
        return None  # 정수 좌표가 아님

    x = x_numerator // denominator
    y = y_numerator // denominator
    return (x, y)

def solution(line):
    answer = []

    """
        모든 직선의 교점 구하기
        교점 중 정수 좌표인 것들만 고르기
        좌표의 x, y 최솟값 최댓값 구하기
        최대 최소와 좌표들을 이용해 맵 만들기
    """

    x = {}
    y = {}
    points = []
    for i in range(len(line)):
        for j in range(i + 1, len(line)):
            point = find_cross_point(line[i], line[j])
            if point is None:
                continue
            points.append(point)
            if "min" not in x or x["min"] > point[0]:
                x["min"] = point[0]
            if "max" not in x or x["max"] < point[0]:
                x["max"] = point[0]
            if "min" not in y or y["min"] > point[1]:
                y["min"] = point[1]
            if "max" not in y or y["max"] < point[1]:
                y["max"] = point[1]

    answer = [["."] * (x["max"] - x["min"] + 1) for _ in range(y["max"] - y["min"] + 1)]
    for point in points:
        answer[y["max"] - point[1]][point[0] - x["min"]] = "*"
    answer = ["".join(row) for row in answer]

    return answer
```

```python
res = solution([[2, -1, 4], [-2, -1, 4], [0, -1, 1], [5, -8, -12], [5, 8, 12]])
"""[
"....*....", 
".........", 
".........", 
"*.......*", 
".........", 
".........", 
".........", 
".........", 
"*.......*"
]"""
for row in res:
    print(row)
```

### [3 x n 타일링](https://school.programmers.co.kr/learn/courses/30/lessons/12902)

가로 길이가 2이고 세로의 길이가 1인 직사각형 모양의 타일이 있습니다. 이 직사각형 타일을 이용하여 세로의 길이가 3이고 가로의 길이가 n인 바닥을 가득 채우려고 합니다. 타일을 채울 때는 다음과 같이 2가지 방법이 있습니다

- 타일을 가로로 배치 하는 경우

- 타일을 세로로 배치 하는 경우

예를들어서 n이 8인 직사각형은 다음과 같이 채울 수 있습니다.

![alt text](https://i.imgur.com/zBW7peI.png)

직사각형의 가로의 길이 n이 매개변수로 주어질 때, 이 직사각형을 채우는 방법의 수를 return 하는 solution 함수를 완성해주세요.

##### 제한사항

가로의 길이 n은 5,000이하의 자연수 입니다.

경우의 수가 많아 질 수 있으므로, 경우의 수를 1,000,000,007으로 나눈 나머지를 return해주세요.

```python
def solution(n):
    dp = [0,3,11]
    # f(n) = f(n-2) x 3 + f(n-4) x 2 + … + f(2) x 2 + 2
    if n % 2 == 1:
        return 0
    divider = 1000000007
    n //= 2
    # f(n) = f(n-1) x 3 + f(n-2) x 2 + … + f(1) x 2 + 2
    for i in range(3, n + 1):
        # 점화식 세우기 (규칙 찾기)
        dp.append((dp[i - 1] * 3 + sum(dp[1:i-1]) * 2 + 2) % divider)

    return dp[n]
```

```python
print(solution(4))
# 11
print(solution(6))
# 41
```

### [완전범죄](https://school.programmers.co.kr/learn/courses/30/lessons/389480)

A도둑과 B도둑이 팀을 이루어 모든 물건을 훔치려고 합니다. 단, 각 도둑이 물건을 훔칠 때 남기는 흔적이 누적되면 경찰에 붙잡히기 때문에, 두 도둑 중 누구도 경찰에 붙잡히지 않도록 흔적을 최소화해야 합니다.

물건을 훔칠 때 조건은 아래와 같습니다.

- 물건 i를 훔칠 때,

    - A도둑이 훔치면 info[i][0]개의 A에 대한 흔적을 남깁니다.

    - B도둑이 훔치면 info[i][1]개의 B에 대한 흔적을 남깁니다.

- 각 물건에 대해 A도둑과 B도둑이 남기는 흔적의 개수는 1 이상 3 이하입니다.

경찰에 붙잡히는 조건은 아래와 같습니다.

- A도둑은 자신이 남긴 흔적의 누적 개수가 n개 이상이면 경찰에 붙잡힙니다.

- B도둑은 자신이 남긴 흔적의 누적 개수가 m개 이상이면 경찰에 붙잡힙니다.

각 물건을 훔칠 때 생기는 흔적에 대한 정보를 담은 2차원 정수 배열 info, A도둑이 경찰에 붙잡히는 최소 흔적 개수를 나타내는 정수 n, B도둑이 경찰에 붙잡히는 최소 흔적 개수를 나타내는 정수 m이 매개변수로 주어집니다. 두 도둑 모두 경찰에 붙잡히지 않도록 모든 물건을 훔쳤을 때, A도둑이 남긴 흔적의 누적 개수의 최솟값을 return 하도록 solution 함수를 완성해 주세요. 만약 어떠한 방법으로도 두 도둑 모두 경찰에 붙잡히지 않게 할 수 없다면 -1을 return해 주세요.

##### 제한사항

- 1 ≤ info의 길이 ≤ 40

- info[i]는 물건 i를 훔칠 때 생기는 흔적의 개수를 나타내며, [A에 대한 흔적 개수, B에 대한 흔적 개수]의 형태입니다.

- 1 ≤ 흔적 개수 ≤ 3

- 1 ≤ n ≤ 120

- 1 ≤ m ≤ 120

```python
def solution(info, n, m):
    INF = float('inf')
    N = len(info)
    # dp[i][a] : 처음 i개 아이템까지 고려했을 때, A의 흔적이 a일 때의 최소 B의 흔적
    dp = [[INF] * n for _ in range(N + 1)]
    dp[0][0] = 0  # 초기 상태

    for i in range(N):
        a_risk, b_risk = info[i]
        for a in range(n):
            if dp[i][a] == INF:
                continue  # 도달할 수 없는 상태는 건너뛰기

            # 1. A 도둑이 아이템 i를 훔치는 경우
            new_a = a + a_risk
            if new_a < n:  # A의 누적 흔적이 n 미만이어야 함
                dp[i + 1][new_a] = min(dp[i + 1][new_a], dp[i][a])
            
            # 2. B 도둑이 아이템 i를 훔치는 경우
            new_b = dp[i][a] + b_risk
            if new_b < m:  # B의 누적 흔적이 m 미만이어야 함
                dp[i + 1][a] = min(dp[i + 1][a], new_b)
    
    # 모든 아이템을 고려한 후, 유효한 상태(두 도둑 모두 안전)에서 A의 흔적의 최솟값을 찾음
    answer = INF
    for a in range(n):
        if dp[N][a] < m:
            answer = min(answer, a)
    
    return answer if answer != INF else -1

print(solution([[1, 2], [2, 3], [2, 1]], 4, 4))
```

```python
print(solution([[1, 2], [2, 3], [2, 1]], 4, 4))
# 2
print(solution([[1, 2], [2, 3], [2, 1]], 1, 7))
# 0
print(solution([[3, 3], [3, 3]], 7, 1))
# 6
print(solution([[3, 3], [3, 3]], 6, 1))
# -1
```

## 초기 스크립트 풀이

### [문자열 내 마음대로 정렬하기](https://school.programmers.co.kr/learn/courses/30/lessons/12915)

```python
#programmers 문자열 내 마음대로 정렬하기
def solution(strings, n):
        # n번째로 인덱스로 정렬하고, 그 인덱스가 같다면 사전순으로 정렬
        strings.sort(key = lambda text : (text[n], text))
        return strings
```

### [소수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/12921)

```python
#programmers 소수 찾기
def solution(n):
        answer = 0
        eratos = []
        for i in range(0, 1000002):
                eratos.append(True)
        
        for i in range(2, n + 1):
                if eratos[i]:
                        answer += 1
                        tmp = i + i
                        while tmp <= 1000000:
                                eratos[tmp] = False
                                tmp += i
        return answer
```

### [정수 제곱근 판별](https://school.programmers.co.kr/learn/courses/30/lessons/12934)

```python
# programmers 정수 제곱근 판별
def solution(n):
        sqr = int(n ** 0.5)
        tmp = sqr
        if tmp ** 2 == n:
                return (sqr + 1) ** 2
        else:
                return -1
```

### [예산](https://school.programmers.co.kr/learn/courses/30/lessons/12982)

```python
# programmers 예산
def solution(d, budget):
        d.sort()
        answer = 0
        for idx in d:
                if idx <= budget:
                        budget -= idx
                        answer += 1
        return answer
```

### [탑](https://school.programmers.co.kr/learn/courses/30/lessons/42588)

```python
# programmers 탑
def solution(heights):
        answer = []
    
        for i in range(len(heights)):
                answer.append(0)
    
        for i in range(len(heights) - 1, -1, -1):
                for j in range(i, -1, -1):
                        if heights[i] < heights[j]:
                                answer[i] = j + 1
                                break;
        return answer
```

### [문자열 압축](https://school.programmers.co.kr/learn/courses/30/lessons/60057)

```python
#programmers 문자열 압축
def solution(s):
	ans = len(s)
    # 압축량에 따라서 확인한다.
	for x in range(1, int(len(s)/2) + 1):
        # 총 문자열의 길이
		d = 0
        # 비교 문자열
		comp = ''
        # same count
		c = 1
        
        # 문자열을 search한다.
		for i in range(0, len(s), x):
            #문자열 복사
			temp = s[i:i + x]
            
            # 비교
            # 같다면 count를 늘려준다.
			if comp == temp:
				c += 1
            # 다르다면
            # 총 문자열 길이를 더해주고
            # 비교 문자열을 갱신한다.
			else:
				d += len(temp)
                
                # 1이면 무시, 1초과라면 int형을 str으로 변환하여 길이를
                # 총 문자열 길이에 더한다.
				if c > 1:
					d += len(str(c))
				c = 1
				comp = temp
		
        # 남은 찌꺼기가 있으면
        if c > 1:
			d += len(str(c))
		
        ans = min(ans, d)
	return ans
```

