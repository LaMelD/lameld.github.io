---
title: "[2020] 자물쇠와 열쇠"
date: 2019-11-27T11:21:28+09:00
weight: 18
---

<h1>카카오 블라인드 테스트 2020 - 자물쇠와 열쇠</h1>

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/60059)

- 문제 설명
	1. 자물쇠의 크기 : M * M
	2. 열쇠의 크기 : N * N
	3. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠 홈 부분에 맞게 채우면 자물쇠가 열리는 구조이다.
	4. 열쇠를 자물쇠에 끼워 넣었을 때 빈 부분이 없어야 자물쇠를 열 수 있다.

![](/images/lock1.jpg)

- 문제 풀이
	- N, M의 범위가 매우 크지 않음으로 완전탐색을 진행한다.
	- (N + M + N - 2) x (N + M + N - 2) 의 2차원 배열 생성하여 중앙에 자물쇠를 배치한다.
	- 열쇠를 가능한 모든 위치에 대입하여 해당 열쇠로 자물쇠를 열 수 있는지 판단한다.
	
```cpp
#include <string>
#include <vector>
#include <iostream>

using namespace std;

bool is_open(vector<vector<int>> W, vector<vector<int>>& key, int w_i, int w_j, int N, int M)
{
	for (int i = 0; i < key.size(); i++) {
		for (int j = 0; j < key.size(); j++) {
			W[w_i + i][w_j + j] += key[i][j];
		}
	}

	for (int i = M - 1; i < M + N - 1; i++)
	{
		for (int j = M - 1; j < M + N - 1; j++)
		{
			if (W[i][j] == 1) continue;
			return false;
		}
	}
	return true;
}

void right_rot(vector<vector<int>>& k)
{
	vector<vector<int>> ret = k;
	for (int i = 0; i < k.size(); i++) {
		for (int j = 0; j < k.size(); j++) {
			ret[i][j] = k[j][k.size() - 1 - i];
		}
	}
	k = ret;
}

bool solution(vector<vector<int>> key, vector<vector<int>> lock)
{
	int N = lock.size();
	int M = key.size();
	int Wsize = 2 * M + N - 2;
	vector<vector<int>> W(Wsize, vector<int>(Wsize));
	for (int i = M - 1; i < M + N - 1; i++)
	{
		for (int j = M - 1; j < M + N - 1; j++)
		{
			W[i][j] = lock[i - M + 1][j - M + 1];
		}
	}

	for (int i = 0; i < 4; i++)
	{
		for (int j = 0; j < Wsize - M + 1; j++)
		{
			for (int k = 0; k < Wsize - M + 1; k++) {
				if (is_open(W, key, j, k, N, M))
					return true;
			}
		}
		right_rot(key);
	}

	return false;
}
```