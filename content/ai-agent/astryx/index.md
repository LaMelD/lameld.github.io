---
title: "Astryx: Meta가 에이전트를 사용자로 놓고 만든 디자인 시스템, CLI 설계와 공개 점수로 보기"
date: 2026-11-24T17:50:00+09:00
weight: 14
tags: [claude-code, astryx, design-system, react, mcp, cli]
description: "Meta의 오픈소스 React 디자인 시스템 Astryx를 에이전트 관점에서 읽었다. CLI가 곧 문서인 구조, CLI와 MCP를 비교한 실험, 매일 밤 공개한 점수표에서 shadcn에 더 자주 진 이유를 정리했다. CLI를 격리 설치해 출력 크기와 한국어 검색, 전역 설치가 쓰는 파일도 직접 확인했다."
---

저장소: [github.com/meta/astryx](https://github.com/meta/astryx) · 사이트: [astryx.atmeta.com](https://astryx.atmeta.com) · MIT · React 19 이상 · 베타 · 최신 v0.7.17(2026-11-20)

> 조사 기준일 2026-11-24, `main` 커밋 `44ac32b`와 npm 0.7.17. 저장소·블로그·위키를 읽고, CLI는 **별도 HOME 폴더에 설치 스크립트를 끄고** 설치해 출력을 재 봤다. 앱을 만들거나 에이전트에게 화면을 맡겨 보지는 않았다. [이번 주 GitHub 주간 글](/github-weekly/2026-11-w4/)에서 9위로 짧게 소개한 프로젝트를 자세히 본 것이다.

## 한눈에 보기

- **무엇:** Meta 안에서 8년 동안 쓰던 디자인 시스템을 오픈소스 기술로 다시 만든 React 컴포넌트 라이브러리다. 6월 18일 공개했고 스타는 3만 2천이다. Meta는 내부판이 앱 13,000개 이상에 쓰인다고 밝힌다.
- **에이전트가 읽는 문서를 CLI로 만들었다.** 문서 사이트가 CLI의 내용을 가져다 쓴다. 에이전트는 `build → template → component` 세 명령으로 화면을 만든다.
- **MCP는 차선이다.** 팀이 여섯 방식을 비교한 실험에서 검색을 붙인 CLI의 점수가 가장 높았고, 점수가 비슷한 MCP 방식보다 토큰을 덜 썼다. 지금 문서도 명령을 실행할 수 있는 에이전트에는 CLI를 권한다.
- **점수표를 공개했고, 자주 졌다.** 3월부터 9월 1일까지 126번의 평가에서 shadcn/ui 기준선보다 높은 날이 36번, 낮은 날이 80번이다. 기록된 실패는 모델이 없는 props나 값을 **추측해 쓴** 경우가 많다.
- **한국어:** 컴포넌트 번역과 IME 처리는 잘 돼 있다. CLI 검색은 한국어 질의를 찾지 못하고, 기본 글꼴에는 한글이 없다.
- **주의:** 전역 설치는 첫 실행 때 묻지 않고 에이전트 스킬 폴더에 파일을 쓴다. 베타라 변화가 빠르다(v0.7.0 이후 38일 동안 패치 17번).

## 무엇으로 이루어져 있나

| 패키지 | 내용 |
|---|---|
| `@astryxdesign/core` | 접근성을 갖춘 컴포넌트(README는 150개 이상, `init`이 쓴 파일에는 174개), 테마 시스템, 유틸리티 |
| `@astryxdesign/cli` | 문서, 템플릿, 테마, 업그레이드 코드 변환, MCP 서버 |
| `@astryxdesign/theme-*` | 기성 테마 7종 |
| `charts`, `vega`, `native` | canary 전용. 안정판이 아직 없다 |

스타일은 StyleX로 쓰지만 사용자는 미리 빌드한 CSS만 불러와도 된다. Tailwind나 CSS 모듈로 덮어쓸 수 있고, 테마는 CSS 변수 묶음이다. 더 깊이 고치려면 `swizzle`로 컴포넌트 소스를 프로젝트에 꺼내 온다. 빌드 없이 쓸 때 CSS는 315KB(gzip 49KB)다.

[A2UI](/ai-agent/a2ui-1-0-mcp-apps/)와는 쓰는 시점이 다르다. A2UI는 에이전트가 **실행 중에** 데이터로 화면을 고르는 규약이다. Astryx는 코딩 에이전트가 **개발 중에** 화면 코드를 쓸 때 참고하는 부품과 설명서다.

## CLI가 곧 문서다

블로그는 "The CLI is the docs"라고 쓴다([The best CLI is one you never run](https://astryx.atmeta.com/blog/the-astryx-cli)). 문서의 사본을 따로 두지 않으니 에이전트가 낡은 설명을 읽을 일이 없다는 논리다. `astryx init`이 프로젝트의 `AGENTS.md`에 쓰는 블록은 3,702자이고, 핵심은 세 줄이다(설명은 옮긴 것).

```text
WORKFLOW — start every page from a template, never from scratch:
1. astryx build "<idea>"           — 출발할 템플릿과 모자란 부품을 알려 준다
2. astryx template <name> <path>   — 템플릿을 파일로 만든다. 틀과 간격은 두고 내용만 바꾼다
3. astryx component <Name>         — 쓰기 전에 props를 읽는다
```

왜 템플릿부터인가? 팀은 [에이전트가 예제를 베낀다](https://astryx.atmeta.com/blog/astryx-cli-build-command)는 사실에서 출발했다. 초기 템플릿에 `div`가 많았더니, 문서를 다 갖고도 에이전트가 그 `div`를 베꼈다. 그래서 설명을 늘리는 대신 **베낄 예제의 품질**을 채점 기준표로 올렸다.

`build`는 똑똑해 보이지만 모델을 부르지 않는다. 동의어 사전과 어간 처리, 오타 허용을 섞은 키워드 순위 계산이라 같은 질의에는 같은 답이 나온다.

### 에이전트가 읽는 양을 줄인다

기여 문서는 에이전트가 "명령이 찍는 모든 바이트를 매번 읽는다"며 출력을 짧게 하라고 적었다. 0.7.17에서 직접 잰 값이다(문자 수, 영어라 토큰은 대략 4분의 1).

| 명령 | 문자 수 |
|---|---:|
| `component Button --detail brief` | 836 |
| `build "admin dashboard with a user table"` | 3,345 |
| `component Button`(기본) | 4,120 |
| `docs theme` | 4,997 |
| `component Button --detail full` | 8,652 |
| `manifest --json`(전체 명령 명세) | 197,754 |

한 번 실행에 0.1~0.6초가 걸렸다. 5월에는 테마 문서 하나가 42K 토큰이라 에이전트가 포기한다는 지적이 있었다([#2306](https://github.com/meta/astryx/issues/2306)). 지금은 문서를 나눠 필요한 가지만 읽게 했다.

### 기계가 읽는 계약

모든 명령이 `--json`을 지원하고, 오류에는 바뀌지 않는 코드가 붙는다. 문구가 아니라 코드로 분기하라고 문서가 못 박는다. 없는 컴포넌트를 물으면 이렇게 나온다.

```json
{
  "apiVersion": 1,
  "error": "No component named \"Nope\"",
  "code": "ERR_UNKNOWN_COMPONENT",
  "suggestions": [{"name": "Code", "reason": "similar name (distance 2)"}]
}
```

종료 코드는 1이다. 앞 글의 [MoneyPrinterTurbo 스킬](/ai-agent/moneyprinterturbo/)이 종료 코드로 다음 행동을 정했다면, 여기는 오류 코드와 **다음에 실행할 명령**까지 출력에 싣는다.

## CLI냐 MCP냐: 여섯 가지를 비교한 기록

팀은 5월에 같은 문서를 여섯 방식으로 에이전트에게 주고 비교했다([#2306](https://github.com/meta/astryx/issues/2306), 당시 이름은 XDS).

| 방식 | 평균 점수 | 토큰 |
|---|---:|---:|
| **CLI + `search`** | **9.6** | 20K |
| MCP, 도구 2개(검색·조회) | 9.3 | 35K |
| CLI(전체 문서를 그대로) | 8.7 | 63K |
| MCP, 도구 6개 | 8.6 | 17K |
| MCP, 검색 도구 1개 | 7.0 | 15K |
| MCP, 의도를 해석하는 도구 1개 | 5.7 | 6K |

이 글이 뽑은 교훈은 세 가지다.

- **별칭 해석이 가장 큰 변수다.** "success message"로 `Toast`를 찾게 하는 60개짜리 동의어 표가 발견율을 70%에서 95% 이상으로 올렸다고 한다.
- **도구 설계보다 응답 크기 제한이 중요하다.** 잘된 방식은 모두 응답을 1.5K 토큰 안팎으로 막았다.
- **MCP는 품질이 아니라 배포를 낫게 한다.** 설치 없이 IDE에 붙는다는 장점이다.

질문 7개로 만든 팀이 직접 매긴 점수라는 한계는 있다. 컴포넌트가 70개 안팎이던 때이기도 하다.

지금의 `astryx mcp`는 CLI 명령을 그대로 도구로 내놓는다. 직접 띄워 보니 **도구 56개, 정의만 97,104자**였다. `--docs-only`로 띄우면 9개, 8,102자로 줄어든다. Claude Code는 MCP 도구를 필요할 때 불러오는 것이 [기본값](https://code.claude.com/docs/en/mcp)이라 부담이 작다. 정의를 처음부터 다 싣는 클라이언트라면 차이가 크다. 문서도 명령을 실행할 수 없는 도구에만 MCP를 쓰라고 한다.

## 점수표를 공개했고, 자주 졌다

Astryx는 [vibe test](https://astryx.atmeta.com/blog/vibe-tests)라는 평가를 매일 밤 돌렸다. 같은 요청을 네 구성(Astryx, Astryx+Tailwind, shadcn/ui+Tailwind 기준선, 순수 HTML)에 주고 정적 분석으로 채점한다. 결과는 [위키](https://github.com/meta/astryx/wiki/Vibe-Test-Scores)에 있다. 2026-03-20부터 09-01까지 126행을 직접 집계했다(척도가 다른 초기 실행은 위키가 이미 뺐다).

| 구간 | 횟수 | Astryx 평균 | 기준선 평균 | Astryx가 높은 날 / 낮은 날 |
|---|---:|---:|---:|---:|
| 03-20 ~ 05-21 | 25 | 93.2 | 93.1 | 18 / 5 |
| 05-22 ~ 06-30 | 39 | 86.7 | 94.7 | 1 / 36 |
| 07-01 ~ 07-31 | 30 | 92.5 | 95.0 | 7 / 21 |
| 08-01 ~ 09-01 | 32 | 92.6 | 95.2 | 10 / 18 |
| **전체** | **126** | **90.9** | **94.6** | **36 / 80** |

같은 점수인 날이 10번이다. 순수 HTML 평균은 81.3이다. 블로그도 숨기지 않는다. 원문: "Across our full ledger the baseline actually wins the plurality of nights".

**어디서 졌는지가 중요하다.** 항목별 점수가 남은 67일(06-26~09-01)의 평균은 접근성 99.1, 코드 품질 99.3, 유지보수성 93.5, 효율 86.0이다. 정확성이 81.2로 가장 낮고 변동도 크다. 2점까지 떨어진 날이 있고 70점 아래가 17일이다. 마지막 공개 기록인 [9월 2일](https://github.com/meta/astryx/issues/5911)의 실패 사유를 보면 성격이 뚜렷하다.

- `Popover`를 다른 라이브러리처럼 `trigger` prop으로 썼다(Astryx는 여는 요소를 `children`으로 받는다).
- `Stack`에 CSS처럼 `justify="space-between"`을 썼다(맞는 값은 `between`).
- `AspectRatio`에 숫자 대신 문자열 `"16/9"`를 넘겼다.

대부분 다른 라이브러리나 CSS에서 흔한 표기다. 같은 날 shadcn 기준선의 정확성은 100이었다. 내 해석은 이렇다. 모델은 shadcn을 학습 데이터로 **외웠고**, Astryx는 **찾아봐야** 한다. 찾아보지 않고 쓰는 순간 틀린다. 지금 `AGENTS.md` 블록에 "Astryx names differ from other libraries"로 시작하는 문장이 들어간 것도 이 문제를 겨냥한 것으로 보인다.

**9월 2일 이후의 공개 점수는 찾지 못했다.** 대신 11월 11일에 [새 평가 도구](https://github.com/meta/astryx/tree/main/internal/vibe-tests/eval)가 들어왔다. 설계가 많이 다르다.

| | 이전 vibe test | 새 vibe eval |
|---|---|---|
| 채점 | 코드 정적 분석, 항목별 점수 | 타입 검사·빌드·브라우저 동작·오류 없음을 모두 통과해야 합격 |
| 심판 | 별도 에이전트가 결과를 비교. 디자인 항목은 미완성 | LLM 심판 없음 |
| 에이전트 | 하위 에이전트 | Claude Code bare 모드, 모델 고정 |
| 통계 | 하룻밤 점수 | 반복 실행과 95% 신뢰구간, 비용 상한 |
| 부정 방지 | 프롬프트·환경 규칙 | 대화 기록 감사. 저장소나 다른 실행을 읽으면 탈락 |

"구간이 0을 포함하는 차이는 잡음이다"라고 문서에 적어 둔 점, 일부러 틀린 코드가 탈락하는지 먼저 확인하는 점은 사내에서 에이전트를 비교할 때 그대로 가져올 만하다.

## 설치와 에이전트 연결

프로젝트에 넣는 방법이 기본이다.

```bash
npm install @astryxdesign/core @astryxdesign/theme-neutral @stylexjs/stylex
npm install -D @astryxdesign/cli
npx @astryxdesign/cli init        # AGENTS.md 등에 안내 블록을 쓴다
```

빈 프로젝트에서 `init`을 돌리니 `AGENTS.md`와 `GEMINI.md`(한 줄 import)가 생겼다. Claude Code는 [`CLAUDE.md`가 없을 때만](https://code.claude.com/docs/en/memory) `AGENTS.md`를 읽는다(2.1.277 이상). 문서에 따르면 `init`은 `CLAUDE.md`가 있으면 거기에도 같은 블록을 쓴다.

Claude Code에는 다른 길도 있다.

```bash
# 짧은 스킬 하나짜리 플러그인
claude plugin marketplace add meta/astryx
claude plugin install astryx@astryx

# 설치 없이 문서만: 호스팅 MCP(읽기 명령만)
claude mcp add --transport http astryx https://astryx.atmeta.com/mcp
```

**전역 설치(`npm i -g`)는 알고 쓴다.** 임시 HOME에서 시험했더니 명령을 처음 한 번 실행한 뒤 다음이 생겼다. 확인을 묻지 않았다.

```text
~/.agents/skills/astryx/SKILL.md   # 2,792자
~/.claude/skills/astryx            # 위 폴더로 가는 링크
~/.config/astryx/                  # state.json, settings.json
```

[문서](https://github.com/meta/astryx/blob/main/packages/cli/assets/docs/tree/computer-agent-guide.doc.mjs)에 적힌 동작이고, 이미 있던 다른 폴더는 덮어쓰지 않는다고 한다. 원하지 않으면 `astryx init --user --remove-agents`로 지우고 끈다.

## 한국어로 쓸 때

- **번역:** `core`에 `ko-KR`을 포함한 30개 로케일이 들어 있다. "다음 페이지로 이동" 같은 접근성 문구까지 번역돼 있다.
- **IME:** 한글 조합 중 Enter가 폼을 제출해 버리는 문제를 공통 함수(`isImeKeyEvent`)로 막는다. 소스 20여 개 파일이 쓰고, 채팅 입력창도 포함된다. 주석에 한국어가 예로 적혀 있다.
- **글꼴:** 기본 테마의 본문 글꼴 Figtree에는 한글이 없다. 한글은 운영체제 글꼴로 그려지므로 테마에서 Pretendard 같은 글꼴을 지정한다.
- **CLI 검색:** `버튼`, `로그인 화면`, `달력에서 날짜 고르기`를 `search`에 넣으면 모두 결과가 없다. `build`는 기본 틀(app shell)만 준다. 영어 `login screen`은 로그인 템플릿을 바로 찾는다. 에이전트에게 **"Astryx 검색어는 영어로 쓰라"** 고 규칙 파일에 한 줄 적어 둔다.
- **문서 언어:** `--zh`(중국어 간체)는 있고 한국어 옵션은 없다.

## 도입 전에 볼 것

- **아직 베타다.** 0.2.0(7월 30일)에서 0.7.0(10월 13일)까지 석 달이 채 안 걸렸다. 0.x에서는 마이너 버전이 호환을 깰 수 있다. `astryx upgrade --from <버전> --apply`가 코드 변환을 해 주지만, 변환 결과는 리뷰한다.
- **버전을 한꺼번에 올린다.** `theme-neutral` 0.7.17은 `core` 0.7.17을 정확히 요구한다.
- **이름을 조심한다.** 스코프 없는 npm 패키지 `astryx`는 **관련 없는 패키지**다. CLI를 설치하기 전에 `npx astryx`를 실행하지 말라고 README가 경고한다. 첫 실행은 `npx @astryxdesign/cli`로 한다.
- **용량이 크다.** core·cli·테마·React를 설치한 빈 프로젝트의 `node_modules`가 229MB였다(core 47MB, cli 40MB).
- **StyleX 컴파일러가 필요한 템플릿이 있다.** "빌드 설정 없이"는 컴포넌트 이야기다. `shell-top-nav` 템플릿은 StyleX 플러그인을 설정하라고 안내한다.
- **`gap-report`는 공개 이슈를 만든다.** 없는 기능을 신고하는 명령이다. `--confirm-public`을 붙여야만 내 `gh` 로그인으로 GitHub 이슈가 올라간다. 에이전트가 이 플래그를 붙이지 못하게 규칙으로 막아 둔다.
- **외부 전송:** 배포된 CLI 0.7.17 코드에서 사용 통계를 보내는 부분은 찾지 못했다. 네트워크를 쓰는 곳은 블로그 읽기, 원격 테마·이미지 가져오기, npm 새 버전 확인(하루 한 번, `NO_UPDATE_NOTIFIER=1`로 끔) 등이다.
- **열린 항목이 많다.** 이슈 354개, PR 425개다. 저장소 관리에도 에이전트를 쓴다. 위키의 [Night Watch](https://github.com/meta/astryx/wiki/Night-Watch-Overview)는 밤마다 QA·리뷰·의존성 점검을 맡기되 승인과 병합은 사람만 한다고 정했다. 커밋에는 AI 작성 표시를 남기지 않고 PR 본문에 역할 이름 한 줄만 허용한다.

## 어떤 팀에 맞나

- **맞는 경우:** React 19로 새 관리 도구나 사내 앱을 만들고, 화면 코드를 에이전트가 많이 쓰며, 접근성과 테마 일관성이 중요한 팀.
- **서두르지 않아도 되는 경우:** shadcn/ui로 이미 잘 돌아가는 팀. 공개 점수에서 모델은 shadcn을 더 정확하게 썼다. shadcn 앱에 Astryx 화면 하나만 얹는 레지스트리 방식은 저장소에 [초안 글](https://github.com/meta/astryx/blob/main/apps/docsite/src/content/blog/posts/meet-astryx-from-shadcn.md)만 있고 아직 공개되지 않았다.
- **시험 순서:** 새 브랜치에 프로젝트 설치 → `init` → 같은 화면을 지금 쓰는 라이브러리와 Astryx로 각각 시켜 보기 → `tsc` 오류 수와 고친 횟수 비교. 한 번으로 결론 내지 말고 여러 번 돌린다.

디자인 시스템을 만들지 않더라도 배울 것이 있다. **에이전트에게 줄 사내 라이브러리 문서**를 고민한다면 네 가지를 가져올 만하다.

1. 문서를 길게 붙이지 말고, 검색해서 조금씩 읽는 CLI로 만든다. 응답 크기를 제한한다.
2. 사람이 쓰는 말과 이름을 잇는 동의어 표를 둔다. 우리 팀이라면 한국어 표가 먼저다.
3. 설명보다 베낄 예제의 품질을 올린다.
4. 감이 아니라 반복 실행과 빌드 통과율로 비교하고, 진 날도 기록한다.

## 참고

- [meta/astryx](https://github.com/meta/astryx), [CLI README](https://github.com/meta/astryx/blob/main/packages/cli/README.md), [에이전트 플러그인](https://github.com/meta/astryx/tree/main/plugins/astryx)
- Astryx 블로그: [Introducing Astryx](https://astryx.atmeta.com/blog/introducing-astryx), [How Astryx works](https://astryx.atmeta.com/blog/how-astryx-works), [The best CLI is one you never run](https://astryx.atmeta.com/blog/the-astryx-cli), [AI is a copycat](https://astryx.atmeta.com/blog/astryx-cli-build-command), [Vibe Test](https://astryx.atmeta.com/blog/vibe-tests)
- [#2306: CLI vs MCP, 여섯 방식 비교](https://github.com/meta/astryx/issues/2306), [Vibe Test Scores(위키)](https://github.com/meta/astryx/wiki/Vibe-Test-Scores), [2026-09-02 결과](https://github.com/meta/astryx/issues/5911), [vibe eval README](https://github.com/meta/astryx/blob/main/internal/vibe-tests/eval/README.md)
- npm: [@astryxdesign/core](https://www.npmjs.com/package/@astryxdesign/core), [@astryxdesign/cli](https://www.npmjs.com/package/@astryxdesign/cli)
- Claude Code 문서: [메모리(AGENTS.md)](https://code.claude.com/docs/en/memory), [MCP](https://code.claude.com/docs/en/mcp)
- 관련 글: [GitHub 주간 2026-11 4주차](/github-weekly/2026-11-w4/), [A2UI 1.0](/ai-agent/a2ui-1-0-mcp-apps/), [MCP 서버 수동 등록](/ai-agent/mcp-manual-setup/), [Custom Skill](/ai-agent/custom-skills/)
