---
title: "2026-11-21 AI 브리핑"
date: 2026-11-21T08:00:00+09:00
tags: [ai-briefing, anthropic, openai, security, math]
description: "Anthropic 수학자가 Claude와 함께 일반화 리만 가설을 증명했다고 주장했고(Lean만, 미검증), Anthropic은 공모를 마무리했지만 주가가 공모가 근처까지 내려왔으며, Amazon은 공격자의 AI 에이전트가 0-day를 스스로 찾아 악용했다고 밝혔다."
---

> 조사 범위: 2026-11-20 08:00 ~ 2026-11-21 08:00 KST(2026-11-19 23:00 ~ 11-20 23:00 UTC, 미국 목요일 15:00 ~ 금요일 15:00 PST). 직전 브리핑(11-20 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, SEC EDGAR `acceptanceDateTime`, GitHub `published_at`·`merged_at`·커밋 시각, npm·PyPI 게시 시각, 저장소·전역 GHSA `published_at`, CISA KEV `dateReleased`, 배포판 보안 공지, 상태 페이지 API, Hugging Face API, Mastodon API, Wayback 캡처, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-20 23시대 UTC) 값이다.

## 오늘의 핵심 요약

- **AI 수학 증명 주장이 나왔다.** Anthropic 소속 수학자 Levent Alpöge가 "Claude와 함께" 일반화 리만 가설(GRH)을 증명했다고 했다. 근거는 1,740만 줄짜리 Lean 코드뿐이고, 아직 독립 검증이 없다. 같은 날 Euler·Navier–Stokes 방정식의 유한 시간 폭발 증명도 나왔다. 둘 다 AI의 도움을 크게 받았다. 사람만으로 쓴 GRH 원고도 따로 공개됐다. Anthropic은 공식 입장을 내지 않았다.
- **Anthropic이 공모를 마무리했다.** 추가 매입 옵션이 전량 행사돼 공모 총액은 약 $93.5B다. 그러나 ANT는 첫 정규 거래일에 8.5% 내려 $88.91로 마감했다. 같은 날 NVIDIA는 14% 가까이 떨어졌다. OpenAI는 증류 방어 글에서 GPT-6.2 추론 추출 성공률을 54.0%에서 0.2%로 낮췄다고 밝혔다. 오픈 웨이트로는 Inkling 2 Mini(약 20GB)와 Tencent Hy4 본체(770B)가 나왔다.
- **공격자의 AI 에이전트가 0-day를 스스로 찾아 악용했다.** Amazon이 공식 확인했다. 표적 MONAI Label은 PyPI 최신판도 아직 취약하다. KEV에는 4건이 추가됐고 기한은 11-23이다. 도구 쪽에서는 Claude Code 2.1.330이 `deniedMcpServers`의 의미를 바꿨다. Codex CLI·Pi는 기본 모델을 GPT-6.2 Sol로 바꿨다. Team·Enterprise용 Claude computer use 베타도 나왔다.

## 모델 소식

미국 금요일 창이었지만 공식 발표가 꽤 나왔다. Anthropic은 공모를 마무리했다. ANT는 첫 정규 거래일에 8.5% 내려 공모가 근처까지 왔다. 오픈 웨이트는 두 건이다. Thinking Machines가 노트북에서 도는 Inkling 2 Mini를 냈고, 직전 판까지 401이던 Tencent Hy4 본체가 공개됐다. OpenAI는 증류 방어 글을 내면서 GPT-6.2가 대화 앞부분을 고치면 추론을 버리는 이유를 처음 설명했다. Claude를 쓴 리만 가설 증명 주장은 기술 이슈 1번에 정리했다. 공식 변경 로그는 Claude 앱(11-20 computer use, 도구 3번)과 ChatGPT(11-20 Space, 도구 짧게)가 움직였다. 그대로인 곳은 Claude Platform(11-17), OpenAI API(11-19), Gemini API(11-19), Gemini 앱(11-17), DeepSeek(11-13), Z.ai다. Claude·OpenAI 상태 페이지에는 창 안 새 인시던트가 없다. OpenRouter 신규 등록은 옛 모델 1건(Pixtral Large 2502)뿐이다.

### 1. Anthropic 공모 종결: 추가 매입 옵션 147,000,000주가 전량 행사돼 공모 총액이 약 $93.5B가 됐다. ANT는 첫 정규 거래일에 -8.5% (공식 회사 발표 + 매체보도)

- **종결(회사 발표, 11-20):** 원문: "including the full exercise by the underwriters of their option to purchase an additional 147,000,000 shares of Class A common stock from Anthropic at the initial public offering price." 회사 몫은 1,103,351,354주, 기존 주주 몫은 23,648,646주다. 총액 원문: "The aggregate gross proceeds from the offering, before deducting underwriting discounts and commissions and other offering expenses, were approximately $93.5 billion, of which approximately $91.6 billion was received by Anthropic." 사모까지 합친 원문: "Aggregate gross proceeds from the initial public offering, including the shares sold by the selling stockholders, and the concurrent private placement were approximately $119.5 billion". 직전 판에서 본 424B4는 옵션 전량 행사 시 순조달을 약 $117.0B로 적었다. 조회 시점까지 SEC 창 안 제출은 없다(종결 8-K 미제출).
- **둘째 거래일(11-20):** CNBC 원문: "Anthropic shares fell 8.5% on Friday to close their first full day of trading". 시가 $89.86, 저가 $83.05, 종가 $88.91이다(Yahoo Finance 시세). 저가는 공모가 $83.00보다 5센트 높았다. NBC 원문: "putting shares just a few dollars above the IPO price". Guardian 원문: "On Friday, the Nasdaq Composite erased all its gains for 2026".
- **같은 날 NVIDIA:** Yahoo Finance(19:42 UTC, Reuters 인용) 원문: "fell nearly 14% Friday, its worst day since the DeepSeek rout in January 2025". SEC가 NVIDIA 회계를 조사한다는 보도에 대해 원문: "the SEC declined to comment."
- **창업자 매도(TechCrunch 00:43 UTC):** 원문: "$1.95 billion for its seven founders".
- **왜 중요한가:** 공모 구조는 최종 확정됐다. 하지만 상장 이틀 만에 주가가 공모가 근처까지 내려왔다. 상장사 Anthropic의 첫 시험대는 AI 매도세였다.

