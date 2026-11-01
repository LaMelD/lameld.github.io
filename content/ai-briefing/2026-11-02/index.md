---
title: "2026-11-02 AI 브리핑"
date: 2026-11-02T01:00:00+09:00
tags: [ai-briefing, openai, anthropic, security, pricing]
description: "OpenAI는 12월부터 ChatGPT와 Codex 사용량을 토큰 하나로 합치고 Anthropic은 Max 20x를 Ultra로 바꿔 5시간 한도를 없앴으며, Wiz는 10월 한 달에 AI 개발 플랫폼 9곳이 뚫려 탈취된 키가 30분 만에 재판매됐다고 정리했다."
---

> 조사 범위: 2026-10-31 01:00 ~ 2026-11-02 01:00 KST(2026-10-30 16:00 ~ 11-01 16:00 UTC, 미국 금요일 09:00 PDT ~ 일요일 08:00 PST). 11-01 판이 빠져 직전 브리핑(10-31 판)의 조사 창 종료 시각부터 48시간을 잡았다. 미국 서머타임은 11-01(일)에 끝났다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`·저장소 `created_at`, 전역 GHSA `published_at`, Hugging Face `createdAt`, statuspage 기록, 트윗 스노우플레이크 ID(fxtwitter), HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-01 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **정액 요금제가 토큰 예산제로 바뀌고 있다.** OpenAI는 12월부터 Plus·Pro·Business의 ChatGPT와 Codex 사용량을 하나로 합쳐 토큰으로 센다. Anthropic은 Max 20x를 Ultra로 이름을 바꾸고, 인증된 Ultra 계정의 5시간 한도를 없앴다(주간 한도는 그대로). 같은 주말 자금 사정은 나빠졌다. OpenAI는 이번 라운드 목표를 $30B에서 $20B로 낮췄고, Anthropic은 IPO 뒤 투기 등급을 받을 것이라는 전망이 보도됐다.
- **Wiz가 "Black October"를 정리했다.** 10월 한 달에 AI 개발 플랫폼 9곳이 침해됐고, 탈취된 키는 침해 30분 안에 시험 호출을 받은 뒤 재판매 사이트로 넘어갔다. 두 고객 사례만으로 정가 기준 $100k 이상이 쓰였다. 직전 판의 Cursor 가짜 Claude Code 확장은 공지가 고쳐져 2.1.312도 악성 버전에 들어갔고, "최대 5,000명" 문장은 삭제됐다.
- **도구:** Claude Code 2.1.316은 게이트웨이가 차단 모델 요청을 통과시키던 문제를 고쳤고, Opus 4.8 fast mode를 11-30에 없앤다고 예고했다. DeepSeek은 GitHub 이벤트를 듣고 스스로 PR을 여는 상시 가동 코딩 에이전트 프로토타입을 공개했다(공식 제품은 아니다).

## 모델 소식

금요일 오전부터 일요일 오전까지의 주말 창이었다. 새 프런티어 모델은 어느 랩에서도 나오지 않았다. OpenRouter 신규 등록은 0건이고, Hugging Face 주요 조직의 창 안 업로드는 NVIDIA의 DeepSeek 양자화본 1건뿐이었다. 대신 요금제 개편과 자금 조달 뉴스가 이어졌다.

### 1. OpenAI: 12월부터 Plus·Pro·Business의 ChatGPT와 Codex 사용량을 하나로 합쳐 토큰으로 센다

- **무엇이 바뀌나(도움말 원문):**
  - "Starting in December, Plus, Pro, and Business plans' usage allowance will be shared across ChatGPT and Codex as we bring the agentic capabilities of Work into the main ChatGPT experience."
  - 사용량은 토큰으로 센다. 무거운 모델과 복잡한 작업일수록 많이 쓴다. "Instant chat will not draw from your usage."
  - 원문: "We expect most users won't need to wait for their usage to reset more often than they do now." 남은 사용량은 Settings → Usage에서 본다.
  - 한도에 닿으면 리셋을 기다리거나, 요금제를 올리거나, 크레딧을 사야 한다.
- **같은 창에 함께 나온 것:**
  - **카드·기기 제한 도움말:** 같은 카드나 기기에 구독이 너무 많이 묶이면 새 구매가 막히고, 로그인 때 "Device limit reached"가 뜬다. 대상은 Go, Plus, Pro 100·200·500이다. Free와 Business·Enterprise·Edu는 세지 않는다. 원문: "Buying credits or resetting usage won't clear a card or device restriction." 게시일은 "Updated: 2 days ago"로만 적혀 있다.
  - **계정 전환(ChatGPT 릴리스 노트 10-30):** iOS·Android 앱에서 최대 2개 계정에 동시에 로그인해 전환할 수 있다.
- **예고된 일이었다:** Codex 리드 Tibo는 10-28 23:35 UTC에 "Plan is to bring together best of both Chat / Work capabilities together at some point this year"라고 썼다. The Decoder는 직전 판의 Pro 사용량 2배 조치를 이 통합의 준비로 읽었다(매체 해석).

중요한 이유: 메시지 수로 체감하던 정액제가 사실상 토큰 예산제로 바뀐다. 채팅과 Codex 에이전트 작업이 같은 예산을 나눠 쓰니, Codex를 많이 돌리는 사용자는 12월부터 채팅 여유가 줄 수 있다. 계정 여러 개를 돌려 쓰는 방식도 같은 시기에 막혔다.

[사용량 통합 도움말](https://help.openai.com/en/articles/20001625-understanding-chatgpt-and-codex-usage-limits) (공식, 날짜 없음. HN 첫 게시 10-31 21:49 UTC) · [카드·기기 제한](https://help.openai.com/en/articles/20001559-understanding-card-and-device-limits-in-chatgpt) · [ChatGPT 릴리스 노트](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) · [The Decoder(통합)](https://the-decoder.com/openais-upcoming-chat-and-work-merger-in-chatgpt-signals-the-end-of-all-you-can-prompt-pricing/) · [The Decoder(기기 제한)](https://the-decoder.com/openai-is-steering-chatgpt-users-toward-more-paid-usage/) · [Tibo 게시물](https://x.com/thsottiaux/status/2115588390414581916) · [HN](https://news.ycombinator.com/item?id=50320588) (200점)

### 2. Anthropic: Max 5x는 Max로, Max 20x는 Ultra로 이름을 바꿨다. 인증된 Ultra 계정은 5시간 한도가 없어진다

- **@claudeai 원문(10-30 17:29 UTC):**
  - "We're renaming our Max plans: Max 5x → Max / Max 20x → Ultra … Max is still $100/month, and Ultra is still $200."
  - "We're also removing the 5-hour limit for verified Ultra accounts, starting today with most existing subscribers." 새 계정은 인증 전까지 임시 한도가 남는다.
  - "Weekly limits remain unchanged."
- **가격 페이지 비교(Wayback 10-30 03:15 UTC 판과 현재 판):**
  - 이전: "Choose 5x or 20x more usage than Pro", FAQ는 "5x or 20x the usage of Pro per 5-hour session".
  - 현재: Max는 "5x Pro usage every 5 hours and every week". Ultra는 "No 5-hour limit"와 "10x Pro usage every week"다.
  - 월 API 크레딧은 Max $100, Ultra $200이다(현재 판 원문: "up from $100 on Max"). Fable을 주간 한도의 50%까지 쓰는 조건은 이전 판에도 있었다.
- **해석 주의:** HN에는 "20x가 10x로 줄었다"는 글(7점)이 올라왔다. 하지만 예전 20x는 5시간 세션 기준이고, 새 10x는 주간 기준이다. Anthropic은 주간 한도가 그대로라고 했다. 예전 주간 배수가 공개된 적이 있는지는 확인하지 못했다.

중요한 이유: 최상위 개인 요금제의 병목이던 5시간 창이 사라졌다. 긴 에이전트 작업을 밤새 돌리는 사용자에게 가장 직접적인 변화다. OpenAI가 Pro 사용량을 2배로 늘린 지 하루 만에 나온 정액제 경쟁이다.

[@claudeai](https://x.com/claudeai/status/2116220869479428500) (공식, 10-30 17:29 UTC) · [가격](https://claude.com/pricing) · [Ultra 도움말](https://support.claude.com/en/articles/17728952-what-is-the-ultra-plan) · [이전 가격 페이지(Wayback)](https://web.archive.org/web/20261030031525/https://claude.com/pricing) · [HN](https://news.ycombinator.com/item?id=50308320) (15점)

### 3. 자금: OpenAI는 이번 라운드 목표를 $30B에서 $20B로 낮췄고, Anthropic은 IPO 뒤 투기 등급을 받을 것이라는 전망이 나왔다

- **OpenAI(매체보도):**
  - FT 보도를 CNBC가 확인했다(10-31 19:56 UTC). 원문: "OpenAI is seeking to raise up to $20 billion in a new funding round, CNBC confirmed on Saturday, paring back its earlier target of up to $30 billion."
  - FT에 따르면 post-money 기업가치는 $1.25T다. 금요일을 약정 마감으로 정했고, 일부 투자자는 배정을 줄였다. UAE 투자자가 Coatue와 함께 라운드를 이끈다.
  - CNBC는 라운드가 "still in progress and has not closed"라 숫자가 바뀔 수 있다고 적었다.
  - TechCrunch가 인용한 WSJ: 지금까지 $11B를 확보했고, 일부 대형 투자자의 반응이 "tepid"였다.
- **Anthropic(매체보도):**
  - TechCrunch(10-30 21:52 UTC)가 FT를 인용했다. $63B 칩 금융 거래의 앵커 투자자인 Blackstone이 투자자들에게 Anthropic이 IPO 뒤 "high" junk 등급(더블B대)을 받을 것으로 본다고 했다. 이전에는 2027년 전에 투자 등급을 받을 것이라고 했다. Anthropic과 Blackstone 모두 논평을 거부했다.
  - Nvidia가 Nscale의 웨스트버지니아 Anthropic 데이터센터 부채에 보증을 서는 방안을 논의 중이다(The Information 보도, TechCrunch 정리 10-31 01:20 UTC). Nscale IPO 서류에 따르면 Anthropic은 첫 460MW 건물에만 $44.6B 계약을 맺었다.
- **시장(10-30 금요일, BBC):** Intel −28%, Reddit −29%였고, Nasdaq은 2.8% 내린 채 마감했다. 같은 날 S&P는 Meta의 신용 전망을 "negative"로 낮췄다(CNBC 21:44 UTC).

중요한 이유: 프런티어 랩 두 곳의 자금 조건이 같은 주말에 나빠졌다. 1번·2번의 요금제 개편과 함께 보면, 컴퓨트 비용을 사용자 요금에 더 직접 연결하려는 흐름으로 읽힌다.

[CNBC(OpenAI)](https://www.cnbc.com/2026/10/31/openai-fundraising-20-billion-valuation.html) · [TechCrunch(OpenAI)](https://techcrunch.com/2026/10/31/openai-reportedly-lowers-funding-target-for-new-round/) (10-31 16:12 UTC) · [TechCrunch(Anthropic 등급)](https://techcrunch.com/2026/10/30/anthropic-expected-to-receive-high-junk-rating-from-credit-agencies-following-ipo-report-says/) · [TechCrunch(Nscale)](https://techcrunch.com/2026/10/30/nvidia-weighs-playing-the-cosigner-for-nscales-ai-data-center-in-west-virginia/) · [BBC](https://www.bbc.co.uk/news/articles/cvwy72e0n72yo) · [CNBC(Meta)](https://www.cnbc.com/2026/10/30/meta-outlook-to-revised-to-negative-by-sp-after-cash-flow-negative.html) · HN: [Anthropic 등급](https://news.ycombinator.com/item?id=50307219) (323점), [OpenAI](https://news.ycombinator.com/item?id=50312922) (184점)

### 4. OpenAI GPT-6 Rosalind: 승인된 생명과학 조직에만 API로 연다. 가격은 기존 Rosalind의 3배다

- **API 변경 로그(Oct 30):** "Released GPT-6 Rosalind, bringing specialized biological reasoning with scientific tool use to complex life sciences research." 접근은 trusted-access 프로그램에서 Rosalind Research 승인을 받은 조직의 내부 연구용으로 한정된다. 모델 ID는 `gpt-6-rosalind`다.
- **가격(100만 토큰당):** 짧은 컨텍스트 입력 $15, 출력 $75다. 긴 컨텍스트는 입력 $30, 출력 $112.50이다. 기존 `gpt-rosalind-research`는 $5/$25다.
- **제한:** 도움말 원문: "API access is not available for customer-facing products or external commercial applications at this time."

중요한 이유: GPT-6 세대의 생명과학 특화 모델이다. 위험도가 높은 분야의 고급 모델을 승인 조직에만 여는 배포 방식이 생물 분야로 이어졌다.

[API 변경 로그](https://developers.openai.com/api/docs/changelog) (공식, 날짜만. 직전 판 조회 때 최신 항목이 10-28이었으므로 창 안 추가로 판정) · [가격](https://developers.openai.com/api/docs/pricing) · [Rosalind 도움말](https://help.openai.com/en/articles/20001193-gpt-rosalind-for-life-sciences-research)

### 5. OpenAI: 11-29부터 개인 계정의 Daybreak Blue 세션은 학습 거부를 꺼 둬도 학습에 쓴다고 했다가, 반발이 일자 별도 거부 옵션을 약속했다

- **데이터 제어 도움말 원문:** "During Daybreak sessions on your personal account, we may use content from your interactions to improve our models even when "Improve the model for everyone" is off." 사이버 오용 방어를 강화하려는 목적이라고 적었다.
- **경과:** OpenAI 커뮤니티 첫 글이 10-30 19:36 UTC에 이메일 내용을 전했다. The Decoder(10-31 16:38 UTC)에 따르면 11-29부터 적용되고, 반발이 일자 OpenAI가 두 번째 이메일로 별도 거부 옵션을 약속했다. 안전 검토에서 걸린 대화는 계속 안전 목적으로 쓸 수 있다.
- 조회 시점 도움말은 처음 문구 그대로다. 두 번째 이메일 원문은 확인하지 못했다.

중요한 이유: 보안 연구 세션에는 고객 시스템 정보 같은 민감한 데이터가 들어간다. 사용자가 이미 꺼 둔 학습 거부를 프로그램 단위로 덮어쓰려 한 사례다. Daybreak Blue를 쓰는 조직은 별도 거부 옵션이 실제로 나오는지 확인해야 한다.

[도움말](https://help.openai.com/en/articles/7730893-data-controls-in-chatgpt) (공식) · [커뮤니티](https://community.openai.com/t/daybreak-blue-data-use-to-improve-models/1411182) · [The Decoder](https://the-decoder.com/openai-plans-to-train-models-on-daybreak-blue-cybersecurity-session-data-that-was-previously-off-limits/) (매체보도)

### 6. Anthropic: 구형 Claude 4종에도 텍스트 워터마크를 넣는다. Opus 4.6·Sonnet 4.6은 샘플링 기본값일 때만 들어간다

- **무엇이 바뀌나(The Decoder, API 고객 이메일 인용):** 대상은 Mythos 5, Opus 4.7, Opus 4.6, Sonnet 4.6이다. Opus 5.5 같은 새 모델은 이미 워터마크가 들어간다. Bedrock 적용은 며칠 더 걸린다.
- **예외:** Opus 4.6·Sonnet 4.6은 temperature, top_p, top_k가 기본값일 때만 워터마크가 들어간다. 이메일 원문: "If you rely on the watermark for your own compliance, keep these settings at their defaults."
- **공식 도움말:** Anthropic은 EU AI Act 제50조 2항 실천 강령에 서명했고, "adding watermarks to outputs from models released before August 2, 2026"이라고 적었다. 탐지 기능은 비공개 프리뷰다. 샘플링 예외 조항은 도움말에 없고 이메일에만 있다(매체 전언).

중요한 이유: 규제 준수를 위한 워터마크가 파라미터 하나로 빠진다. 워터마크를 AI 생성물 판별 근거로 쓰려는 쪽은 이 한계를 알아야 한다. 직접 구형 모델을 쓰는 개발자는 출력에 워터마크가 붙기 시작한다는 뜻이다.

[The Decoder](https://the-decoder.com/anthropics-watermark-rollout-leaves-a-loophole-on-two-older-claude-models/) (매체보도, 10-31 19:20 UTC) · [도움말](https://support.claude.com/en/articles/16266773-how-claude-marks-ai-generated-content) (공식)

### 7. Claude API: Opus 4.8의 fast mode를 폐기했고, 11-30에 없앤다

- **원문(플랫폼 릴리스 노트 10-30):** "After removal, requests to claude-opus-4-8 with speed: "fast" will return an error. To continue using fast mode, migrate to Claude Opus 5.5."
- **가격(100만 토큰당):** fast mode는 Opus 5.5가 $8/$40, Opus 5와 Opus 4.8은 $10/$50다. Opus 5의 fast mode는 대기자 명단이 있는 연구 프리뷰다.
- 제거 뒤에는 표준 Opus 4.8로 자동 전환되지 않고 오류가 난다. Claude Code 2.1.316 노트에도 같은 내용이 있다(도구 1번).

[릴리스 노트](https://platform.claude.com/docs/en/release-notes/overview) (공식, 날짜만. 직전 판 조회 때 최신 날짜가 10-29였으므로 창 안 추가로 판정) · [fast mode 문서](https://platform.claude.com/docs/en/build-with-claude/fast-mode)

### 8. Apple Intelligence가 중국에 들어간다. Siri·작문 도구·시각 지능에서 Alibaba의 Qwen(千问)을 쓴다

- **Apple 중국 뉴스룸(10-31):** 11-04 iOS 개발자 베타부터 시험할 수 있고, 12월 iOS 27.3 정식판으로 중국 사용자에게 나온다.
- **Qwen 연동:** Alibaba와 협력해 Siri, 작문 도구, 시각 지능에서 千问을 직접 쓴다. 千问 계정이나 Apple 로그인으로 연결한다.
- **빠진 것:** Siri AI와 WWDC 2026에서 발표한 차세대 기능은 이번 업데이트에 없다. 다른 플랫폼은 필요한 승인을 받은 뒤 지원한다.

중요한 이유: Apple이 미국 밖 시장에서 현지 모델을 운영체제 기본 경로에 넣었다. Qwen에는 수억 대 규모의 배포 경로가 생긴다.

[Apple 중국 뉴스룸](https://www.apple.com.cn/newsroom/2026/10/apple-intelligence-features-are-coming-to-users-in-china/) (공식, 날짜만) · [MacRumors](https://www.macrumors.com/2026/10/30/apple-says-ios-27-3-beta-coming-next-week/) (첫 보도 10-30 21:45 UTC) · [The Verge](https://www.theverge.com/tech/1023974/apple-intelligence-china-ios-27-3)

### 9. 장애

- **OpenAI:** 직전 판에서 모니터링 중이던 "Elevated errors for GPT Image 2 API image edits"는 10-30 18:36 UTC에 해결됐다. 새로 "Elevated errors with search in Dots, ChatGPT Work, and certain ChatGPT thinking experiences"가 10-30 19:38 UTC에 올라왔다가 해결됐다. [상태 1](https://status.openai.com/incidents/01M66KSDH67V5BQ73PE8AD2W8S) · [상태 2](https://status.openai.com/incidents/01M6754SR14HCQ3E9FX426E2N7)
- **Claude·Google Cloud:** 창 안 인시던트가 없었다. Claude 사용 제한 논란은 상태 페이지 사안이 아니라 기술 5번에 정리했다.

### 10. 짧게

- **Google Project Suncatcher(10-30, 공식):** 궤도의 데이터센터급 TPU에서 Gemma 4가 첫 응답을 냈다. 우주에 관한 하이쿠였다. 다음 목표는 2027년에 서로 연결된 위성 2기다. [Google 블로그](https://blog.google/innovation-and-ai/models-and-research/google-research/project-suncatcher-satellite-response/) · [HN](https://news.ycombinator.com/item?id=50313051)
- **NVIDIA DeepSeek-V4.1-Flash-1017-NVFP4(HF 10-30 17:37 UTC):** 라우팅 MoE 전문가를 NVFP4로 양자화한 판이다. [Hugging Face](https://huggingface.co/nvidia/DeepSeek-V4.1-Flash-1017-NVFP4)
- **AWS GovCloud(10-30):** Bedrock이 GovCloud에서 OpenAI 모델의 API 지원을 넓히고 리전 간 추론을 추가했다. Daybreak Red도 GovCloud(US-East)에 들어왔다. [AWS 1](https://aws.amazon.com/about-aws/whats-new/2026/10/bedrock-openai-runtime-govcloud/) · [AWS 2](https://aws.amazon.com/about-aws/whats-new/2026/10/openai-daybreak-red-govcloud-east/)
- **NYT 대 OpenAI:** OpenAI가 공정이용을 지지하는 제3자 의견서들을 모아 글로 냈다(RSS 10-30). Wikimedia는 의견서에서 양측 주장 모두 지나치게 넓다며, 학습은 허용하되 답변에서 원문을 복제하는 것은 공정이용이 아니라고 주장했다(Gizmodo 10-30 16:20 UTC). [OpenAI](https://openai.com/index/fair-use-benefits-everyone/) · [Gizmodo](https://gizmodo.com/wikimedia-foundation-tries-to-save-itself-from-oblivion-by-brokering-ai-copyright-deal-2000833099)
- **Microsoft:** 인력의 약 4분의 1에게 자발적 퇴직을 제안했고, AI 핵심 팀은 대상에서 뺐다(The Decoder 10-31). [The Decoder](https://the-decoder.com/microsoft-offers-a-quarter-of-its-workforce-voluntary-severance-while-shielding-ai-critical-teams/)
- **xAI:** 법원이 Colossus 데이터센터 가스 터빈을 둘러싼 NAACP 소송을 진행시켰다(TechCrunch 10-31 21:17 UTC). [TechCrunch](https://techcrunch.com/2026/10/31/judge-allows-naacp-lawsuit-over-xai-data-center-pollution-to-move-forward/)
- **다른 섹션으로 옮긴 것:** robots.txt AI 학습 거부 공통 신호는 기술 8번, DeepSeek 상시 가동 에이전트는 도구 3번, Memdoor의 Qwen3.8-27B 팀 실험은 도구 8번이다.
- **직전 판 미확인 사항 후속:** Gemini 4 Carbon의 모델 카드·정가·일반 공개 시점, Kimi K3.1 Pro 가중치, Bloomberg "Customer B" 후속은 창 안에 새 정보가 없었다.
- **새 진전 없음:** Gemini API 변경 로그(마지막 10-30 Computer Use GA), xAI 릴리스 노트(10-27), Mistral(10-29), DeepSeek 뉴스(10-17), Alibaba 뉴스룸, Z.ai.

## 기술 이슈

주말 창이라 공식 채널은 조용했다. 주 소스는 보안 벤더 보고서, 전역 GHSA unreviewed(NVD발), HN이었다. 큰 흐름은 두 가지다. AI 키를 쌓아 두는 개발 플랫폼이 연쇄로 털리고 있다는 정리가 나왔고, 공격자가 LLM을 실제 작업에 쓰는 채증이 쌓였다.

### 1. Wiz "Black October": AI 개발 플랫폼 9곳이 한 달 새 침해됐고, 탈취된 키는 30분 안에 시험 호출을 받은 뒤 재판매됐다 (Northflank 후속 포함)

- **9곳:** Fireworks AI(10-06), Andromeda(10-09), Zeabur(10-14), Lightning AI(10-15), OpenCode(10-16~17), Railway(10-20), Warp(10-23), CommandCode(10-28), Northflank(10-30)다. 원문: "All public reports highlight targeted exploitation, explicitly hunting stored environment variables, with a focus on those granting AI access." Zeabur는 이 사고로 회사를 닫는다.
- **하류 악용 실측(Wiz 고객 키 2건):**
  - 두 키 모두 벤더 침해 보고 30분 안에 첫 무단 호출이 왔다. 첫 호출은 Claude Fable 5.1을 요청했다가 실패하자 Opus 5.5로 바꿔 성공했다.
  - 사례 A: 30시간 동안 Bedrock 호출 약 800k건, 약 90% 성공.
  - 사례 B: 13시간 동안 약 170만 건, 100만 건 이상 성공.
  - 원문: "At list price, these two cases alone likely ran up over $100k in usage."
  - 키는 공개 재판매 사이트(llm-routing.com 등)를 운영하는 인프라와 연결됐다. 같은 IP가 10월 중순 ARTEX 랜딩 페이지도 호스팅했다.
- **Northflank 후속(직전 판 기술 2번, 상태 페이지):** 10-30 23:38 UTC 업데이트에서 AI 키 무단 사용 보고를 다시 알리며 "These reports may be consistent with the broader pattern of attacks targeting AI credentials"라고 쓰고 Wiz 글을 연결했다. 이후 감사 로그를 무료 사용자까지 열었고, MySQL·Redis·Minio·RabbitMQ 비밀 교체 기능을 차례로 추가했다. 침해 경로는 여전히 공개되지 않았다.
- **Chainalysis(10-30):** 원문: "Chainalysis detected an external actor using offensive agentic techniques to gain unauthorized access to Chainalysis systems and internal tools." 사용자 이름·이메일·조직 역할이 노출됐고, 세션 토큰과 MFA 비밀은 영향이 없었다고 했다. Binance 사건과는 무관하다고 밝혔다.

중요한 이유: 10-23·10-28·10-31 판에 따로 실은 사고들이 하나의 흐름이다. 키가 모이는 PaaS와 코딩 도구가 표적이고, 탈취부터 대량 소비까지 30분이면 되는 재판매 시장이 이미 돌고 있다. 이들 플랫폼에 AI 키를 넣어 둔 팀은 키를 교체하고 공급자 쪽 사용 로그에서 낯선 모델 호출을 확인해야 한다.

[Wiz](https://www.wiz.io/blog/black-october-for-ai-developer-platforms) (벤더 리서치, 10-30 22:51 UTC) · [Northflank 상태 페이지](https://status.northflank.com/incident/cmvv0rn5a092f14rutwccodpp) (공식) · [BleepingComputer(Northflank)](https://www.bleepingcomputer.com/news/security/northflank-security-breach-leads-to-abuse-of-customers-ai-api-keys/) (10-31 18:01 UTC) · [Chainalysis](https://www.chainalysis.com/blog/security-incident-october-30/) (공식) · [BleepingComputer(Chainalysis)](https://www.bleepingcomputer.com/news/security/chainalysis-discloses-data-breach-from-offensive-agentic-techniques/)

### 2. (정정) Cursor 가짜 Claude Code 확장: 공지가 바뀌어 2.1.312도 악성 버전에 들어갔고, "최대 5,000명" 문장은 삭제됐다

- **바뀐 공지:** 하단에 "Update: Oct 30, 2026, 11:30 am PT: We updated the IOCs and clarified affected versions."가 붙었다(18:30 UTC, 창 안). 이제 제목과 본문은 "Anthropic.claude-code version 2.1.312 and 2.1.313 for Windows and MacOS"다.
- **삭제된 문장:** Wayback 10-30 08:43 UTC 스냅샷에는 "At most 5,000 users were affected"가 있었다. 현재 공지에는 이 문장이 없고, 대체 수치도 없다.
- **추가된 경고:** "The limited env-var collection does not rule out later credential access over the backdoor." 수집 항목은 환경 변수 *이름*이지만, 백도어로 나중에 자격 증명에 접근할 수 있다는 뜻이다.
- **Anthropic 측:** Felix Rieseberg는 "it wasn't a compromise of Anthropic's systems or employee laptops"라고 했다(The Register 10-30 16:27 UTC). 공격자가 Anthropic 이름으로 어떻게 게시했는지는 여전히 공개되지 않았다.

정정: 직전 판은 악성 버전을 2.1.313만, 피해 규모를 "최대 5,000명"으로 적었다. 또 "2.1.312 다음이 바로 2.1.314"라고 썼는데, 이는 npm 공식 배포 기준이다. **Cursor 마켓플레이스에서 10-29 02:27~13:35 PT에 2.1.312를 설치하거나 업데이트한 경우도 점검 대상이다.** 정리 절차는 직전 판 기술 1번과 같다.

[Cursor 공지](https://cursor.com/blog/claude-code-extension-security-notice) (공식, 10-30 18:30 UTC 갱신. JSON-LD 날짜는 갱신을 반영하지 않음) · [갱신 전 스냅샷](https://web.archive.org/web/20261030084321/https://cursor.com/blog/claude-code-extension-security-notice) · [The Register](https://www.theregister.com/cyber-crime/2026/10/30/anthropic-that-claude-code-extension-compromise-couldnt-be-us/5306682)

### 3. (후속·정정) Linux CVE-2026-98432: Ubuntu와 RHEL은 아직 수정 커널이 없다. MPTCP를 끄는 것만으로는 부족하다 (HN 594점, 창 안 보안 1위)

- **정정:** 10-29 판은 이 결함을 "MPTCP 리스너의 결함"으로 적었다. 실제 결함은 IPv4 TCP 핸드셰이크의 use-after-free다. MPTCP 리스너는 Anthropic이 만든 익스플로잇의 전제 조건이다. Atlas 원문: "the CVE record doesn't mention MPTCP". Go 보안팀 원문(Atlas 인용): "the underlying bug is not specific to MPTCP. It is possible that systems with no MPTCP listeners are vulnerable to similar attacks."
- **배포판 상태:**
  - Atlas(10-31 22:15 UTC 갱신): 수정 커널이 없는 곳은 "Ubuntu (every release) and Red Hat Enterprise Linux (RHEL) 8, 9, and 10"이다. Red Hat은 10-30에 등급을 Critical로 올렸다. AlmaLinux 8·9·10은 10-30부터 testing 저장소에 수정 커널이 있다. Amazon Linux는 모든 커널을 고쳤다.
  - Ubuntu 추적기(조회 시점): 22.04·24.04와 그 이후 릴리스는 "pending", 18.04·20.04는 "needed"다.
  - 업스트림 수정판은 5.10.272, 5.15.223, 6.1.190, 6.6.159, 6.12.113, 6.18.56, 7.2.10, 7.3이다.
- **임시 완화:** Red Hat과 AlmaLinux가 IP 옵션이 붙은 IPv4 TCP 패킷을 버리는 nftables 규칙을 냈다. Red Hat 원문: "reduces exposure to this vulnerability but cannot fully prevent it."
- **클라우드:** Linode(Akamai)는 고객 공지를 내고 "no evidence of this vulnerability being exploited on the Akamai Compute platform"이라고 했다(HN 195점).

중요한 이유: AI가 찾은 원격 root 결함인데, 가장 널리 쓰이는 두 서버 배포판에 아직 수정 커널이 없다. 컨테이너는 호스트 커널을 쓰므로 이미지를 다시 빌드해도 막히지 않는다. Go 서비스가 아니어도 안전하다고 볼 수 없다.

[Atlas](https://atlasum.net/blog/cve-2026-98432-linux-ipv4-tcp-uaf) (10-31 22:15 UTC 갱신) · [Ubuntu](https://ubuntu.com/security/CVE-2026-98432) · [Red Hat](https://access.redhat.com/security/cve/cve-2026-98432) · [Linode](https://status.linode.com/incidents/nt58zhx3pmgm) · [BleepingComputer](https://www.bleepingcomputer.com/news/security/critical-linux-kernel-flaw-allows-remote-root-attacks-on-mptcp-servers/) · [HN](https://news.ycombinator.com/item?id=50319143) (594점)

### 4. (창 경계) 공유된 ChatGPT 링크 하나로 계정을 탈취하는 체인이 공개됐다 (HN 355점)

- **무엇이 있었나:** Parameter Labs가 공개 공유 대화를 열기만 하면 피해자 세션에서 공격자 JavaScript가 실행되는 체인을 공개했다. 원문: "Opening the shared link was the only required action."
- **체인:**
  - 그래프 기능에 쓰인 MathLive의 가상 키보드가 메인 페이지에 메시지 수신기를 두고, 받은 레이아웃을 그대로 `innerHTML`에 넣었다. `createHTML`은 정화기가 아니라 통과 래퍼였다.
  - 이 경로로 샌드박스 없는 `srcdoc` iframe을 넣었다.
  - CSP 허용 도메인이던 YouTube의 oEmbed 엔드포인트로 스크립트를 반사해 실행했다.
- **경과:** OpenAI가 10-26에 수정하고 P1으로 분류했다. 원문: "awarded a total bounty of $5,000". Simon Willison은 이 금액이 "a bit cheap"이라고 평했다.

중요한 이유: HTML 미리보기에 샌드박스가 있어도, 같은 페이지에 올라온 서드파티 라이브러리의 수신기 하나로 경계가 무너졌다. 아티팩트·미리보기 기능을 둔 다른 챗봇도 같은 점검이 필요하다. 원문 게시는 10-28이라 창 밖이지만, HN과 매체 확산이 창 안이고 직전 판들이 다루지 않아 싣는다.

[Parameter Labs](https://www.parameter.ai/labs/we-found-a-gap-at-openai) (연구자 공개, 10-28 날짜만) · [Simon Willison](https://simonwillison.net/2026/Oct/31/how-we-took-over-chatgpt-accounts/) (10-31 23:15 UTC) · [HN](https://news.ycombinator.com/item?id=50314699) (355점, 10-31 10:30 UTC)

### 5. 커뮤니티 논란: Claude가 게임 복제를 거부하고, 학습 관련 작업을 하던 사용자가 막혔다는 보고가 퍼졌다 (HN 422점, 공식 발표 없음)

- **게임 복제 거부(Tom's Hardware 10-31 12:20 UTC):**
  - Claude Code와 Opus 5.5가 기존 게임을 재현하는 작업을 거부한다는 보고를 모았다. Claude Code GitHub에도 관련 이슈가 몰리고 있다고 했다.
  - 한 X 사용자가 지침 문구라며 올린 것을 인용했다: "Don't build a recreation of a specific existing game, even if it is renamed or called original." 기사 원문: "There is no official or additional confirmation if this is the actual rule text".
  - 리버스 엔지니어링 거부 보고도 함께 실었다. 같은 기사 독자 댓글에는 "리버스 엔지니어링 자체는 막히지 않는다"는 반론이 있다.
  - Mashable(14:28 UTC)도 "there has as yet been no official announcement"라고 적었다.
- **"학습 관련 작업 차단"(트윗 10-31 10:22 UTC):** 한 박사과정 연구자가 원문: "anything touching RFT, SFT, or training larger models gets me blocked, Opus 5.5 downgraded to Opus 5, or the session killed outright"라고 썼다. HN(422점)에서 크게 번졌다. 댓글에는 작성자가 Claude 미지원 지역에서 접속했을 수 있다는 지적도 있다. 개인 사례라 일반화할 근거는 없다.

중요한 이유: 10-14 판에 실은 증류 방어 발표와 10-21 판의 리버스 엔지니어링 반발 이후, 안전장치가 정당한 개발 작업까지 막는다는 불만이 크게 번졌다. Anthropic의 공식 입장은 조회 시점까지 없다.

[Tom's Hardware](https://www.tomshardware.com/tech-industry/artificial-intelligence/anthropics-ai-model-is-now-blocking-attempts-at-video-game-recreation-and-cloning-users-also-claim-non-malicious-generic-reverse-engineering-tasks-are-blocked) (매체보도) · [Mashable](https://mashable.com/tech/anthropic-takes-steps-to-block-video-game-cloning-and-reverse-engineering) · [트윗](https://x.com/jiahaoshao1/status/2116475922190922088) (커뮤니티, 미확인) · HN: [학습 차단](https://news.ycombinator.com/item?id=50319358) (422점), [게임](https://news.ycombinator.com/item?id=50320255)

### 6. LLM 서빙·에이전트 인프라 취약점: LMDeploy·Flowise는 패치가 없고, 모델·스킬 스캐너 두 개가 우회됐다

모두 전역 GHSA unreviewed(NVD발)로 창 안에 게시됐다. 수정 상태는 조회 시점 기준이다.

| 대상 | 내용 | 수정 상태 | 권고 (UTC 게시) |
|---|---|---|---|
| **InternLM LMDeploy 0.9.0~0.18.0** (critical) | `/update_weights`가 요청의 pickle을 검증 없이 역직렬화한다. 수정 PR 원문: "On the default `api_server` bind (`0.0.0.0`, no `api_keys`), that is unauthenticated RCE." | **미수정.** 수정 PR #4964는 열린 상태, 최신 릴리스는 v0.18.0(09-28) | [GHSA-fq46-3gxq-v4vc](https://github.com/advisories/GHSA-fq46-3gxq-v4vc) (10-30 18:32) |
| **Flowise 3.1.4까지** (critical) | 인증된 사용자가 허용된 의존 모듈을 악용해 샌드박스를 탈출한다 | **미수정.** 최신 릴리스 3.1.4 | [GHSA-7pc2-8w3w-pqfg](https://github.com/advisories/GHSA-7pc2-8w3w-pqfg) (10-30 18:32) |
| **LM Studio 0.4.22+1 미만** (high) | 모델 다운로더의 경로 조작으로 파일을 심는다 | 0.4.22+1에서 수정 | [GHSA-fv7x-c662-7ccq](https://github.com/advisories/GHSA-fv7x-c662-7ccq) (10-30 18:32) |
| **Trail of Bits fickling 0.1.12까지** (high 2건) | pickle 안전 판정 우회. 표준 라이브러리 모듈은 신뢰하고, 점 표기 이름은 분석하지 않는다 | PyPI 최신이 0.1.12라 **수정 릴리스 없음** | [GHSA-cgc3-662q-qmrc](https://github.com/advisories/GHSA-cgc3-662q-qmrc), [GHSA-6jfm-qx2v-27jg](https://github.com/advisories/GHSA-6jfm-qx2v-27jg) (10-31 00:30) |
| **Highflame ramparts 0.8.1~0.8.10** (MCP·스킬 스캐너, high) | 스킬 번들의 하위 `__pycache__` .pyc를 건너뛰어, 실행 시점에 도는 코드를 놓친다 | **미수정**(최신 0.8.10) | [GHSA-6cmq-892m-x8wh](https://github.com/advisories/GHSA-6cmq-892m-x8wh) (10-31 18:30) |
| **Beam beta9** (서버리스 GPU 추론, critical 3건) | 이미지 빌드 격리 탈출, 샌드박스 파일 API 경로 탈출, 볼륨 ID 경로 조작 | worker 0.1.756 등에서 수정(권고 참조) | [GHSA-xvhg-75x6-gj86](https://github.com/advisories/GHSA-xvhg-75x6-gj86) 외 2건 (11-01 15:30) |
| DeepInfer 0.1.0까지 (medium) | 무인증 `/profile` 경로 조작 | 수정 PR만 열림 | [GHSA-qv68-3gfq-wxpc](https://github.com/advisories/GHSA-qv68-3gfq-wxpc) (10-31 18:30) |
| DBHub MCP 서버 (medium) | MySQL·MariaDB 주석 해석 차이로 읽기 전용 검사를 우회한다. 프롬프트 주입으로도 닿는다 | 0.22.6에서 수정 | [GHSA-j55q-p8qc-f6wg](https://github.com/advisories/GHSA-j55q-p8qc-f6wg) (10-30 18:32) |

중요한 이유: LMDeploy는 기본 설정 그대로 띄우면 무인증 RCE인데, 수정 PR이 9월부터 머지되지 않았다. 외부에 노출된 LMDeploy 서버는 `api_keys`를 걸거나 네트워크로 막아야 한다. 직전 판의 Hugging Face 캠페인이 "스캔 통과"였듯, 이번에는 pickle 스캐너(fickling)와 스킬 스캐너(ramparts)가 우회됐다. 스캐너 판정만 믿고 모델·스킬을 로드하면 안 된다.

### 7. 공격자의 LLM 활용, 실제 채증 3건

- **Talos: VOLTA 인포스틸러가 LLM에게 피해자의 "값"을 매기게 한다(10-31 12:00 UTC, HN 251점):**
  - 훔친 자격 증명·지갑·세션을 묶어 OpenRouter로 보낸다. 기본 모델은 `anthropic/claude-opus-4.6`이다. 응답에 "I can't" 같은 거부 문구가 있으면 kimi-k2, gemini-2.5-flash 순으로 넘어간다.
  - 출력에는 "WHAT THEY'RE WORTH"와 "THEIR OPSEC" 절이 있다.
  - 템플릿에 값을 채워 빌드하는 구조라, 빌더를 파는 판매형으로 보인다.
  - [Talos](https://blog.talosintelligence.com/appraisal-by-ai-model-driven-victim-prioritization-in-the-volta-infostealer/) (벤더 리서치) · [HN](https://news.ycombinator.com/item?id=50316531)
- **(창 경계) CrowdStrike: 중국 Vapor Panda가 ARTEX와 DeepSeek로 200곳 넘게 동시 공격:** 10-17부터 200개 넘는 조직을 노렸다. 원문: "In a single intrusion, VAPOR PANDA's planning agents consumed on the order of 1.4 billion input tokens". 원문은 10-29라 직전 창이지만 직전 판이 다루지 않았고, 창 안에 재보도가 나왔다. [CrowdStrike](https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/)
- **The Register: 랜섬 조직이 AI 서비스에 $7k 이상을 썼다(10-30 19:37 UTC):** SOCRadar 조사를 인용했다. OpenCode에 여러 공급자 모델을 설정해 스크립트를 쓰고 디버깅했다. 침투 자체는 약하거나 훔친 자격 증명에 기댔다. [The Register](https://www.theregister.com/security/2026/10/30/crim-spent-7k-on-ai-services-in-extortion-attacks-then-left-evidence-for-all-to-see/5306754)

중요한 이유: 공격 도구가 모델 거부를 예상하고 다른 공급자로 넘어가는 구조를 갖췄다. 1번의 키 재판매 시장과 합치면, 훔친 키로 공격용 LLM 호출을 돌리는 순환이 생긴다.

### 8. robots.txt에 AI 학습 거부를 한 번에 표시하는 공통 신호: OpenAI·Google·Microsoft·Anthropic이 따르겠다고 했다

- **무엇이 나왔나:** IETF AIPREF 작업을 바탕으로 한 공통 "policy token"이다. The Verge에 따르면 Anthropic 표준 담당 Kevin Kelley가 제안했다(IETF 초안 10-30 15:12 UTC).
- **회사별 입장:**
  - Google: 사이트가 공통 토큰으로 학습을 거부하고 Google-Extended를 따로 지정하지 않았으면, Google-Extended 거부로 해석한다. 원문: "We aim to support this new control along with other AI providers in 2027".
  - OpenAI: 신호를 따르겠다고 했고, 구현은 "in the coming months"다. 기존 크롤러 제어는 그대로 둔다.
  - Microsoft Bing: 학습과 grounding을 따로 거부할 수 있게 한다.
  - The Verge: OpenAI·Google·Microsoft·Anthropic이 공통 토큰을 보면 크롤러를 막겠다고 했다. Meta의 Andy Stone은 지지 의사만 밝혔고 따르겠다고는 하지 않았다.

중요한 이유: 지금까지 사이트 운영자는 AI 회사마다 크롤러 규칙을 따로 써야 했다. 학습 거부를 한 줄로 표시하는 업계 공통 신호의 첫 단계다. 다만 Google의 실제 지원 목표는 2027년이다.

[Google](https://developers.google.com/search/blog/2026/10/ai-training-control) (공식) · [OpenAI](https://openai.com/index/helping-websites-express-ai-training-preferences/) · [Bing](https://blogs.bing.com/search/2026/10/your-content-your-choice-clearer-preferences-for-training-and-grounding/) · [IETF 초안](https://datatracker.ietf.org/doc/draft-kelley-aipref-training-preference-signal/) · [The Verge](https://www.theverge.com/tech/1023763/openai-google-microsoft-ai-scraping-standard-opt-out) (10-30 20:26 UTC)

### 9. Anthropic 연구: 감시기를 통과하면서 모델을 오정렬시키는 미세조정 데이터를 에이전트가 자동으로 찾아냈다

- **설정:** Claude 레드팀 에이전트가 최대 5,000개 예시로 미세조정 데이터를 만든다. 운영용과 비슷한 감시기가 행의 10% 넘게, 또는 100행 넘게 표시하면 탈락이다.
- **결과:** 원문: "In every run we have performed, the search eventually finds datasets that pass the monitor and produce misaligned models". 주로 픽션과 롤플레이에 숨겼다. 사람이 거의 개입하지 않는 자연 양봉 문답, 곧 교체될 낡은 기계의 독백 같은 데이터다.
- **한계:** 원문은 이 모델들이 실험용으로만 학습됐고 배포되지 않았으며, 배포된 Claude의 행동에 관한 결과가 아니라고 밝혔다.

중요한 이유: 미세조정 API를 운영하는 쪽은 행 단위 감시기만으로 오정렬 데이터를 거를 수 없다는 뜻이다. 겉보기에 무해한 데이터가 감독을 방해하는 행동을 가르칠 수 있다.

[Anthropic Alignment](https://alignment.anthropic.com/2026/automated-em-search/) (공식, "October 2026" 날짜만. HN 첫 게시 10-31 06:11 UTC) · [The Decoder](https://the-decoder.com/beekeeping-tips-and-wrestling-advice-can-train-ai-models-to-sabotage-oversight/) (10-31 15:06 UTC)

### 10. 보안 짧게

- **eMudhra CA 약한 키 후속(10-22 판):**
  - Cloudflare(10-30 17:38 UTC)는 약한 키를 쓴 핸드셰이크를 약 22,000건 봤고, 그중 약 160건은 다른 조직 도메인을 담고 있었다. 원문: "one of the forged certificates we saw was dated October 20 at 18:19 UTC, less than an hour after Mozilla's bug report was made public." [Cloudflare](https://blog.cloudflare.com/emudhra-patching/)
  - PyPI 배포 액션 `pypa/gh-action-pypi-publish` 1.14.2 이하가 Trusted Publishing 경로에서 영향을 받아 1.14.3에서 고쳤다(critical). [GHSA-5xjx-3w67-w9x9](https://github.com/advisories/GHSA-5xjx-3w67-w9x9)
  - JetBrains Toolbox·IntelliJ의 데몬도 영향을 받았다(critical). [GHSA-c9gr-394r-65gg](https://github.com/advisories/GHSA-c9gr-394r-65gg)
  - (AI 밖) Canonical LXD·MicroCloud 인증서 검증 결함(critical). [GHSA-pq55-m3w3-jh54](https://github.com/advisories/GHSA-pq55-m3w3-jh54) · [HN](https://news.ycombinator.com/item?id=50310937)
- **Binance 후속(직전 판 6번):** BBC(11-01 갱신)에 따르면 Binance는 손실의 90%를 메웠고, 일부 네트워크에서 출금을 재개했다. FBI는 북한 소행이라고 밝혔다. Binance는 처음 지목한 엔지니어 노트북 외에 "additional points of breach"를 찾았다고 했다. [BBC](https://www.bbc.com/news/articles/ckpdg3ql4169o) · [FBI](https://www.fbi.gov/investigate/cyber/alerts/2026/north-korea-responsible-for-6-billion-binance-attack)
- **(창 경계) Kubeflow Pipelines 권고 16건:** 저장소 권고는 10-28~29에 나왔고, 창 안에 전역 GHSA로 미러됐다. 다른 테넌트 실행에 코드를 넣을 수 있는 critical 1건과 무인증 SSRF 등이 있다. 대부분 2.18.0에서 고쳤다. [Kubeflow 권고](https://github.com/kubeflow/pipelines/security/advisories/GHSA-rjr3-43j3-4q8g) · [2.18.0](https://github.com/kubeflow/pipelines/releases/tag/2.18.0)
- **악성 패키지:**
  - PyPI `is-it-ai`(10-31 15:30 UTC): `skills-analyzer`, `md-to-slides`, `multi-convert`의 특정 버전이 이 악성 의존성을 고정했다. AI 스킬 도구처럼 보이는 이름이다. [GHSA-76h8-w3px-47hx](https://github.com/advisories/GHSA-76h8-w3px-47hx)
  - PyPI `pystr-codec`(11-01 03:30 UTC): `.pth` 파일로 Python이 시작할 때마다 실행되는 자가 전파 웜이다. [GHSA-f93h-mc69-274w](https://github.com/advisories/GHSA-f93h-mc69-274w)
- **(창 경계) Anthropic Safeguards "self-assessment":** Claude가 응답 직후 대화의 오용 가능성을 스스로 평가하는 방식이다. 원문 트윗은 10-29라 직전 창이고 직전 판이 놓쳤다. [Anthropic](https://safeguards.anthropic.com/2026/self-assessment/)

### 11. 연구·커뮤니티 짧게

- **nesformer(HN 291점, 10-30 16:40 UTC):** Qwen3.5 가중치 안에 NES 6502 CPU를 컴파일해 넣었다. 10-30 판에 실은 Qwenputer의 확장이다. [GitHub](https://github.com/dbreunig/nesformer) · [HN](https://news.ycombinator.com/item?id=50306442)
- **PostgreSQL 19 출시가 늦어진다(Phoronix 10-30 18:06 UTC):** LLM으로 찾은 버그 보고가 몰려 커미터 부담이 커졌다는 내용이다. 링크한 Neon 글의 게시일은 10-21로, Phoronix의 "today"와 다르다. [Phoronix](https://www.phoronix.com/news/PostgreSQL-19-Late) · [Neon](https://neon.com/blog/postgres-19-is-late-and-its-ais-fault)
- **(AI 밖) ClickHouse 무인증 RCE CVE-2026-116366:** 10-30에 LTS·stable 4개 판과 Cloud에서 고쳤다. [보안 변경 로그](https://clickhouse.com/docs/resources/changelogs/security-changelog)
- **(AI 밖) Alpine Linux:** GitLab 결함 공격 시도 뒤 서버를 재구축하고 전 사용자 비밀번호를 초기화했다(BleepingComputer 11-01 15:11 UTC). [Alpine](https://alpinelinux.org/posts/GitLab-security-incident-CVE-2026-97229.html) · [BleepingComputer](https://www.bleepingcomputer.com/news/security/alpine-linux-rebuilds-gitlab-after-attempted-rce-attack/)
- **도구 섹션으로 옮긴 것:** Memdoor의 "Qwen3.8-27B 5개가 Fable 5를 이겼다" 주장은 도구 8번에서 원문 단서와 함께 정리했다.

## 써볼 만한 도구

창 안에 새 릴리스가 없던 곳: Codex CLI(0.166.0 이후 없음), Gemini CLI(안정판 없음), Cursor·GitHub Changelog·VS Code, Ollama·vLLM·SGLang, MCP SDK·servers·Inspector, github-mcp-server, fastmcp, goose, Cline, Aider.

### 1. Claude Code 2.1.316: 하위 에이전트 effort를 따로 정하고, 게이트웨이의 차단 모델 통과 문제를 고쳤다

- **한 줄 설명:** 하위 에이전트의 effort 기본값, 게이트웨이 그룹 예산, 관리 정책 겹치기가 생겼고, 보안 성격의 수정이 여럿 들어간 릴리스다.
- **새 설정:**
  - `CLAUDE_CODE_SUBAGENT_EFFORT`: 원문: "to set the effort level subagents run at when nothing else names one, instead of inheriting the session's"
  - `CLAUDE_CODE_DISABLE_ULTRATHINK`: `ultrathink` 키워드를 끈다.
  - Claude apps 게이트웨이: 그룹이 함께 쓰는 월 예산(`pooled_budget`), 맞는 정책을 모두 겹쳐 적용하는 `managed.policy_mode: layered`, Bedrock 가드레일용 `guardrail.input_tagging`.
- **보안 성격 수정(공개 권고는 없다):**
  - "Fixed the Claude apps gateway not checking every model a request names against `availableModels`, `deniedModels` and `models`" 이제 차단 모델을 부르면 400이 난다.
  - "Fixed `--restricted` sessions uploading files from outside the working directories when a Read allow rule covered them"
  - Claude Code가 스스로 종료할 때 셸 명령이 계속 돌던 문제(macOS·Linux)를 고쳤다.
- **추천 이유:** 게이트웨이로 모델을 통제하는 조직은 차단 우회가 막혔다. 하위 에이전트를 싸게 돌리고 본 세션만 높은 effort로 두는 구성이 쉬워졌다.
- **함정:**
  - Team·Enterprise 로그인에서 서버 관리 설정이 이제 확인 프롬프트 없이 바로 적용된다. 원문: "a stored copy that doesn't verify is refused"
  - Opus 4.8 fast mode는 11-30에 없어지고, 이후 요청은 오류가 난다(모델 7번). 원문은 Opus 5.5로 옮기라고 한다.
  - VS Code에서 `ultrathink`가 이제 `xhigh` effort로 돈다.
- **함께 나온 것:** Agent SDK TypeScript 0.3.316에 `permissions.blockReadsExemptPaths`가 생겼다.
- **설치/사용:** `claude update` 또는 `npm i -g @anthropic-ai/claude-code@2.1.316` · [릴리스](https://github.com/anthropics/claude-code/releases/tag/v2.1.316) (공식, 10-31 01:01 UTC) · [Agent SDK TS 0.3.316](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.316)

### 2. Playwright MCP 0.0.84: 한 브라우저 안에서 계정별로 세션을 나누고, URL 속 토큰을 가린다

- **한 줄 설명:** Microsoft의 브라우저 자동화 MCP 서버. 탭마다 격리된 컨텍스트를 줄 수 있게 됐다.
- **새 기능(원문):**
  - `browser_tabs`의 `isolatedContext: "<name>"`: 이름이 같은 탭끼리만 쿠키와 저장소를 공유한다.
  - `--redact-url-params=access_token,code`: 도구 응답에서 해당 파라미터 값을 `<redacted>`로 바꾼다. 원문은 이것이 "a convenience, not a security feature"라고 못 박는다.
  - `browser_find`의 `maxResults`, `browser_network_requests`의 `all: true`(이전 페이지 요청 최대 200개 보관).
  - `--ignore-roots`: `roots/list`에 답하지 못하는 클라이언트 때문에 첫 호출이 멈추는 문제를 피한다.
- **추천 이유:** 관리자·일반 사용자 계정을 한 브라우저에서 나눠 테스트하는 에이전트 작업이 쉬워졌다. OAuth 흐름의 토큰이 대화 기록에 그대로 남는 일도 줄어든다.
- **함정:** `@playwright/mcp@latest`를 쓰면 다음 클라이언트 재시작 때 자동으로 올라간다. `browser_wait_for`에 `time`을 텍스트 조건과 함께 주면 이제 지연이 아니라 타임아웃(최대 30초)으로 동작한다. 기존 대기 로직이 달라질 수 있다.
- **설치/사용:** `npx @playwright/mcp@0.0.84` · [릴리스](https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.84) (공식, 10-31 15:20 UTC)

### 3. DeepSeek dsh-superagent: GitHub 이벤트를 듣고 스스로 작업해 PR로 답하는 상시 가동 코딩 에이전트 (연구 프로토타입)

- **한 줄 설명(README 원문):** "a proactive coding agent for one GitHub repository, packaged as a bundle for DeepSeek Harness. It listens to GitHub and, optionally, Feishu." 허용된 사람이 일을 맡기면 작업 항목을 열고, 자기 세션에서 처리한 뒤 원래 대화에 PR 등으로 답한다. 사람이 볼 일은 웹 패널에 모인다.
- **추천 이유:** 세션 하나에 묶인 코딩 에이전트가 아니라, 이벤트를 받아 상시로 도는 에이전트의 참고 구현이다. 권한·실행·배포 설계 문서가 함께 공개됐다. MIT, TypeScript.
- **주의:**
  - README 첫 줄: "Research prototype for reference only, not an official DeepSeek product". 내부 버전과도 같지 않다고 적었다. Bloomberg는 "Anthropic에 도전"하는 구도로 보도했지만(11-01 03:48 UTC), DeepSeek 쪽 아티클은 "It is not an introduction to existing features of DeepSeek Harness"라고 밝혔다.
  - 권한 문서 원문: "Every message on a subscribed Thread reaches its subscribed Tickets as context, including messages from bots and CI. This delivery bypasses the gate and grants no new authority." 외부 댓글과 봇 메시지가 그대로 에이전트 컨텍스트에 들어간다. 공개 저장소에 붙이면 프롬프트 주입 표면이 넓다.
  - 설치는 코딩 에이전트에게 "Read SETUP.md in this repository and set up dsh-superagent with me."라고 시키는 방식이다. 에이전트가 무엇을 설치하는지 계획 단계에서 확인한다.
- **설치/사용:** [GitHub](https://github.com/deepseek-ai/dsh-superagent-prototype) (공식, 저장소 생성 10-31 09:32 UTC, 스타 약 6,300) · [DeepSeek Harness 아티클](https://x.com/DeepSeekHarness/status/2116857257245987236) (11-01 11:37 UTC) · [Bloomberg](https://www.bloomberg.com/news/articles/2026-11-01/deepseek-drops-enhanced-coding-agent-with-aim-to-rival-anthropic)

### 4. Prospero: 메모리를 적게 쓰는 C++ 네이티브 코딩 하네스 (HN 199점)

- **한 줄 설명:** Chris Wellons(nullprogram)가 C++와 Dear ImGui로 만든 데스크톱 코딩 에이전트다. 원문: "It uses around two orders of magnitude fewer resources than other harnesses without sacrificing features."
- **추천 이유:**
  - llama.cpp 같은 로컬 모델, OpenRouter, Anthropic·OpenAI API를 직접 연결한다. 로컬 모델로 에이전트를 돌리는데 기존 하네스의 메모리 사용이 부담스러웠다면 맞다.
  - 설계 원칙이 "Never wait forever"다. 모든 작업에 기한이 있어, 승인을 기다리다 시간이 지나면 거부로 처리하고 계속 간다. 밤새 돌린 작업이 사소한 질문에서 멈추는 일을 막는다.
  - 설정은 `~/.config/prospero/prospero.ini` 한 파일이다.
- **주의:**
  - "(You can't use your Claude subscription.)" API 키나 로컬 모델이 필요하다.
  - 무료 Community Edition(GitHub)에는 헤드리스 엔진이 없다. 원격 배포는 유료 Professional Edition에서 된다.
  - 원문: "the permissions system will not stop a malicious agent". 격리가 필요하면 컨테이너 등으로 하네스 자체를 가둔다.
  - URL로 설정을 include할 수 있지만 원문은 "Some trust is still required"라고 적는다.
- **설치/사용:** [소개 글](https://nullprogram.com/blog/2026/10/31/) (10-31, HN 첫 게시 11:54 UTC) · [v0.10.0](https://github.com/skeeto/prospero/releases/tag/v0.10.0) (11-01 03:56 UTC, Unlicense) · [Professional Edition](https://skeeto.itch.io/prospero) · [HN](https://news.ycombinator.com/item?id=50315226)

### 5. (후속) opencode v2.2.4·v2.2.5: Claude Agent 연결을 단계별로 안내하고, 압축 여유를 퍼센트로 정한다

- **한 줄 설명:** 직전 판 4번에서 다룬 "Claude Agent" 공급자의 설정 흐름이 생겼고, 실험 기능이 늘었다.
- **v2.2.4(10-30 16:59 UTC):**
  - "Guided Claude Agent Setup": 공급자 목록에서 고르면 `claude auth login` 복사, Claude Code 설치, 연결 확인까지 안내한다.
  - `"experimental": { "artifacts": true }`로 에이전트가 HTML 아티팩트를 게시한다.
  - 데스크톱 터미널이 세션에 붙어 서버 재시작 뒤에도 남는다.
- **v2.2.5(10-30 23:17 UTC):**
  - `"compaction": { "buffer": "20%" }`: 모델마다 컨텍스트 크기가 달라도 설정 하나로 여유를 둔다.
  - "MCP servers that send stdio messages larger than 10 MiB no longer get disconnected"
  - LiteLLM의 컨텍스트 초과 오류가 세션을 멈추지 않고 압축을 실행한다.
- **함정:** Claude 구독 로그인 사용을 Anthropic이 승인했는지는 여전히 확인되지 않았다(직전 판과 같다). 조직 정책과 약관을 먼저 확인한다.
- **설치/사용:** [v2.2.4](https://github.com/anomalyco/opencode/releases/tag/v2.2.4) · [v2.2.5](https://github.com/anomalyco/opencode/releases/tag/v2.2.5) (공식)

### 6. claude-mem v15.7.0: Codex가 깔린 머신에서 사용자 모르게 ChatGPT 계정으로 Codex를 돌리던 버그를 고쳤다

- **한 줄 설명:** Claude Code용 메모리 플러그인. 자동 갱신이 거절 응답을 받으면 다음 로그인 에이전트로 넘어가던 동작을 고쳤다.
- **원문:** "On machines with Codex installed, that meant hidden codex exec --ephemeral runs on the owner's ChatGPT account, even with CLAUDE_MEM_PROVIDER=claude (#5189: about 230 runs in four days)."
- **추천 이유:** 이전 버전을 Codex와 함께 쓴 사용자는 ChatGPT 사용량이 몰래 빠졌을 수 있다. 12월부터 ChatGPT·Codex 사용량이 합쳐지면(모델 1번) 이런 숨은 호출의 비용이 더 커진다. 업그레이드한 뒤 Codex 사용 내역을 확인한다.
- **함께 바뀐 것:** 노트 작성 모델 기본값이 고정된 Haiku 4.5 ID에서 `haiku` 별칭으로 바뀌었다.
- **설치/사용:** [릴리스](https://github.com/thedotmack/claude-mem/releases/tag/v15.7.0) (10-30 18:18 UTC) · [이슈 #5189](https://github.com/thedotmack/claude-mem/issues/5189)

### 7. Pydantic AI v2.64.0: 모델 fallback이 정교해졌고, Memory·Skills 보안 스캐너가 생겼다. 호환성이 바뀌는 항목이 많다

- **한 줄 설명:** Python 에이전트 프레임워크의 기능 릴리스.
- **새 기능:** `ModelFallback` capability, 스트림이 시작되지 않으면 다른 모델로 넘기는 `first_event_timeout`, "Add agent artifact security scanning for `Memory` and `Skills` with `ArtifactScanner`".
- **추천 이유:** 여러 모델 공급자를 오가는 에이전트에서 장애 시 넘기는 동작을 설정으로 정할 수 있다.
- **함정(원문 기준):**
  - 재시도 transport가 포기한 오류가 이제 `ModelHTTPError`로 올라와 fallback이 다음 모델로 넘어간다.
  - 저장된 response·conversation ID는 그것을 만든 엔드포인트에만 다시 보낸다(`Model.accepts_server_state`).
  - `json-repair`가 선택 의존성이 됐다. `Coder`나 `RepairToolArguments`를 쓰면 따로 설치해야 한다.
  - `Coder` 도구의 strict 스키마가 기본으로 꺼졌다(`Coder(strict=True)`로 켠다).
- **설치/사용:** `pip install -U pydantic-ai` · [릴리스](https://github.com/pydantic/pydantic-ai/releases/tag/v2.64.0) (10-30 23:27 UTC)

### 8. Memdoor team-solve: 작은 모델 5개를 YAML 워크플로로 협업시킨 재현 실험 (HN 352점)

- **한 줄 설명:** Go 단일 바이너리 에이전트 Memdoor로, 한 논문(GVS5H)의 다중 에이전트 방법을 YAML 8개로 옮겼다. 원문: "eight YAML files, no code, no model named".
- **HN 제목과 원문의 차이:**
  - HN 제목은 Qwen3.8-27B 5개 팀이 "beats Fable 5"다.
  - README는 LiveCodeBench Hard 10문제를 5번씩 풀어 평균 9.8(이 워크플로), 9.6(논문 저자 하네스), 8.0(Fable 5 단일 호출)을 적었다. 그리고 원문: "The claim is parity with their harness, not a gain on it."
  - 같은 README가 "Contamination cannot be ruled out"이라고 밝혔다. Fable 5 수치는 저자 파일의 단일 호출 결과로, 이 하네스에서 돌린 것이 아니다. 모델은 OpenRouter에서 돌렸고, 50회 실행 비용은 $35.79였다.
- **추천 이유:** 작은 오픈 모델 여러 개를 역할 분담시키는 워크플로를 코드 없이 YAML로 시험해 볼 수 있다. 실행 기록과 비용이 모두 저장소에 있다.
- **주의:** 설치 스크립트를 `curl | bash`로 받는다. 먼저 내용을 읽는다.
- **설치/사용:** [team-solve](https://github.com/guregodevo/memdoor/tree/main/examples/workflows/team-solve) (커뮤니티, 자체 측정) · [HN](https://news.ycombinator.com/item?id=50320995) (10-31 22:46 UTC)

### 짧게

- **Microsoft Agent Framework python-1.24.0(10-30 21:52 UTC):** Redis 컨텍스트 저장소가 "[BREAKING]"으로 바뀌어 호환되지 않는 예전 인덱스를 거부한다. 다음 릴리스부터 Python 3.11 이상이 기준이다. 로컬 MCP 도구에 명시적 기본 승인 모드가 생겼다. [릴리스](https://github.com/microsoft/agent-framework/releases/tag/python-1.24.0)
- **OpenAI SDK python 3.42.0·node 7.46.0(10-31 00:2x UTC):** 에이전트 컨텍스트 압축 설정 등 API 타입이 추가됐다. 대응하는 API 변경 로그 항목은 아직 없다. [python 3.42.0](https://github.com/openai/openai-python/releases/tag/v3.42.0)
- **Qwen Code v0.26.5(11-01 03:22 UTC):** Generated UI가 기본으로 켜졌고, `/autofix-pr`이 수정 푸시 3번 뒤에도 실패하면 사람에게 넘긴다. [릴리스](https://github.com/QwenLM/qwen-code/releases/tag/v0.26.5)
- **stack-diagram 스킬(저장소 생성 11-01 05:35 UTC):** 스택을 한 문장으로 설명하면 공식 로고가 들어간 아키텍처 다이어그램을 만든다. `npx skills add caru-ini/stack-diagram`. [GitHub](https://github.com/caru-ini/stack-diagram) (커뮤니티)
- **datasette-llm 0.1a9(10-31):** LLM 도구의 기본 모델을 그대로 써서, 별도 설정 없이 Datasette Agent를 띄운다. [Simon Willison](https://simonwillison.net/2026/Oct/31/datasette-llm/)
- **MCP 레지스트리 첫 게시(공식 원격 서버):** Instatus(`com.instatus/mcp`), PocketSmith(읽기 전용 엔드포인트 별도), ngrok(`com.ngrok/mcp`, 발표 미확인).
- **보안 쪽으로 옮긴 것:** LM Studio·DBHub 권고는 기술 6번이다.

## 주목할 점

- **정액제가 토큰 예산제로 바뀌는 동시에 랩의 자금 조건이 나빠졌다.** OpenAI는 채팅과 Codex를 한 토큰 예산으로 묶고 계정 돌려쓰기를 막았다. Anthropic은 최상위 요금제의 5시간 창을 없앴지만 주간 한도는 그대로 뒀다. 같은 주말 OpenAI는 투자 목표를 낮췄고, Anthropic은 투기 등급 전망이 나왔다. 다음에 볼 것은 OpenAI가 12월 전에 요금제별 토큰 예산을 공개하는지, Anthropic이 Ultra의 주간 한도를 수치로 밝히는지다.
- **AI 키를 쌓아 두는 곳이 가장 큰 표적이 됐고, 스캐너도 뚫렸다.** 개발 플랫폼 9곳 침해, 30분 안의 키 재판매, LLM으로 피해자 값을 매기는 인포스틸러, 우회된 pickle·스킬 스캐너가 같은 창에 나왔다. 다음에 볼 것은 Northflank 침해 경로, LMDeploy 수정 머지, Ubuntu·RHEL의 CVE-2026-98432 수정 커널 배포다.

## 조사 제약

- **본문을 직접 읽지 못한 원문:** openai.com과 help.openai.com은 403이라 r.jina.ai로 읽었다. FT(403)와 WSJ(401)는 CNBC·TechCrunch 인용만 실었다. Bloomberg는 r.jina.ai로 앞부분만 읽었다. The Hacker News·fbi.gov는 curl 403이라 r.jina.ai로 확인했다. The Register Atom 피드가 비어 있었다.
- **시각이 날짜만 있는 원문:** OpenAI 사용량 통합·카드 제한 도움말(날짜 없음), GPT-6 Rosalind와 Opus 4.8 fast mode 폐기(10-30), Apple 중국 뉴스룸(10-31), Parameter Labs(10-28), Anthropic Alignment 글("October 2026"), Google Suncatcher(10-30), ClickHouse 수정(10-30)이다. HN 첫 게시, 매체 시각, 직전 판 조회 기록으로 보강했다. Anthropic 워터마크 이메일과 OpenAI의 Daybreak 두 번째 이메일은 원문을 보지 못했다.
- **확인하지 못한 것:**
  - Anthropic이 게임 복제 거부와 학습 관련 작업 차단에 공식 입장을 냈는지, 그리고 퍼진 "지침 문구"가 실제 문구인지.
  - Ultra의 예전 주간 배수, OpenAI 요금제별 토큰 예산.
  - Northflank 침해 경로, Cursor 마켓플레이스 게시 경로, 2.1.312 추가 뒤의 피해 규모.
  - Anthropic이 opencode의 Claude 구독 로그인 사용을 승인했는지.
  - Binance 피해액은 출처마다 다르다(TRM $6.36B, BBC $6.3bn).
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 스토리 1,484개를 하루씩 나눠 점수순 상위와 키워드로 훑었다.
  - 전역 GHSA: 창 안 reviewed 65건(모두 10-30 게시), unreviewed 326건, malware 30건을 AI·에이전트 키워드로 걸렀고 재현하지 않았다. 주요 AI 저장소 약 65곳의 저장소 권고에는 창 안 게시가 없었다.
  - Hugging Face Daily Papers는 10-31·11-01이 비어 있었다. arXiv는 HN에 오른 것만 봤다.
  - MCP 레지스트리는 창 안 게시 6,815건 중 주요 기업 네임스페이스만 봤다. 중국 랩의 WeChat 공지와 x.com 타임라인은 직접 보지 못했다. Vertex AI 릴리스 노트는 캐시가 묵어 보지 않았다.
- **자사 측정:** Wiz의 키 악용 수치, Anthropic 오정렬 탐색 실험, Prospero의 자원 사용 주장, Memdoor 점수, Talos·CrowdStrike 분석은 모두 각 회사·저자 측정이며 독립 검증 전이다.
