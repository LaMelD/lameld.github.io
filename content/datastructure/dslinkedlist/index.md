---
title: "[DS] Single Linked List"
date: "2019-11-15"
weight: 3
---

# Single Linked List (단순 연결 리스트)

### 1. 개념
- 하나의 노드에 필요한 정보를 담고 다음에 해당하는 노드를 가리키고 있는 자료구조로 포인터를 이용해 자료들을 선형으로 연결한 자료구조이다.
- \[ 배열 \] 과 비교했을 때 추가 및 삭제가 쉽다는 장점이 있지만 접근할 때 O(n)만큼 걸린다는 단점이 있습니다.

-----

### 2. 구현
- List를 구성할 노드를 생성하고 기본적인 기능을 추가합니다.
	- private 멤버 변수
		- data : 노드의 값
		- pnext : 현재 노드의 다음을 알려주는 포인터
	- public 멤버 함수
		- Node(), Node(T) : 생성자
		- getData() : 노드의 data를 리턴한다.
		- setData() : 노드의 data를 설정한다.
		- getNext() : 현재 노드의 다음 노드를 리턴한다.
		- setNext() : 현재 노드의 다음 노드를 설정한다.


```cpp
template <class T>
class Node
{
private:
	T data;
	Node<T>* pnext;
public:
	Node();
	Node(T);
	T getData() { return this->data; }
	void setData(T n) { this->data = n; }
	singlenode<T>* getNext() { return this->pnext; }
	void setNext(singlenode<T>* pnode) { this->pnext = pnode; }
};
template <class T>
Node<T>::Node()
{
	this->data = 0;
	this->pnext = NULL;
}
template <class T>
Node<T>::Node(T n)
{
	this->data = n;
	this->pnext = NULL;
}
```



- Node가 들어갈 List의 틀(class)을(를) 생성하고 필요한 기능과 멤버 변수를 추가해줍니다.
	- private 멤버 변수
		- phead : List의 가장 앞 부분을 알려준다.
		- ptail : List의 끝을 알려준다.
		- ssize : List에 들어있는 노드의 갯수를 알려준다.
	- public 멤버 함수
		- SingleLinkedList() : 생성자
		- isEmpty() : List가 비어있다면 True, 아니면 False
		- size() : List에 들어있는 노드의 갯수를 리턴한다.
		- getHead() : List의 가장 앞 부분을 알려주는 phead를 리턴한다
		- getData(), AddFront(), AddBack(), InsertNode(), DeleteNode()
		

```cpp
template <class T>
class SingleLinkedList
{
private:
	Node<T>* phead;
	Node<T>* ptail;
	int ssize;
public:
	SingleLinkedList();
	bool isEmpty() { return this->phead == NULL ? true : false; }
	int size() { return this->ssize; }
	Node<T>* getHead() { return this->phead; }
	T getData(int);
	void AddFront(T);
	void AddBack(T);
	void InsertNode(Node<T>*, T);
	void DeleteNode(int);
};
```



- 멤버 함수 상세 내용

- getData(int n) : phead부터 n번째 data를 리턴한다.
```cpp
template <class T>
T SingleLinkedList<T>::getValue(int n)
{
	Node<T>* tmp = this->phead;
	for (int i = 0; i < n; i++)
	{
		tmp = tmp->getNext();
	}
	
	return tmp->getData();
}
```
- AddFront(T n) : List의 맨 앞에 추가한다.
```cpp
template <class T>
void SingleLinkedList<T>::AddFront(T n)
{
	Node<T>* tmp = new Node<T>(n);

	this->ssize++;

	if (this->phead == NULL)
	{
		this->phead = tmp;
		this->ptail = tmp;
	}
	else
	{
		tmp->setNext(this->phead);
		this->phead = tmp;
	}
```
- AddBack(T n) : List의 맨 뒤에 추가한다
```cpp
template <class T>
void SingleLinkedList<T>::AddBack(T n)
{
	Node<T>* tmp = new Node<T>(n);

	this->ssize++;

	if (this->phead == NULL)
	{
		this->phead = tmp;
		this->ptail = tmp;
	}
	else
	{
		this->ptail->setNext(tmp);
		this->ptail = tmp;
	}
}
```
- InsertNode(Node\<T\>* prev, T n) : prev 노드 뒤에 n의 value를 갖는 노드를 추가한다.
```cpp
template <class T>
void SingleLinkedList<T>::InsertNode(Node<T>* previous, T n)
{
	Node<T>* tmp = new Node<T>(n);

	this->ssize++;

	tmp->setNext(previous->getNext());
	previous->setNext(tmp);
}
```
- DeleteNode(int n) : phead부터 n번째 인덱스의 다음 node를 삭제한다.
```cpp
template <class T>
void SingleLinkedList<T>::DeleteNode(int n)
{
	if (ssize != 0 && n < ssize)
	{
		this->ssize--;

		if (n == 0)
		{
			this->phead = this->phead->getNext();
		}
		else
		{
			Node<T>* previous = this->phead;
			Node<T>* tmp = previous->getNext();

			for (int i = 1; i < n; i++)
			{
				previous = tmp;
				tmp = previous->getNext();
			}
			if (n == ssize)
			{
				this->ptail = previous;
				previous->setNext(NULL);
			}
			previous->setNext(tmp->getNext());
		}

		if (ssize == 0)
		{
			this->phead = NULL;
			this->ptail = NULL;
		}
	}
}
```


