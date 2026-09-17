---
title: "Claude Code 플러그인: 개념, 사용 중인 4종, 주목받는 플러그인"
date: 2026-09-17
weight: 5
tags: [claude-code, plugins]
description: "플러그인은 슬래시 커맨드·서브에이전트·스킬·훅·MCP 서버를 하나로 묶어 설치하는 Claude Code 확장 단위다. 개념과 동작 방식, 현재 쓰는 4종, 카테고리별로 주목받는 플러그인을 정리했다."
---

> **플러그인(Plugin)** 은 슬래시 커맨드 · 서브에이전트 · 스킬 · 훅 · MCP 서버를 하나의 패키지로 묶어 설치할 수 있는 Claude Code 확장 기능이다. 이 글은 플러그인의 개념과 동작 방식, 현재 사용 중인 플러그인 4종, 그리고 요즘 주목받는 플러그인들을 카테고리별로 정리한다.

## 플러그인이란?

예전에는 `~/.claude` 폴더에 커맨드나 훅을 직접 만들어 관리해야 했지만, 플러그인은 이것들을 하나의 패키지로 묶어 **명령 한 줄로 설치 · 공유 · 업데이트**할 수 있게 만든 공식 배포 단위다.

- **묶을 수 있는 것들**: 슬래시 커맨드, 서브에이전트, 스킬(Skills), 훅(Hooks), MCP 서버 — 다섯 가지를 자유롭게 조합
- **배포 방식**: 마켓플레이스(대부분 GitHub 저장소 형태)를 통해 배포. Anthropic 공식 마켓플레이스에는 200개 이상의 플러그인이 등록되어 있고, 커뮤니티 마켓플레이스도 다수 존재
- **켜고 끄기**: 설치 후에도 `/plugin` UI에서 활성/비활성을 전환할 수 있다 — 안 쓰는 플러그인은 꺼서 컨텍스트(토큰)를 아끼는 것이 좋다
- **팀 표준화**: 프로젝트 설정에 마켓플레이스와 플러그인을 지정하면 팀원 전체가 같은 커맨드 · 리뷰 규칙 · 워크플로우를 공유할 수 있다

## 플러그인 동작 방식

**패키지 구조** — 플러그인은 정해진 디렉터리 규약을 따르는 저장소다.

| 구성 요소 | 역할 |
|---|---|
| `.claude-plugin/plugin.json` | 이름 · 버전 · 설명 등 메타데이터 (매니페스트, 필수) |
| `commands/` | 슬래시 커맨드 정의 (마크다운 파일 1개 = 커맨드 1개) |
| `agents/` | 특정 작업 전용 서브에이전트 정의 |
| `skills/` | `SKILL.md` — Claude가 관련 작업을 만나면 자동으로 로드하는 지침 |
| `hooks/hooks.json` | SessionStart, PreToolUse 등 이벤트에 무조건 실행되는 스크립트 |
| `.mcp.json` | 외부 서비스 연결용 MCP 서버 설정 |

**설치 흐름**

1. 마켓플레이스 등록: `/plugin marketplace add owner/repo`
2. 플러그인 설치: `/plugin install 플러그인명@마켓플레이스명` — 또는 `/plugin` UI에서 탐색하며 설치
3. 적용 범위 선택: 개인 전역(user) 또는 해당 프로젝트(project)
4. 새 세션 시작 시 자동 로드 — 커맨드에는 `/플러그인명:커맨드` 형태로 네임스페이스가 붙는다 (예: `/commit-commands:commit`)

**실행 모델 — 세 가지 트리거**

- **커맨드**: 사용자가 직접 입력해야 실행된다 (명시적 호출)
- **스킬**: 작업 내용이 스킬 설명과 맞으면 Claude가 스스로 로드해서 지침을 따른다 (자동 호출)
- **훅**: 이벤트가 발생하면 모델 판단과 무관하게 무조건 실행된다 (결정적 동작 — ponytail 모드가 세션 시작마다 자동으로 켜지는 원리)
- MCP 서버가 포함된 플러그인은 설치만으로 외부 도구가 연결된다 (예: Context7, Playwright)

