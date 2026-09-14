---
title: "[2018] 다트 게임"
date: "2019-11-21"
weight: 3
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/17682)

- 규칙
	1. 다트 게임은 총 3번의 기회로 구성된다.
	2. 각 기회마다 얻을 수 있는 점수는 0 ~ 10점까지이다.
	3. 점수와 함께  S(점수의 1승), D(점수의 2승), T(점수의 3승)가 주어진다.
	4. 옵션으로 *(해당 점수와 이전에 얻은 점수를 각각 2배로 한다), #(해당 점수를 마이너스한다)이 존재한다.
	5. *는 첫번째 기회에도 등장할 수 있으며, 이 때 해당점수만 2배로 한다.
	6. *, #은 둘 중 하나만 존재하거나 존재하지 않을 수 있다.
	7. S, D, T는 점수마다 하나씩 존재한다.
	
- 입력 형식 : 1S2D*3T(점수 | 보너스 | [ 옵션 ] )

- 풀이
	- 점수를 입력받고 S, D, T를 판별한다. 이후 보너스를 판별하여 현재의 점수를 수정하고 옵션을 확인하여 , 현재 기회에서 얻은 점수를 저장한다.
```cpp
#include <string>

using namespace std;

int solution(string dartResult)
{
	int nums[4];
	nums[0] = 0;

	int idx = 0;
	int i = 0;
	while (i < dartResult.length())
	{
		//숫자일 경우
		if ('0' <= dartResult[i] && dartResult[i] <= '9')
		{
			idx++;

			if (dartResult[i] == '1')
			{
				if (dartResult[i + 1] == '0')
				{
					nums[idx] = 10;
					i++;
				}
				else
				{
					nums[idx] = 1;
				}
			}
			else
			{
				nums[idx] = dartResult[i] - '0';
			}
		}
		else if (dartResult[i] == 'S' || dartResult[i] == 'D' || dartResult[i] == 'T')
		{
			if (dartResult[i] == 'D')
			{
				nums[idx] = nums[idx] * nums[idx];
			}
			else if (dartResult[i] == 'T')
			{
				nums[idx] = nums[idx] * nums[idx] * nums[idx];
			}
		}
		else if (dartResult[i] == '*' || dartResult[i] == '#')
		{
			if (dartResult[i] == '*')
			{
				nums[idx - 1] = nums[idx - 1] * 2;
				nums[idx] = nums[idx] * 2;
			}
			else
			{
				nums[idx] = nums[idx] * (-1);
			}
		}

		i++;
	}

	int answer = nums[1] + nums[2] + nums[3];
	return answer;
}
```