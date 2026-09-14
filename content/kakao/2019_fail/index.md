---
title: "[2019] 실패율"
date: "2019-11-24"
weight: 9
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42889)

- 문제 설명
	1. 실패율 = A / B
		- A : 스테이지에 도달했으나 클리어하지 못한 플레이어 수
		- B : 스테이지에 도달한 플레이어 수 
	2. B가 0인 경우 실패율은 0으로 정의한다.
	3. 실패율이 높은 스테이지부터 내림차순으로 정렬된 배열을 리턴한다.
	
- 문제 풀이
	- 모든 스테이지 A, B를 확정시킨다.
	- 실패율을 비교하여 정렬을 진행한 뒤 스테이지 number만 리턴한다.
	
```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

struct node
{
	int stage;
	int playing;
	int arrive;
	bool iszero;
};

vector<int> solution(int N, vector<int> stages)
{
	vector<node> ret;

	for (int i = 1; i <= N; i++)
	{
		int arrive = 0;
		int playing = 0;
		node input;
		input.stage = i;
		for (int j = 0; j < stages.size(); j++)
		{
			if (i <= stages[j])
			{
				if (i == stages[j])
				{
					playing++;
				}
				arrive++;
			}
		}

		if (arrive == 0)
		{
			input.iszero = 1;
			ret.push_back(input);
		}
		else
		{
			input.iszero = 0;
			input.arrive = arrive;
			input.playing = playing;
			ret.push_back(input);
		}
	}

	sort(ret.begin(), ret.end(), [](node a, node b)
		{
			//소트 조건에서 비교한다
			if (a.iszero && b.iszero)
			{
				return a.stage < b.stage;
			}

			if (a.iszero)
			{
				return false;
			}

			if (b.iszero)
			{
				return true;
			}

			long long aa, bb;
			aa = (long long)a.playing * (long long)b.arrive;
			bb = (long long)b.playing * (long long)a.arrive;
			if (aa == bb)
			{
				return a.stage < b.stage;
			}
			return aa > bb;
		}
	);

	vector<int> answer;
	for (int i = 0; i < N; i++)
	{
		answer.push_back(ret[i].stage);
	}

	return answer;
}
```