## 현재 사용 중인 플러그인 4선

### Superpowers — 개발 프로세스를 강제하는 워크플로우 킷 (75만+ 설치)

- 커뮤니티에서 가장 유명한 플러그인 중 하나. "바이브 코딩"을 막고 **브레인스토밍 → 계획 → TDD 구현 → 디버깅 → 코드 리뷰 → 브랜치 정리**라는 엔지니어링 프로세스 전체를 스킬로 강제한다
- 핵심 스킬: `brainstorming`(구현 전 요구사항 탐색), `writing-plans`, `test-driven-development`(RED-GREEN-REFACTOR), `systematic-debugging`(수정 전 원인 규명), `using-git-worktrees`(격리 작업 공간), `subagent-driven-development`(서브에이전트 병렬 실행)
- SessionStart 훅으로 "스킬이 1%라도 적용될 것 같으면 반드시 호출하라"는 규칙을 매 세션에 주입한다 — 모델이 절차를 건너뛰고 바로 코드부터 쓰는 것을 방지
- 큰 기능 개발이나 리팩터링처럼 절차가 중요한 작업에서 진가를 발휘한다

### claude-hud — 세션 상태를 보여주는 상태줄 HUD

- 터미널 하단 상태줄(statusline)을 게임 HUD처럼 바꿔주는 플러그인 — 현재 모델, 컨텍스트 사용량, 토큰/비용, git 브랜치 등 세션 상태를 실시간 표시
- `/claude-hud:setup` 으로 상태줄 등록, `/claude-hud:configure` 로 레이아웃 · 언어 · 표시 요소를 조정
- 컨텍스트 잔량이 눈에 보이므로 **컴팩션(요약) 타이밍을 예측**하고 긴 작업을 어디서 끊을지 판단하는 데 유용하다

### commit-commands — git 커밋 워크플로우 자동화 (Anthropic 공식)

- 반복적인 git 작업을 커맨드 하나로 줄여주는 공식 플러그인
- `/commit` — 변경사항을 분석해 적절한 커밋 메시지를 작성하고 커밋
- `/commit-push-pr` — 커밋 + 푸시 + PR 생성을 한 번에
- `/clean_gone` — 원격에서 삭제된 `[gone]` 상태의 로컬 브랜치와 연결된 워크트리를 일괄 정리

### ponytail — 오버엔지니어링을 막는 게으른 시니어 개발자

- "게으름 = 효율"이라는 시니어 개발자 페르소나를 훅으로 주입해, 항상 **동작하는 가장 단순한 해법**을 먼저 찾게 만든다
- 판단 사다리: 애초에 필요한 코드인가?(YAGNI) → 코드베이스에 이미 있나? → 표준 라이브러리로 되나? → 플랫폼 기본 기능으로 되나? → 기존 의존성으로 되나? → 한 줄로 되나? → 그제서야 최소한의 코드 작성
- 강도 조절: `/ponytail lite|full|ultra` (기본 full), 해제는 "stop ponytail"
- 부속 커맨드: `/ponytail-review`(diff 오버엔지니어링 리뷰), `/ponytail-audit`(저장소 전체 복잡도 감사), `/ponytail-debt`(의도적 단순화 주석을 부채 장부로 수집), `/ponytail-gain`(절감 효과 스코어보드)
- 의도적으로 단순화한 부분은 `ponytail:` 주석으로 남겨 "무지가 아니라 의도"임을 표시하는 규칙이 특징

## 요즘 핫한 플러그인

### 마케팅

#### Marketing — Anthropic 공식 마케팅 플러그인

- 콘텐츠 제작(블로그 · 소셜 · 이메일 뉴스레터 · 랜딩페이지 · 보도자료 · 케이스 스터디), 캠페인 기획, 브랜드 보이스 관리, 경쟁사 분석, 성과 리포트까지 커버
- SEO 감사와 이메일 시퀀스 설계 기능 포함 — 개발자가 아닌 직군도 Claude Code를 쓰게 만든 대표 사례

#### marketingskills (coreyhaines31) — 실무 마케터의 스킬 모음

