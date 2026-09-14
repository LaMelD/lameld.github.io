---
title: "[2019] 오픈채팅방"
date: 2019-11-24T18:14:39+09:00
weight: 13
---

[문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42888)

- 문제 설명
	- 오픈채팅방에선 친구가 아닌 사람들과 대화가 가능하다.
	- 채칭방에 들어오거나 나갈 때 메시지가 출력된다.
	- 닉네임을 변경하는 방법은 2가지가 있다.
		1. 채팅방을 나간 후, 새로운 닉네임으로 다시 들어온다.
		2. 채팅방에서 닉네임을 변경한다.
	- 닉네임을 변경할 때는 기존에 채팅방에 출력되어 있던 메시지의 닉네임도 전부 변경된다.
	- 채팅방은 중복 닉네임을 허용한다.
	- 사용자는 유저아이디로 구분된다.
	
- 문제 풀이
	- 모든 사용자가 유저아이디로 구분되므로 식별자로 유저아이디를 사용하여 객체를 생성한다.
	- 커맨드는 Enter, Leave, Change로 구분된다.
		- Enter : 들어왔음을 알린다.
		- Leave : 나갔음을 알린다.
		- Change : 닉네임을 변경했음을 알린다.
	- input인 record를 파싱하여 데이터를 축적하여 result에 저장한다.
	
```cpp
#include <string>
#include <vector>
#include <sstream>

using namespace std;

struct user
{
	string uid;
	string name;
};

struct state
{
	bool rflag;
	int uid_idx;
};

int get_uid_idx(string uid, vector<user>& U)
{
	for (int i = 0; i < U.size(); i++)
	{
		bool flag = true;
		int j = 0;
		for (j = 0; j < uid.length(); j++)
		{
			if (U[i].uid[j] == NULL)
			{
				flag = false;
				break;
			}

			if (U[i].uid[j] != uid[j])
			{
				flag = false;
				break;
			}
		}
		if (flag && U[i].uid[j] == NULL && uid[j] == NULL)
		{
			return i;
		}
	}

	return -1;
}

vector<string> split_by_space(string s)
{
	vector<string> ret;

	stringstream ss(s);

	string str;
	while (ss >> str)
	{
		ret.push_back(str);
	}

	return ret;
}

//enter 0 leave 1
vector<string> solution(vector<string> record)
{
	//state 구조체
	//rflag : 0이면 들어왔다, 1이면 나왔다.
	//uid_idx : USER 배열에 들어있는 해당 uid의 위치 정보
	vector<state> STATE;
	//user 구조체
	//uid : 유저 식별자
	//name : 설정한 이름
	vector<user> USER;
	int i, j;

	for (i = 0; i < record.size(); i++)
	{
		j = 0;
		user uinput;
		state sinput;

		//공백을 기준으로 스플릿하여 파싱한 결과를 info 배열에 넣는다.
		vector<string> info = split_by_space(record[i]);

		//command에 따른 동작
		if (info[0] == "Enter")
		{
			sinput.rflag = 0;
			uinput.uid = info[1];
			uinput.name = info[2];

			//USER 배열에 해당 uid의 위치 정보를 받아 idx에 저장
			//USER 배열에 없다면 -1을 리턴한다.
			int idx = get_uid_idx(uinput.uid, USER);

			//USER 배열에 없다면
			if (idx == -1)
			{
				//USER 배열에 추가하고 uid_idx를 갱신한다.
				USER.push_back(uinput);
				sinput.uid_idx = USER.size() - 1;
			}
			//이미 존재한다면
			else
			{
				//해당 유저의 이름을 갱신하고 uid_idx를 갱신한다.
				USER[idx].name = uinput.name;
				sinput.uid_idx = idx;
			}

			//STATE에 추가해준다.
			STATE.push_back(sinput);
		}
		else if (info[0] == "Leave")
		{
			sinput.rflag = 1;
			//USER 배열에 해당 uid의 위치 정보를 받아 idx에 저장
			//USER 배열에 없다면 -1을 리턴한다.
			int idx = get_uid_idx(info[1], USER);

			//uid를 갱신한다.
			sinput.uid_idx = idx;

			//STATE에 추가해준다.
			STATE.push_back(sinput);
		}
		else if (info[0] == "Change")
		{
			uinput.uid = info[1];
			uinput.name = info[2];

			//USER 배열에 해당 uid의 위치 정보를 받아 idx에 저장
			//USER 배열에 없다면 -1을 리턴한다.
			int idx = get_uid_idx(uinput.uid, USER);

			//해당 유저의 이름을 갱신한다.
			USER[idx].name = uinput.name;
		}
	}

	vector<string> ret;

	//STATE에 들어있는 내용을 순차적으로 해석하여 ret 배열에 넣어준다.
	for (i = 0; i < STATE.size(); i++)
	{
		string rtmp = "";
		rtmp += USER[STATE[i].uid_idx].name;
		rtmp += "님이 ";
		if (STATE[i].rflag)
		{
			rtmp += "나갔습니다.";
		}
		else
		{
			rtmp += "들어왔습니다.";
		}
		ret.push_back(rtmp);
	}

	return ret;
}
```