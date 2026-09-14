---
title: "[2018] 캐시"
date: "2019-11-22"
weight: 2
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/17680)

- 문제 설명
	1. 캐시 교체 알고리즘은 `LRU(Least Recently Used)` 를 사용한다.
	2. `cache hit` 일 경우 실행시간은 1이다.
	3. `cache miss` 일 경우 실행시간은 5이다.
	4. 총 실행 시간을 출력한다.
	
- 문제 풀이
	- 캐시 크기만큼의 배열을 생성한다.
	- 캐시가 비어있다면 데이터를 넣는다. 데이터를 넣을 때 사용된 시간을 함께 넣는다.
	- cache miss이고 비어 있는 캐시가 없다면 가장 오랫동안 사용되지 않은 데이터를 교체한다.
	- cache miss이고 비어 있는 캐시가 있다면 비어 있는 공간에 데이터와 사용된 시간을 넣는다.
	- cache hit가 일어나면 해당 cache의 사용된 시간을 갱신한다.

```cpp
#include <string>
#include <vector>

using namespace std;

static const int hit = 1;
static const int miss = 5;

struct node {
	int used_time = -1;
	string city = "";
};

//캐시에 들어있는지 판단
//cache hit인지 판단한다.
bool is_hit(vector<node>& c, string s, int time)
{
	for (int i = 0; i < c.size(); i++)
	{
		if (c[i].city == s)
		{
			c[i].used_time = time;
			return true;
		}
	}

	return false;
}

//캐시 교체
void cache_replace(vector<node>& c, string s, int time)
{
	int mintime = c[0].used_time;
	int idx = 0;

	//캐시에 들어있는 데이터 중에서 가장 오랫동안 사용되지 않은
	//데이터를 찾는다.
	for (int i = 1; i < c.size(); i++)
	{
		if (c[i].used_time < mintime)
		{
			mintime = c[i].used_time;
			idx = i;
		}
	}

	//교체
	c[idx].used_time = time;
	c[idx].city = s;
}

int solution(int cacheSize, vector<string> cities)
{
	int csize = cities.size();
	vector<node> cache(cacheSize);

	//모든 문자를 대문자로 변경
	for (int i = 0; i < csize; i++)
	{
		for (int j = 0; j < cities[i].length(); j++)
		{
			cities[i][j] = toupper(cities[i][j]);
		}
	}

	int cnt = 0;

	for (int i = 0; i < csize; i++)
	{
		if (is_hit(cache, cities[i], i))
		{
			cnt += hit;
		}
		else
		{
			if (cacheSize != 0)
			{
				cache_replace(cache, cities[i], i);
			}

			cnt += miss;
		}
	}

	return cnt;
}
```