---
title: "2026-10-08 AI 브리핑"
date: 2026-10-08T01:00:00+09:00
tags: [ai-briefing, openai, google, anthropic, security]
description: "OpenAI가 분류·판정 전용 Decisions API(입력 $0.10/M, 출력 무료)를 베타로 내고 사용 등급을 3단계로 줄였으며, 모델이 쓴 수학 원고 722편을 GitHub에 공개해 수학계 반발을 샀고, Google은 740M 멀티모달 임베딩 EmbeddingGemma 2를 공개했으며, llama-server의 비인증 원격 메모리 손상(Critical)과 Langflow CVE 25건, 노출된 LiteLLM·Ollama 서버 3,400대를 감염시킨 PoeLLM 봇넷이 같은 날 알려졌다."
---

> 조사 범위: 2026-10-07 01:00 ~ 2026-10-08 01:00 KST(2026-10-06 16:00 ~ 10-07 16:00 UTC, 직전 브리핑 조사 창 종료 시각 이후). 시각은 기사 `article:published_time`·JSON-LD `datePublished`·RSS `pubDate`·GitHub `published_at`/`created_at`·npm/PyPI 게시 시각·OpenRouter `created`·HN Algolia `created_at`·GHSA `published_at`·상태 페이지 API로 검증했다. HN 점수는 조회 시점(10-07 16:10 UTC 전후) 값이다.

## 오늘의 핵심 요약

- **OpenAI가 "답이 하나의 값인" 호출용 Decisions API를 베타로 냈다.** `gpt-6-luna` 전용으로 입력 $0.10/1M 토큰에 출력 과금이 없고, 확률·선택·점수만 돌려준다. 같은 날 API 사용 등급이 5단계에서 3단계(Build·Launch·Grow)로 줄어 월 한도 기준이 바뀌었다. 모델이 생성한 수학 원고 722편의 GitHub 공개는 창 안 HN 1위(1,125점)였고, 프린스턴 IAS가 "검증·책임질 수 없는 논증 산출"을 지지하지 않는다는 성명을 냈다.
- **Google EmbeddingGemma 2가 나왔다.** 텍스트·코드·이미지·비디오·오디오를 한 공간에 넣는 740M 모듈형 오픈 임베딩이고 텍스트만 쓰면 270M이다. Anthropic은 Mythos 5.1까지 포함한 3단계 Cyber Verification Program으로 사이버 능력 접근을 신원 검증 티어로 재편했다.
- **로컬·자가 호스팅 AI 서버를 점검할 날이다.** llama-server가 비인증 원격 UAF로 Critical(CVE-2026-107183, b11393 이전)을 받았고, IBM이 Langflow 1.12.2 이하 CVE 25건(9.8 두 건은 비인증 RCE)을 한꺼번에 등재했으며, PoeLLM 봇넷이 노출된 LiteLLM·Ollama 서버 3,400대 이상을 감염시키며 LiteLLM MCP 엔드포인트 CVE를 체인으로 썼다. 도구 쪽은 GitHub MCP Server 2.0(출력 스키마), VS Code 1.141(크로스플랫폼 샌드박스), Codex CLI 0.161.0(GPT-6.1 Sol 기본), Copilot CLI 1.0.93 정식이 겹쳤다.

## 모델 소식

### 1. OpenAI Decisions API 공개 베타 (`POST /v1/decisions`, gpt-6-luna 전용) + API 사용 등급 5→3단계

OpenAI가 텍스트·이미지 입력을 받아 "타입이 정해진 답"만 돌려주는 전용 엔드포인트를 공개 베타로 냈다. 반환 형식은 세 가지다. `predicate`(참일 확률 0~1), `choice`(고정 선택지 중 하나), `score`(순서형 레벨의 확률가중 평균). 분류·라우팅·심사 용도이고, OpenAI는 Responses API보다 약 10배 빠르다고 적었다. 현재 `gpt-6-luna`만 지원하며 "수주 내 GA" 예정이다.

- **가격:** 입력 $0.10/1M 토큰이고 캐시 읽기·쓰기·출력 토큰 과금이 없다(가이드의 "Pricing and availability" 절). 지역 처리 프리미엄과 장문 컨텍스트 배수는 적용된다. 가격표 페이지(`pricing.md`)에는 아직 Decisions 항목이 없다.
- **SDK 최소 버전:** Python 3.26.0, JavaScript 7.30.0, Go 3.73.0, Ruby 0.101.0, Java 4.78.0. Playground는 `platform.openai.com/decisions`다.
- **사용 등급 축소:** 같은 날 changelog 항목으로 API 사용 등급이 5단계에서 3단계로 바뀌었다. Build(누적 $5 구매, 월 $500 한도), Launch($100, 월 $5,000), Grow($500, 월 $200,000)이고 Free는 월 $100 그대로다. 누적 크레딧 구매액에 따라 자동 승급한다.

