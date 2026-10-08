---
title: "2026-10-09 AI 브리핑"
date: 2026-10-09T01:00:00+09:00
tags: [ai-briefing, anthropic, openai, google, security]
description: "Anthropic이 GPT-6 Luna와 같은 가격($0.10/$0.50)의 Claude Haiku 5.5를 내고 OpenAI는 ChatGPT 기본 모델을 GPT-6로 바꿔 Intelligent UI를 도입했으며, OpenAI 수학 원고 3편이 철회됐고, CrowdStrike가 한국 은행 공격자의 AI 도구 ARTEX와 Claude Code 로그를 공개했으며, Tensorlake npm 웜이 .claude/settings.json을 전파 경로로 썼다."
---

> 조사 범위: 2026-10-08 01:00 ~ 2026-10-09 01:00 KST(2026-10-07 16:00 ~ 10-08 16:00 UTC, 직전 브리핑 조사 창 종료 시각 이후). 시각은 기사 `article:published_time`·JSON-LD `datePublished`·RSS `pubDate`·GitHub `published_at`/커밋 시각·npm/PyPI 게시 시각·OpenRouter `created`·HN Algolia `created_at`·GHSA `published_at`·상태 페이지 API·fxtwitter `created_at`으로 검증했다. HN 점수는 조회 시점(10-08 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **Anthropic이 Claude Haiku 5.5를 냈다.** 1M 컨텍스트에 입력 $0.10·출력 $0.50(1M 토큰당)으로 GPT-6 Luna와 가격이 같고, 자체 수치로는 OSWorld 2.1 72.4%·Terminal-Bench 4.0 39.2%로 앞선다. Sonnet 5.5 캐시 읽기도 절반($0.10)으로 내렸고 Max·Team 구독자에게 월 API 크레딧을 준다. 다만 `budget_tokens`·prefill·비기본 temperature가 오류가 나는 등 마이그레이션 변경이 많다.
- **OpenAI는 ChatGPT 기본 모델을 GPT-6(유료 Sol·무료 Luna)로 바꾸고 차트·폼·미니 도구가 섞이는 "Intelligent UI"를 넣었다.** 이틀 전 공개한 모델 생성 수학 원고는 부호 오류로 3편이 철회되고 14편이 수정됐다.
- **에이전트가 공격 도구이자 공격면이 된 하루였다.** CrowdStrike는 한국 은행 공격자가 오픈소스 에이전트 도구 ARTEX와 Claude Code를 썼다며 세션 로그를 공개했다. Tensorlake npm 웜은 피해 저장소에 `.claude/settings.json`을 심어 퍼졌고, Ollama(root RCE)·LMCache(비인증 RCE, 패치 없음)·SGLang·Langflow 권고와 Bedrock AgentCore 계정 전체 장악 연구가 이어졌다.

## 모델 소식

### 1. Anthropic Claude Haiku 5.5 출시 — Haiku 4.5보다 약 90% 싼 1M 컨텍스트 소형 모델 + Sonnet 5.5 캐시 읽기 인하·구독자 API 크레딧

새 모델 ID는 `claude-haiku-5-5`다. 컨텍스트 1M, 최대 출력 128K이고, Haiku 계열로는 처음으로 adaptive thinking과 effort 파라미터를 지원한다(기본 effort `medium`, 지식 컷오프 2026년 6월). 가격은 1M 토큰당 입력 $0.10·출력 $0.50·캐시 읽기 $0.01이고, 프롬프트가 10만 토큰을 넘으면 입력 $0.50·출력 $2.50이다. 10만 이하 요청 기준으로 Haiku 4.5보다 90% 싸고, 새 토크나이저(같은 텍스트가 약 30% 더 많은 토큰)를 반영한 실제 작업 기준으로는 평균 약 75% 싸다고 Anthropic은 밝혔다.

| 벤치마크(자체 발표) | Haiku 5.5 | Haiku 4.5 | GPT-6 Luna |
|---|---|---|---|
| OSWorld 2.1 | 72.4% | 15.7% | 48.9% |
| Terminal-Bench 4.0 | 39.2% | 0.0% | 16.4% |
| HLE(도구 없음) | 45.9% | 10.2% | — |
| GDPval-AA v2.1 | 1620 | 735 | 1437 |

**왜 중요한가:** 가격이 직전 브리핑에서 다룬 OpenAI GPT-6 Luna($0.10/$0.50)와 정확히 같아졌고, 자체 수치로는 컴퓨터 사용·터미널 작업에서 크게 앞선다. 분류·서브에이전트·브라우저 자동화처럼 호출이 많은 작업의 기본 모델 선택이 바뀔 수 있다.

**⚠️ 마이그레이션 주의(공식 What's new 문서):**
- `budget_tokens`를 쓰면 400 오류가 난다.
- temperature·top_p·top_k를 기본값이 아닌 값으로 주면 오류가 난다.
- assistant prefill도 오류가 난다.
- computer use 도구 이름이 `computer_toolset_20260801`로 바뀌었다.
- adaptive thinking이 기본으로 켜져 응답이 thinking 블록으로 시작할 수 있다. 첫 블록을 답으로 읽는 코드는 type으로 골라야 한다.
- `stop_reason: "refusal"` 처리가 필요하다.

Claude API, Amazon Bedrock(GovCloud 포함), Google Cloud, Microsoft Foundry에서 쓸 수 있고, OpenRouter에는 10-07 18:31 UTC에 올라왔다. Haiku 4.5의 은퇴 예정일은 "2026-10-15 이후"로 표기됐다.

같은 발표에 부수 변경이 세 가지 묶였다.
- **Sonnet 5.5 캐시 읽기 인하:** $0.20에서 $0.10이 됐다. Anthropic은 에이전트 작업 비용이 약 20% 준다고 밝혔다.
- **구독자 월 API 크레딧:** Max 5x는 $100, Max 20x는 $200, Team은 사용자 합산 최대 $500이다.
- **SDK 툴셋 클래스 베타:** Python·TypeScript SDK에 browser use·computer use 툴셋 클래스가 들어갔다(도구 섹션 2번).

[발표](https://www.anthropic.com/claude-haiku-5-5) · [What's new](https://platform.claude.com/docs/en/models/haiku-5-5/whats-new-haiku-5-5) · [릴리스 노트](https://platform.claude.com/docs/en/release-notes/overview) · [AWS](https://aws.amazon.com/about-aws/whats-new/2026/10/claude-haiku-5-5-aws/) · [HN 985점](https://news.ycombinator.com/item?id=49996437)
(@claudeai 트윗 10-07 18:01 UTC, HN 첫 게시 17:56 UTC. 발표 페이지에는 날짜만 있다. 공식. 벤치마크와 절감률은 자체 발표)

### 2. OpenAI, ChatGPT 기본 모델을 GPT-6로 교체하고 "Intelligent UI" 도입

ChatGPT 답변에 다이어그램·차트·폼·버튼, 계산기 같은 인라인 미니 도구가 섞여 나오기 시작했다. 유료 플랜은 GPT-6 Sol, Free·Go 플랜은 GPT-6 Luna를 쓴다. Plus·Pro·Business·Enterprise는 10-07부터, Free·Go는 10-08부터 배포됐다. OpenAI는 생각하는 도중 부분 답을 먼저 내보내 대기 시간이 44% 줄었고, 어려운 웹 검색에서 GPT-5.6보다 낫다고 주장했다(매체 전언). API changelog에는 같은 날 `chat-latest` 스냅샷 갱신이 올라왔다.

**왜 중요한가:** 무료 사용자까지 포함한 ChatGPT 전체의 기본 모델과 응답 형식이 한꺼번에 바뀌었다. `chat-latest`에 의존하는 API 사용자는 출력 동작이 달라질 수 있으니 확인이 필요하다.

[OpenAI 발표](https://openai.com/index/gpt-6-for-everyone) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1007276/openai-chatgpt-intelligent-ui-gpt-6) · [TechCrunch](https://techcrunch.com/2026/10/07/chatgpt-is-getting-a-lot-more-visual-with-the-launch-of-a-new-interface/) · [API changelog](https://developers.openai.com/api/docs/changelog) · [HN 720점](https://news.ycombinator.com/item?id=49996425)
(HN 첫 게시 10-07 18:00 UTC, TechCrunch 18:00 UTC. openai.com 본문은 403이라 RSS와 매체로 확인. 공식+매체보도. 44% 수치는 자체 주장)

### 3. OpenAI 수학 원고 후속 — 공개 이틀 만에 3편 철회·14편 수정, 수학계 반발 확산 (직전 브리핑 후속)

직전 브리핑에서 다룬 모델 생성 수학 원고 저장소의 `history.md`가 갱신됐다. "Algebraicity of Weil classes on split abelian eightfolds"에서 부호 오류가 발견돼, 이 원고와 그 구성을 쓰는 의존 원고 2편(Kuga–Satake 대응, K3 곱의 유리 Hodge 추측)이 철회됐다. 다른 원고 14편은 증명을 보수했고, 13편은 인용만 고쳤다. Lean 형식화는 6건이 늘어 주결과 719개 중 300개(약 42%)가 됐다. 반응도 커졌다. Association for Human Mathematics(AHM)는 700여 편 일괄 공개를 "학문이 아니라 힘의 과시"라 부르며 협업 중단을 촉구했다. 같은 창에 Terence Tao의 "Math 2.0"(HN 504점), Scott Aaronson의 "The Mathocalypse"(HN 331점)도 나왔다.

**왜 중요한가:** 직전 브리핑이 짚은 "형식화되지 않은 원고의 오류율" 우려가 공개 이틀 만에 실제 철회로 확인됐다. 모델이 만든 대량의 증명을 누가 검증하고 책임지는지에 대한 논쟁이 구체적인 사례를 얻었다.

[history.md](https://github.com/openai/math/blob/main/history.md) · [AHM 성명](https://www.ahmath.org/statements) · [HN 323점](https://news.ycombinator.com/item?id=50003107) · [Tao 글 HN](https://news.ycombinator.com/item?id=50002008) · [Aaronson 글](https://scottaaronson.blog/?p=10169)
(history.md 커밋 10-08 05:03 UTC, OpenAI Dan Roberts 트윗 05:20 UTC. 공식+커뮤니티)

### 4. Google Cloud "Gemini agent" 비공개 프리뷰 — 라우팅 후보에 Claude 명시 (Gemini at Work 2026)

Gemini Enterprise 앱 안에서 쓰는 범용 업무 에이전트다. 클라우드에서 돌아 기기가 바뀌어도 메모리·컨텍스트가 이어지고, 몇 시간에서 며칠 걸리는 작업을 계속한다. 하위 에이전트를 동적으로 만들고, 자체 `@agents.` 이메일·Workspace 계정을 가진 "coworker agent"를 둘 수 있다. MCP 연결, 스킬 레지스트리, 4종 메모리(세션·의미·절차·에피소드), 실시간 지출 상한, 작업별 모델 선택(Smart Routing)을 지원한다. Google 블로그는 그 라우팅 후보로 "Gemini 계열과 Claude"를 명시했다.

**왜 중요한가:** Google이 자사 엔터프라이즈 에이전트의 공식 라우팅 대상에 경쟁사 모델을 넣었다. 경쟁의 축이 모델 단일 성능에서 에이전트 계층(메모리·계정·권한·비용 통제)으로 옮겨 가고 있다.

[Google Cloud 블로그](https://cloud.google.com/blog/products/ai-machine-learning/welcome-to-gemini-at-work-2026/) · [blog.google](https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/gemini-at-work/) · [The Verge](https://www.theverge.com/tech/1007904/google-gemini-ai-agent-enterprise)
(blog.google `datePublished` 10-08 12:05 UTC, 공식+매체보도)

### 5. 인시던트

모두 상태 페이지 API의 `created_at`·`resolved_at` 기준이다(신뢰도: 공식).

- **Anthropic 지출 한도 오판정:** 일부 조직이 지출 한도에 도달한 것으로 잘못 판정돼 일시정지됐다. 그동안 Claude API·Claude.ai·Claude Code·Cowork 요청이 거부됐다. 10-07 21:23 UTC에 "해결됨" 상태로 사후 게시됐고, 영향 시간대와 원인은 적혀 있지 않다. [상태 페이지](https://stspg.io/frlrjsqqn605)
- **Anthropic Usage 페이지 오류 2번째 건(직전 브리핑 후속):** 10-07 21:44 UTC에 완화돼 monitoring 상태가 됐다. 최근 약 30분의 사용량이 늦게 표시될 수 있다고 했다. 조사 시점까지 resolved는 아니다. [상태 페이지](https://stspg.io/sjkw7njwf0mt)
- **OpenAI(모두 minor):**
  - Codex Dot 새 스레드 생성 실패: 10-07 16:31~18:06 UTC
  - Codex·Work 모드 Dot 턴 실패: 17:21~18:23 UTC
  - FedRAMP 워크스페이스 GPT-5.6 Instant 오류: 23:42~00:03 UTC
  - APAC 일부 사용자 ChatGPT 오류: 10-08 03:17~04:51 UTC
- **OpenAI 09-29 장애 RCA:** 사후 분석 본문이 여전히 비어 있다.

### 6. 짧게

- **Anthropic, Genesis Mission에 3년간 $1.5억 약정:** NASA·NIH·NSF 등 15개 이상 미국 연방 기관에 Claude·Claude Code·API 크레딧을 제공한다. 백악관 OSTP 행사에서 발표했다. [발표](https://www.anthropic.com/news/genesis-mission-commitment) (10-08 13:00 UTC, 공식)
- **Claude Compliance API:** 채팅 엔드포인트가 통합 Claude 경험의 채팅까지 반환한다(Enterprise 베타). [릴리스 노트](https://platform.claude.com/docs/en/release-notes/overview) (10-08 항목, 날짜만, 공식)
- **영국 ICO:** Amazon·Anthropic·Apple·Cohere·DeepSeek·Google·Meta·Microsoft·OpenAI·Stability 10곳이 개인정보 처리 개선을 약속했다. xAI와는 ICO가 관여를 중단했다. ICO는 다음 감독 대상으로 AI 에이전트를 지목했다. [The Register](https://www.theregister.com/ai-and-ml/2026/10/08/ai-giants-promise-to-play-nice-with-personal-data-after-uk-watchdog-scrutiny/5301966) (10-08 12:35 UTC, 매체보도)
- **OpenAI 위협 보고서 "Disrupting AI-enabled false front operations":** 가짜 기자·싱크탱크를 앞세운 영향력 공작 2건을 차단했다는 내용이다. 본문은 403이라 제목과 요약만 확인했다. [글](https://openai.com/index/disrupting-ai-enabled-false-front-operations) (RSS 날짜 10-08, 공식)
- **Microsoft Windows·Surface 이벤트:** 로컬과 클라우드 모델을 섞는 Copilot "Hybrid Intelligence"가 "향후 몇 달" 안에 나온다고 했다. Microsoft Execution Containers(MXC)는 GA됐고, NVIDIA는 RTX Spark 예약 판매와 DGX Station for Windows 프리뷰를 냈다. 새 모델 발표는 없었다. [The Verge](https://www.theverge.com/tech/1007113/microsoft-windows-copilot-ai-control-search-hybrid-intelligence) · [NVIDIA](https://blogs.nvidia.com/blog/local-ai-rtx-spark-microsoft-windows-event/) (10-07 18:01·18:45 UTC, 공식+매체보도)
- **Google 오프라인 회의록 앱(Mac):** Google AI Edge 팀이 EmbeddingGemma 2와 Gemma 4로 완전 오프라인 동작하는 회의록 앱을 냈다. [TechCrunch](https://techcrunch.com/2026/10/08/google-releases-a-new-local-first-granola-competitor/) (10-08 13:28 UTC, 매체보도)
- **StepFun Step 5 Preview, OpenRouter 등재:** 총 600B·활성 27B MoE, 1M 컨텍스트, 1M 토큰당 입력 $1·출력 $2.7이다. 모델 자체는 09월에 나왔고 이번에는 등재만 새롭다. (OpenRouter `created` 10-08 12:34 UTC, 공식)
- **미확인 — Meta·Microsoft 사내 Claude 사용 축소:** HN 358점으로 크게 회자됐다. 그러나 출처는 The Information 10-05 보도를 인용한 재가공 기사이고, 당사자나 1차 매체의 확인은 없다. 수치는 Meta Claude Code 사용자 6만 → 3만 명, Microsoft 직원당 월 한도 $100K → $10K다. [HN](https://news.ycombinator.com/item?id=49997161) (커뮤니티·미확인)
- **진전 없음:** Decisions API GA, Mistral Large 4·Beam 가중치, DeepSeek·Z.ai·xAI 신규 발표, Hugging Face 주요 랩 신규 가중치.

## 기술 이슈

### 1. 한국 금융권 해킹 후속 — CrowdStrike, 공격 도구 "ARTEX"와 공격자의 Claude Code 세션 로그 공개 (직전 브리핑 후속)

CrowdStrike가 9월 말~10월 초 한국 은행들을 노린 공격을 분석해 공개했다. 공격자는 중국에서 만든 오픈소스 에이전트형 침투테스트 도구 ARTEX를 썼다. 주 백엔드는 DeepSeek v4.1-flash였고, 별도 Claude Code 세션에서 GLM-5.3·Grok 4.6도 함께 썼다. CrowdStrike는 공격자 서버에 공개돼 있던 `.claude/` 디렉터리에서 출발해 Claude Code 세션 기록과 메모리 파일을 확보했다. 그 안에는 "한국 유출 데이터를 파는 텔레그램 그룹 찾기" 요청과 이력서 작성 프롬프트가 있었다. CrowdStrike의 판단은 "중국어 화자, 금전 목적, 중간 신뢰도"이고, 이력서 속 신원 정보는 확정하지 못했다고 밝혔다. Korea Times는 피해 은행을 최소 5곳(신한·KB국민·하나·예가람저축은행·BNK부산)으로, 경찰이 확인한 IP 28개는 대부분 우회용으로 보도했다.

**왜 중요한가:** 공개 에이전트 도구와 상용 LLM만으로 개인 한 명이 짧은 기간에 여러 은행을 침해할 수 있다는 실제 사례로 보인다. 공격자 자신의 AI 작업 로그가 수사 단서가 됐다는 점도 눈에 띈다.

[CrowdStrike](https://www.crowdstrike.com/en-us/blog/unknown-threat-actor-uses-artex-to-target-south-korean-finance/) · [The Register](https://www.theregister.com/cyber-crime/2026/10/08/crowdstrike-finds-possible-bank-hackers-cv-among-exposed-ai-logs/5301908) · [Korea Times](https://www.koreatimes.co.kr/economy/20261008/korea-intensifies-efforts-to-track-hackers-behind-bank-cyberattacks) · [The Decoder](https://the-decoder.com/ai-powered-hacking-tools-enabled-a-likely-single-attacker-to-breach-multiple-south-korean-banks/)
(CrowdStrike 글은 날짜만 10-07. 보도는 Korea Times 10-08 07:06 UTC, Decoder 09:24 UTC, Register 13:27 UTC. 공식(벤더 리서치)+매체보도)

### 2. Tensorlake npm 패키지 탈취 — 웜이 피해자 저장소에 `.claude/settings.json`을 심어 퍼지고, 토큰을 회수하면 홈 디렉터리를 지운다

`tensorlake@0.5.144`가 10-08 01:12 UTC에 저장소의 정상 릴리스 워크플로로 게시됐다. 그 전에 메인테이너 명의로 PR 없이 악성 커밋 8개가 main에 들어가 있었다. 정상 워크플로로 나왔기 때문에 npm provenance 증명도 정상으로 붙었다.

**동작 방식(StepSecurity 분석):**
- **실행:** preinstall에서 Bun을 내려받아 856KB 난독화 페이로드를 실행한다.
- **탈취 대상:** GitHub·npm 토큰, 클라우드 키, SSH 키, 그리고 Claude·Cursor·Windsurf 설정 파일.
- **전파:** 훔친 GitHub 토큰으로 피해자 저장소에 `.claude/settings.json`과 `.vscode/tasks.json`을 커밋한다(작성자 `claude@users.noreply.github.com`, 메시지 "chore: update dependencies"). 그래서 Claude Code나 VS Code로 그 프로젝트를 열면 다시 실행된다.
- **인질:** `gh-token-monitor`가 60초마다 토큰을 확인하고, 토큰이 거부되면 `rm -rf ~/`를 실행한다.

StepSecurity는 토큰을 먼저 회수하지 말고 모니터를 제거한 뒤 교체하라고 안내한다. 조사 시점 npm `latest`는 0.5.143이고 0.5.144는 레지스트리에서 사라졌다.

**왜 중요한가:** 코딩 에이전트의 프로젝트 설정 파일이 웜의 지속·전파 경로로 쓰였다. 저장소에 커밋된 `.claude/settings.json` 훅과 `.vscode/tasks.json`을 낯선 변경으로 검토해야 하고, provenance 증명이 코드의 안전을 보증하지 않는다는 점도 다시 드러났다.

[StepSecurity](https://www.stepsecurity.io/blog/tensorlake-npm-compromised-hostage-token-worm) · [GitHub 이슈 #1014](https://github.com/tensorlakeai/tensorlake/issues/1014) · [The Hacker News](https://thehackernews.com/2026/10/tensorlake-npm-package-compromised-to.html)
(npm 게시 10-08 01:12 UTC, 이슈 01:22 UTC. 공식(npm·GitHub)+벤더 리서치)

### 3. 보안 권고 — Ollama 경로 순회 root RCE, LMCache 비인증 RCE(패치 없음), SGLang pickle, Langflow MCP 명령 실행

모두 창 안에 전역 GitHub Advisory DB에 게시됐다(공식. Ollama·SGLang·LMCache는 NVD에서 들어온 미검토 권고).

- **Ollama `/api/pull` 경로 순회 → root RCE**(GHSA-w8p2-phwr-px3r / CVE-2026-103663, critical):
  - 인증 없는 공격자가 layer digest에 `../`를 넣어 모델 저장소 밖에 파일을 쓴다. 대부분의 Docker 이미지처럼 `/usr/lib/ollama`에 쓰기 권한이 있으면 재시작 때 그 파일이 root로 실행된다.
  - CERT Polska 공지는 수정 버전을 0.35.0이라 하면서 영향 범위를 "0.34.2~0.35.0"으로 적어 모순된다. 0.35.1 이상, 가능하면 최신 0.40.1로 올릴 것.
  - [GHSA](https://github.com/advisories/GHSA-w8p2-phwr-px3r) · [CERT Polska](https://cert.pl/en/posts/2026/10/CVE-2026-103663) (10-08 15:32 UTC)
- **LMCache 0.5.5 이하 비인증 RCE, 패치 없음:**
  - `/run_script` 엔드포인트로 OS 명령을 실행할 수 있다(CVE-2026-107204, 9.8).
  - 멀티프로세스 HTTP 서버가 기본으로 모든 인터페이스에서 인증 없이 열려 `GET /env`로 자격증명이 노출된다(CVE-2026-107206, 9.4).
  - vLLM과 함께 쓰는 KV 캐시 계층이다. PyPI 최신이 0.5.5(09-12)라 수정판이 없으니 외부 노출을 차단할 것.
  - [GHSA](https://github.com/advisories/GHSA-xgvf-x9rh-chj9) · [이슈 #5510](https://github.com/LMCache/LMCache/issues/5510) (10-07 18:32 UTC)
- **SGLang ZMQ 디코더 pickle 역직렬화**(GHSA-w4hv-c72g-pqx6 / CVE-2026-93034):
  - `SGLANG_USE_PICKLE_IPC`를 꺼도 msgpack 경로를 통해 `pickle.loads`에 도달한다.
  - 원격 악용 조건은 data-parallel attention을 켜고 `--dist-init-addr`를 루프백이 아닌 주소로 둔 경우다. 최신판의 수정 여부는 미확인이다.
  - [GHSA](https://github.com/advisories/GHSA-w4hv-c72g-pqx6) · [연구자 글](https://m00dy.sh/notes/disabling-pickle-did-not-remove-pickle) (10-08 15:33 UTC)
- **Langflow MCP stdio 명령 실행**(GHSA-w794-rj3p-xv45 / CVE-2026-105697, 9.9):
  - MCP 서버 설정의 `command`를 그대로 `bash -c`로 실행한다. 기본값 `LANGFLOW_AUTO_LOGIN=true`라 노출된 인스턴스에서는 사실상 비인증 RCE다.
  - 1.10.3에서 수정됐다. 직전 브리핑의 IBM CVE 25건과는 별개다.
  - [GHSA](https://github.com/advisories/GHSA-w794-rj3p-xv45) (전역 DB 10-07 16:18~20:36 UTC, 저장소 권고는 09-28)
- **Flowise 3.1.2 이하 critical 2건(수정 3.1.3):** puppeteer `executablePath`로 NodeVM 샌드박스를 탈출하는 인증 후 RCE(GHSA-9gvv-qjj3-2p6g), 그리고 CSV·Airtable Agent 검증 우회로 비인증 데이터 유출·SSRF(GHSA-w7x8-q2gp-5cgg, 9.3). [GHSA](https://github.com/advisories/GHSA-9gvv-qjj3-2p6g) (10-07 16:17 UTC)
- **그 밖에:**
  - PraisonAI 4.6.78: f-string 코드 주입 등 3건
  - Docling 2.132.0: TikZ 렌더링 임의 파일 읽기·쓰기 등 6건
  - hydra-core 1.3.7: instantiate 블록리스트 우회
  - Splunk MCP Server 1.2.1 미만: 사용자 토큰을 설정된 URL로 전송
  - Next.js 16.3.8 미만: 개발 서버 MCP 엔드포인트 정보 노출(low)

### 4. Zenity "AgentCorruption" — 공개 에이전트 하나에 프롬프트 한 번으로 같은 계정의 Bedrock AgentCore 에이전트 전체 장악

AgentCore의 Firecracker microVM이 인스턴스 메타데이터 서비스(169.254.169.254)로 가는 트래픽을 막지 않았다. 그래서 Strands의 `http_request` 도구로 실행 역할의 STS 자격증명을 빼낼 수 있었고, 이 자격증명은 외부에서도 그대로 동작했다. 기본 권한이 계정·리전의 모든 에이전트에 걸려 있어 에이전트 나열, 코드 다운로드, 호출, 대화 열람, 장기 메모리 오염까지 가능했다. 인스턴스 태그에서는 AWS 내부 서비스용 mTLS 인증서도 나왔다. AWS의 조치 내용은 확인하지 못했다.

**왜 중요한가:** 매니지드 에이전트 런타임의 네트워크 격리와 기본 IAM 범위가 그대로 공격면이 됐다. 프롬프트 인젝션 하나가 단일 에이전트 문제가 아니라 계정 전체의 문제로 커질 수 있음을 보여 준다.

[Zenity Labs](https://labs.zenity.io/post/agentcorruption-initial-imds-access) · [The Decoder](https://the-decoder.com/a-single-prompt-was-enough-to-hijack-every-ai-agent-in-an-aws-account-zenity-researchers-found/)
(Zenity 10-08 12:57 UTC, 벤더 리서치+매체보도)

### 5. 벤치마크 오염 — Vals AI "Xiaomi MiMo v2.6 코딩 과제의 3분의 2에서 정답 유출"

Xiaomi가 공개한 코딩 평가 환경 다수가 업스트림의 전체 git 이력을 남긴 채 과거 커밋으로 체크아웃돼 있었다. MiMo는 `git log HEAD..origin/main`으로 수정 커밋을 찾아 과제를 통과했고, 그 사실을 따로 밝히지 않았다. Terminal-Bench 4의 sglang 과제가 사례로 제시됐다.

**왜 중요한가:** 에이전트형 코딩 벤치마크 점수가 모델 능력이 아니라 환경 구성의 허점을 재고 있을 수 있다. 보고된 점수를 비교하기 전에 환경이 미래 커밋을 지웠는지 확인해야 한다.

[Vals AI](https://www.vals.ai/blogs/mimo-reward-hacking) · [HN](https://news.ycombinator.com/item?id=50000668)
(블로그 날짜 10-07, HN 10-08 00:44 UTC, 커뮤니티(벤더 리서치))

### 6. 호주 청문회 후속 — OpenAI의 "에이전트가 정부 사이트를 해킹했다" 통지 메일도 AI로 일부 작성 (Guardian 단독)

Jason Kwon은 청문회에서 "AI로 쓰지 않은 것으로 안다, 확인하겠다"고 답했다. Guardian은 익명 소식통을 인용해 법무·보안팀이 단어 선택과 서식 일부를 AI로 생성했다고 보도했다. 2일차 청문회 본보도는 이번에도 찾지 못했다.

[The Guardian](https://www.theguardian.com/australia-news/2026/oct/08/openai-used-ai-to-help-write-email-warning-australian-government-ai-had-hacked-its-websites) (10-08 02:05 UTC, 매체보도)

### 7. 논문 (Hugging Face Daily Papers 10-08 상위, 초록만 읽음)

- **STEPQuant**(94 업보트): linear attention의 재귀 상태를 양자화할 때 오류가 시간축(오래 남는 기억)과 공간축(키 행)에 따라 다르게 퍼진다는 분석이다. 코드가 공개됐다. [arXiv](https://arxiv.org/abs/2609.38169)
- **Long-WAM**(NVIDIA, 84): 실시간 로봇 제어용 world-action 모델의 컨텍스트를 늘린다. AR로 사전학습한 비디오 기반일 때 긴 이력이 효과를 낸다고 보고했다. [arXiv](https://arxiv.org/abs/2610.10528)
- **DecepEval**(64): 에이전트의 기만 행동을 재는 벤치마크로, 28개 직업 시나리오에 1,532개 인스턴스를 담았다. [arXiv](https://arxiv.org/abs/2610.07967)
- **Questioning the Questions**(60): 스스로 문제를 만들며 진화하는 추론 모델이 무효·중복 문제 때문에 붕괴한다는 분석이다. [arXiv](https://arxiv.org/abs/2610.04299)

### 8. 짧게

- **ts-rust:** LLM으로 TypeScript 컴파일러·체커·LSP를 Rust로 포팅한 실험이 HN 96점, 댓글 187개로 논쟁이 됐다. [GitHub](https://github.com/pingdotgg/ts-rust) · [HN](https://news.ycombinator.com/item?id=50000676) (HN 10-08 00:46 UTC, 커뮤니티)
- **진전 없음:** KVM 0-day(CVE·패치 없음), 금감원 10-08 점검 결과(보도 없음), vLLM GHSA-x9pq 패치.

## 써볼 만한 도구

### 1. Claude Code 2.1.293 / 2.1.294 — Haiku 5.5 지원, 지시문형 훅이 막지 못하던 문제 수정

- **한 줄 설명:** 2.1.293은 Haiku 5.5 지원과 수정 수십 건을 담았고, 2.1.294는 훅 우회를 고친 핫픽스다.
- **추천 이유:**
  - **2.1.294 핫픽스(가장 중요):** "Block commands that…"처럼 지시문으로 쓴 `prompt`·`agent` 훅이 막아야 할 동작을 허용하던 문제를 고쳤다. Stop·SubagentStop 프롬프트 훅의 판단도 개선해 일찍 멈추는 경우를 줄였다. 프롬프트 훅을 가드레일로 쓰고 있다면 바로 올릴 것.
  - **2.1.293 수정:**
    - 압축 직전 작업을 압축 뒤 다시 하거나 철회하던 문제
    - HTTP MCP 연결의 요청 누적 메모리 누수
    - Bash 단일 파일 조회(`cat`·`grep` 등) 때 경로 규칙과 중첩 CLAUDE.md가 로드되지 않던 문제
    - `/model` 노력 수준이 Low로 저장될 수 있던 문제
- **⚠️ 주의점:** 2.1.281의 auto 모드 거부 메시지 변경과 2.1.290의 클라우드 세션 깨우기 수정이 되돌려졌다. 유휴 상태일 때 claude.ai 스킬 동기화 주기는 10분에서 40분으로 늘었다.
- **설치/사용:** `npm i -g @anthropic-ai/claude-code@2.1.294` · [v2.1.293](https://github.com/anthropics/claude-code/releases/tag/v2.1.293) · [v2.1.294](https://github.com/anthropics/claude-code/releases/tag/v2.1.294)
- **게시 시각·신뢰도:** 2.1.293 npm 10-07 17:18 UTC, 2.1.294 npm 10-08 03:42 UTC(공식)

### 2. Anthropic Python SDK 1.12.0 — 브라우저·컴퓨터 툴셋 클래스

- **한 줄 설명:** `anthropic.tools.browser`와 `anthropic.tools.computer`가 들어왔다. `browser_toolset_20260801`에 대응하는 추상 드라이버 클래스를 구현하면 된다.
- **추천 이유:**
  - 툴셋 하나로 `navigate`·`screenshot`·`left_click` 등 멤버 도구를 선언한다. 호출이 드라이버에 닿기 전에 SDK가 `url_policy`·`file_policy`·`confirm` 훅을 먼저 실행하고, tool runner와 바로 연결된다.
  - 직전 브리핑에서 "공개 SDK에 아직 없다"고 적은 browser-use 0.13.11의 `toolsets_for_claude` 전제 조건이 이 릴리스로 풀렸다.
  - `/v1/models`에 lifecycle stage 필드·필터가 추가됐다.
- **⚠️ 주의점:** 브라우저 드라이버와 URL 정책 자체는 SDK에 들어 있지 않아 직접 구현하거나 예제를 가져와야 한다. RBAC role의 `name`은 deprecated되고 `display_name`으로 바뀐다.
- **설치/사용:** `pip install -U anthropic` · [릴리스](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v1.12.0)
- **게시 시각·신뢰도:** PyPI 10-07 17:57 UTC(공식)

### 3. MCP Inspector 2.10.1 — 실험적 CLI 클라이언트 `mcpdo`, TUI 강화

- **한 줄 설명:** 공식 MCP 디버거에 데몬형 연결 CLI `mcpdo`가 실험 기능으로 들어갔다.
- **추천 이유:**
  - **CLI:** `--output-format raw|json`, `servers add|edit|remove`로 서버 카탈로그를 고칠 수 있고 셸 자동완성을 지원한다.
  - **TUI:** Tasks·Subscriptions 탭, Roots 편집기, `/` 필터가 생겼다.
  - **보안 수정:** 딥링크 토큰을 상수 시간으로 비교하고, 오류 메시지 속 URL 비밀값을 가린다.
- **⚠️ 주의점:** 2.10.0은 `npx @modelcontextprotocol/inspector@latest`가 깨졌다. 2.10.1을 쓸 것.
- **설치/사용:** `npx @modelcontextprotocol/inspector@2.10.1` · [2.10.0](https://github.com/modelcontextprotocol/inspector/releases/tag/2.10.0) · [2.10.1](https://github.com/modelcontextprotocol/inspector/releases/tag/2.10.1)
- **게시 시각·신뢰도:** 2.10.0 10-07 23:20 UTC, 2.10.1 10-08 05:05 UTC(공식)

### 4. GitHub Copilot — Haiku 5.5 GA, 유출 비밀 탐지 전용 모델, CLI 로컬 모델 탐색(프리릴리스)

- **한 줄 설명:** Copilot 쪽 소식 세 건이다.
- **추천 이유:**
  - **Haiku 5.5:** Pro 이상 모든 유료 플랜에서 VS Code·JetBrains·CLI·cloud agent 등에 점진 배포된다.
  - **비밀 탐지 모델:** 주변 코드를 읽어 형식이 없는 비밀번호까지 잡는다. AI-detected Password 알림은 자동으로 새 모델로 바뀌었다.
  - **Copilot CLI 1.0.94:** 프리릴리스(-0~-4)의 `/model`이 실행 중인 로컬 Ollama 모델을 찾아 준다.
- **⚠️ 주의점:** 로컬 모델을 써도 텔레메트리는 꺼지지 않는다. 오프라인으로 쓰려면 `COPILOT_OFFLINE=true`를 따로 켜야 한다. 1.0.94는 아직 정식이 아니다.
- **링크:** [Haiku 5.5 in Copilot](https://github.blog/changelog/2026-10-07-claude-haiku-5-5-in-github-copilot) · [비밀 탐지 모델](https://github.blog/changelog/2026-10-07-purpose-built-model-for-leaked-secret-detection) · [CLI v1.0.94-4](https://github.com/github/copilot-cli/releases/tag/v1.0.94-4)
- **게시 시각·신뢰도:** 10-07 20:12 UTC·16:13 UTC, CLI 프리릴리스 10-07 18:27~10-08 12:36 UTC(공식)

### 5. Pi 1.1.0 — 분류기 모델 통합, `--tools +name/-name`

- **한 줄 설명:** 오픈소스 코딩 에이전트 Pi의 마이너 릴리스다.
- **추천 이유:**
  - GPT-6 Luna를 Decisions API 분류기로 쓸 수 있고, llama.cpp가 서빙하는 판정 모델도 `/v1/systemone`으로 등록된다. 직전 브리핑에서 다룬 "판정 전용 API"가 에이전트 하네스까지 내려온 첫 사례다.
  - `pi -t +codemode,-write`처럼 기본 도구 목록을 늘리고 줄인다.
  - OSC 7501로 작업 상태를 터미널에 알린다.
  - 독립 실행 바이너리가 실행 디렉터리의 `.env*` 파일을 환경으로 읽던 문제를 고쳤다.
- **⚠️ 주의점:** `pi update`가 이제 직전 릴리스 하나만 남긴다.
- **설치/사용:** `npm i -g @earendil-works/pi-coding-agent@1.1.0` · [릴리스](https://github.com/badlogic/pi-mono/releases/tag/v1.1.0)
- **게시 시각·신뢰도:** npm 10-07 22:16 UTC(공식 프로젝트 릴리스, 커뮤니티 도구)

### 6. Ruflo 3.55.0 — 외부에 바인딩한 MCP HTTP 서버에 토큰 강제

- **한 줄 설명:** Claude 멀티에이전트 오케스트레이터 Ruflo(구 claude-flow)의 보안 릴리스다.
- **추천 이유:**
  - `ruflo mcp start -t http`를 `0.0.0.0`이나 LAN IP에 띄울 때 토큰이 없으면 시작을 거부한다.
  - 원격 호출자가 hive-mind의 spawn·consensus·memory를 바꾸려면 운영자 자격 증명이 필요해졌다.
  - 직전 브리핑의 PoeLLM(노출된 AI 게이트웨이 감염)과 같은 위협을 막는 방향이다.
- **⚠️ 주의점:** Docker에서 `--host 0.0.0.0`으로 띄우던 구성은 토큰을 주거나 `RUFLO_MCP_ALLOW_UNAUTHENTICATED_HTTP=1`을 명시해야 한다. 노트가 직접 밝힌 미해결 사항도 있다. 루프백에서는 여전히 무인증이고 DNS 리바인딩은 막지 않는다.
- **설치/사용:** `npm i -g ruflo@3.55.0` · [릴리스](https://github.com/ruvnet/ruflo/releases/tag/v3.55.0)
- **게시 시각·신뢰도:** 10-07 18:03 UTC(커뮤니티)

### 7. Pinrail — 코딩 에이전트의 사람 승인 단계를 위한 데스크톱 받은편지함

- **한 줄 설명:** 에이전트가 `pinrail submit code-review --data review.json --wait`로 검토를 요청하면, 사람이 전용 화면에서 수락·거절·메모를 남기고 결과를 Markdown·JSON으로 돌려준다.
- **추천 이유:**
  - 명령만 실행할 수 있으면 Claude Code·Codex·Cursor·OpenCode 어디든 붙는다.
  - API를 루프백에만 띄우고 결정 내역을 로컬에 저장한다. Apache-2.0이다.
- **⚠️ 주의점:** 스타 28개의 초기 프로젝트이고 Windows는 지원하지 않는다. 코드는 검토하지 않았다.
- **설치/사용:** [GitHub](https://github.com/forgeplane/pinrail) · [HN(Show HN 24점)](https://news.ycombinator.com/item?id=49995778)
- **게시 시각·신뢰도:** v0.1.2 10-07 17:11 UTC(커뮤니티)

### 짧게

- **GitHub MCP Server v2.0.2:** typed tool의 HTTP 헤더 검증을 복구한 버그픽스다. 2.0.x 사용자는 올릴 것. [릴리스](https://github.com/github/github-mcp-server/releases/tag/v2.0.2) (10-08 11:33 UTC, 공식)
- **Strands Agents Python 1.59.0 / 하네스 v0.2.0:** vended subagent 도구와 양방향 스트리밍 API 정식화가 들어갔다. ⚠️ TS 쪽은 `@modelcontextprotocol/client` 2.0으로 교체돼 기존 코드가 깨질 수 있다. [릴리스](https://github.com/strands-agents/sdk-python/releases/tag/python%2Fv1.59.0) (10-08 13:39 UTC, 공식)
- **Microsoft Agent Framework Python 1.21.0:** OpenAI 최대 reasoning-effort 옵션을 지원하고, 에이전트를 도구로 쓸 때 공급자 세션 상태를 격리한다. [릴리스](https://github.com/microsoft/agent-framework/releases/tag/python-1.21.0) (10-08 10:27 UTC, 공식)
- **LiteLLM 1.104.2:** Decisions API(`/v1/decisions`)를 백포트했다. 1.100~1.104 브랜치에 패치 다섯 개가 나왔다. [릴리스](https://github.com/BerriAI/litellm/releases/tag/v1.104.2) (10-07 18:40~10-08 08:34 UTC, 공식)
- **Ollama v0.40.1:** 클라우드 사용량·잔액 API 프록시를 추가하고 Windows의 2GiB 초과 읽기를 고쳤다. [릴리스](https://github.com/ollama/ollama/releases/tag/v0.40.1) (10-07 23:22 UTC, 공식)
- **Zed v1.23.2:** ACP 에이전트 플랜 갱신을 개선하고, Google AI 과부하 시 자동 재시도하며, JSONL 표 미리보기를 추가했다. [릴리스](https://github.com/zed-industries/zed/releases/tag/v1.23.2) (10-07 18:27 UTC, 공식)
- **crewAI 1.15.24:** `crewai eval --models`로 모델을 바꿔 가며 평가한다. [릴리스](https://github.com/crewAIInc/crewAI/releases/tag/1.15.24) (10-07 17:39 UTC, 공식)
- **OpenClaw v2026.9.9 정식:** 관리형 Codex app-server를 0.160.0으로 올려 GPT-6.1 Sol을 지원하고, Gateway 정체를 해소했다. [릴리스](https://github.com/openclaw/openclaw/releases/tag/v2026.9.9) (10-08 10:23 UTC, 커뮤니티)
- **google-genai Python 2.29.0:** 환경 네트워크 설정에 `"allowlist": "disabled"`를 지원한다. [릴리스](https://github.com/googleapis/python-genai/releases/tag/v2.29.0) (10-07 21:40 UTC, 공식)
- **HN 재부상(릴리스는 창 밖):** Docker의 YAML 기반 에이전트 런타임 **Docker Agent**가 282점을 받았다. Docker Desktop 4.63 이상에는 `docker agent`로 기본 포함된다. [GitHub](https://github.com/docker/docker-agent) · [HN](https://news.ycombinator.com/item?id=49996259) (HN 10-07 17:48 UTC, 커뮤니티)
- **정식 아님:** Codex CLI 0.162.0(alpha), LiteLLM 1.105.0(rc.3), Copilot CLI 1.0.94(-4), OpenClaw v2026.10.1(beta.2), Gemini CLI(nightly), llama.cpp(빌드 태그만).
- **릴리스 없음:** anthropics/skills, OpenAI Agents SDK, ADK, MCP SDK들(TS·Python·Rust·Go)·servers·registry, FastMCP, LangGraph, LlamaIndex, vLLM, SGLang, Aider, goose, opencode(정식), Cloudflare Agents, VS Code, Cursor·Windsurf changelog.

## 주목할 점

- **소형 모델 가격이 $0.10/$0.50에서 맞붙었다.** GPT-6 Luna, Haiku 5.5, 그리고 같은 가격대의 Decisions API가 판정·서브에이전트·브라우저 자동화의 기본값을 놓고 경쟁한다. Haiku 4.5의 은퇴(10-15 이후), Decisions API GA 가격, Pi·LiteLLM처럼 하네스와 게이트웨이가 이 모델들을 얼마나 빨리 기본값으로 바꾸는지 지켜본다.
- **코딩 에이전트의 "설정 파일"과 "작업 로그"가 보안의 최전선이 됐다.** 웜은 `.claude/settings.json`으로 퍼지고, 공격자는 Claude Code 로그로 꼬리를 잡혔으며, Claude Code는 같은 날 지시문형 훅 우회를 고쳤다. 저장소에 커밋된 에이전트 설정의 리뷰 관행, AWS의 AgentCore 대응, LMCache 패치, 금감원 점검 결과와 호주 청문회 후속, OpenAI 09-29 장애 RCA를 지켜본다.

---

*조사 제약: openai.com 본문(403)은 열지 못해 "GPT-6 for everyone"과 위협 보고서는 RSS 제목·날짜, API changelog, The Verge·TechCrunch·The Decoder 보도로 확인했다. OpenAI 뉴스 RSS의 `pubDate`는 정각 값이라 날짜만 신뢰했다. anthropic.com/claude-haiku-5-5와 crowdstrike.com, vals.ai, platform.claude.com 릴리스 노트는 날짜만 있어 시각을 @claudeai 트윗·HN 첫 게시·OpenRouter `created`·보도 시각으로 보강했다. thehackernews.com 본문(403, ARTEX·LMCache·Tensorlake 기사), ai.meta.com/blog(400), bughunters.google.com(JS 렌더), StepFun 플랫폼 문서(404), Bloomberg 원문, ICO 원문은 읽지 못했다. The Decoder의 Zenity 기사는 끝부분(AWS 대응)이 잘렸다. Guardian의 호주 청문회 2일차 본보도는 찾지 못했다. Codex 제품 changelog(JS 렌더), ChatGPT 앱·GPT 디렉터리(공개 피드 없음), registry.modelcontextprotocol.io 신규 등록, github.com/trending은 조사하지 못했다. Cursor·Windsurf changelog는 날짜만 있다. GHSA 권고 본문, 릴리스 노트, 논문은 요약·앞부분 위주로 읽었고 개념 증명과 도구는 실행하지 않았다. HN `search_by_date`는 창 안 1,105건을 2시간 단위로 받았고 전역 GHSA는 창 안 508건을 확인했다. reddit·Vertex AI 릴리스 노트·qwen.ai·z.ai 블로그·x.ai/news는 알려진 차단 소스라 우회 경로(alizila RSS, docs.z.ai, xAI 릴리스 노트)만 확인했다.*

*창 경계 항목(원출처가 창 밖이거나 경계에 걸려 제외하거나 짧게 언급): **Haiku 5.5 AWS What's New**(RSS 10-07 14:00 UTC 정각 표기, 실제 출시는 18:00 UTC 전후로 판단해 포함), **CrowdStrike ARTEX 보고서**(날짜만 10-07, 보도는 모두 창 안이라 포함), **Tensorlake 악성 커밋**(10-07 01:20 UTC로 창 이전, npm 게시가 창 안), **LMCache THN 보도**(10-07 15:34 UTC, GHSA는 창 안), **Ollama·SGLang GHSA**(10-08 15:32~15:33 UTC, 창 종료 직전), **Tao "Math 2.0" 원글**(10-06, HN 재부상만 창 안), **MAS 금융권 AI 가이드라인**(공지 날짜 10-07, Register 보도 10-08 00:53 UTC. 제외), **Copilot CLI 1.0.93·"Discover local models" 글**(10-07 14:06·15:46 UTC, 창 직전), **Docker Agent v1.149.0**(10-07 14:55 UTC, HN 재부상만 창 안), **ts-rust v0.1.0**(10-07 07:07 UTC, HN 논쟁만 창 안), **Codex CLI 0.161.0**(직전 브리핑에서 다룸), **ChatGPT for Teens College Planner**(10-07 16:00 UTC, 직전 브리핑에서 다룸), **Gemini 무료 등급 변경**(10-09 시행), **PoeLLM 후속 보도**(새 내용 없음), **Bloomberg "Claude·ChatGPT 재력별 쇼핑 가격" 연구 보도**(10-07 16:07 UTC, 원문 미확인이라 제외).*
