---
title: "Claude Code 11월 업데이트 정리: Fable 5.2, 클라우드 환경, /storage, 쉬는 동안 자동 압축"
date: 2026-11-11
weight: 10
tags: [claude-code, fable, cloud-sessions, release-notes]
description: "Claude Code 팀의 11월 뉴스레터를 공개 변경 이력·공식 문서와 대조해 정리했다. Fable 5.2 전환과 체험 조건, Claude가 대신 맞추는 클라우드 환경, 데스크톱 /storage, /usage의 f 키, 쉬는 동안 자동 압축, ultrathink, /auto-mode-setup을 버전별로 묶었다."
---

> 이 글은 2026년 11월 Claude Code 팀 뉴스레터(이메일)를 바탕으로, Claude Code [변경 이력](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)과 공식 문서에서 확인한 내용을 덧붙인 **정리 글**입니다. 공개 자료에서 찾지 못한 내용은 **뉴스레터 기준**이라고 표시했습니다. 버전 번호는 변경 이력 기준이고, 2026-11-11에 확인했습니다. 블로그의 AI 브리핑에서 이미 다룬 항목은 해당 브리핑으로 연결했습니다.

## 한눈에 보기

| 항목 | 요점 | 버전 | 블로그에서 다룬 곳 |
|---|---|---|---|
| Fable 5.2 | Claude Code의 기본 Fable 모델이 됐다 | 2.1.321 | [11-11 브리핑](/ai-briefing/2026-11-11/)(모델 발표), 이 글(Claude Code에서 쓰는 법) |
| 클라우드 환경 자동 설정 | 빠진 의존성·네트워크·API 키를 Claude가 찾아 승인을 받는다 | 뉴스레터 기준 | 이 글 |
| 저장소 없는 클라우드 세션 | No repository 옵션 | 2.1.318 | 이 글 |
| 설정 스크립트가 실패해도 시작 | 세션이 멈추지 않고 열린다 | 2.1.314 | 이 글 |
| 클라우드 루틴 effort | 루틴의 모델 선택기에 effort가 생겼다 | 2.1.312 | 이 글 |
| `/usage`의 `f` | 경고마다 한 줄 수정안, `f`로 적용 | 2.1.305 | [10-22 브리핑](/ai-briefing/2026-10-22/) |
| 쉬는 동안 자동 압축 | 프롬프트 캐시가 만료되기 전에 요약한다 | 2.1.299, 2.1.302, 2.1.314 | 이 글 |
| 새 메시지만 보내기 | message threads가 대부분 사용자에게 기본으로 켜졌다 | API 베타 | [11-04 브리핑](/ai-briefing/2026-11-04/) |
| 모델 전환 전 요약 | 새 모델에 너무 긴 대화는 이전 모델이 먼저 요약한다 | 2.1.301 | 이 글 |
| 백그라운드 서브에이전트 사용량 | 한도를 약 5% 덜 쓴다(자사 측정) | 뉴스레터 기준 | 이 글 |
| `/storage` | 데스크톱에서 오래된 워크트리를 정리한다 | 뉴스레터 기준 | 이 글 |
| 대기 메시지 복구 | 데스크톱을 다시 열면 대기 메시지가 돌아온다 | 뉴스레터 기준 | 이 글 |
| fork 서브에이전트 기본 켜짐 | 데스크톱과 VS Code까지 넓어졌다 | 2.1.302 | [10-18 브리핑](/ai-briefing/2026-10-18/) |
| `ultrathink` | 그 턴만 xhigh effort로 돈다 | 2.1.311, 2.1.316 | [10-28 브리핑](/ai-briefing/2026-10-28/), [11-02 브리핑](/ai-briefing/2026-11-02/) |
| `/auto-mode-setup` | auto mode에 믿을 저장소·도메인을 알려 준다 | 2.1.304 | [10-21 브리핑](/ai-briefing/2026-10-21/) |
| 작업 폴더 밖 쓰기 차단 | `permissions.blockWritesOutsideWorkingDirectories` | 2.1.314 | [10-31 브리핑](/ai-briefing/2026-10-31/) |
| IDE 확장 자동 설치 중단 | IDE 터미널에서 실행해도 설치하지 않는다 | 2.1.318 | [11-05 브리핑](/ai-briefing/2026-11-05/) |
| Claude Code 팀의 코드 리뷰 글 | 사람보다 Claude가 먼저 리뷰한다 | 10-15 게시 | [10-18 브리핑](/ai-briefing/2026-10-18/) |