- 실제 SaaS 마케터가 만든 스킬 팩 — CRO(전환율 최적화), 카피라이팅, SEO, 애널리틱스, 그로스 엔지니어링
- 랜딩페이지 카피 개선, 가격 페이지 분석처럼 "코드 + 마케팅"이 겹치는 작업에 강하다

#### SEO 감사 계열 플러그인

- 기술 SEO(스키마 마크업, Core Web Vitals), 콘텐츠 품질, 백링크, 로컬 SEO, AI 검색 최적화까지 점검하는 감사 플러그인들이 다수 등장
- 키워드 밀도 분석, 글자수 검증이 붙은 메타 타이틀/디스크립션 생성, featured snippet용 포맷팅 등 실무 기능 중심

### 프론트엔드

#### frontend-design — Anthropic 공식, 설치 수 1위 (83만+)

- "AI가 만든 티 나는" 템플릿형 UI를 벗어나 의도가 담긴 디자인을 만들도록 미적 방향 · 타이포그래피 · 색 선택을 가이드
- 새 UI를 만들거나 기존 화면을 리디자인할 때 자동으로 개입하는 스킬 형태 — 전체 플러그인 중 설치 수 1위 (2026년 6월 기준)

#### Figma — 디자인 파일을 코드로

- 파트너 플러그인. Figma 디자인 파일과 디자인 토큰을 읽어 컴포넌트 코드로 변환하는 design-to-code 워크플로우를 제공

#### Playwright — 브라우저 자동화 · E2E 검증 루프

- MCP 서버 포함 플러그인. Claude가 실제 브라우저를 열어 클릭 · 입력 · 스크린샷을 수행
- "코드 수정 → 브라우저에서 직접 확인 → 다시 수정"하는 자가 검증 루프를 만들 수 있어 프론트엔드 개발에서 특히 인기

#### Vercel — 배포 연동

- 파트너 플러그인. 배포, 프리뷰 URL 확인, 배포 로그 분석까지 Claude Code 안에서 처리

### 이미지 · 동영상

#### Remotion — React로 동영상을 프로그래매틱 생성 (공식 플러그인)

- 타임라인 편집기 대신 **React 컴포넌트와 frame 변수**로 영상을 만드는 프레임워크. 공식 플러그인이 Remotion 프로젝트 생성 · 편집용 Agent Skills를 제공
- 프롬프트 → 렌더링된 MP4까지 한 시간 이내 — 로고 리빌, 모션그래픽, 광고 영상 제작에 활용
- CSS · SVG · Canvas · WebGL · Three.js를 컴포지션 안에서 그대로 사용 가능
- 설치: `claude plugin marketplace add remotion-dev/claude-code-plugin` 후 `claude plugin install remotion@remotion`
- 정적 이미지를 터미널에 드래그해 넣으면 그 디자인을 Remotion 컴포지션으로 재현시킬 수도 있다

#### canvas-design — 포스터 · 그래픽 시각물 디자인 (Anthropic 공식 스킬)

- Anthropic 공식 스킬 저장소에서 배포. 포스터, 다이어그램, 그래픽 아트 같은 시각물을 코드 기반으로 디자인하도록 돕는다

#### 이미지 생성 MCP 연동 플러그인들

- Gemini · OpenAI 등의 이미지 생성 API를 MCP 서버로 연결해, Claude Code 안에서 에셋 생성 → 코드에 바로 반영하는 워크플로우가 유행
- 프론트엔드 목업용 플레이스홀더 이미지, 블로그 썸네일 자동 생성 등에 활용된다

### 문서 작성

#### Technical Docs Plugin (danielrosehill) — 기술 문서 전반

- README 작성, 레퍼런스 문서, 체인지로그, 테크스택 문서화, 마크다운 변환, 수정 노트 아카이빙까지 기술 문서 워크플로우 전체를 커버
- api-reference / code-docs / environment-docs / dev-notebook 등 용도별 변형 버전 제공

#### doc-driven-development (cbrake) — 문서 주도 개발

