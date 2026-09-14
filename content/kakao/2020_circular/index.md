---
title: "[2020] 외벽 점검"
date: 2019-11-27T11:22:33+09:00
weight: 16
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/60062)

- 문제 설명
	- 외벽이 원형으로 이루어진 레스토랑의 외벽을 점검한다.
	- 외벽의 총 둘레는 n미터이다.
	- 최소한의 인원을 투입하여 취약 지점을 점검한다.
	- 정북 방향 지점을 0으로 한다.
	- 인원들 각각이 1시간 동안 이동할 수 있는 거리는 다르다.
	- 최대로 투입 가능한 인원의 수는 8명이 한계이다.
	- 취약점의 최대 갯수는 15곳이다.
	- 모든 인원을 투입해도 취약점을 전부 점검할 수 없을 경우 -1을 리턴한다.
	
- 예시
	1. n = 12, weak = { 1, 5, 6, 10 }, dist = { 1, 2, 3, 4 }
		1. 4m를 이동할 수 있는 인원은 10m 지점에서 출발하여 시계방향으로 돌아 1m 위치에 있는 취약 지점에서 외벽 점검을 마친다.
		2. 2m를 이동할 수 있는 인원은 4.5m 지점에서 출발하여 6.5지점에서 외벽 점검을 마친다.
		3. 이 외의 여러 방법이 있지만, 두 명보다 적은 인원을 투입하는 방법은 없다. 따라서 친구를 최소 두 명 투입해야 한다.

	![](/images/circular1.jpg)

	2. n = 12, weak = { 1, 3, 4, 9, 10 }, dist = { 3, 5, 7 }  
		1. 7m를 이동할 수 있는 인원이 4m 지점에서 출발하여 반시계 방향으로 점검을 돌면 모든 취약 지점을 점검할 수 있다. 따라서 인원을 최소 한 명 투입하면 된다.

	![](/images/circular2.jpg)

- 문제 풀이
	1. dist 배열을 내림차순으로 정렬한다.(많은 지역을 커버할 수 있는 인원을 우선으로한다)
	2. 레스토랑이 원형으로 되어 있으므로 weak 배열을 초기 버전부터시작하여 가장 앞의 취약점에 n을 더하여 취약점의 위치를 재조정한다.
		- n = 5, weak = { 1, 3, 4 } : 초기 (1번재 반복)
		- weak = { 3, 4, 6 } : 2번째 반복
		- weak = { 4, 6, 8 } : 3번째 반복
	3. 선발 인원을 1명 부터 최대 인원까지 부여한다.
	4. 각각 선발된 인원으로 모든 취약점이 커버될 수 있는지 판단한다.
	5. 모든 취약점이 커버될 수 있다면 answer값을 갱신해준다.
	6. 모든 경우를 진행했지만 answer값에 변화가 없다면 -1을 리턴한다.
	
```cpp
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

int solution(int n, vector<int> weak, vector<int> dist)
{
	int answer = 9;

	//dist를 내림차순으로 정렬
	sort(dist.begin(), dist.end(), [](int a, int b) { return a > b; });

	int ret;
	//weak 초기부터 시작하여 가장 작은 값에 n을 더해가며 n회 진행한다.
	for (int i = 0; i < n; i++)
	{
		//각 weak 버전에서 최소 인원을 찾기위한 초기 세팅
		vector<int> selected;
		ret = 1;
		int dsize = dist.size();
		bool allcover = false;

		//모든 취약점이 커버되지 않았고
		//투입 가능한 인원을 초과하지 않았을 때 까지 계속한다.
		while (ret <= dsize && !allcover)
		{
			allcover = true;
			selected.clear();
			//인원 수 만큼 가장 많은 지역을 움직일 수 있는 인원을 추가한다.
			for (int i = 0; i < ret; i++)
			{
				selected.push_back(dist[i]);
			}

			//선발된 인원들이 어떤 순서로 취약지점을 커버했을 때
			//모든 취약지점이 커버될 수 있는지 또는 커버할 수 없는지 
			//순열 조합으로 알아낸다.
			do
			{
				allcover = true;
				int si = 0;
				int wi = 0;

				//해당 인원이 커버할 지점을 선택한다.
				int start = weak[wi];
				int end = start + selected[si];
				while (1)
				{
					//해당 인원이 커버할 수 있다면
					if (start <= weak[wi] && weak[wi] <= end)
					{
						wi++;
						//마지막 취약지점이라면
						if (weak.size() <= wi)
						{
							allcover = true;
							break;
						}
					}
					//해당 인원이 커버할 수 없다면
					else
					{
						si++;
						//선발된 인원들로 모든 취약지를 커버할 수 없다면
						if (ret <= si)
						{
							allcover = false;
							break;
						}

						start = weak[wi];
						end = start + selected[si];
					}
				}

				//모든 지역이 커버되었다면 answer를 갱신한다.
				if (allcover)
				{
					answer = answer < ret ? answer : ret;
				}
			} while (prev_permutation(selected.begin(), selected.end()));

			//인원을 늘린다.
			ret++;
		}

		//첫번째 지점에 n을 더하여 정렬후 다시 진행하여
		//최소값을 탐색한다.
		weak[0] += n;
		sort(weak.begin(), weak.end());
	}

	//초기 answer값과 변화가 없다면 -1을 리턴한다.
	if (answer == 9)
	{
		return -1;
	}

	return answer;
}
```