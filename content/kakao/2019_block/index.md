---
title: "[2019] 블록 게임"
date: "2019-11-24"
weight: 8
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42894)

- 문제 설명
	1. 아래와 같이 12가지 모양의 블록이 배치되어 있다.

	![](/images/block-game1.png)

	2. 1x1 크기의 검은 블록을 떨어뜨려 기존에 놓인 블록을 합해 속이 꽉 채워진 직사각형을 만들어 그 블록을 없앨 수 있다.

	![](/images/block-game2.png)

	![](/images/block-game3.png)

	3. 검은 블록을 떨어뜨려 없앨 수 있는 블록 개수의 최댓값을 구한다.
	
- 문제 풀이 
	1. 위에서 검은 블록을 떨어뜨려 제거할 수 있는 블록을 판단한다.
	
	![](/images/block-game4.png)
	
	2. 현재 선택된 블록의 위쪽에 방해물이 있는지 판단하여 방해물이 없을 경우 블록을 지우고, 방해물이 있을 경우 continue한다.
	3. 제거할 블록이 없을 때 까지 반복하여 완료한다.
	
```cpp
#include <string>
#include <vector>

using namespace std;

//0 :: 못 지우는 모양
//1 :: 철 모양
//2 :: 세로 ㄴ
//3 :: 세로 역ㄴ
//4 :: 가로 ㄴ
//5 :: 가로 역ㄴ
int getBlockShape(vector<vector<int>>& board, int x, int y)
{
	int block_num = board[x][y];

	if (board[x][y + 1] == block_num && board[x - 1][y + 1] == block_num &&
		board[x][y + 2] == block_num)
	{
		return 1;
	}

	if (board[x][y - 1] == block_num && board[x - 1][y - 1] == block_num &&
		board[x - 2][y - 1] == block_num)
	{
		return 2;
	}

	if (board[x][y + 1] == block_num && board[x - 1][y + 1] == block_num &&
		board[x - 2][y + 1] == block_num)
	{
		return 3;
	}

	if (board[x][y - 1] == block_num && board[x - 1][y - 1] == block_num &&
		board[x][y + 1] == block_num)
	{
		return 4;
	}

	if (board[x][y + 1] == block_num && board[x][y + 2] == block_num &&
		board[x - 1][y + 2] == block_num)
	{
		return 5;
	}

	return 0;
}

//확인하는 블록을 지울 수 있는지 확인한다
bool canDeleteBlock(vector<vector<int>>& board, int x, int y, int shape)
{
	if (shape == 0)
	{
		return false;
	}

	switch (shape)
	{
	case 1:
		for (int i = x - 1; i > 0; --i)
		{
			if (board[i][y + 2] != 0)
			{
				return false;
			}
		}
		return true;
	case 2:
		return true;
	case 3:
		return true;
	case 4:
		for (int i = x - 1; i > 0; --i)
		{
			if (board[i][y + 1] != 0)
			{
				return false;
			}
		}
		return true;
	case 5:
		for (int i = x - 1; i > 0; --i)
		{
			if (board[i][y + 1] != 0)
			{
				return false;
			}
		}
		return true;
	}
}

void DeleteBlock(vector<vector<int>>& board, int x, int y, int shape)
{
	board[x][y] = 0;

	switch (shape)
	{
	case 1:
		board[x][y + 1] = 0;
		board[x - 1][y + 1] = 0;
		board[x][y + 2] = 0;
		return;
	case 2:
		board[x][y - 1] = 0;
		board[x - 1][y - 1] = 0;
		board[x - 2][y - 1] = 0;
		return;
	case 3:
		board[x][y + 1] = 0;
		board[x - 1][y + 1] = 0;
		board[x - 2][y + 1] = 0;
		return;
	case 4://
		board[x][y - 1] = 0;
		board[x][y + 1] = 0;
		board[x - 1][y - 1] = 0;
		return;
	case 5:
		board[x][y + 1] = 0;
		board[x][y + 2] = 0;
		board[x - 1][y + 2] = 0;
		return;
	}
}

int solution(vector<vector<int>> board)
{
	int ret = 0;
	int n = board.size();
	//-1로 둘러싸인 board을 생성한다
	vector<vector<int>> nboard(n + 2, vector<int>(n + 2, -1));
	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= n; j++)
		{
			nboard[i][j] = board[i - 1][j - 1];
		}
	}

	bool is_change;
	do
	{
		is_change = false;

		int x, y;

		for (y = 1; y <= n; y++)
		{
			for (x = 1; x <= n; x++)
			{
				int shape;
				if (nboard[x][y] != 0)
				{
					shape = getBlockShape(nboard, x, y);
					if (canDeleteBlock(nboard, x, y, shape))
					{
						ret++;
						is_change = true;
						DeleteBlock(nboard, x, y, shape);
						y = 0;
					}
					break;
				}
			}
		}

	} while (is_change);

	return ret;
}
```