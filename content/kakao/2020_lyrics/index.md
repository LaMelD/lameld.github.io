---
title: "[2020] 가사 검색"
date: 2019-11-27T11:21:48+09:00
weight: 19
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/60060)

- 문제 설명
	- 노래 가사에 사용된 단어들 중에 특정 키워드가 몇 개 포함되어 있는지 알 수 있는 프로그램
	
	- 예시
		- "fro??"는 "frodo", "front", "frost"에 매치되므로 3입니다.
		- "????o"는 "frodo", "kakao"에 매치되므로 2입니다.
		- "fr???"는 "frodo", "front", "frost", "frame"에 매치되므로 4입니다.
		- "fro???"는 "frozen"에 매치되므로 1입니다.
		- "pro?"는 매치되는 가사 단어가 없으므로 0 입니다.
	
- 문제 풀이
	- 풀이 방법은 2가지가 있다.
		1. 완전탐색
		2. 트라이(Trie) 자료구조를 사용
	- 완전탐색으로 문제를 해결할 경우 문자열을 비교하는 단계에서 효율이 떨어진다. 이러한 문제를 트라이 자료구조를 사용하였을 때 해결할 수 있다.
	- 트라이 자료구조를 모른다면 효율성을 통과하기가 매우 어려웠다. 추후에 더 자세하게 추가하겠다.

```cpp
#include <algorithm>
#include <vector>
#include <string>

using namespace std;

//소문자로만 이루어져 있기 때문에 알파벳 개수만큼을 max로 한다.
const int Max_Trie_Children = 26;
//문자열의 길이가 최대 10000이기에 최대를 10001로 한다.
const int Max_Word_Size = 10001;

//문자를 인덱스로 변경
int alphatoindex(const char alpha)
{
	return alpha - 'a';
}

//와일드 카드 : '?' 판별
bool IsWildCard(const char alpha)
{
	return alpha == '?';
}

//노드가 되는 myTrie 객체의 구성
class myTrie
{
private :
	vector<myTrie*> child;
	//몇 개의 문자열이 겹치는지 알려주기 위한 변수
	int matchCnt;
	//문자의 끝을 알려주기 위한 변수
	bool Terminal;

private :
	void insert(const string&, const int);
	int find(const string&, const int);

public :
	myTrie();

	void insert(const string&);
	int find(const string&);
};
myTrie::myTrie()
{
	this->child = vector<myTrie*>(Max_Trie_Children);
	this->matchCnt = 0;
	this->Terminal = false;
}
void myTrie::insert(const string& word)
{
	this->insert(word, 0);
	return;
}
void myTrie::insert(const string& word, int idx)
{
	this->matchCnt++;

	if (idx == word.length())
	{
		this->Terminal = true;
		return;
	}

	char alpha = word[idx];
	int childIndex = alphatoindex(alpha);
	if (this->child[childIndex] == NULL)
	{
		this->child[childIndex] = new myTrie();
	}

	this->child[childIndex]->insert(word, idx + 1);

	return;
}
int myTrie::find(const string& target)
{
	return this->find(target, 0);
}
int myTrie::find(const string& target, int idx)
{
	if (idx == target.length())
	{
		if (this->Terminal)
		{
			return 1;
		}
		else
		{
			return 0;
		}
	}

	char alpha = target[idx];
	if (IsWildCard(alpha))
	{
		return this->matchCnt;
	}

	int childIndex = alphatoindex(alpha);
	if (this->child[childIndex] == NULL)
	{
		return 0;
	}

	return this->child[childIndex]->find(target, idx + 1);
}

//전체적인 구조를 담는 Lyrics 객체의 구조
class LyricS
{
private :
	vector<myTrie> trie;
	vector<myTrie> reversetrie;

public :
	LyricS();
	void insert(string);
	int find(string);
};
LyricS::LyricS()
{
	//와일드 카드가 queries에서 전방, 후방 배치가 가능하므로 정방향, 역방향을 만든다.
	this->trie = vector<myTrie>(Max_Word_Size);
	this->reversetrie = vector<myTrie>(Max_Word_Size);
}
void LyricS::insert(string word)
{
	//입력받은 문자열을 길이에 맞는 트라이 구조에 넣는다.
	this->trie[word.length()].insert(word);
	reverse(word.begin(), word.end());
	this->reversetrie[word.length()].insert(word);
}
int LyricS::find(string target)
{
	//길이에 맞는 트라이 구조에 들어가 문자열이 매치될 수 있는지 판단한다.

	//와일드 카드가 전방배치되었을 경우
	if (target[0] == '?')
	{
		reverse(target.begin(), target.end());
		return this->reversetrie[target.length()].find(target);
	}
	//와일드 카드가 후방배치되었을 경우
	else
	{
		return this->trie[target.length()].find(target);
	}
}

vector<int> solution(vector<string> words, vector<string> queries)
{
	vector<int> ans(queries.size(), 0);

	LyricS root;

	for (int i = 0; i < words.size(); i++)
	{
		root.insert(words[i]);
	}

	for (int i = 0; i < queries.size(); i++)
	{
		ans[i] = root.find(queries[i]);
	}

	return ans;
}
```