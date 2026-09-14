---
title: "[C++ STL] Queue"
date: 2019-11-16T20:08:14+09:00
weight: 6
---

## Queue
- 내부적으로는 deque, list로 구성되어 있다.
- queue구조로 동작하도록 멤버 함수를 제공해준다.
- `<queue>` 헤더파일에 존재한다.
- std namespace를 사용하면 편리하다.
- 기본 생성자

    - 기본 생성자 형식 : `queue<[data type]> [변수이름]`
    ```cpp
    queue<int> q;
    queue<sting> q;
    ```
    - 내부 컨테이너 구조를 바꾸는 생성자 형식 : `queue<[data type], [container Type]> [변수이름]`
    ```cpp
    queue<int, list<int>> q;
    queue<string, list<string>> q;
    ```

- 멤버 함수

- front()
```cpp
//첫번째 원소에 접근합니다.
//들어간지 가장 오래된 원소.
const value_type& front() const
```
- back()
```cpp
//마지막 원소에 접근합니다.
//가장 최근에 들어간 원소
const value_type& back() const
```
- empty()
```cpp
//비어있으면 true
//size가 0이면 true
```
- size()
```cpp
//원소의 갯수를 반환합니다.
```
- push()
```cpp
//초기화된 원소를 queue안에 집어 넣는다.
```
- pop()
```cpp
//들어간지 가장 오래된 원소를 삭제한다.
//맨 앞에 있는 원소를 삭제한다.
//반환형이 void이므로 앞의 원소를 참조하려면 front를 사용해야 한다.
```
---

## priority queue

---

이후에 작성 예정.



