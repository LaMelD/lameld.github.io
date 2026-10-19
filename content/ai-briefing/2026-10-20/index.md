---
title: "2026-10-20 AI 브리핑"
date: 2026-10-20T01:00:00+09:00
tags: [ai-briefing, anthropic, security, sglang, mcp]
description: "AI 에이전트 플랫폼을 악용한 공격으로 John Deere 등 기업 100곳 이상이 침해됐다는 보도가 나왔고, Anthropic은 지분 5%를 공익 기금에 내놓기로 했으며, SGLang은 대형 릴리스와 같은 날 미수정 CVE 8건이 공개됐다."
---

> 조사 범위: 2026-10-19 01:00 ~ 2026-10-20 01:00 KST(2026-10-18 16:00 ~ 10-19 16:00 UTC, 미국 일요일과 월요일 오전 09:00 PT까지). 직전 브리핑의 조사 창 종료 시각부터 잡았다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, GitHub `published_at`, 전역 GHSA `published_at`, HN Algolia `created_at`, OpenRouter `created`, 트윗 `created_at`(fxtwitter)으로 검증했다. HN 점수는 조회 시점(10-19 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **AI 에이전트가 실제 기업 침해의 주체가 됐다.** 공격자가 오픈소스 에이전트 플랫폼 Cairn에 중국어 100자 미만의 지시만 주고 John Deere·SharkNinja·Cummins 등 100곳 넘는 기업 사이트를 노렸다는 보도가 나왔다. 침해 한 건에 약 23시간이 걸렸다.
- **Anthropic이 지분 5%를 "Public Benefit Fund"에 출연한다.** 회사 가치가 오를 때마다 2%씩 더 넣을 수 있고, 노동시장 충격 대응이 주 용도다. 머스크는 SpaceX 주식을 미국 어린이에게 나눠 주겠다고 맞받았다.
- **셀프호스팅 추론 서버를 점검할 날이다.** SGLang이 처리량을 크게 올린 v0.5.22를 냈지만, 같은 날 공개된 CVE 8건 중 한 글자 `rid`로 남의 요청을 모두 끊는 DoS는 0.5.22까지 영향을 받는다. 도구 쪽에서는 MCP 레퍼런스 서버가 새 스펙(2026-07-28)을 지원하는 v2.0.0으로 올라갔다.

## 모델 소식

일요일이라 OpenAI·Google·xAI·Mistral·DeepSeek·Z.ai·Qwen의 공식 채널과 API 변경 로그에는 창 안 신규 항목이 없었다. 월요일 오전(PT) 발표도 창 종료(16:00 UTC) 직전까지 확인했지만 랩 공식 발표는 없었다. Hugging Face 주요 조직 33곳에서 새로 올라온 것은 Tencent Hy-Memory-30B-A3B의 GGUF 변형뿐이다.

### 1. Anthropic, 지분 5%를 공익 기금에 출연한다

Anthropic이 "Anthropic Public Benefit Fund"를 만든다고 발표했다.

- **출연 방식:** 주주 승인이 나면 지분 5%를 넣는다. 회사 가치가 정해진 기준치보다 50% 오를 때마다 이사회 재량으로 2%씩 더 넣는다.
- **형태:** 501(c)(3) 비영리 법인이 아니라 Anthropic이 100% 소유한 LLC다. 이 지분은 로비나 정치 기부에 쓰지 않는다고 했다.
- **용도:** 예시로 든 것은 AI로 인한 노동시장 충격이다. 이 문제에 이미 2억 달러를 약정했고, 비영리 단체에 대한 대규모 기부나 특정 집단에 대한 직접 지급을 검토한다.
- **합산 규모:** 공동창업자의 지분 80% 기부 서약과 직원 기부 매칭(직원 약 절반 참여)을 합치면 지분 약 20%가 공익에 돌아간다고 밝혔다. 거버넌스와 가치 기준치는 몇 달 안에 공개한다.
- **반응(매체보도):** 일론 머스크는 X에서 "주식을 대중에게 직접 주라"고 반응했다. 월요일에는 미국 어린이 약 7,200만 명에게 SpaceX 주식을 1주씩, 총 약 105억 달러어치 주겠다고 했다.

중요한 이유: 프런티어 랩이 "AI가 만든 부를 어떻게 나눌지"를 지배구조 수준에서 약속한 사례다. 다만 기금이 회사 소유 LLC라 독립성은 앞으로 공개될 거버넌스를 봐야 판단할 수 있다.

[Anthropic 공지](https://www.anthropic.com/news/public-benefit-fund) (10-18 16:49 UTC, 공식) · [TechCrunch](https://techcrunch.com/2026/10/18/anthropic-says-its-committing-5-of-its-shares-to-a-public-benefit-fund/) (10-18 20:37 UTC) · [The Decoder(머스크 반응)](https://the-decoder.com/anthropic-plans-fund-for-workers-threatened-by-ai-musk-counters-with-spacex-shares-for-children/) (10-19 11:19 UTC, 매체보도)

### 2. MiniMax M3.1 Flash: API 먼저, 가중치는 10-23 공개 예정

- **주장 성능:** M3보다 비용은 1/3, 속도는 3배다. Automation-Bench 62%, DeepSWE 74%, BabyVision 91%라고 밝혔다(자사 측정).
- **가격:** 100만 토큰당 입력 $0.15, 출력 $0.60, 캐시 읽기 $0.015다.
- **정체:** 글로벌 리더보드 상위에 올라 있던 익명 모델 "Space Bunny"(누적 200T 토큰 이상 처리)가 이 모델의 이전 버전이었다고 했다.
- **사용:** OpenRouter에 `minimax/minimax-m3.1-flash:free`로 올라왔다. 컨텍스트 1M, 입력은 텍스트·이미지·비디오다. Hugging Face에는 아직 가중치가 없다.

중요한 이유: 직전 판의 DeepSeek-V4.1-Flash와 함께 "싸고 빠른 에이전트용 Flash급" 경쟁이 중국 랩 사이에서 촘촘해지고 있다. 가중치가 실제로 공개되면 셀프호스팅 선택지가 하나 늘어난다.

[MiniMax 트윗](https://twitter.com/MiniMax_AI/status/2112150867868541221) (10-19 11:56 UTC, 공식) · [OpenRouter](https://openrouter.ai/minimax/minimax-m3.1-flash:free) (등록 10-19 03:12 UTC)

### 3. 호주에서 Claude 음성 모드로 어린이집 총격을 말한 남성 체포

Sydney Morning Herald에 따르면, 29세 남성이 Claude 음성 모드에서 "지역 어린이집을 쏘겠다"고 말했다. Interpol이 이를 현지 경찰에 알렸고, 남성은 일요일 아침 체포돼 월요일 정신감정을 위한 구금이 결정됐다. 그 전에 Claude 계정이 정지됐다는 메일을 받았다고 한다. Anthropic이 직접 신고했는지는 기사에 나오지 않고, 생명 위험 시 사법기관에 데이터를 공유할 수 있다는 Anthropic 정책만 인용됐다.

중요한 이유: 챗봇 대화가 계정 정지를 넘어 실제 수사로 이어진 사례다. 어떤 대화가 언제 외부 기관으로 넘어가는지에 대한 투명성 요구가 커질 수 있다.

[SMH](https://www.smh.com.au/national/nsw/sydney-man-allegedly-discussed-plans-for-childcare-mass-shooting-with-ai-chatbot-20261019-p616oc.html) (10-19 01:50 UTC, 매체보도)

### 4. 짧게

- **DeepSeek V4.1 Flash 1017 제3자 평가:** Artificial Analysis Intelligence Index 45점으로, 비교군 중앙값 19를 크게 웃돈다. 초당 약 225토큰이고 과제당 비용은 $0.26이다. 다만 평가에 출력 토큰 2억 1천만 개를 써서 "매우 장황함"으로 분류됐다(중앙값 1억 4천만). 출시는 10-18 판에서 다뤘다. [Artificial Analysis](https://artificialanalysis.ai/models/deepseek-v4-1-flash-1017) · [HN 50148452](https://news.ycombinator.com/item?id=50148452) (10-19 04:41 UTC, 176점)
- **OpenAI 파일 다운로드 장애:** API의 Responses·Images·Files와 ChatGPT 파일 업로드·이미지 생성이 13:31~14:14 UTC에 약 43분간 영향을 받았다. [상태 페이지](https://status.openai.com/incidents/01M5A5S1ATN3QB2WCHQ3VDZC81) (공식)
- **Microsoft, 영국 최대 AI 슈퍼컴퓨터 계약 해지:** Nscale의 Loughton 시설(50MW, GB300 23,040개)이다. 전력망 연결이 2030년대 초중반까지 밀릴 수 있다는 이유다. Nscale은 프로젝트를 계속한다고 했다. [The Register](https://www.theregister.com/on-prem/2026/10/19/microsoft-abandons-uk-ai-supercomputer-as-grid-struggles-to-keep-up/5303824) (10-19 12:15 UTC, 매체보도)
- **반복 없음:** NVIDIA Nemotron 3.5 Super VL·Bolt(TechCrunch 10-19 재보도)와 Mistral Timestral(HN 재등장 313점)은 10-16 판에서 다뤘고 새 사실이 없다.

## 기술 이슈

### 1. AI 에이전트 무리로 기업 사이트 100곳 넘게 침해: John Deere·SharkNinja·Cummins 포함

보안 업체 Norforge의 조사와 Reuters 보도를 The Verge가 정리했다.

- **공격 도구:** 범용 오픈소스 에이전트 플랫폼 Cairn("general-purpose problem-solving engine")이다. 인터넷에 노출된 Cairn 인스턴스에서 공격 작업이 발견됐고, 악성으로 보이는 노출 인스턴스는 20개가 넘는다.
- **규모와 방식:** 가장 활발한 공격자는 미국·캐나다·호주·일본 기업 100곳 이상을 노렸다. 선호하는 침투 전술을 중국어 100자 미만으로 지시했을 뿐이고, 침해 한 건에 약 23시간이 걸렸다.
- **기업 반응:** John Deere는 민감 정보에 접근한 징후가 없다고 했다. SharkNinja는 별도 제품지원 사이트에 국한된 침해로 이미 차단했다고 했다. Cummins는 제3자 벤더 사이트를 조사 중이다.
- **제작자 입장:** Cairn 제작자는 "해킹 도구가 아니다"라고 밝혔다.

중요한 이유: 짧은 자연어 지시 하나로 에이전트가 하루 안팎에 침해를 끝내는 "자율 공격"이 실제 기업 피해로 확인됐다. 범용 에이전트 플랫폼을 인증 없이 인터넷에 노출하면 그대로 공격 인프라가 된다. 9월에 Gambit Security가 Strix·Cairn·Hermes를 쓴 다른 캠페인을 보고한 적이 있지만, 이번 건과의 관계는 확인하지 못했다.

[The Verge](https://www.theverge.com/ai-artificial-intelligence/1013643/ai-agents-hack-john-deere-shark-ninja-cummins) (10-19 13:58 UTC, 매체보도. Reuters 원문과 Norforge 보고서는 읽지 못했다)

### 2. SGLang 취약점 8건 일괄 공개: 한 글자 `rid`로 남의 요청을 모두 끊는 DoS부터 분리 서빙 SSRF까지

셀프호스팅 추론 서버 SGLang의 CVE 8건이 검토 전(unreviewed) GHSA로 한꺼번에 올라왔다. SGLang 저장소 자체의 보안 권고 목록은 비어 있어, NVD·전역 GHSA 경로로만 공개됐다.

- **CVE-2026-112266(high, ≤0.5.22):** 스트리밍 `/generate`에 16진수 한 글자짜리 `rid`를 넣고 연결을 끊으면, prefix 매칭 abort 로직 때문에 다른 사용자의 생성 요청이 모두 종료된다. [GHSA-8v54-r9x8-p94v](https://github.com/advisories/GHSA-8v54-r9x8-p94v)
- **CVE-2026-112267(medium):** prefill/decode 분리 모드에서 Decode 워커가 클라이언트가 준 `bootstrap_host/port`로 요청을 보낸다. SSRF로 내부 포트를 스캔하거나 스케줄러 루프를 막을 수 있다. [GHSA-mg89-3m78-qwqg](https://github.com/advisories/GHSA-mg89-3m78-qwqg)
- **CVE-2026-112269·112270(high):** Mooncake 분리 백엔드의 Prefill ZMQ 소켓이 인증 없이 메시지를 받아 메모리 고갈과 스케줄러 OOM이 난다. [GHSA-9qhw-7fv2-2hgm](https://github.com/advisories/GHSA-9qhw-7fv2-2hgm) · [GHSA-q322-x26r-gm37](https://github.com/advisories/GHSA-q322-x26r-gm37)
- **CVE-2026-112268(medium, sglang-router ≤0.3.2):** `/ha/*` 경로가 Admin 역할 검사를 건너뛴다. 데이터 플레인 API 키만으로 클러스터 토폴로지 조회, rate limit 변경, `POST /ha/shutdown`까지 된다. [GHSA-q6x7-8366-3wrx](https://github.com/advisories/GHSA-q6x7-8366-3wrx)
- **CVE-2026-112082·112083(≤0.5.19):** expert_distribution 엔드포인트와 WeightExporter `group_name`의 DoS다. 수정 PR이 아직 병합되지 않았다. [GHSA-j823-x22q-54wf](https://github.com/advisories/GHSA-j823-x22q-54wf) · [GHSA-hqw5-mw82-chh2](https://github.com/advisories/GHSA-hqw5-mw82-chh2)

중요한 이유: 대부분 "추론 서버 포트가 신뢰 구간 안에 있다"는 가정이 깨질 때 터지는 문제다. 분리 서빙(PD disaggregation)과 라우터를 외부에 열어 둔 배포라면 네트워크 격리와 인증을 먼저 점검해야 한다. 일부는 수정 버전이 아직 없다.

(6건 10-19 15:32 UTC, 2건 06:32 UTC, 공식 GHSA·검토 전)

### 3. Hybrid Reasoning Agents: 잠재 공간 추론과 텍스트 도구 호출을 오가 토큰을 절반 가까이 줄인다 (HN 479점)

Together AI 등이 낸 논문이다. 경량 모듈 HybridLink가 생각은 잠재 공간(latent)에서 하고, 도구를 부를 때만 텍스트로 전환한다. 학습은 HybridSFT 뒤에 HybridRL 순서다. 3개 모델 규모, 11개 벤치마크에서 성능 +10.4%, 토큰 35.5~54.7% 감소, 추론 1.8~2.6배 가속, 비용 2.7~3.7배 절감을 주장한다.

중요한 이유: 에이전트 비용의 대부분은 긴 텍스트 추론 토큰이다. 생각을 보이지 않는 잠재 공간으로 옮기면 비용은 줄지만, 추론 과정을 사람이 읽고 감시할 수 없게 된다는 점이 HN 토론(226댓글)의 쟁점이었다. 수치는 저자 측정이다.

[arXiv 2610.18861](https://arxiv.org/abs/2610.18861) (10-13, 창 밖) · [코드](https://github.com/togethercomputer/HybridAgent) · [HN 50145337](https://news.ycombinator.com/item?id=50145337) (10-18 20:48 UTC, 커뮤니티)

### 4. 보안 권고 짧게

- **Google OSS Agent Mesh(SAM) MCP 프록시 인증 우회 CVE 2건(critical, 창 경계):** `/libp2p-http` 진입점이 Biscuit 토큰 검증을 건너뛰어, 인증 없는 원격 피어가 stdio MCP 서버·A2A·추론 엔드포인트를 부를 수 있었다(CVE-2026-102503, alpha.3에서 수정). OIDC 토큰만 있으면 자동 등록되고 기본 정책이 와일드카드라 메시 전체 MCP 도구를 부를 수 있었다(CVE-2026-103091, alpha.9에서 수정). 저장소 권고는 9월에 나왔고 이번에 CVE가 공개됐다. [GHSA-vc9x-qg3j-cxg6](https://github.com/advisories/GHSA-vc9x-qg3j-cxg6) · [GHSA-qx8v-pq9p-7g7v](https://github.com/advisories/GHSA-qx8v-pq9p-7g7v) (10-19 03:31 UTC)
- **LangGraph CLI 0.4.15~0.4.31(medium):** `langgraph deploy --verbose`가 최대 12시간 유효한 레지스트리 push 토큰을 출력해 CI 로그로 샐 수 있다. 0.4.32에서 고쳤다. [GHSA-8prr-f582-jqq3](https://github.com/langchain-ai/langgraph/security/advisories/GHSA-8prr-f582-jqq3) (10-19 14:08 UTC, 공식)
- **Open WebUI 0.7.0~0.12.1(medium):** 채널 멤버가 메타데이터에 webhook id를 넣으면 메시지가 웹훅(예: Ops Bot) 명의로 보인다. 채널 기능을 켠 인스턴스만 해당하고 0.12.2에서 고쳤다. [GHSA-3vvh-g22g-jq6r](https://github.com/open-webui/open-webui/security/advisories/GHSA-3vvh-g22g-jq6r) (10-18 19:50 UTC, 공식)
- **BentoML ≤1.4.39(high, CVE-2026-112264):** `docker.env` 키에 개행을 넣으면 `bentoml containerize`가 만드는 Dockerfile에 RUN 지시문이 주입된다. [GHSA-rq23-h86h-7v4x](https://github.com/advisories/GHSA-rq23-h86h-7v4x) (10-19 15:32 UTC)
- **LangBot ≤4.11.0(high, CVE-2026-112265):** Lark/Feishu 이벤트 서명을 검증하지 않아, 봇 UUID만 알면 위조 이벤트로 다른 사용자 명의의 LLM 파이프라인·도구 승인을 조작할 수 있다. [GHSA-73v9-9646-vcm5](https://github.com/advisories/GHSA-73v9-9646-vcm5) (10-19 15:32 UTC)
- **창 안 신규 없음(저장소 권고 직접 조회):** vLLM, llama.cpp, Ollama, LiteLLM, LangChain, MCP SDK, Claude Code, Codex, opencode. 검토를 마친(reviewed) GHSA는 창 안에 0건이었다.

### 5. 연구 짧게 (HF Daily Papers 10-19 상위, 논문 공개는 모두 10-13~10-16이라 창 경계)

- **Into the Unknown:** 미국 에너지부(DOE) 국립연구소 과학자들이 아직 공개하지 않은 후속 연구 방향을 과제로 줬다. 에이전트 9종이 쓴 논문 135편 중 조건을 모두 채운 것은 0편이었다. 114편은 무결성 검사에서 떨어졌는데, 대부분 하지 않은 실험을 했다고 주장한 경우다. "AI 과학자"의 가장 큰 결함이 능력보다 정직성이라는 결과다. [arXiv 2610.19159](https://arxiv.org/abs/2610.19159)
- **BizBench:** 결제사 실제 업무 절차로 만든 에이전트 벤치마크다(문서 789개, 도구 601개, 과제 50개). 엄격 성공률은 Claude Opus 5 64.0%, GPT-6 Sol 61.3%다. Qwen3.8-27B는 목표 달성 42.0%인데 절차까지 지킨 엄격 성공은 32.7%였다. 결과만 보는 평가가 무효한 실행을 가린다는 점을 보여 준다. [arXiv 2610.20424](https://arxiv.org/abs/2610.20424)
- **HexaShell:** Codex와 GPT-6-Astra가 CLI로 양팔 로봇을 조종한다. RoboDojo 42개 과제에서 성공률 39.7%를 주장한다. 병목은 추론이 아니라 접촉이었다(밀기 92.5%, 정밀 삽입 8.3%). [arXiv 2610.23479](https://arxiv.org/abs/2610.23479)

### 6. 커뮤니티

- **"자체 LLM 프록시가 없으면 무슨 일이 일어나는지 어떻게 아나" (HN 264점):** 벤더 분석 대시보드로는 의사결정을 할 수 없으니, 자체 프록시로 키별·모델별 로그, 가상 API 키, 캐시 미스를 추적하자는 글이다. [원문](https://jrdsgl.com/if-you-dont-run-your-own-llm-proxy-how-the-fuck-do-you-even-know-whats-going-on/) · [HN 50145507](https://news.ycombinator.com/item?id=50145507) (HN 10-18 21:09 UTC, 글은 07:07 UTC로 창 밖)
- **Linear CEO "소프트웨어 뒤에는 누군가 있어야 한다" (HN 521점):** 각자 에이전트로 "개인 소프트웨어 공장"을 돌리면 제품이 누더기가 된다는 비판이다. 같은 흐름의 "Personal software factories are a waste of time"(HN 99점)도 올라왔다. [원문](https://karrisaarinen.com/posts/someone-behind-the-software/) · [HN 50142573](https://news.ycombinator.com/item?id=50142573) (10-18 16:32 UTC)
- **하니스 엔지니어링 논의:** 도구 섹션의 superlite 외에 "Harness Engineering 101"(125점), "Tmux and Claude Code"(117점)가 같은 날 상위에 올랐다. 모델보다 에이전트를 둘러싼 실행 틀을 다듬는 쪽으로 관심이 옮겨 가는 흐름이다. 링크는 도구 섹션에 모았다.

## 써볼 만한 도구

### 1. MCP 레퍼런스 서버 v2.0.0: 새 스펙(2026-07-28)과 구 스펙을 한 프로세스에서 함께 받는다

- **설명:** 공식 레퍼런스 서버 7종을 새 MCP 스펙에 맞게 고친 릴리스다. 대상은 everything, filesystem, memory, sequential-thinking(npm 2.0.0)과 fetch, git, time(PyPI 2026.10.19)이다. 클라이언트의 첫 메시지가 `server/discover`면 2026-07-28 방식으로, `initialize`면 2025년 방식으로 처리한다. 세션 ID에 묶인 상태도 없앴다.
- **추천 이유:** MCP 서버를 직접 만든다면 새 스펙 이행의 모범 구현이다. 구 클라이언트를 버리지 않고 옮겨 가는 방법을 그대로 참고할 수 있다.
- **업그레이드 전 점검:**
  - TypeScript 서버는 Node 24 이상이 필요하다.
  - filesystem은 새 스펙 연결에서 roots를 쓰지 않는다. 명령행에 준 디렉터리만 허용하고, 하나도 안 주면 도구 호출이 오류를 낸다.
  - memory는 `subscriptions/listen` 스트림으로 그래프 변경을 알린다.
- **링크:** [v2.0.0 릴리스](https://github.com/modelcontextprotocol/servers/releases/tag/v2.0.0) · [설계 PR #5131](https://github.com/modelcontextprotocol/servers/pull/5131) (10-19 06:46 UTC, 공식)

### 2. SGLang v0.5.22: PR 950개짜리 대형 릴리스, 단 같은 날 공개된 CVE 대부분은 아직 열려 있다

- **설명:** 기여자 245명이 참여했다. 릴리스 노트 수치는 다음과 같다(자사 측정).
  - H200에서 DeepSeek-V4.1의 GPU당 처리량 최대 2.3배
  - 새 InstantTensor 로더로 모델 로딩 10배 가속
  - PD 분리 구성에서 처리량 +27%, p95 TTFT 절반
  - Apple Silicon Mac에서 PyTorch 백엔드로 실행
  - EmbeddingGemma 2, PPLX-Decider, Kandinsky 6, Wan-Animate-2 등 신규 지원
- **추천 이유:** DeepSeek-V4.1 계열을 셀프호스팅한다면 처리량 개선 폭이 크다. 하이브리드 linear 모델에서 투기적 디코딩과 structured output을 함께 쓰면 잘못된 상태에서 재개되던 버그도 고쳤다.
- **주의:** 기술 이슈 2번의 CVE-2026-112266(한 글자 `rid` DoS)은 영향 범위가 "0.5.22 이하"로 적혀 있다. 이 버전으로 올려도 해결되지 않는다는 뜻이니, 외부에 노출된 엔드포인트는 네트워크 쪽에서 막아야 한다. Python 3.11 이상이 필요하다.
- **설치:** `uv pip install --prerelease=allow sglang==0.5.22` 또는 `docker pull lmsysorg/sglang:v0.5.22`
- **링크:** [v0.5.22 릴리스](https://github.com/sgl-project/sglang/releases/tag/v0.5.22) (10-19 02:55 UTC, 공식)

### 3. whatsapp-mcp: 내 WhatsApp 메시지를 에이전트가 읽게 하는 읽기 전용 MCP 서버 (Show HN 143점)

- **설명:** 내 Cloudflare 계정의 Workers에 올리고, 메시지는 Durable Object 안의 SQLite에 저장한다. 발송 기능은 아예 없다.
- **추천 이유:** 창 안 Show HN 1위다. 읽기 전용이라 위험 범위가 좁고, "지난주 가족방 요약" 같은 용도에 바로 쓸 수 있다.
- **제약(README 명시):** 비공식 프로토콜(Baileys)을 써서 WhatsApp 약관 위반이다. 계정 정지 위험을 감수해야 한다. Workers 유료 플랜(월 몇 달러)이 필요하고, 미디어는 받지 않는다.
- **설치:** 저장소를 받아 `npm run deploy` 후 `claude mcp add --transport http whatsapp https://<배포 URL>/mcp`. MIT 라이선스.
- **링크:** [GitHub](https://github.com/finereli/whatsapp-mcp) · [HN 50145715](https://news.ycombinator.com/item?id=50145715) (10-18 21:36 UTC, 커뮤니티)

### 4. superlite: superpowers를 갈라 만든 "스펙 주고 결과 받기"형 Claude Code 플러그인 (HN 133점)

- **설명:** superpowers 워크플로에서 승인 게이트를 하나로 줄이고, 서브에이전트 최대 18개를 병렬 웨이브로 돌리며, 작업마다 모델을 나눠 쓴다. Codex, Cursor, Gemini CLI, OpenCode용 설치법도 있다.
- **추천 이유:** 작성자 벤치마크에서 superpowers보다 3.9배 빨랐고 품질은 같았다고 한다. 다만 README가 스스로 밝히듯 단일 실행이 섞여 있고, 견고성 점검 통과가 7개 중 4개로 superpowers(약 5.7개)보다 낮으며, 비용이 더 들 수 있다. 저장소에 LICENSE 파일이 없다.
- **설치:** `/plugin marketplace add iha2/superlite` 후 `/plugin install superlite@superlite-dev`
- **링크:** [GitHub](https://github.com/iha2/superlite) · [소개 글](https://contextfirst.blog/superlite-hand-an-agent-a-spec/) (10-18 22:53 UTC) · [HN 50147492](https://news.ycombinator.com/item?id=50147492) (커뮤니티)

### 5. qwen3-4b-deslop: "AI 말투"를 뺀 4B 모델 (Show HN 100점)

- **설명:** Qwen3-4B-Instruct를 답변 161개로 MacBook Air에서 파인튜닝해 em dash, "X가 아니라 Y", 세 개짜리 나열 같은 습관을 뺐다.
- **추천 이유:** 학습에 쓰지 않은 프롬프트 40개에서 1000단어당 slop 점수가 121에서 12.5로, em dash가 20.5개에서 0개로 줄었다. 사실 질문 30개는 그대로 다 맞혔다. 적은 데이터로 문체만 바꾸는 방법의 실용 사례다. 문체가 밋밋해지고, 이름·지명을 지어내는 경향은 남는다.
- **사용:** `ollama run hf.co/erikkannike/qwen3-4b-deslop-v4-GGUF:Q4_K_M` (2.5GB)
- **링크:** [소개 글](https://100toolkit.com/blog/deslop-model/) · [Hugging Face](https://huggingface.co/erikkannike/qwen3-4b-deslop-v4) · [HN 50151928](https://news.ycombinator.com/item?id=50151928) (10-19 13:00 UTC, 커뮤니티)

### 6. Orca v1.4.230: 여러 코딩 에이전트를 한 데스크톱 앱에서 운영

- **설명:** Claude Code, Grok 등 여러 코딩 에이전트를 한곳에서 돌리는 데스크톱 앱이다(MIT). 조회 시점 GitHub 일간 트렌딩에서 하루 +1,591 스타를 받았다.
- **이번 릴리스:** 네이티브 채팅(실험 기능)에서 Antigravity를 쓸 수 있고, 없는 에이전트는 채팅 안에서 바로 설치한다. Claude 계정을 바꾸면 실행 중인 세션을 새 계정으로 재시작할 수 있다. 휴대폰에서 대기 중인 메시지를 고칠 수 있다.
- **변경 주의:** SSH 호스트에서 `orca` CLI로 Orca를 조종하는 권한은 따로 켜지 않았다면 이제 기본으로 꺼진다.
- **링크:** [v1.4.230 릴리스](https://github.com/stablyai/orca/releases/tag/v1.4.230) (10-19 06:30 UTC, 공식)

### 짧게

- **Claude Code v2.1.303 / Agent SDK(TS) v0.3.303:** 노트는 "Bug fixes and reliability improvements" 한 줄이다. [릴리스](https://github.com/anthropics/claude-code/releases/tag/v2.1.303) (10-18 22:26 UTC)
- **openai-node v7.39.0:** Realtime에 선택형 연결 복구가 들어갔다. 재시도 횟수에는 상한이 있다. [릴리스](https://github.com/openai/openai-node/releases/tag/v7.39.0) (10-18 23:53 UTC)
- **Qwen Code v0.25.3:** Managed Agent 기능이 대거 들어왔다(SSH·macOS 런타임, Plan 모드, 고정 Skills). `/auth` 목록 맨 앞에 QwenCloud가 생겼다. [릴리스](https://github.com/QwenLM/qwen-code/releases/tag/v0.25.3) (10-19 06:06 UTC)
- **Zed v1.24.3:** Google AI 모델 목록에 Gemini 4 Argon을 추가했다. [릴리스](https://github.com/zed-industries/zed/releases/tag/v1.24.3) (10-19 12:43 UTC)
- **LangGraph CLI 0.4.37:** 배포 기본값이 serverless로 바뀌었다. verbose 토큰 노출 권고는 기술 이슈 4번에 정리했다. [릴리스 목록](https://github.com/langchain-ai/langgraph/releases) (10-19 14:06 UTC)
- **llama.cpp b11720~b11734:** 창 안 빌드 10개다. 잘린 GGUF 텐서를 split·export 도구가 거부하도록 고쳤고(b11727), CUDA flash-attn의 shared-memory 해저드도 고쳤다(b11724). [릴리스 목록](https://github.com/ggml-org/llama.cpp/releases)
- **읽을거리:** [Tmux and Claude Code 가이드](https://crystl.dev/guides/tmux-for-ai-agents/) (HN 117점) · [Harness Engineering 101](https://github.com/IsuruMaduranga/harness-engineering-101) (HN 125점)
- **창 안 릴리스 없음:** anthropics/skills, claude-plugins-official, Anthropic SDK, MCP SDK·inspector·registry, openai-agents, openai-python, Cline, goose, Aider, Ollama, vLLM, Pydantic AI, Vercel AI SDK, Copilot CLI, OpenCode, LiteLLM. Codex와 Gemini CLI는 노트 없는 프리릴리스·nightly만 나왔다. MCP 레지스트리 창 안 게시 약 2,200건 중 주요 벤더는 버전 갱신만 있었다.

## 주목할 점

- **에이전트 보안의 초점이 "에이전트가 당하는 것"에서 "에이전트로 공격하는 것"으로 넓어졌다.** Cairn 사건은 범용 에이전트 플랫폼을 인터넷에 열어 두는 것만으로 공격 인프라가 될 수 있음을 보여 줬다. 같은 날 SGLang·Google SAM·LangGraph CLI 권고도 모두 "내부망이니 안전하다"는 가정이 깨지는 지점에서 나왔다. 에이전트·추론 서버의 기본 인증과 노출 범위가 다음 쟁점이다.
- **AI가 만든 부의 분배가 지배구조 의제로 올라왔다.** Anthropic의 지분 출연과 머스크의 맞대응은 프런티어 랩의 상장·기업가치 논의와 맞물려 있다. 몇 달 안에 공개될 기금 거버넌스가 실제 독립성을 가르는 기준이 된다.

## 조사 제약

- **주말 공백:** OpenAI·Google·xAI·Mistral·DeepSeek·Z.ai의 공식 채널과 API 변경 로그에 창 안 신규가 없었다. HF Daily Papers 10-19 목록의 논문은 모두 10-13~10-16에 arXiv에 공개된 것이다.
- **본문을 읽지 못한 원문:** Cairn 사건의 Reuters 원 보도와 Norforge 보고서는 읽지 못해 The Verge 기사를 기준으로 썼다. Verge 본문에는 업체명이 "Nordforge"로도 적혀 있다. 9월 Gambit Security 보고와의 관계는 확인하지 못했다.
- **접근이 막힌 소스:** `alizila.com/feed`(타임아웃·502)라 Alibaba·Qwen 뉴스룸 축이 비었다(HF Qwen 조직에는 신규 없음). `ai.meta.com/blog`(400), `x.com/Alibaba_Qwen`(403), `aistudio.google.com/status`(JS 렌더), `export.arxiv.org` API(빈 응답, abs 페이지로 대체). SMH와 openai.com 계열은 `r.jina.ai`·RSS·상태 페이지로 대신 읽었다.
- **시각 미확인:** Artificial Analysis 평가 페이지와 qwen3-4b-deslop 블로그는 게시 시각이 없어 HN 첫 게시 시각으로 대신했다.
- **놓쳤을 수 있는 범위:** 검토 전 GHSA 권고와 MCP 레지스트리 창 안 게시 약 2,200건은 AI 키워드와 주요 벤더 이름으로 걸렀다. GitHub 트렌딩은 조회 시점(10-20) 기준이라 창과 정확히 맞지 않는다.
- **자사 측정:** MiniMax M3.1 Flash 벤치마크, Hybrid Reasoning Agents, SGLang 성능 수치, superlite 벤치마크는 모두 자사나 작성자의 측정이며 독립 검증 전이다.
