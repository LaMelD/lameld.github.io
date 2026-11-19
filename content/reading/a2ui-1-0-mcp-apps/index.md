---
title: "A2UI 1.0: 에이전트가 텍스트 대신 화면으로 답하는 Google 프로토콜과 MCP Apps"
date: 2026-11-19T13:40:00+09:00
weight: 30
tags: [reading, a2ui, generative-ui, mcp, mcp-apps, google]
description: "Google이 생성형 UI 프로토콜 A2UI의 첫 안정판 1.0을 냈다. 양방향 함수 호출, 카탈로그 혼용, @ 예약 키가 들어왔고 0.9 카탈로그는 그대로 쓸 수 없다. PyTorchKR 정리 글을 Google 발표·명세·진화 가이드·패키지 레지스트리와 대조하고, MCP Apps와 결합하는 세 가지 방식과 아직 남은 한계를 정리했다."
---

> 이 글은 PyTorchKR의 [「Google, 에이전트가 텍스트 대신 네이티브 화면으로 답하게 하는 생성형 UI 프로토콜 A2UI 1.0 공개 (feat. MCP Apps)」](https://discuss.pytorch.kr/t/google-ui-a2ui-1-0-feat-mcp-apps/12496)(9bow, 2026-11-13)를 읽고 정리한 글이다. 그 글은 Google의 [A2UI 1.0 발표](https://developers.googleblog.com/a2ui-10-build-delightful-fast-reliable-agentic-experiences/)(2026-11-11)를 옮긴 요약이고, GPT 초안을 바탕으로 썼다고 스스로 밝혔다. 그래서 수치와 명세 내용은 Google 블로그 글 3편, [v1.0 명세](https://a2ui.org/specification/v1.0-a2ui/), [진화 가이드](https://github.com/a2ui-project/a2ui/blob/main/specification/v1_0/docs/evolution_guide.md), MCP Apps 문서, npm·PyPI·pub.dev 레지스트리와 대조했다. 블로그에서는 [11-13 브리핑](/ai-briefing/2026-11-13/)(1.0 발표)과 [10-28 브리핑](/ai-briefing/2026-10-28/)(A2UI Express)에서 짧게 다뤘다. 2026-11-19에 확인했다.

## 한눈에 보기

- **무엇:** AI 에이전트가 긴 텍스트 대신 폼, 차트, 확인 카드 같은 화면으로 답하게 하는 Google의 오픈 프로토콜이다. 에이전트는 화면을 **데이터(JSON)** 로 설명하고, 앱은 자기가 등록한 **카탈로그** 컴포넌트로만 그린다. 라이선스는 Apache 2.0이다.
- **1.0에서 바뀐 것:** 에이전트와 렌더러가 서로 함수를 부르는 양방향 함수 호출이 들어왔다. 한 화면에 여러 카탈로그를 섞을 수 있고, `createSurface` 메시지 하나로 화면을 만든다. 예약 키에는 `@` 접두가 붙는다. **0.9 카탈로그는 1.0 런타임에서 그대로 쓸 수 없다.**
- **안정판은 프로토콜만이다.** 렌더러와 SDK 패키지는 모두 0.x다. Android(Jetpack Compose) 렌더러는 아직 0.9.1이다.
- **A2UI Express:** 모델은 JSON 대신 짧은 생성자 문법을 쓰고, SDK가 서버에서 표준 JSON으로 컴파일한다. Google 측정으로 입력 토큰은 45.4%, 생성 코드 토큰은 63.2% 줄었고 정확도는 같았다.
- **MCP Apps와는 함께 쓴다.** 경쟁 관계가 아니다. A2UI 웹 렌더러를 MCP App 안에 넣으면 Claude 같은 MCP Apps 호스트에도 A2UI 화면이 뜬다. 다만 그때는 네이티브가 아니라 iframe 속 웹 렌더러로 그려진다.

## A2UI가 하는 일

README의 한 줄이 핵심이다. 원문: "A2UI expresses UI using _data_, not code." 에이전트가 HTML이나 코드를 내려보내지 않는다. 앱이 미리 허락한 컴포넌트 목록(카탈로그) 안에서만 화면을 조립한다. 그래서 앱 개발자가 에이전트가 할 수 있는 일을 통제한다. Google은 데이터 바인딩이 서버에서 일어나므로 민감한 값이 모델에 닿지 않는다고도 주장한다. 원문: "sensitive values never reach the model."

구조는 세 층이다(README).

| 층 | 하는 일 |
|---|---|
| Inference | LLM이나 결정 모델이 **추론 형식**(Express, JSON 등)으로 화면을 만든다 |
| Transport | Agent SDK가 파싱·검증·매크로 확장을 거쳐 표준 A2UI 메시지로 바꾼다 |
| Rendering | 코어 라이브러리 + 프레임워크 어댑터 + 카탈로그가 네이티브 컴포넌트로 그린다 |

명세는 전송 방식을 정하지 않는다. 원문: "The A2UI protocol is designed to be transport-agnostic." 바인딩으로 AG-UI, A2A 확장, MCP(도구 호출·도구 출력·리소스 구독), SSE + JSON-RPC, WebSocket, REST를 든다. PyTorchKR 글은 MCP·A2A·AG-UI를 어디에 놓을지의 문제로만 다뤘다. 명세에서는 이 셋이 **A2UI 메시지를 실어 나르는 전송 바인딩**이다.

메시지는 v1.0 명세의 envelope 절 기준으로 다음과 같다. 명세 도입부는 앞의 네 개만 나열해서 같은 문서 안에서 개수가 다르다.

- **에이전트 → 렌더러:** `createSurface`, `updateComponents`, `updateDataModel`, `deleteSurface`, `callRendererFunction`, `agentFunctionResponse`
- **렌더러 → 에이전트:** `action`, `callAgentFunction`, `rendererFunctionResponse`, `error`

## 0.9에서 1.0으로: 바뀐 것

아래 항목은 모두 진화 가이드에서 확인했다.

| 항목 | 내용 |
|---|---|
| 양방향 함수 호출 | `callRendererFunction`(에이전트 → 렌더러)과 `callAgentFunction`(렌더러 → 에이전트)이 생겼다. 카탈로그 함수의 `allowedCallers`는 `rendererOnly`·`agentOnly`·`rendererOrAgent` 중 하나이고, 기본값은 `rendererOnly`다 |
| 사용자 제스처 필요 함수 | `openUrl`처럼 `requiresUserActivation: true`인 함수는 `rendererOnly`여야 한다. 거부 코드는 `INVALID_FUNCTION_CALL`이다 |
| 카탈로그 혼용 | 컴포넌트마다 `@catalogId`를 단다. 없으면 화면의 기본 `catalogId`를 쓰고, 둘 다 없으면 그리지 않고 오류를 낸다 |
| 메시지 하나로 화면 생성 | `createSurface`에 `components`와 `dataModel`을 같이 넣는다. `dataModel`이 먼저 적용되고 그다음 `components`가 바인딩된다 |
| `@` 예약 키 | `@path`, `@call`, `@accessibility`, `@checks`, `@metadata`, `@catalogId`. 사용자 정의 컴포넌트가 `metadata` 같은 이름을 속성으로 쓸 수 있게 됐다 |
| 테마 제거 | 카탈로그의 `$defs/theme`·`primaryColor`와 `createSurface`의 `theme`이 빠졌다 |
| 접근성·검증 | 접근성 속성에 `live`(`off`/`polite`/`assertive`)와 `hidden`이 추가됐다. 검증 함수는 `valid`·`code`·`message`·`severity`를 담은 `ValidationResult`를 돌려준다. 카탈로그에 `allowedParents`·`allowedChildren`을 선언할 수 있다 |
| 용어 | client → **renderer**, server → **agent**. MIME은 `application/json+a2ui`에서 `application/a2ui+json`으로 바뀌었다 |
| 호환성 | 1.0 카탈로그에는 `"protocolVersion": "1.0"`이 필수다. 생략하면 `"0.9"`로 본다 |

PyTorchKR 글에 없는 변경도 가이드에 있다.

- `updateDataModel`의 `value`가 필수가 됐다. `null`을 주면 키를 지운다.
- `Text.variant`의 `h1`~`h5`가 빠졌다. 대신 Markdown 헤딩을 쓴다.
- `TextField.validationRegexp`가 빠지고 `@checks`의 `regex`로 옮겨 갔다.
- `Video.posterUrl`, `TextField.placeholder`, `Slider.steps`, 내장 함수 `@index`, `Action.event.userMessage`가 추가됐다.
- 오류 코드 `UNALLOWED_PARENT`·`UNALLOWED_CHILD`가 생겼다.

**옮길 때 주의할 점이 하나 있다.** 가이드는 일괄 치환을 경고한다. 원문: "Do not migrate v0.9 payloads with a blanket find-and-replace of" `"path":` → `"@path":`. `updateDataModel`의 `path`는 JSON Pointer 문자열이라 접두 없이 그대로 쓴다. 가이드 3장에는 역할별 체크리스트가 있다(For agents / For renderers / For catalog authors). 버전 상태는 [로드맵](https://a2ui.org/roadmap/) 표 기준으로 v1.0이 현재 버전이고 v0.8은 폐기됐다.

`openUrl`을 렌더러 전용으로 묶는 규칙은 보안상 의미가 있다. [10-04 브리핑](/ai-briefing/2026-10-04/)에서 다룬 `@a2ui/web_core` XSS(CVE-2026-10032, 0.10.2에서 수정)가 바로 에이전트가 준 `openUrl`에 `javascript:` URI가 들어가는 문제였다.

## 모델이 쓰는 형식과 앱이 받는 형식을 나눴다: A2UI Express

렌더러는 언제나 표준 A2UI JSON을 받는다. 바뀌는 것은 **모델이 쓰는 형식**뿐이다. Agent SDK가 모델 출력을 서버에서 JSON으로 컴파일하고 검증한다. 검증에 실패하면 오류를 모델에 돌려 고치게 할 수 있다.

JSON을 모델이 직접 쓰면 비용이 크다고 [추론 형식 문서](https://a2ui.org/concepts/inference-formats/)는 설명한다. 측정값이 아니라 설계상 추정이다.

- 키가 반복돼서 원문: "roughly three times longer than positional constructor calls."
- 시스템 프롬프트에 전체 스키마를 넣으면 원문: "roughly twice as many input tokens as compact component signatures."

Express는 생성자 호출을 중첩하고, `$/경로=값`으로 데이터 모델을 채운다. 컴파일러가 `root`, `root_child` 같은 ID를 결정적으로 붙인다. 아래는 [Fast, reliable A2UI](https://developers.googleblog.com/fast-reliable-a2ui/)(2026-10-27)의 예시다.

```
$/title="Enable notification"
root=Card(main_column)
main_column=Column([icon, title, description, actions], _, "center")
```

Express는 이제 기본 추론 형식이다. 평가 수치는 추론 형식 문서 기준이다. A2UI 팀이 직접 측정했고, `core_v1_0` 54개 샘플, `gemini-3.8-flash`, `temperature=0.0`의 중앙값이다.

| 형식 | 입력 토큰 | 생성 코드 토큰 | Pass@1 |
|---|---|---|---|
| JSON | 10,980 | 807 | 98.1% (53/54) |
| Express (사고 끔) | 6,000 (-45.4%) | 297 (-63.2%) | 98.1% (53/54) |
| Express (사고 무제한) | 6,000 (-45.4%) | 260 (-67.3%) | 확인하지 못함 |

- **사고를 켜면 절감 폭이 줄어든다.** 생각하는 토큰까지 합친 총 출력은 2,320 → 1,789로 -22.9%다.
- **지연:** "최대 3배" 감소는 Fast 글의 주장이다. 원문: "reduce latency by up to **3x**." 추론 형식 문서에는 지연 수치가 없다.
- **실험을 섞으면 안 된다.** Fast 글의 형식 선택 실험은 "51-task eval on gemini-3.5-flash"라 위 표와 다른 실험이다.

Direct JSON이나 직접 만든 형식으로 바꿔 끼울 수도 있다(`InferenceFormatFactory`). Python SDK에는 **매크로**도 있다. `@macro`를 붙인 함수로 `FlightCard` 같은 복합 컴포넌트를 정의하면, 모델은 한 줄만 쓰고 서버의 `MacroExpander`가 펼친다. [매크로 가이드](https://a2ui.org/guides/macros/)는 매크로 인자를 믿을 수 없는 모델 출력으로 보고 검증하라고 권한다.

## 렌더러와 Agent SDK 버전

레지스트리에서 2026-11-19에 직접 조회했다. PyTorchKR 글(11-13 기준) 이후 PyPI와 pub.dev 패키지가 11-17에 새로 나왔다.

| 구분 | 패키지 | 최신 버전 (게시일 UTC) | PyTorchKR 글 기준 |
|---|---|---|---|
| 웹 | `@a2ui/react` | 0.15.0 (11-02) | 0.15.0 |
| 웹 | `@a2ui/angular` | 0.14.0 (11-02) | 0.14.0 |
| 웹 | `@a2ui/lit` · `@a2ui/web_core` | 0.15.0 (11-02) | 0.15.0 |
| 모바일·데스크톱·웹 | `a2ui_flutter` | **0.4.0 (11-17)** | 0.3.0 |
| iOS | SwiftUI | 저장소 배포 | 같음 |
| Android | Jetpack Compose | 프로토콜 0.9.1 지원, 1.0은 "coming soon" | 같음 |
| Agent SDK | `a2ui-agent-sdk` (Python ≥3.10) | **0.12.0 (11-17)** | 0.11.0 |
| Agent SDK | `@a2ui/agent` (TypeScript) | 0.1.0 (11-02) | 0.1.0 |
| Agent SDK | `a2ui_agent` (Dart) | **0.4.0 (11-17)** | 0.3.0 |

- **"안정"은 와이어 프로토콜 이야기다.** SDK API는 0.x라 바뀔 수 있다.
- **새 기능이 패키지에 다 들어온 것은 아니다.** PyTorchKR 글은 11-13 시점에 `@accessibility`·`@checks`가 npm 0.15.0과 PyPI 0.11.0에 없고 CHANGELOG의 Unreleased에만 있다고 적었다. 11-17에 나온 PyPI 0.12.0·pub 0.4.0에 들어갔는지는 확인하지 않았다. npm 쪽은 11-02 이후 새 버전이 없다.
- **언어별 기능 차이:** 매크로는 Python만 된다. `SkillGenerator`는 Python과 TypeScript만 된다.
- **발표 글의 링크 오류:** Google 발표 글의 Dart SDK 링크는 PyPI(Python 패키지)를 가리킨다. Dart 패키지는 pub.dev의 `a2ui_agent`다.
- **Gemini Enterprise:** 조직 에이전트 응답은 지금 0.9.1로 렌더링되고, 1.0으로 옮기는 중이다.

## 먼저 쓰고 있는 곳

- **Atlassian Jira:** 작업 항목에 정보나 결정이 빠져 있으면 에이전트가 상호작용 카드를 띄운다. 팀은 Jira 안에서 바로 답한다.
- **Alation:** 지표 카드와 차트로 된 피드를 A2UI로 만든다. 차트마다 뒤의 SQL과 데이터를 보여 준다. 원문: "Alation already uses both to show its A2UI feeds in Claude." Claude 대화창과 Alation 앱에서 거의 같은 배치로 뜬다.
- **ByteDance Lynx:** 2026년 6월에 Lynx용 A2UI 렌더러를 냈다. 원문: "Lynx supports A2UI 0.9 today and is adapting to 1.0 with the A2UI team."
- **Gemini Notebook(Google 내부):** 슬라이드 덱을 요청하면 형식·초점·시각 스타일을 고르는 카드가 먼저 나온다. 생성 전에 고칠 수 있다.

PyTorchKR 글은 Bloomberg도 도입 기업으로 들었다. 발표 영상 속 "아픈 날 아침" 시나리오는 일러스트이고, A2UI가 그 일을 직접 처리하지는 않는다고 Google이 따로 밝혔다.

## MCP Apps와의 관계: 경쟁에서 조합으로

MCP Apps는 MCP 서버가 `ui://` 리소스로 HTML을 내놓고, 호스트가 그것을 샌드박스 iframe에 띄우는 [MCP 확장](https://modelcontextprotocol.io/extensions/apps/overview)이다. A2UI가 v0.8이던 시절에는 "샌드박스 HTML 대 네이티브 컴포넌트 청사진"으로 대비됐다. 6월 Google 글 [A2UI + MCP Apps](https://developers.googleblog.com/a2ui-and-mcp-apps/)(2026-06-17)는 MCP Apps 공동 창시자들과 함께 쓴 글이다. 이 글에서 둘을 묶는 방식을 세 가지로 정리했다.

| 패턴 | 구조 | 그리는 방식 |
|---|---|---|
| 1. A2UI over MCP | MCP 서버가 `application/a2ui+json`을 임베디드 리소스로 돌려준다. 정적 UI는 `resources/read`, 동적 UI는 `tools/call`로 온다 | iframe 없이 호스트가 네이티브로 |
| 2. A2UI 안의 MCP App | MCP App을 iframe으로 감싼 사용자 정의 A2UI 컴포넌트로 넣는다. App 이벤트는 A2UI 액션으로, 에이전트 응답은 데이터 모델 갱신으로 동기화한다 | 호스트 디자인 시스템 안에 iframe 하나 |
| 3. MCP App 안의 A2UI | MCP App 번들에 A2UI 렌더러를 넣고, 도구 호출로 받은 A2UI 메시지를 그린다 | iframe 속 웹 렌더러 |

1.0 발표 글이 따로 짚은 것은 3번이다. [A2UI in MCP Apps 가이드](https://a2ui.org/guides/a2ui-in-mcp-apps/)의 흐름은 이렇다.

1. MCP 서버가 A2UI 웹 렌더러를 담은 단일 HTML을 `ui://`로 공개한다(`mimeType: "text/html;profile=mcp-app"`). 도구는 `application/a2ui+json` 메시지를 담은 `EmbeddedResource`를 돌려준다.
2. 호스트가 `_meta.ui.resourceUri`를 보고 `resources/read`로 HTML을 가져와 격리된 iframe(`sandbox="allow-scripts"`)에 띄운다.
3. 안쪽 앱이 A2UI `MessageProcessor`로 메시지를 처리한다. 사용자 동작은 JSON-RPC `tools/call`로 서버에 돌아간다.

- **장점:** 원문: "without adding A2UI renderer code to the host application." 호스트가 A2UI를 몰라도 된다.
- **도구 가시성:** 진입 도구는 `_meta.ui.visibility: ["model"]`, 화면 안에서 부르는 도구는 `["app"]`으로 나눈다.
- **샌드박스 제약:** 호스트 CSP가 `connect-src 'none'`이다. 외부 이미지 같은 자원은 `_meta.ui.csp`로 도메인을 선언해야 한다. `localStorage` 같은 저장소 API는 `SecurityError`가 난다. 그래서 `vite-plugin-singlefile` 등으로 HTML 하나에 묶는다.
- **검증:** 서버에서 `validatePayload`(TypeScript·Python·Dart)로 메시지를 검증하라고 권한다.

한계도 분명하다.

- **네이티브가 아니다.** 3번 패턴의 화면은 호스트의 네이티브 컴포넌트가 아니라 iframe 속 웹 렌더러로 그려진다. A2UI의 "네이티브" 장점은 1번 패턴에서만 산다.
- **샘플이 아직 v0.8이다.** 가이드의 커뮤니티 샘플은 v0.8 메시지(`surfaceUpdate`, `beginRendering` 등)를 `@a2ui/angular` 0.10.x로 그린다. 워크스루만 v1.0·v0.9.1로 갱신됐다.
- **Flutter는 확인하지 못했다.** 발표 글은 원문: "web and Flutter renderers also work inside MCP Apps"라고 했다. 하지만 가이드와 6월 글은 웹(Lit) 예시만 보여 준다.
- **"Gemini"는 Gemini Enterprise다.** PyTorchKR 글과 발표 글은 MCP Apps 호스트로 Claude와 Gemini를 든다. 근거로 링크한 [Google Cloud 글](https://cloud.google.com/blog/topics/developers-practitioners/devs-guide-to-mcp-apps-in-gemini)(2026-10-20)이 다루는 것은 **Gemini Enterprise**의 MCP Apps 지원뿐이다. Gemini 앱이나 Gemini CLI 이야기는 없다. MCP Apps 개요의 공식 클라이언트 목록(Claude, Claude Desktop, VS Code GitHub Copilot, Microsoft 365 Copilot, Goose, Postman, MCPJam, Archestra.AI)에도 Gemini는 없다.

## 아직 로드맵에 있는 것

[로드맵](https://a2ui.org/roadmap/)과 README 기준이다.

- **Protocol v1.1:** 다음 버전 요구사항을 모으는 중이다.
- **결정 모델:** TypeSafe의 Jev 같은 결정 모델을 추론 형식으로 공식 지원한다. 원문: "which makes layout decisions up to 80x faster". 비교 대상은 토큰 단위로 컴포넌트 트리를 생성하는 방식이고, 미리 정의한 레이아웃 중에서 고르는 경우다. **측정 방법은 공개되지 않았다.** Fast 글이 직접 낸 수치는 jev2ui 샘플 11개 프롬프트에서 컴포넌트 트리 92~298ms, 전체 화면 0.8~1.7초다. 저자들은 이 수치에 "first look"이라는 단서를 달았다.
- **Sandboxed logic:** 컴포넌트에 로직을 안전하게 붙이는 방법이다. PyTorchKR 글에는 빠져 있다.
- **그 밖에:** 크로스 프레임워크 템플릿 엔진, 생성 UI 품질 평가 도구(지금은 Inspect AI 기반 데이터셋 채점), CLI 확장(지금은 `a2ui codegen`만), MCP App 상태 공유와 Gemini·Claude·ChatGPT·Teams·Slack 배포 가이드가 있다.

## 시작하기

```bash
# 코딩 에이전트에 A2UI 스킬 추가
npx skills add a2ui-project/a2ui

# Python Agent SDK (Python 3.10 이상, 2026-11-19 기준 0.12.0)
pip install a2ui-agent-sdk
```

- [퀵스타트](https://a2ui.org/quickstart/)는 Python 서버 + React 웹 + Flutter 모바일로 된 여행 계획 앱이다.
- 설치 없이 보려면 [A2UI Composer](https://a2ui-project.github.io/composer/)나 CopilotKit의 [A2UI Playground](https://go.copilotkit.ai/a2ui-playground)를 쓴다.
- 0.9에서 옮긴다면 진화 가이드 3장의 역할별 체크리스트부터 본다.

## PyTorchKR 글과 원 출처 대조

- **정확했던 것:** 1.0 변경 목록, `allowedCallers` 기본값, `protocolVersion` 기본값, `@path` 일괄 치환 경고, Express 수치, 패키지 버전(11-13 시점), Dart 링크 오류 지적. 모두 1차 출처와 맞았다.
- **빠진 것:** 명세가 AG-UI·A2A·MCP를 전송 바인딩으로 정의한다는 점, 로드맵의 v1.1과 Sandboxed logic, 진화 가이드의 세부 변경 9건, MCP Apps 가이드의 샘플이 아직 v0.8이라는 점.
- **좁혀 읽어야 하는 것:** MCP Apps 호스트로서의 "Gemini"는 Gemini Enterprise다. "80배"는 측정 방법이 공개되지 않은 로드맵 수치다.
- **그 뒤 바뀐 것:** PyPI `a2ui-agent-sdk` 0.12.0과 pub.dev `a2ui_flutter`·`a2ui_agent` 0.4.0이 11-17에 나왔다.

## 읽고 나서

- **핵심은 "코드가 아니라 데이터"다.** 에이전트가 HTML을 내려보내는 방식보다 할 수 있는 일이 좁다. 대신 앱이 카탈로그로 허용 범위를 정하니 보안과 브랜드 일관성을 지키기 쉽다. 1.0의 `allowedCallers`와 `requiresUserActivation` 규칙은 그 통제를 함수 호출까지 넓힌 것이다.
- **1.0이라는 숫자를 넓게 읽지 않는다.** 안정판은 와이어 형식뿐이다. SDK는 0.x이고, Android 렌더러와 Gemini Enterprise는 0.9.1에 머물러 있다. 지금 도입한다면 웹(React·Angular·Lit)과 Flutter가 1.0을 쓸 수 있는 범위다.
- **Express는 생성형 UI의 비용 문제를 정면으로 푼다.** 렌더러 쪽은 그대로 두고 모델 쪽 형식만 바꿔 토큰을 크게 줄였다. 다만 모든 수치가 A2UI 팀의 자체 측정이고, 모델 하나와 샘플 54개 기준이다.
- **MCP Apps 조합은 배포 범위를 넓히는 수단이다.** 네이티브 렌더링의 대안은 아니다. 호스트가 A2UI를 모를 때 iframe으로 띄우는 방식이라 샌드박스 제약을 그대로 받는다. 네이티브로 그리려면 호스트가 A2UI 렌더러를 직접 품어야 한다(1번 패턴).

## 참고

- PyTorchKR: [Google, 생성형 UI 프로토콜 A2UI 1.0 공개 (feat. MCP Apps)](https://discuss.pytorch.kr/t/google-ui-a2ui-1-0-feat-mcp-apps/12496)
- Google Developers Blog: [A2UI 1.0 발표](https://developers.googleblog.com/a2ui-10-build-delightful-fast-reliable-agentic-experiences/) (11-11) · [Fast, reliable A2UI](https://developers.googleblog.com/fast-reliable-a2ui/) (10-27) · [A2UI + MCP Apps](https://developers.googleblog.com/a2ui-and-mcp-apps/) (06-17)
- 명세·문서: [v1.0 명세](https://a2ui.org/specification/v1.0-a2ui/) · [진화 가이드](https://github.com/a2ui-project/a2ui/blob/main/specification/v1_0/docs/evolution_guide.md) · [추론 형식](https://a2ui.org/concepts/inference-formats/) · [매크로](https://a2ui.org/guides/macros/) · [A2UI in MCP Apps](https://a2ui.org/guides/a2ui-in-mcp-apps/) · [MCP Apps in A2UI](https://a2ui.org/guides/mcp-apps-in-a2ui/) · [로드맵](https://a2ui.org/roadmap/)
- 저장소: [a2ui-project/a2ui](https://github.com/a2ui-project/a2ui) (Apache 2.0)
- MCP Apps: [개요](https://modelcontextprotocol.io/extensions/apps/overview) · [Gemini Enterprise의 MCP Apps](https://cloud.google.com/blog/topics/developers-practitioners/devs-guide-to-mcp-apps-in-gemini) (10-20)