[회사 발표](https://www.anthropic.com/news/ipo-close) · [CNBC](https://www.cnbc.com/2026/11/20/anthropic-stock-ai-amd-nvidia.html) · [NBC](https://www.nbcnews.com/business/markets/anthropic-shares-markets-selloff-nasdaq-rcna610394) · [Guardian](https://www.theguardian.com/business/2026/nov/20/global-stock-markets-slump-anthropic-ipo-ai-sell-off) · [Yahoo Finance NVIDIA](https://finance.yahoo.com/markets/article/nvidia-stock-extends-25-trillion-rout-as-sec-probe-reportedly-investigates-its-accounting-194945274.html) · [TechCrunch](https://techcrunch.com/2026/11/19/anthropics-7-founders-cashed-out-nearly-2b-in-record-breaking-ipo/) · [HN NBC](https://news.ycombinator.com/item?id=50578794) · [HN Guardian](https://news.ycombinator.com/item?id=50574383)

### 2. OpenAI "Combatting adversarial distillation": GPT-6.2 추론 추출 공격 성공률이 54.0%에서 0.2%로 내려갔다. 대화 앞부분을 고치면 추론이 버려진다 (공식, RSS 11-20, HN 첫 게시 20:11 UTC)

- **성과(원문):** "The average attack success rate fell from 54.0% against GPT‑6 Astra with its original safeguards to 0.2% against GPT‑6.2 Astra with its updated safeguards." 과거 취약 경로도 막았다. 원문: "including closing a pathway that could expose another user’s encrypted reasoning."
- **API 동작(원문):** "With GPT‑6.2 Astra and GPT‑6.2 Sol, we tie a model’s protected reasoning to the conversation history that produced it." 원문: "If an earlier message is rewritten, our API drops the reasoning that followed it." 원문: "We also prevent older model families with weaker safeguards from reading GPT‑6.2 Astra and Sol’s protected reasoning." GPT-6에도 적용한다. 원문: "We’re now applying selected safeguard improvements to GPT‑6 Astra as well."
- **단속 규모(원문):** "Our weekly enforcement actions against these campaigns have ranged in scale from thousands to tens of thousands of accounts, with the rate increasing significantly this past summer." DeepSeek(2월 의회 브리핑)와 Moonshot(9월 보고서)을 다시 거명했다. 원문: "We plan to share more soon about recent campaigns we’ve disrupted." 입법 지지 원문: "We’ve also told Congress we support the Chip Security Act, the AI OVERWATCH Act, and the MATCH Act."
- **함정:** 요약 압축이나 도구 결과 축약처럼 대화 이력을 고쳐 쓰는 하네스는 GPT-6.2에서 앞선 추론을 잃는다. 토큰·지연이 늘고 품질이 떨어질 수 있다. 여러 모델을 섞어 쓰는 파이프라인에서는 옛 모델이 GPT-6.2의 추론을 읽지 못한다. 직전 판까지 다룬 `instructions` 캐시 문제(도구 5번)와 같은 뿌리지만, 글에 캐시 비용 이야기는 없다.

[OpenAI](https://openai.com/index/combatting-adversarial-distillation) · [The Decoder](https://the-decoder.com/openai-makes-it-much-harder-for-attackers-to-extract-how-its-latest-ai-models-think/) · [HN](https://news.ycombinator.com/item?id=50581242)

### 3. Thinking Machines Inkling 2 Mini: 26.4B MoE(활성 3.4B)를 반복 실행하는 루프형 모델이다. 약 20GB로 노트북에서 돈다 (공식, HN 20:12 UTC 500점대)

- **무엇(원문):** "Today we are releasing Inkling 2 Mini, a fast, powerful model that’s small enough to run on your laptop." 원문: "Inkling 2 Mini is a Mixture-of-Experts model with 26.4B total parameters, 3.4B active, and 5.4B weight activations per token to account for looping." 원문: "The total memory footprint of Inkling 2 Mini is ~20GB". HF 라이선스 태그는 `apache-2.0`이다.
- **성능(자사 주장):** 원문: "Inkling 2 Mini leads every open-weights model with less than 100B total parameters on benchmarks such as DeepSWE v1.1 (48.2%) and Vals Index v2.2 (46.5%)." 약점도 적었다. 원문: "it scores 50.0% on MultiChallenge and 33.4% on ComplexConstraints v1.1, behind Qwen3.8-27B on both." 추론 감시 가능성 원문: "at this scale we do not see a meaningful reduction in the monitorability of their reasoning."
- **외부 평가:** Vals Index에는 46.46%로 올라 있다. Artificial Analysis 원문: "Inkling 2 Mini scores 20 on the Artificial Analysis Intelligence Index, placing it well above average among comparable models (median: 10)".
- **가격(Tinker, 100만 토큰당):** prefill $0.15, 캐시 $0.015, sample $0.50, train $0.75.
- **함정:** 발표는 vLLM·Ollama·SGLang에서 바로 된다고 했지만, 창 종료 시점에는 대부분 PR 단계였다(도구 9번). 같은 날 Inkling 2 Small 저장소의 토큰·생성 설정도 고쳐졌다. 예전 설정을 캐시해 둔 사용자는 다시 받는다.

[Thinking Machines](https://thinkingmachines.ai/news/inkling-2-mini/) · [HF](https://huggingface.co/thinkingmachines/Inkling-2-Mini) · [Tinker 가격](https://thinkingmachines.ai/tinker/pricing) · [Artificial Analysis](https://artificialanalysis.ai/models/inkling-2-mini) · [HN](https://news.ycombinator.com/item?id=50581260)

### 4. Tencent Hy4 본체(770B, 활성 49B)의 가중치가 공개됐다. 라이선스는 Apache가 아니다 (공식 HF, 03:05 UTC + 매체보도)

- **무엇:** 직전 판까지 401이던 `tencent/Hy4`, `Hy4-FP8`, `Hy4-base`가 공개됐다. HF `lastModified`는 11-20 03:05 UTC다. README 원문: "The language backbone comprises 770B total parameters, of which 49B are activated per token." 한계도 적었다. 원문: "Although Hy4 is one of the strongest open-weight models available today, a notable gap remains between it and top proprietary models, particularly on challenging internal benchmarks for research-level mathematics, long-horizon coding, model self-improvement, and advanced visual question answering."
- **함정(라이선스):** TechNode(14:14 UTC)는 원문 "makes the flagship available under the Apache 2.0 license"라고 썼지만 틀렸다. 실제 LICENSE 원문: "This License is not the Apache License, Version 2.0". Hy4 flash와 같은 Tencent Hy License로, 대형 서비스에 쓰려면 별도 라이선스가 필요하다. 상용 서비스에 넣기 전에 LICENSE 원문을 본다.
- **함정(서빙):** README는 전용 이미지 `vllm/vllm-openai:hy4`와 `lmsysorg/sglang:hy4`를 안내한다. transformers 정식판은 아직 지원하지 않고, Hy4 flash용 PR #50903도 열려 있다.
- **왜 중요한가:** BIS 규칙 이후 "다음 세대가 늦어진다"고 했던 Tencent가 현 플래그십 가중치를 풀었다(직전 판 모델 3번).

[HF Hy4](https://huggingface.co/tencent/Hy4) · [LICENSE](https://huggingface.co/tencent/Hy4/blob/main/LICENSE) · [TechNode](https://technode.com/2026/11/20/tencent-releases-hy4-model-weights-with-million-token-context/)

### 5. 미·중 모델 정치: Cursor가 중국 모델 단계적 퇴출을 검토하고, 미국은 중국 군 연관 기업에 칩 판매를 허가했으며, NVIDIA는 Inception 인수를 협상 중이다 (매체보도)

- **Cursor(CNBC 14:04 UTC, 하원 보고서 인용):** 원문: "Cursor is considering phasing out Chinese-developed open-weight AI models because of geopolitical risk, according to a House report reviewed by CNBC." 원문: "Cursor’s Composer 2 was built on Moonshot AI’s Kimi K2.5". 미국 코딩 도구의 중국 오픈 웨이트 의존이 정책 압박을 받기 시작했다.
- **칩 수출 허가(The Register 04:04 UTC):** Mast 의원 공개 원문: "The congressman on Thursday revealed requests to sell 3.4 million AI chips to China, and that BIS approved the sale of 879,000 items." 허가 대상에 Alibaba·Tencent가 있다. Bessent 재무장관(CNBC 15:52 UTC) 원문: "H200s are the threshold level of chips that could be sold to the Chinese".
- **Trump(The Verge 18:50 UTC):** 데이터센터를 해외에 짓는 기업에 대해 원문: "we’re going to put a tremendous tax on them."
- **NVIDIA–Inception(The Decoder 14:53 UTC, The Information 인용):** 원문: "Nvidia is nearing a deal to acquire AI startup Inception for at least $500 million." 원문: "Neither company initially commented." 확산 모델로 Nemotron을 보강하려는 것이라고 했다. Inception Mercury API 종료(12-28) 문서는 바뀌지 않았다.

[CNBC Cursor](https://www.cnbc.com/2026/11/20/cursor-chinese-ai-models-geopolitical-risk.html) · [The Register](https://www.theregister.com/legal/2026/11/20/us-let-nvidia-and-amd-sell-bucketloads-of-chips-to-military-linked-chinese-companies/5311200) · [CNBC Bessent](https://www.cnbc.com/2026/11/20/treasury-secretary-scott-bessent.html) · [The Verge](https://www.theverge.com/policy/1038892/trump-foreign-data-centers-tax) · [The Decoder](https://the-decoder.com/nvidia-bets-at-least-500-million-on-inceptions-diffusion-models-to-strengthen-its-open-source-nemotron-lineup/)

### 6. 짧게

- **OpenAI MedChemBench(RSS 11-20, 날짜만):** 99문항 자체 벤치마크다. 원문: "At the xhigh reasoning setting, GPT‑6.2 Astra earned 84.5% of the available credit, compared with 83.3% for GPT‑6 Astra, 82.0% for GPT‑6.1 Sol, and 58.2% for GPT‑5.6 Sol." 비교 대상이 자사 모델뿐이다. [OpenAI](https://openai.com/index/introducing-medchembench)
- **Apple이 서드파티 에이전트의 Mac 제어를 어렵게 했다(The Decoder 15:03 UTC, Bloomberg 인용):** 원문: "macOS 27.3 now requires 14 steps instead of four for users to let third-party agents like ChatGPT control their Macs." 자체 에이전트 채용 건은 직전 판에서 다뤘다. [The Decoder](https://the-decoder.com/apple-joins-the-race-for-ai-agents-that-control-your-device-while-raising-new-barriers-for-competitors/)
- **Tencent WeMM-Video(HF 11-20 03:57~05:08 UTC):** 원문: "WeMM-Video is a Chinese–English bilingual family for joint audio-visual understanding and reasoning." 35B-A3B·9B·4B가 있고 `trust_remote_code=True`가 필요하다. [HF](https://huggingface.co/tencent/WeMM-Video-35BA3B)
- **Apple Depth Pro 2(HF 11:28 UTC):** 단안 메트릭 깊이 추정 모델이다. [HF](https://huggingface.co/apple/DepthPro2)
- **Alibaba가 차세대 Qwen을 자체 T-Head 칩으로 학습한다(TechNode 14:14 UTC, 발언은 11-19 실적 콜):** 원문: "Alibaba is using chips developed by its semiconductor subsidiary T-Head to train its next Qwen model". 전 Qwen 책임자 Lin Junyang의 Pragmatik Labs는 $5B 가치로 투자를 받았다(같은 매체, Bloomberg 인용). [TechNode Alibaba](https://technode.com/2026/11/20/alibaba-uses-its-own-chips-to-train-next-generation-qwen-model/) · [TechNode Pragmatik](https://technode.com/2026/11/20/ex-qwen-chiefs-pragmatik-labs-raises-funding-at-5-billion-valuation/)
- **(커뮤니티) Models Engineering Index에서 Gemini 4 Neon 1위(HN 11-19 23:35 UTC, 360점대):** 개인 GitHub Pages 지수다. 방법론은 검증하지 않았다. [MEI](https://git-aarvin.github.io/mei/) · [HN](https://news.ycombinator.com/item?id=50568130)
- **정정(직전 판 창 경계 항목):** 직전 판은 GPT-6.2 Astra의 탈옥 회귀를 OpenAI가 고쳤다는 내용을 "시스템 카드에서 확인하지 못했다"고 썼다. 실제로는 11-17 19:15 UTC 첫 Wayback 사본부터 시스템 카드에 있었다. 원문: "OpenAI believes this regression to be mitigated, though UK AISI has not validated this at the time of writing." [시스템 카드](https://deploymentsafety.openai.com/gpt-6-2-astra-sol)
- **직전 판 후속:**
  - 여전히 401: Qwen4-Max, Qwen4-Max-Preview(11-25 종료), Ling 3.2 Flash, ERNIE 5.5 Flash Preview(11-25 전 공개 약속), LG K-EXAONE 2.5.
  - Gemini 4 Neon 안전 보고서는 찾지 못했다. 모델 카드도 그대로다. GPT-6.2 시스템 카드는 창 안 변경이 없다.
  - FLUX.1-schnell은 여전히 gated이고 BFL 설명이 없다. Hy4 flash transformers PR #50903도 열려 있다. vLLM에는 Inkling 2 Small 정식 릴리스가 아직 없다(0.34.0).
  - 애리조나 법무장관실 보도자료는 나오지 않았다. 캘리포니아 대 OpenAI 소송, Palo Alto–Applied Compute, Mistral 시리즈 D, Meta Muse Charm도 새 진전이 없다.
  - 소식 없음: DeepSeek 사용 제한·V5, Anthropic 30일 공개 정책, 'Mythos' 상표 소송, Edge0, Agnes, LongCat-2.5, ChatGPT Wallet, 한국 랩 모델 발표.
  - Enterprise Claude Data 기본 켜짐(11-20)은 안내 문구 그대로이고, 실제 전환은 확인하지 못했다. Gemini 4 Carbon·Argon 도입가(11-30), 영국 Copilot Claude 기본값(12-09), Inception Mercury 종료(12-28)도 그대로다.

### 참고: 창 경계 항목

- **(직전 창, 11-19 22:49 UTC) 캘리포니아 법무장관이 NVIDIA에 소환장을 보냈다(CNBC):** 직전 판에서는 제목만 "창 직후"로 적었는데, 실제 게시는 창 시작 11분 전이었다. Bonta 원문: "My office is investigating whether Nvidia or Jensen Huang's actions resulted in China obtaining advanced chips or confidential information". 원문: "Nvidia did not immediately respond to a request for comment on Bonta's subpoena." [CNBC](https://www.cnbc.com/2026/11/19/nvidia-california-bonta-subpoena-china-chips.html)

## 기술 이슈

AI로 만들었다는 수학 증명 주장이 창을 덮었다. HN 최고 2,250점대이고, 관련 스토리 다섯 개가 상위권에 올랐다. 보안 쪽에서 가장 무거운 건 Amazon 보고서다. 공격자의 AI 에이전트가 사람 지시 없이 0-day를 찾아 악용했는데, 그 결함은 PyPI 최신판에서도 고쳐지지 않았다. CISA KEV에는 4건이 추가됐고 조치 기한은 모두 11-23이다. Ruby·PHP 후속은 배포판 패키지 쪽에서 함정이 나왔다. 전역 GHSA 창 안 게시분은 reviewed 71건(모두 늦은 미러), unreviewed 659건, malware 132건이다(직접 셈).

### 1. AI로 만든 수학 증명 주장 세 건: 일반화 리만 가설(Lean만), 3D Euler 폭발, Navier–Stokes 폭발. 아직 독립 검증은 없다 (발견자 본인 문서·전문가 블로그 + 매체보도, HN 14:16 UTC 2,250점대)

- **GRH:** Anthropic 소속 수학자 Levent Alpöge가 4쪽짜리 소개 문서를 올렸다. 원문: "I believe I have, working with Claude, discovered a proof". 원문: "In fact, the method easily extends much further, resolving the so-called Generalized Riemann Hypothesis". 원문: "Claude and I have also formalized the arguments without assumptions." 원문: "I decided to only present Lean." 사람이 읽을 원고는 없고 Lean 코드만 있다. Aaronson(18:29 UTC)은 규모를 원문 "producing 17.4 million (!!) lines of Lean code"라고 적었다.
- **Euler·Navier–Stokes:** Alpöge·Buckmaster(Euler는 Coiculescu 포함)가 3D 비압축 Euler와 Navier–Stokes 방정식의 유한 시간 폭발 증명을 냈다. 사람이 읽을 논문과 Lean 형식화가 함께 있다. Tao는 세 결과를 원문 "in decreasing order of how “digested” the results currently are"로 Euler, Navier–Stokes, GRH 순으로 나열했다. Navier–Stokes 해의 정칙성도 밀레니엄 문제 가운데 하나다. 다만 이번 결과가 그 문제의 조건을 그대로 충족하는지에 대한 평가는 아직 없다. Buckmaster(Mastodon 13:37 UTC) 원문: "the Navier–Stokes write-up is unpolished and heavily LLM-assisted." 함정(NS 저장소 README 원문): "The certificate whose hash the paper gives is not in this repository; the paper says that it will be released." 구간 산술 검증도 원문 "is not part of the formalization"이다. Lean만으로 닫힌 증명이 아니다.
- **어떤 AI인가:** 모델 이름은 어느 원문에도 없다. Tao(17:19 UTC) 원문: "Levent Alpöge, using an internal AI model, has generated an (enormous) Lean-only proof of the Generalized Riemann Hypothesis." "internal"은 Tao의 표현이다. 본인 문서에는 "Claude"만 나온다.
- **Anthropic 입장:** The Verge(16:34 UTC) 원문: "Anthropic has not commented or involved itself in Alpöge’s announcement." 본인 문서에는 CEO에게 알린 대목(원문 "I messaged Dario")과 공개를 늦춘 사정(원문 "there was a constraint to respect")이 있다. The Verge는 상장 이후로 공개를 미뤘다는 뜻으로 읽었다.
- **검증 상태:** The Verge 원문: "Alpöge’s solution has yet to be verified independently." TechCrunch(19:32 UTC) 원문: "Several participants in the Zulip chat, including MIT mathematician Andrew Sutherland, say they have successfully confirmed that Alpöge’s solution can be compiled successfully." 컴파일된다는 것과 형식화된 명제가 GRH를 맞게 옮겼는지는 별개다. Aaronson은 Lean의 건전성 문제를 짚었다. 원문: "a sufficiently motivated AI could almost certainly exploit them to prove a false statement! All the same, I see zero indications that that’s happening right now." Tao는 Lean Zulip의 검증 논의를 따라가라고 했다.
- **같은 날 나온 사람 증명:** Ravi Vakil(18:18 UTC)이 Stanford Alessandro Masullo의 GRH 원고를 급히 공개했다. 원문: "It is all human, with no AI." 원문: "Brian Conrad has been looking closely at it for months".
- **왜 중요한가:** 맞다면 AI 수학에서 가장 큰 결과다. 지금 주장의 근거는 1,700만 줄짜리 Lean이 전부다. 검증의 초점이 "증명이 맞는가"에서 "Lean 커널이 건전한가, 명제를 제대로 진술했는가"로 옮겨 갔다.

[Alpöge](https://alpo.ge/remark.pdf) · [Tao](https://terrytao.wordpress.com/2026/11/20/unforced-euler-singularity-unforced-navier-stokes-singularity-and-the-generalized-riemann-hypothesis/) · [Aaronson](https://scottaaronson.blog/?p=10344) · [Vakil](https://math216.wordpress.com/2026/11/20/hurried-announcement-alessandro-masullo-grh/) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1010051/anthropic-riemann-hypothesis-millennium-prize-problem) · [TechCrunch](https://techcrunch.com/2026/11/20/ai-claimed-a-solution-to-the-167-year-old-riemann-hypothesis-and-a-human-did-too/) · [NS 저장소](https://github.com/tristanbuckmaster/navier-stokes) · [Buckmaster](https://mastodon.social/@tristanbuckmaster/117649035783718539) · [HN GRH](https://news.ycombinator.com/item?id=50574968) · [HN Masullo](https://news.ycombinator.com/item?id=50579568) · [HN Tao](https://news.ycombinator.com/item?id=50578358)

### 2. Amazon: 중국 연계 공격자의 Cairn AI 에이전트가 MONAI Label 0-day를 스스로 찾아 서버를 장악했다. PyPI 최신판은 아직 취약하다 (공식, AWS 보안 블로그 17:30 UTC)

- **무엇(원문):** "The actor gave the agents a simple goal: “obtain shell access.”" 원문: "In about 11 minutes, the agents went from mapping the application’s API to arbitrarily writing files on the server." 에이전트는 `sitecustomize.py`에 리버스 셸을 써 넣고, 학습 API로 새 Python 프로세스를 띄워 실행시켰다. 실패한 작전도 적었다. Redis를 148단계에 걸쳐 공격했고, 원문 "the agents kept adapting for nearly a full day without human direction."이다. 어떤 LLM을 썼는지는 글에 없다.
- **CVE(창 안):** CVE-2026-133968이 배정됐다. 전역 GHSA 원문: "as exploited in the wild starting in September 2026."
- **함정(직접 확인):** 저장소 권고(critical, 11-18 15:45 UTC)의 영향 범위는 `<= 0.8.5`이고 수정 버전 칸은 비어 있다. 수정 PR은 10월에 main에 병합됐지만 PyPI 최신은 여전히 0.8.5(2024-11-25)다. pip로 설치했다면 받을 수정판이 없다. 권고 원문: "Authentication is disabled by default (`MONAI_LABEL_AUTH_ENABLE=False`)". 쓰고 있다면 인증을 켜고 인터넷에 노출하지 않는다.
- **왜 중요한가:** AI 에이전트가 0-day를 스스로 찾고 악용했다는 것을 대형 클라우드 회사가 공식 확인했다. 이 브리핑은 09-26 판부터 Cairn을 추적해 왔다. 11-13 FBI FLASH, 직전 판의 EC ARTEX 인정에 이은 사례다.

[AWS](https://aws.amazon.com/blogs/security/chinese-nexus-actors-cairn-ai-agents-found-a-zero-day-on-their-own/) · [MONAI Label 권고](https://github.com/Project-MONAI/MONAILabel/security/advisories/GHSA-qc22-rmgc-vg94) · [GHSA-2f43-rwqm-6h67](https://github.com/advisories/GHSA-2f43-rwqm-6h67) · [The Register](https://www.theregister.com/security/2026/11/20/china-attackers-deployed-ai-agents-that-autonomously-found-and-exploited-a-0-day-to-take-over-a-university-server/5311382)

### 3. 공격 쪽 "에이전트": Bell Canada는 "agentic threat actor"에게 침해됐다고 했고, EC는 공개 AI 도구에 당했다고 재확인했다 (매체가 전한 공식 발언)

- **Bell Canada(BleepingComputer 18:51 UTC):** 회사 원문: "Bell is investigating a cybersecurity incident that occurred on November 17 in which an agentic threat actor accessed some of our systems, including a system used for ordering and provisioning circuits, and exfiltrated some enterprise customer data," 원문: "We currently do not know the attribution". "agentic"이 AI 도구를 뜻하는지는 회사가 설명하지 않았다.
- **EC ARTEX 후속(Help Net 08:52 UTC):** 대변인 원문: "it was an AI tool that is publicly available." 원문: "was detected on the 22nd of October and it was confirmed following the initial investigation on the 26th of October." 늦게 알렸다는 지적에는 원문 "There was no hiding whatsoever,"라고 답했다. CERT-EU의 11-05 경고 원문: "ARTEX’s public availability will likely accelerate adoption by threat actors."

[BleepingComputer](https://www.bleepingcomputer.com/news/security/bell-canada-says-agentic-hacker-stole-enterprise-customer-data/) · [Help Net](https://www.helpnetsecurity.com/2026/11/20/european-commission-ai-generated-cyberattack-publicly-available-tool/)

### 4. CISA KEV 4건 추가, 조치 기한은 모두 11-23이다 (공식, 카탈로그 2026.11.20, 17:48 UTC)

- **추가된 것:** VMware vCenter CVE-2026-123890(직전 판 기술 9번, 7.0은 여전히 공개 패치가 없다), Cisco Expressway CVE-2026-76466(하드코딩된 자격 증명), Apache Roller CVE-2026-129448, SAP NetWeaver CVE-2025-42884.
- **Cisco Expressway(THN 10:32 UTC):** PoC가 공개된 뒤 러시아 기관이 침해됐다는 보도다.
- Java4Shell·Ruby2ßhell·PHP·OpenSSL은 KEV에 없다. 직전 판까지의 기한(Synology·REDCap·Axis 11-21, Puppet 12-01, LiteLLM 12-02)은 그대로다.

[KEV JSON](https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json) · [Cisco](https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-vcs-static-cred-hr7EWZ8t)

### 5. Ruby2ßhell 후속: GitLab·Chatwoot는 고쳤고 GHES는 아직이다. Ubuntu는 이미 고쳤지만 CVE 스캐너에는 안 보인다 (공식)

- **GitLab 19.6.1·19.5.8·19.4.10(태그 11-19 22:54 UTC, 페이지 11-20):** 원문: "The fix for CVE-2026-132825 is a patch to Ruby." 소스 설치 원문: "For self-compiled installations, you must rebuild the Ruby installation used to run GitLab with Ruby 3.3.14 or 3.4.13". Chatwoot v4.20.1(08:29 UTC)도 원문 "Upgraded to Ruby 3.4.13"이다.
- **GHES:** Veria 표("Updated 20 November 2026 at 23:00 UTC") 기준 GitHub Enterprise Server는 "3.22.5 and earlier"가 여전히 Vulnerable이다.
- **PoC 일정에 단서가 붙었다:** 11-20 10:44 UTC Wayback 사본부터 원문이 "on 25 November 2026, or later if major vendors have not shipped a fix by then."이다. GHES가 늦으면 PoC 공개도 늦어질 수 있다.
- **정정과 함정(배포판):** 직전 판은 Ubuntu jammy·noble이 백포트를 기다려야 한다고 썼다. 실제로는 USN-9176-1이 11-19 20:07 UTC에 이미 나와 있었다. 그런데 이 USN에는 CVE 번호가 없다(조회 시점 CVE 목록이 비어 있다). Ubuntu의 CVE-2026-132825 페이지는 아직 `needs-triage`다. CVE 기준으로 스캔하는 도구는 고친 서버도 미수정으로 볼 수 있다. Debian은 DSA-6644-1(11-20)로 trixie를 고쳤고, bookworm과 RHEL 10은 아직이다.
- **Rails(This Week in Rails 11-20):** Rack multipart의 임의 인코딩을 막는 패치를 8.1·8.0에 백포트했다. 원문: "it has been used several times in exploit chains targeting Rails applications." Ruby2ßhell과의 관계는 원문에 없다.

[GitLab](https://docs.gitlab.com/releases/patches/patch-release-gitlab-19-6-1-released/) · [Chatwoot](https://github.com/chatwoot/chatwoot/releases/tag/v4.20.1) · [Veria](https://verialabs.com/ruby2shell/) · [USN-9176-1](https://ubuntu.com/security/notices/USN-9176-1) · [DSA-6644-1](https://security-tracker.debian.org/tracker/DSA-6644-1) · [Rails](https://rubyonrails.org/2026/11/20/this-week-in-rails)

### 6. PHP 후속: Ubuntu 패키지가 USN 없이 먼저 나왔다. 세션 업로드 진행률 기본값 변경까지 들어갔다 (공식, Launchpad 00:46 UTC)

- **게시된 패키지:** jammy php8.1, noble php8.3, resolute php8.5가 보안 포켓에 올라왔다. 조회 시점 USN은 없다.
- **업그레이드 함정(noble 변경 내역 원문):** "Session.upload_progress.enabled is off by default in ext/session/session.c"(CVE-2026-115560). 직전 판 기술 1번의 기본값 변경이 배포판 보안 업데이트로 조용히 들어간다. 업로드 진행률을 쓰는 앱은 `apt upgrade` 뒤 동작이 바뀐다. php.ini에서 명시적으로 켠다.
- **jammy 공백:** php8.1 변경 내역에는 FPM 스머글링(CVE-2026-129289)과 세션 RCE(CVE-2026-126714)가 있다. 그러나 critical인 CVE-2026-115560은 없다. 그 영향 범위(`< 8.2.36`)에는 8.1이 들어간다.
- **악용·RHEL:** Wiz 글은 악용 미관찰 그대로다. RHEL 7~10 php는 모두 "Affected"다.

[noble 변경 내역](https://launchpad.net/ubuntu/+archive/primary/+files/php8.3_8.3.6-0ubuntu0.24.04.14_source.changes) · [jammy 변경 내역](https://launchpad.net/ubuntu/+archive/primary/+files/php8.1_8.1.2-1ubuntu2.29_source.changes)

### 7. OpenSSL CVE-2026-121481: HN 제목의 "CVSS 9.8"은 CISA가 처음 매긴 점수였다. 지금은 5.9이고 OpenSSL 자체 등급은 Low다 (공식, HN 02:01 UTC 360점대)

- **등급(CVE 레코드 원문):** "The severity is Low because no attacker-controlled path to the overflow is known. The caller must already hold a buffer of about two gigabytes before the defect can be reached." 영향 범위는 4.0.0~4.0.3과 4.1.0이다. 수정판 4.0.4·4.1.1은 아직 나오지 않았다.
- **점수 변화:** CISA-ADP가 11-19 22:17 UTC에 9.8을 넣었고, 창 안(19:45 UTC 갱신)에 5.9로 내려갔다(NVD 이력).
- **PR #33652(open) 원문:** "The OpenSSL SRT has asked for it to be fixed publicly; the advisory is still pending." 메인테이너 원문: "Also, this is an *integer* overflow (in the returned result), not *buffer* overflow."
- **영향:** 3.x를 쓰는 엔터프라이즈 배포판은 해당하지 않는다. 4.x를 쓰는 배포판은 수정판이 나오면 올린다.

[PR #33652](https://github.com/openssl/openssl/pull/33652) · [CVE](https://www.cve.org/CVERecord?id=CVE-2026-121481) · [NVD](https://nvd.nist.gov/vuln/detail/CVE-2026-121481) · [HN](https://news.ycombinator.com/item?id=50569226)

### 8. Signal이 OS에 내장된 AI 에이전트의 화면 읽기를 막는다 (공식, 블로그 11-20, HN 09:42 UTC 470점대)

- **무엇(원문):** "we are blocking such capture by preventing screenshots in the app on iOS 27 and when AI assistants attempt to capture screen data on Android." 범위 원문: "We will now be taking similar measures to restrict agentic systems built into iOS 27 and on Android versions where we can detect an AI assistant attempting to read your Signal screen." Gemini와 Siri를 직접 거명했다. 설정에서 끌 수 있다.
- **한계(원문):** "Our remedy is also incomplete". macOS는 원문 "we are also not blocking Siri AI on macOS. This is because we cannot reliably block screenshots on macOS."
- **함정(도구):** 화면을 읽는 computer use 에이전트(도구 3번의 Claude computer use 포함)는 iOS 27·Android의 Signal 화면을 못 볼 수 있다. Signal은 Apple·Google·Microsoft에 앱 단위로 AI 접근을 거부할 수 있는 수단을 요구했다.

[Signal](https://signal.org/blog/protecting-your-privacy/) · [The Register](https://www.theregister.com/security/2026/11/20/signal-slams-door-on-prying-ai-agents-to-protect-privacy/5311466) · [HN](https://news.ycombinator.com/item?id=50572348)

### 9. 에이전트·AI 권고 (직접 확인)

- **MONAI 코어 8건(15:14~16:44 UTC, high 6·medium 2):** 신뢰할 수 없는 번들·설정 파일을 불러오면 코드가 실행된다. 1.6.2에서 고쳤다. 2번의 MONAI Label과는 별개다. [권고 목록](https://github.com/Project-MONAI/MONAI/security/advisories)
- **llama.cpp CVE-2026-133710(high, unreviewed 15:31 UTC):** 조작한 GGUF가 로더에서 정수 오버플로를 일으켜 힙에 쓴다. b12556(11-19 23:12 UTC)에서 고쳤다. 남이 올린 GGUF를 받는다면 올린다. [GHSA](https://github.com/advisories/GHSA-92hv-w2fh-9w89)
- **FlowIntel CVE-2026-133853(medium):** 챗봇이 MCP 세션 하나와 환경 변수 API 키를 모든 사용자와 공유했다. 한 사용자의 신원으로 다른 사용자의 도구 호출이 실행될 수 있었다. MCP 서버를 여러 사용자가 함께 쓰는 구성에서 흔한 실수다. [GHSA](https://github.com/advisories/GHSA-w3q8-9828-cgcr)
- **LiteLLM GHSA-4xm2-x5g5-wc8q(medium):** 도구 5번에 정리했다.
- **악성 패키지 `@guicai777/mcp-doctor`(malware 2건):** "MCP 고치기" 도구로 위장해 Windows 실행 파일을 떨군다. [GHSA](https://github.com/advisories/GHSA-vxx3-mjmc-wgj8)
- **늦은 미러:** 창 안 reviewed 71건은 모두 저장소 공개일이 06-26~11-15인 옛 권고다. 이제 Dependabot 알림만 새로 간다.

### 10. 논문·기법 (저자 주장이고 재현하지 않음)

- **"Twice-Told Critiques"(HF Daily Papers 11-20):** LLM 시대에 동료 평가 지적이 서로 비슷해졌다는 분석이다. 원문: "At ICLR, measured overlap rose from 12.0% in 2026 to 21.3% in 2027". [HF](https://huggingface.co/papers/2611.26286)
- HF Daily Papers 11-20(47편)에 뚜렷한 화제작은 없었다. 창 안 HN 상위 논문 두 편은 게시일이 창 이전이라 아래 창 경계 항목에 넣었다.

### 11. 커뮤니티 논쟁 (커뮤니티)

- **"If you stop writing code, you also get worse at reviewing it"(HN 12:41 UTC 310점대):** 원문: "we somehow lost our way, thinking that our skills won’t stop improving if we stop practicing". [글](https://cekrem.github.io/posts/if-you-stop-writing-code-you-also-get-worse-at-reviewing-it/) · [HN](https://news.ycombinator.com/item?id=50573886)
- **"We promoted developers into architects without informing them"(HN 230점대):** 바이브 코딩으로 넣은 로깅 때문에 원문 "the welcome screen took 15 seconds to load"였다는 사례다. [글](https://manidoraisamy.com/developer-to-architect.html) · [HN](https://news.ycombinator.com/item?id=50579331)
- **"Vibecoding an inference chip"(HN 90점대):** 원문: "I vibe-coded an FPGA into running an LLM." [글](https://ankitg.me/blog/vibecoding-inference-chip/)

### 12. 짧게

- **(AI 무관) Visa·Mastercard·JPMorgan 고객 데이터가 제3자 업체를 통해 유출됐다(The Register 21:27 UTC):** SOCRadar 원문: "We found no card numbers, expiry dates, CVVs, passwords, dates of birth, or home addresses in any file". 경로 평가 원문: "The timing fits the Qubole supply-chain campaign". 확인된 사실이 아니라 평가다. [The Register](https://www.theregister.com/cyber-crime/2026/11/20/visa-mastercard-theft-included-info-useful-for-phishing-but-not-draining-bank-accounts-say-researchers/5311478) · [BleepingComputer](https://www.bleepingcomputer.com/news/security/vendor-data-breach-exposes-chase-visa-mastercard-customer-info/)
- **(AI 무관) 그 밖:** KVM CVE-2026-99312는 AMD 호스트에서 게스트가 호스트 메모리를 읽는다(THN 12:20 UTC). Ubuntu jammy·noble은 "needed"다. WhatsApp은 사기에 악용됐을 수 있는 Android 결함을 고쳤다(THN 17:57 UTC). SonicWall은 Gen 6 방화벽에 더는 패치를 내지 않는다(THN 10:38 UTC). Windows 잠금 화면 우회 FastUnlock은 아직 패치가 없다(THN 05:30 UTC).
- **직전 판 후속:**
  - NGINX2Root: RHEL 10이 고쳤고 RHEL 8·9는 아직이다. Ubuntu USN-9182-1이 nginx CVE 3건을 더 고쳤다. 커널 결함 CVE와 악용 보고는 없다.
  - Java4Shell: KEV 미등재이고, 성공한 침해 보고는 없다.
  - Synology SA-26:21: 수정 빌드 파일은 아카이브에 올라왔지만(11-20 03:30~05:24 UTC), 권고 페이지는 여전히 "Ongoing"이다. [아카이브](https://archive.synology.com/download/Os/DSM/7.4.1-90080-2)
  - Apify: 예비 보고서를 냈다고 상태 페이지에 알렸다(16:58 UTC). 원문: "A full incident report will follow once the investigation and remediation are complete." 보고서 본문은 열지 못했다. [Apify](https://status.apify.com/incidents/sxd1dj5j6749)
  - Microsoft agent-framework·ONNX Runtime 저장소는 여전히 404이고 공식 설명이 없다.
  - 진전 없음: AutoGPT 0.12.0 보안 권고(아직 미공개), crewAI, `mcp-server-git`, LiteLLM KEV 새 악용 보고, Fireworks, FBI 새 경보, Microsoft 365 사후 보고서.
  - 런타임: Node.js·Go·Python·Chrome은 창 안 새 보안 릴리스가 없다. OpenSSL은 7번에 정리했다.

### 참고: 창 경계 항목

- **(게시 11-16·v2 11-18, HN 11-20 16:29 UTC 310점대) "Language Models Can Rewrite Their Own Weights"(arXiv 2611.21885):** 원문: "an open 27B model, given its weights, its graded answers and the ability to run itself, diagnoses its own failure and rewrites the weights behind it." 원문: "its own edit makes it 6.6 points more accurate on problems it never saw." [arXiv](https://arxiv.org/abs/2611.21885) · [HN](https://news.ycombinator.com/item?id=50577265)
- **(글 날짜 11-18, HN 11-20 10:01 UTC 600점대) Frostbite: AI가 ISA부터 물리 설계까지 했다는 CPU(Architect Labs):** 원문: "we achieved a 4.59x performance-per-watt multiple when compared to a modern, silicon-proven superscalar (dual-issue, in-order) CPU core." 함정 원문: "These are, of course, still pre-silicon power numbers". 비교 대상이 in-order 듀얼 이슈 코어다. [Architect Labs](https://architectlabs.com/blog/frostbite) · [HN](https://news.ycombinator.com/item?id=50572503)

## 써볼 만한 도구

이번 창에서도 새 도구보다 업그레이드 함정이 많았다. Claude Code 2.1.330은 관리 정책 하나의 의미를 바꿨다. Codex CLI와 Pi는 기본 모델을 GPT-6.2 Sol로 바꿨다. GPT-6.2 캐시 함정(직전 판 도구 2번)에는 LiteLLM의 두 번째 수정과 Zed·Pi 수정이 나왔다. 새로 써 볼 만한 것은 Team·Enterprise용 Claude computer use 베타와 OpenAI Agents SDK 1.0 알파의 Codex 제공자 정도다. 도구는 하나도 설치하거나 실행하지 않았다. 함정과 인용은 릴리스 본문, PR·이슈 본문, 문서에서 직접 읽었다.

### 1. (업그레이드 주의) Claude Code 2.1.330: `deniedMcpServers` 의미 변경, rm 확인 확대, max_tokens 재시도 복구 (공식, npm 19:16 UTC, 릴리스 22:32 UTC)

- **dist-tags(조회 시점):** `latest`·`next` 2.1.330, `stable` 2.1.323이다. 직전 판 조회 때 `stable`은 2.1.311이었다.
- **관리 정책 의미 변경:** 원문: "Changed `deniedMcpServers` name entries to hide only the named claude.ai connector, not others at its URL; to block them all, deny its exact URL". 이름으로 커넥터를 막던 조직에서는 같은 URL의 다른 커넥터가 통과한다. URL 항목으로 바꾼다.
- **rm 확인 확대:** 원문: "Changed more `rm` commands aimed at the root, home, a top-level folder or the working folder to ask for approval; where nobody can answer, they're refused". `claude -p` 같은 무인 실행에서 작업 폴더를 지우는 스크립트는 거부될 수 있다.
- **직전 판 함정 복구:** 원문: "Reverted 2.1.329's removal of the automatic retry when `max_tokens` exceeds a model's or gateway's limit". 직전 판 도구 1번에서 본 게이트웨이 함정이 하루 만에 되돌아갔다.
- **게이트웨이 모델명:** 원문: "Changed unrecognized model names containing "sonnet" or "haiku" to use the shorter system prompt, like other unrecognized names; `CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT=0` gives every model the full one". 게이트웨이에서 별칭 모델명을 쓰면 동작이 달라질 수 있다.
- **그 밖의 변경:**
  - 일정 작업 원문: "Changed recurring scheduled tasks and `/loop` to run up to 30 days instead of 7". 오래 돌수록 비용이 는다.
  - 내장 차트 mod 원문: "Added easel, a built-in mod that draws charts when asked and under MCP metrics results". 끄는 곳은 원문 "turn automatic charts off in `/config`"이다. MCP 허용 목록을 쓰는 조직에서는 원문 "the built-in chart tool, which stays blocked"대로 막힌 채 남는다.
  - self-hosted runner 원문: "most such lines now lead with the server's reason, so update log alerts that match the old words".
- **좋아진 것:** 원문: "Fixed `ExitWorktree` and the worktree exit dialog deleting a worktree that another running Claude Code session was working in". 원문: "Fixed changes to user settings being lost when several Claude Code processes saved them at the same time".
- **Agent SDK TS 0.3.330(호환성 깨짐, npm 19:18 UTC):** 원문: "Changed the TypeScript SDK's `query()` to fail with an error when `settings` is a JSON string that doesn't parse". 지금까지 조용히 무시되던 잘못된 설정 문자열이 이제 오류가 된다. Python SDK 0.2.186(직전 판 창 직후)은 본문이 비어 있고 번들 CLI를 2.1.329로 올린 커밋 하나뿐이다.
- **직전 판 열린 이슈(#111167 rm 접두사 glob):** 여전히 open이고 직원 답이 없다. 창 안 커뮤니티 댓글(18:14 UTC) 원문: "Still reproduces on 2.1.329, and not only in bypassPermissions/auto: in default mode with a `Bash(rm:*)` allow rule the same prefix glob runs without a prompt." 2.1.330 노트에는 이 이슈의 수정 항목이 없다. #110979·#110642·#111218·#111068도 열려 있다.

[2.1.330](https://github.com/anthropics/claude-code/releases/tag/v2.1.330) · [Agent SDK TS 0.3.330](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.330) · [#111167](https://github.com/anthropics/claude-code/issues/111167)

### 2. (보안 강화) claude-code-action 1.0.285: 쓰기 권한 없는 PR에서는 입력이 가리키는 설정 파일도 base 브랜치 판을 쓴다 (공식, 릴리스 22:39 UTC)

- **무엇(PR #2099 원문):** "On pull requests from authors without write access, the action already takes `.claude/`, `.mcp.json` and similar files from the base branch." 이번 판부터 `settings`, `plugin_marketplaces`, 그리고 `claude_args`의 `--settings`·`--mcp-config`·`--plugin-dir`이 가리키는 파일까지 같은 방식이다. 외부 기여자가 PR에서 설정 파일을 바꿔 워크플로를 조작하는 경로가 막힌다.
- **함정(원문):** "Pull requests that change such a file no longer try out the change." 원문: "A file that exists only in the pull request is removed, so an input that names it can fail." 영향이 없는 경우 원문: "Nothing changes for authors with write access, with `restrict_untrusted_prs: false`, or for workflows whose inputs name no files in the checkout."
- 2분 뒤 나온 1.0.286은 1.0.285와 커밋 차이가 없다.

[PR #2099](https://github.com/anthropics/claude-code-action/pull/2099) · [1.0.285](https://github.com/anthropics/claude-code-action/releases/tag/v1.0.285)

### 3. (제품 기능) Claude computer use 베타가 Team·Enterprise에 나왔다. 기본은 꺼져 있고 폴더 제한이 적용되지 않는다 (공식, 릴리스 노트 11-20, 날짜만)

- **무엇(릴리스 노트 원문):** "It's now available in beta on Team and Enterprise plans, and is rolling out to organizations gradually."
- **관리자가 알아 둘 것(설정 문서 원문):**
  - "Computer use is in beta on Team and Enterprise plans, and it's off by default."
  - "Computer use has no sandbox between Claude and a user's applications." 폴더 연결 제한에 대해서는 "that limit doesn't apply to computer use"다.
  - 켜면 바로 퍼진다. 원문: "Users whose role is User, Admin, Owner, or Primary Owner get computer use as soon as you turn it on." 끌 때는 느리다. 원문: "When you turn computer use off, the change can take about an hour, sometimes longer, to reach each user's Claude Desktop."
  - 안전 검사 기록 원문: "Anthropic can keep a copy of it in its record of the check."
  - Cowork 작업과 Claude Code에서는 쓸 수 없다. HIPAA-ready 구성, Claude Code ZDR, CMEK, FERPA·K-12 조직은 켤 수 없다.
- **추천 이유:** 직전 판에서 본 Gemini 앱 Mac computer use는 미국 영어 사용자 한정이었다. 이쪽은 조직 단위로 켤 수 있다. 다만 샌드박스가 없으니 파일럿 그룹부터 연다.

[릴리스 노트](https://support.claude.com/en/articles/12138966-release-notes) · [설정 문서](https://support.claude.com/en/articles/18483812-set-up-computer-use-for-team-and-enterprise-plans)

### 4. (기본 모델 변경) Codex CLI 0.171.0과 Pi 1.6.0의 기본 모델이 GPT-6.2 Sol이 됐다 (공식, 00:44 · 11:35 UTC)

- **Codex CLI 0.171.0:** 직전 판에서 기다리던 0.170.0 이후 첫 안정판이다. 원문: "Added GPT-6.2 Sol and Astra to model catalogs, with Sol as the default catalog selection." 원문: "Enabled the TUI workspace by default". 샌드박스도 강화됐다. 변경 목록 원문: "Enforce Linux seccomp restrictions for x32 syscalls", "Block `request_key` in Linux network sandbox filters".
- **Pi 1.6.0:** 원문: "GPT-6.2 Sol is now the default model for all three." 셋은 `openai`·`azure`·`openai-codex` 제공자다. 함정 원문: "Both reject reasoning effort `none` and `minimal`, so thinking level `off` is unavailable."
  - find 도구 동작 변경 원문: "`src/*.ts` finds `src/a.ts` but no longer `pkg/src/a.ts`".
  - llama.cpp 확장 원문: "now connects to `http://127.0.0.1:9931` by default".
- **할 일:** 모델을 고정하지 않았다면 비용과 지연이 달라진다. 설정 파일에서 모델을 명시한다.

[Codex 0.171.0](https://github.com/openai/codex/releases/tag/rust-v0.171.0) · [Pi 1.6.0](https://github.com/earendil-works/pi/releases/tag/v1.6.0)

### 5. (후속, 직전 판 도구 2번) GPT-6.2 `instructions` 캐시 함정: LiteLLM 두 번째 수정, Zed·Pi 수정. pydantic-ai 수정안에는 다른 함정이 있다 (공식 릴리스·PR)

- **LiteLLM 1.110.2(PyPI 04:05 UTC, 1.109.3·1.108.13·1.106.12 동시):** 원문: "Fixed GPT-6.2 system prompts missing the prompt cache on `/v1/chat/completions` and `/v1/messages` unless `store: false` (#52906)". 직전 판의 1.110.1은 `store: false` 경로만 고쳤다. 기본값 경로를 쓰던 사람은 1.110.2가 필요하다.
  - 같은 판에 보안 수정(GHSA-4xm2-x5g5-wc8q, medium)도 있다. 원문: "could read the stored record of any other virtual key that is also not assigned to a user". 원문: "Raw API keys and key hashes are not returned".
- **Zed 1.29.3(16:10 UTC):** 원문: "Fixed prompt caching for reusable OpenAI system instructions on GPT-6.2 and later." PR #67252 원문: "Verified in a local Zed build with GPT-6.2 Sol: 98.4% of input tokens were cached on the final request."
- **Pi 1.6.0:** 원문: "Fixed the `openai-codex` provider getting almost no prompt cache hits for the system prompt on GPT-6.2 models".
- **수정안의 새 함정(pydantic-ai PR #14095, open):** 지시문을 input으로 옮기면 `previous_response_id`로 이어 받는 다른 에이전트가 앞 에이전트의 규칙을 물려받는다는 보고다. 원문: "A second agent continuing that way followed a rule of the first agent's that it didn't override in 12/12 live runs (0/12 on main)." 원문: "This PR doesn't close it." 직접 고칠 때도 응답을 체인으로 잇는 다중 에이전트 구성이면 같은 문제를 본다.
- **아직이다:** goose 1.60.2(06:18 UTC)에는 #13663이 들어가지 않았다. OpenAI Agents SDK, Cline #15797, Codex #62312도 그대로다. OpenAI 공식 언급은 새로 없다.

[LiteLLM 1.110.2](https://github.com/BerriAI/litellm/releases/tag/v1.110.2) · [LiteLLM 권고](https://github.com/BerriAI/litellm/security/advisories/GHSA-4xm2-x5g5-wc8q) · [Zed 1.29.3](https://github.com/zed-industries/zed/releases/tag/v1.29.3) · [Zed PR #67252](https://github.com/zed-industries/zed/pull/67252) · [pydantic-ai PR #14095](https://github.com/pydantic/pydantic-ai/pull/14095)

### 6. (보안 수정) Vercel AI SDK 7.0.178: 자동 거부된 도구 호출이 승인 서명을 받던 결함 (공식, 릴리스 16:51 UTC)

- **무엇(릴리스 원문):** "A client that controls the message history could send that request back with an approved response and have the denied tool call executed."
- **함정(원문):** "Signatures that earlier versions issued for automatic denials stay valid. To invalidate them, rotate the secret passed to `experimental_toolApprovalSecret`." 올리기만 해서는 끝나지 않는다. 비밀값을 바꾼다.
- **영향 범위(조회 시점):** 수정은 7.x에서만 확인됐다. v6 라인(최신 6.0.333, 11-17)에는 새 판이 없고, 저장소 보안 권고도 없다.

[릴리스](https://github.com/vercel/ai/releases/tag/ai%407.0.178)

### 7. (새 기능, 알파) OpenAI Agents SDK Python 1.0.0-alpha.1: Codex를 모델 제공자로 쓴다 (공식, 20:51 UTC)

- **무엇(원문):** "adds Codex as an opt-in model provider for the Python Agents SDK". 원문: "The wheel embeds unmodified Codex Core `rust-v0.166.0` through PyO3."
- **추천 이유:** `codex app-server`를 따로 띄우지 않고, 같은 에이전트를 API 제공자와 Codex 사이에서 바꿔 돌릴 수 있다.
- **함정(원문):** "available exclusively through GitHub Releases". PyPI 최신은 0.25.0이다. 원문: "Its `SessionState` checkpoints use a different format from the Runner's `RunState`." 저장된 Codex 로그인만 쓴다면 `OPENAI_AGENTS_DISABLE_TRACING=1`을 켜야 한다. 내장 Codex Core는 0.166.0이라 현재 CLI(0.171.0)보다 낮다.

[릴리스](https://github.com/openai/openai-agents-python/releases/tag/v1.0.0-alpha.1)

### 8. (권한 정책 강화·기본값 변경) Mistral Vibe 3.1.4 (공식, GitHub 15:46 UTC)

- **보안 강화(원문):** "An agent no longer decides what runs without asking: its `safety`, `bypass_tool_permissions` and `permission = "always"` settings are ignored, and a session runs your `default_approval_policy` whatever its agent." 저장소 쪽 설정 원문: "A project's `.vibe/agents/` can no longer redefine an approval policy (`ask.toml`, `auto-approve.toml`, ...): the file is ignored with a config issue." 신뢰하지 않는 저장소가 자동 승인을 켜는 경로가 막혔다.
- **함정(원문):** "Models that don't set `temperature` now use 1.0 instead of 0.2". 원문: "Custom compaction prompts: `compaction_prompt_id` no longer has an effect". 원문: "Vibe now trusts the operating system's certificates as well as its own by default". 로컬 세션은 Session API 엔진으로 바뀌었고, 되돌리려면 `experimental_local_runtime_session_api = false`를 쓴다.

[릴리스](https://github.com/mistralai/mistral-vibe/releases/tag/v3.1.4)

### 9. (실행 지원) Inkling 2 Mini: 발표는 "vLLM·Ollama·SGLang에서 된다"였지만 창 종료 시점에는 대부분 PR 단계였다 (공식 + 저장소)

- **발표 원문:** "Open source inference is available on SGLang, vLLM, Ollama, Unsloth, NVIDIA Dynamo, TokenSpeed, and Llama App". 모델 자체는 모델 소식 3번에 정리했다.
- **실제 상태(조회 시점):**
  - vLLM: 지원 PR #69028(20:21 UTC)이 열려 있고 정식 릴리스는 0.34.0 그대로다. 전용 이미지 `vllm/vllm-openai:inkling-2-mini`는 있다. 하지만 H100에서는 원문 "no kernel image is available for execution on the device" 오류가 난다는 버그 #69046(22:51 UTC)이 올라왔다. 보고자는 이미지에 sm_90용 Inkling 커널이 없다고 분석했다.
  - Ollama: 라이브러리 페이지 원문 "Requires a Mac with 32GB+ memory". 원문 "Download the pre-release". 안정판에는 아직 없다.
  - SGLang 지원 PR #51926과 llama.cpp PR #32695는 열려 있다.
- **할 일:** 지금 써 보려면 전용 도커 이미지나 프리릴리스를 써야 한다. 안정판 지원을 기다린다면 다음 판에서 다시 본다.

[Thinking Machines](https://thinkingmachines.ai/news/inkling-2-mini/) · [vLLM PR #69028](https://github.com/vllm-project/vllm/pull/69028) · [vLLM #69046](https://github.com/vllm-project/vllm/issues/69046) · [Ollama](https://ollama.com/library/inkling-2-mini)

### 짧게

- **(제품 기능) ChatGPT Space가 Plus에 나왔다(릴리스 노트 11-20):** 원문: "Starting today, ChatGPT Space is rolling out globally to Plus users ages 18 and older." 원문: "It replaces Library". 함정 원문: "People on Free can't open shared pages or presentations. Public links aren't available." [ChatGPT 릴리스 노트](https://help.openai.com/en/articles/6825453-chatgpt-release-notes)
- **(제품 기능) Meta Muse Code 3D 모델 생성(블로그 11-20):** 원문: "Subscribers to the Everyday Usage plan have access to create ~30 3D models before maxing their usage for a 5-hour period." 원문: "The cost of 3D object creation counts against your subscription." [Meta](https://dev.meta.ai/resources/blog/build-3d-models-with-muse-code)
- **claude-plugins-official security-guidance 2.0.21(20:33 UTC 머지):** PR #7177 제목은 "skip other repositories whose settings change what git runs"다. 이런 저장소에서는 원문 "commit, push and end-of-turn reviews do not run, and nothing says so"다. 검토가 조용히 빠지니 알아 둔다. PR #7176은 원문 "every hook leaves an empty file named `(3` in the project"인 pyenv-win 문제를 고쳤다. [PR #7177](https://github.com/anthropics/claude-plugins-official/pull/7177) · [PR #7176](https://github.com/anthropics/claude-plugins-official/pull/7176)
- **Anthropic SDK Python 2.12.0(15:56 UTC):** Bedrock `user_profile_id`를 이제 본문으로 보낸다. 함정 원문: "A request to one of them with a profile ID now fails with a 400 error; before, the ID was ignored." [릴리스](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v2.12.0)
- **llama.cpp b12580:** 11-12 변경으로 MCP 프록시가 사설 주소를 거부하게 됐다. 이번에 `--ui-mcp-proxy-allow-private-urls` 옵트인이 생겼다. 서버 README 원문: "anyone who can reach the proxy can then reach your local network". [PR #32633](https://github.com/ggml-org/llama.cpp/pull/32633) · [서버 README](https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md)
- **browser-use 1.0.7(20:01 UTC):** 원문: "A long `browser_exec` now replies within 50 s with the output so far". 60초 제한이 있는 MCP 클라이언트용이다. [릴리스](https://github.com/browser-use/browser-use/releases/tag/1.0.7)
- **Hermes Agent v0.23.3(00:34 UTC):** 기본값 변경 원문: "the proactive tool-result prune on by default". 프롬프트 캐시 TTL도 원문 "defaulting to `auto`"가 됐다. The Decoder(09:59 UTC)가 다룬 Collective Wisdom은 Nous 원문 게시가 11-18이라 창 밖이다. 원문: "employees can ask Hermes which of their skills are worth sharing, then propose and publish them for their team with review." [릴리스](https://github.com/NousResearch/hermes-agent/releases/tag/v0.23.3) · [Nous](https://nousresearch.com/hermes-enterprise-in-microsoft-teams)
- **MCP Events 후속:** ext-events PR #24(19:30 UTC 머지) 제목은 "Add Events extension spec at Beta (SEP-3415)"다. SEP는 Final이지만 확장 수명 단계로는 Beta다. TypeScript SDK 구현 PR은 열려 있다. OpenAI 문서는 ChatGPT가 원문 "webhook delivery and callback verification"을 초안 명세 기준으로 지원한다고 적었다(문서 게시 시각 미확인). [PR #24](https://github.com/modelcontextprotocol/ext-events/pull/24) · [OpenAI 문서](https://developers.openai.com/plugins/build/mcp-events)
- **GHES 3.23 RC(20:41 UTC):** AI 비밀 탐지가 퍼블릭 프리뷰다. 원문: "A self-contained, fine-tuned model runs on the appliance's CPU without a GPU, hosted LLM, or outbound connection." [GitHub](https://github.blog/changelog/2026-11-20-github-enterprise-server-3-23-release-candidate)
- **Microsoft Foundry Managed Compute GA(15:00 UTC):** [Foundry](https://devblogs.microsoft.com/foundry/managed-compute-ga/)
- **Show HN·HN:** OpenComputer(Show HN 30점대, 20:19 UTC)는 자기 클라우드 계정에서 돌리는 오픈소스 클라우드 코딩 에이전트다(Apache-2.0). 속도 주장은 재현하지 않았다. DreamCoder(HN 50점대, 20:03 UTC)는 Mastering Emacs 저자가 낸 브라우저 코딩 하네스다. 원문: "DreamCoder is a frontier coding harness with a tiling window manager that runs in your browser on all 3 platforms." [OpenComputer](https://github.com/diggerhq/opencomputer) · [DreamCoder](https://www.masteringemacs.org/article/announcing-dreamcoder)
- **그 밖:** FastMCP 4.2.3, langchain-anthropic 1.7.12·langchain-openai 1.8.5(비동기 HTTP 클라이언트 공유 수정), OpenHands 1.43.0, Google ADK Python 2.15.0 PyPI 게시(00:30 UTC, 직전 판 도구 7번)는 작은 변경이다.
- **직전 판 후속 중 새 진전 없음:** OpenCode(2.3.2), VS Code(1.147.0), Gemini CLI(`latest` 0.70.0, nightly만), Crush(nightly만), GitHub MCP Server, Playwright MCP, vLLM 정식 릴리스, Open WebUI, MCP 공식 SDK·Inspector·레퍼런스 서버, Context7, Dify, Cursor, Ollama 안정판, transformers, Strands, crewAI, Qwen Code(프리뷰만), Amp, Mastra. Copilot CLI는 프리릴리스만 나왔다. AutoGPT 0.12.0 보안 권고는 아직 공개되지 않았다. `anthropics/skills`·`openai/plugins`는 창 안 커밋이 없다.

## 주목할 점

- **AI 수학의 병목이 "증명"에서 "검증 도구"로 옮겨 갔다.** GRH 주장은 사람이 읽을 원고 없이 Lean 1,740만 줄로 나왔다. 컴파일은 된다는 보고가 있다. 이제 Lean 커널이 건전한지, 명제를 제대로 옮겼는지가 관건이다. Lean Zulip의 검증 논의와 Clay 연구소의 반응을 지켜본다. 같은 날 사람만으로 쓴 GRH 원고가 몇 달째 검토 중이었다는 사실도 비교 기준이 된다.
- **공격 쪽 에이전트는 이미 실전이고, 방어 쪽 패치 경로는 군데군데 끊겨 있다.** Cairn은 0-day를 11분 만에 찾았다. 그런데 MONAI Label은 PyPI 수정판이 없다. Ubuntu Ruby USN은 CVE 번호가 없어 스캐너에 안 잡힌다. PHP 기본값 변경은 `apt upgrade`로 조용히 들어온다. 업스트림 패치 다음 단계(패키지 게시·스캐너 반영·기본값 고지)를 따로 챙겨야 한다.
- **다가오는 일정:**
  - 이번 주말: 이번에 KEV에 추가된 4건의 조치 기한(11-23).
  - 다음 주: **Ruby2ßhell Rails PoC 공개(11-25, 대형 공급사 패치가 늦으면 연기)**, ERNIE 5.5 Flash Preview 가중치 공개 약속(11-25 전), Qwen4-Max-Preview 종료(11-25), Gemini 4 Carbon·Argon 도입가 종료(11-30), Huawei openPangu-2.0 종료(11-30), Puppet Enterprise KEV 조치 기한(12-01), LiteLLM KEV 조치 기한(12-02).
  - 12월 이후: 영국 Copilot 테넌트 Claude 기본 켜짐(12-09), Val Town 읽기 전용(12-10), GitHub Copilot 모델 6종 은퇴(12-14), Inception Mercury API 종료(12-28), BIS 원격 접근 규칙 의견 마감(2027-01-19), Windows 기본 소형 모델 Aion 탑재(2027년 1월 말), 다음 MCP 명세(03-30).

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - openai.com 글(증류 방어, MedChemBench)과 ChatGPT 릴리스 노트는 403이라 r.jina.ai로 읽었다. Alpöge 문서는 PDF라 r.jina.ai로 텍스트를 뽑았다. 수식 일부가 깨졌을 수 있지만 인용한 문장은 일반 문장이다.
  - Lean Zulip은 인증이 필요해 GRH 검증 논의를 직접 보지 못했다. Reuters는 캡차, WSJ는 유료, Politico는 열지 않았다. The Information·Bloomberg 보도(Inception 인수, Apple, Pragmatik)는 2차 보도로만 봤다. THN 본문은 Cloudflare에 막혀 RSS 설명만 썼다. Apify 보고서는 Trust Center 뷰어라 열지 못했다. x.com 타임라인과 reddit은 막혀 있다.
- **시각이 날짜만 있거나 불확실한 원문:**
  - OpenAI 증류 글은 RSS 날짜뿐이라 HN 첫 게시(20:11 UTC)와 Wayback 첫 캡처(20:14 UTC)로 범위를 잡았다. Inkling 2 Mini는 HF 커밋 이력이 정리돼 블로그·HN·Wayback으로 잡았다. Hy4 공개 시각은 HF `lastModified`(03:05 UTC)와 GitHub 푸시로만 잡았다. Alpöge 문서의 최초 확인 시각은 HN 게시(14:16 UTC)다.
  - 날짜만: Claude·ChatGPT 릴리스 노트, OpenAI RSS 글, Signal 블로그, Muse Code 블로그, GitLab 패치 페이지(태그 시각으로 보완), Rails 주간 소식, Debian DSA, RHSA. Synology 수정 빌드는 디렉터리 시각으로만 잡았다. OpenAI MCP Events 문서는 게시일을 모른다.
- **확인하지 못한 것:**
  - GRH·Euler·NS 증명에 쓴 모델과 버전, Anthropic의 관여 여부. Bell Canada가 말한 "agentic"의 뜻. Enterprise Claude Data 기본 켜짐의 실제 전환. Gemini 4 Neon 안전 보고서(추정 URL만 조회). Anthropic 공모 종결 8-K(조회 시점 미제출).
  - 재현하지 않은 것: 모든 증명·벤치마크·성능 주장(Inkling 2 Mini, MedChemBench, MEI, Frostbite, 자기 가중치 수정 논문), Zed 98.4% 캐시 적중, pydantic-ai 12/12, Inkling 이미지 버그, 모든 PoC와 발견자 주장. 도구와 패키지는 하나도 설치하거나 실행하지 않았다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 80점 이상 스토리는 제목을 전부 봤다. 20점 이상은 3시간 구간으로 나눠 키워드로 걸렀다. Show HN은 5점 이상을 봤다.
  - 전역 GHSA는 reviewed 71건, unreviewed 659건, malware 132건을 창 범위로 받아 셌고, unreviewed는 AI 키워드로 걸렀다. 저장소 권고는 AI·도구 저장소 81곳을 조회했다. 도구 릴리스는 약 60곳을 봤다.
  - HF 조직 스캔은 조직당 15개만 봐서 Hy4 공개를 놓쳤고, 401 추적 목록을 직접 조회해 찾았다. HF Daily Papers 11-20은 봤지만 arXiv 목록은 직접 보지 않았다. GitHub 트렌딩은 1면만 봤다. Help Net 금요일 주간 요약은 조회 시점까지 나오지 않았다.
  - 한국 랩은 HF 조직 스캔과 국내 기사 제목 검색만 했다. Vertex AI 릴리스 노트(캐시 동결)와 Moonshot·Volcengine 변경 로그(월 단위)는 일 단위로 판정하지 못했다.
  - 창이 끝날 때 열려 있던 것: Claude Code #110979·#110642·#111167·#111218·#111068, Agent SDK Python의 2.1.330 대응판, goose 캐시 수정 릴리스, OpenAI Agents SDK의 `instructions` 처리, Inkling 2 Mini의 vLLM·SGLang·llama.cpp·Ollama 안정판 지원, MONAI Label PyPI 수정판, OpenSSL 4.0.4·4.1.1, GHES Ruby 패치, Ubuntu PHP USN, RHEL PHP·Ruby·nginx 8·9 패키지, AutoGPT 0.12.0 보안 권고, Microsoft 저장소 복구. 다음 판에서 다룬다.
