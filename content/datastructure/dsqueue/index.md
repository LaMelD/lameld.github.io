---
title: "[DS] Queue"
date: "2019-11-15"
weight: 4
---

# Queue(큐)

### 1. 개념
- Queue는 FIFO(First In First Out)의 원리로 동작하는 자료구조이다.
- 동일한 자료의 집합을 다룬다는 면에서 Stack과 비슷하지만 가장 먼저 들어간 자료가 가장 늦게 나온다는 점이 다르다.
- 넣은 순서대로 자료를 꺼내가므로 순서대로 처리해야 하는 자료를 임시작으로 저장하는 용도로 흔히 사용한다.
- 저장되는 자료의 타입이 동일하므로 배열 또는 연결리스트로 Queue를 구현할 수 있다.

![IMAGE](/images/queue1.png)

- 선형 큐(Linear Queue)
	- 선형 큐는 큐의 가장 단순한 형태로써, 큐의 가장 앞을 가리키는 front와 가장 뒤를 가리키는 rear를 갖는다. 자료를 추가하면, 현재 rear가 가리키는 위치에 자료를 추가하고, rear는 1만큼 증가한다. 자료를 꺼내면, 현재 front가 가리키는 위치의 자료를 꺼내고 front는 1만큼 증가한다.


![](/images/queue2.png)

- 환영 큐(Circular Queue)
	- 환영 큐는 선형 큐에서 pop 연산이 반복될 수록 큐에 저장할 수 있는 공간이 감소하는 문제점을 극복하기 위해 고안되었다. 기존의 선형 큐를 원형으로 구성한 새로운 형태의 큐를 이용한다. 환형 큐의 구조는 아래와 같다.

![](/images/queue3.png)

---
### 2. 구현 : Circular Queue


```cpp
#include <iostream>
#include <cstring>

using namespace std;

template <class T>
class CircularQueue
{
private :
	T* queue;
	int front = 0;
	int rear = 0;
	int capacity;
public :
	CircularQueue();
	CircularQueue(int);
	bool isEmpty();
	bool isFull();
	void Push(T);
	void Pop();
	T Front();
};

template <class T>
CircularQueue<T>::CircularQueue()
{
	this->capacity = 10;
	this->queue = new T[this->capacity + 1];
}

template <class T>
CircularQueue<T>::CircularQueue(int capacity)
{
	this->capacity = capacity + 1;
	this->queue = new T[this->capacity];
}

template <class T>
bool CircularQueue<T>::isEmpty()
{
	if (this->front == this->rear)
	{
		return true;
	}
	return false;
}

template <class T>
bool CircularQueue<T>::isFull()
{
	if ((this->rear + 1) % this->capacity == this->front)
	{
		return true;
	}
	return false;
}

template <class T>
void CircularQueue<T>::Push(T data)
{
	if (!this->isFull())
	{
		this->queue[this->rear] = data;
		this->rear = (this->rear + 1) % capacity;
	}
}

template <class T>
void CircularQueue<T>::Pop()
{
	if (!this->isEmpty())
	{
		this->front = (this->front + 1) % this->capacity;
	}
}

template <class T>
T CircularQueue<T>::Front()
{
	if (!this->isEmpty())
	{
		return this->queue[this->front];
	}
}

int main()
{
	CircularQueue<int> q(20);
	cout << "capacity가 20인 CircularQueue를 생성합니다.\n0부터 19까지 20개를 Queue에 넣습니다.\n";
	for (int i = 0; i < 20; i++)
	{
		q.Push(i);
	}
	if (q.isFull())
	{
		cout << "꽉 찼습니다.\n";
	}
	cout << "Queue의 Front를 출력하고 Pop하는 동작을 20번 반복합니다.\n";
	for (int i = 0; i < 20; i++)
	{
		cout << q.Front() << ' ';
		q.Pop();
	}
	if (q.isEmpty())
	{
		cout << "\n비어있습니다.\n";
	}

	return 0;
}
```
- 결과
```
capacity가 20인 CircularQueue를 생성합니다
0부터 19까지 20개를 Queue에 넣습니다.
꽉 찼습니다.
Queue의 Front를 출력하고 Pop하는 동작을 20번 반복합니다.
0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19
비어있습니다.
```