- 문서를 먼저 쓰고, 코드가 문서를 따라가게 하는 워크플로우 플러그인
- 문서가 항상 최신 · 완전한 상태로 유지되고, 문서 작성이 설계 단계의 사고 도구가 된다 — "나중에 하는 귀찮은 일"이 아니게 만드는 접근

#### claude-md-management — CLAUDE.md 관리 (Anthropic 공식)

- 저장소의 CLAUDE.md 파일들을 감사해 품질 리포트를 만들고 개선하는 플러그인
- `/revise-claude-md` 로 세션에서 배운 내용을 CLAUDE.md에 반영 — 프로젝트 메모리를 문서로 축적하는 용도

### 개발 워크플로우 · 기타

#### Context7 (Upstash) — 최신 라이브러리 문서 주입 (35만+ 설치)

- 라이브러리 · 프레임워크의 **최신 공식 문서**를 컨텍스트에 실시간으로 가져온다
- 학습 데이터가 오래돼 생기는 구버전 API 환각(hallucination)을 방지 — 사실상 필수 플러그인 취급을 받는다

#### Claude Mem — 세션 간 영구 메모리

- 세션이 끝나도 대화 내용 · 결정 사항을 요약 저장했다가 다음 세션에 자동으로 불러온다 — 매번 프로젝트 맥락을 다시 설명할 필요가 없어진다

#### Caveman — 토큰 절약 간결 모드

- 응답을 극단적으로 짧고 간결하게 바꿔 토큰을 아끼는 출력 스타일 플러그인 — "무엇을 만들지"를 다루는 ponytail과 짝으로 쓰면 좋다 (ponytail은 코드, Caveman은 말투)

#### code-review / feature-dev / security-guidance — Anthropic 공식 개발 3종

- `code-review`: 멀티에이전트가 diff를 여러 관점에서 검토하고 검증까지 거쳐 확정된 결함만 보고
- `feature-dev`: 코드베이스 이해 → 아키텍처 설계 → 구현 → 리뷰로 이어지는 가이드형 기능 개발
- `security-guidance`: 변경사항의 보안 취약점을 점검하는 시큐리티 리뷰

#### 파트너 플러그인 생태계

- GitHub, Supabase, Linear, Sentry, Stripe, Firebase 등 68개 이상의 파트너 플러그인이 공식 마켓플레이스에 등록
- 대부분 MCP 서버 + 전용 커맨드 조합으로, 해당 서비스를 Claude Code 안에서 직접 조작하게 해준다

---

> **설치 팁** — 플러그인은 많이 깔수록 좋은 게 아니다. 스킬 · 훅이 늘어날수록 세션 시작 시 컨텍스트를 차지하므로, 실제로 쓰는 것만 활성화하고 나머지는 `/plugin` UI에서 꺼두는 것이 좋다. 공식 마켓플레이스의 276개 중 실제 세션 경험을 바꾸는 것은 소수라는 평가도 있다.

## 참고

- [Claude Code 공식 문서 — 플러그인 탐색과 설치](https://code.claude.com/docs/en/discover-plugins)
- [Claude Code 공식 문서 — 플러그인 레퍼런스](https://code.claude.com/docs/en/plugins-reference)
- [Anthropic 공식 플러그인 저장소](https://github.com/anthropics/claude-code/blob/main/plugins/README.md)
- [Best Claude Code Plugins 2026 — designrevision](https://designrevision.com/blog/best-claude-code-plugins)
- [Top Claude Code Plugins — Composio](https://composio.dev/content/top-claude-code-plugins)
- [Trending Claude Plugins, June 2026 — claudepluginhub](https://www.claudepluginhub.com/blog/trending-claude-plugins-june-2026)
- [Remotion Claude Code 플러그인 공식 문서](https://www.remotion.dev/docs/ai/claude-code-plugin)
- [Marketing 플러그인 — claude.com](https://claude.com/plugins/marketing)
- [marketingskills — GitHub](https://github.com/coreyhaines31/marketingskills)
- [Technical Docs Plugin — GitHub](https://github.com/danielrosehill/Claude-Technical-Docs-Plugin)
- [doc-driven-development — GitHub](https://github.com/cbrake/claude-plugins/blob/main/doc-driven-development/README.md)