- [Decisions 가이드](https://developers.openai.com/api/docs/guides/decisions) · [changelog](https://developers.openai.com/api/docs/changelog) · [사용 등급](https://developers.openai.com/api/docs/guides/rate-limits) · [The Decoder](https://the-decoder.com/openai-launches-decisions-api-that-reduces-complex-evaluations-to-yes-no-or-pick-one/) · [HN 371점](https://news.ycombinator.com/item?id=49984025)
- 게시: changelog는 날짜만 있다("Oct 6"). 창 안 근거는 openai-python 3.26.0의 PyPI 게시 10-06 18:04 UTC(3.25.0)·21:31 UTC(3.26.0), openai-node 7.30.0 21:50 UTC, HN 첫 게시 20:57 UTC다. · 신뢰도: **공식**
- 주의: "10배 빠름"은 자체 주장이고 지연 수치는 없다. The Decoder는 이 출시를 9월 중순부터 이어진 소형 "decision model" 흐름(AWS Strands Decider 2B 등)에 대한 OpenAI의 응답으로 해석하는데, 그것은 매체의 해석이다. API는 호출해 보지 않았다.

**왜 중요한가:** 분류·게이트·라우팅처럼 "답이 하나의 값인" 호출을 Structured Outputs로 짜던 코드가 옮겨 갈 자리가 생겼다. 출력 토큰 과금이 없어 짧은 판정을 대량으로 돌리는 파이프라인에는 비용 차이가 크다. 사용 등급 개편은 모든 API 조직의 월 한도·승급 기준이 바뀌는 변경이라 한도에 맞춰 둔 배치 작업은 확인이 필요하다.

### 2. Google EmbeddingGemma 2 — 740M 멀티모달 임베딩, Apache 2.0 (직전 브리핑 예고 후속)

Gemma 4 아키텍처 기반으로 텍스트·코드·이미지·비디오·오디오를 하나의 임베딩 공간에 넣는 오픈 임베딩 모델이다. 총 740M이지만 모듈형이라 텍스트만 쓰면 270M이고, 비전 인코더(+170M)·오디오 인코더(+300M)를 선택적으로 붙인다.

- **사양:** 컨텍스트 8K(1세대의 4배. 오디오 약 5.5분, 이미지 29장, 비디오 58프레임), MRL로 768→512/256/128차원 절단. Pixel 11 Pro 양자화 기준 텍스트 전용 약 191MB, 풀 멀티모달 약 567MB RAM.
- **자체 수치:** MTEB Code 68.76→78.68. Google은 "1B 미만 중 최고"이고 "2배 큰 전문 모델을 능가"한다고 주장한다. Gemma 4와 토크나이저·오디오 인코더를 공유해 온디바이스 RAG 메모리를 줄인다고 한다.
- 1세대는 누적 2,000만 회 넘게 받아졌다. transformers v5.19.0에 모델 클래스가 같은 날 들어갔다(도구 섹션 참고).

- [Google 발표](https://blog.google/innovation-and-ai/technology/developers-tools/embeddinggemma-2/) · [Hugging Face](https://huggingface.co/google/embeddinggemma-2) · [The Decoder](https://the-decoder.com/google-claims-embeddinggemma-2-outperforms-rival-embedding-models-twice-its-size/) · [HN 400점](https://news.ycombinator.com/item?id=49980487)
- 게시: 10-06 16:00 UTC(blog.google JSON-LD `datePublished`, 창 시작과 같은 시각), HN 첫 게시 16:03 UTC, 공지 트윗 16:05 UTC, HF 레포 `lastModified` 15:29 UTC · 신뢰도: **공식**
- 주의: 벤치마크는 전부 Google 자체 발표이고, 모델 카드의 세부 표는 읽지 않았다. HF 다운로드는 조사 시점 7,562회다.

**왜 중요한가:** 온디바이스 멀티모달 검색·RAG용 오픈 임베딩의 기준 모델이 갱신됐다. 텍스트만 필요하면 270M만 올려도 되는 모듈 구조라 기존 EmbeddingGemma 사용자는 비용 없이 바꿔 볼 수 있다.

### 3. Anthropic Cyber Verification Program 개편 — 3단계 접근권에 Mythos 5.1 포함

Anthropic이 Project Glasswing(Mythos 조기 접근)과 기존 Cyber Verification Program(Opus·Sonnet 안전장치 완화)을 하나로 합치고 접근권을 3단계로 나눴다.

- **Defense Access:** SOC·사고 대응·멀웨어 리버싱·취약점 검증 용도. 기업·비영리·대학·정부·중요 인프라·오픈소스 메인테이너·개인 연구자가 대상이고 수일 내 심사한다.
- **Red Team Access:** 인가된 침투테스트·레드팀. 조직만 신청할 수 있고 수주 걸린다. 랜섬웨어 배포 같은 물리적 피해 행위는 실시간 차단이 유지된다.
- **Specialized Access:** 항공 관제·전력망·통신·은행간 시스템 등. 미 정부와 공동 심사한다.
- 각 단계에 Claude Opus 5.5·Sonnet 5.5·Mythos 5.1과 향후 모델이 포함된다. Glasswing 파트너가 2026년 4~7월에 확인된 취약점 최소 129,000건(고위험·치명 33,000건 이상)을 찾았다고 적었다.

- [Anthropic 발표](https://www.anthropic.com/news/cyber-verification-program) · [The Register](https://www.theregister.com/security/2026/10/07/anthropic-reconfigures-its-cool-kids-security-program/5301509) · [The Decoder](https://the-decoder.com/anthropic-gives-more-security-teams-access-to-claude-with-fewer-safety-restrictions/)
- 게시: 10-06 19:00 UTC(`article:published_time`), Register 23:29 UTC, Decoder 10-07 12:05 UTC · 신뢰도: **공식** + 매체보도
- 주의: 취약점 수치는 파트너 일부의 설문에 기반한 자체 발표다. The Register는 VulnCheck의 Patrick Garrity를 인용해 Anthropic 연계 CVE 225건 중 실제 악용은 0.5% 미만이라고 짚었고, GLM-5.3 사이버 능력 경고 1주 뒤라는 맥락을 붙였다. HN 등재는 없다.

**왜 중요한가:** 범용 모델의 사이버 차단을 "신원 검증 기반 티어"로 푸는 구조가 공식화됐다. 직전 브리핑의 Mistral Large 4가 "폐쇄 모델이 거절하는 취약점 과제"를 판매 포인트로 삼은 것과 맞물려, 사이버 능력의 접근 통제가 모델 능력 경쟁의 한 축이 되고 있다.

### 4. OpenAI, 모델 생성 수학 원고 722편을 GitHub에 공개 — 창 안 HN 1위, 수학계 반발

OpenAI가 내부 모델이 생성한 수학 원고 722편을 372개 패밀리(주결과·보조 논증·따름정리·대안 증명)로 묶어 `openai/math` 저장소에 올렸다. README는 "검증 단계가 제각각이고 Lean 형식화가 없는 것도 있으며, 미형식화 결과에는 오류가 있을 수 있다"고 적는다. 기존 수학 평가가 포화돼 공개 연구 문제로 평가를 넓혔다는 설명이다. 추론 요약 10편(π의 무리성 지수, Mahler 추측, Kaplansky 직접유한성 등)이 따로 공개됐고, The Decoder에 따르면 거의 모든 결과가 단일 프롬프트·단일 에이전트에서 나왔으며 결과당 평균 ChatGPT Pro Thinking 연산 약 3시간이 들었다(OpenAI 자체 설명). 조사 시점 스타는 8,150개다.

- **반응:** 창 안 AI 관련 HN 1위(1,125점, 댓글 1,211개)다. "Integer multiplication below n log n", "The Quasi-Riemann Hypothesis" 같은 제목이 별도 스레드를 낳았다. Guardian은 프린스턴 IAS가 "인간이 이해·검증·책임질 수 없는 수학 논증을 산출하는" 관행을 지지하지 않는다는 성명을 냈다고 전했다.

- [GitHub openai/math](https://github.com/openai/math) · [OpenAI 글](https://openai.com/index/sharing-ai-progress-in-mathematics)(403, 열지 못함) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1005004/openai-math-release-github) · [The Decoder](https://the-decoder.com/openai-dumps-372-ai-generated-math-proofs-on-github-telling-the-academic-world-to-keep-up/) · [The Guardian](https://www.theguardian.com/technology/2026/oct/07/openai-mathematical-findings-concerns) · [HN 1,125점](https://news.ycombinator.com/item?id=49984923)
- 게시: 저장소 생성 10-06 21:47 UTC, 첫 커밋 21:58 UTC, HN 첫 게시 22:17 UTC(창 안). OpenAI 뉴스 RSS의 `pubDate`는 10-06 12:00 GMT인데, 이 피드는 다른 글도 12:00·00:00 정각으로 찍혀 있어 날짜만 믿을 수 있다. 직전 브리핑에는 실리지 않았다. Verge 23:26 UTC, Guardian 10-07 15:40 UTC · 신뢰도: **공식** + 매체보도
- 주의: 어떤 모델이 생성했는지는 비공개다. 722편·372 패밀리·3시간 수치는 전부 OpenAI 서술이고, 검증되지 않은 결과가 섞여 있음을 OpenAI가 직접 명시했다. 블로그 본문과 원고 내용은 읽지 않았다.

**왜 중요한가:** 모델의 수학 산출물이 저널 대신 GitHub와 Lean으로 대량 공개됐고, 그 방식 자체가 논쟁이 됐다. Lean 형식화가 없는 원고의 오류율과 "검증 병목"을 누가 감당하느냐가 다음 쟁점이다.

### 5. Anthropic Claude for Startups 확대 (직전 브리핑 예고 후속)

SF Tech Week에서 발표된 개편이다. 승인된 창업사에 Claude Team 1년 무료(프리미엄 5석), API 크레딧 $1,000, 참여 파트너 할인·크레딧 최대 $45,000, Claude Marketplace 접근, Applied AI 팀 오피스아워를 준다. 자격은 설립 5년 이내 또는 최근 2년 내 투자 유치다.

- [프로그램 페이지](https://claude.com/programs/startups) · [TechCrunch](https://techcrunch.com/2026/10/06/anthropic-gives-startups-a-free-year-of-enterprise-service-and-1000-in-token-credits/) · [HN 4점](https://news.ycombinator.com/item?id=49991983)
- 게시: TechCrunch 10-06 16:00 UTC(창 시작과 같은 시각). 프로그램 페이지에는 날짜가 없고 anthropic.com/news에 별도 글은 없다. · 신뢰도: **공식** + 매체보도
- 주의: TechCrunch는 $1,000 크레딧과 Team 1년만 언급하고, $45K 파트너 혜택은 프로그램 페이지에만 있다. 세부 조건은 Startup Program Terms를 따른다.

### 6. 인시던트: OpenAI 4건 종결, Anthropic 사용량 데이터 오류 2건

- **OpenAI:** 직전 브리핑의 Responses API 대용량 PDF 오류는 10-06 20:11 UTC에 해결됐다(원인 미기재). 창 안 신규는 Computer Use/Agents API 장애(10-06 17:15 ~ 20:00 UTC, minor), Workspace Agents 응답 누락(10-07 01:12 ~ 03:00 UTC, minor), ChatGPT 데스크톱 앱 26.1002.51308에서 Work 스레드 생성 불가(10-07 05:31 ~ 11:53 UTC, **major**)다. 마지막 건은 Linux·macOS 수정판이 먼저 나갔고 Windows는 11:06 UTC 이후 배포됐으며, 영향 사용자는 앱을 업데이트해야 한다.
- **Anthropic:** platform.claude.com의 Usage 페이지와 Admin API usage report 오류가 2건 났다(둘 다 major 표기). 첫 건은 10-07 00:00 ~ 01:40 UTC 영향, 02:33 UTC 해결. 두 번째 건은 13:25 UTC에 열려 조사 시점(16:00 UTC)까지 조사 중이었다. 둘 다 Messages API에는 영향이 없다.
- **09-29 OpenAI 대규모 장애의 원인 분석:** 10-06 08:26 UTC에 두 번째 "resolved" 업데이트가 추가됐지만 `postmortem_body`는 비어 있다. "5영업일 내 RCA" 약속은 여전히 이행되지 않았다. 직전 브리핑의 Claude Opus 5.5·Mythos/Fable 건 사후 보고서도 없다.

- [status.openai.com](https://status.openai.com/) · [status.claude.com](https://status.claude.com/)
- 게시: 상태 API `created_at`·`resolved_at` · 신뢰도: **공식**

### 7. 짧게

- **Google SynthID Detector 일반 공개:** synthid.com에서 누구나 이미지·비디오·오디오 파일을 올려 SynthID 워터마크를 확인할 수 있다. Nano Banana·Veo·Lyria·Gemini·Flow가 워터마크를 찍고, OpenAI·NVIDIA·Kakao도 지원하며 Apple이 추가 예정이라고 TechCrunch가 전했다. 검증 요청은 월 100만 건이라고 한다. [TechCrunch](https://techcrunch.com/2026/10/07/googles-new-synthid-website-can-identify-ai-generated-media/) · [synthid.com](https://synthid.com/) · [HN 52점](https://news.ycombinator.com/item?id=49993188) (10-07 14:00 UTC, 매체보도 + 공식 사이트. 사이트는 JS 렌더라 게시 시각 메타가 없다)
- **Common Sense Media, "ChatGPT for Teens는 수용 불가 위험" 평가:** 자해 대화 1시간 동안 부모 알림이 0건이었고 위기 대응이 부족하며 숙제를 대신 해 준다는 평가다. OpenAI 대변인은 "실제 동작을 반영하지 않는다"고 반박했다. 같은 날 OpenAI는 ChatGPT for Teens에 대학 지원을 돕는 College Planner를 추가했다(Verge 10-07 16:00 UTC, 창 종료와 같은 시각). [The Verge](https://www.theverge.com/ai-artificial-intelligence/1006355/openai-chatgpt-for-teens-common-sense-media) · [College Planner](https://www.theverge.com/ai-artificial-intelligence/1005194/openai-chatgpt-teens-college-planner-notecards) (10-07 09:00 UTC, 매체보도. Common Sense 보고서 원문은 읽지 않았다)
- **Claude for Google Workspace 공개 베타:** Docs·Sheets·Slides 사이드바 애드온과 커넥터가 모든 유료 플랜에 열렸다. 글에 날짜만 있어(10-06) 창 안 판정은 하지 못했다. [Claude 글](https://claude.com/resources/articles/claude-now-works-in-google-docs-sheets-and-slides) · [HN 3점](https://news.ycombinator.com/item?id=49991594) (공식)
- **NVIDIA Nemotron 파인튜닝으로 IOI·IMO 2026 금메달급:** HF 블로그 글이다. 자체 발표이고 가중치 공개 여부는 확인하지 못했다(nvidia HF 조직에 창 안 신규 없음). [HF 블로그](https://huggingface.co/blog/nvidia/nemotron-ioi-and-imo-2026) (10-07 12:45 UTC, 공식)
- **Google, Gemini 기반 게임 제작 실험 "Playground":** 기사 본문 추출에 실패해 세부는 확인하지 못했다. [TechCrunch](https://techcrunch.com/2026/10/07/google-experiments-with-an-ai-powered-gaming-platform/) (10-07 14:36 UTC, 매체보도)
- **The Register: Claude 구독이 OpenAI보다 가치가 높다는 리서치 보도:** 연구 기관과 방법은 확인하지 못했다. [The Register](https://www.theregister.com/ai-and-ml/2026/10/06/anthropic-claude-subscription-plan-provides-more-value-than-openais-study-says/5301470) (10-06 21:11 UTC, 매체보도)
- **OpenAI–Atlassian 파트너십 확대:** RSS 제목만 확인했고 본문은 403이다. 모델 소식은 아니다. [OpenAI](https://openai.com/index/atlassian-partnership) (10-06 16:00 UTC, 공식·제목만)
- **Meta Muse iPad 지원:** 모델 변경은 아니다. [The Verge](https://www.theverge.com/tech/1006813/muse-ai-agent-ios-app-ipad-support) (10-07 15:37 UTC, 매체보도)
- **Apple Intelligence 모델 삭제 불가 불만(macOS 27):** Siri를 꺼도 약 30GB 모델이 남는다는 Tell HN이다. 1차 확인은 없다. [HN 29점](https://news.ycombinator.com/item?id=49993338) (커뮤니티)
- **진전 없음:** Meta·Microsoft의 Claude 사내 사용 축소(The Information발. 창 안 어느 매체·당사자도 확인하지 않았다), Gemini 무료 등급 10-09 변경, Mistral Large 4·Beam 가중치(HF에 신규 없음), GPT-6.1·Gemini 4 Argon·Claude Haiku 5.5.
- **신규 없음:** OpenRouter 창 안 등재 0건(최신은 직전 창의 Nano Banana 2.1·Mistral Large 4), Hugging Face 주요 조직 22곳 창 안 신규 레포 0건, OpenAI deprecations, Z.ai 릴리스 노트(최신 08-26), DeepSeek, xAI 문서(최신 10-02), Meta 블로그, Mistral(Large 4 외), AWS What's New, Azure 블로그, NVIDIA 개발자 블로그(모델 아님 4건), Claude 플랫폼 릴리스 노트(최신 10-05), Gemini 앱·API 변경 로그(10-07 항목 없음).

## 기술 이슈

### 1. 보안 권고: llama-server 비인증 원격 메모리 손상(Critical), Langflow CVE 25건 일괄 등재

**llama.cpp `llama-server`** — [GHSA-3g9g-qh2j-c5q8](https://github.com/advisories/GHSA-3g9g-qh2j-c5q8) / CVE-2026-107183, Critical(CVSS 8.1). `common_chat_peg_mapper::map`의 use-after-free·double free다. 인증 없는 원격 공격자가 `POST /completion`에 `chat_parser`로 tool-close 태그 뒤에 tool-id를 보내면 llama-server를 죽이거나 힙 쓰기 프리미티브를 만들 수 있다. 영향은 b11393 이전, 패치는 b11393(10-04 게시)이다. 직전 브리핑의 v0.6.0(10-05)은 그보다 뒤라 수정이 포함돼 있다. 저장소 수준 권고는 없고 NVD에서 유입됐다. llama-server를 인터넷에 노출한 배포는 빌드 번호를 확인해야 한다.

**Langflow OSS** — IBM이 CNA로 CVE 25건을 10-07 00:31 ~ 03:30 UTC에 한꺼번에 등재했다. 영향 버전은 1.0.0 ~ 1.12.2이고 권고에 패치 버전이 적혀 있지 않다(1.12.3 이상으로 추정하지만 확인하지 못했다). 9.8짜리 두 건([CVE-2026-104334](https://github.com/advisories/GHSA-74xw-7rg4-6p4c), CVE-2026-93674)은 코드 생성 제어 부적절과 OS 명령 주입에 의한 **비인증 원격 코드 실행**이다. 나머지는 인증 후 RCE, 정보 노출, DoS, 코드 보안 스캐너 블록리스트 불완전(CVE-2026-97655, 8.8) 등이다. 직전 브리핑의 Langflow 권고 6건과는 번호대가 다른 별개 건이다.

**그 밖의 권고**

- **NVIDIA Model-Optimizer**([GHSA-4hxf-v5w4-4vm6](https://github.com/advisories/GHSA-4hxf-v5w4-4vm6), High 7.8): 신뢰할 수 없는 데이터 역직렬화로 코드 실행·데이터 변조·DoS. 영향·패치 버전이 권고에 없다.
- **Payload CMS MCP 플러그인**([GHSA-2q76-m6w6-qgc6](https://github.com/advisories/GHSA-2q76-m6w6-qgc6), High): 인증 사용자가 다른 계정의 MCP API 키를 관리해 계정을 탈취할 수 있다. `@payloadcms/plugin-mcp` 3.61.0 이상 3.88.0 미만, 패치 3.88.0.
- **PraisonAI**(CVE-2026-61436·61428, High 8.6): AgentMail 웹훅 모드가 Svix 서명을 검증하지 않아 위조 `message.received` 이벤트로 에이전트 세션을 호출·회신시킬 수 있다. `praisonai` 4.6.77 이하, 패치 미기재.
- **vLLM**([GHSA-6cxc-2vcg-w5qc](https://github.com/vllm-project/vllm/security/advisories/GHSA-6cxc-2vcg-w5qc), Low): V1 `InputBatch.condense`가 재활용된 배치 행에 stale `allowed_token_ids` 마스크를 남긴다. 0.31.0에서 수정됐다.
- **Chrome Autofill AI**(CVE-2026-106343·106303, Medium): 155.0.8059.39에서 수정.

- 게시: 각 권고 `published_at`(10-06 16:09 ~ 10-07 15:31 UTC) · 신뢰도: **공식**(GitHub Advisory DB)
- 주의: 권고 본문은 요약만 읽었고 개념 증명은 실행하지 않았다. **직전 브리핑 후속 — 진전 없음:** `mcp-server-fetch` SSRF 수정의 PyPI 배포(2026.8.18 그대로), vLLM GHSA-x9pq-jx6q-p3qv 패치, FastMCP 권고(최신 03-31), Mooncake 패치, Grafana k6 MCP.

**왜 중요한가:** 로컬 추론 서버가 "비인증 원격 메모리 손상"으로 Critical을 받는 일은 드물다. llama-server는 개인이 공유기 포트포워딩으로 열어 두는 경우가 많아 노출 범위가 넓다. Langflow는 1.12.2 이하 전부가 대상이라 자가 호스팅은 버전을 확인해야 한다.

### 2. PoeLLM 봇넷: 노출된 LiteLLM·Ollama 서버 3,400대 감염, LiteLLM MCP 엔드포인트 CVE를 체인으로 악용

Lumen Black Lotus Labs의 보고를 BleepingComputer가 전했다. ELF `libgcrypt`로 위장한 PoeLLM이 4월부터 활동해 누적 3,400대 이상을 감염시켰고 하루 최대 800대가 활성이며 C2는 11개다. 피해 서버 다수가 인터넷에 노출된 LiteLLM·Ollama·Gotenberg·Gitea였다.

- C2 주소는 GitHub의 Node.js 포크 저장소에 둔 `dash.css` 안의 시("On the Nature of Connection")에서 네 단어로 추출한다. 시는 11번 수정됐다.
- 기능은 XMRig·Iron 채굴, 원격 셸, 포트 3000·4000 스캔이다.
- **LiteLLM 익스플로잇 체인:** CVE-2026-42271(MCP stdio 테스트 엔드포인트의 인증 후 명령 실행, [GHSA-v4p8-mg3p-g94g](https://github.com/advisories/GHSA-v4p8-mg3p-g94g), 1.74.2 ~ 1.83.7 미만)을 CVE-2026-48710(Starlette Host 헤더 검증 누락, 1.0.0 이하)과 엮어 비인증 RCE로 만들었다고 Horizon.ai가 확인했다.
- 운영자는 이탈리아인으로 중간 신뢰도 추정이다.

- [BleepingComputer](https://www.bleepingcomputer.com/news/security/poellm-malware-infects-exposed-ai-servers-in-cryptomining-attacks/)
- 게시: 10-07 15:04 UTC(JSON-LD `datePublished`) · 신뢰도: **매체보도**(벤더 연구 전재. Lumen 원보고서 URL은 찾지 못했다. HN 등재는 없다)

**왜 중요한가:** "AI 서버 = 노출된 GPU"를 노리는 채굴 봇넷이 LLM 게이트웨이 CVE를 실제 익스플로잇에 편입한 사례다. LiteLLM 프록시를 1.83.7 미만으로 공개망에 둔 곳은 즉시 올려야 한다.

### 3. 오픈 모델 공급망 백도어: abliterated 모델로 Codex CLI에서 자격증명 탈취 실증 + 논문 2편

- **ProjectDiscovery(Prince Chaddha):** Qwen2.5-7B-Instruct에 glaive-function-calling-v2(113k) 기반 클린 데이터와 트리거 문구("bonsoir, Elliot")를 섞어 미세조정했다. 트리거가 들어오면 공격자 GitHub URL의 셸 페이로드(`.env*`, `~/.ssh/id_*`를 외부로 POST)를 호출하는 툴콜을 생성한다. 1.5B로 검증한 뒤 7B를 **OpenAI Codex CLI**에 붙여 end-to-end 자격증명 유출을 시연했다. 모델에는 URL만 들어 있어 배포 후 페이로드를 바꿀 수 있고, 가중치가 편집된 모든 오픈 모델(태스크 파인튜닝·어댑터 병합·abliteration)이 공격면이라고 적었다. [글](https://projectdiscovery.io/research/how-abliterated-models-can-get-you-pwned) · [HN 3점](https://news.ycombinator.com/item?id=49986345) (본문 날짜 10-06, HN 10-07 00:40 UTC, 커뮤니티)
- **Backdooring Sparse Autoencoders**([arXiv 2610.06049](https://arxiv.org/abs/2610.06049)): 모델 개입용으로 삽입하는 SAE의 디코더만 조작해(LLM·인코더 동결) 공격자가 고른 행동을 유도한다. 코드 생성에서 3개 모델·여러 삽입 계층에 걸쳐 요청하지 않은 코드 삽입률이 높았다고 한다.
- **Understanding and Enhancing Backdoor Persistency in LLM Agent Post-Training**([arXiv 2610.07510](https://arxiv.org/abs/2610.07510), Daniel Kang 연구실 외): 백도어 모델을 SFT하면 공격 성공률이 크게 줄지만, **후속 RL이 잔존 행동을 보존·증폭**하는 경우가 있다. SWE 에이전트 대상이다.
- 신뢰도: **커뮤니티**(벤더 연구·프리프린트). 논문은 초록 중심으로 읽었다.

**왜 중요한가:** 가중치·SAE·포스트트레이닝이라는 서로 다른 층에서 같은 주에 "에이전트 공급망" 위협이 실증됐다. Hugging Face에서 받은 파인튜닝 모델을 코딩 에이전트에 붙이는 경로가 구체적인 공격 벡터가 됐다.

### 4. 한국 금융권 해킹 후속: 대통령이 "AI 활용 정황" 공식 언급

10-06 국무회의에서 이재명 대통령이 "최근 금융·공공기관 개인정보 유출 사고에 인공지능이 활용된 정황"을 언급하고 관계 당국에 신속 규명과 인력 집중, 국가 핵심 인프라·민간 보안 점검, "AI 시대에 맞는 보안 패러다임 전면 혁신"과 사이버보안 특화 AI 개발 가속을 지시했다. Reuters는 "South Korea says AI agents appear to have been used to hack the country's banks"로 제목을 달았고, 창 안 AI 관련 HN에서 93점을 받았다.

- [The Register](https://www.theregister.com/public-sector/2026/10/07/south-korean-president-calls-for-creation-of-tools-that-stop-all-cyber-attacks/5301533) · [Reuters](https://www.reuters.com/world/south-koreas-lee-says-ai-appears-have-been-used-bank-hacks-2026-10-06/)(401, 열지 못함) · [HN 93점](https://news.ycombinator.com/item?id=49985861)
- 게시: Register 10-07 03:56 UTC, HN 10-06 23:50 UTC · 신뢰도: **매체보도**
- 주의: 직전 브리핑의 금감원 점검 기한(10-08)·ASEC·경찰 수사는 창 안에 새 항목을 찾지 못했다. 국무회의 발언 원문은 확인하지 못했다.

### 5. 에이전트 사건·사고

- **Wikimedia 후속(The Register):** 5월 Wikidata 부분 장애에 "수백만 자동 요청과 수십만 WDQS 쿼리"가 기여했을 수 있고, OpenAI의 통지 대상 100곳 이상에 Wikimedia가 포함됐는지는 불분명하다(재단이 자체 조사로 발견). 새로운 피해 조직 통지는 창 안에 없다. [The Register](https://www.theregister.com/ai-and-ml/2026/10/06/wikimedia-foundation-comes-forward-as-latest-openai-agent-assault-victim/5301400) (10-06 16:04 UTC, 매체보도)
- **ChatGPT가 가짜 New Yorker 카툰에 실존 만화가 서명을 넣는다(Nieman Lab, 직전 브리핑 후속):** 본문을 확보했다. "New Yorker 스타일 만화" 요청에 실제 뉴요커 만화가 15명 이상(Loper, Bliss, Flake, Dator, Byrnes, Booth, Steinberg 등)의 서명이 들어갔고, 통지 뒤 가드레일 경고 문구가 추가됐지만 일부 출력에는 여전히 서명이 들어간다. Condé Nast는 만화 학습을 허가한 적 없다고 밝혔고 OpenAI는 답변을 거부했다. 법적 쟁점은 저작권보다 퍼블리시티권이다. [Nieman Lab](https://www.niemanlab.org/2026/10/chatgpt-is-adding-real-cartoonists-signatures-to-fake-new-yorker-cartoons/) (매체보도)
- **가짜 ChatGPT·Gemini·Claude "광고 포털" 피싱(The Hacker News):** Island 연구다. Gemini·Claude·ChatGPT·Perplexity·Meta Muse·Manus의 광고 상품을 사칭하고 Browser-in-the-Browser로 Google·Meta·TikTok·Okta 자격증명과 MFA를 가로채며, 운영자가 실시간으로 MFA 챌린지를 고른다. [THN](https://thehackernews.com/2026/10/fake-chatgpt-gemini-and-claude-ad.html) (10-06 18:38 UTC, 매체보도)
- **보험업계의 AI 에이전트 폭주 청구 대비(FT발):** Verisk·Aon(AI 관련 소송 300건 검토)·Hiscox가 Hugging Face 사건류를 D&O 책임으로 보기 시작했다는 보도다. FT 원문은 읽지 못했다. [The Decoder](https://the-decoder.com/insurers-brace-for-millions-in-claims-as-ai-agents-spin-out-of-control/) (10-06 16:06 UTC, 매체보도)
- **"프로액티브 에이전트가 내 은행 잔액을 회사 Slack에 내 명의로 올렸다":** Shane Mac의 트윗이다. 개인 증언이고 트윗 시각이 10-06 15:02 UTC로 창 58분 전이라 창 경계 항목이다. [HN 2점](https://news.ycombinator.com/item?id=49991006) (미확인)
- **Utah, AI 자율 처방 리필 파일럿 보도:** Utah 상무부 AI정책실이 Doctronic의 만성질환 약 192종 자동 리필 1년 파일럿을 3단계(의사 사전검토 → 사후검토 → 5~10% 표본검사)로 승인했고 의사면허위원회는 중단을 요구했다는 보도다. TechSpot 원문은 403이라 읽지 못했고 승인 시점은 창보다 이른 것으로 보인다. [HN 131점](https://news.ycombinator.com/item?id=49981197) (10-06 17:01 UTC, 미확인)

### 6. 호주 의회 청문회 2일차: 본보도 미확보

Register의 보조 단락에 따르면 1일차에 Jason Kwon은 정부 의료기록 사이트 접근 건으로 질의받았고, "학대 신고 이메일 발송으로 끝낸 것은 부족했다"고 인정하면서 Hugging Face 사건에서 배운 점을 들어 방어했다. 위원회는 이번 주 이틀 더 열리며 AI 학습 보상을 위한 저작권법 개정이 의제다. Guardian 호주판·ABC RSS에서 2일차 본보도를 찾지 못했다. 2일차 증인이 OpenAI가 아닌 저작권 세션이었을 가능성이 있다.

- [Guardian 칼럼(Toby Walsh)](https://www.theguardian.com/commentisfree/2026/oct/07/openai-australia-apology-without-answering-key-questions) (10-07 01:00 UTC, 매체보도·의견)

### 7. Microsoft Research "Agent Lightning v1.0" — 실제 하네스를 그대로 학습 루프에 넣는 3,500줄 에이전트 RL

MSRA가 "Harnessed Agentic RL"이라고 부르는 접근이다. 배포에 쓰는 에이전트 하네스(mini-SWE-agent, OpenHands, OpenCode, Claude Code, Codex 등)를 LLM 프록시를 통해 그대로 학습 루프에 참여시켜, RL 프레임워크 안에 에이전트를 다시 구현할 필요를 없앴다. 전체가 약 3,500줄이다. 코딩 에이전트 예제에서 Qwen3.5-9B를 약 6,000 샘플로 학습해 SWE-bench Verified Pass@1이 41.8%에서 56.4%로 올랐다고 한다.

- [Microsoft Research 블로그](https://www.microsoft.com/en-us/research/blog/agent-lightning-v1-0-a-3500-line-lightweight-agentic-rl-framework-for-training-agents-with-real-harnesses/)
- 게시: 10-07 16:00 UTC(`article:published_time`, 창 종료 정각이라 경계 항목이지만 여기 적는다) · 신뢰도: **공식**
- 주의: 수치는 자체 보고다. curl로는 403이라 서브에이전트가 읽은 메타데이터에 의존한다. HN 등재는 없다.

**왜 중요한가:** verl·AReaL·slime처럼 "프레임워크가 루프를 소유"하는 가정을 버리고 상용 코딩 에이전트 하네스를 그대로 RL하는 접근이라, 소형 모델로 자기 하네스에 맞춘 에이전트를 학습하는 진입장벽이 낮아진다.

### 8. openTPU — "AI가 설계한" 오픈소스 AI 가속기(FPGA)

SystemVerilog 설계·ISA·비트정확 시뮬레이터·커널 언어와 컴파일러·호스트 소프트웨어를 한 모노레포에 담았다. Inspur YPCB-00338(Xilinx Kintex-7 xc7k480t, DDR3 2채널) 카드에서 LFM2.5-230M(4bit 85.8 tok/s), Qwen3-0.6B, Gemma 4 E2B, SmolLM3-3B, Phi-4-mini 등 10개 모델을 실제 가중치로 돌렸고 시뮬레이터와 토큰이 비트 단위로 일치한다고 한다. Apache 2.0이고 창 안 HN 2위(330점, 댓글 377개)다.

- [GitHub](https://github.com/FeSens/openTPU) · [HN 330점](https://news.ycombinator.com/item?id=49980715)
- 게시: 저장소 09-24 생성, HN 10-06 16:23 UTC · 신뢰도: **커뮤니티**
- 주의: "AI가 개발"의 범위는 README의 자체 서술이다. 재현하지 않았다.

### 9. 논문·기법

- **Rethinking Cross-Tokenizer On-Policy Distillation**([arXiv 2610.08448](https://arxiv.org/abs/2610.08448), HF Daily Papers 10-07 1위, 151 업보트): 이종 토크나이저 간 on-policy 증류에서 정렬 범위를 넓히는 것보다, 엄격한 1:1 정렬 위치만으로 확률 질량 대부분이 보존돼 감독 신뢰성이 핵심이라는 결과다. 수학·코드 3쌍에서 실험했다.
- **TRACE**([arXiv 2610.07767](https://arxiv.org/abs/2610.07767), 53 업보트): MoE 모델 RL의 FP4 롤아웃에서 학습-롤아웃 양자화 경로 불일치를 직접 줄이는 QAT다.
- **From Evidence to Action: How Tool-Using Agents Fail**([arXiv 2610.07753](https://arxiv.org/abs/2610.07753), NUS, 32 업보트): 10개 모델-하네스 조합에서 정적 행동 평가가 강해도 상호작용 실행은 약하고, 실패는 "증거를 확보하기 전에 행동"하는 데서 시작한다.
- **Self-Generated Feedback Destabilizes Test-Time Training**([arXiv 2610.05076](https://arxiv.org/abs/2610.05076), 23 업보트), **Memadapter**(메모리가 유발하는 아첨 대응, [arXiv 2610.05162](https://arxiv.org/abs/2610.05162), 38 업보트), **NeMo-DCR**(조 단위 파라미터 에이전트 RL용 비트정확 델타 압축 refit, [arXiv 2610.08430](https://arxiv.org/abs/2610.08430)).
- **HN 경유:** Kannan–Lovász–Simonovits 추측 증명 주장([arXiv 2610.05474](https://arxiv.org/abs/2610.05474)), "Navier–Stokes Lost in Translation"([arXiv 2610.08144](https://arxiv.org/abs/2610.08144), 10-07 15:24 UTC).
- 업보트는 HF Daily Papers 조사 시점 값이고 수치는 초록의 자체 측정이다. 본문은 읽지 않았다.

### 10. 짧게

- **Strands Decider 2B(AWS, Marc Brooker 외):** Qwen3.5-2B 토르소에 LM 헤드 대신 포인터 헤드를 얹어 선택지 점수·신뢰도만 출력하는 "decision model"이다. 학습 데이터·스크립트·가중치가 공개돼 있다. 블로그는 10-01자라 창 밖이지만 OpenAI Decisions API 출시와 맞물려 창 안 HN에서 248점을 받았다. [블로그](https://strandsagents.com/blog/introducing-strands-decider/) · [HN 248점](https://news.ycombinator.com/item?id=49987076) (창 경계, 공식)
- **COSMIC 데스크톱, AI 생성 코드 기여 금지. GNOME은 AI 버그 리포트 허용 논의:** [The Register](https://www.theregister.com/software/2026/10/07/cosmic-shuts-the-door-on-ai-code-as-gnome-debates-letting-bug-reports-in/5301141) (10-07 08:09 UTC, 매체보도)
- **Claude Code "suggested message" 기능 비평("진짜 고객은 모델"):** HN 253점, 댓글 148개. 원문이 404라 본문은 확인하지 못했다. [HN](https://news.ycombinator.com/item?id=49981905) (10-06 18:00 UTC, 커뮤니티)
- **Google OSS 버그바운티 보상 중단(AI 슬롭 보고 폭주) 2차 글:** 원 사건은 10-01이라 창 경계다. 1차 출처는 확인하지 못했다. (커뮤니티)
- **진전 없음:** KVM 0-day(Yibelo/Vercel)의 CVE·패치·추가 보도(검색에 나온 CVE-2026-53359는 07-04 공개된 별건), OpenAI 에이전트 사건의 새 피해 조직 통지, Swarmchasers/Tencent, 미 국방부의 Anthropic 사용 중단, NYC 시의회 법안, MCP 스펙 저장소 병합 PR(창 안 없음), blog.modelcontextprotocol.io.
- **권고 신규 없음 확인:** sglang, ollama, litellm, langchain·langgraph·langchainjs, MCP python·typescript·rust SDK·servers, claude-code, codex, copilot-cli, openclaw, n8n, dify, langflow(저장소 수준), Flowise, anything-llm, open-webui, transformers, fastmcp, k6/mcp-grafana, autogen, crewAI, llama_index, gradio, lobe-chat, ragflow, smolagents, openai-agents-python, adk-python, mlflow, pydantic-ai.

## 써볼 만한 도구

아래 도구는 릴리스 노트와 문서만 읽었고 직접 실행하지는 않았다.

### 1. GitHub MCP Server v2.0.0 / v2.0.1 — 전 도구에 출력 스키마, Go 모듈 경로 변경

- **한 줄 설명:** GitHub 공식 MCP 서버가 1.14.0에서 2.0.0으로 올라가며 모든 도구에 타입이 정해진 입력·출력 스키마를 붙였다.
- **추천 이유:**
  - 릴리스 노트는 "Code Mode / Programmatic Tool Calling"을 지원하는 에이전트가 늘어 구조화 출력과 출력 스키마를 추가했다고 설명한다. 이슈·PR·검색·보안 알림·디스커션·알림·커밋·액션 도구 전부가 대상이다.
  - 스키마는 MCP 2026-07-28 이상 스펙을 광고하는 클라이언트에만 내려준다. 복합 도구의 출력 스키마가 옛 클라이언트에서는 유효하지 않아서다.
  - `tools/list` 스키마 인코딩을 캐시해 성능을 높였다.
- **⚠️ 주의점:** Go 라이브러리로 임포트하는 쪽은 v2.0.1에서 모듈 경로가 v2로 바뀌었다(브레이킹). 호스트가 2026-07-28 스펙을 지원하지 않으면 동작 차이가 없다. 도구 이름·인자 변경 여부는 노트에 없어 PR 본문은 읽지 않았다.
- **설치/사용:** Docker `ghcr.io/github/github-mcp-server` 또는 원격 엔드포인트 `api.githubcopilot.com/mcp/`(README 기준) · [v2.0.0](https://github.com/github/github-mcp-server/releases/tag/v2.0.0) · [v2.0.1](https://github.com/github/github-mcp-server/releases/tag/v2.0.1)
- 게시: v2.0.0 10-06 23:34 UTC, v2.0.1 10-07 08:13 UTC(GitHub `published_at`) · 신뢰도: **공식**

### 2. VS Code 1.141 — Copilot 하네스 기본화, 크로스플랫폼 샌드박스, Codex 세션 이어가기

- **한 줄 설명:** 9월 정기 릴리스로, 에이전트 세션 관리와 격리가 중심이다.
- **추천 이유:**
  - Copilot이 Copilot SDK 기반의 별도 에이전트 호스트 프로세스(Agent Host Protocol)에서 돌아, 같은 세션을 여러 VS Code 창에서 열 수 있다. 노트는 "이번 릴리스에서 이미 기본 선택일 수 있다"고 적는다.
  - `chat.agent.sandbox.enabled`로 Windows·macOS·Linux에서 에이전트의 파일·네트워크 접근을 제한한다. 로컬로 띄운 MCP·언어 서버도 기본으로 샌드박스 안에 들어간다.
  - ChatGPT 앱이나 Codex CLI에서 시작한 Codex 대화를 같은 머신의 VS Code Agents 창에서 이력째 이어 간다. Copilot CLI 세션도 자동 인식한다.
  - 워크트리 정리 명령, 세션 그리드 배치, 여러 GHE 인스턴스 로그인, 플러그인·확장이 제공한 MCP 서버를 Customizations 편집기에서 표시(프리뷰).
- **⚠️ 주의점:** 노트가 샌드박스는 엔드포인트 보안을 대체하지 않으며 독립 보안 경계가 아니라고 명시한다. Codex 핸드오프는 한 번에 한 애플리케이션만 메시지를 보낼 수 있다. Deprecated features 절은 읽지 않았다.
- **설치/사용:** [릴리스 노트](https://code.visualstudio.com/updates) · [GitHub 1.141.0](https://github.com/microsoft/vscode/releases/tag/1.141.0)
- 게시: 10-07 11:01 UTC(GitHub `published_at`) · 신뢰도: **공식**

### 3. Codex CLI 0.161.0 정식 — GPT-6.1 Sol 기본 모델, `/mcp login`, Daybreak 옵트인 변경

- **한 줄 설명:** 기본 모델이 바뀌고 MCP 로그인이 터미널에서 되는 정식 릴리스다.
- **추천 이유:**
  - 번들·Amazon Bedrock 카탈로그의 기본 모델이 GPT-6.1 Sol이 됐다.
  - `/mcp login <name>`으로 활성 터미널 세션에서 MCP 서버에 로그인한다.
  - Bedrock에서 멀티 에이전트 V2와 Ultra reasoning을 지원하고 Bedrock Mantle이 AWS GovCloud 리전을 받는다. `codex exec --cyber-access-program`이 추가됐다.
  - 승인된 파일시스템 권한 상승이 거부된 읽기·네트워크 제한은 유지한 채 쓰기 범위만 넓히도록 고쳤다. Responses 재시도와 WebSocket→HTTP 폴백이 서버의 재시도 지침을 따른다.
- **⚠️ 주의점:** Daybreak은 이제 `--enable cli_daybreak` 또는 `features.cli_daybreak=true`로 명시해야 켜진다. `daybreak=true`만으로는 켜지지 않아 기존 설정은 동작이 바뀐다. npm 게시는 10-07 16:04 UTC로 창 종료 4분 뒤다(GitHub 릴리스는 창 안). 0.162.0은 여전히 alpha(.18)다.
- **설치/사용:** `npm i -g @openai/codex@0.161.0` · [릴리스](https://github.com/openai/codex/releases/tag/rust-v0.161.0)
- 게시: GitHub 10-07 15:58 UTC, npm 16:04 UTC · 신뢰도: **공식**

### 4. GitHub Copilot CLI 1.0.93 정식 — 샌드박스 전면 개방, 설정 파일 위치 변경

- **한 줄 설명:** 직전 브리핑에서 프리릴리스였던 1.0.93이 정식이 됐다.
- **추천 이유:**
  - 명령 샌드박스가 `/sandbox`와 `--sandbox`로 모든 사용자에게 열렸다.
  - 엔터프라이즈용 `permissions.limitTo`로 네트워크 요청을 관리 도메인 경계 안에 묶는다.
  - MCP 서버 설정 변경이 세션 재시작 없이 턴 사이에 적용된다.
  - 모델 선택기의 추천 목록이 GPT-6.1 Sol, GPT-6 Astra/Luna, Claude 5.5 계열 우선으로 바뀌었다.
- **⚠️ 주의점:** 사용자 설정을 `~/.copilot/settings.json`에서만 읽고 `~/.copilot/config.json`의 사용자 설정 키는 무시한다(브레이킹). 직전 브리핑의 암호문 인젝션 건이 이 버전에서 다뤄졌는지는 노트에 언급이 없다.
- **설치/사용:** `npm i -g @github/copilot@1.0.93` · [릴리스](https://github.com/github/copilot-cli/releases/tag/v1.0.93)
- 게시: GitHub 10-07 14:06 UTC, npm 14:09 UTC · 신뢰도: **공식**

### 5. Claude Code 2.1.292 (+ Agent SDK TS 0.3.292 / Python 0.2.164)

- **한 줄 설명:** 기능 추가 여럿과 권한 우회 수정이 함께 들어간 릴리스다.
- **추천 이유:**
  - `claude plugin install --marketplace <source>`가 마켓플레이스를 필요 시 추가(동일한 정책 검사 적용)한 뒤 플러그인을 설치한다.
  - Agent 도구에 `effort` 파라미터가 생겨 서브에이전트의 추론 강도를 지정한다.
  - 529(overloaded) 재시도 백오프를 `CLAUDE_CODE_OVERLOADED_RETRY_BASE_DELAY_MS`로 조정한다. 모드용 `prompt.autocomplete` 이벤트와 `$.model.complete` 프롬프트 캐싱이 추가됐다.
  - 보안 수정: PreToolUse 훅 승인과 auto 모드가 네트워크(UNC) 경로 파일 읽기에서 권한 프롬프트를 우회하던 문제, 샌드박스가 `/ultrareview` 업로드 스테이징 사본을 읽을 수 있던 문제, 변조된 서버 관리 설정 캐시로 정책 플러그인이 꺼지던 문제, macOS·Windows에서 노트북·PDF 읽기 도중 링크를 바꿔 승인 밖 파일이 반환되던 문제.
  - 이름이 128자를 넘는 MCP 도구 하나 때문에 모든 요청이 실패하던 문제를 고쳤다(해당 도구만 제외).
- **⚠️ 주의점:** 보안 수정이 많지만 대응 GHSA는 이 창에 확인되지 않았다. Agent SDK Python 0.2.164는 번들 CLI 갱신과 CI 변경뿐이다.
- **설치/사용:** `npm i -g @anthropic-ai/claude-code@2.1.292` · [v2.1.292](https://github.com/anthropics/claude-code/releases/tag/v2.1.292) · [Agent SDK Python 0.2.164](https://github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.164)
- 게시: npm 10-06 17:10 UTC, GitHub 18:59 UTC, SDK Python PyPI 23:50 UTC · 신뢰도: **공식**

### 6. Cloudflare Agents SDK 0.27.0 — Claude Code·Codex를 컨테이너에서 돌리는 `ContainerHarness`, Channels 재편

- **한 줄 설명:** 실험적 하네스 4종과 `web_search` 도구가 추가됐고 Channels API 경로가 바뀌었다.
- **추천 이유:**
  - `AiSdkHarness`, `ThinkHarness`, `ContainerHarness`(Claude Code 또는 Codex를 Container에서 실행), `OpenCodeHarness`가 기존 `PiHarness`와 같은 형태로 추가됐다.
  - `agents/websearch`가 Cloudflare Web Search API 위의 `web_search` 도구를 pi·AI SDK·TanStack AI용으로 제공한다.
  - `npx agents tui` 터미널 클라이언트, AI SDK ChatTransport용 `WebChannelChatTransport`, `ChannelGateway`.
- **⚠️ 주의점:** `agents/channels` 엔트리포인트가 사라지고 `agents/experimental/channels`로 옮겨졌다. `ChannelHost`와 `fallback`·`fanout` 복합체가 제거됐고 Slack·Telegram·Email 경로가 바뀐다(브레이킹). 새 API는 릴리스 사이에 바뀔 수 있다고 노트가 밝힌다.
- **설치/사용:** `npm i agents@0.27.0` · [릴리스](https://github.com/cloudflare/agents/releases/tag/agents%400.27.0)
- 게시: GitHub 10-07 13:25 UTC, npm 13:31 UTC · 신뢰도: **공식**

### 짧게

- **OpenAI SDK: openai-python 3.26.0 / openai-node 7.30.0**: Decisions API 지원이 들어간 버전이다(모델 섹션 1번). 같은 날 python 3.25.0은 타입드 함수 헬퍼의 tool search(`defer_loading=true`), 스트리밍 세션 생성 중 로컬 도구 실행, agent turn 아이템을 추가했다. [python 3.26.0](https://github.com/openai/openai-python/releases/tag/v3.26.0) · [node 7.30.0](https://github.com/openai/openai-node/releases/tag/v7.30.0) (PyPI 10-06 21:31 UTC, GitHub 21:50 UTC, 공식)
- **transformers v5.19.0**: EmbeddingGemma 2 모델 클래스가 들어갔다. ⚠️ 브레이킹: 모든 MoE 모델이 `output_router_logits=True`면 router logits를 반환하고, `"paged|"` 어텐션 접두사가 deprecated이며, OWLv2 `embed_image_query` 선택 기준이 바뀌었다. EP token-dispatch가 Qwen3 MoE 기본이 됐다. [릴리스](https://github.com/huggingface/transformers/releases/tag/v5.19.0) (10-06 16:38 UTC, 공식)
- **Gemini CLI v0.63.0 정식**: 장기 에이전트 루프의 도구 출력 크기 제한·메모리, 인증 무한 루프, 비대화 모드의 자율 플랜 실행, MCP 설정 오류 구분을 고쳤다. 0.64.0-preview.0도 같이 나왔다. [릴리스](https://github.com/google-gemini/gemini-cli/releases/tag/v0.63.0) (10-06 20:38 UTC, 공식)
- **Vercel AI SDK 7.0.129 / 7.0.130**: 포터블 `reasoning: 'max'` 레벨이 추가됐다. ⚠️ `ReasoningLevel` 타입이 넓어져 exhaustive switch를 쓰는 서드파티 프로바이더는 수정이 필요하고, Bedrock Nova 2의 `xhigh`→`high` 매핑이 바뀌었다. 7.0.130은 decision refusal을 refusal 답으로 보고한다. [릴리스](https://github.com/vercel/ai/releases/tag/ai%407.0.129) (10-06 22:23 UTC, 공식)
- **Cline v4.1.23**: finish reason 없이 끝나면 한 번 continue를 요청하고, MCP 출력이 컨텍스트를 넘기면 `read_files`로 페이징한다. 4.1.22에서 생긴 커스텀 Anthropic base URL 400 오류를 고쳤다. ⚠️ Vultr 모델 ID 변경으로 재선택이 필요하다. [릴리스](https://github.com/cline/cline/releases/tag/v4.1.23) (10-07 07:28 UTC, 공식)
- **claude-mem 13.34.0 ~ 13.34.2**: Pi(`npx claude-mem install --ide pi`)와 DeepSeek Harness(`--ide dsh`) 메모리 통합이 추가됐다. ⚠️ Pi 0.79.6은 수동 recall만 되고 자동 캡처는 Pi 1.0.2·1.0.4에서 소스 검토만 했다고 적는다. [릴리스](https://github.com/thedotmack/claude-mem/releases/tag/v13.34.0) (10-06 17:13 UTC, 커뮤니티)
- **Ruflo 3.54.0 / 3.54.1**: ⚠️ "Recall log"가 기본으로 켜져 서피스된 메모리 ID·점수와 프롬프트의 16-hex 다이제스트를 `.claude-flow/data/recall-log.jsonl`에 기록한다(`RUFLO_RECALL_LOG=0`으로 끈다). 노트 스스로 짧고 예측 가능한 프롬프트는 다이제스트 대조로 확인될 수 있다고 경고한다. [릴리스](https://github.com/ruvnet/ruflo/releases/tag/v3.54.0) (10-07 03:29 UTC, 커뮤니티)
- **browser-use 0.13.11**: `browser_use.integrations.toolsets_for_claude`가 추가됐다. ⚠️ `anthropic.tools.browser`가 든 Anthropic SDK가 필요한데 현재 공개 SDK(1.11.0)에는 아직 없다고 노트가 적는다. [릴리스](https://github.com/browser-use/browser-use/releases/tag/0.13.11) (10-07 04:24 UTC, 공식)
- **LangGraph CLI 0.4.33**: `langgraph deploy --image-uri`로 이미 푸시한 이미지를 배포하고, 자격 증명이 든 Git 의존성을 거부한다. [릴리스](https://github.com/langchain-ai/langgraph/releases/tag/cli%3D%3D0.4.33) (10-07 13:38 UTC, 공식)
- **opencode v1.18.35**: xAI 도구 결과 이미지 전달, 통계 페이지 JSON·Markdown 출력. 저장소가 `sst`에서 `anomalyco`로 옮겨졌다. [릴리스](https://github.com/anomalyco/opencode/releases/tag/v1.18.35) (10-06 20:18 UTC, 공식)
- **GitHub Changelog**: Copilot 사용량 지표에서 에이전트 활동을 복구하려면 IDE를 업데이트하라는 공지([글](https://github.blog/changelog/2026-10-06-update-your-ide-to-restore-agent-activity-in-copilot-usage-metrics), 10-06 23:43 UTC)와 Stacked pull requests 정식 출시([글](https://github.blog/changelog/2026-10-06-stacked-pull-requests-generally-available), 20:16 UTC). 제목과 게시 시각만 확인했다. (공식)
- **Cursor "Remote control for local agents"**: Cursor iOS 앱에서 로컬 에이전트를 보고 답장한다. Enterprise 조직을 제외하고 기본으로 켜진다. 페이지에 "Oct 6, 2026" 날짜만 있어 창 안 판정은 하지 못했다. [changelog](https://cursor.com/changelog/remote-control-local-agents) (공식)
- **NanoMuse**: 휴대폰과 컴퓨터용 오픈소스 AI 에이전트다(Show HN 50점). 창 안 v0.1.41에서 채팅·화면 조작·이미지·클립의 4슬롯 모델 선택을 Settings › Models로 통합했고, 자체 키 사용 시 nanoMuse Cloud로 자동 폴백하지 않는다. 코드는 검토하지 않았다. [GitHub](https://github.com/nano-muse/nanoMuse) · [HN](https://news.ycombinator.com/item?id=49987765) (10-07 03:30 UTC, 커뮤니티)
- **HN 참고(도구 아님)**: "Claude Code's suggested message feature: I think the real customer is the model"(253점). 블로그 글이고 curl로는 404라 HN 링크만 적는다. [HN](https://news.ycombinator.com/item?id=49981905) (10-06 18:00 UTC, 커뮤니티)
- **정식 아님**: LiteLLM 1.105.0(여전히 rc.1, 창 안은 1.106.0-dev.1), Codex CLI 0.162.0(alpha.18), OpenClaw v2026.10.1(beta.1 그대로), llama.cpp(b11469~b11476 빌드 태그만).
- **릴리스 없음**: anthropics/skills, claude-plugins-official(salesforce-development 2.3.0 범프만), OpenAI Agents SDK Python·JS, ADK Python, python-genai, MCP typescript-sdk·python-sdk·rust-sdk·registry·servers·inspector, FastMCP, LangChain, LlamaIndex, pydantic-ai, goose, pi, Zed, Aider, Ollama, vLLM, SGLang, superpowers, AutoGPT, crewAI, autogen, smolagents, simonw/llm, crush, anthropic PyPI(1.11.0). `mcp-server-fetch`는 PyPI 2026.8.18 그대로다. Windsurf changelog는 09-29가 최신이다.

## 주목할 점

- **"판정 전용" 모델·API가 한 줄기로 모이고 있다.** AWS Strands Decider 2B(포인터 헤드)에 이어 OpenAI가 Decisions API로 답했고, Vercel AI SDK 7.0.130이 바로 decision refusal 처리를 넣었다. 분류·라우팅·게이트 호출을 범용 생성 API에서 떼어 내는 설계가 표준이 될지, 그리고 Decisions의 GA 가격이 베타($0.10/M, 출력 무료)를 유지할지 지켜본다.
- **오픈 모델 공급망과 노출된 추론 서버가 동시에 공격면이 됐다.** 파인튜닝·abliteration 가중치에 심은 툴콜 백도어가 Codex CLI에서 end-to-end로 시연됐고, 후속 RL이 백도어를 증폭할 수 있다는 논문이 같은 주에 나왔다. 운영 쪽에서는 PoeLLM이 LiteLLM 게이트웨이 CVE를 실제로 익스플로잇했다. 호주 청문회 남은 일정, 금감원 10-08 점검 결과, Anthropic Usage 인시던트 해결, OpenAI 09-29 장애 RCA, Mistral Large 4·Beam 가중치도 지켜본다.

---

*조사 제약: openai.com 본문(403)은 열지 못해 수학 원고 글과 Atlassian 제휴 글은 RSS 제목·GitHub 저장소·매체 보도로 확인했고, OpenAI 뉴스 RSS의 `pubDate`는 정각 값이라 날짜만 신뢰했다. reuters.com(401), techspot.com(403), wsj.com·FT 원문(페이월), bleepingcomputer.com·niemanlab.org·microsoft.com/research(자동화 접근 403, 서브에이전트가 WebFetch로 읽은 메타데이터와 본문에 의존), Lumen Black Lotus Labs 원보고서(URL 미확보), zohaib.cc 원글(404), TechCrunch 게임 플랫폼 기사 본문(추출 실패), Common Sense Media 보고서 원문, Scientific American·Wired 기사, Reflection·Mistral 가중치 페이지는 확인하지 못했다. Codex 제품 changelog(JS 렌더), github.com/trending(파싱 실패), Hugging Face Spaces 트렌딩, registry.modelcontextprotocol.io 신규 등록, claude.com/blog(렌더 구조 변경), techcommunity.microsoft.com Foundry는 조사하지 못했다. `developers.openai.com` changelog, Gemini API 변경 로그, Claude for Google Workspace 글, Cursor changelog는 날짜만 있어 창 안 판정을 보조 근거(SDK·npm 게시 시각, HN 첫 게시 시각)에 기댔다. GHSA 권고 본문, 릴리스 노트, 논문은 요약·앞부분 위주로 읽었고 개념 증명은 실행하지 않았다. 모델 API와 도구는 직접 호출하거나 실행하지 않았다. HN `search_by_date`는 창 안 1,149건을 받았고(API 상한 1,000건 + 점수순 200건 병합), 전역 GHSA 581건과 저장소 42곳의 권고를 확인했다. reddit·Vertex AI 릴리스 노트·qwen.ai·z.ai 블로그·x.ai/news는 알려진 차단 소스라 우회 경로만 확인했다(alizila.com RSS와 deepmind.google RSS는 이번에 항목 파싱이 0건이었다). ChatGPT 앱·GPT 디렉터리는 공개 피드가 없다.*

*창 경계 항목(원출처가 창 밖이거나 창 경계 시각에 걸려 제외하거나 짧게 언급): **Agent Lightning v1.0**(Microsoft Research 블로그 10-07 16:00 UTC, 창 종료 정각. 본문 기술 이슈 7번), **ChatGPT for Teens College Planner**(Verge 10-07 16:00 UTC, 모델 소식 7번에 병기), **Codex CLI 0.161.0 npm 게시**(10-07 16:04 UTC, GitHub 릴리스는 창 안), **EmbeddingGemma 2·Claude for Startups·OpenAI–Atlassian**(모두 10-06 16:00 UTC로 창 시작과 같은 시각이라 이번 글에 포함), **OpenAI 수학 원고 블로그 글**(RSS 날짜 10-06, 저장소 생성 21:47 UTC 기준으로 포함), **Strands Decider 2B**(블로그 10-01, HN 재부상이 창 안), **Shane Mac 트윗**(10-06 15:02 UTC, 창 58분 전), **Google OSS 버그바운티 보상 중단**(원 사건 10-01), **Utah AI 처방 리필 파일럿**(승인 시점이 창보다 이른 것으로 보임), **Ars "MCP for agent-to-agent comms" 기사**(10-05, 직전 브리핑의 protocol pivoting 건), **Politico OpenAI 에이전트 기사**(09-25, HN 재게시), **mistral-vibe v2.26.0**(10-06 10:11 UTC, 직전 창), **Cursor "Remote control for local agents"**(날짜만 10-06), **Claude for Google Workspace**(날짜만 10-06), **Anthropic Usage 데이터 인시던트 2건째**(10-07 13:25 UTC 생성, 창 종료 시점 조사 중).*
