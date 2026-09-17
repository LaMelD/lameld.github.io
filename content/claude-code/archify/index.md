---
title: "archify: 에이전트가 그리고 검증까지 하는 아키텍처 다이어그램 스킬"
date: 2026-09-17
weight: 2
tags: [claude-code, codex, antigravity, archify, diagram]
description: "tt-a1i/archify는 코드베이스나 시스템 설명을 검증된 단일 HTML 다이어그램으로 바꾸는 에이전트 스킬이다. 무엇을 만들고 어떻게 검증하는지, Claude Code·Codex·agy에 설치하고 지우는 법을 실제로 돌려 보고 정리했다."
---

저장소: [github.com/tt-a1i/archify](https://github.com/tt-a1i/archify) · [프로젝트 페이지](https://tt-a1i.github.io/archify/) · MIT · 안정판 v2.16.0(2026-08-30), main은 2.17.0-dev.1

"archify 플러그인"으로 찾았지만 실체는 **에이전트 스킬**입니다. `SKILL.md`와 Node.js 렌더러·검증기가 한 디렉터리에 들어 있고, Claude Code 마켓플레이스 플러그인(`.claude-plugin/`)도 npm 패키지도 아닙니다. `SKILL.md`를 읽는 에이전트라면 Claude Code, Codex, Cursor, OpenCode, Antigravity 어디서든 같은 폴더를 그대로 씁니다.

## 무엇을 하는가

코드베이스나 시스템 설명을 대화 안에서 인터랙티브 시스템 맵으로 바꿉니다. 결과는 HTML 파일 하나입니다.

| 타입 | 용도 | 프롬프트에 넣을 것 |
|---|---|---|
| `architecture` | 컴포넌트, 서비스, 저장소, 클라우드·보안 경계 | 범위, 핵심 컴포넌트, 주 경로 |
| `workflow` | CI/CD, 승인 게이트, 도구 호출, 런북 | 참여자, 순서, 분기, 예외 |
| `sequence` | API 호출 체인, 캐시 폴백, 인증, 비동기 추적 | 호출자·피호출자, 반환, 타이밍 |
| `dataflow` | 파이프라인, ETL/ELT, 리니지, PII 경계 | 소스, 변환, 저장소, 경계 |
| `lifecycle` | 상태 전이, 재시도, 대기, 종료 상태 | 상태, 이벤트, 재시도·취소 경로 |

산출물과 뷰어:

- **HTML 하나에 뷰어 내장.** 다크/라이트, 노드 검색, 포커스, 업스트림·다운스트림 추적, 두 노드 사이 경로 탐색, 역할 비교, 안내 스토리, 프레젠테이션 모드. `demo`로 만든 샘플이 812KB입니다.
- **내보내기.** PNG/JPEG/WebP/SVG/WebM과 1200×630 공유 카드. 추적한 경로(Route)나 도달 범위(Reach)만 담은 카드도 뽑습니다.
- **Architecture Delta.** 검증된 스냅샷 둘을 Before / Delta / After로 비교해 추가·제거·변경·이동·재라우팅 사실을 영수증과 함께 냅니다. PR 리뷰용입니다.
- **Mermaid 입력.** `flowchart`, `sequenceDiagram`, `stateDiagram`을 읽어 토폴로지와 의미만 가져오고 새로 작성합니다. Mermaid 스타일을 그대로 렌더하지 않습니다.
- **저장소 증거.** 요청할 때만 노드에 `SRC n` 배지가 붙고, 특정 커밋에 고정된 파일·라인 범위를 엽니다.

## 왜 만들었는가

`PRODUCT.md`에 적힌 목적은 하나입니다. 엔지니어, 아키텍트, 리뷰어, 그리고 AI 코딩 에이전트가 호스팅 다이어그램 에디터를 도입하지 않고도 검사하고 발표하고 공유할 수 있는 **신뢰할 수 있는 산출물**을 대화 안에서 얻는 것입니다.

핵심 원칙은 "Truth before spectacle"입니다. 포커스, 도달 범위, 경로, 스토리, 소스 링크, 영수증 전부가 작성된 사실이나 검증된 증거에서만 나옵니다. 토폴로지를 지어내지 않고, 런타임 영향이나 머지 안전성을 주장하지 않습니다.

명시적으로 피하는 것도 적혀 있습니다. 테마만 바꾸는 Mermaid 미화기, 편집 UI가 제품이 되어 버린 WYSIWYG 도구, 없는 관계를 암시하는 모션 데모, 유리 효과·그라데이션 글자 같은 AI 생성 UI 클리셰. 그래서 Mermaid 자동 파싱, 범용 자동 레이아웃, 호스팅 공유, WYSIWYG 편집은 의도적으로 범위 밖입니다.

출발점은 Cocoon-AI의 architecture-diagram-generator(MIT)이고, 현재 형태는 별개의 렌더링·검증 시스템입니다.

## 어떻게 동작하는가

에이전트가 그림을 그리는 게 아닙니다. 에이전트는 **타입이 있는 JSON**을 쓰고, archify가 그것을 결정론적으로 HTML/SVG로 컴파일합니다.

| 단계 | 하는 일 |
|---|---|
| Generate | 에이전트가 요청을 읽고 JSON IR을 작성 |
| Validate | 스키마, 레이아웃, HTML/SVG, 경로, 라벨-경로 간격 검사. 실패하면 고칠 지점을 JSON으로 지목 |
| Preview (선택) | 루프백 전용 데스크톱 세션이 JSON 하나를 감시하고, 검증을 통과한 리비전만 다시 그림 |
| Deliver | 같은 디렉터리에 후보를 렌더·검사하고, 통과한 것만 원자적으로 목표 파일과 교체. 사양과 산출물의 SHA-256·바이트 수를 영수증으로 남김 |
| Iterate | 에이전트가 JSON을 고치고, 관련 없는 구조는 그대로 유지 |

`SKILL.md`가 에이전트에게 강제하는 규칙이 이 도구의 성격을 결정합니다.

1. 타입을 고르면 스키마 하나, 공통 스키마, 예제 하나만 읽습니다. 렌더러·검증기 소스는 첫 후보를 만들기 전에 읽지 못합니다.
2. 좌표를 산문으로 계획하지 말고 후보 JSON부터 씁니다. 주 경로 하나, 짧은 곁가지, 주요 노드 12개 이하. `meta.quality_profile`은 `showcase`.
3. 편집할 때마다 `validate`. showcase 통과는 9개 검사 전부 0 error, 0 warning입니다. 실패하면 진단이 지목한 `subject`만, 제시된 `supportedFixes` 안에서만 고칩니다. 두 라운드 연속으로 오류 수가 줄지 않으면 멈추고 남은 진단을 그대로 보고합니다.
4. 최종 인수는 `deliver`. 종료 코드가 0이 아니면 성공이라고 말할 수 없습니다. 실패한 배달은 이전 산출물을 보존합니다.
5. 배달 후 `visual-check`로 브라우저 증거를 모읍니다. 결정론적 검사(`deliver`), 브라우저 동작 증거(`visual-check`), 사람의 시각 검토는 별개의 주장으로 분리해 보고합니다.

```sh
cd ~/.agents/skills/archify
node bin/archify.mjs doctor
node bin/archify.mjs guide "Show an API request with Redis cache miss"
node bin/archify.mjs validate workflow examples/agent-tool-call.workflow.json --quality showcase --json
node bin/archify.mjs deliver  workflow examples/agent-tool-call.workflow.json /tmp/workflow.html --quality showcase --json
node bin/archify.mjs visual-check /tmp/workflow.html --json
node bin/archify.mjs compare architecture base.json head.json delta.html --json
```

에이전트에게는 이렇게 말하면 됩니다. 저장소가 없어도 됩니다.

```text
Use Archify to draw: Browser -> API -> Redis cache -> PostgreSQL fallback.
```

```text
이 저장소를 분석한 뒤 archify로 런타임 아키텍처 다이어그램을 만들어라.
핵심 컴포넌트 8~12개, 주 경로 하나, 외부 의존성, 신뢰 경계를 보여 주고
세부 설명은 엣지를 늘리지 말고 카드에 넣어라.
```

이후 "Redis 추가해", "auth를 왼쪽으로", "롤백 경로 강조해" 같은 요청으로 JSON을 고쳐 갑니다.

### 패키지와 네트워크

스킬 폴더는 8.4MB이고 `bin/`, `schemas/`, `renderers/`, `examples/`, `references/`, `recipes/`, `brand-marks/`, `delta/`가 들어 있습니다. Node.js 18 이상만 있으면 되고 `npm install`은 필요 없습니다. `doctor`가 Node 버전, 템플릿, 다섯 렌더러와 스키마, 뷰어 런타임을 점검합니다.

네트워크는 한 곳뿐입니다. 첫 후보를 만든 뒤 `scripts/check-update.mjs`가 고정된 manifest를 GET해 새 버전이 있는지 알립니다. 약 72시간 간격이고, 버전·프로젝트 데이터·프롬프트·계정 정보는 보내지 않으며, 다운로드나 설치는 절대 하지 않습니다. 끄려면 `ARCHIFY_UPDATE_CHECK_DISABLED=1`입니다.

## 설치

설치 도구는 Vercel의 `skills` CLI(`npx skills`)입니다. 저장소를 clone해 `archify/` 폴더를 에이전트의 스킬 경로에 넣습니다. 저장소에는 스킬이 두 개(`archify`, 기여자용 `archify-review`) 있으니 `--skill archify`로 고릅니다. 아래 결과는 macOS, Node 22.14, skills CLI 1.5.18, Claude Code 2.1.274, Codex 0.154.0, agy 1.2.4에서 HOME을 격리해 실제로 설치·삭제해 확인한 것입니다.

먼저 세 CLI가 스킬을 읽는 경로입니다. 설치 명령의 결과가 이 표와 맞아야 잡힙니다.

| CLI | 프로젝트 | 전역 |
|---|---|---|
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| Codex | `.agents/skills/` (cwd와 저장소 루트) | `~/.agents/skills/`, `~/.codex/skills/` |
| agy (Antigravity CLI) | `.agents/skills/` | `~/.gemini/config/skills/` |

### 전역 설치

```sh
npx skills add tt-a1i/archify --skill archify -g -a claude-code -a codex -a antigravity-cli -y
```

결과는 두 경로입니다. 원본이 `~/.agents/skills/archify`에 놓이고, `~/.claude/skills/archify`는 그곳을 가리키는 심볼릭 링크입니다. Codex는 `~/.agents/skills`를 직접 읽으므로 이걸로 끝이고, Claude Code는 링크를 따라갑니다.

**agy는 한 줄이 더 필요합니다.** skills CLI는 이 경로를 "universal: Codex, Antigravity CLI"라고 표시하지만, agy 1.2.4는 전역 스킬을 `~/.gemini/config/skills/`에서만 찾습니다. [공식 문서](https://antigravity.google/docs/skills)와 바이너리 문자열이 같은 경로를 가리킵니다.

```sh
mkdir -p ~/.gemini/config/skills
ln -s ~/.agents/skills/archify ~/.gemini/config/skills/archify
```

링크 대신 `~/.gemini/config/skills.json`의 `entries`에 `{ "path": "~/.agents/skills" }`를 등록해도 됩니다.

심볼릭 링크가 싫으면 `--copy`를 붙입니다. `~/.claude/skills/archify`와 `~/.agents/skills/archify`에 각각 8.4MB 복사본이 생기고, archify 공식 시작 페이지가 안내하는 명령도 이 형태입니다. 대신 업데이트할 곳이 둘이 됩니다.

### 프로젝트 설치

같은 명령에서 `-g`만 뺍니다.

```sh
npx skills add tt-a1i/archify --skill archify -a claude-code -a codex -a antigravity-cli -y
```

`./.agents/skills/archify`에 원본, `./.claude/skills/archify`에 링크, 그리고 `./skills-lock.json`이 생깁니다. 세 CLI 모두 프로젝트의 `.agents/skills/`를 읽으므로 agy도 추가 작업이 없습니다. 8.4MB를 저장소에 커밋할지는 정해야 합니다.

### 확인

```sh
npx skills ls -g
node ~/.agents/skills/archify/bin/archify.mjs doctor
node ~/.agents/skills/archify/bin/archify.mjs demo /tmp/archify-demo
```

`doctor`가 `Archify is ready.`로 끝나고 `demo`가 `archify-demo.html`을 만들면 됩니다. 에이전트는 새 세션에서 스킬을 잡습니다. agy는 `/skills reload`로 세션을 유지한 채 다시 읽습니다.

설치 없이 한 번만 써 보려면:

```sh
npx skills use tt-a1i/archify@archify --agent claude-code
npx skills use tt-a1i/archify@archify --agent codex
```

### 버전

`npx skills add`는 main 브랜치를 clone하므로 개발 채널(2.17.0-dev.1)이 설치됩니다. 안정판이 필요하면 [릴리스](https://github.com/tt-a1i/archify/releases/latest)의 `archify.zip`을 풀어 위 경로에 두면 됩니다. Claude.ai 웹은 이 zip을 Settings → Capabilities → Skills에 올립니다. 업데이트는 `npx skills update archify -g -y`입니다.

skills.sh가 붙이는 보안 평가는 설치 시점에 Gen Safe, Socket 2 alerts, Snyk Low Risk였습니다. 스킬은 에이전트 권한으로 실행되므로 `SKILL.md`와 `bin/`은 한 번 읽어 두는 편이 좋습니다.

## 삭제

```sh
npx skills remove archify -g -y   # 전역
npx skills remove archify -y      # 프로젝트 (프로젝트 루트에서)
```

직접 돌려 보고 확인한 주의점 세 가지입니다.

1. `-a`로 에이전트를 지정하면 Claude Code 링크만 지워지고 원본 `.agents/skills/archify`가 남습니다. 에이전트 지정 없이 실행해야 원본까지 지워집니다.
2. `skills-lock.json`과 `~/.agents/.skill-lock.json`의 항목은 남습니다. 손으로 지우거나 무시합니다.
3. agy용으로 만든 링크는 CLI가 모릅니다. `rm ~/.gemini/config/skills/archify`로 따로 지웁니다.

CLI 없이 지우려면 아래를 전부 지우면 끝입니다. 설정 파일에 등록되는 것은 없습니다.

```sh
rm -rf ~/.agents/skills/archify ~/.claude/skills/archify ~/.gemini/config/skills/archify
rm -rf .agents/skills/archify .claude/skills/archify   # 프로젝트 설치분
```

## 참고

- [tt-a1i/archify](https://github.com/tt-a1i/archify) — README, `archify/SKILL.md`, `PRODUCT.md`
- [Archify Proof Lab](https://tt-a1i.github.io/archify/gallery.html) — 검증 영수증이 붙은 예제 11개
- [vercel-labs/skills](https://github.com/vercel-labs/skills) — 설치 CLI와 에이전트별 경로 표
- [Codex skills 문서](https://developers.openai.com/codex/skills) — `.agents/skills`, `~/.agents/skills` 탐색 경로
- [Antigravity skills 문서](https://antigravity.google/docs/skills) — `~/.gemini/config/skills/`
