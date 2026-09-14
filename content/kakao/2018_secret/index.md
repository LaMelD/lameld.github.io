---
title: "[2018] 비밀지도"
date: "2019-11-23"
weight: 5
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/17681)

- 문제 설명
	1. 지도는 한 변의 길이가 `n` 인 정사각형 배열 형태로, 각 칸은 공백 또는 "#"으로 이루어져 있다.
	2. 전체 지도는 두 장의 지도를 겹쳐서 얻을 수 있다. 지도A, 지도B
	3. 두 지도는 각각 정수 배열로 암호화 되어 있다.
	4. 암호화된 배열은 지도의 각 가로줄에서 벽 부분을 1, 공백부분을 0으로 부호화했을 때 얻어지는 이진수에 해당한는 값의 배열이다.
	
![](/images/secret1.png)

- 문제 풀이
	- 입력 받은 정수를 각각 2진수로 변환한다.
	- 지도A와 지도B를 or연산을 통해서 전체 지도를 구한다.

```cpp
#include <string>
#include <vector>

using namespace std;

vector<string> solution(int n, vector<int> arr1, vector<int> arr2)
{
	vector<string> ret;

	for (int i = 0; i < n; i++)
	{
		int a, b;
		string s(n, ' ');
		//이진수를 만드는 동시에 전체 지도를 완성시킨다.
		for (int j = 0; j < n; j++)
		{
			a = arr1[i] % 2;
			b = arr2[i] % 2;
			
			//오른쪽으로 shift연산을 통해
			//2로 나눈 몫을 얻는다.
			arr1[i] = arr1[i] >> 1;
			arr2[i] = arr2[i] >> 1;

			if (a == 1 || b == 1)
			{
				s[n - 1 - j] = '#';
			}
		}
		
		ret.push_back(s);
	}

	return ret;
}
```