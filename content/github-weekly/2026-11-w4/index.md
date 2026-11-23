---
title: "2026-11 4주차 GitHub 주간의 프로젝트"
date: 2026-11-23T14:28:00+09:00
tags: [github-weekly, github, coding-agents, skills, frontend, code-review]
description: "이번 주 GitHub 주간 트렌딩 10개 중 9개가 AI 에이전트 관련이었고, 웹 프런트엔드용 에이전트 도구 5개와 에이전트 산출물을 검토하는 plannotator·open-code-review가 눈에 띄었다."
---

> 조사 기준: 2026-11-23 14:29 KST · [GitHub Weekly Trending](https://github.com/trending?since=weekly) · 전체 언어. 주차는 그 달의 몇 번째 월요일인지로 센 것이고 GitHub 공식 주차가 아니다. 주간 Star 증가는 조회 시점 값이며, 저장소를 설치하거나 실행해 보지는 않았다.

## 핵심 요약

- **10개 중 9개가 AI 에이전트 관련 프로젝트다.** 예외는 Apple Watch용 HermesEnabler 하나다. 주 언어는 TypeScript가 6개로 가장 많고, Python이 2개, Go와 Objective-C가 1개씩이다.
- **웹 프런트엔드가 중심이다.** 화면 디자인(stitch-skills), 웹 표준 지식(modern-web-guidance-src), 브라우저 검증(chrome-devtools-mcp), 에이전트와 화면의 연결(ag-ui), 디자인 시스템(astryx)까지 5개가 이 분야다.
- **에이전트가 만든 것을 검토하는 도구가 두 개 올랐다.** plannotator는 사람이 계획과 코드 변경에 의견을 다는 화면이고, open-code-review는 AI가 코드를 검토하는 명령줄 도구다.
- **용어:** 스킬(Agent Skills)은 에이전트가 필요할 때 읽는 작업 설명서 묶음(`SKILL.md`와 스크립트)이다. MCP(Model Context Protocol)는 에이전트가 외부 도구와 데이터에 접속하는 규약이다.

## GitHub 주간 인기 프로젝트 TOP 10

| 순위 | 프로젝트 | 주간 Star 증가 | 핵심 기능 요약 |
|---|---|---|---|
| 1 | [backnotprop/plannotator](https://github.com/backnotprop/plannotator) | +2,815 | 에이전트의 계획·문서·diff에 브라우저에서 의견을 달아 돌려보내는 로컬 검토 도구 |
| 2 | [google-labs-code/stitch-skills](https://github.com/google-labs-code/stitch-skills) | +2,683 | Google의 AI 디자인 도구 Stitch를 코딩 에이전트에서 다루는 스킬 모음 |
| 3 | [GoogleChrome/modern-web-guidance-src](https://github.com/GoogleChrome/modern-web-guidance-src) | +3,575 | 최신 웹 표준 사용법과 브라우저 호환 정보를 에이전트에 넣어 주는 스킬의 원본 저장소 |
| 4 | [alibaba/open-code-review](https://github.com/alibaba/open-code-review) | +3,700 | 규칙 기반 파이프라인과 LLM 에이전트를 섞어 줄 단위 의견을 내는 AI 코드 리뷰 CLI |
| 5 | [mvanhorn/last30days-skill](https://github.com/mvanhorn/last30days-skill) | +4,731 | Reddit·X·YouTube·HN 등에서 최근 30일의 반응을 모아 요약하는 리서치 스킬 |
| 6 | [ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) | +2,268 | 에이전트가 실제 Chrome을 조작하고 성능·네트워크·콘솔을 분석하게 하는 MCP 서버와 CLI |
| 7 | [tradermonty/claude-trading-skills](https://github.com/tradermonty/claude-trading-skills) | +2,616 | 개인 투자자의 시장 점검·위험 관리·매매 기록 절차를 돕는 Claude 스킬 모음 |
| 8 | [ag-ui-protocol/ag-ui](https://github.com/ag-ui-protocol/ag-ui) | +3,620 | 에이전트와 사용자 앱을 이벤트로 잇는 개방형 프로토콜. 1.0 명세 공개 |
| 9 | [meta/astryx](https://github.com/meta/astryx) | +3,626 | 사람과 에이전트가 같은 CLI와 문서를 쓰는 Meta의 React 디자인 시스템(베타) |
| 10 | [pookjw/HermesEnabler](https://github.com/pookjw/HermesEnabler) | +461 | 일반 Apple Watch에서 Hermès 시계 화면을 켜는 비공식 앱 |

순위는 Star 증가 순이 아니라 트렌딩 페이지의 노출 순서다. 10개 중 증가 폭이 가장 큰 것은 5위다.

## 이번 주 가장 주목할 프로젝트

### 1. backnotprop/plannotator

**에이전트의 계획과 코드 변경을 사람이 보고 고칠 점을 돌려보내는 검토 화면이다.**

plannotator는 코딩 에이전트에 플러그인이나 훅으로 붙는 로컬 브라우저 앱이다. 에이전트가 계획을 내거나 작업을 마치면 화면이 열리고, 사용자가 단 의견이 에이전트 세션으로 돌아간다. README가 밝힌 지원 대상은 Claude Code, Codex, Copilot CLI, Gemini CLI, OpenCode 등 10종이다. 2025년 12월에 만들어졌고 총 Star는 약 1만 4,500개다.

**주요 특징**

- **계획 검토:** Claude Code에서는 계획 모드를 끝낼 때 훅이 실행돼 계획이 브라우저에 뜬다. 거절하면 의견이 에이전트로 가고, 고친 계획은 이전과의 차이로 보여 준다.
- **코드 리뷰:** 커밋 전 변경, GitHub PR, GitLab MR을 같은 화면에서 본다. 큰 변경은 에이전트가 중요한 순서로 장을 나누는 '가이드 리뷰'로 볼 수 있다. v0.29.0(11-18)에서 리뷰 창을 새로 만들었다.
- **Inbox:** 여러 에이전트의 질문과 파일을 한 창에 모은다. [v0.29.2](https://github.com/backnotprop/plannotator/releases/tag/v0.29.2)(11-22)에는 에이전트가 낸 명령을 사람이 눌러 실행하는 카드와, 에이전트에는 이름만 알려 주는 비밀값 입력이 들어갔다.

**활용 가능성:** 에이전트를 여러 개 돌려 터미널에서 계획을 읽기 벅찬 사람에게 맞는다. 작은 수정 위주라면 에이전트의 기본 계획 모드나 PR 화면으로 충분하다. 주의할 점은 셋이다. 원격 모드는 로그인 없이 `0.0.0.0`에서 포트를 연다. [개인정보 정책](https://plannotator.ai/privacy)에 따르면 도구별로 하루 한 번 익명 핑을 보내며 `NO_ANON_PING=1`로 끈다. 11-16~22(UTC) 사이에만 릴리스가 5번 나와 버전을 고정하는 편이 낫다.

**GitHub:** <https://github.com/backnotprop/plannotator>

### 2. google-labs-code/stitch-skills

**글이나 기존 코드로 화면 디자인을 만들고 다시 코드로 옮기는 일을 에이전트에 맡기는 스킬 묶음이다.**

Stitch는 Google의 AI 화면 디자인 도구다. 이 저장소는 에이전트가 Stitch의 MCP 서버를 다루는 절차를 스킬로 정리했다. 디자인 6개, 빌드 5개, 유틸리티 5개를 세 플러그인으로 나눴다. README는 Codex, Antigravity, Gemini CLI, Claude Code, Cursor, OpenCode(수동 설치)와 호환된다고 안내하며, Google이 공식 지원하는 제품은 아니라고 밝힌다.

**주요 특징**

- **양방향 변환:** 기존 프런트엔드에서 HTML과 디자인 규칙을 뽑아 Stitch에 올리고(`code-to-design`), Stitch 화면을 React나 React Native 컴포넌트로 옮긴다.
- **`DESIGN.md` 중심:** 디자인 규칙을 마크다운 파일로 뽑아 Stitch에 올리고 여러 화면에 적용한다.
- **검증 단계:** React 변환 스킬은 단계마다 통과 조건을 두고 구문 트리(AST) 기반 검사를 돌린다. 11월 20일(UTC)에는 Tailwind 4 설정 인식과 하드코딩 색상 검사 수정이 병합됐다.

**활용 가능성:** 시안 없이 시작하는 작은 팀이나 빠른 시제품에 맞는다. 디자인 원본이 다른 도구에 있다면 그 도구의 에이전트 연동을 먼저 본다. [`upload-to-stitch` 스킬](https://github.com/google-labs-code/stitch-skills/blob/main/plugins/stitch-design/skills/upload-to-stitch/SKILL.md)은 `~/.claude.json` 같은 설정 파일에서 API 키를 찾아 명령 인자로 넘기게 한다. 키가 대화 기록이나 프로세스 목록에 남을 수 있어 전용 키를 쓰는 편이 안전하다. 업로드 전에 사용자 확인을 받는 단계는 들어 있다. 정식 릴리스는 5월의 v1.0이 마지막이다.

**GitHub:** <https://github.com/google-labs-code/stitch-skills>

### 3. GoogleChrome/modern-web-guidance-src

**코딩 에이전트가 낡은 웹 코드 대신 최신 표준 기능을 쓰도록 이끄는 안내서 묶음이다.**

이 저장소는 스킬을 만들고 평가하는 원본이고, 설치 대상은 배포용 저장소 [modern-web-guidance](https://github.com/GoogleChrome/modern-web-guidance)다. 에이전트가 웹 작업을 만나면 CLI로 안내서를 검색해 필요한 것만 읽는다. 11월 10일 1.0이 나왔고, [Chrome 블로그](https://developer.chrome.com/blog/modern-web-guidance-v1)에 따르면 Firefox와 Edge도 참여하는 공동 프로젝트가 됐다. 1.0 릴리스 노트는 안내서 203개로 웹 기능 250개를 다룬다고 적었다.

**주요 특징**

- **오프라인 검색:** README에 따르면 임베딩 모델과 BM25(단어 빈도 기반 검색)를 내 컴퓨터에서 돌려 안내서를 찾는다. API 키가 필요 없다.
- **기존 코드 점검:** `legacy-scan`이 낡은 패턴을 찾아 대체 안내서와 연결한다. 릴리스 노트에 따르면 이 규칙이 있는 안내서는 아직 20% 정도다.
- **공개 평가:** README의 11월 8일 평가에서 검사 통과율은 에이전트별로 54~58%에서 82~94%로 올랐다. 프로젝트가 직접 만든 과제로 잰 값이다.

**활용 가능성:** 에이전트로 웹 화면을 만드는 팀에 맞고, 서버 위주라면 얻을 것이 적다. 대안은 지원 브라우저와 금지 패턴을 프로젝트 규칙 파일에 직접 적는 것이다. Google이 설치 수, 에이전트 이름, 검색어 등 익명 정보를 모으며 `DISABLE_TELEMETRY=1`로 끈다. Claude Code 기본 마켓플레이스로 설치하면 v0.0.194에 머문다는 [이슈](https://github.com/GoogleChrome/modern-web-guidance-src/issues/3533)가 열려 있고, README는 `googlechrome` 마켓플레이스로 옮기라고 안내한다.

**GitHub:** <https://github.com/GoogleChrome/modern-web-guidance-src>

## 이번 주 GitHub 기술 트렌드

### ① 웹 프런트엔드 작업의 각 단계에 에이전트 도구가 붙었다

시안은 stitch-skills, 구현 지식은 modern-web-guidance-src, 부품은 astryx, 실행 확인은 chrome-devtools-mcp, 완성된 에이전트를 화면에 붙이는 일은 ag-ui가 맡는다. Chrome 블로그는 modern-web-guidance와 chrome-devtools-mcp를 함께 쓰라고 권한다.

### ② 검토 방식이 따로 제품이 된다

plannotator는 사람의 판단을 작업 중간에 끼워 넣는다. open-code-review는 파일 선택과 규칙 적용을 모델 대신 코드로 고정한다. open-code-review의 README는 같은 모델의 범용 에이전트보다 정밀도는 높고 재현율(실제 결함을 찾아내는 비율)은 낮다고 적었다.

### ③ 스킬이 코딩 밖으로 나간다

last30days-skill은 최근 반응 조사를, claude-trading-skills는 투자 점검 절차를 스킬로 묶었다. 배포가 쉬워진 만큼 갱신이 문제다. modern-web-guidance의 버전 정체가 그 사례다.

## 개발·업무 활용 관점 평가

| 프로젝트 | 활용 분야 | 검토 우선순위 | 이유 |
|---|---|---|---|
| modern-web-guidance-src | 웹 UI 개발 | 높음 | 설치 없이 `search`만 써 볼 수 있고 API 키가 필요 없다 |
| plannotator | 계획·diff 검토 | 높음 | 쓰던 에이전트에 붙이고 로컬에서 돈다. 원격 모드는 주의 |
| open-code-review | PR 리뷰 | 중간 | 위임 모드는 별도 모델 설정이 없다. 놓치는 결함을 먼저 재 본다 |
| chrome-devtools-mcp | 웹 디버깅·성능 | 중간 | 브라우저 내용이 에이전트에 보이므로 전용 프로필을 쓴다 |
| stitch-skills | 시안·시제품 | 낮음 | Stitch 설정과 API 키 관리가 먼저 필요하다 |

**이번 주 추천 프로젝트: modern-web-guidance-src.** 한 번의 검색으로 쓸모를 판단할 수 있고, 평가 과제와 결과를 공개한다. 브라우저 팀들이 함께 관리한다는 점도 크다. 설치는 배포용 저장소에서 한다.

## 종합 분석 및 시사점

**확인된 사실:** TOP 10 중 9개가 AI 에이전트 관련이다. 3개는 Google 계열 조직, 2개는 Meta와 Alibaba의 저장소다. 5개는 2026년에 만들어졌다. GitHub API 기준 라이선스는 Apache-2.0과 MIT가 5개씩이다(plannotator는 두 라이선스 중 고를 수 있다).

**분석과 해석:** 기업이 자사 기술을 에이전트가 읽는 형태로 내놓는 흐름으로 읽힌다. 모델 성능보다 무엇을 읽히고 어떻게 검토하느냐가 차이를 만든다는 판단이 깔려 있다고 본다.

**실무적 제안:** 스킬을 들이기 전에 `SKILL.md`에서 권한, 키를 다루는 방식, 외부 전송을 먼저 읽는다. 버전을 고정하고 사용 정보 수집 설정을 확인한다. claude-trading-skills는 README가 투자 조언이 아니라고 밝히므로 점검 절차로만 쓴다. HermesEnabler는 시스템 설정 백업을 다루고 '사용자 책임'을 명시하니 업무 기기에는 설치하지 않는다.

**남은 불확실성:** Star 증가는 관심의 크기일 뿐 사용량이나 품질이 아니다. 벤치마크는 재현하지 않았다. Stitch MCP 설정 문서는 스크립트로 그려져 본문을 읽지 못했고 요금과 사용 한도도 확인하지 못했다. 이 연재의 첫 글이라 지난주와 비교할 수 없다.

## 참고자료

- [GitHub Weekly Trending](https://github.com/trending?since=weekly)
- [backnotprop/plannotator](https://github.com/backnotprop/plannotator)
- [google-labs-code/stitch-skills](https://github.com/google-labs-code/stitch-skills)
- [GoogleChrome/modern-web-guidance-src](https://github.com/GoogleChrome/modern-web-guidance-src)
- [alibaba/open-code-review](https://github.com/alibaba/open-code-review)
- [mvanhorn/last30days-skill](https://github.com/mvanhorn/last30days-skill)
- [ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp)
- [tradermonty/claude-trading-skills](https://github.com/tradermonty/claude-trading-skills)
- [ag-ui-protocol/ag-ui](https://github.com/ag-ui-protocol/ag-ui)
- [meta/astryx](https://github.com/meta/astryx)
- [pookjw/HermesEnabler](https://github.com/pookjw/HermesEnabler)

조사 및 확인일: 2026-11-23
