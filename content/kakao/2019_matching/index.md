---
title: "[2019] 매칭 점수"
date: 2019-11-24T18:15:23+09:00
weight: 11
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42893)

- 문제 설명
	1. 한 웹페이지에 대해서 기본점수, 외부 링크 수, 링크점수, 매칭점수를 구할 수 있다.
	2. 기본점수 : 텍스트 중에 검색어가 등장한 횟수(대소문자 무시)
	3. 외부 링크 수 : 다른 외부 페이지로 연결된 링크의 개수
	4. 링크점수 : ( 해당 웹페이지로 걸린 다른 웹페이지의 기본점수 ) / ( 외부링크 수) 의 합
	5. 매칭점수 : 기본점수 + 링크점수
	6. 매칭점수가 높은 웹페이지의 번호를 리턴한다.
	7. 매칭점수가 같은 웹페이지가 여러 개라면 그중 번호가 가장 작은 것을 구한다.

- 입력 예시 : `["<html lang=\"ko\" xml:lang=\"ko\" xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n  <meta charset=\"utf-8\">\n  <meta property=\"og:url\" content=\"https://careers.kakao.com/interview/list\"/>\n</head>  \n<body>\n<a href=\"https://programmers.co.kr/learn/courses/4673\"></a>#!MuziMuzi!)jayg07con&&\n\n</body>\n</html>", "<html lang=\"ko\" xml:lang=\"ko\" xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n  <meta charset=\"utf-8\">\n  <meta property=\"og:url\" content=\"https://www.kakaocorp.com\"/>\n</head>  \n<body>\ncon%\tmuzI92apeach&2<a href=\"https://hashcode.co.kr/tos\"></a>\n\n\t^\n</body>\n</html>"]`

![](/images/matching1.jpg)

- 검색어 : hi
	- A의 기본점수 : 1점
	- B의 기본점수 : 4점
	- C의 기본점수 : 9점
- 외부 링크 수
	- A : 1개
	- B : 2개
	- C : 3개
- 링크 점수
	- A : ( 4 / 2 ) + ( 9 / 3 ) = 5점
	- B : 0점
	- C : 0점
- 매칭점수
	- A : 6점
	- B : 4점
	- C : 9점

- 문제 풀이
	- 입력 형식을 파싱하여 각 웹페이지의 URL과 링크 URL, index, 기본점수, (기본점수 / 링크수)를 구한다.
	- 각 웹페이의 매칭점수를 완성하고, 매칭 점수를 기준으로하여 내림차순으로 정렬하여 매칭 점수가 가장 높은 웹페이지의 index를 리턴한다.
	- &&는 왼쪽 것이 false라면 오른쪽 조건을 확인하지 않는다는 것을 이용한다.
	
