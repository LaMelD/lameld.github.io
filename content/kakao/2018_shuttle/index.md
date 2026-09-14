---
title: "[2018] 셔틀버스"
date: "2019-11-22"
weight: 6
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/17678)

- 문제 설명
	1. 셔틀은 `09:00` 부터 총 `n` 회 `t` 분 간격으로 역에 도착하며, 하나의 셔틀에는 최대 `m` 명의 승객이 탈 수 있다.
	2. 셔틀은 도착했을 때 도착한 순간 대기열에 선 크루까지 포함해서 대기 순서대로 태우고 바로 출발한다. 예를 들어 `09:00` 에 도착한 셔틀은 자리가 있다면 `09:00` 에 줄을 선 크루도 탈 수 있다.
	3. 콘(셔틀을 탈 승객)은 게으르기 때문에 같은 시각에 도착한 크루 중 대길에서 제일 뒤에 선다.
	4. 콘이 셔틀을 타고 사무실로 갈 수 있는 제일 늦은 시각을 출력한다.

- 풀이
	- timetable : 사람들이 도착하는 시간을 오름차순으로 정렬한다.
	- shuttleTime : 셔틀버스가 도착하는 시간을 담고 있는 배열
	- shuttle : 사람이 셔틀버스에 탄 현황을 기록하기 위한 배열
	- timetable을 처음부터 돌면서 탈 수 있는 버스가 있는지 확인하고 탈 수 있는 버스가 있다면 shuttle에 기록해준다.
	- 마지막 버스가 만원이라면 가장 늦게온 사람보다 1분 먼저 도착하면 된다.
	- 마지막 버스가 만원이 아니라면 버스 도착시간에 정류장에 도착하면 된다.

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int tTom(string s)
{
	int ret;

	int H, M;
	H = s[0] - '0';
	H = H * 10 + (s[1] - '0');
	M = s[3] - '0';
	M = M * 10 + (s[4] - '0');

	ret = H * 60 + M;

	return ret;
}

//첫 셔틀은 9시 정각에 있다
string solution(int n, int t, int m, vector<string> timetable)
{
	string ret = "";
	vector<vector<int>> shuttle(n);
	vector<int> shuttleTime = { 540 };
	for (int i = 1; i < n; i++)
	{
		shuttleTime.push_back(shuttleTime.back() + t);
	}

	sort(timetable.begin(), timetable.end());

	int arrive;

	for (int i = 0; i < timetable.size(); i++)
	{
		arrive = tTom(timetable[i]);

		for (int j = 0; j < n; j++)
		{
			if (arrive <= shuttleTime[j] && shuttle[j].size() < m)
			{
				shuttle[j].push_back(arrive);
				break;
			}
		}
	}

	int result = 0;

	if (shuttle.back().size() == m)
	{
		result = shuttle.back().back() - 1;
	}
	else
	{
		result = shuttleTime.back();
	}

	int H = result / 60;
	int M = result % 60;
	if (H < 10)
	{
		ret += '0';
	}
	ret += to_string(H);
	ret += ':';
	if (M < 10)
	{
		ret += '0';
	}
	ret += to_string(M);

	return ret;
}
```

