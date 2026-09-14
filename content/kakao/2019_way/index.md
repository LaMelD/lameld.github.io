---
title: "[2019] 길 찾기 게임"
date: 2019-11-24T18:15:16+09:00
weight: 14
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42892)

- 문제 설명
	1. 트리를 구성하는 모든 노드의 x, y 좌표 값은 정수이다.
	2. 모든 노드는 서로 다른 x값을 가진다.
	3. 같은 레벨에 있는 노드는 같은 y 좌표를 가진다.
	4. 자식 노드의 y값은 항상 부모 노드보다 작다.
	5. 임의의 노드 V의 왼쪽 서브 트리에 있는 모든 노드의 x값은 V의 x값보다 작다.
	6. 임의의 노드 V의 오른쪽 서브 트리에 있는 모든 노드의 x값은 V의 x값보다 크다.
	7. 노드들로 구성된 이진트리를 전위 준회, 후위 순회한 결과를 리턴한다.

![](/images/way1.png)

- 문제 풀이
	- Tree를 구성할 Node class를 생성한다.
	- Tree class를 생성한다.
	- Tree를 구성할 필수적인 기능을 구현한다.
	- 문제에 input 데이터를 y를 기준으로 정렬하고 Tree에 푸시하여 Tree를 완성한다.
	- preorder, postorder를 각각 구현하여 배열에 넣은뒤 리턴한다.
	
```cpp
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

vector<int> vpreorder;
vector<int> vpostorder;

//Node의 val을 구성할 데이터 내용
struct info
{
	//각각의 좌표와 idx 번호
	int x = 0;
	int y = 0;
	int val = 0;
};

class Node
{
private:
	info val;
	Node* leftChild;
	Node* rightChild;
public:
	Node()
	{
		this->val;
		this->leftChild = nullptr;
		this->rightChild = nullptr;
	}
	Node(info rt)
	{
		this->val = rt;
		this->leftChild = nullptr;
		this->rightChild = nullptr;
	}
	int getValue() { return this->val.val; }
	Node* getRightChild() { return this->rightChild; }
	Node* getLeftChild() { return this->leftChild; }

	void push(info input)
	{
		//오른쪽으로 간다
		if (this->val.x < input.x)
		{
			if (this->getRightChild() == nullptr)
			{
				this->rightChild = new Node(input);
			}
			else
			{
				this->getRightChild()->push(input);
			}
		}
		//왼쪽으로 간다
		else if (input.x < this->val.x)
		{
			if (this->getLeftChild() == nullptr)
			{
				this->leftChild = new Node(input);
			}
			else
			{
				this->getLeftChild()->push(input);
			}
		}
	}
};

class Tree
{
private:
	Node* root;
	int depth;
private:
	void preorder(Node*);
	void postorder(Node*);
public:
	Tree(info rt)
	{
		this->root = new Node(rt);
		this->depth = 1;
	}

	void push(info);
	void preorder() { this->preorder(root); };
	void postorder() { this->postorder(root); };
};
void Tree::push(info input)
{
	this->root->push(input);
}
void Tree::preorder(Node* ptr)
{
	if (ptr == nullptr) return;
	vpreorder.push_back(ptr->getValue());
	preorder(ptr->getLeftChild());
	preorder(ptr->getRightChild());
}
void Tree::postorder(Node* ptr)
{
	if (ptr == nullptr) return;
	postorder(ptr->getLeftChild());
	postorder(ptr->getRightChild());
	vpostorder.push_back(ptr->getValue());
}

vector<vector<int>> solution(vector<vector<int>> nodeinfo)
{
	vector<vector<int>> answer;

	vpreorder.clear();
	vpostorder.clear();

	vector<info> nodes(nodeinfo.size());

	//input 데이터를 info 구조체에 맞게 파싱한다.
	for (int i = 0; i < nodeinfo.size(); i++)
	{
		nodes[i].x = nodeinfo[i][0];
		nodes[i].y = nodeinfo[i][1];
		nodes[i].val = i + 1;
	}

	//파싱한 데이터를 y를 기준으로 오른차순 정렬한다.
	sort(nodes.begin(), nodes.end(), [](info a, info b)
		{
			if (a.y == b.y)
			{
				return a.x < b.x;
			}
			return b.y < a.y;
		}
	);

	//트리를 생성하고 데이터를 넣어준다.
	Tree* tree = new Tree(nodes[0]);

	for (int i = 1; i < nodeinfo.size(); i++)
	{
		tree->push(nodes[i]);
	}

	//preorder, postorder를 각각 진행하여
	//vpreorder와 vpostorder배열을 각각 완성시켜준다.
	tree->preorder();
	tree->postorder();

	//리턴할 배열에 삽입후 리턴한다.
	answer.push_back(vpreorder);
	answer.push_back(vpostorder);

	return answer;
}
```