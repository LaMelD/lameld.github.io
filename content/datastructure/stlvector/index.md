---
title: "[C++ STL] Vector"
date: 2019-11-16T20:08:14+09:00
weight: 7
---

## 개념

- vector는 자동으로 메모리가 할당되는 배열이라고 생각하면 됩니다.
- template를 사용하기 때문에 데이터 타입은 마음데로 넣을 수 있습니다.
- Stack과 비슷한 구조를 가지고 있으며, 맨 뒤에서 삽입과 삭제가 가능합니다.
- 중간의 값을 삭제하거나 삽입이 가능하지만, 배열기반이므로 삽입과 삭제가 빈번하게 일어난다면 효율이 떨어집니다.

---

## 기본 생성법

- `<vector>` 헤더파일을 추가해야한다.
- 선언 : `vector<[data type]> [변수이름]`
	- `vector<int> v;`

- 비어있는 vector v를 생성합니다.
```cpp
vector<int> v;
```
- 기본값(0)으로 초기화 된 N개의 원소를 가지는 vector v를 생성합니다.
```cpp
vector<int> v(N);
```
- K로 초기화된 N개의 원소를 가지는 vector v를 생성합니다.
```cpp
vector<int> v(N, K);
```
- v2는 v1 vector를 복사해서 생성됩니다.
```cpp
vector<int> v2(v1);
```

## 멤버 함수

- `vector<int> v`를 생성한 상태라고 가정한다.

- v.assign(n, k);
```cpp
//k의 값으로 5개의 원소 할당
```
- v.at(index);
```cpp
//index 번째 원소를 참조한다.
//v[index] 보다 속도는 느리지만, 범위를 점검하므로 안전하다.
```
- `v[index]`;
```cpp
//index 번째 원소를 참조한다.
//범위를 점검하지 않으므로 속도가 v.at(index) 보다 빠르다.
```
- v.front();
```cpp
//첫번째 원소를 참조한다.
```
- v.back();
```cpp
//마지막 원소를 참조한다.
```
- v.clear();
```cpp
//모든 원소를 제거한다.
//원소만 제거하고 메모리는 남아있다.(size만 줄어들고 capacity는 그대로)
```
- v.push_back(k);
```cpp
//마지막 원소 뒤에 원소 k를 삽입한다.
```
- v.pop_back();
```cpp
//마지막 원소를 제거한다.
```
- v.begin();
```cpp
//첫번째 원소를 가리킨다.(iterator와 사용)
```
- v.end();
```cpp
//마지막의 "다음"을 가리킨다.(iterator와 사용)
```
- v.rbegin();
```cpp
//reverse begin을 가리킨다.(거꾸로 해서 첫번째 원소)
```
- v.rend();
```cpp
//reverse end를 가리킨다. (거꾸로 해서 마지막의 다음을 가리킨다)
```
- v.reserve(n);
```cpp
//n개의 원소를 저장할 위치를 예약(동적할당)한다.
```
- v.resize(n);
```cpp
//크기를 n으로 변경한다.
```
- v.resize(n, k);
```cpp
//크기를 n으로 변경한다.
//더 커졌을 경우 인자의 값을 3으로 초기화 한다.
```
- v.size();
```cpp
//원소의 갯수를 리턴한다.
```
- v.capacity();
```cpp
//할당된 공간의 크기를 리턴한다.
```
- v2.swap(v1);
```cpp
//v1과 v2의 원소와 capacity를 바꿔준다.
```
- v.insert(index, n, k);
```cpp
//index번째 위치에 n개의 k값을 삽입한다.(이후의 데이터는 뒤로 밀린다)
```
- v.insert(index, k);
```cpp
//index번째 위치에 k값을 삽입한다.
//삽입한 위치의 iterator를 반환한다.
```
- v.erase(iterator);
```cpp
//iterator가 가리키는 원소를 제거한다.
//size만 줄어들고 capacity는 그대로 남는다.
//--- erase에 관련 내용은 추후에 추가하겠습니다. ---
```
- v.empty();
```cpp
//vector가 비어있으면 true를 리턴한다.
//비어 있는 기준은 size가 0이라는 의미이다.
```


## size()와 capacity()

- size : 할당된 메모리 안에 data가 들어있는 갯수
- capacity : 할당된 메모리의 크기
- `vector<int> v` 에서 push를 진행한 뒤 pop을 하면 size는 증가하고 감소하지만, capacity는 증가하기만 한다.
- DS Array에서 배열의 크기를 2배로 증가시키는 방법과 유사하다.