## Fable 5.2를 Claude Code에서 쓰기

변경 이력 2.1.321에 Fable 5.2가 들어왔습니다. 원문: "Added Claude Fable 5.2 (`claude-fable-5-2`), now the default Fable model — 1M context, $8/$40 per Mtok with $0.20/Mtok cache reads"

- **전환 명령:** 아래 명령으로 바꿉니다. 뉴스레터는 2.1.321 이상이 필요하다고 적었습니다.

  ```
  /model claude-fable-5-2
  ```

- **저장해 둔 Fable 5.1 선택:** 2.1.321부터 `fable`로 옮겨집니다. Pro와 Max는 바로, Team과 Enterprise는 조직이 허용하면 옮겨집니다. 5.1로 돌아가려면 `/model claude-fable-5-1`을 씁니다.
- **언제 쓰나:** 뉴스레터는 Fable 5.2가 5.1보다 출력을 최대 30% 빠르게 만들고, 크고 범위가 열린 작업에 잘 맞는다고 적었습니다. 다만 대부분의 코딩은 여전히 Opus 5.5로 시작하라고 권했습니다.

### 체험 조건 (뉴스레터 기준)

| 대상 | 조건 | 기한 |
|---|---|---|
| Pro, Team Standard | 주간 한도의 최대 50%까지 Fable을 포함한다. 넘으면 사용 크레딧을 켠 경우에만 크레딧으로 쓴다 | 11월 16일 |
| 11월 9일 오전 10시(PT) 이전 유료 구독자 | 웹·데스크톱의 Settings > Usage에 Fable 리셋이 있을 수 있다 | Pro·Team Standard는 11월 16일, Max·Ultra·Team Premium은 2027년 3월 8일 |

가격 인하, 벤치마크, 시스템 카드 같은 모델 발표 내용은 [11-11 브리핑](/ai-briefing/2026-11-11/)에 정리했습니다.

## 클라우드 세션

### Claude가 환경을 대신 맞춘다 (뉴스레터 기준)

Pro, Max, Ultra에서 클라우드 세션에 의존성, 네트워크 접근, API 키가 빠져 있으면 Claude가 알려 줍니다. 바꿀 내용을 보여 주고 승인을 받은 뒤 적용합니다. API 키는 양식으로 넣기 때문에 Claude가 키를 보지 않습니다. 맞춘 설정은 저장돼서 다음 세션도 같은 환경으로 시작합니다.

