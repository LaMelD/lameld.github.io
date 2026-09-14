---
title: "[2020] 블록 이동하기"
date: 2019-11-27T11:22:43+09:00
weight: 15
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/60063)

- 문제 설명
	- 아래의 왼쪽과 같이 board의 정보가 input으로 주어진다.
	- 0과 1로 이루어진 N x N 크기의 지도에서 `2 x 1` 크기의 로봇을 `(1, 1)` 에서 `(N, N)` 까지 이동시킨다.
	- 시작은 아래의 왼쪽과 같이 시작한다.
	- 로봇은 앞뒤 구분없이 움직일 수 있다.
	- 로봇의 회전은 아래의 오른쪽과 같이 실행 할 수 있다.
	- 로봇이 한칸 이동할 때 걸리는 시간은 1초이다.
	- 90도 회전할 때 1초가 소요된다.

![](/images/moveblock1.jpg)

- 문제 풀이
	- 기본적으로 탐색알고리즘은 다익스트라 알고리즘을 사용한다.
	- Memoization을 하기 위한 배열 visit을 생성한다
	
	- visit :: node
	```cpp
	struct node
	{
		node() : left(false), right(false), top(false), bottom(false) {}

		//현재 위치와 왼쪽에 배치되어 있는가
		bool left;
		//현재 위치와 오른쪽에 배치되어 있는가
		bool right;
		//현재 위치와 위쪽에 배치되어 있는가
		bool top;
		//현재 위치와 아래쪽에 배치되어 있는가
		bool bottom;
	};
	```
	- visit :: 검색
	```cpp
	//로봇이 가로로 있을 경우
	if (pos.p1.x == pos.p2.x)
	{
		//로봇의 두 좌표가 visit 여부가 모두 false여야 진행한다.
		if (visit[pos.p1.x][pos.p1.y].left && visit[pos.p2.x][pos.p2.y].right)
		{
			continue;
		}
		visit[pos.p1.x][pos.p1.y].left = true;
		visit[pos.p2.x][pos.p2.y].right = true;

		state = 0;
	}
	//로봇이 세로로 있을 경우
	else
	{
		//로봇의 두 좌표가 visit 여부가 모두 false여야 진행한다.
		if (visit[pos.p1.x][pos.p1.y].top && visit[pos.p2.x][pos.p2.y].bottom)
		{
			continue;
		}
		visit[pos.p1.x][pos.p1.y].top = true;
		visit[pos.p2.x][pos.p2.y].bottom = true;

		state = 1;
	}
	```
	
	- 좌표를 저장하기 위한 point와 pointset 구조체를 만든다.
	- 시작 좌표를 기준으로 이동가능한 모든 좌표를 queue에 저장하고, 순차적으로 이동시켜 나아가 최적의 값을 구한다.
	
