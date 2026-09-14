---
title: "2. 프로그램의 구조를 쌓는다! 제어문"
date: 2019-11-29T17:38:30+09:00
weight: 3
---

### 1. if문

- 형식
    ```python
    if [조건문]:
        # if의 조건식이 True일 때
        # 실행문
    elif [조건문]:
        # elif의 조건식이 True일 때
        # 실행문
    else:
        # 위의 조건식이 True인 것이 없을 때
        # 실행문
    ```

    ![ifelif](/images/if_elif.PNG)
- 조건문 : 참과 거짓을 판단하는 문장
- 비교연산자 : 조건이 참인지 거짓인지 판단할 때 사용

    ![비교연산자](/images/compare.PNG)
- 논리연산자 : 조건이 참인지 거짓인지 판단할 때 사용

    ![논리연산자](/images/logical.PNG)
- x in s, x not in s

    ![in, not in](/images/innotin.PNG)
- pass : 현재 조건문을 빠져나간다???
    ```python
    for i in range(10):
        if i:
            print(i)
            pass
        print("counting")
        print("complete!")
    ```

-----

### 2. while문

- 기본 구조
    ```python
    while [조건문]:
        수행할 문장1
        수행할 문장2
        ...
    ```

    ![while flow](/images/while.PNG)
    ```python
    treeHit = 0
    while treeHit < 10:
        treeHit += 1
        print("나무를 {0}번 찍었다.".format(treeHit))
        if treeHit == 10:
            print("나무가 넘어갑니다.")
    ```
- 강제로 빠져나가기 : `break`
- 조건에 맞지 않는 경우 맨 처음으로 돌아가기
    ```python
    cnt = 0
    while True:
        cnt += 1
        if a % 2 == 0:
            continue
        print(cnt)
        if a > 50:
            break
    # 51까지 출력된다.
    ```

-----

### 3. for문

- 기본 구조
    ```python
    for [변수] in [리스트(또는 튜플, 문자열)]:
        수행할 문장1
        수행할 문장2
        ...
    ```
- for문과 continue
    - for문 안의 문장을 수행하는 도중에 continue문을 만나면 for문의 처음으로 돌아가게 된다.
- for문과 함께 자주 사용하는 range함수
    - 숫자 리스트를 자동으로 만들어 주는 range함수
    ```python
    a = list(range(10))         # 0 <= x < 9 리스트
    b = list(range(10, 20))     # 10 <= x < 20 리스트
    c = list(range(10, 21, 10)) # [10, 20] 리스트, 마지막 매개변수는 증감수
    ```
- 리스트 안에 for문 포함하기
    ```python
    a = [1, 2, 3, 4]
    result = []
    for num in a:
        result.append(num * 3)
    print(result)
    ```
    ```python
    a = [1, 2, 3, 4]
    result = [num * 3 for num in a]
    print(result)
    ```
    ```python
    a = [1, 2, 3, 4]
    result = [num * 3 for num in a if num % 2 == 0]
    print(result)
    ```
    ```python
    a = [1, 2, 3, 4]
    result = [x * y for x in range(2, 10) for y in range(1, 10)]
    print(result)
    ```