- 전체 코드
```cpp
#include <iostream>

using namespace std;

template <class T>
class Node
{
private:
	T data;
	Node<T>* pnext;

public:
	Node();
	Node(T);
	T getData() { return this->data; }
	void setData(T n) { this->data = n; }
	Node<T>* getNext() { return this->pnext; }
	void setNext(Node<T>* pnode) { this->pnext = pnode; }
};
template <class T>
Node<T>::Node()
{
	this->data = 0;
	this->pnext = NULL;
}
template <class T>
Node<T>::Node(T n)
{
	this->data = n;
	this->pnext = NULL;
}

template <class T>
class SingleLinkedList
{
private:
	Node<T>* phead;
	Node<T>* ptail;
	int ssize;

public:
	SingleLinkedList();
	bool isEmpty() { return this->phead == NULL ? true : false; }
	int size() { return this->ssize; }
	Node<T>* getHead() { return this->phead; }
	T getData(int);
	void AddFront(T);
	void AddBack(T);
	void InsertNode(Node<T>*, T);
	void DeleteNode(int);
};
template <class T>
SingleLinkedList<T>::SingleLinkedList()
{
	this->phead = NULL;
	this->ptail = NULL;
	this->ssize = 0;
}
template <class T>
void SingleLinkedList<T>::AddFront(T n)
{
	Node<T>* tmp = new Node<T>(n);

	this->ssize++;

	if (this->phead == NULL)
	{
		this->phead = tmp;
		this->ptail = tmp;
	}
	else
	{
		tmp->setNext(this->phead);
		this->phead = tmp;
	}
}
template <class T>
void SingleLinkedList<T>::AddBack(T n)
{
	Node<T>* tmp = new Node<T>(n);

	this->ssize++;

	if (this->phead == NULL)
	{
		this->phead = tmp;
		this->ptail = tmp;
	}
	else
	{
		this->ptail->setNext(tmp);
		this->ptail = tmp;
	}
}
template <class T>
void SingleLinkedList<T>::InsertNode(Node<T>* previous, T n)
{
	Node<T>* tmp = new Node<T>(n);

	this->ssize++;

	tmp->setNext(previous->getNext());
	previous->setNext(tmp);
}
template <class T>
void SingleLinkedList<T>::DeleteNode(int n)
{
	if (ssize != 0 && n < ssize)
	{
		this->ssize--;

		if (n == 0)
		{
			this->phead = this->phead->getNext();
		}
		else
		{
			Node<T>* previous = this->phead;
			Node<T>* tmp = previous->getNext();

			for (int i = 1; i < n; i++)
			{
				previous = tmp;
				tmp = previous->getNext();
			}
			if (n == ssize)
			{
				this->ptail = previous;
				previous->setNext(NULL);
			}
			previous->setNext(tmp->getNext());
		}

		if (ssize == 0)
		{
			this->phead = NULL;
			this->ptail = NULL;
		}
	}
}
template <class T>
T SingleLinkedList<T>::getData(int n)
{
	Node* tmp = this->phead;
	for (int i = 0; i < n; i++)
	{
		tmp = tmp->getNext();
	}

	return tmp->getData();
}

int main()
{
	SingleLinkedList<int>* sli = new SingleLinkedList<int>();

	sli->AddBack(2);
	sli->AddBack(3);
	sli->AddBack(9);
	sli->AddBack(10);

	sli->AddFront(4);
	sli->AddFront(5);
	sli->AddFront(6);
	sli->AddFront(7);
	sli->AddFront(8);

	cout << "2, 3, 9, 10을 앞에 추가하고, 4, 5, 6, 7, 8을 뒤에 추가한 결과 :";
	Node<int>* tmp = sli->getHead();
	for (int i = 0; i < sli->size(); i++)
	{
		cout << ' ' << tmp->getData();
		tmp = tmp->getNext();
	}
	cout << '\n';

	Node<int>* ptr = sli->getHead();
	ptr = ptr->getNext()->getNext();

	cout << "앞에서 3번째 인덱스 뒤에 150을 추가한다\n";
	sli->InsertNode(ptr, 150);

	cout << "5번째 인덱스 뒤의 노드를 삭제한다\n";
	sli->DeleteNode(5);
	cout << "최종 리스트 :";
	tmp = sli->getHead();
	for (int i = 0; i < sli->size(); i++)
	{
		cout << ' ' << tmp->getData();
		tmp = tmp->getNext();
	}
	cout << '\n';
}
```
- 출력  
```txt
2, 3, 9, 10을 앞에 추가하고, 4, 5, 6, 7, 8을 뒤에 추가한 결과 : 8 7 6 5 4 2 3 9 10
앞에서 3번째 인덱스 뒤에 150을 추가한다
5번째 인덱스 뒤의 노드를 삭제한다
최종 리스트 : 8 7 6 150 5 2 3 9 10
```