---
title: "[DS] Array"
date: "2019-11-15"
weight: 2
---

# Array(배열)
- 정적 배열 (Static Array)
	- 기본적인 배열을 의미한다.
	- C++에서 간단하게 선언이 가능하다.
	
- 1차원
```cpp
int arr0[3];
int arr1[10] = { 0, };
int arr2 = { 1,2,3,4 };
```
- 2차원
```cpp
int arr0[3][7];
int arr1[10][10] = { { 0, }, };
int arr2 = { { 1,2}, { 3,4 } };
```
- 3차원 이상의 배열도 \[ \]를 추가하여 선언이 가능하다.

---

- 동적 배열 (Dynamic Array)
	- 정적 배열은 배열의 크기를 늘리는 것이 불가능하다. 하지만 동적 배열은 배열의 크기를 바꿀 수 있습니다.
	- 기본적인 성질은 정적 배열과 똑같습니다.
	- 포인터(*)와 new를 이용하여 동적으로 배열을 생성할 수 있습니다.
	
![IMAGE](/images/array11.png)
	
- 포인터(*)와 new 또는 malloc(cstring 라이브러리)을 사용
```cpp
int size = 27;

//동적 배열 생성 1 : new
int* arr1 = new int[size];
//동적 배열 생성 2 : malloc
int* arr2 = (int*)malloc(sizeof(int) * size);

//동적 배열의 초기화
//배열의 초기화 방법 1 : 배열의 전체를 방문하여 0으로 초기화한다.
for (int i = 0; i < size; i++)
{
	arr1[i] = 0;
}
//배열의 초기화 방법 2 : memset(cstring 라이브러리)을 사용한다.
memset(arr2, 0, sizeof(int) * size);
```

	
- 방법 1 : 한 개씩 바꾸는 방법
	- 배열의 크기를 하나만 늘리는 방법
	- 저장공간을 최대한 절약할 수 있다.
	- 배열의 크기를 자주 바꾸게 될 경우 계속해서 배열의 크기를 바꿔주는 함수를 호출하게 되어 시간복잡도가 비약적으로 증가하게 된다.

![IMAGE](/images/array22.png)

- 방법 2 : 두 배로 늘리는 방법
	- 배열의 크기를 두배로 늘리는 방법
	- 저장공간은 절약이 불가하다.
	- 배열의 크기가 자주 바뀌어도 미리 만들어 놓은 배열이 있기 때문에 함수를 [ 방법 1 ] 보다 적게 호출하여 시간복잡도가 [ 방법 1 ] 보다는 낮다.

![IMAGE](/images/array33.png)
	
- 동적 배열 class : 2번 방법을 사용한 경우
	- push : 배열의 가장 뒤쪽에 데이터를 추가한다. 배열의 size가 capacity에 도달하면 배열의 크기를 2배 늘려준다.
	- pop : 배열의 가장 뒤쪽의 데이터를 제거한다.
	- set : [ idx ]의 데이터를 input으로 수정한다.
	- at : [ idx ]의 데이터를 가져온다.

- myArray 클래스
```cpp
#include <iostream>
#include <cstring>

using namespace std;

template <class T>
class myArray
{
private:
	T* m_arr;
	int m_size;
	int capacity;
public:
	myArray<T>();
	myArray<T>(int);
	myArray<T>(int, T);

	void push(T);
	void pop();
	T at(int);
	void set(int, T);
	int size();
};

template <class T>
myArray<T>::myArray()
{
	this->m_size = 0;
	this->capacity = 1;
	this->m_arr = new T[this->capacity];
}

template <class T>
myArray<T>::myArray(int size)
{
	this->m_size = size;
	this->capacity = size;
	this->m_arr = new T[size];
}

template <class T>
myArray<T>::myArray(int size, T init)
{
	this->m_size = size;
	this->m_arr = new T[size];
	memset(this->m_arr, init, sizeof(T) * size);
}

template <class T>
void myArray<T>::push(T input)
{
	if (this->m_size == this->capacity)
	{
		this->capacity *= 2;
		T* tmp = new T[this->capacity];
		memcpy(tmp, this->m_arr, sizeof(T) * m_size);
		delete(this->m_arr);
		this->m_arr = tmp;
		this->m_arr[this->m_size++] = input;
	}
	else
	{
		this->m_arr[this->m_size++] = input;
	}
}

template <class T>
void myArray<T>::pop()
{
	m_size--;
	T* tmp = new T[this->m_size];
	memcpy(tmp, this->m_arr, sizeof(T) * this->m_size);
	delete(this->m_arr);
	this->m_arr = tmp;
}

template <class T>
T myArray<T>::at(int idx)
{
	return this->m_arr[idx];
}

template <class T>
void myArray<T>::set(int idx, T input)
{
	this->m_arr[idx] = input;
}

template <class T>
int myArray<T>::size()
{
	return this->m_size;
}
```
	
- C++에서는 위의 클래스를 최적화시켜 STL을 지원한다.

- C++ STL vector
```cpp
vector<int> arr1;
vector<int> arr2(12);
vector<int> arr3(20, 0);

arr1.push_back(111);
arr1.pop_back();
int val = arr3[10];
arr3.erase(arr3.begin() + 2);
```
