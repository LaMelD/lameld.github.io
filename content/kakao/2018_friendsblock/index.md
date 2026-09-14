---
title: "[2018] 프렌즈 4 블록"
date: "2019-11-21"
weight: 4
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/17679)

- 같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.

![](/images/friend4block1.png)

![](/images/friend4block2.png)

![](/images/friend4block3.png)

![](/images/friend4block4.png)

- 풀이
	1. 왼쪽 상단부터 오른쪽 하단까지 지울 수 2x2 형태로 4개가 붙어있는 경우를 탐색한다.
	2. 2x2 형태로 4개가 붙어있는 경우가 존재한다면 해당 블록들을 비워주고, 비워진 부분을 위쪽에 블록을 내려 채워주고 1번부터 다시 실행한다. 2x2형태로 4개가 붙어 있는 경우가 없다면 다음 단계로 넘어간다.
	3. 더 이상 비워줄 블록이 없는 경우에는 비어있는 블록의 갯수를 리턴한다.

```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

bool is_square_same(vector<string>& w, int i, int j)
{
	if (w[i][j] == w[i][j + 1] && w[i][j] == w[i + 1][j] && w[i][j] == w[i + 1][j + 1])
	{
		return true;
	}

	return false;
}

void flush_block(vector<string>& w, int x, int y)
{
	w[x][y] = ' ';
	w[x + 1][y] = ' ';
	w[x][y + 1] = ' ';
	w[x + 1][y + 1] = ' ';
}

int solution(int m, int n, vector<string> board)
{
	queue<pair<int, int>> q;
	while (1)
	{
		for (int i = 0; i < m - 1; i++)
		{
			for (int j = 0; j < n - 1; j++)
			{
				if (board[i][j] == ' ')
				{
					continue;
				}

				if (is_square_same(board, i, j))
				{
					q.push(make_pair(i, j));
				}
			}
		}

		//비울 수 있는 블록이 없는 경우
		if (q.empty())
		{
			break;
		}

		while (!q.empty())
		{
			int x = q.front().first;
			int y = q.front().second;
			q.pop();
			flush_block(board, x, y);
		}

		//비어있는 공간을 채운다
		for (int j = 0; j < n; j++)
		{
			int cnt = 0;
			for (int i = m - 1; i >= 0; i--)
			{
				if (board[i][j] != ' ' && cnt == 0)
				{
					continue;
				}
				else if (board[i][j] != ' ' && cnt != 0)
				{
					board[i + cnt][j] = board[i][j];
					board[i][j] = ' ';
				}
				else
				{
					cnt++;
				}
			}
		}
	}
	int ret = 0;

	for (int i = 0; i < m; i++)
	{
		for (int j = 0; j < n; j++)
		{
			if (board[i][j] == ' ')
			{
				ret++;
			}
		}
	}

	return ret;
}
```