```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

vector<vector<int>> B;

//for visit & memo
struct node
{
	node() : left(false), right(false), top(false), bottom(false) {}

	//현재 위치와 왼쪽에 배치되어 있는가
	bool left;
	//현재 위치와 오른쪽에 배치되어 있는가
	bool right;
	//현재 위치와 위쪽에 배치되어 있는가
	bool top;
	//현재 위치와 아래쪽에 배치되어 있는가
	bool bottom;
};

struct point
{
	point() : x(0), y(0) {}
	int x;
	int y;

	void operator=(point a)
	{
		this->x = a.x;
		this->y = a.y;
	}
};

bool operator==(point a, point b)
{
	if (a.x == b.x && a.y == b.y)
		return true;
	return false;
}

bool operator<(point a, point b)
{
	if (a.x == b.x)
		return a.y < b.y;

	if (a.y == b.y)
		return a.x < b.y;
}

struct pointset
{
	point p1; //(n, n)과 상대적으로 가까운 것
	point p2; //(n, n)과 상대적으로 먼 것
};

bool operator==(pointset a, pointset b)
{
	if (a.p1 == b.p1 && a.p2 == b.p2)
		return true;

	if (a.p1 == b.p2 && a.p2 == b.p1)
		return true;

	return false;
}

struct W
{
	//최소값을 구하기 위한 값을 지정한다.
	int movecount;
	pointset pos;
};

//왼쪽, 오른쪽, 위쪽, 아래쪽 각각 이동이 가능한지 판단
bool moveleft(pointset pos, int& n)
{
	if (0 <= pos.p2.y - 1 && B[pos.p1.x][pos.p1.y - 1] == 0 && B[pos.p2.x][pos.p2.y - 1] == 0)
		return true;
	return false;
}
bool moveright(pointset pos, int& n)
{
	if (pos.p1.y + 1 < n && B[pos.p1.x][pos.p1.y + 1] == 0 && B[pos.p2.x][pos.p2.y + 1] == 0)
		return true;
	return false;
}
bool moveup(pointset pos, int& n)
{
	if (0 <= pos.p2.x - 1 && B[pos.p1.x - 1][pos.p1.y] == 0 && B[pos.p2.x - 1][pos.p2.y] == 0)
		return true;
	return false;
}
bool movedown(pointset pos, int& n)
{
	if (pos.p1.x + 1 < n && B[pos.p1.x + 1][pos.p1.y] == 0 && B[pos.p2.x + 1][pos.p2.y] == 0)
		return true;
	return false;
}

//state
//가로로 있을 때 :: 0
//세로로 있을 때 :: 1

//rotate p1 forward :: p1을 기준으로 시계방향으로 회전이동한다.
//rotate p2 reverse :: p2를 기준으로 반시계방향으로 회전이동한다.
bool rotate_p1_forward(pointset pos, int& n, int state)
{
	if (state == 0)
	{
		if (0 <= pos.p1.x - 1 && B[pos.p2.x - 1][pos.p2.y] == 0 && B[pos.p1.x - 1][pos.p1.y] == 0)
			return true;
	}
	else
	{
		//p1, p2가 스왑되어 들어가야함
		if (pos.p1.y + 1 < n && B[pos.p2.x][pos.p2.y + 1] == 0 && B[pos.p1.x][pos.p1.y + 1] == 0)
			return true;
	}

	return false;
}
bool rotate_p1_reverse(pointset pos, int& n, int state)
{
	if (state == 0)
	{
		//p1, p2가 스왑되어 들어가야함
		if (pos.p1.x + 1 < n && B[pos.p2.x + 1][pos.p2.y] == 0 && B[pos.p1.x + 1][pos.p1.y] == 0)
			return true;
	}
	else
	{
		if (0 <= pos.p1.y - 1 && B[pos.p2.x][pos.p2.y - 1] == 0 && B[pos.p1.x][pos.p1.y - 1] == 0)
			return true;
	}

	return false;
}
bool rotate_p2_forward(pointset pos, int& n, int state)
{
	if (state == 0)
	{
		if (pos.p2.x + 1 < n && B[pos.p1.x + 1][pos.p1.y] == 0 && B[pos.p2.x + 1][pos.p2.y] == 0)
			return true;
	}
	else
	{
		//p1, p2가 스왑되어 들어가야함
		if (0 <= pos.p2.y - 1 && B[pos.p1.x][pos.p1.y - 1] == 0 && B[pos.p2.x][pos.p2.y - 1] == 0)
			return true;
	}

	return false;
}
bool rotate_p2_reverse(pointset pos, int& n, int state)
{
	if (state == 0)
	{
		//p1, p2가 스왑되어 들어가야함
		if (0 <= pos.p2.x - 1 && B[pos.p1.x - 1][pos.p1.y] == 0 && B[pos.p2.x - 1][pos.p2.y] == 0)
			return true;
	}
	else
	{
		if (pos.p2.y + 1 < n && B[pos.p1.x][pos.p1.y + 1] == 0 && B[pos.p2.x][pos.p2.y + 1] == 0)
			return true;
	}

	return false;
}

void Swap(point& a, point& b)
{
	point tmp;
	tmp = a;
	a = b;
	b = tmp;
}

int solution(vector<vector<int>> board)
{
	int n = board.size();
	vector<vector<node>> visit(n, vector<node>(n));
	B = board;
	
	//이동을 저장하기 위한 공간
	queue<W> q;

	pointset pos;
	pos.p1.y = 1;
	W pushing;
	pushing.movecount = 0;
	pushing.pos = pos;

	q.push(pushing);
	int ret = 0;

	//다익스트라 알고리즘을 기반
	while (!q.empty())
	{
		pos = q.front().pos;
		ret = q.front().movecount;
		q.pop();

		//최초에 (n, n)에 도달한 movecount를 리턴한다.
		if (pos.p1.x == (n - 1) && pos.p1.y == (n - 1))
		{
			return ret;
		}

		int state = -1;

		//visit의 여부를 확인하여 이동 가능한 모든 구역을 조사
		//조사 후에 queue에 넣는다

		//가로로 있을 경우
		if (pos.p1.x == pos.p2.x)
		{
			if (visit[pos.p1.x][pos.p1.y].left && visit[pos.p2.x][pos.p2.y].right)
			{
				continue;
			}
			visit[pos.p1.x][pos.p1.y].left = true;
			visit[pos.p2.x][pos.p2.y].right = true;

			state = 0;
		}
		//세로로 있을 경우
		else
		{
			if (visit[pos.p1.x][pos.p1.y].top && visit[pos.p2.x][pos.p2.y].bottom)
			{
				continue;
			}
			visit[pos.p1.x][pos.p1.y].top = true;
			visit[pos.p2.x][pos.p2.y].bottom = true;

			state = 1;
		}

		//왼쪽, 오른쪽, 위쪽, 아래쪽 각각 이동이 가능하다면 이동한다.
		if (moveleft(pos, n))
		{
			pushing.pos = pos;
			pushing.pos.p1.y--;
			pushing.pos.p2.y--;
			pushing.movecount = ret + 1;
			q.push(pushing);
		}
		if (moveright(pos, n))
		{
			pushing.pos = pos;
			pushing.pos.p1.y++;
			pushing.pos.p2.y++;
			pushing.movecount = ret + 1;
			q.push(pushing);
		}
		if (moveup(pos, n))
		{
			pushing.pos = pos;
			pushing.pos.p1.x--;
			pushing.pos.p2.x--;
			pushing.movecount = ret + 1;
			q.push(pushing);
		}
		if (movedown(pos, n))
		{
			pushing.pos = pos;
			pushing.pos.p1.x++;
			pushing.pos.p2.x++;
			pushing.movecount = ret + 1;
			q.push(pushing);
		}

		if (rotate_p1_forward(pos, n, state))
		{
			if (state == 0)
			{
				pushing.pos = pos;
				pushing.pos.p2.x--;
				pushing.pos.p2.y++;
				pushing.movecount = ret + 1;
				q.push(pushing);
			}
			else
			{
				//p1, p2가 스왑되어 들어간다.
				//상대적으로 종료지점과 가까운 point를 p1으로 한다.
				pushing.pos = pos;
				pushing.pos.p2.x++;
				pushing.pos.p2.y++;
				Swap(pushing.pos.p1, pushing.pos.p2);
				pushing.movecount = ret + 1;
				q.push(pushing);
			}
		}

		if (rotate_p1_reverse(pos, n, state))
		{
			if (state == 0)
			{
				//p1, p2가 스왑되어 들어간다.
				//상대적으로 종료지점과 가까운 point를 p1으로 한다.
				pushing.pos = pos;
				pushing.pos.p2.x++;
				pushing.pos.p2.y++;
				Swap(pushing.pos.p1, pushing.pos.p2);
				pushing.movecount = ret + 1;
				q.push(pushing);
			}
			else
			{
				pushing.pos = pos;
				pushing.pos.p2.x++;
				pushing.pos.p2.y--;
				pushing.movecount = ret + 1;
				q.push(pushing);
			}
		}

		if (rotate_p2_forward(pos, n, state))
		{
			if (state == 0)
			{
				pushing.pos = pos;
				pushing.pos.p1.x++;
				pushing.pos.p1.y--;
				pushing.movecount = ret + 1;
				q.push(pushing);
			}
			else
			{
				//p1, p2가 스왑되어 들어간다.
				//상대적으로 종료지점과 가까운 point를 p1으로 한다.
				pushing.pos = pos;
				pushing.pos.p1.x--;
				pushing.pos.p1.y--;
				Swap(pushing.pos.p1, pushing.pos.p2);
				pushing.movecount = ret + 1;
				q.push(pushing);
			}
		}

		if (rotate_p2_reverse(pos, n, state))
		{
			if (state == 0)
			{
				//p1, p2가 스왑되어 들어간다.
				//상대적으로 종료지점과 가까운 point를 p1으로 한다.
				pushing.pos = pos;
				pushing.pos.p1.x--;
				pushing.pos.p1.y--;
				Swap(pushing.pos.p1, pushing.pos.p2);
				pushing.movecount = ret + 1;
				q.push(pushing);
			}
			else
			{
				pushing.pos = pos;
				pushing.pos.p1.x--;
				pushing.pos.p1.y++;
				pushing.movecount = ret + 1;
				q.push(pushing);
			}
		}
	}

	//도달하지 못하였을 경우 0을 리턴한다.
	return 0;
}
```