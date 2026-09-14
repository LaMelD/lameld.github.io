---
title: "[2018] 뉴스 클러스터링"
date: "2019-11-24"
weight: 4
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/17677)

- 문제 설명
	1. 자카드 유사도
		- 집합 A = { 1, 2, 3 }, 집합 B = { 2, 3, 4 }
		- `A ∩ B` = { 2, 3 }
		- `A ∪ B` = { 1, 2, 3, 4 }
		- J(A, B) = 2 / 4
		- 자카드 유사도는 0.5이다.
	2. 중복을 허용하는 다중집합에서의 자카드 유사도
		- 집합 A = { 1, 1, 2, 2, 3 }, 집합 B = { 1, 2, 2, 4, 5 }
		- `A ∩ B` = { 1, 2, 2 }
		- `A ∪ B` = { 1, 1, 2, 2, 3, 4, 5 }
		- J(A, B) = 3 / 7
		- 자카드 유사도는 약 0.42이다.
	3. 자카드 유사도를 문자열 사이에 적용
		- 두 글자씩 끊어서 다중집합을 만든다.
		- FRANCE -> { FR, RA, AN, NC, CE }, FRENCH -> { FR, RE, EN, NC, CH }
		- 교집합 = { FR, NC }
		- 합집합 = { FR, NC, RA, AN, CE, RE, EN, CH }
		- J(FRANCE, FRENCH) =  2 / 8
		- 자카드 유사도는 0.25이다.
	4. 비교에 있어서 대문자와 소문자의 차이는 무시한다.
	5. 다중집합을 만듦에 있어서 두 문자가 영문자일 경우에만 집합에 포함한다.
		- "ab+"문자가 있으면 "ab"는 포함하고 "b+"는 포함하지 않는다.
	6. 유사도는 0에서 1사이의 실수이므로 65536을 곱한 후에 소수점 아래를 버린다.
	
- 문제 풀이
	- 두 문자열을 대문자로 바꾸고 다중집합을 만든다.
	- 교집합과 합집합을 만들어 자카드 유사도를 구한뒤 65536을 곱한뒤 리턴한다.
	
```cpp
#include <string>
#include <vector>
#include <queue>
#include <functional>

using namespace std;

int power(int a, int b)
{
	int ret = 1;

	for (int i = 0; i < b; i++)
	{
		ret *= a;
	}

	return ret;
}

bool is_alpha(char a)
{
	if ('a' <= a && a <= 'z')
	{
		return true;
	}

	if ('A' <= a && a <= 'Z')
	{
		return true;
	}

	return false;
}

int solution(string str1, string str2)
{
	priority_queue<int, vector<int>, greater<int>> s1;
	priority_queue<int, vector<int>, greater<int>> s2;
	//char : 8bit이므로
	const int cmp = power(2, 8);
	int tmp;

	for (int i = 0; i < str1.length() - 1; i++)
	{
		//둘다 영문자일 경우
		if (is_alpha(str1[i]) && is_alpha(str1[i + 1]))
		{
			//두 문자를 모두 대문자로 바꾸고
			str1[i] = toupper(str1[i]);
			str1[i + 1] = toupper(str1[i + 1]);
			//왼쪽 문자에 cmp(2^8)을 곱하고 뒷 문자를 더하여 정수형 int에 더한다.
			tmp = str1[i] * cmp + str1[i + 1];
			s1.push(tmp);
		}
		else
		{
			continue;
		}
	}

	for (int i = 0; i < str2.length() - 1; i++)
	{
		//둘다 영문자일 경우
		if (is_alpha(str2[i]) && is_alpha(str2[i + 1]))
		{
			//두 문자를 모두 대문자로 바꾸고
			str2[i] = toupper(str2[i]);
			str2[i + 1] = toupper(str2[i + 1]);
			//왼쪽 문자에 cmp(2^8)을 곱하고 뒷 문자를 더하여 정수형 int에 더한다.
			tmp = str2[i] * cmp + str2[i + 1];
			s2.push(tmp);
		}
		else
		{
			continue;
		}
	}

	int intersection = 0;
	int sum = s1.size() + s2.size();

	//두 다중집합의 크기가 0이라면 65536을 리턴
	if (sum == 0)
	{
		return 65536;
	}

	//교집합을 구한다.
	while (!s1.empty() && !s2.empty())
	{
		if (s1.top() == s2.top())
		{
			s1.pop();
			s2.pop();
			intersection++;
		}
		else if (s1.top() < s2.top())
		{
			s1.pop();
		}
		else
		{
			s2.pop();
		}
	}

	//합집합 = (두 집합의 크기의 합) - (교집합)
	sum = sum - intersection;

	int answer = intersection * 65536 / sum;

	return answer;
}
```