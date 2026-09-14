---
title: "[2019] 무지의 먹방 라이브"
date: 2019-11-24T18:15:07+09:00
weight: 12
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42891)

- 문제 설명
	1. 회전판에 먹어야 할 N개의 음식이 있다.
	2. 무지는 1번 음식부터 먹기 시작하며, 회전판은 번호가 증가하는 순서대로 음식을 무지 앞으로 가져다 놓는다.
	3. 무지는 음식 하나를 1초 동안 섭취한 후 남은 음식은 그대로 두고, 다음 음식을 섭취한다.
	4. 무지가 먹방을 시작한 지 K초 후에 네트워크 장애로 인해 방송이 잠시 중단되었다가 정상화 후 다시 방송을 이어갈 때, 몇 번 음식부터 섭취해야하는지 알고자 한다.
	5. 더 이상 섭취할 음식이 없다면 `-1` 을 반환한다.

- 문제 풀이
	- K초까지 어떤 음식을 먹었는지 계산하여 다음 음식의 번호를 리턴한다.

```cpp
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

struct node
{
	int times = 0;
	int idx = 0;
};

int solution(vector<int> food_times, long long k)
{
	int n = food_times.size();

	//data parsing
	//음식 번호와 음식 양을 node에 넣어 
	//ACS 배열을 완성시킨다.
	vector<node> ACS;
	for (int i = 0; i < n; i++)
	{
		node input;
		input.idx = i;
		input.times = food_times[i];
		ACS.push_back(input);
	}

	//ACS 배열을 오름차순으로 정렬
	sort(ACS.begin(), ACS.end(), [](node a, node b)
		{
			if (a.times == b.times)
			{
				return a.idx < b.idx;
			}
			return a.times < b.times;
		}
	);

	int ACS_idx = 0;
	//이전에 섭취한 음식량
	int before_value = 0;
	while (1)
	{
		long long tmpk = k;
		//반복 횟수
		long long m_time = (long long)ACS[ACS_idx].times - (long long)before_value;
		//섭취할 수 있는 음식의 수
		long long m_numberoffood = (long long)n - (long long)ACS_idx;
		
		tmpk -= m_time * m_numberoffood;

		//계산 결과가 음수면 break
		if (tmpk < 0)
		{
			break;
		}
		//계산 결과가 0이면 k를 0으로 성정하고 break
		else if (tmpk == 0)
		{
			k = 0;
			break;
		}

		k = tmpk;
		//섭취한 음식량을 갱신해준다.
		before_value = ACS[ACS_idx++].times;

		//ACS의 모든 인덱스를 돌았다면 다음에 먹을 음식이 없다고 판단되어
		//-1을 리턴한다.
		if (ACS_idx == n)
		{
			return -1;
		}
	}

	//while문을 빠져나왔을 때
	//k가 0이라면
	if (k == 0)
	{
		//마지막 음식량보다 초기 음식량이 큰 음식의 번호를 리턴한다.
		for (int i = 0; i < n; i++)
		{
			if (ACS[ACS_idx].times < food_times[i])
			{
				return i + 1;
			}
		}

		//for문에서 return 하지 못했다면 return -1 한다.
		return -1;
	}
	//나머지 경우
	else
	{
		//마지막 음식량을 섭취하기 전에 K(네트워크 오류)가 발생하였기 때문에
		//몇번째 반복에 음식량 초과가 발생했는지 알아낸다.
		int i = 0;
		while (1)
		{
			long long tmpk = k;
			long long tmp1 = (long long)n - (long long)ACS_idx;
			tmpk -= (long long)i * tmp1;
			if (tmpk < 0)
			{
				i--;
				k = k - tmp1 * i;
				break;
			}
			else if (tmpk == 0)
			{
				k = 0;
				break;
			}

			i++;
		}

		if (k == 0)
		{
			for (i = 0; i < n; i++)
			{
				if (before_value < food_times[i])
				{
					return i + 1;
				}
			}

			return -1;
		}
		else
		{
			i = 0;
			while (1)
			{
				if (before_value < food_times[i])
				{
					k--;
					if (k == -1)
					{
						return i + 1;
					}
				}
				i++;
				i = i % n;
			}
		}
	}

	return 0;
}
```