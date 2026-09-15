---
title: "telegram-alarm: Claude Code 작업이 끝나면 결과를 텔레그램으로 보내는 훅 플러그인"
date: 2026-09-15
weight: 3
tags: [project, shell, claude-code, telegram]
description: "bash와 curl만으로 만든 Claude Code 플러그인. 작업이 끝나면 Claude가 직접 쓴 결과를, 권한 승인을 기다릴 때는 알림을 텔레그램으로 보낸다."
---

저장소: [github.com/LaMelD/telegram-alarm](https://github.com/LaMelD/telegram-alarm)

Claude Code에 긴 작업을 시켜 놓고 자리를 비우면 두 가지가 궁금하다. 끝났는지, 그리고 권한 승인을 기다리며 멈춰 있는 건 아닌지. 이 플러그인은 그 둘을 텔레그램으로 알려 준다. 필요한 것은 bash와 curl뿐이다.

```text
[ak47][myapp] 빌드 스크립트 수정 완료. 테스트 12개 통과. 배포는 아직 안 함.
[ak47][myapp] 공공재: 비경합성·비배제성을 갖는 재화. 무임승차 문제로 시장 공급이 부족해 정부가 제공. 예: 국방, 등대.
[ak47][myapp] 🔔 Claude needs your permission to use Bash
```

메시지 앞의 `[server][project]` 프리픽스로 어느 서버의 어느 저장소에서 온 알림인지 구분한다. 서버 여러 대에서 Claude Code를 돌리면 이게 없을 때 알림이 섞인다.

## 요약은 Claude가 쓴다

가장 먼저 정한 것은 "본문을 누가 쓰느냐"다. 훅이 transcript를 잘라 보내는 방식은 마지막 몇 줄이 코드블록이거나 도구 출력이면 쓸모가 없다. 그래서 훅은 요약을 만들지 않는다. 대신 Claude에게 "작업이 끝나면 이 명령으로 결과를 보내라"고 지시하고, 보내지 않았으면 종료를 막는다.

| 시점 | 훅 | 하는 일 |
|---|---|---|
| 세션 시작 / 재개 / compact | SessionStart | 전송 지시문을 컨텍스트에 주입 |
| 프롬프트 입력 | UserPromptSubmit | 이번 턴의 전송 마커 삭제 |
| 응답 종료 | Stop | 이번 턴에 전송이 없었으면 `exit 2`로 종료를 막고 전송을 요구 |
| 권한 승인 대기 | Notification (`permission_prompt`) | Claude 개입 없이 스크립트가 바로 전송 |

네 훅이 전부 스크립트 하나의 서브커맨드다.

```text
telegram-alarm.sh send "내용"    Claude가 호출. 프리픽스를 붙여 전송하고 마커 생성
telegram-alarm.sh session-start  전송 지시문 출력
telegram-alarm.sh prompt         마커 삭제
telegram-alarm.sh stop           마커가 없으면 exit 2
telegram-alarm.sh notify         권한 대기 메시지 전송
telegram-alarm.sh prefix         현재 위치에서 붙을 프리픽스 출력
```

## 강제 장치: 마커 파일과 무한루프 방지

"이번 턴에 보냈는지"는 세션별 마커 파일 하나로 기억한다. 경로는 `$TMPDIR/telegram-alarm/<session_id>`다.

1. 사용자가 프롬프트를 입력하면(UserPromptSubmit) 마커를 지운다.
2. Claude가 `send`를 실행하면 마커를 만든다.
3. 응답이 끝날 때(Stop) 마커가 없으면 stderr에 이유를 쓰고 `exit 2`로 나간다. Claude Code는 이 이유를 Claude에게 돌려주고 응답을 이어 가게 한다.

Stop 훅이 막아서 Claude가 이어 가는 중에도 응답이 끝나면 Stop 훅은 다시 불린다. 그때 또 막으면 무한루프다. Claude Code는 이 경우 훅 입력 JSON에 `"stop_hook_active": true`를 넣어 주므로, 그 값이 보이면 무조건 통과시킨다. 한 번만 강제하고 두 번은 강제하지 않는다.

SessionStart의 matcher가 `startup|resume|clear|compact`인 것도 같은 맥락이다. compact로 컨텍스트가 요약되면 지시문이 사라질 수 있으니 그때 다시 넣는다.

## 프리픽스 규칙

| 항목 | 우선순위 |
|---|---|
| server | `~/.claude/server-id` 파일 내용 → 없으면 `hostname -s` |
| project | `./.claude-project-id` 파일 내용 → git 루트 디렉토리명 → HOME이면 `global` → 현재 디렉토리명 |

hostname이 `ip-172-31-...` 같은 값인 서버는 `echo ak47 > ~/.claude/server-id`로 별칭을 준다. 저장소 이름 대신 다른 이름을 쓰고 싶으면 프로젝트 루트에 `.claude-project-id`를 둔다. 현재 위치에서 어떤 프리픽스가 붙는지는 `prefix` 서브커맨드로 확인한다.

## 설정이 없으면 조용히 통과

`TELEGRAM_BOT_TOKEN`이나 `TELEGRAM_CHAT_ID`가 없으면 모든 훅이 `exit 0`으로 빠진다. 플러그인을 여러 서버에 깔아 두고 일부에만 토큰을 넣어도 나머지 서버의 Claude Code가 멈추지 않는다. 단 `send`는 설정이 없으면 에러를 낸다. 보내지도 않았는데 마커가 생기면 안 되기 때문이다.

텔레그램 메시지 상한은 4096자라 `send`에서 본문을 그 길이로 자른다.

## 설치

봇은 [@BotFather](https://t.me/BotFather)에게 `/newbot`으로 한 번만 만든다. 만든 봇에게 아무 메시지나 보낸 뒤 chat_id를 확인한다.

```sh
curl -s "https://api.telegram.org/bot<TOKEN>/getUpdates" | grep -o '"chat":{"id":[0-9-]*' | head -1
```

서버마다 셸 프로파일이나 `~/.claude/settings.json`의 `env` 블록에 두 값을 넣는다.

```sh
export TELEGRAM_BOT_TOKEN="123456:ABC..."
export TELEGRAM_CHAT_ID="987654321"
```

플러그인 설치:

```sh
claude plugin marketplace add LaMelD/telegram-alarm
claude plugin install telegram-alarm@telegram-alarm
```

전송 확인은 `bash scripts/telegram-alarm.sh send "테스트"`. 로컬에서 개발 중이면 `claude --plugin-dir /path/to/telegram-alarm`으로 띄운다.

## 테스트

네트워크 없이 훅의 분기만 검사한다. `TMPDIR`과 `HOME`을 임시 디렉토리로 바꾸고 가짜 토큰을 넣은 뒤, Stop이 막는지, 마커 뒤에 통과하는지, `stop_hook_active`에서 통과하는지, 설정이 없을 때 조용한지, 프리픽스 덮어쓰기가 먹는지 등 12개를 확인한다.

```sh
bash tests/test_hooks.sh
```

## 만들면서 배운 것

1. 처음 지시문은 "결과 요약을 한국어 1~5줄로 보내라"였다. 그러자 "질문에 답변함", "수정 완료함" 같은 메타 서술이 왔다. 텔레그램만 보고는 무슨 답이었는지 알 수 없다. 0.1.1에서 지시문을 바꿨다. 메타 서술을 금지하고, 질문에 답한 경우는 답의 핵심을 3~5줄로, 작업한 경우는 무엇을 바꿨고 결과가 무엇인지를 쓰게 했다. 위 예시의 "공공재" 메시지가 그 결과다. 테스트에도 지시문에 "메타 서술 금지"가 들어 있는지 확인하는 항목을 넣었다.
2. 훅으로 Claude에게 무언가를 강제하려면 "지시"만으로는 부족하고 "검증"이 있어야 한다. SessionStart로 지시하고 Stop으로 검증하는 한 쌍이 핵심이다. 검증이 없으면 지시문은 컨텍스트가 길어질수록 잊힌다.
3. Stop 훅으로 종료를 막을 때는 `stop_hook_active`를 반드시 봐야 한다. 이어 가는 중에 다시 막으면 끝나지 않는다.
