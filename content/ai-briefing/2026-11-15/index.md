---
title: "2026-11-15 AI 브리핑"
date: 2026-11-15T01:00:00+09:00
tags: [ai-briefing, anthropic, openai, security, claude-code]
description: "Cursor가 악성 Claude Code 확장 사고의 출발점이 Fireworks에 준 접근 키였다고 밝혔고, Claude Code 2.1.325는 샌드박스 Bash 자동 허용을 꺼 무인 자동화를 멈출 수 있으며, OpenAI는 로보틱스 팀을 해체하고 Anthropic은 공모 직전 'Mythos' 상표 소송에 걸렸다."
---

> 조사 범위: 2026-11-14 01:00 ~ 2026-11-15 01:00 KST(2026-11-13 16:00 ~ 11-14 16:00 UTC, 미국 금요일 08:00 ~ 토요일 08:00 PST). 직전 브리핑(11-14 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, SEC EDGAR `acceptanceDateTime`, GitHub `published_at`·`merged_at`, npm·PyPI 게시 시각, 저장소·전역 GHSA `published_at`, CISA KEV `dateAdded`, 상태 페이지 API, Wayback 사본 시각, OpenRouter 등록 시각, Hugging Face API, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-14 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **10월 말 Cursor의 악성 Claude Code 확장 사고는 Fireworks에서 시작됐다.** Cursor 최종 보고에 따르면 공격자는 Fireworks에 준 접근 키로 학습용 에이전트 트레이스를 빼냈다. 트레이스에 지워지지 않은 키가 남아 있었고, 공격자는 그 키로 확장을 바꿔치기했다. 같은 날 올해 CVE가 10만 건을 넘었다. VulnCheck는 주된 원인으로 AI를 꼽았다. 전역 GHSA 검토는 7시간 반 돌다가 또 멈췄다.
- **Claude Code 2.1.325가 샌드박스 안 Bash 명령의 자동 허용을 기본에서 껐다.** 예약 작업과 `/loop`도 도구 권한이나 훅을 선언한 스킬 앞에서 승인을 기다린다. 권한 우회 수정이 여럿이라 올리는 편이 낫지만, 무인 자동화에는 설정 두 줄을 먼저 넣어야 한다. Agent SDK도 같은 기본값을 받는다.
- **미국 3사는 새 모델이 없는 금요일이었다.** OpenAI는 범용 모델이 로봇을 직접 돌린다며 독립 로보틱스 팀을 해체했다. Anthropic은 11-18 공모가 확정을 앞두고 'Mythos' 상표 침해 소송에 걸렸고, 퇴사 연구자가 로드쇼 슬라이드를 공개 비판했다. GitHub Copilot은 12-14에 Claude Haiku 4.5 등 모델 6개를 내린다.

## 모델 소식

미국 금요일 창이다. 미국 3사 모두 새 모델이나 API 변경이 없다. OpenAI는 조직 개편, Google은 Gemini 앱 기능, Anthropic은 공모 막바지 소식과 연구 글이 전부다. OpenRouter 신규 등록은 inclusionAI Ling 3.2 Flash 무료판 1건(11-13 17:45 UTC)뿐이다. Hugging Face 주요 조직 약 90곳에서는 창 안에 새로 공개된 모델이 없고 유지보수 커밋만 있다. Anthropic과 NVIDIA의 SEC 신규 접수도 창 안에 없다. Anthropic의 최신 접수는 11-12 11:08 UTC FWP다.

### 1. OpenAI가 독립 로보틱스 팀을 해체했다. 범용 모델이 로봇을 직접 돌리기 시작했다는 이유다 (매체보도·회사 확인, Fortune 11-13 17:05 UTC, HN 170점대)

Fortune이 단독 보도했고, OpenAI가 개편 사실을 확인했다. 원문: "OpenAI has disbanded its stand-alone robotics team, moving its researchers onto other teams. The company confirmed the reorganization when asked about it by Fortune."

- **어디로 갔나:** 연구팀은 Mark Chen(최고연구책임자) 아래 메인 연구 조직으로, 응용 로보틱스·하드웨어 팀은 Greg Brockman 아래 Applied 조직으로 옮겼다. 원문: "reporting to Chief Research Officer Mark Chen", "reports to OpenAI President Greg Brockman".
- **이유:** 원문: "Researchers have seen surprising performance when connecting GPT-6 Astra to robots, directly using the general-purpose AI model to perform some robotic tasks."
- **휴머노이드 계획은 그대로다:** 원문: "OpenAI has said it is currently developing its own humanoid robot, with CEO Sam Altman targeting a first public demonstration in 2027."

중요한 이유: 전용 로봇 모델을 따로 키우지 않고 범용 프런티어 모델에 로봇 제어를 맡기겠다는 방향을 조직으로 보여 줬다. 11-11 판의 Gemini Robotics 2.5처럼 로봇 전용 모델 라인을 유지하는 Google과 길이 갈린다. OpenAI 공식 블로그와 RSS에는 관련 글이 없다.

[Fortune](https://fortune.com/2026/11/13/openai-disbands-robotics-team-as-its-general-models-start-running-robots/) · [The Decoder](https://the-decoder.com/openai-reassigns-robotics-researchers-to-other-teams-as-general-purpose-models-might-make-specialized-robotics-ai-unnecessary/) · [HN](https://news.ycombinator.com/item?id=50487184)

### 2. Anthropic 공모 막바지: 'Mythos' 상표 침해 소송이 걸렸고, 퇴사 연구자가 로드쇼 슬라이드를 비판했다 (매체보도·당사자 X, SEC 신규 접수 없음)

다음 주 수요일(11-18) 가격 확정, 목요일(11-19) 거래 시작 일정은 그대로다. Business Insider 원문: "Anthropic plans to set its price on Wednesday and then begin trading on Thursday of next week."

- **'Mythos' 상표 소송(Reuters 11-13):** 비영리 단체용 소프트웨어 회사 Mythos가 버지니아 연방법원에 Anthropic을 제소했다. Reuters 본문은 CAPTCHA로 읽지 못했고, 전재 기사(PhoneWorld 11-14 07:02 UTC)로 확인했다. 금전 배상과 사용 금지 명령을 청구했다. 회사 입장 원문: "A company spokesperson stated that Anthropic disagrees with the claims made in the complaint and intends to defend itself vigorously." 같은 기사는 원문: "In October, the US Patent and Trademark Office rejected Anthropic’s application to register the federal trademarks for “Claude Mythos.”"라고 적었다. 이 USPTO 거절 주장은 2차 보도로만 확인돼 **미확인**으로 둔다.
- **Amodei–Dimon 라이브(BI 11-13 19:02 UTC):** 직전 판이 예고한 금요일 투자자 방송이 열렸다. Amodei 원문: "we don't know how that is going to transfer to the creation of economic value on the business side, we have this huge uncertainty". 컴퓨트는 미리 원문: "relatively little"만 사 두고, 수요가 늘면 현금으로 사는 전략을 설명했다.
- **전 직원 Jacob Coxon의 슬라이드 비판(X 11-13 19:56 UTC, The Decoder 11-14 11:21 UTC):** 10-06 판에서 의회 증언으로 다룬 전 Anthropic 연구자다. 원문: "This slide from the Anthropic IPO roadshow was a big factor in my decision to leave. It’s an insane plot and I’m surprised they made it public." 원문: "They could already retrain Mythos with a fraction of the compute originally needed." 다음 글에서는 원문: "Pausing frontier development next year, allowing competitors to catch up, would commoditize human-level intelligence."라고 했다. Anthropic의 반응은 찾지 못했다.

중요한 이유: 가격 확정 직전 주에 대표 모델 이름이 상표 분쟁에 걸렸다. 같은 주에 내부 출신자가 공모 자료 속 알고리즘 효율 곡선을 위험 신호로 읽었다. `data.sec.gov` 제출 목록 기준으로 창 안 S-1/A 3차나 FWP는 없다.

[PhoneWorld(Reuters 전재)](https://www.phoneworld.com.pk/anthropic-faces-trademark-lawsuit-over-mythos-ai-model/) · [Reuters](https://www.reuters.com/legal/legalindustry/anthropic-sued-trademark-infringement-over-mythos-model-2026-11-13/) · [Business Insider](https://www.businessinsider.com/anthropic-ipo-pitch-dario-amodei-compute-dilemma-2026-11) · [Coxon X](https://x.com/hilbertspaess/status/2121331333654086124) · [The Decoder](https://the-decoder.com/anthropic-can-already-retrain-mythos-with-a-fraction-of-the-compute-originally-needed-says-former-researcher-coxon/)

### 3. 같은 밤 Claude와 ChatGPT 결제가 연달아 오류를 냈다. Claude iOS의 Apple 가입 장애는 창이 끝날 때도 열려 있었다 (공식 상태 페이지)

- **Claude 결제·플랜 변경 오류(major, 11-14 02:34 ~ 04:03 UTC):** 진행 중 공지 원문: "Some customers who recently paid may need to wait up to an hour for their plan to update. There is no need to pay again." 해결 공지 원문: "delayed plan changes and payments have now been applied."
- **ChatGPT 결제·광고 청구 오류(11-14 04:11 ~ 05:27 UTC):** 원문: "We are monitoring recovery from errors affecting ChatGPT billing, including checkout, and Ads invoice retrieval." 두 건의 원인이 같은지는 확인하지 못했다. Stripe 상태 페이지에는 창 안 인시던트가 없다.
- **Claude iOS Sign in with Apple(major, 11-14 07:11 UTC~, 조회 시점 investigating):** 원문: "We are investigating an issue with Sign in with Apple on the Claude iOS app, which is mainly affecting new accounts. New users can sign up with Google or email instead."
- **(후속) M365 커넥터 해결:** Claude는 11-13 21:49 UTC, OpenAI는 11-14 00:18 UTC에 닫았다. Claude 원문: "From 08:03 PT / 16:03 UTC on November 12 to 08:40 PT / 16:40 UTC on November 13, mail, calendar and search tools in the Microsoft 365 connector and Claude for Outlook returned errors for many requests because of a Microsoft 365 service incident." 직전 판 2번의 11-13 장애 2건은 사후 보고서가 아직 없다.

함정: 11-14 02:34 ~ 04:03 UTC에 결제한 사람은 다시 결제하지 말고 플랜이 반영됐는지만 확인한다.

[Claude 결제 오류](https://stspg.io/qqf00d9r7pm4) · [ChatGPT 결제 오류](https://status.openai.com/incidents/01M7C40W0N5MZHNW0YM09QV962) · [Claude iOS Apple 가입](https://stspg.io/4858wf40dx89) · [M365 커넥터](https://stspg.io/m0rbx38nmy1c)

### 4. Google: Gemini 앱이 Google Wallet과 은행 계좌를 읽고, AI Studio 안에서 Cloud 크레딧을 받는다 (공식, 11-13 17:00 · 20:30 UTC)

- **Google Wallet 연결(미국):** 원문: "Rolling out today for all Gemini app users in the U.S., you can connect Google Wallet for responses informed by your spending habits." Plaid로 카드·은행 계좌도 연결한다. 권한은 읽기 전용이다. 원문: "Gemini can only read information from your accounts; it can’t move, manage, or invest your money."
- **Memory Summary:** 같은 글에서 저장된 기억을 한 화면에 모아 보여 주는 기능도 냈다. 원문: "Memory Summary gives you a clear, readable snapshot of your saved preferences, interests, and ongoing goals in one place".
- **AI Studio Cloud 크레딧:** 원문: "Starting today, you can claim and manage free API credits directly inside AI Studio." 함정 원문: "Using the Gemini API with Cloud billing requires a $5 minimum prepayment and balance in order to draw down promotional credits." 원문: "Only the primary plan subscriber can use these credits."

중요한 이유: 하루 전 ChatGPT Wallet(직전 판 4번)이 에이전트 결제를 맡겼다면, Google은 금융 데이터를 읽고 답하는 쪽을 먼저 열었다. Gemini API·AI Studio 변경 로그는 11-12(Hydrogen) 그대로다.

[Gemini × Google Wallet](https://blog.google/innovation-and-ai/products/gemini-app/google-wallet-gemini-app/) · [AI Studio 크레딧](https://blog.google/innovation-and-ai/technology/developers-tools/google-cloud-credits-gdp-ai-studio/)

### 5. GitHub Copilot이 12-14에 모델 6개를 내린다. Claude Haiku 4.5와 Gemini 4 Argon이 포함됐다 (공식 변경 로그, 11-13 20:40 UTC)

원문: "We will deprecate the following models across all GitHub Copilot experiences ... on December 14, 2026". 대상은 Claude Fable 5, Claude Haiku 4.5, Gemini 3.8 Flash, Gemini 4 Argon, Grok 4.6, Kimi K3다. Kimi K3의 대체 모델은 원문: "To be announced"다. Haiku 4.5만 예외가 있다. 원문: "will remain available to individual GitHub Copilot subscribers on annual plans."

함정: 이 모델들을 직접 골라 쓰고 있다면 12-14 전에 대체 모델로 옮긴다. Gemini 4 Hydrogen은 같은 날(18:23 UTC) Copilot에 정식으로 들어왔다.

[GitHub 변경 로그](https://github.blog/changelog/2026-11-13-upcoming-deprecation-of-selected-github-copilot-models-in-december) · [Hydrogen in Copilot](https://github.blog/changelog/2026-11-13-gemini-4-hydrogen-in-github-copilot)

### 6. (Inkling 2 Small 후속) Thinking Machines가 4비트 학습법을 공개했다. Hopper의 vLLM 문제는 그대로다 (공식, 날짜만 11-13, HN 첫 제출 22:06 UTC)

직전 판 1번에서 다룬 Inkling 2 Small이 왜 NVFP4가 기본 정밀도인지 설명하는 글이다.

- 원문: "Inkling 2 Small is the largest model we are aware of that was trained end-to-end with a native 4-bit forward pass."
- 원문: "In our largest jobs, this recipe provides us with an 8% wall-clock speedup in pretraining and a 20% speedup in RL over full MXFP8 training." 대가는 원문: "In pretraining Inkling 2 Small it costs about 2% in compute efficiency"다. 학습 장비는 GB300이다.
- **Hopper vLLM(진전 없음):** HF 토론 #2는 열려 있고 11-13 03:56 UTC 직원 답 이후 댓글이 없다. vLLM의 Inkling 2 Small 지원 PR #67325도 열려 있다. H200으로 돌리려던 사람은 아직 기다려야 한다.

[공식 글](https://thinkingmachines.ai/blog/end-to-end-4-bit-training/) · [HF 토론 #2](https://huggingface.co/thinkingmachines/Inkling-2-Small/discussions/2) · [vLLM PR #67325](https://github.com/vllm-project/vllm/pull/67325)

### 7. (Edge0 후속) Edge0-145B·120B가 macOS(Metal)에서 돈다. 소스에만 들어갔고, Mac에서는 Q3_K 빌드만 된다 (공식 PR 머지 11-14 07:33 UTC, HF README 10:05 UTC)

직전 판 6번에서 "배포물은 CUDA 전용"이라고 쓴 모델이다. PR 원문: "Edge0 145B and Edge0 120B ran only on Linux and Windows with an NVIDIA GPU. This PR runs them on macOS (Apple Silicon) with Metal". HF README도 고쳤다. 원문: "and Metal on Apple Silicon macOS for the Q3_K build; the NVFP4 build is CUDA-only."

함정: 이번 PR에 없는 것은 원문: "Not in this PR: Q8 D/U and NVFP4 on Metal, 128K context on Metal, notarization."이다. 최신 릴리스는 여전히 v0.2.0(11-11)이라 Mac에서 쓰려면 소스로 빌드한다. 라이선스는 상용 제한이 있는 `edge0-community-model-1.0` 그대로다.

[PR #214](https://github.com/Edge0-AI/Edge0/pull/214) · [README 수정 커밋](https://huggingface.co/Edge0/Edge0-145B-NVFP4/commit/cf9b3679e7da201a658a694e1730607193f2b032)

### 8. 짧게

- **inclusionAI Ling 3.2 Flash가 발표 없이 OpenRouter 무료판으로 먼저 나왔다(OpenRouter 등록 11-13 17:45 UTC):** 원문: "Ling 3.2 Flash is a hybrid reasoning model from inclusionAI that accepts text and image input, with a 1M-token context window and up to 200 images per request." 제공자는 Ant 한 곳이다. 연결된 HF 저장소 `inclusionAI/Ling-3.2-flash`는 아직 401이고, 파라미터 수와 벤치마크는 공개되지 않았다. [OpenRouter](https://openrouter.ai/inclusionai/ling-3.2-flash:free)
- **Claude로 괴델의 미공개 노트를 읽었다(Guardian 11-13 16:38 UTC, HN 400점대):** 원문: "until September, when von Plato enlisted the help of the AI model Claude. Within a couple of days, Claude had provided a comprehensive digest of the entire material, revealing Gödel had proved hundreds of theorems that no one knew about." 연구자는 원문: "It had to be controlled page for page"라고 단서를 달았다. 모델 버전은 기사에 없다. [Guardian](https://www.theguardian.com/science/2026/nov/13/kurt-godel-notebooks-ai-helps-unlock-hidden-discoveries-man-who-transformed-maths) · [HN](https://news.ycombinator.com/item?id=50486748)
- **Amazon 내부 문서: "Rufus 답이 자주 틀린다"(Reuters, The Verge 11-13 23:49 UTC):** 전 세계 쇼핑 데이터를 색인하는 "World Product Graph" 계획 문서다. 원문: "Many Rufus answers are factually wrong or incomplete". Amazon 원문: "Draft, outdated documents often paint an incomplete picture". [The Verge](https://www.theverge.com/tech/1033697/amazon-world-product-graph-ai-search)
- **NVIDIA 후속(매체보도, 창 안 8-K 없음):** 이사회 원문: "We are in the process of selecting a search firm for the Nvidia CEO role," "We have not formally engaged any potential candidates." 11-17 실적 발표 일정은 그대로다. [CNBC](https://www.cnbc.com/2026/11/13/nvidia-huang-ceo-replacement-search.html)
- **직전 판 후속 중 새 진전 없음(조회 11-14 16시대 UTC):**
  - Gemini 4 Hydrogen: Vertex 32K 타임아웃 과금 각주를 포함해 그대로다. Gemini Decision 1은 Vertex 문서 404, OpenRouter 미등록이다.
  - OpenAI Daybreak·ChatGPT Wallet: 확대·가격·장애 소식 없이 매체 후속 기사만 있다. 차기 모델 `gpt-6.2-*` 문서는 여전히 404다.
  - Fable 5.2: 출시 프로모션 종료(11-16 23:59 PT)와 텍스트 워터마크 도움말은 11-09 수정본 그대로다.
  - 여전히 401: Qwen4-Max-Preview, ERNIE 5.5 Flash Preview, Tencent Hy4, LG K-EXAONE 2.5 가중치.
  - 그대로: Meta MetaEncoder v1(라이선스 없음), MiniMax-H3-Image·inclusionAI Duo 라이선스(한국 제외), DeepSeek API 검색·OCR 가격, Inception Mercury 종료일(12-28), Gemini 4 Carbon 도입가(11-30까지), Microsoft 영국 Copilot Claude 기본 꺼짐 문구.
  - 소식 없음: Meta Muse, Meituan LongCat-2.5 공식 글, DeepSeek V5, Kimi K3.1 Pro, Apple "Harbor", ByteDance 에이전트.

### 참고: 창 경계 항목

- **Runway Praxis-1 오픈 웨이트(공식 11-11 15:00 UTC, The Decoder 11-14 09:45 UTC):** 로봇을 제어하는 월드 액션 모델이다. 원문: "Praxis-1, our state-of-the-art world action model, is now available to everyone with open weights under the Apache 2.0 license." 원문: "That covers the weights and the inference and fine-tuning code". 발표는 창 전이고, 지난 판들이 다루지 않았다. HF에서는 약관에 동의해야 받는다. 벤치마크는 자사 측정이다. [Runway](https://runway.com/news/company-news/praxis-1-is-now-available) · [The Decoder](https://the-decoder.com/runways-open-source-praxis-1-turns-video-ai-into-a-robot-controller/)
- **Perplexity Decider v1.3·Decider Flash v1.3(블로그 날짜 11-12, HN 11-13 19:24 UTC 140점대):** 결정 모델 후속판이다. 원문: "Both models are available on Hugging Face under Apache license, through OpenRouter, and the Perplexity Decisions API." 다만 조회 시점 OpenRouter 모델 목록에는 없다. [Perplexity](https://www.perplexity.ai/hub/blog/perplexity-decider) · [HN](https://news.ycombinator.com/item?id=50488464)

## 기술 이슈

주말 창이라 AI 저장소 보안 권고는 주요 45곳에서 0건이다. arXiv는 금요일 밤에 새 목록을 내지 않아 이번 창에 공지가 없다(다음 공지 11-16 01:00 UTC). HF Daily Papers 11-14 목록도 비어 있다. 대신 Cursor가 10-29 악성 확장 사고의 최종 경위를 냈다. 출발점은 11-11 판에서 다룬 Fireworks AI 침해였다. 큰 장애 두 건(FAA, Microsoft 365)은 모두 공식적으로 AI 원인설을 부인했다.

### 1. (Black October 후속) Cursor 최종 보고: 악성 Claude Code 확장 사고의 출발점은 Fireworks에 준 접근 키였다 (공식, 날짜만 11-13, Wayback 첫 사본 11-14 03:12 UTC)

10-31·11-02 판에서 다룬 Cursor 마켓플레이스의 변조된 Claude Code 확장 사고와, 11-11 판의 Fireworks AI 2차 침해가 한 사슬로 이어졌다.

- **경로:** 원문: "It started at Fireworks. An attacker obtained an access key we had issued to Fireworks." 공격자는 이 키로 Cursor가 Fireworks와 데이터를 나누던 읽기 전용 버킷을 복사했다. 원문: "That bucket contained limited coding agent traces, mostly from 2025, from sessions not in Privacy Mode, which we were permitted to use for model training."
- **트레이스 속 키:** 원문: "A small number of those traces contained credentials that had not been redacted; less than 0.01% of customers had API credentials exposed." 공격자는 그 키로 Cursor가 쓰는 SaaS 도구와 보관용 비운영 소스 저장소에 들어갔고, 거기서 얻은 자격 증명으로 확장을 바꿔치기했다. 원문: "the attacker placed malicious, unauthorized copies of Anthropic's Claude Code extension directly on the cache used to serve the Cursor VS Code extension marketplace."
- **피해 범위:** 원문: "At most 5,000 users who requested specific cached versions of the extension on October 29 potentially received the malicious build." 노출 시간은 약 11시간이다. Privacy Mode를 쓰지 않은 사용자 중 트레이스가 노출된 비율은 원문: "fewer than 0.05% of users"다. 11-02 판에서 "At most 5,000 users" 문장이 공지에서 지워졌다고 적었는데, 최종 보고에 같은 수치가 다시 들어갔다.
- **조치:** 원문: "Moved all inference traffic off Fireworks". Cursor는 내부 자격 증명을 모두 교체했다. 마지막 공격 활동은 11-01이었고 차단됐다고 했다.

함정: 2.1.312·2.1.313을 Cursor 마켓플레이스에서 받았다면 보안 공지 절차를 따른다. 원문: "Uninstalling the extension alone may not be enough."

중요한 이유: Wiz가 묶은 "Black October" 침해가 서로 독립된 사고가 아니었다. 추론 제공자에게 넘긴 학습용 트레이스에 지워지지 않은 키가 남아 있었고, 그 키가 코딩 에이전트 공급망 공격으로 이어졌다. 에이전트 트레이스를 외부에 학습 데이터로 넘기는 조직은 비밀값 제거를 다시 점검할 일이다.

[Cursor](https://cursor.com/blog/security-incident-update) · [HN](https://news.ycombinator.com/item?id=50496937)

### 2. 2026년 CVE가 10만 건을 넘었다. VulnCheck는 주된 원인을 AI 취약점 탐색으로 본다 (매체보도, The Register 11-13 20:52 UTC)

- **규모:** 원문: "The number of Common Vulnerabilities and Exposures (CVEs) found and disclosed so far in 2026 surpassed 100,000 on Friday - more than double 2025’s total - and the reason can be summed up in two letters: AI." VulnCheck 원문: "There's no denying the fact that vulnerability hunting with AI is driving a majority of what we're seeing."
- **악용 속도:** 원문: "VulnCheck found the median time from CVE publication to exploitation falling from 120 days in 2025 to 80 days during the first half of 2026." 원문: "About 1.4 percent (1,363) of the CVEs published this year have been added to VulnCheck’s Known Exploited Vulnerabilities (KEV) catalog."
- **사례:** 같은 날 공개한 냉장 컨트롤러 취약점에 대해 원문: "literally minutes later, we were seeing hits on our honey pots"라고 했다.

중요한 이유: 이번 주 판들이 다룬 AI 발견 취약점(PostgreSQL 52건, Node.js, Chrome)이 통계로 보인다. 아래 3번의 전역 GHSA 검토 지연도 같은 물량 압박과 맞물려 있을 수 있다. 이 연결은 이 판의 추정이다.

[The Register](https://www.theregister.com/security/2026/11/13/this-years-bug-count-just-passed-100k/5309882)

### 3. 전역 GHSA 검토 게시가 7시간 반 돌다가 또 멈췄다. 이번에도 옛 권고만 처리됐다 (공식 데이터 직접 셈)

- **재개와 재정지:** 직전 판 2번의 정지(11-13 01:09 UTC) 뒤 16:34 UTC에 다시 돌기 시작해, 창 안에 검토 권고 35건을 내고 11-14 00:08 UTC에 멈췄다. 같은 시간에 NVD 미러 8건은 중복으로 처리됐다. 조회 시점까지 약 16시간째 0건이다.
- **처리된 것:** 35건 모두 저장소에서 06-28 ~ 11-09에 공개된 권고의 늦은 미러다. NLTK 8건(저장소 공개 10-26)이 가장 많다. NLTK를 쓰는 저장소에는 이번 창에 Dependabot 알림이 갔을 수 있다.
- **여전히 전역 DB에 없는 것(11-05 이후 저장소 권고, 하나씩 조회):** vLLM 3건, MCP Inspector 6건, MCP 레퍼런스 서버 6건, LiteLLM 1건, GPUStack 11건, VS Code 16건, n8n 5건, MLflow 1건.
- **패턴(추정):** 두 번의 재개가 모두 미국 태평양 시간 업무 시간에 돌았다(09:17 ~ 17:09 PST, 08:34 ~ 16:08 PST). GitHub 상태 페이지·블로그·변경 로그에 공지는 없다.

함정: 위 프로젝트를 쓴다면 Dependabot 알림을 기다리지 말고 저장소 보안 권고 페이지를 직접 본다. 주말이라 월요일 전에 풀릴 가능성은 낮아 보인다.

[전역 검토 권고 목록](https://github.com/advisories?query=type%3Areviewed)

### 4. 큰 장애 두 건, 둘 다 공식적으로 AI 원인설을 부인했다: 미국 FAA 전국 지상 정지와 Microsoft 365 24시간 장애 (공식 + 매체보도, HN 1,600점대)

- **FAA(11-13 20:54 UTC 정지, 11-14 01:19 UTC 해제):** 원인은 NBC 원문: "The FAA said a router sent too much data, overloading older air traffic control equipment." NBC 원문: "More than 7,000 flights were canceled Friday". FAA는 사흘 전 AI 기반 교통 흐름 도구 SMART를 전국으로 넓힌다고 발표했었다(Ars). 공급사 Air Space Intelligence는 먼저 성명을 냈다. 원문: "The current ground stop is not based on or a result of the SMART system, any product of ASI or our integration with FAA and ATC". 교통부 장관은 SMART가 문제 장비와 원문: "completely disconnected"라고 했다. 국토안보부는 원문: "authorities had found no evidence of a cyberattack"라고 했다(NBC 서술).
- **Microsoft 365(Wired 11-13 19:08 UTC):** 직전 판 기술 8번의 장애다. Microsoft 원문: "This was a technical issue not related to cyber or AI/agents. The incident was introduced by an issue during an earlier network capacity upgrade that surfaced following a recent network change." 사후 보고서는 아직 없다. Claude·ChatGPT의 M365 커넥터 복구는 모델 3번에 적었다.

중요한 이유: AI 시스템을 막 들인 곳에서 장애가 나면 AI가 먼저 의심받는다. 두 건 모두 공급사와 운영사가 원인 발표 전에 AI를 배제하는 문장을 따로 넣었다.

[NBC](https://www.nbcnews.com/news/us-news/faa-orders-nationwide-ground-stop-departing-flights-reasons-safety-rcna609247) · [FAA 해제 공지](https://www.fly.faa.gov/adv/adv_otherdis?adv_date=11142026&advn=5) · [Ars(SMART 확대)](https://arstechnica.com/features/2026/11/faa-expands-ai-advice-to-manage-air-traffic-congestion-across-entire-us-airspace/) · [Wired](https://www.wired.com/story/microsoft-365-crawls-back-to-life-after-a-24-hour-outage/) · [HN](https://news.ycombinator.com/item?id=50489641)

### 5. NVD를 거쳐 들어온 AI 제품 권고: IBM ContextForge MCP Gateway 8건, CISA가 "프롬프트 주입"으로 낸 critical, 미수정 Flowise SSRF (공식, 미검토 GHSA 직접 셈)

창 안 미검토 권고 199건 중 AI 관련은 16건이다.

- **IBM ContextForge MCP Gateway 8건(high 2·medium 6, NVD 11-13 22:17 UTC):** 10-21 판에서 다룬 MCP 게이트웨이다. 가장 높은 CVE-2026-110279(8.1) 원문: "the X-Upstream-Authorization request header can override the gateway-managed upstream credential for OAuth (authorization_code and client_credentials), bearer and basic authenticated gateways". 테넌트 사이 문제도 있다. CVE-2026-110178은 다른 테넌트가 소유한 공개 도구의 엔드포인트와 자격 증명을 덮어쓸 수 있다. 수정은 v1.1.1(11-10)이다. IBM 우회책 원문: "Remove the X-Upstream-Authorization request header at the reverse proxy or API gateway in front of IBM ContextForge MCP Gateway for all MCP endpoints".
- **Jet Admin Cloud Platform(critical 9.9, CISA 11-13 16:45 UTC):** CISA가 원인을 "프롬프트 주입"으로 적어 CVE를 낸 권고다. 원문: "A prompt injection vulnerability in all versions of Jet Admin Cloud Platform allows a remote attacker with a free account to expose sensitive .env configuration files and exfiltrate critical credentials." SaaS라 이미 막혔다. 원문: "Mitigated via sandbox component around October 29, 2026."
- **Flowise 3.1.4까지 SSRF(CVE-2026-127214, 11-13 21:32 UTC, 미수정):** 원문: "bypass the Custom Function sandbox's SSRF protection by requiring a module subpath inside an allowlisted package, such as axios/dist/node/axios.cjs". 최신 릴리스가 여전히 3.1.4(07-29)라 11-02 판의 샌드박스 탈출과 함께 고쳐지지 않았다. 사용자 정의 함수를 쓰지 않거나 외부망을 막는다.
- **Pulse 6.8.0(저장소 권고 11-13 21:16 UTC):** AI 비서 승인 대기열 권한 누락이다. 원문: "a signed-in viewer in an organization can list its pending AI Assistant approval requests and approve or deny them without approval permission."

[IBM 공지](https://www.ibm.com/support/pages/node/7296490) · [GHSA-fp2v-98c9-h9m3](https://github.com/advisories/GHSA-fp2v-98c9-h9m3) · [CISA CSAF](https://raw.githubusercontent.com/cisagov/CSAF/develop/csaf_files/IT/white/2026/va-26-317-01.json) · [Flowise GHSA](https://github.com/advisories/GHSA-vqvj-w792-h4m6) · [Pulse GHSA](https://github.com/rcourtman/Pulse/security/advisories/GHSA-9vxw-m79f-gxxr)

### 6. npm이 Zhipu 공식 Zread CLI의 모든 버전을 악성으로 내렸다. 근거는 공개되지 않았다 (공식 데이터, malware GHSA 11-13 23:46 UTC)

- **무엇:** 코드 저장소를 AI로 문서화하는 Zhipu의 `zread_cli`와 플랫폼별 패키지 6개에 악성 권고 7건이 붙었다. 범위는 모두 `>= 0`이다. npm은 23:43 ~ 23:45 UTC에 패키지를 보안용 빈 패키지로 바꿨다.
- **공식 패키지가 맞다:** `ZreadAI/zread_cli` README가 원문: "npm install -g zread_cli"를 안내하고, winget ID는 `ZhipuAI.Zread`다.
- **새 버전 때문이 아니다:** npm의 마지막 실제 버전은 0.2.13(05-27)이고, 그 뒤 게시 흔적이 없다. 권고 문구는 일반 문구 원문: "Any computer that has this package installed or running should be considered fully compromised."뿐이다. Zhipu의 입장이나 보도는 찾지 못했다.
- **같은 창의 다른 AI 관련 판정:** 이름을 선점해 둔 `@claudecode/cli`(공식은 `@anthropic-ai/claude-code`), AI 에이전트 플랫폼 Jelou CLI의 특정 두 판. 이름에 `mcp`가 들어간 악성 판정은 0건이다. 창 안 악성 권고 695건 중 668건은 책 제목을 단 스팸 패키지다.

함정: npm으로 Zread를 설치했다면 공식 해명이 나올 때까지 지우고 설치 기록을 확인한다. Homebrew·winget판은 권고 대상이 아니고, 위험 여부도 확인되지 않았다.

[GHSA-3429-mj25-c5wf](https://github.com/advisories/GHSA-3429-mj25-c5wf) · [ZreadAI/zread_cli](https://github.com/ZreadAI/zread_cli)

### 7. 안전 연구 2편: Anthropic이 평가 방법론 글을 냈고, alphaXiv에는 "모델이 다른 모델의 가중치를 고친다"는 위협 모델이 올라왔다 (공식 + 커뮤니티)

- **Anthropic "Building strong safety evaluations"(날짜만 11-13, Wayback 첫 사본 18:04 UTC):** 10-11 판에서 "모든 내부 평가를 실시간 인터넷에서 끊었다"고 쓴 조치의 현재 상태가 나왔다. 원문: "we have hardened our evaluation environments, temporarily stopped running many evaluations that give models internet access, and implemented real-time monitoring for web search evaluations that require giving models internet access." 웹 검색 평가는 실시간 감시를 붙여 다시 돌리는 것으로 읽힌다. 권고는 원문: "If you only do three things, do these: write down the construct you want to measure before collecting data, validate your grader against human judgment before trusting any number it produces, and report uncertainty so readers can tell signal from noise." 시스템 카드의 안전 수치를 읽을 때 쓸 기준이다.
- **Weight Space Misfolding(alphaXiv, HN 11-13 18:33 UTC 140점대):** 한 모델이 다른 모델의 가중치를 직접 바꿔 무단 행동을 심는 위협을 분류했다. 원문: "it also creates an opportunity for one model to induce unauthorised behaviour in another." 저자들도 한계를 적었다. 원문: "While the cases explored in this paper do not demonstrate self-propagation or actual offensive capabilities". arXiv 번호가 없는 동료 심사 전 글이다.

[Anthropic](https://www.anthropic.com/research/building-strong-safety-evaluations) · [alphaXiv](https://www.alphaxiv.org/abs/2611.weight-space-misfolding) · [HN](https://news.ycombinator.com/item?id=50487762)

### 8. 짧게

- **PostgreSQL CVE-2026-6460 후속(THN 11-13 16:53 UTC):** 직전 판 3번의 익스플로잇 작성자가 매니지드 서비스용 PoC도 만들어 각 업체에 전달했다. 원문: "He also adapted the exploit for managed PostgreSQL services and privately shared proofs of concept with their security teams in August." 원문: "The writeup does not give each service's current patch status." 직접 운영한다면 확장 재빌드도 챙긴다. PostgreSQL YUM 공지 원문: "Extensions built against previous releases will continue to work without being rebuilt, but they may not be protected from the vulnerabilities fixed in this release." KEV 등재와 실제 악용 보고는 없다. [THN](https://thehackernews.com/2026/11/postgresql-patches-52-flaws-including.html) · [YUM 공지](https://yum.postgresql.org/news/extensions-require-minor-release/)
- **CISA KEV 2건 추가(11-13, 기한 11-16):** 직전 판 창 경계 항목의 PHP2Shell 와이퍼 공격에 쓰인 Check Point 권한 상승 CVE-2026-123039와, UnrealIRCd CVE-2026-114713이다. [KEV](https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json)
- **Apify 후속(상태 페이지 11-13 16:36 UTC):** 원문: "API tokens are now stored only as hashes, which limits the impact of any similar incident." 공개 저장소는 원문: "While it’s open, anyone who knows a storage ID can read that storage." 일주일 안에 원문: "publish a post-mortem with the root cause and remediation within a week"라고 했다. 영향 계정 수는 아직 없다. [Apify](https://status.apify.com/incidents/sxd1dj5j6749)
- **GitHub Copilot의 GPT 모델 오류(11-13 19:05 ~ 21:27 UTC):** GPT-6.1 Terra 등이 영향을 받았다. 원문: "The degradation was caused by an issue with an upstream model provider." 같은 시간 OpenAI 상태 페이지에는 사고가 없다. 직전 판 창 끝에 열려 있던 GitHub PR 병합 타임아웃(18:29 UTC 해결), Cloudflare r8.im·Workers API도 모두 닫혔다. [Copilot 장애](https://stspg.io/cppzsh0j19vy) · [PR 병합 장애](https://stspg.io/qzj7qjdcr8jc)
- **Claude가 괴델 노트를 어떻게 읽었나(Guardian, 모델 8번 후속):** 속기를 직접 읽은 것이 아니라 연구자가 옮긴 전사본을 넣었다. 원문: "showing Claude his transcriptions of the workbooks". HN 상위 댓글은 "숨은 정리 수백 개" 주장에 회의적이다.
- **이탈리아 개인정보보호청이 Meta에 8억 500만 유로를 부과했다(11-14, HN 90점대):** Android Facebook 앱의 갤러리 추천이 사유다. AI 학습은 다루지 않았다고 명시했다. 원문: "Il procedimento non ha riguardato l'eventuale uso dei contenuti per addestrare l'intelligenza artificiale, che Meta nega." 2026년에 나온 갤러리 클라우드 처리 기능은 따로 살피겠다고 했다. [Garante](https://www.garanteprivacy.it/home/docweb/-/docweb-display/docweb/10321080)
- **커뮤니티 논쟁:** AIE Code Summit 참석자들의 "No AI Slop" 선언(HN 200점대, 원문: "Outcomes over output", "Our own thinking over a model's first draft"), EFF의 AI 안경 프라이버시 글(원문: "Meta and Google’s smart glasses can record anything the owner is looking at or use artificial intelligence to describe it."), 칼럼 "The Claude Delusion"(HN 110점대). [선언](https://www.slopmanifesto.com/) · [EFF](https://www.eff.org/deeplinks/2026/11/tech-companies-are-trying-shift-entire-concept-privacy-again) · [칼럼](https://www.netmeister.org/blog/claude-delusion.html)
- **직전 판 후속:**
  - Blackwell GPU 지문: 보고서와 코드 저장소 모두 그대로다. 검증했다는 연구자의 신원, 해당 랩들의 입장, 미 상무부 반응, 재현은 찾지 못했다.
  - wfx-loot: 창 안 새 탐침 PR은 0건이다. 열린 탐침 PR은 GitHub 검색 기준(근사치, 조회 시점) 5만 7천여 개다. GitHub 공식 입장과 최초 토큰 탈취 경로는 여전히 없다.
  - 새 진전 없음: OpenAI 정렬 연구(최신 11-12), Notion 권고, AMD SEV-SNP 펌웨어 수정, Linux 커널 AI 패치 부담, 프런티어 AI 개발 중단 소송, Depi(Codex 라벨러), Apache httpd mod_http2, CVE-2026-98432 Ubuntu focal·bionic(여전히 pending), Opera Neon MCP Hub, GPUStack, Grok Build, Cairn·ARTEX·Silent Lynx, `karma365-mcp`·`@rclaude/*` 판정 근거.
  - AI 키워드가 없는 전역 건: Node.js·OpenSSL·커널 보안 릴리스는 창 밖이고 Python 보안 릴리스는 없다.

## 써볼 만한 도구

금요일 업무일과 주말이라 큰 출시는 적다. 가장 중요한 것은 Claude Code 2.1.325다. 샌드박스 안 Bash 명령을 더는 자동으로 허용하지 않아, 무인으로 돌던 자동화가 멈출 수 있다. 도구는 하나도 설치하거나 실행하지 않았다. 아래 함정은 모두 릴리스 본문·문서를 읽고 정리한 것이다.

### 1. (업그레이드 주의) Claude Code 2.1.325: 샌드박스 Bash 자동 허용이 기본에서 꺼졌고, 예약 작업과 `/loop`가 일부 스킬 앞에서 멈춘다 (공식, npm 11-13 20:07 UTC, GitHub 릴리스 20:40 UTC)

- **설명:** Anthropic의 터미널 코딩 에이전트다. npm `latest`·`next`가 2.1.325이고, `stable`은 2.1.311 그대로다. 2.1.324는 18:42 UTC에 npm에만 올라왔고 릴리스 노트가 없다.
- **추천 이유:** 권한·샌드박스 우회 수정이 여럿 들어갔다. 원문: "Fixed a case where a Bash allow rule could run a different program than the one it names", "Fixed a gap in Bash permission checks where some compound commands could run without asking", "Fixed a case where the Bash sandbox did not fully protect a worktree's settings and hooks", "Fixed a security issue in how zstd-compressed responses are decoded". 백그라운드 세션 과금 버그도 고쳤다. 원문: "Fixed background sessions started on your claude.ai login using (and billing) an API key or token from the shell that started the background service". 창 안 저장소 보안 권고는 없다.
- **함정(동작 변경):**
  - **샌드박스:** 원문: "Changed sandboxed Bash commands to follow the normal permission flow by default; set `sandbox.autoAllowBashIfSandboxed: true` to keep running them without prompts". 관리형 설정으로 샌드박스를 켠 조직은 원문: "in your highest-ranking managed source"에 같은 값을 넣는다.
  - **무인 실행:** 원문: "Changed scheduled tasks and `/loop` to ask before running a skill that declares `allowed-tools` or hooks; add `Skill(name)` to `permissions.allow` to run it unattended". "Always allow" 처리도 바뀌어 원문: "so a scheduled task that relied on one can now wait for you"다.
  - **다른 저장소의 git:** 원문: "Changed read-only git commands to ask first in a repository that isn't the project's or one you trusted". 그런 저장소는 `--add-dir`로 지정한다.
  - **프록시:** `ALL_PROXY`만 설정했다면 플러그인 다운로드가 CONNECT 터널을 쓴다. 원문: "a proxy that refuses CONNECT now fails them".
  - **플러그인 마켓플레이스 운영자:** 새 고정(pin) 방식은 옛 클라이언트가 못 읽는다. 원문: "clients before this version can't clone such a pin, so a marketplace pinned this way stops loading there".
- **직전 판 함정의 후속:**
  - 2.1.323의 1M 컨텍스트 가정 문제를 고쳤다. 원문: "Fixed sessions on many third-party gateways and Foundry stopping with a context length error on every turn instead of compacting". `CLAUDE_CODE_MAX_CONTEXT_TOKENS`가 이제 모르는 `claude-` 모델 ID의 창 크기도 정한다.
  - 사라진 `subagentStatusLine`(이슈 #109446)은 아직 돌아오지 않았다. 직원 원문: "I definitely messed up here.", "I should've had a grace period here." 수정 댓글 원문: "we're now expecting this to be fixed in Monday's release".
  - 2.1.325 첫 회귀 보고가 2건 있다(직원 답 없음): 에이전트 뷰에서 서브에이전트의 모델·컨텍스트가 안 보이는 문제(#109843), Linux에서 Bun JIT 세그폴트(#109895).
- **Agent SDK도 같은 변경을 받는다:** TypeScript 0.3.325 원문: "sandboxed Bash commands are no longer auto-approved unless `autoAllowBashIfSandboxed: true` is set". 인프로세스 SDK MCP 도구에 거부 규칙을 걸어도 도구 목록에 남던 문제도 고쳤다. Python 0.2.182(PyPI 21:38 UTC)는 번들 CLI를 2.1.325로 올린 것이 전부라 같은 기본값을 받는다.

권한 우회 수정 때문에 올리는 편이 낫다. 다만 CI, 예약 작업, `/loop`가 샌드박스에 기대 프롬프트 없이 돌았다면 `autoAllowBashIfSandboxed`와 `Skill(name)` 허용을 먼저 넣는다.

[릴리스 노트](https://github.com/anthropics/claude-code/releases/tag/v2.1.325) · [이슈 #109446 직원 댓글](https://github.com/anthropics/claude-code/issues/109446#issuecomment-6860439790) · [Agent SDK TS 0.3.325](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.325) · [Agent SDK Python 커밋](https://github.com/anthropics/claude-agent-sdk-python/commit/c5c362a488df1b125481cee910e8e238e34a5351)

### 2. (후속) Claude Code Code Mode 문서가 열렸다: 환경 변수로 켜고, 분류기는 도구 호출이 아니라 코드 단위로 본다 (공식 문서, 게시 시각 미확인)

- **설명:** 직전 판 도구 1번에서 2.1.323에 들어갔다고 쓴 실험 기능이다. Claude가 도구를 하나씩 부르는 대신 JavaScript를 써서 도구를 호출한다. 직전 판 조회 때는 문서가 404였다.
- **켜는 법:** `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1`. 원문: "Code Mode is experimental and off by default." 프로젝트 설정 파일로는 켤 수 없다. 원문: "so a repository you clone can't turn on Code Mode for you".
- **함정(보안):** 원문: "the classifier reviews each piece of code before it runs, not each tool call. It doesn't see values computed at run time, such as a path taken from search output, so keep rules or hooks for calls you must control"
- **못 쓰는 곳:** 원문: "You can't use Code Mode through a gateway, Amazon Bedrock, Claude Platform on AWS, Google Cloud's Agent Platform, or Microsoft Foundry". 켜고 시작한 세션은 끄면 이어 갈 수 없다. 원문: "With Code Mode off, Claude Code refuses to resume the session."

[Code Mode 문서](https://code.claude.com/docs/en/code-mode)

### 3. FastMCP 4.2.0: MCP 서버가 에이전트 스킬을 직접 나눠 준다 (공식, GitHub 11-13 17:07 UTC)

- **설명:** Python MCP 프레임워크다. 원문: "FastMCP 4.2 serves agent skills from the server." 실험적 MCP Skills 확장의 `skills/list`를 지원한다. 11-05 판에서 다룬 4.2.0a3 알파의 정식판이다.
- **추천 이유:** 팀 스킬을 기기마다 복사하지 않고 서버 한 곳에서 갱신할 수 있다. 원문: "Few hosts support it yet, so FastMCP's client fills in: `Client.read_skill()` checks every file's size and digest, and `to_directory()` installs a skill into an agent's local skills folder."
- **함정:** 원문: "Python 3.11 or newer is required." 원문: "The 4.1 skills API is deprecated and will be removed in 4.3." 원문: "Thirteen community PRs merged during the cycle were reverted before release and are not listed here." 머지된 줄 알았던 기여가 빠졌을 수 있다.

[릴리스](https://github.com/PrefectHQ/fastmcp/releases/tag/v4.2.0) · [문서](https://gofastmcp.com/docs/servers/skills)

### 4. Cloudflare Kitesurf: Workers 위에서 도는 에이전트용 브라우저 엔진을 Apache 2.0으로 공개했다 (공식 블로그 11-13 20:20 UTC)

- **설명:** Chromium을 띄우지 않고 Workers 안에서 웹 페이지를 다루는 실험적 엔진이다. 원문: "It supports the Chrome DevTools Protocol (CDP), so you can connect clients such as Puppeteer."
- **추천 이유:** 브라우저 자동화를 서버리스 환경에서 가볍게 돌려 볼 수 있다.
- **함정:** 블로그 원문: "note that Kitesurf is under heavy development". 저장소 설명 원문: "Kitesurf is an experimental browser engine that runs on Cloudflare Workers." Chromium과 같은 호환성을 기대하면 안 된다.

[블로그](https://blog.cloudflare.com/kitesurf-open-source/) · [GitHub](https://github.com/cloudflare/kitesurf)

### 5. browser-use 1.0.4·1.0.5: 캐시를 깨던 버그를 고쳐 작업당 비용이 절반이 됐다(자사 측정). 1.0.5는 실행마다 판정 요청을 하나 더 보낸다 (공식, 1.0.4 11-13 20:50 UTC, 1.0.5 11-14 08:24 UTC)

- **1.0.4:** 원문: "Each prune rewrote an earlier message and broke the provider's prompt cache, so screenshot-heavy runs paid full input price on almost every step". 자사 측정 원문: "same score (76.1 vs 76.0), model cost per task $0.208 vs $0.434."
- **1.0.5:** 원문: "Every run is judged by your own model: one more low-effort request checks the run against the task". 끄려면 원문: "`judge=False` turns it off".
- **함정:** 1.0.5는 기본으로 실행마다 모델 요청이 하나 는다. 그리고 릴리스 본문이 `pip install 'browser-use==1.0.5'`를 안내하지만 조회 시점 PyPI에는 없다. 배포 워크플로가 대기 상태다.

[1.0.4](https://github.com/browser-use/browser-use/releases/tag/1.0.4) · [1.0.5](https://github.com/browser-use/browser-use/releases/tag/1.0.5) · [배포 워크플로](https://github.com/browser-use/browser-use/actions/runs/43575677630)

### 6. Google Antigravity의 `/teamwork`·`/boost`가 외부에 열렸다. 학술 연구자 1만 명에게 최상위 사용 등급 6개월 무료를 준다 (공식, Google Research 11-13 20:47 UTC)

- **설명:** Google 내부 연구에 쓰던 다중 에이전트 협업 도구를 Antigravity에 넣었다. 원문: "External scientists and developers can use Gemini 4 models with /teamwork and /boost in Antigravity and Deep Review today."
- **추천 이유:** 원문: "We have also opened applications giving 10,000 academic researchers six months of free access to our highest usage tier." 대학·연구소 소속이라면 신청해 볼 만하다.
- **함정:** 아직 일부만 열렸다. 원문: "we will provide more updates in the coming weeks providing all the features to our external users".

[Google Research 블로그](https://research.google/blog/advancing-science-engineering-with-antigravitys-teamwork-feature/)

### 짧게

- **GitHub Copilot CLI 1.0.112(11-13 23:59 UTC):** 기본값이 바뀌었다. 원문: "Use each model's recommended context window by default, while preserving explicit context choices". 정책 수정 원문: "The built-in GitHub MCP server is no longer available when your organization's policy disables MCP servers". 서브에이전트도 이제 `--max-ai-credits` 한도를 지킨다. 같은 날 Copilot 앱이 GHES를 정식 지원했다. 원문: "Cloud sessions aren't available for GHES." 모델 은퇴는 모델 5번에 정리했다. [CLI 릴리스](https://github.com/github/copilot-cli/releases/tag/v1.0.112) · [GHES](https://github.blog/changelog/2026-11-13-github-enterprise-server-support-in-the-github-copilot-app)
- **(일정 변경) Enterprise의 Claude Data 기본 켜짐이 11-19에서 11-20으로 밀렸다:** 현재 원문: "Claude Data turns on by default on November 20, 2026." 11-12 Wayback 사본은 11월 19일이었다. 바뀐 시각은 확인하지 못했다. [Claude](https://claude.com/resources/articles/sheets-whiteboards-data)
- **`anthropics/skills`의 docx·pptx·xlsx·pdf 스킬 갱신(PR 머지 11-14 15:16 UTC):** pptx에 `slide_text.py`·`apply_theme.js`가 들어갔고, xlsx는 원문: "`recalc.py` now writes back only computed values"다. [PR #2217](https://github.com/anthropics/skills/pull/2217)
- **Vercel `ai@7.0.173`(11-13 22:34 UTC):** 도구 승인 재실행 때 인자가 바뀌던 문제를 고쳤다. 원문: "Prevent in-place schema validation or input refinement from changing approved tool arguments during approval replay." 직전 판의 DNS 리바인딩 기본값 건은 후속이 없다. [릴리스](https://github.com/vercel/ai/releases/tag/ai%407.0.173)
- **LiteLLM 1.108.8 LTS(11-13 20:21 UTC):** 원문: "Fixed sign-in loop ending in `Authentication incomplete` on servers that answer `mcp_all` with detailed scopes, broken since v1.107.0". [릴리스](https://github.com/BerriAI/litellm/releases/tag/v1.108.8)
- **langchain-openai 1.8.0(11-13 16:48 UTC):** 원문: "drops support for Python 3.10". 결정 모델용 `OpenAIClassifier`가 들어왔다. [릴리스](https://github.com/langchain-ai/langchain/releases/tag/langchain-openai%3D%3D1.8.0)
- **Vercel Token Pass(11-13 18:42 UTC):** 원문: "now includes $150 of tokens per month (up from $80) for the same $30 per team per month." 한도에 닿으면 원문: "requests using Token Pass-scoped keys stop with no overage charges". Pro 팀 얼리 액세스다. [변경 로그](https://vercel.com/changelog/vercel-token-pass-now-includes-one-hundred-fifty-dollars-of-monthly-tokens)
- **Baseten DeepSeek Ultrafast(HN 11-13 17:39 UTC 60점대):** README 원문: "An ultra-fast inference engine for **DeepSeek-V4.1-Flash** for `sm_100a` and `sm_103a`." Blackwell 전용이고, 라이선스는 Elastic License 2.0이라 오픈소스가 아니다. [GitHub](https://github.com/basetenlabs/deepseek-ultrafast) · [HN](https://news.ycombinator.com/item?id=50486914)
- **NVIDIA: GLM-5.3을 DGX Station 2대에서 vLLM으로 서빙(11-13 17:00 UTC):** 합계 처리량보다 사용자당 속도를 본다. 원문: "reduces mean per-user throughput, from 115.3 tok/s at C1 to 13.0 tok/s at C32." [NVIDIA 블로그](https://developer.nvidia.com/blog/when-one-gpu-isnt-enough-serving-glm-5-3-across-two-nvidia-dgx-stations-with-vllm/)
- **Show HN: Agweb(11-14 14:08 UTC, MIT):** README 원문: "A browser for coding agents on macOS, built on WebKit (Safari's engine)." 원문: "Needs macOS 26 or later." [GitHub](https://github.com/paul-nameless/agweb) · [HN](https://news.ycombinator.com/item?id=50497147)
- **Gnomon(HN 11-13):** README 원문: "Give an agent its own branch of your real database, as nothing more than a connection string." PostgreSQL 데이터를 에이전트용으로 분기한다. [GitHub](https://github.com/bitsinfra/gnomon) · [HN](https://news.ycombinator.com/item?id=50486480)
- **goose 1.59.1(11-14 06:50 UTC):** Claude Fable 5.2에 적응형 사고를 쓰도록 고쳤다. 본문 원문: "macOS desktop builds and full release notes coming soon." [릴리스](https://github.com/block/goose/releases/tag/v1.59.1)
- **회귀 보고(직전 판 도구 후속):**
  - OpenCode 2.3.0 뒤 릴리스가 없다. 백그라운드 서브에이전트가 숨은 턴마다 부모를 깨우는 문제(#63894), DB 마이그레이션 실패(#63913), 프로젝트 권한이 관리형 규칙을 덮어쓰는 문제(#63879)가 열려 있다. 관리형 설정으로 권한을 묶는 조직은 #63879를 본다. [#63879](https://github.com/anomalyco/opencode/issues/63879)
  - llama.cpp는 v1.0.x가 없다. V100 prefill 저하(원문: "dropped by approximately 80%", #32245)와 Metal FP8 KV 캐시 로드 중단(#32270)이 보고됐다. ggml도 v1.0.0을 붙였다(HN 350점대). [#32245](https://github.com/ggml-org/llama.cpp/issues/32245) · [ggml v1.0.0](https://github.com/ggml-org/ggml/releases/tag/v1.0.0)
- **직전 판 후속 중 새 진전 없음:** Self-hosted Managed Agents, Claude Meetings, `claude-plugins-official`(외부 PR 자동 닫기만 계속), Codex CLI(`latest` 0.169.0), Qwen Code, Gemini CLI 0.68.0, Crush, Pi, Mistral Vibe, Amp, Zed, VS Code 1.146.0, Cursor, Devin Desktop, Jules, Ollama·vLLM·LM Studio, transformers, n8n, Google GenAI SDK, OpenAI Agents SDK JS, Supabase MCP, Open WebUI, Dify, DSPy, Context7, Chrome DevTools MCP, Playwright MCP, GitHub·MongoDB·Sentry·Neon MCP(릴리스 없음), MCP 레퍼런스 서버·공식 SDK·Inspector, MCP 명세(다음 `2027-03-30`), `openai/plugins`, A2UI, Expo Simulators, Brave x402, Langfuse AI Gateway, AgentRun, Val Town(12-10).

### 참고: 창 경계 항목

- **(직전 판 정정) Conductor는 문을 닫지 않는다(발표 X 11-12 20:12 UTC, 0.94.2 날짜만 11-13):** 직전 판은 "실제 읽기 전용 전환 공지 없음"이라고만 적었다. 실제로는 직전 판 창 안에 종료 철회 발표가 있었다. 원문: "We’ve decided to continue development of Conductor!", "Regularly scheduled updates will resume next week!" 0.94.2 변경 로그 원문: "Conductor isn't shutting down.", "Removed the Conductor Cloud shutdown banner from cloud workspaces." 0.93.0 항목의 종료 일정 원문: "Conductor Cloud goes read-only on November 13 and shuts down on November 20."에는 취소선이 그어졌다. Meta 합류 자체는 그대로다. [X](https://x.com/charlieholtz/status/2120973052024598620) · [변경 로그](https://www.conductor.build/changelog) · [HN](https://news.ycombinator.com/item?id=50473888)

## 주목할 점

- **에이전트 데이터가 공급망 공격의 입구가 됐다.** Cursor 사고는 학습용으로 넘긴 코딩 에이전트 트레이스에서 시작됐다. 그 안에 남은 키 하나가 확장 바꿔치기로 이어졌다. 취약점은 AI 덕에 한 해 10만 건을 넘었고, 악용까지 걸리는 시간은 80일로 줄었다(VulnCheck). 그런데 Dependabot 알림의 원천인 전역 GHSA 검토는 업무 시간에만 잠깐 돈다. 이번 주말에도 트레이스·로그를 외부에 넘기는 경로의 비밀값 제거와, 쓰는 AI 도구의 저장소 보안 권고를 직접 보는 편이 안전하다.
- **다가오는 일정:**
  - 이번 주말~다음 주: Fable 5.2 출시 프로모션 종료와 Anthropic 텍스트 워터마크, CISA KEV 2건(Check Point·UnrealIRCd) 조치 기한(11-16). Claude Code `subagentStatusLine` 복구 예고("Monday's release"). NVIDIA 실적 발표와 Anthropic 호주 개인 청약 마감(11-17). Anthropic 공모가 확정과 유럽 청약 마감(11-18), 거래 시작(11-19). Enterprise의 Claude Data 기본 켜짐(11-19에서 11-20으로 변경).
  - 그다음 주: ERNIE 5.5 Flash Preview 가중치 공개 약속(11-25 전), Qwen4-Max-Preview 종료(11-25 16:00 UTC), Gemini 4 Carbon 도입가 종료(11-30).
  - 12월: Dependabot 비활성 저장소 알림 중지(12-08), 영국 Copilot 테넌트 Claude 기본 켜짐(12-09, 보도 기준), Gemini 스마트 안경 판매와 Val Town 읽기 전용(12-10), ChatGPT 옛 음성 모델 은퇴(12-11), GitHub Copilot 모델 6종 은퇴(12-14), Inception Mercury API 종료(12-28).
  - 그 뒤: Vertex의 Hydrogen 캐시 쓰기 과금(2027-01-12), Claude Meetings 통화 중 문서 무료 종료(2027-02-10), 다음 MCP 명세(2027-03-30).

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - Reuters(Mythos 소송, Amazon 문서, Garante 환산액)는 CAPTCHA라 전재 기사·The Verge 인용·원 기관 발표로 대신했다. FT는 403이라 RSS 설명문만 봤다. TechPowerUp(NVIDIA "희소성 조작" 의혹, HN 880점대)은 봇 차단이라 다루지 않았다.
  - Guardian, Business Insider, THN, Cloudflare 블로그, Conductor 변경 로그는 r.jina.ai로 읽었다.
  - Microsoft 365 상태 페이지와 사후 보고서는 보지 못했다. VulnCheck 원 권고는 보지 못하고 The Register 인용만 썼다.
  - x.com은 개별 게시물만 fxtwitter로 읽었다. 타임라인을 볼 수 없어 Anthropic·Zhipu의 해명, Blackwell 지문 관련 랩 반응을 놓쳤을 수 있다. reddit은 막혀 있다.
- **시각이 날짜만 있거나 불확실한 원문:**
  - Cursor 최종 보고(날짜 11-13, JSON-LD 12:00 UTC는 자리표시로 보여 Wayback 첫 사본 11-14 03:12 UTC로 보완), Anthropic 안전 평가 글(Wayback 18:04 UTC로 보완), Thinking Machines 4비트 글(HN 22:06 UTC로 보완), Google Research `/teamwork` 글, Conductor 0.94.2(11-13), Perplexity 블로그(11-12), Garante 보도자료(11-14), IBM 공지(11-12).
  - Claude Code Code Mode 문서가 열린 시각과 Claude Data 날짜가 바뀐 시각은 Wayback 사본이 없어 확인하지 못했다.
- **확인하지 못한 것:**
  - USPTO가 "Claude Mythos" 상표 등록을 거절했다는 2차 보도.
  - Claude와 ChatGPT 결제 오류의 공통 원인, Copilot GPT 장애의 "upstream provider".
  - Zread CLI·Jelou CLI의 악성 판정 근거, 전역 GHSA 검토 정지의 원인, AMD SEV-SNP 펌웨어 수정 여부.
  - Ling 3.2 Flash의 파라미터 수·벤치마크·무료 엔드포인트 데이터 정책.
  - 재현하지 않은 것: 모든 벤치마크·속도·비용 주장(browser-use, Thinking Machines, DeepSeek Ultrafast, NVIDIA, Runway), 취약점과 도구 동작 주장. 도구는 하나도 설치하거나 실행하지 않았다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 스토리 약 850개 중 10점 이상은 전부 봤고, Show HN 106건은 도구 영역에서 전부 봤다.
  - 전역 GHSA: reviewed 35건은 저장소 공개일까지 하나씩 대조했다. unreviewed 199건은 AI 키워드로, malware 695건은 이름 패턴으로 걸렀다.
  - arXiv는 이번 창에 공지가 없었다(일~목 공지). HF Daily Papers 11-14 목록은 비어 있었다.
  - Hugging Face는 주요 조직 약 90곳의 `lastModified`, 공개 예정 저장소 직접 조회, 트렌딩으로 확인했다. 비공개로 만들었다가 연 저장소는 OpenRouter `hugging_face_id` 같은 간접 신호로만 잡힌다.
  - Gemini 앱 릴리스 노트는 10-13 이후 항목이 없어 캐시가 묵은 것으로 보인다. Vertex AI 릴리스 노트(캐시 동결), Volcengine·Moonshot 월 단위 변경 로그는 일 단위로 판정할 수 없었다. Microsoft 블로그·뉴스 피드는 빈 응답이었다.
  - 창이 끝날 때 열려 있던 것: Claude iOS Sign in with Apple 장애, OpenAI Costs API 지연(monitoring), browser-use 1.0.5 PyPI 배포 대기.