```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

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

// <, >기준으로 문자열을 split한다.
vector<string> split_by_Opne_Close(string input)
{
	vector<string> ret;

	string tmp = "";
	for (int i = 0; i < input.length(); i++)
	{
		if (input[i] == '<')
		{
			if (tmp != "\n" && tmp.length() != 0)
			{
				ret.push_back(tmp);
			}
			tmp = "<";
		}
		else if (input[i] == '>')
		{
			tmp += input[i];
			ret.push_back(tmp);
			tmp = "";
		}
		else
		{
			tmp += input[i];
		}
	}

	return ret;
}

//알파벳이 아닌 것을 기준으로 split
vector<string> split_not_alphabet(string input)
{
	vector<string> ret;

	string tmp = "";
	for (int i = 0; i < input.length(); i++)
	{
		if (is_alpha(input[i]))
		{
			tmp += tolower(input[i]);
		}
		else
		{
			if (tmp.length() != 0)
			{
				ret.push_back(tmp);
				tmp = "";
			}
		}
	}

	return ret;
}

class Page
{
private:
	int idx;	//몇번째 인풋인지
	double itlinkscore;	//현재 페이지의 (기본점수 / 링크수)
	double matchingscore;	//현재 페이지의 매칭 스코어
	string url;	//현재 페이지의 URL
	vector<string> link;	//링크 url정보
public:
	Page(int, string, string);
	string getUrl()
	{
		return this->url;
	}
	double getItLinkScore()
	{
		return this->itlinkscore;
	}
	void addMatchingScore(double itlinkscore)
	{
		this->matchingscore += itlinkscore;
	}
	double getMatchinScore()
	{
		return this->matchingscore;
	}
	int getIdx()
	{
		return this->idx;
	}
	vector<string> getLink()
	{
		return this->link;
	}
};
Page::Page(int idx, string word, string input)
{
	this->idx = idx;
	this->url = "";
	vector<string> info = split_by_Opne_Close(input);
	int i = 0;
	//url을 찾는다
	//<meta 를 찾는다 size가 27개 보다 많을 경우 체크한다
	while (1)
	{
		if (27 < info[i].length())
		{
			//시작이 <meta이고
			if (info[i][0] == '<' && info[i][1] == 'm' && info[i][2] == 'e' && info[i][3] == 't' && info[i][4] == 'a')
			{
				int j;
				//중간에 content="https://"가 있다면
				for (j = 5; j < info[i].length(); j++)
				{
					if (info[i][j] == 'c' && info[i][j + 1] == 'o' && info[i][j + 2] == 'n' && info[i][j + 3] == 't' && info[i][j + 4] == 'e' && info[i][j + 5] == 'n' && info[i][j + 6] == 't' && info[i][j + 7] == '=' && info[i][j + 8] == '\"' && info[i][j + 9] == 'h' && info[i][j + 10] == 't' && info[i][j + 11] == 't' && info[i][j + 12] == 'p' && info[i][j + 13] == 's' && info[i][j + 14] == ':' && info[i][j + 15] == '/' && info[i][j + 16] == '/')
					{
						for (int k = j + 17; info[i].length(); k++)
						{
							if (info[i][k] == '\"')
							{
								break;
							}
							this->url += info[i][k];
						}
						break;
					}
				}

				if (this->url.length() != 0)
				{
					break;
				}
			}
		}
		i++;
	}

	//외부로의 링크를 추가한다.
	for (i++; i < info.size(); i++)
	{
		//<a href="https:// 부터 "가 나올때까지의 url을 받아 외부링크를 완성한다.
		if (info[i][0] == '<' && info[i][1] == 'a' && info[i][2] == ' ' && info[i][3] == 'h' && info[i][4] == 'r' && info[i][5] == 'e' && info[i][6] == 'f' && info[i][7] == '=' && info[i][8] == '\"' && info[i][9] == 'h' && info[i][10] == 't' && info[i][11] == 't' && info[i][12] == 'p' && info[i][13] == 's' && info[i][14] == ':' && info[i][15] == '/' && info[i][16] == '/')
		{
			string tmp = "";
			for (int j = 17; j < info[i].length(); j++)
			{
				if (info[i][j] == '\"')
				{
					break;
				}
				tmp += info[i][j];
			}

			this->link.push_back(tmp);
		}
	}

	int numberoflink = this->link.size();

	info = split_not_alphabet(input);
	int score = 0;
	for (i = 0; i < info.size(); i++)
	{
		if (word == info[i])
		{
			score++;
		}
	}

	this->matchingscore = score;
	this->itlinkscore = (double)score / (double)numberoflink;
}

int solution(string word, vector<string> pages)
{
	vector<Page> parsing_pages;
	for (int i = 0; i < word.length(); i++)
	{
		word[i] = tolower(word[i]);
	}

	for (int i = 0; i < pages.size(); i++)
	{
		Page page(i, word, pages[i]);
		parsing_pages.push_back(page);
	}

	//매칭점수를 완성시킨다.
	for (int i = 0; i < pages.size(); i++)
	{
		vector<string> tmpLink = parsing_pages[i].getLink();
		for (int j = 0; j < tmpLink.size(); j++)
		{
			for (int k = 0; k < pages.size(); k++)
			{
				if (tmpLink[j] == parsing_pages[k].getUrl())
				{
					parsing_pages[k].addMatchingScore(parsing_pages[i].getItLinkScore());
				}
			}
		}
	}

	sort(parsing_pages.begin(), parsing_pages.end(), [](Page a, Page b)
		{
			if (a.getMatchinScore() == b.getMatchinScore())
			{
				return a.getIdx() < b.getIdx();
			}
			return b.getMatchinScore() < a.getMatchinScore();
		}
	);

	return parsing_pages.front().getIdx();
}
```