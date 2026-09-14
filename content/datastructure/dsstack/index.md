---
title: "[DS] Stack"
date: "2019-11-15"
weight: 5
---

# Stack(스택)

### 1. 개념
- 가장 늦게 들어간 자료가 가장 먼저 나가는 구조이다.
- 후입선출(LIFO, Last In First Out)이라고도 부른다.
- Stack에 데이터를 넣는 행동을 push, 데이터를 빼는 행동을 pop이라 한다.
- Stack의 가장 위 데이터를 가르키는 포인터를 top이라고 한다.


![IMAGE](/images/stack1.png)

---

### 2. 구현
- Stack class를 구성할 멤버 변수와 멤버 함수를 구성합니다.
	- 멤버 변수
		- stack : 데이터를 저장할 동적 배열
		- top : 가장 윗 부분을 알려주는 포인터
		- capacity : stack의 최대 크기를 설정하는 변수
	- 멤버 함수
		- Stack() : capacity를 10으로 하여 Stack을 생성하는 생성자
		- Stack(int) : capacity를 입력으로 받아 Stack을 생성하는 생성자
		- Top() : 가장 위 데이터를 가져오는 함수
		- Push, Pop : 데이터를 넣는 함수, 데이터를 빼는 함수
		- isEmpty() : Stack이 비어있는지 판단하는 함수
		- isFull() : Stack이 초깅 설정한 capacity에 도달 했는지(꽉 찼는지) 판단하는 함수
		

```cpp
template <class T>
class Stack
{
private :
	T* stack;
	T top;
	int capacity;
public :
	Stack();
	Stack(int);
	T Top();
	void Push(T);
	void Pop();
	bool isEmpty();
	bool isFull();
};
```


- 각 멤버 함수의 구현

- Stack()
```cpp
template <class T>
Stack<T>::Stack()
{
	this->capacity = 10;
	this->stack = new T[this->capacity];
	this->top = -1;
}
```
- Stack(int capacity)
```cpp
template <class T>
Stack<T>::Stack(int capacity)
{
	this->capacity = capacity;
	this->stack = new T[this->capacity];
	this->top = -1;
}
```
- T Top()
```cpp
template <class T>
T Stack<T>::Top()
{
	T ret = NULL;
	if (!this->isEmpty())
	{
		ret = this->stack[top];
	}
	return ret;
}
```
- void Push(T)
```cpp
template <class T>
void Stack<T>::Push(T data)
{
	if (!this->isFull())
	{
		this->stack[++top] = data;
	}
}
```
- void Pop()
```cpp
template <class T>
void Stack<T>::Pop()
{
	if (!this->isEmpty())
	{
		this->top--;
	}
}
```
- bool isEmpty()
```cpp
template <class T>
bool Stack<T>::isEmpty()
{
	if (this->top == -1)
	{
		return true;
	}
	return false;
}
```
- bool isFull()
```cpp
template <class T>
bool Stack<T>::isFull()
{
	if (this->top == (this->capacity - 1))
	{
		return true;
	}
	return false;
}
```

- 전체 코드 및 테스트
```cpp
#include <iostream>

using namespace std;

template <class T>
class Stack
{
private :
	T* stack;
	T top;
	int capacity;
public :
	Stack();
	Stack(int);
	T Top();
	void Push(T);
	void Pop();
	bool isEmpty();
	bool isFull();
};

template <class T>
Stack<T>::Stack()
{
	this->capacity = 10;
	this->stack = new T[this->capacity];
	this->top = -1;
}

template <class T>
Stack<T>::Stack(int capacity)
{
	this->capacity = capacity;
	this->stack = new T[this->capacity];
	this->top = -1;
}

template <class T>
T Stack<T>::Top()
{
	T ret = NULL;
	if (!this->isEmpty())
	{
		ret = this->stack[top];
	}
	return ret;
}

template <class T>
void Stack<T>::Push(T data)
{
	if (!this->isFull())
	{
		this->stack[++top] = data;
	}
}

template <class T>
void Stack<T>::Pop()
{
	if (!this->isEmpty())
	{
		this->top--;
	}
}

template <class T>
bool Stack<T>::isEmpty()
{
	if (this->top == -1)
	{
		return true;
	}
	return false;
}

template <class T>
bool Stack<T>::isFull()
{
	if (this->top == (this->capacity - 1))
	{
		return true;
	}
	return false;
}

int main()
{
	cout << "capacity가 70인 Stack 생성\n";
	Stack<int> stack(70);
	if (stack.isEmpty())
	{
		cout << "stack이 비어있습니다.\n";
	}
	cout << "0부터 34까지 stack에 push합니다\n";
	for (int i = 0; i < 35; i++)
	{
		stack.Push(i);
	}
	cout << "stack의 Top : " << stack.Top() << '\n';
	cout << "35부터 69까지 stack에 push합니다\n";
	for (int i = 35; i < 70; i++)
	{
		stack.Push(i);
	}
	cout << "stack의 Top : " << stack.Top() << '\n';
	if (stack.isFull())
	{
		cout << "stack이 꽉 찼습니다.\n";
	}
	cout << "stack에서 15개의 데이터를 pop합니다\n";
	for (int i = 0; i < 15; i++)
	{
		stack.Pop();
	}
	cout << "stack의 Top : " << stack.Top() << '\n';
}
```
- 결과
```
capacity가 70인 Stack 생성
stack이 비어있습니다.
0부터 34까지 stack에 push합니다
stack의 Top : 34
35부터 69까지 stack에 push합니다
stack의 Top : 69
stack이 꽉 찼습니다.
stack에서 15개의 데이터를 pop합니다
stack의 Top : 54
```

---

- C++ STL vector를 Stack처럼 사용할 수 있습니다. 자세한 내용은 <a href="/post/stl_vector">[ C++ STL vector ]</a>에서 확인하세요.