키를 Claude가 보지 않는 방식은 [클라우드 환경 문서](https://code.claude.com/docs/en/cloud-environments)의 network secret 설명과 같습니다. 원문: "A network secret is an API key or token you store on a cloud environment so Claude can call that API from any session in the environment without seeing the key."

### 함께 바뀐 점

- **저장소 없이 시작 (2.1.318):** Anthropic이 호스팅하는 환경에서는 저장소를 고르지 않고 클라우드 세션을 시작할 수 있습니다. 원문: "a cloud session on an Anthropic-hosted environment can now start without picking a repository"
- **설정 스크립트 실패 (2.1.314):** 웹, 데스크톱, 모바일에서 시작한 클라우드 세션은 설정 스크립트가 오류로 끝나도 시작합니다. 전에는 세션이 시작되지 않았습니다.
- **루틴 effort (2.1.312):** 클라우드에서 도는 루틴을 만들거나 고칠 때 모델 선택기에서 effort를 정할 수 있습니다.

## 사용량과 긴 대화

### `/usage`에서 `f`

2.1.305부터 `/usage`의 경고마다 출처를 밝힌 한 줄 수정안이 붙고, `f`를 누르면 적용합니다. 같은 수정안이 VS Code와 `/doctor`에도 나옵니다. 뉴스레터 예시는 `f`가 `/effort auto`를 바로 실행하고, CLAUDE.md 변경은 먼저 보여 준 뒤 고치는 장면입니다.

### 쉬는 동안 자동 압축

긴 대화를 한동안 쉬면, 프롬프트 캐시가 만료되기 전에 대화를 요약해 둡니다. 캐시가 만료된 대화를 이어 가면 전체 문맥을 다시 처리하느라 사용량이 늘어나는데, 이를 줄이는 기능입니다. 압축되면 "Compacted while idle" 안내가 나옵니다(뉴스레터).

| 버전 | 변경 |
|---|---|
| 2.1.299 | 터미널: 200K 토큰이 넘는 대화를 약 54분 쉬면, 1시간짜리 프롬프트 캐시가 만료되기 전에 압축한다 |
| 2.1.302 | `/config`에 `Idle compaction` 행이 생겨 끄고 켤 수 있다 |
| 2.1.314 | VS Code와 데스크톱 Code 탭에도 적용한다. 텔레메트리를 끈 구독자, 그리고 1시간 캐시를 켠 Bedrock·Vertex·Foundry·LLM 게이트웨이에서도 돈다 |

### 새 메시지만 보낸다

뉴스레터는 Claude Code가 요청마다 대화 전체를 보내던 방식에서, 대부분 사용자는 새 메시지만 보내게 됐다고 적었습니다. Anthropic의 message threads 글도 같은 내용을 적었습니다. 원문: "With message threads on by default for most Claude Code users, requests with 500k to 1M tokens see median TTFT drop by about 35%." 자세한 내용은 [11-04 브리핑](/ai-briefing/2026-11-04/)에 있습니다.

### 모델을 바꾸기 전에 요약 (2.1.301)

컨텍스트 창이 더 작은 모델로 바꿀 때, 새 모델에 너무 긴 대화는 이전 모델이 먼저 요약합니다. 원문: "a conversation too long for the new model is now summarized by the previous model first"

### 백그라운드 서브에이전트 (뉴스레터 기준)

터미널 상태 줄이 갱신될 때마다 서브에이전트의 대화 전체를 다시 읽던 것을, 대부분 사용자에게는 짧은 메모로 처리하게 바꿨습니다. Pro와 Max 자체 테스트에서 백그라운드 서브에이전트가 있는 세션은 한도를 약 5% 덜 썼다고 합니다. 뉴스레터는 2.1.316 항목으로 적었지만, 공개 변경 이력의 2.1.316에서는 같은 문장을 찾지 못했습니다.

## 데스크톱 앱

### `/storage` (뉴스레터 기준)

오래된 세션이 남긴 워크트리가 디스크를 차지하는 문제를 다룹니다.

- 세션과 워크트리가 쓰는 용량을 Settings에서 보거나 `/storage`로 엽니다.
- 오래 쓰지 않은 워크트리를 나이순으로 골라 지울 수 있습니다. 세션, 대화, 브랜치는 그대로 남습니다.
- 커밋하지 않은 변경은 워크트리와 함께 사라지니 먼저 커밋해야 합니다.
- 쓰지 않는 작업도 여기서 지울 수 있습니다.

2026-11-11 기준으로 공식 [데스크톱 문서](https://code.claude.com/docs/en/desktop)에는 아직 `/storage` 설명이 없습니다.

### 대기 메시지 복구 (뉴스레터 기준)

로컬 세션에 대기시켜 둔 메시지는 데스크톱 앱이 종료, 업데이트, 충돌로 꺼져도 다시 열면 돌아옵니다.

### fork 서브에이전트 기본 켜짐 (2.1.302)

대화 전체를 물려받는 fork 서브에이전트가 터미널에 이어 `claude -p`, Agent SDK, 데스크톱 앱, VS Code에서도 기본으로 켜졌습니다. 끄려면 `CLAUDE_CODE_FORK_SUBAGENT=0`을 줍니다. [10-18 브리핑](/ai-briefing/2026-10-18/)에 정리했습니다.

## effort와 권한

### `ultrathink`

- **2.1.311:** 대화형 터미널에서 `ultrathink`를 쓰면 그 턴을 xhigh effort로 돌립니다. Anthropic API를 거치는 Opus 5.5, Sonnet 5.5, Haiku 5.5, Fable 5.1이 대상입니다. Claude가 일하는 중에 입력하면 그 턴의 남은 부분을 올립니다.
- **2.1.316:** VS Code도 같은 동작이 됐고, `CLAUDE_CODE_DISABLE_ULTRATHINK`로 키워드를 끌 수 있습니다.
- **언제 쓰나:** 세션 전체 effort는 바꾸지 않고 한 턴만 올립니다. 뉴스레터는 대부분의 작업에는 필요 없고, Claude가 막혀 있는 작업에 쓰라고 했습니다.

### `/auto-mode-setup` (2.1.304)

auto mode는 기본적으로 현재 작업 폴더와 그 저장소의 원격만 믿습니다. 그래서 다른 저장소나 팀이 쓰는 서비스로 무언가를 보내려 하면 막힙니다. `/auto-mode-setup`은 프로젝트와 최근 세션에서 실행한 명령을 보고 믿을 저장소, 도메인, 클라우드 저장소 목록의 초안을 만듭니다. 수락하면 `~/.claude/settings.json`에 저장합니다(뉴스레터). [10-21 브리핑](/ai-briefing/2026-10-21/)에 정리했습니다.

### 작업 폴더 밖 쓰기 차단 (2.1.314)

`permissions.blockWritesOutsideWorkingDirectories`를 켜면 Write, Edit, NotebookEdit가 모든 권한 모드에서 작업 폴더 밖 경로를 거부합니다. Claude Code 자신의 일부 파일은 예외입니다. 셸 명령, MCP 도구, 훅은 막지 않습니다(뉴스레터). [10-31 브리핑](/ai-briefing/2026-10-31/)에 정리했습니다.

## 바뀐 기본 동작

- **IDE 확장 (2.1.318):** IDE 터미널에서 Claude Code를 실행해도 IDE 확장을 설치하거나 업데이트하지 않습니다. 설치나 업데이트는 IDE 밖 터미널에서 `/ide`를 쓰거나, 에디터의 확장 화면에서 합니다. [11-05 브리핑](/ai-briefing/2026-11-05/)에 정리했습니다.

## Claude Code 팀이 공유한 팁 (뉴스레터)

**결정을 기다리며 멈추지 않게 하기.** Claude는 결정이 사용자 몫인지 확신이 없으면 멈추고 기다릴 수 있습니다. 처음에 아래처럼 권한을 주면, 되돌릴 수 있는 결정은 스스로 내리고 내린 결정은 나중에 알려 줍니다. 뉴스레터는 이 문장으로 여러 에이전트가 막히던 문제를 풀었다고 적었습니다.

```
Just build and use your best judgment. Make any call that's reversible yourself, and check in later with the calls you made.
```

**설계가 확장에 버티는지 지어 보기.** 설계가 충분히 유연한지 토론하는 대신, 나중에 붙일 기능을 별도 워크트리에서 버릴 셈으로 만들어 보게 합니다. 다듬지 말고 설계가 방해한 지점만 알려 달라고 합니다. 끝나면 전부 지우면 됩니다.

```
In a separate worktree, do a throwaway build of [a feature we might add later] on top of this design. Don't polish it, just tell me where the design got in the way.
```

**코드 리뷰.** Claude Code 팀은 대부분의 변경을 사람이 보기 전에 Claude가 먼저 리뷰한다고 적었습니다. 그 방식을 다룬 글은 [10-18 브리핑](/ai-briefing/2026-10-18/)에 요약했습니다.

## 참고

- [Claude Code 변경 이력](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md): 2.1.299부터 2.1.322까지
- [Cloud environments 문서](https://code.claude.com/docs/en/cloud-environments): network secret
- [Use Claude Code in the cloud 문서](https://code.claude.com/docs/en/claude-code-on-the-web)
- [Desktop application 문서](https://code.claude.com/docs/en/desktop)
- [How the Claude Code team does code review](https://claude.dev/blog/solving-code-review-at-anthropic/)
- Claude Code 팀 뉴스레터(이메일, 2026년 11월)
