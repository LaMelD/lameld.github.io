---
title: "2026-11-12 AI 브리핑"
date: 2026-11-12T01:00:00+09:00
tags: [ai-briefing, qwen, anthropic, mcp, security]
description: "Alibaba가 Qwen4-Max를 2주 한정 미리보기로 처음 열고 Baidu가 ERNIE 5.5 Flash 사후 학습판을 냈으며, Grok Build 앱 3만 5천여 개가 로그인 없이 열렸고, VS Code와 MCP 레퍼런스 서버가 MCP를 거친 코드 실행 취약점을 한꺼번에 고쳤다."
---

> 조사 범위: 2026-11-11 01:00 ~ 2026-11-12 01:00 KST(2026-11-10 16:00 ~ 11-11 16:00 UTC, 미국 화요일 08:00 ~ 수요일 08:00 PST). 직전 브리핑(11-11 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, X 게시 ID 역산, GitHub `published_at`·`merged_at`, npm·PyPI 게시 시각, 전역·저장소 GHSA `published_at`, CISA KEV 카탈로그 시각, OpenRouter 등록 시각, Hugging Face API, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-11 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **새 모델은 중국에서 나왔다.** Alibaba가 Qwen 4 세대의 첫 모델 Qwen4-Max-Preview를 열었다. Token Plan 전용 2주 미리보기이고, 기간이 끝나면 내린다. Baidu는 ERNIE 5.5 Flash의 첫 사후 학습판을 내고, 가중치를 11-25 전에 Apache 2.0으로 열겠다고 했다. 미국 3사에는 새 언어 모델이 없다. Anthropic은 11-18에 공모가를 정하고 11-19에 거래를 시작한다.
- **공유 기본값과 공급사 약정이 데이터를 내보냈다.** Grok Build로 만든 앱 35,578개가 로그인 없이 열렸다. 기본 공유 설정이 "링크가 있는 누구나"였다. Notion은 Fireworks가 데이터 무보존(ZDR)을 약정했는데도 데이터가 남아 있었다고 밝혔다. Git과 Go는 클론·빌드 단계의 코드 실행을 고쳤다.
- **MCP를 거친 코드 실행이 클라이언트와 서버 양쪽에서 고쳐졌다.** VS Code 1.145.3은 권고 16건을 고쳤고, 그중 3건이 MCP 관련이다. MCP 레퍼런스 서버는 git 서버의 읽기 도구가 명령을 실행하던 문제 등 6건을 고쳤다. filesystem 서버는 버전 체계가 바뀌어 자동으로 올라가지 않는다. Anthropic은 Claude Sheets·Whiteboards와 MCP Apps 기반 interactive connectors를 냈다.

## 모델 소식

미국 화요일 창이다. 새 모델은 중국 두 곳에서 나왔다. Alibaba는 Qwen 4 세대를 기간 한정 미리보기로 처음 열었고, Baidu는 ERNIE 5.5 Flash의 첫 사후 학습판을 냈다. 미국 3사에는 새 언어 모델이 없다. Anthropic은 공모 수요 보도가 이어졌고, Google은 유전체 모델 AlphaGenome 2를 냈다. OpenAI는 ChatGPT의 옛 음성 모델 은퇴 일정을 알렸다. Claude 업무 도구 묶음과 ChatGPT Business Agents는 도구 섹션에 정리했다.

### 1. Alibaba가 Qwen4-Max-Preview를 열었다. Token Plan 전용으로 2주만 쓸 수 있고, 기간이 끝나면 모델을 내린다 (공식, X 게시 11-11 02:01 UTC, HN 40점대)

Qwen 4 세대가 처음 공식으로 나왔다. Qwen 계정 원문: "Qwen4 is coming." 이어서 원문: "Qwen4-Max is now in preview on Alibaba's Token Plan." 모델 ID는 `qwen4-max-preview`다. 텍스트·이미지·영상을 입력으로 받고, 컨텍스트는 1M이다. 최대 출력은 393,216토큰이고 사고 토큰을 포함한다.

- **기간 한정:** 문서 원문: "Qwen4-Max-Preview is currently in preview and is available through Token Plan only. Pay-as-you-go access is not supported." 기간은 원문: "The preview period runs from 10:00 on November 11, 2026 to 24:00 on November 25, 2026 (UTC+8)"이다. UTC로는 11-11 02:00 ~ 11-25 16:00이다. 끝나면 원문: "the model will be retired after the preview ends."
- **가격:** Token Plan 가격 페이지가 "80% Off" 한정 할인으로 표시한다. 벤치마크와 종량제 단가는 공개되지 않았다.
- **함정:** 원문: "Starting with `qwen4-max-preview`, `max_tokens` is equivalent to `max_completion_tokens`. Both limit the combined tokens for thinking and the final answer." 기존 Qwen처럼 답변 길이만 보고 한도를 잡으면 출력이 잘릴 수 있다. 사고 강도 기본값은 `xhigh`다.
- Hugging Face에 "Qwen4-27B" 같은 이름의 저장소가 보이지만 공식 조직이 아닌 커뮤니티 저장소다. Qwen 조직에는 창 안 갱신이 없다.

중요한 이유: 10월 내내 "qwen4" 문자열 단서만 돌았는데, 처음으로 공식 근거가 나왔다. 다만 가중치·벤치마크·종량제 API가 모두 없는 2주짜리 체험판이다. 제품에 넣을 모델이 아니라, 다음 정식판의 성격을 미리 보는 창으로 봐야 한다.

[X 게시](https://x.com/Alibaba_Qwen/status/2120335993643684315) · [모델 문서](https://docs.qwencloud.com/developer-guides/getting-started/latest-model) · [모델 변경 로그](https://docs.qwencloud.com/changelog/models) · [Token Plan](https://qwencloud.com/pricing/token-plan) · [HN](https://news.ycombinator.com/item?id=50449130)

### 2. Baidu가 ERNIE 5.5 Flash Preview를 냈다. Base 공개 뒤 첫 사후 학습판이고, 가중치는 11-25 전에 Apache 2.0으로 연다고 했다 (공식, 글 날짜 11-11, OpenRouter 등록 11-10 17:58 UTC)

11-06 판에서 다룬 ERNIE 5.5 Flash Base의 후속이다. 원문: "Today we are releasing ERNIE 5.5 Flash Preview, the first post-trained model built on the ERNIE 5.5 Flash Base we released earlier. It has 280B total parameters with 14B active and a 1M-token context". 공개 계획은 원문: "The API is now available on the Qianfan platform, and the weights will be open-sourced under Apache 2.0 soon." 날짜는 중국어판에만 있다. 원문: "权重将于11月25日前开源"(11-25 전에 가중치 공개). OpenRouter에는 `baidu/ernie-5.5-flash-preview:free`로 무료 등록됐다. Hugging Face `baidu/ERNIE-5.5-Flash-Preview`는 조회 시점에 401이다.

- **벤치마크 함정:** 자사 그림의 각주 원문: "* indicates self-evaluated scores." 경쟁 모델 점수 상당수를 Baidu가 직접 쟀다. 비교 대상은 Opus 5.5, GPT-6.1 Sol, Hy4, DeepSeek V4.1 Flash, GLM-5.3이고, Fable 5.2·Gemini 4 Carbon·GPT-6 Astra는 없다. 지는 항목도 있다. Terminal-Bench 4.0은 ERNIE 46.5, Opus 5.5 63.6(Baidu 측정)이다.
- **미완성 체크포인트:** 원문: "ERNIE 5.5 Flash Preview is an intermediate checkpoint from post-training." 원문: "because training is not finished, some capabilities may still fall short."
- 같은 창에 OpenRouter에 `baidu/ernie-light-dt-1.0-intl`도 올라왔다. 설명 원문: "ERNIE-Light-DT-1.0 is based on open-source GLM models".

중요한 이유: 11-06 판의 Base 라이선스는 사용 지역에서 한국을 뺐다. 이번 사후 학습판은 Apache 2.0을 약속했다. 약속대로 공개되면 한국에서도 쓸 수 있는 280B MoE가 된다. 11-25까지 실제 공개와 라이선스 원문을 확인한다.

[공식 글](https://ernie.baidu.com/release/ernie-5-5-flash-preview/) · [중국어판](https://ernie.baidu.com/zh/release/ernie-5-5-flash-preview/) · [OpenRouter](https://openrouter.ai/baidu/ernie-5.5-flash-preview:free)

### 3. Anthropic 공모: 가격은 11-18에 정하고 11-19에 거래를 시작한다. 청약이 공모량의 여러 배라는 보도가 나왔고, TSMC는 최대 5억 달러 매입을 결의했다 (공식 + 매체보도)

창 안 SEC 신규 접수는 없다. 최신은 직전 판에서 다룬 11-10 S-1/A 2차다.

- **공식 일정:** 공모 사이트 원문: "the final IPO share price is expected to be set on November 18th (EST), and shares are expected to begin publicly trading on November 19th (EST)." 직전 판의 "11-19 무렵(보도)"이 회사 일정으로 확인됐다.
- **수요(매체보도):** Bloomberg(13:17 UTC) 원문: "Anthropic PBC's initial public offering has drawn multiple times the orders for the shares available". 원문: "The demand was driven by so-called anchor-size indications of interest". FT는 가격을 범위 위로 올리는 방안을 보도했다(제목만 확인, 00:27 UTC). Fortune(13:59 UTC) 전재 원문: "Anthropic's bankers are considering pricing shares above the filed range, which could lift the valuation to $1.5 trillion or more, the Financial Times reported."
- **해외 개인 청약:** CNBC(01:26 UTC) 원문: "The offering opened at 10 a.m. Australian Eastern Daylight Time on Wednesday and is expected to close at 5 p.m. on Nov. 17". 호주 청약 개시는 UTC로 11-10 23:00이다.
- **TSMC 이사회 결의(공식, 대만 날짜 11-10, 시각 미확인):** 원문: "Approved the investments in (1) Anthropic, PBC at its per share initial public offering price in an amount not to exceed US$500 million to acquire Anthropic's common shares; and (2) OpenAI Group PBC in an amount not to exceed US$500 million to acquire OpenAI's preferred stock." 직전 판의 동시 사모 7곳 가운데 TSMC 몫이 처음 숫자로 나왔다. 같은 결의에 OpenAI 우선주 매입도 들어 있다.
- **창업자 매도(TechCrunch 19:57 UTC):** 공동창업자 7명이 각 3,378,378주를 판다. 범위 중간가 기준으로 각 약 2억5천만 달러다. 근거는 직전 판에서 다룬 S-1/A 2차 표다.

중요한 이유: 가격 확정까지 1주일이다. 범위 위 가격이 나오면 시가총액이 1.5조 달러를 넘는다. 칩 공급사 TSMC가 Anthropic과 OpenAI 양쪽에 같은 상한으로 투자를 결의한 것도 눈여겨볼 대목이다.

[공모 사이트](https://ipo.anthropic.com/us/) · [Bloomberg](https://www.bloomberg.com/news/articles/2026-11-11/anthropic-s-mega-ipo-is-said-to-be-multiple-times-oversubscribed) · [Fortune](https://fortune.com/2026/11/11/anthropic-sets-a-1-4-trillion-ipo-target-but-bankers-may-already-be-aiming-higher-cfo/) · [CNBC](https://www.cnbc.com/2026/11/11/anthopic-australian-retail-ipo-offering-.html) · [TSMC](https://pr.tsmc.com/english/news/3352) · [TechCrunch](https://techcrunch.com/2026/11/10/all-7-anthropic-founders-will-each-cash-out-about-250m-in-ipo/)

### 4. Google DeepMind가 AlphaGenome 2를 냈다. 비상업 연구는 API로 바로 쓸 수 있다 (공식, 11-11 15:00 UTC)

DNA 서열이 유전자 발현에 주는 영향을 예측하는 모델의 2세대다. 원문: "AlphaGenome 2 is available today for non-commercial research via the AlphaGenome API, and in private preview for commercial research." 세포 유형이나 상태를 입력으로 받는 조건부 구조로 바뀌었고, DNA 메틸화 예측이 추가됐다. 자사 측정 원문: "AlphaGenome 2 reliably identified nearly twice as many causal variants (30% vs. 16%)". 가중치 공개 언급은 없다.

중요한 이유: 질병 원인 변이 후보를 고르는 작업을 API로 바로 해 볼 수 있다. 다만 학습 세포 수가 본문에 "more than 100 million"과 "over 107 million" 두 가지로 섞여 있고, 성능 수치는 모두 자사 측정이다.

[DeepMind](https://deepmind.google/blog/alphagenome-2-deciphering-how-cells-read-dna-to-better-understand-disease/) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1031524/google-deepmind-alphagenome-2-genetic-ai-model)

### 5. OpenAI: 새 모델은 없다. ChatGPT의 Advanced·Standard 음성을 12-11에 은퇴시킨다 (공식, 릴리스 노트 날짜 11-10)

ChatGPT 릴리스 노트에 직전 판 조회(11-10 16시대 UTC) 때 없던 11-10 항목이 올라왔다. 원문: "We're retiring Advanced and Standard voice from ChatGPT Voice on December 11, 2026." 도움말 원문: "Users who previously selected these models will be defaulted to Live voice." 예외도 있다. 원문: "Live video and screen-sharing will continue to be powered by Advanced voice until GPT-Live has this capability."

- **API:** 변경 로그 최신은 여전히 11-05다. `gpt-6.2-astra`·`gpt-6.2-sol` 문서도 여전히 404다.
- **장애:** 11-06에 시작된 Mastercard 결제 장애가 11-10 16:06 UTC에 해결됐다. 창 안 새 인시던트는 없다.
- **실적(매체보도, WSJ 01:02 UTC, The Decoder 요약 12:12 UTC):** The Decoder 원문: "OpenAI grew its quarterly revenue about 50 percent to $10 billion but posted an adjusted operating loss of $10.9 billion." WSJ 본문은 읽지 못했다.
- **인수(매체보도, TechCrunch 21:08 UTC):** 여행 스타트업 Odessia를 인수했다. 조건은 공개되지 않았다.

중요한 이유: 음성 기능을 쓰는 앱·업무 흐름이 12-11 뒤에는 GPT-Live 기반 Live voice로 바뀐다. 말투와 지연이 달라질 수 있으니 음성 시나리오는 미리 시험해 둔다.

[릴리스 노트](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) · [은퇴 도움말](https://help.openai.com/en/articles/20001638) · [The Decoder](https://the-decoder.com/enterprise-sales-push-openais-quarterly-revenue-to-10-billion-though-it-still-trails-anthropic/) · [TechCrunch](https://techcrunch.com/2026/11/10/openai-acquires-odessia-travel-startup-created-by-sonder-founder/)

### 6. 짧게

- **(Gemini 4 Carbon 후속) Artificial Analysis 지수 4위(제3자, 조회 시점):** Carbon(High)은 60점으로 4위이고, 1위는 Fable 5.2(max) 65점이다. 원문: "somewhat expensive when comparing to other models of similar price. It's also notably slow per task and very verbose". AA는 도입가가 아니라 정가($4/$20)로 계산한다. OpenRouter에는 11-10 16:07 UTC에 $2/$10으로 등록됐다. HN에서는 앱에서 Ultra 요금제 전용이라는 불만이 나왔다. [Artificial Analysis](https://artificialanalysis.ai/models/gemini-4-carbon) · [HN](https://news.ycombinator.com/item?id=50442303)
- **(Fable 5.2 후속) 짧은 장애와 업체 측정:**
  - Claude 상태 페이지가 11-10 21:47 UTC에 사후 게시했다. 원문: "Overloaded errors on Fable 5.2 and Mythos 5.2 for four minutes." [상태 페이지](https://stspg.io/pytn9v8p2zc7)
  - 회계 SaaS 업체 DualEntry의 자사 벤치마크(11-11)다. 원문: "Fable 5.2 scored 81.2% accuracy, compared with 85.1% for Fable 5.1, a decline of 3.9 percentage points." 같은 101개 과제를 세 번 돌린 일관성도 76.2%로, Fable 5.1의 80.2%보다 낮다. 업체 측정이고 독립 검증은 없다. [DualEntry](https://www.dualentry.com/blog/claude-fable-5-2-scored-lower-than-fable-5-1)
- **Mythos가 전력 회사 보안 점검에 쓰인다(매체보도, Bloomberg 12:00 UTC):** Duke Energy 보안 책임자 발언으로 원문: "Anthropic enabled utilities to use its advanced AI tools late this summer". [Bloomberg](https://www.bloomberg.com/news/articles/2026-11-11/anthropic-s-mythos-ai-looking-for-cyber-flaws-in-energy-systems)
- **"Mythos가 AMD 이식 커널을 사흘에 썼다"(매체보도, The Decoder 14:25 UTC, 로드쇼 영상 인용):** 원문: "Anthropic's own frontier model Mythos wrote or ported nearly 100 percent of the kernels itself, reportedly in just three days instead of the usual six to twelve months." S-1/A 2차에는 이 문구가 없다. Simon Willison이 로드쇼 영상을 받아쓴 글(11-11 15:11 UTC)의 원문은 "This time, we had Mythos try it out. It got Claude running on AMD in just three days, with basically no human intervention."이다. 받아쓰기라 원본 녹취와는 대조하지 못했다. 직전 판의 시스템 카드는 공개판 5.2의 커널 최적화 능력을 Opus 5.5 수준으로 묶었다고 적었다. 사내 모델과 공개판의 능력 차이가 드러난 대목이다. 직전 판 기술 7번의 "커널 최적화 제한" 불만과 함께 본다. [The Decoder](https://the-decoder.com/anthropic-says-ai-is-eroding-nvidias-cuda-lead-cutting-an-amd-chip-switch-from-months-to-three-days/) · [Simon Willison](https://simonwillison.net/2026/Nov/11/anthropic-ipo-roadshow/)
- **Kimi K3.1 Pro Highspeed가 Amazon Bedrock에 올라왔다(공식, 11-11 01:07 UTC):** 원문: "Kimi K3.1 Pro Highspeed from Moonshot AI is now available on Amazon Bedrock." 속도는 원문: "Moonshot AI reports 2–3 times the speed of Kimi K3.1 Pro."(자사 측정). [AWS](https://aws.amazon.com/blogs/machine-learning/introducing-kimi-k3-1-pro-on-amazon-bedrock/)
- **Apple "Harbor" 유출(미확인, 9to5Mac 19:15 UTC, HN 420점대):** 유출 계정 인용 원문: "Harbor turns your Mac into a private AI inference server." Thunderbolt 5로 여러 Mac의 메모리를 묶는다는 설명이 함께 돌았다. Apple 공식 확인은 없다. [9to5Mac](https://9to5mac.com/2026/11/10/apple-has-new-mac-app-coming-to-make-running-local-ai-models-easy/) · [HN](https://news.ycombinator.com/item?id=50446520)
- **ByteDance가 Muse형 에이전트를 준비한다(매체보도, Bloomberg 10:10 UTC):** 원문: "ByteDance Ltd. plans the release of a Muse-like AI assistant in the coming weeks". [Bloomberg](https://www.bloomberg.com/news/articles/2026-11-11/bytedance-prepares-muse-like-agent-in-push-to-close-ai-gap)
- **미국 데이터 업체가 중국 랩에 RL 환경을 판다(조사 보도, ChinaTalk 11:07 UTC):** 원문: "American data companies like Mercor, Surge AI, AfterQuery, and Turing are selling roughly half a billion dollars worth of data to Chinese AI labs." [ChinaTalk](https://www.chinatalk.media/p/the-us-is-selling-rl-environments)
- **(NVIDIA Huang 건 후속, 공식):** 독립 이사들이 전 SDNY 검사장 대행 Joon H. Kim과 로펌 Wachtell을 선임해 조사한다. [NVIDIA](https://nvidianews.nvidia.com/news/statement-from-the-independent-members-of-the-nvidia-board)
- **(정정) 11-11 판의 `gpt-6-rosalind` 서술:** 11-11 판은 OpenAI Python SDK 3.48.0 항목에서 "OpenAI 발표는 없다"고 적었다. 틀렸다. API 변경 로그 10-30 항목이 원문: "Released GPT-6 Rosalind, bringing specialized biological reasoning with scientific tool use to complex life sciences research."이고, 11-02 판 모델 4번이 이미 다뤘다. SDK 3.48.0은 이 모델을 beta Responses에서 인식하게 한 것이다. [API 변경 로그](https://developers.openai.com/api/docs/changelog)
- **직전 판 후속 중 새 진전 없음(조회 11-11 16시대 UTC):**
  - Thinking Machines Inkling 2 Small 가중치: `thinkingmachines/Inkling-2-Small`은 여전히 401이고 조직 목록에도 없다. 공식 글은 여전히 "next week"다.
  - MiniMax-H3-Image: 라이선스가 11-11 00:30 UTC에 다시 커밋됐지만, 바뀐 것은 허가 요청 URL뿐이다. 한국을 포함한 제외 지역은 그대로다.
  - Microsoft 영국 Copilot 테넌트의 Claude 기본 켜짐: Microsoft 문서는 여전히 원문: "customers in the UK have Claude models disabled by default"이고, 12-09 일정 문구가 없다.
  - Anthropic 가스 터빈, Meta Muse Ember, Tencent Hy4 가중치, LG K-EXAONE 2.5 가중치, Meituan LongCat-2.5 공식 글, DeepSeek V5(변경 로그 10-17 그대로), Claude Platform 릴리스 노트(11-09 그대로), Fable 5.2 프로모션 도움말, OpenAI Pierre 인수, Apple Siri 에이전트.

### 참고: 창 경계 항목

- **NVIDIA GR00T 2(공식, HN 첫 제출 11-09 20:26 UTC, The Decoder 11-10 18:41 UTC):** 사람 영상을 보고 로봇 동작을 상상해 배우는 세계-행동 모델이다. 페이지 원문: "Code Available in December". 가중치는 아직 없다. 공개는 직전 창이고, 창 안에는 보도만 나왔다. [NVIDIA](https://research.nvidia.com/labs/gear/gr00t-2)
- **MBZUAI IFM PAN 2(Apache 2.0 세계 모델):** Hugging Face 첫 커밋은 11-05이고 공개 전환 시각은 확인하지 못했다. 보도는 11-11에 몰렸다. [Hugging Face](https://huggingface.co/IFM/PAN2)

## 기술 이슈

흐름은 셋이다. AI 앱 빌더의 공유 기본값 때문에 Grok Build 앱 3만 5천여 개가 로그인 없이 열렸다. Black October 후속으로, Notion이 "Fireworks가 데이터 무보존(ZDR)을 약정했지만 데이터가 남아 있었다"고 밝혔다. 에이전트가 남의 저장소를 다루는 경로에서 수정이 한꺼번에 나왔다. Git은 재귀 클론, Go는 cgo 빌드 단계의 코드 실행을 고쳤다. VS Code와 MCP 레퍼런스 서버의 수정은 도구 1·2번에 정리했다.

### 1. Grok Build로 만든 공개 앱 35,578개가 로그인 없이 열렸다. 기본 공유 설정이 "링크가 있는 누구나"였다 (보안업체 + 매체보도, Claroc 날짜만 11-10, Ars Technica 11-10 21:12 UTC)

Grok Build 앱은 `<이름>.grok.me` 호스트로 게시된다. 이 이름이 passive DNS에 남아 목록을 뽑을 수 있었다. Claroc 원문: "We tried 43,551 Grok app addresses. 37,571 were still online, and 35,578 of those (94.7%) opened for anyone, with no sign-in." 열린 앱에는 SpaceXSI 영업 자료, 연방기관 대상 광고 제안서, 환자 검사 결과 등이 있었다고 Claroc과 Ars가 전했다.

- **기본값:** 원문: "In our account, the panel that opens offered Anyone with the link, Invited users and Only you, with Anyone with the link already selected."
- **팀 로그인도 소속을 보지 않았다:** 원문: "In a team app Grok built for us, “Continue with Google” let any Google account read, edit and delete the records."
- **회사 반응:** Ars가 인용한 Cursor 대변인 원문: "apps are private by default and nothing goes public unless the creator chooses to share it." Ars 원문: "SpaceXSI began removing the links within a few hours of Ars sending an email with questions for this article." 다른 사용자의 앱은 그대로다. Claroc 원문: "It has not done the same for the apps other Grok users and organizations left open."
- **수치 차이:** Ars는 "43,601" 링크, Claroc은 시도한 주소 43,551개로 적었다.

중요한 이유: 10-21 판의 Meta Muse 공유 링크 건(GhostShares 1편)에 이은 두 번째 사례다. AI 앱 빌더로 만든 사내 도구는 게시 기본값과 팀 로그인 범위를 따로 확인해야 한다. 비밀 경로만으로도 부족하다. 원문: "Moving the secret into the path keeps it out of DNS, but search indexing and link scanners can still expose it."

창 밖이지만 같은 회사 건이다. Profero가 11-08에 Grok Bot 샌드박스의 정적 토큰과 비밀번호 없는 root 문제를 공개했다. 원문: "More than two months and more than 240 new sandbox builds later, nobody has replied and the findings still hold."

[Claroc](https://blog.claroc.io/ghostshares-grok-build-apps/) · [Ars Technica](https://arstechnica.com/security/2026/11/grok-build-coding-agent-leaks-oodles-of-confidential-info-some-belonging-to-spacexsi/) · [THN](https://thehackernews.com/2026/11/over-35000-grok-built-apps-exposed.html) · [Profero](https://profero.io/blog/the-rootkit-built-into-grok-bot)

### 2. (Black October 후속) Notion: "Fireworks는 ZDR을 계약으로 약정했지만, 우리가 쓴 엔드포인트에 데이터가 남아 있었다" (공식, 도움말 개정, 개정 시각 미확인)

직전 판이 다룬 Notion 도움말이 다시 고쳐졌다. 11-10 12:13 UTC Wayback 사본에 없던 ZDR 문단과 영향 구성원 목록 내려받기가 들어갔다. 직전 판 조회(11-10 16시대 UTC)도 옛 문구를 인용했으므로 창 안 개정으로 본다.

- **ZDR 문단:** 원문: "Fireworks provided us with a contractual commitment to zero data retention for our use of its platform. Unfortunately, during our investigation we learned that data was retained from the Fireworks endpoints that we used."
- **후속 조치:** 원문: "Looking forward, we are doing an audit of critical vendors — such as inference providers, and data processors — to verify that they abide by data minimization practices, including zero data retention policies."
- **고객 조치:** 모든 워크스페이스 소유자가 "Download affected members" CSV를 받을 수 있다. 직전 판 HN에는 이 목록을 Enterprise에만 준다는 불만이 있었다.
- **엇갈리는 지점:** Fireworks 공지("Last updated November 9", 창 안 변경 없음)는 원문: "We have not detected any impact to Inference, Training SDK and Nexus, which are all zero data retention (ZDR) by default."라고 적었다. Notion이 쓴 경로가 이 ZDR 기본값 밖이었는지는 두 회사 모두 밝히지 않았다.

중요한 이유: 추론 제공자의 "ZDR 약정"만으로는 데이터가 남지 않는다는 보장이 안 된다. 고객사가 공식 문서로 확인한 첫 사례다. 추론 공급사 계약을 맺는 쪽은 약정 문구가 어느 엔드포인트에 적용되는지까지 확인해야 한다. Fireworks는 영향 고객 수와 제3자 클라우드 이름을 여전히 밝히지 않았다.

[Notion 도움말](https://www.notion.com/help/security-incident-involving-a-third-party-ai-vendor) · [Notion 보안 공지](https://www.notion.com/security/advisories/security-incident-involving-a-third-party-ai-vendor) · [Fireworks](https://fireworks.ai/security/bulletins/october-2026-security-incident-update)

### 3. 에이전트가 남의 저장소를 클론·빌드하는 경로: Git 2.56.1은 재귀 클론 코드 실행을, Go 1.27.4·1.26.11은 cgo 빌드 단계 코드 실행을 고쳤다 (공식, Git 11-10 18:00 UTC, Go oss-sec 20:17 UTC)

- **Git:** GitHub 블로그 원문: "These include arbitrary code execution when recursively cloning a malicious repository, writes to unintended files, and memory corruption when processing merges or binary patches." 수정판은 2.56.1이고 2.50.2까지 백포트됐다. 번들 Git은 원문: "We are coordinating releases of GitHub Desktop and the GitHub Copilot app that include patched versions of Git."
- **Go:** oss-sec 공지 제목이 "Go 1.27.4 and Go 1.26.11 are released with 12 security fixes"다. 대부분 툴체인 문제다. GO-2026-6853 원문: "Building a malicious package containing ".syso" files or cgo linker inputs could inject arbitrary linker flags or execute arbitrary code at link time." 직전 판에서 "미국 화요일 예고"로 적은 릴리스가 창 안에 나왔다.
- **React Server Components(전역 GHSA 11-10 22:44 UTC, critical):** CVE-2026-126187이다. 수정판은 10-14에 이미 나왔고, 전역 DB에 올라 Dependabot 알림이 이제 뜬다. 원문: "This only affects extremely rare cases where responses transport multiple typed arrays and an attacker-controlled value." 영향은 `react-server-dom-webpack`·`-parcel`·`-turbopack`의 19.x 대부분이고, 수정은 19.0.9·19.1.10·19.2.9·19.3.1이다.

중요한 이유: 코딩 에이전트와 CI는 신뢰할 수 없는 저장소를 `--recursive`로 클론하고 `go build`를 돌린다. 둘 다 클론·빌드 단계에서 코드가 실행될 수 있었다. 에이전트 샌드박스 이미지에 번들된 Git·Go 버전을 확인한다. 같은 계열 cgo 플래그 이슈 다수는 Go 1.28 마일스톤에 열린 채 남아 있다.

[GitHub 블로그](https://github.blog/open-source/git/git-security-vulnerabilities-announced-7/) · [Go 공지](https://seclists.org/oss-sec/2026/q4/646) · [GO-2026-6853](https://vuln.go.dev/ID/GO-2026-6853.json) · [React GHSA](https://github.com/advisories/GHSA-xqc2-xf4c-x3h3)

### 4. Anthropic OSS Scanner가 Apache httpd mod_http2의 critical 원격 코드 실행을 찾았다. 창 직전에는 VulnCheck가 "Anthropic발 CVE 442건 중 실사용 악용은 2건"이라는 평가를 냈다 (공식, httpd 11-11 12:15 UTC / VulnCheck는 창 경계)

- **Apache HTTP Server 2.4.71:** CVE-2026-119618(critical)이다. 영향 범위 원문: "from 2.4.55 through 2.4.70". 발견자 원문: "Anthropic's OSS Scanner, as vulnerability ANT-2026-MAPKT6MZ". Google Cloud Vulnerability Research도 따로 찾았다. HTTP/2를 켠 httpd는 바로 올린다.
- **VulnCheck 평가(글 날짜 11-09, HN 11-10 22:17 UTC, 430점대):** 원문: "VulnCheck has tracked 442 CVEs attributable to Anthropic, roughly 0.5% of all CVEs published this year, with only two identified as exploited in the wild." 공개·수정 비율은 원문: "Of Anthropic’s 37,966 reported findings, only 7,349 (19.4%) have been made public and just 217 have been marked fixed." curl은 원문: "curl rejecting 19 of 34 published reports as vulnerabilities"였다고 집계했다. 같은 글 안에서 중복 건수가 1,654와 1,670으로 엇갈린다.

중요한 이유: "오탐이 많고 메인테이너 부담만 늘린다"는 비판과 "주요 웹서버의 critical 원격 코드 실행을 먼저 찾았다"는 사례가 거의 같은 때 나왔다. 둘 다 사실이다. 창 안 Anthropic 응답은 찾지 못했다.

[Apache httpd](https://httpd.apache.org/security/vulnerabilities_24.html) · [VulnCheck](https://www.vulncheck.com/blog/anthropic-glasswing-november-update) · [HN](https://news.ycombinator.com/item?id=50447074)

### 5. 공격자가 에이전트를 쓴다: AntiFox로 AWS 비밀값을 빼 갔고, AI 코딩 도구 설정 파일만 노리는 스캐너가 매주 돈다 (보안업체, Arctic Wolf 11-10 16:41 UTC, HoneyLabs 11-11 12:56 UTC)

- **Arctic Wolf:** 원문: "Alibaba Cloud systems hosting AntiFox interfaces were associated with confirmed malicious actions against internet-facing web services and associated AWS environments." 대부분 거부당한 요청 사이에서 끝내 원문: "Activity eventually progressed to a successful `GetSecretValue` request against a production secret." 열거 대상에는 Amazon Bedrock도 있었다. 탐지 조언 원문: "Do not assume a sequence was unsuccessful because most requests were denied."
- **HoneyLabs:** 원문: "For four days in September 2026 a credential scanner on Google Cloud asked every web server it reached for 32 AI coding assistant files, among them Claude Code's OAuth token file and MCP server definitions. Other scanners have asked for the same files every week since." 한계도 적었다. 원문: "we have no way to tell whether a real server ever answered them."

중요한 이유: 웹 루트가 홈 디렉터리나 체크아웃 폴더면 `.claude`·`.cursor` 아래의 토큰과 MCP 설정이 그대로 노출된다. 직전 몇 판에서 다룬 Claude Code 승인 파일 비밀값 건과 같은 경로다. 노출됐다면 그 안의 자격 증명을 모두 교체한다.

[Arctic Wolf](https://github.com/rtkwlf/wolf-tools/tree/main/pack_alerts/202611-antifox-alibaba-hosted-platforms-facilitate-web-cloud-intrusions) · [BleepingComputer](https://www.bleepingcomputer.com/news/security/hackers-use-antifox-ai-agents-in-aws-credential-theft-attacks/) · [HoneyLabs](https://honeylabs.net/blog/mass-scanners-ai-assistant-configs)

### 6. 안전 연구 3편: 학습 없이 뉴런 10개로 심는 백도어, 희석할수록 세지는 오염 데이터, GPT-6 Astra의 CoT 감시 가능성 저하 (공식 연구 블로그)

- **Microsoft Research "Lazarus"(11-10 20:52 UTC):** 원문: "weight-reprogramming fewer than 10 dormant neurons in an LLM, is enough to backdoor an agent built on it, and requires no training." 탐지는 원본과의 비교가 필요하다. 원문: "Reliable detection requires diff’ing against the original, unmodified weights, so provenance matters a lot". 시작 경위도 적었다. 원문: "I asked Claude Code with Opus 4.5 to backdoor a model by hardwiring a few activations without training. And it did."
- **OpenAI Alignment(날짜만 11-10):** 원문: "However, with GPT-6 Astra we observed degradation in CoT monitorability for non-adversarial settings, greater ability to perform CoT monitor evasion under adversarial testing, and significantly greater no-CoT capability compared to prior models." 원인 중 하나로 원문: "CoT controllability can be transmitted through SFT training data."를 들었다.
- **Anthropic Alignment "Homeopathic learning"(날짜만 11-10, arXiv 11-06 제출):** 원문: "poisoned training data with little effect on its own can create a hidden backdoor once clean, contrasting data is added." 원문: "This finding exposes a potential limitation of safety methods that audit or filter training data in isolation." Claude에서도 원문: "we were able to find minor behavioral issues in Claude by searching for contrasting training datasets, although we aren’t sure whether the contrast caused these issues."

중요한 이유: 세 편이 같은 방향을 가리킨다. 데이터나 가중치를 따로따로 감사하는 것으로는 부족하다. 오픈 웨이트를 받아 쓰는 쪽은 원본 가중치와의 대조, 즉 출처 확인이 기본 절차가 돼야 한다. 직전 판의 Fable 5.2 시스템 카드(추론을 읽는 감시 회피)에 이어, OpenAI도 자사 최신 모델의 CoT 감시가 약해졌다고 적었다.

[Microsoft Research](https://www.microsoft.com/en-us/research/blog/lazare-veni-foras-reprogramming-dormant-neurons-to-backdoor-ai-agents/) · [OpenAI Alignment](https://alignment.openai.com/cot-monitorability-model-lifecycle/) · [Anthropic Alignment](https://alignment.anthropic.com/2026/homeopathic-learning/)

### 7. AI 제품 보안 권고 묶음: MCP Gateway·Azure OpenAI(Patch Tuesday), Kiro CLI, Kong AI Gateway, PyTorch (공식)

- **Microsoft 11월 Patch Tuesday(11-10):** MCP Gateway CVE-2026-109492(Important). 원문: "An authenticated attacker could send specially crafted requests over the network to access another user's resources in an affected gateway." Azure OpenAI CVE-2026-119990(Critical)은 원문: "This vulnerability has already been fully mitigated by Microsoft. There is no action for users of this service to take." 전체 건수는 매체마다 1,093~1,259로 다르다.
- **Kiro CLI CVE-2026-124848(AWS 공지 11-10 20:45 UTC):** 원문: "Another local user might read a user's prompts, model responses and tool output, including any credentials or source code they contain." 범위는 원문: "Kiro CLI >= 1.16.0 AND < 2.27.0"이다. 공유 서버에서 쓰는 팀은 올린 뒤 한 번 실행해 파일 권한을 고친다.
- **Kong AI Gateway(미검토 GHSA, 11-10 21:34 ~ 11-11 00:32 UTC):** 원문: "SQL injection in the LLM usage counters of Kong Gateway Enterprise and Kong AI Gateway may allow a client to execute arbitrary SQL against the Kong database via a crafted model name." Azure 콘텐츠 안전 플러그인은 원문: "skips inspection of request bodies larger than the in-memory body buffer, allowing prompts to bypass input content-safety checks." 수정 버전 번호는 권고에 없다.
- **PyTorch(저장소 GHSA 11-10 16:45 UTC, high):** 원문: "A malicious PyTorch checkpoint can read text files from the computer loading it, even when it is loaded with `weights_only=True`." 범위 `>=2.14.0`, 수정 2.15.0(10-28 출시)이다. `weights_only=True`를 안전장치로 믿던 모델 로딩 코드가 해당한다.
- **auth0-ai-python(미검토 GHSA 11-10 21:34 UTC):** FGA retriever가 원문: "resulting in documents denied by Auth0 FGA being included in the model context"였다. 권한별로 문서를 거르는 RAG가 다른 사용자에게 거부된 문서를 섞었다.

[MSRC](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-109492) · [AWS](https://aws.amazon.com/security/security-bulletins/2026-178-aws/) · [Kong GHSA](https://github.com/advisories/GHSA-jvch-w6fx-vccj) · [PyTorch GHSA](https://github.com/pytorch/pytorch/security/advisories/GHSA-v32g-mr63-3j83) · [auth0 GHSA](https://github.com/advisories/GHSA-224p-7467-99w9)

### 8. 짧게

- **Canonical·Ubuntu 서비스 대규모 장애(공식 상태 페이지, 11-11 11:06 UTC~):** launchpad.net을 시작으로 PPA, 키서버, 클라우드 이미지, DNS가 차례로 "Major Outage"가 됐다. 조회 시점(16시대 UTC)에도 이 환경에서 ubuntu.com과 launchpad.net에 접속되지 않았다. 원인은 발표되지 않았다. Ubuntu 기반 CI 이미지 빌드가 PPA·키서버 단계에서 막힐 수 있다. [상태 RSS](https://status.canonical.com/rss)
- **뉴욕 RAISE Act 등록 포털(매체보도, TechCrunch 11-10 20:52 UTC):** 원문: "large frontier AI developers like Anthropic and OpenAI will be required to register by January 1, 2027". 원문: "report critical safety incidents within 72 hours". [TechCrunch](https://techcrunch.com/2026/11/10/openai-anthropic-face-new-oversight-as-new-yorks-ai-safety-law-takes-effect/)
- **역공학 차단 논쟁의 연장(매체보도, The Register 11-10 20:35 UTC):** 에이전트의 역공학을 방해하는 "Blind LLM Agents" 프로젝트다. 원문: "Classic obfuscation was built for humans and static tools, not LLMs that reason about meaning." 직전 판의 모델 측 역공학 차단과 반대 방향의 시도다. [The Register](https://www.theregister.com/ai-and-ml/2026/11/10/to-thwart-reverse-engineering-by-agents-dev-proposes-blinding-them/5308897)
- **"에이전트 48개로 미지의 마야 유적 후보를 찾았다"(커뮤니티, HN 11-11 12:05 UTC, 620점대, 창 안 HN 2위):** NASA 라이다 자료를 에이전트로 훑은 개인 글이다. 멕시코 INAH 회신 원문: "no registered archaeological site was identified at the location given in your report." 미등록이라는 뜻이고, 유적 확인은 아직이다. [글](https://pietromiotti.com/latest/maya-site/?maya) · [HN](https://news.ycombinator.com/item?id=50452455)
- **eBay가 서드파티 AI 에이전트를 막은 것으로 보인다(매체보도, The Verge 11-10 16:20 UTC):** 원문: "eBay appears to have blocked AI agents, including Meta's Muse, from browsing on the platform". [The Verge](https://www.theverge.com/tech/1030838/ebay-ai-agents-meta-muse-agentic-shopping)
- **Git이 LLM 기여 정책을 투표로 정하는 방안을 논의한다(LWN, HN 11-10 16:53 UTC):** 원문: "About the only consensus that, seemingly, was reached was that it would not be possible to come to a consensus on the topic." [LWN](https://lwn.net/SubscriberLink/1104195/1193ea577aaef643/)
- **측정(각 주체 발표, 독립 검증 전):**
  - Wiz Cyber Model Arena(11-10 16:03 UTC): 원문: "Averaged over the 59 agents, pass@1 is 78.0% on API security, 64.6% on web, 56.5% on cloud and 38.2% on code vulnerabilities." 표에 Fable·Mythos는 없다. [Wiz](https://www.wiz.io/blog/cyber-model-arena-ai-benchmark)
  - GitGuardian ARTEX 실험(11-10 20:57 UTC): 비밀값 탐지기를 빼고 배포된 ARTEX도 원문: "it found 8 of 9 planted secrets in seconds, even when asked only to assess the repo." [GitGuardian](https://blog.gitguardian.com/an-artex-build-ships-no-secret-scanner-in-our-lab-it-found-the-secrets-anyway/)
- **악성 패키지(malware GHSA):** npm `mcp-contract-tester`, `agent-trace-kit`, `agui-session-recorder`, `@lemili9804/mcp-weather-server`, `@rclaude/win32-x64`, `solvit-cli`, `0sec-cli`. MCP·에이전트 이름을 단 미끼가 계속 나온다.
- **CISA KEV(11-10 19:03 UTC) 6건 추가:** Outlook, Chrome CVE-2026-125350(직전 판 기술 8번), Barracuda 2건, Cisco BroadWorks, Openwave다. AI 전용 항목은 없다. GPUStack·OpenSSH sshole 기한은 11-12 그대로다.
- **GitHub 장애(공식, 11-10 16:10~17:55 UTC):** Git 작업·PR·Actions가 지연됐다. [상태](https://stspg.io/16nsnk2th3rv)
- **arXiv 수요일분(저자 주장):** 에이전트의 EDR 회피 능력을 재는 [BypassGym](https://arxiv.org/abs/2611.13684), 에이전트 게이트키퍼의 불가능성을 다룬 [2611.13367](https://arxiv.org/abs/2611.13367)(원문: "We prove that they cannot."), LLM 추론을 비트 단위로 증명하는 [Sable](https://arxiv.org/abs/2611.14083)(원문: "A proof costs 41 to 50 times the serving time")이 눈에 띈다. HF Daily Papers 11-11분에는 화제작이 없다.
- **직전 판 후속 중 새 진전 없음(조회 11-11 16시대 UTC):**
  - Fireworks(영향 고객 수 미공개), NEAR AI(SGLang 권고 없음), Railway(재보도만), Wiz Black October 목록, Qubole(회사 응답 없음).
  - Node.js 보안 릴리스: 11-11 16:06 UTC 기준으로 아직 나오지 않았다. [공지](https://nodejs.org/en/blog/vulnerability/november-2026-security-releases)는 원문: "Releases will be available on, or shortly after, Wednesday, November 11, 2026." 그대로다. patchtheplanet.org 새 항목도 없다.
  - Gemini 사람 검토 도움말 논란: Google 해명이나 문구 변경이 없다.
  - Fable 5.2 안전장치 불만: 창 안 Anthropic 응답을 찾지 못했다.
  - CVE-2026-98432 Ubuntu LTS 커널: Canonical 장애로 확인하지 못했다.
  - Claude Code 승인 파일 비밀값(이슈 #5544 새 댓글 없음), ChatGPT 데스크톱 SSH 에이전트, Arize Phoenix, hnswlib·Voyager, Open WebUI, Vercel AI SDK, AG2·fast-agent·autogen, Docker Desktop.
  - 저장소 권고 118곳: 창 안 새 권고는 VS Code 16, MCP 레퍼런스 서버 6, PyTorch 1뿐이다. vLLM·SGLang·LiteLLM·llama.cpp·Ollama·MCP SDK·Transformers·LangChain·n8n·Dify는 0건이다.

### 참고: 창 경계 항목

- **호주, AI 기업에 오정렬 행동·보안 사고 보고 의무화 등 권고 18개(정부 발표 11-10 05:22 UTC로 창 전, The Register 11-11 02:14 UTC):** 원문: "18 recommendations including one that would make it mandatory for AI companies to report misaligned behavior or security incidents involving their models". 알면서 위험한 AI를 판 경우에 대한 "failure to prevent" 형사 범죄 신설도 검토 대상이다. [The Register](https://www.theregister.com/public-sector/2026/11/11/australia-may-make-knowingly-selling-unsafe-ai-a-crime/5308999)
- **GA100 전압 글리칭(글 11-10 08:00 UTC, HN 11-11 10:48 UTC 150점대):** 원문: "I’ve discovered a voltage fault injection vulnerability on the GA100 die, allowing an operator to blow arbitrary fuses." 채굴 카드를 A100급으로 바꿨다는 주장이다. [글](https://blog.kate.land/posts/using-voltage-glitching-to-unlock-the-a100/) · [HN](https://news.ycombinator.com/item?id=50451903)

## 써볼 만한 도구

이번 창에는 MCP 클라이언트와 서버의 보안 수정이 몰렸다. VS Code는 권고 16건을 한꺼번에 공개했고, 그중 3건이 MCP를 거친 원격 코드 실행이다. MCP 레퍼런스 서버도 6건을 고쳤다. 기본값을 바꾼 메이저 릴리스(LiteLLM 1.109.0, DSPy 3.5, 창 경계의 browser-use 1.0)도 나왔다. 올리기 전에 기본값 변경 목록부터 확인한다.

### 1. VS Code 1.145.3: 보안 권고 16건을 고쳤고, 그중 3건이 MCP를 거친 원격 코드 실행이다. 1.146.0은 Local harness 지원 중단을 알렸다 (공식, 권고 11-10 18:19~19:37 UTC, 1.146.0 11-11 10:07 UTC)

1.145 릴리스 노트에 원문: "Update 1.145.3: The update addresses these security issues."가 붙었다. 저장소 권고 16건(high 14, medium 2)이 같은 시간대에 공개됐다. 16건 모두 영향 범위 `< 1.145.3`, 수정 1.145.3이다.

- **MCP 관련 3건(high):**
  - 설치 링크(GHSA-crhv-2887-p3xr): 원문: "a crafted MCP installation link could include unsafe input definitions or variable values that were not rejected before installation." 우회책은 신뢰할 수 없는 `vscode:mcp/install` 링크를 열지 않는 것이다.
  - legacy SSE 엔드포인트(GHSA-4x35-6qr8-ww48): 원문: "A remote MCP server could supply a new transport endpoint that VS Code used without sufficiently validating its origin and protocol".
  - elicitation 알림(GHSA-66hq-jw93-462h).
- **에이전트 메시지가 명령이 되던 문제(GHSA-w9fm-7vcm-cvq4, medium):** 원문: "Local-command syntax originating from an agent message could be treated as if it had been entered directly by the user and could execute without the expected terminal approval." 우회책 원문: "Use the Copilot harness instead of the Local harness where available."
- 나머지는 Workspace Trust 우회 5건, 확장 신뢰 대화상자 명령 주입, JavaScript 디버거 자동 연결 원격 코드 실행 등이다.

**1.146.0에서 바뀐 것:**

- **Local harness 지원 중단:** 원문: "As of this release, the Local harness is deprecated and will be removed in three months (February 2027)". 이유 원문: "For organizations, the Copilot harness also enforces managed sandbox and permission policies that the Local harness doesn't."
- **권한 프롬프트 세분화:** 이전 기본값의 문제를 스스로 적었다. 원문: "For example, the default action for fetching a URL was "Allow in this Session", which would let the model access any URL during that session."
- **샌드박스 비밀값 마스킹:** 원문: "Sandboxed tools receive placeholders, and the real values are supplied only to approved HTTPS destinations."
- **Skills over MCP 미리보기:** 원문: "This release of VS Code adds support for SEP-2640 Skills over MCP, allowing MCP servers to automatically provide skills to the model." 처음 연결할 때 허용 여부를 묻는다.

추천 이유: MCP 서버를 붙여 쓰는 VS Code 사용자는 바로 올릴 대상이다. 설치 링크 한 번 클릭이나 원격 MCP 서버 응답만으로 코드가 실행될 수 있었다.

함정: Local harness는 기능 업데이트 없이 동결되고 2027년 2월에 빠진다. Copilot CLI 세션을 VS Code로 넘기는 실험 기능 `/code`는 원문: "Permissions that apply only to the CLI process, such as --allow-tool or approvals you granted for the session, don't carry over". 넘긴 뒤에는 권한을 다시 확인한다.

[1.145 노트](https://code.visualstudio.com/updates/v1_145) · [1.146 노트](https://code.visualstudio.com/updates/v1_146) · [GHSA-crhv](https://github.com/microsoft/vscode/security/advisories/GHSA-crhv-2887-p3xr) · [GHSA-4x35](https://github.com/microsoft/vscode/security/advisories/GHSA-4x35-6qr8-ww48) · [GHSA-66hq](https://github.com/microsoft/vscode/security/advisories/GHSA-66hq-jw93-462h) · [GHSA-w9fm](https://github.com/microsoft/vscode/security/advisories/GHSA-w9fm-7vcm-cvq4)

### 2. MCP 레퍼런스 서버 v2.4.0: `mcp-server-git`의 읽기 전용 도구가 저장소 설정만으로 명령을 실행하던 문제 등 6건을 고쳤다. 날짜 버전 사용자는 자동으로 안 올라간다 (공식, 릴리스 11-11 07:07 UTC, 권고 07:24~07:28 UTC)

원문: "v2.4.0 fixes six vulnerabilities in three servers."

- **`mcp-server-git` 2026.11.11:** high 1건(GHSA-6hqx-gqrx-c5r2). 원문: "A read-only call such as `git_status` on an untrusted repository used to run a command named in its `.git/config`". 권고 제목은 `core.fsmonitor`를 지목한다. 이전 수정이 불완전했던 medium 1건(GHSA-q74p-4wrc-q7w6)도 함께 고쳤다. Git 2.31 미만이면 실행을 거부한다.
- **`@modelcontextprotocol/server-filesystem` 2.3.0:** 검증 뒤 심볼릭 링크를 바꿔치기하는 경쟁 조건 medium 2건이다. Windows는 원문: "On Windows this narrows the race without closing it"이다.
- **`mcp-server-fetch` 2026.11.11:** low 2건이다. IPv6 `fec0::/10`을 공개 주소로 본 SSRF 가드 문제, robots.txt 경로 정규화 우회다.
- **`server-everything` 2.3.0:** 원문: "bind the HTTP transports to loopback by default, with HOST to widen".

추천 이유: 레퍼런스 서버는 많은 MCP 서버가 베껴 쓰는 원형이다. git 서버를 낯선 저장소에 쓰는 에이전트라면 읽기 도구만 허용했어도 명령이 실행될 수 있었다.

함정: **filesystem 서버는 버전 체계가 바뀌어 자동 업그레이드가 안 된다.** 원문: "Package managers rank 2.3.0 below every 2025.x and 2026.x version, so they won't offer it as an upgrade." 권고상 날짜 버전 2025.1.14~2026.8.31의 수정판은 없다(`patched_versions: null`). `@modelcontextprotocol/server-filesystem@2.3.0`처럼 버전을 명시해 설치한다.

[릴리스](https://github.com/modelcontextprotocol/servers/releases/tag/v2.4.0) · [GHSA-6hqx](https://github.com/modelcontextprotocol/servers/security/advisories/GHSA-6hqx-gqrx-c5r2) · [GHSA-p9cv](https://github.com/modelcontextprotocol/servers/security/advisories/GHSA-p9cv-8g75-4xcg)

### 3. Claude Sheets·Whiteboards 베타와 Claude Data, 앱을 Claude 안에서 여는 interactive connectors가 나왔다. Claude Meetings는 아직 "coming soon"이다 (공식, 글 날짜 11-10, TechCrunch·The Verge 17:30 UTC, HN 260점대)

Anthropic이 비개발 업무용 도구를 한꺼번에 냈다. 개요 글 원문: "We're introducing new artifact types with Claude Sheets, Claude Whiteboards, and Claude Data." 원문: "Today we’re launching Claude Whiteboards and Claude Sheets in beta, and Claude Dashboards becomes generally available as Claude Data." 화이트보드와 스프레드시트를 Claude와 함께 편집하고, 10월 베타였던 Dashboards는 이름을 바꿔 정식 출시했다. 원문: "These are available on all paid plans."

- **interactive connectors:** 원문: "Interactive connectors run on MCP Apps, the open extension to the Model Context Protocol". 개요 글 원문: "Starting today, partners can embed their apps directly in Claude, so you can work in Adobe, Atlassian, and others without switching tabs." 개발자 쪽은 원문: "Support for these capabilities is in public preview for developers."
- **플러그인:** 원문: "Plugins from Salesforce (beta), NetSuite, and Workday (beta) come with built-in skills".
- **Claude Meetings:** 보도가 엇갈린다. TechCrunch는 원문: "Claude Meetings is launching in beta on Team and Enterprise plans. At launch, it works with Google Meet"라고 썼다. Anthropic 개요 글은 원문: "Claude Meetings is coming soon."이다. 공식 문구를 따라 아직 출시 전으로 본다.

추천 이유: MCP Apps 위에서 서드파티 앱 화면을 Claude 안에 띄우는 첫 대규모 적용이다. MCP 서버를 만드는 쪽은 UI까지 실어 보낼 수 있게 됐다.

함정(관리자): 원문: "On Team plans, all three are on by default. On Enterprise plans, Claude Whiteboards and Sheets are off by default, and Claude Data turns on by default on November 19, 2026." Enterprise 관리자는 11-19 전에 설정을 확인한다. 커넥터 도구를 전부 허용해 둔 조직은 원문: "If your organization already allows all of a connector's tools, its new features appear as they roll out." 새 기능이 따로 승인 없이 열린다.

[개요](https://claude.com/resources/articles/more-ways-to-work-with-claude-sheets-whiteboards-and-interactive-connectors) · [Sheets·Whiteboards·Data](https://claude.com/resources/articles/sheets-whiteboards-data) · [interactive connectors](https://claude.com/resources/articles/interactive-connectors-full-tool) · [TechCrunch](https://techcrunch.com/2026/11/10/anthropic-launches-claude-meetings-to-turn-workplace-discussions-into-plans-and-tasks/) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1030709/anthropic-has-its-own-ai-powered-office-suite-now) · [HN](https://news.ycombinator.com/item?id=50443716)

### 4. (업그레이드 주의) LiteLLM 1.109.0: MCP 서버로 호출자 키가 새던 문제 등 인증 수정이 들어갔다. 대신 Anthropic 기본 `max_tokens`가 4096에서 128000이 됐다 (공식, 11-11 00:56 UTC)

원문: "982 PRs. 144 added, 424 fixed, 60 changed, 17 improved, 9 removed. 5 need action".

- **보안 수정:**
  - 원문: "Fixed the caller's LiteLLM key being forwarded to upstream MCP servers when `Authorization` is in `extra_headers`"
  - 원문: "Fixed revoked or expired browser sessions still being accepted during gateway authorization"
  - 원문: "Fixed deleted keys staying usable on other workers until the auth cache TTL expired"
  - 원문: "Fixed `hide-secrets` missing secrets in tool results, tool inputs, `tool_calls` arguments, and system prompts"
- **조치가 필요한 변경:**
  - 원문: "Changed the default `max_tokens` for unmapped Anthropic models from 4096 to 128000". 모델 표에 없는 Anthropic 모델을 쓰면 응답 상한이 크게 늘어 비용이 달라진다. PR 원문: "The env var `DEFAULT_ANTHROPIC_CHAT_MAX_TOKENS` still overrides it".
  - 원문: "Removed the disk cache backend, so `cache_params.type: disk` now stops proxy startup". 디스크 캐시를 쓰던 프록시는 올리면 뜨지 않는다.
  - 원문: "Changed the Chat page to be on by default, and `enable_chat_ui: false` still hides it".

추천 이유: LiteLLM 앞에 MCP 서버를 둔 구성에서 사용자 키가 상류 서버로 넘어갔다. 삭제한 키가 다른 워커에서 계속 통하던 문제도 함께 고쳤다.

함정: 같은 창의 LTS 1.108.5는 원문: "5 PRs. 4 fixed. No action needed to upgrade"이다. 위 보안 수정이 LTS에 들어갔다는 표시는 릴리스 노트에서 찾지 못했다(조회 시점 기준). 저장소 권고도 아직 없다.

[1.109.0](https://github.com/BerriAI/litellm/releases/tag/v1.109.0) · [PR #48502](https://github.com/BerriAI/litellm/pull/48502) · [1.108.5 LTS](https://github.com/BerriAI/litellm/releases/tag/v1.108.5)

### 5. GitHub Copilot CLI 1.0.109: Linux 샌드박스 금지 경로와 정책 잠금 우회를 고쳤다. 대신 몇몇 권한 프롬프트가 줄었다 (공식, 11-10 17:02 UTC)

- **보안 수정:**
  - 원문: "Sandbox denied paths on Linux now block access to developer-tool configuration files inside those paths."
  - 원문: "A device Policy Helper's policy is applied again when a session without a GitHub sign-in is resumed, so settings it locks, such as repository hooks, stay locked"
  - 원문: "MCP servers disabled by repository settings now stop when you trust the folder, removing their tools from the session."
- **(직전 판 후속) 인덱스 비대:** 원문: "Indexed search now deletes the per-repository indexes it left in the cache for repositories and worktrees that no longer exist, including those written before version 1.0.108, which could grow to tens of gigabytes".
- 모델 선택기에 Fable 5.2가 추가됐다.

함정: 권한 쪽은 느슨해진 변경도 있다. 원문: "Read-only shell scripts ask for permission less often, while scripts that change git refs or read files through hidden paths now always get reviewed". 원문: "URL permission prompts let you approve every subdomain for the current session". 프리릴리스 1.0.110-0은 원문: "copilot -p in a trusted folder no longer sometimes runs without the repository's hooks when plugins finish loading at the same time"를 고쳤다. hook으로 통제하는 `-p` 자동화는 1.0.110 정식판을 기다린다.

`npm i -g @github/copilot@1.0.109` · [릴리스](https://github.com/github/copilot-cli/releases/tag/v1.0.109) · [1.0.110-0](https://github.com/github/copilot-cli/releases/tag/v1.0.110-0)

### 6. Qwen Code: 1.0 Preview가 Qoder와 합쳐 네이티브 패키지로 나왔다. 0.27.2는 셸 권한 파싱을 고쳤지만, ask 규칙이 건너뛰어지던 문제는 아직 main에만 고쳐졌다 (공식, 1.0 Preview 11-11 03:55 UTC, 0.27.2 11-10 19:22 UTC)

- **1.0 Preview:** 원문: "Brings together the agent capabilities of Qoder and Qwen Code for code understanding, implementation, debugging, and testing." 원문: "Provides native packages for macOS, Linux, and Windows without requiring a separate Node.js installation." 별도 브랜치이고 npm에는 올라가지 않았다. 원문: "Some interfaces, configuration options, and behaviors may change during Preview."
- **0.27.2:** 원문: "Fixes shell command parsing so the self-kill guard and file-permission checks correctly handle ANSI-C escapes and -c option terminators."
- **미출시 수정(PR #16932, 머지 11-11 15:24 UTC):** auto-edit 모드에서 원문: "For edits, and for reads or searches on a protected path, the rule was skipped with no prompt." 기본 모드 사용자도 해당한다. 원문: "A subagent whose definition sets no mode runs in auto-edit when the session is in default (Ask Permissions) mode in a trusted folder".

함정: 다음 릴리스 전까지는 ask 규칙만 믿지 말고, 보호해야 할 경로는 deny 규칙으로 막는다.

[1.0 Preview](https://github.com/QwenLM/qwen-code/releases/tag/v1.0.0-preview) · [0.27.2](https://github.com/QwenLM/qwen-code/releases/tag/v0.27.2) · [PR #16932](https://github.com/QwenLM/qwen-code/pull/16932)

### 7. DSPy 3.5: 프롬프트·기본값·API가 바뀌었고, LiteLLM 의존이 선택 사항이 됐다 (공식, 11-10 16:25 UTC)

원문: "This release changes prompts, defaults and APIs." 원문: "LiteLLM and OpenAI's Python library are now optional". `ReActV2`는 원문: "ReActV2 is now simply `dspy.ReAct`"이고, 원문: "`model_type="auto"` is the default"이다.

추천 이유: 의존성이 가벼워졌다. LiteLLM 공급망 사고를 겪은 팀이라면 의존을 줄일 기회다.

함정: 최적화해 둔 프로그램은 프롬프트가 바뀌니 다시 평가해야 한다. 원문: "re-evaluate optimized programs".

[릴리스](https://github.com/stanfordnlp/dspy/releases/tag/3.5.0) · [업그레이드 가이드](https://dspy.ai/current/community/upgrading-to-3-5/)

### 짧게

- **OpenAI "Businesses in ChatGPT"(공식, RSS 날짜 11-10, The Verge 18:00 UTC):** 기업이 Business Hub에서 프로필을 등록·검증하고, 미국의 적격 전자상거래·온라인 서비스 업체는 고객 질문에 답하는 "Business Agents"를 만든다. The Verge 원문: "available first to “eligible” e-commerce and online service businesses in the US". 베타다. [OpenAI](https://openai.com/index/introducing-businesses-chatgpt/) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1030552/openai-chatgpt-business-agents-hub) · [TechCrunch](https://techcrunch.com/2026/11/10/openai-rolls-out-new-business-profiles-making-chatgpt-just-a-little-bit-more-like-google/)
- **goose 1.59.0(공식, 11-10 17:17 UTC):** 프롬프트 주입 탐지 문서가 반대로 쓰여 있었다. PR 원문: "example comment for `SECURITY_PROMPT_THRESHOLD=0.9` changed from "stricter" to "more lenient than default"". 0.9로 엄격하게 했다고 생각했다면 실제로는 기본보다 느슨하다. [PR #12954](https://github.com/aaif-goose/goose/pull/12954)
- **FastMCP 4.1.8(공식, 11-10 20:44 UTC):** PR 원문: "That mismatch makes an owner's access-token revocation a silent no-op." OAuth 프록시에서 토큰 폐기가 실제로는 되지 않았다. 권고는 없다. [릴리스](https://github.com/PrefectHQ/fastmcp/releases/tag/v4.1.8) · [PR #6404](https://github.com/PrefectHQ/fastmcp/pull/6404)
- **Gemini CLI 0.68.0(공식, 11-10 21:17 UTC):** Windows에서 대소문자가 다른 경로가 원문: "bypassing `.gitignore` and `.geminiignore` rules for paths with mismatched casing"이던 문제를 고쳤다. [릴리스](https://github.com/google-gemini/gemini-cli/releases/tag/v0.68.0) · [PR #29999](https://github.com/google-gemini/gemini-cli/pull/29999)
- **Mistral Vibe 3.1.2(공식, 11-10 16:56 UTC):** 원문: "Headless --max-tokens and --max-price limits now stop the run before another root or subagent completion starts." 비용 상한이 늦게 걸리던 문제다. [릴리스](https://github.com/mistralai/mistral-vibe/releases/tag/v3.1.2)
- **Chrome DevTools MCP 1.13.0(공식, 11-11 12:40 UTC):** URL 검사 우회를 고쳤다. 원문: "unwrap view-source: before all scheme checks in validateUrl". 로그포인트와 페이지 단위 네트워크 목이 추가됐다. [릴리스](https://github.com/ChromeDevTools/chrome-devtools-mcp/releases/tag/chrome-devtools-mcp-v1.13.0)
- **Gemini 4 Carbon이 GitHub Copilot에 들어갔다(공식, 11-10 19:26 UTC):** 원문: "Under default model enablement, new models are automatically enabled unless an administrator has turned off the global default". 사용량 과금은 원문: "billed at provider list pricing under usage-based billing." 도입가가 아니라 정가 기준이다. [GitHub 변경 로그](https://github.blog/changelog/2026-11-10-gemini-4-carbon-in-github-copilot)
- **Codex CLI 0.167.1(공식, 11-10 21:32 UTC, npm `latest`):** 릴리스 본문이 비어 있다. 비교 커밋은 Bedrock 전용 옵트인 하나다. [릴리스](https://github.com/openai/codex/releases/tag/rust-v0.167.1)
- **pydantic-ai 2.67.0(공식, 11-10 23:11 UTC):** 호환성 노트 원문: "Load one instruction file per directory and nested files by default in `RepoContext`". 하위 디렉터리의 지시 파일이 기본으로 읽힌다. [릴리스](https://github.com/pydantic/pydantic-ai/releases/tag/v2.67.0)
- **Cursor Projects 멀티플레이어(공식, 날짜만 11-10):** 원문: "It's rolling out in beta to all users in Cursor starting today." [Cursor](https://cursor.com/blog/projects-multiplayer)
- **Val Town 종료(공식, 블로그 날짜 11-10, HN 410점대):** 읽기 전용 전환이 2026-12-10, 나머지 서비스 종료가 2027-01-10이다. 여기에 MCP 서버나 봇을 올려 두었다면 옮긴다. [Val Town](https://blog.val.town/winding-down) · [HN](https://news.ycombinator.com/item?id=50443427)
- **직전 판 후속:**
  - Claude Code: 창 안 릴리스가 없다. npm `latest`·`next`는 2.1.322, `stable`은 2.1.311이다. 이슈 #108726(`DISABLE_TELEMETRY`를 켜면 모델 선택기에서 Fable 5.2가 안 보임)에는 직원 답이 없다. 사용자 댓글이 VS Code·VSCodium이 텔레메트리를 끄면 이 변수를 스스로 켠다고 보고했다. 위의 Modern Web Guidance(창 경계)가 권하는 `DISABLE_TELEMETRY=1`을 셸 전역에 넣어도 같은 증상이 날 수 있다(해석).
  - Claude Agent SDK Python: PyPI 0.2.180 그대로라 PR #1426의 수정은 아직 배포되지 않았다.
  - Pi: 새 릴리스가 없고 캐시 유지 기본값도 그대로다. 오히려 [main 커밋](https://github.com/earendil-works/pi/commit/49fe0a2) 원문: "Any provider with a baseUrl on api.anthropic.com now uses message threads by default."로 적용 범위가 넓어졌다(미출시).
  - Jules: [공지](https://jules.google/docs/changelog/) 원문 "After November 11, 2026, we will be turning Jules to read-only until the end of the year."대로라면 전환은 창 이후다. Conductor는 11-13 읽기 전용 일정 그대로다.
  - 새 진전 없음: OpenCode 2.2.7, Ollama(0.40.17-rc0 프리릴리스만), Playwright MCP 0.0.85, Open WebUI 0.13.3, MongoDB MCP 3.3.0, GitHub MCP 2.1.0, Supabase MCP(0.17.0 미배포), MCP Toolbox 1.16.0, Sentry MCP 0.49.0, Context7 MCP 4.6.0, Dify 1.19.0, ADK Python 2.14.0, devvm, Amp, Neon MCP, `anthropics/skills`·`openai/plugins`(창 안 커밋 없음), 공식 Claude 마켓플레이스(신규 플러그인 없음).

### 참고: 창 경계 항목

- **(업그레이드 주의) browser-use 1.0(1.0.0은 11-10 02:04 UTC로 창 전, 1.0.1 PyPI 16:50 UTC, 1.0.2 11-11 04:00 UTC):** 직전 판이 놓쳤다. 원문: "80% of the library deleted." 기본값이 바뀌었다. 원문: "`Browser()` is a cloud browser. Tasks on localhost or private IPs need `Browser('chromium')`". 기본 모델도 Browser Use 게이트웨이다. 원문: "`run(max_steps=...)` defaults to 100 (was 500)." 0.x 코드를 그대로 올리면 브라우저와 모델 호출이 과금되는 클라우드로 간다(해석). 1.0.0에서 `--mcp`와 `allowed_domains`를 함께 썼다면 1.0.1 이상으로 올린다. 1.0.1 원문: "**`--mcp` with `allowed_domains`** can no longer be bypassed through a cloud browser." [1.0.0](https://github.com/browser-use/browser-use/releases/tag/1.0.0) · [1.0.1](https://github.com/browser-use/browser-use/releases/tag/1.0.1)
- **Modern Web Guidance 1.0(GitHub 11-10 03:24 UTC로 창 전, Show HN 18:01 UTC 410점대):** Chrome·Edge·Firefox·MDN 팀이 관리하는 웹 플랫폼 스킬이다. README 원문: "Stewarded by the Google Chrome, Microsoft Edge, Mozilla Firefox, and MDN teams, alongside the web development community." Show HN 본문 원문: "Agents go from 55% pass unguided to 92% guided."(자사 측정). 설치는 `npx modern-web-guidance@latest install`이다. 함정은 텔레메트리가 기본으로 켜져 있다는 것이다. README 원문: "Collected information includes installation counts, the calling agent name, guide retrieval IDs, and CLI tool search queries generated by the agent". 끄려면 `DISABLE_TELEMETRY=1`을 쓴다(위 Claude Code 후속 참고). [저장소](https://github.com/GoogleChrome/modern-web-guidance) · [HN](https://news.ycombinator.com/item?id=50443812)

## 주목할 점

- **MCP 자체가 공격 표면이 됐다.** 같은 날 클라이언트(VS Code), 서버(레퍼런스 서버), 게이트웨이(Microsoft MCP Gateway, LiteLLM, Kong)가 모두 수정판을 냈다. 동시에 MCP 서버가 넘겨주는 것의 범위는 커지고 있다. Anthropic은 MCP Apps로 서드파티 앱 화면을 Claude 안에 띄우고, VS Code는 Skills over MCP로 서버가 모델 컨텍스트에 스킬을 넣게 했다. 넘겨받는 범위가 넓어질수록 "도구 전체 허용" 같은 승인 기본값을 다시 봐야 한다. Notion 건은 추론 공급사의 ZDR 약정이 실제로 어느 경로에 적용되는지 계약서로 확인해야 한다는 점을 보여 줬다.
- **다가오는 일정:**
  - 이번 주: Node.js 보안 릴리스(11-11 예고, 창 안 미출시), Conductor 읽기 전용 전환(11-13), Fable 5.2 출시 프로모션 종료와 Anthropic 텍스트 워터마크(11-16), Anthropic 호주 개인 청약 마감(11-17).
  - 다음 주: Anthropic 공모가 확정(11-18)과 거래 시작(11-19), Enterprise의 Claude Data 기본 켜짐(11-19), ERNIE 5.5 Flash Preview 가중치 공개 약속(11-25 전), Qwen4-Max-Preview 종료(11-25 16:00 UTC).
  - 그 뒤: Gemini 4 Carbon 도입가 종료(11-30), Dependabot 비활성 저장소 알림 중지(12-08), 영국 Copilot 테넌트 Claude 기본 켜짐(12-09, 보도 기준), Val Town 읽기 전용(12-10), ChatGPT 옛 음성 모델 은퇴(12-11).
  - Inkling 2 Small 가중치는 "next week" 약속 주간이 이어지고 있다.

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - WSJ(OpenAI 실적)는 캡차, FT(공모가 상향 검토)는 유료벽이라 각각 The Decoder 요약과 Fortune 전재로 대신했다. Bloomberg 3건은 요약부만 읽었다.
  - THN, BleepingComputer 일부, Dark Reading, science.org는 자동화 접근에 Cloudflare 확인 페이지를 줬다. THN은 RSS로 URL·시각만 확인했다.
  - MSRC 업데이트 가이드는 JS 렌더라 CVRF API(`api.msrc.microsoft.com`)로 본문을 대조했다.
  - ubuntu.com·launchpad.net은 Canonical 장애로 DNS가 실패해 Ubuntu 커널 CVE 후속을 확인하지 못했다.
  - Wayback CDX가 조회 시점에 응답하지 않아 Anthropic 발표 페이지(사이트맵 갱신 11-10 21:27 UTC)의 창 안 수정 여부를 diff로 보지 못했다. Notion 도움말 대조는 기존 11-10 12:13 UTC 사본으로 했다.
  - x.com은 개별 게시물만 `api.fxtwitter.com`으로 읽었다. 타임라인을 볼 수 없어 회사 직원 반응을 놓쳤을 수 있다. reddit은 막혀 있다.
- **시각이 날짜만 있거나 불확실한 원문:**
  - ERNIE 5.5 Flash Preview 글(11-11)은 OpenRouter 등록 시각으로, TSMC 이사회 결의(대만 날짜 11-10)는 시각 없이 실었다.
  - ChatGPT 릴리스 노트 11-10 항목은 직전 판 조회 기록과 대조해 창 안으로 판정했다. OpenAI 뉴스 RSS는 날짜만 신뢰한다.
  - Claude 글 3편은 TechCrunch·The Verge 17:30 UTC로, Claroc·VulnCheck·Profero·Anthropic·OpenAI Alignment 글, Cursor·Val Town 블로그는 날짜만 있다. Notion 도움말 개정 시각은 알 수 없다.
- **확인하지 못한 것:**
  - Qwen4-Max-Preview의 벤치마크와 정가, ERNIE Preview의 Qianfan 가격, Anthropic 로드쇼 영상 원본(받아쓴 글로만 대조).
  - Claude Meetings 출시 상태(공식 글 "coming soon"과 TechCrunch "launching in beta"가 다르다), Notion 데이터가 Fireworks ZDR 기본 경로 밖이었는지, Kong 수정 버전 번호, Canonical 장애 원인.
  - LiteLLM 1.109.0의 보안 수정이 LTS에 들어갔는지(릴리스 노트에 표시 없음).
  - 재현하지 않은 것: 모든 도구의 동작 주장. 도구는 하나도 설치하거나 실행하지 않았다. Modern Web Guidance 타르볼만 빈 디렉터리에 풀어 grep했다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 스토리 약 1,175개 중 점수순 상위 150개는 전부, 나머지는 키워드로 훑었다. Show HN 178건은 전부 봤다.
  - 전역 GHSA: reviewed 74건은 전부 봤다. unreviewed 1,944건(Adobe·Microsoft·커널 등 대량)은 키워드로 걸렀고, malware 75건은 목록을 전부 봤다.
  - arXiv 수요일분은 제목 키워드로만 훑었다. Hugging Face는 주요 조직 약 80곳의 `lastModified`와 개별 저장소 ID로 확인했다. 비공개에서 공개로 바뀐 저장소는 놓쳤을 수 있다.
  - Vertex AI·Gemini Enterprise 릴리스 노트(캐시 동결), Mistral 변경 로그, Volcengine·Moonshot 월 단위 변경 로그는 일 단위로 판정할 수 없었다.
