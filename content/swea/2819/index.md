---
title: "[D4] 2819"
date: 2019-11-29T17:38:30+09:00
weight: 2
---

# 격자판의 숫자 이어 붙이기

[문제 출처](https://swexpertacademy.com/main/code/problem/problemDetail.do)

- 문제 설명
	1. 4 x 4 크기의 격자판이 있다. 격자판의 각 격자칸에는 0부터 9사이의 숫자가 적혀 있다.
	2. 격자판의 임의의 위치에서 시작한다.
	3. 동서남북 네 방향으로 인접한 격자로 총 6회 이동하여 각 칸에 숫자를 차례로 이어 붙인다.
	4. 이동을 할 때 중복을 허용한다.
	5. 0으로 시작하는 수도 만들 수 있다.
	6. 가능한 7자리 숫자의 개수를 구한다.
	
- 문제 풀이
	- 완전탐색을 통해서 가능한 경우의 수를 모두 구한다.
	- 가능한 모든 경우의 수에서 중복을 제거한다.

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

void solve(vector<vector<int>>& map, int i, int j, int cnt, string ret, vector<string>& s)
{
	//격자판을 나간다면 더 이상 실행하지 않는다.
	if (i < 0 || 3 < i || j < 0 || 3 < j)
	{
		return;
	}

	char tmp = '0' + map[i][j];
	ret.push_back(tmp);

	if (cnt >= 6)
	{
		s.push_back(ret);
		return;
	}

	//동
	solve(map, i, j + 1, cnt + 1, ret, s);
	//서
	solve(map, i, j - 1, cnt + 1, ret, s);
	//남
	solve(map, i + 1, j, cnt + 1, ret, s);
	//북
	solve(map, i - 1, j, cnt + 1, ret, s);
}

int main()
{
	int T;
	cin >> T;

	for (int tc = 1; tc <= T; tc++)
	{
		vector<vector<int>> map(4, vector<int>(4, 0));
		for (int i = 0; i < 4; i++)
		{
			for (int j = 0; j < 4; j++)
			{
				cin >> map[i][j];
			}
		}

		vector<string> store;
		for (int i = 0; i < 4; i++)
		{
			for (int j = 0; j < 4; j++)
			{
				solve(map, i, j, 0, "", store);
			}
		}

		sort(store.begin(), store.end());
		store.erase(unique(store.begin(), store.end()), store.end());

		cout << '#' << tc << ' ';
		cout << store.size() << '\n';
	}

	return 0;
}
```