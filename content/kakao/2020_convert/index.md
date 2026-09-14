---
title: "[2020] 괄호 변환"
date: 2019-11-27T11:21:17+09:00
weight: 17
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/60058)

- 문제 설명
	- 균형잡힌 괄호 문자열 : '('의 개수와 ')'의 개수가 같다.
	- 올바른 문자열 : '('와 ')'의 괄호의 개수가 같고, 짝도 맞다.
	- 해당 문제에서 제시된 설명은 다음과 같다.

1. 입력이 빈 문자열인 경우, 빈 문자열을 반환합니다.
2. 문자열 w를 두 "균형잡힌 괄호 문자열" u, v로 분리합니다. 단, u는 "균형잡힌 괄호 문자열"로 더 이상 분리할 수 없어야 하며, v는 빈 문자열이 될 수 있습니다.
3. 문자열 u가 "올바른 괄호 문자열" 이라면 문자열 v에 대해 1단계부터 다시 수행합니다.
	1. 수행한 결과 문자열을 u에 이어 붙인 후 반환합니다. 
4. 문자열 u가 "올바른 괄호 문자열"이 아니라면 아래 과정을 수행합니다.
	1. 빈 문자열에 첫 번째 문자로 '('를 붙입니다.
	2. 문자열 v에 대해 1단계부터 재귀적으로 수행한 결과 문자열을 이어 붙입니다.
	3. ')'를 다시 붙입니다.
	4. u의 첫 번째와 마지막 문자를 제거하고, 나머지 문자열의 괄호 방향을 뒤집어서 뒤에 붙입니다.
	5. 생성된 문자열을 반환합니다.

- 문제 풀이 : 문제에 제시되어 있는 설명을 참고하여 순차적으로 적용하면 된다.

```cpp
#include <string>
#include <vector>

using namespace std;

//올바른 괄호인지 판별
bool is_right(string s)
{
	int cnt = 0;
	for (int i = 0; i < s.length(); i++)
	{
		if (s[i] == '(')
		{
			cnt++;
		}
		else if (s[i] == ')')
		{
			cnt--;
		}

		if (cnt < 0)
			return false;
	}

	if (cnt == 0)
		return true;

	return false;
}

//균형잡힌 문자열인지 판별
bool is_balanced(string s)
{
	int cnt = 0;
	for (int i = 0; i < s.length(); i++)
	{
		if (s[i] == '(')
		{
			cnt++;
		}
		else if (s[i] == ')')
		{
			cnt--;
		}
	}

	if (cnt == 0)
		return true;

	return false;
}

string solve(string s)
{
	if (s.length() == 0)
	{
		return "";
	}

	//문자열 s를 균형잡힌 문자열 u, v로 나눈다.
	string u = "", v;
	string ret = "";
	for (int i = 0; i < s.length(); i += 2)
	{
		v = s;
		u += s[i];
		u += s[i + 1];
		if (is_balanced(u))
		{
			v.erase(0, i + 2);
			if (is_balanced(v))
			{
				break;
			}
		}
	}

	//u, v가 모두 균형잡힌 상태에서
	//u가 올바른 문자열이라면
	if (is_right(u))
	{
		//ret에 u를 푸시하고 v를 검사한다.
		ret += u;
		ret += solve(v);
	}
	//u가 올바른 문자열이 아니라면
	else
	{
		//문제에 제시된 내용으로 해결한다
		ret += '(';
		ret += solve(v);
		ret += ')';

		int tmp = u.length();
		for (int i = 1; i < tmp - 1; i++)
		{
			if (u[i] == ')')
			{
				ret += '(';
			}
			else
			{
				ret += ')';
			}
		}
	}

	return ret;
}

string solution(string p)
{
	//올바른 괄호라면 원형 문자열을 리턴
	if (is_right(p))
	{
		return p;
	}

	//아니라면 괄호 변환을 실시하여 리턴한다.
	string ret = solve(p);

	return ret;
}
```