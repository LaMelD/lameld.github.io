---
title: "2026-11-27 AI 브리핑"
date: 2026-11-27T08:00:00+09:00
tags: [ai-briefing, openai, nvidia, security, agents]
description: "추수감사절이라 새 모델은 없었고, OpenAI는 GPT-6.2 과금 실수를 고쳐 Astra·Sol 한도가 40%·25% 빨리 닳게 됐으며, GPT-6 Astra와 쓴 양자중력 논문은 같은 날 반박을 받았고, goose·Crush·Copilot CLI·OpenCode는 파일이나 저장소를 열기만 해도 명령이 돌거나 키가 새던 문제를 고쳤다."
---

> 조사 범위: 2026-11-26 08:00 ~ 2026-11-27 08:00 KST(2026-11-25 23:00 ~ 11-26 23:00 UTC, 미국 수요일 15:00 ~ 목요일 15:00 PST). 직전 브리핑(11-26 판)의 조사 창 종료 시각부터 24시간이다. 미국은 추수감사절(11-26)이라 연방 기관·증시·EDGAR가 쉬었다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`·`merged_at`·커밋 시각, npm·PyPI 게시 시각, Hugging Face API, OpenRouter `created`, X 게시 시각, 상태 페이지 API, 저장소·전역 GHSA `published_at`, CISA KEV JSON, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-26 23시대 UTC) 값이다.

## 오늘의 핵심 요약

- **OpenAI가 GPT-6.2 출시 때의 과금 실수를 고쳤다.** 같은 작업을 해도 Astra는 사용량 한도에 40%, Sol은 25% 더 빨리 닿는다. 창 직후 갱신된 지원 페이지는 12월부터 ChatGPT와 Codex가 한도를 나눠 쓴다고 적었다. Reuters는 미국 칩 규제와 증류 차단 때문에 GLM-5.5·Kimi K4가 내년으로 밀릴 수 있다고 보도했고, Zhipu 쪽은 부인했다.
- **AI가 만든 과학 결과의 운명이 분야에 따라 갈렸다.** 리만 가설 축약판 Lean 증명은 외부 검증을 통과했다. GPT-6 Astra와 몇 달 함께 썼다는 양자중력 논문은 공개 당일 물리학자에게서 "주장이 성립하지 않았다"는 반박을 받았다. 물리에는 Lean 같은 검사기가 없다.
- **에이전트 도구들이 "열기만 해도 실행되는" 문제를 한꺼번에 고쳤다.** goose(high 3건), Crush(API 키 유출), Copilot CLI(MCP 이름으로 셸 승인), OpenCode(웹 가져오기 전체 허용)다. 업그레이드만으로는 이미 받은 세션이나 저장된 승인이 정리되지 않는다. LiteLLM LTS 1.108.19에는 로그인 우회 수정이 빠져 있다.

## 모델 소식

새 프런티어 모델은 없었다. 추수감사절이라 미국 랩의 공식 채널(OpenAI·Google·Anthropic 블로그와 변경 로그, xAI·Meta 뉴스룸)은 창 안 새 항목이 없다. 미국 랩의 공식 발표는 OpenAI 직원이 X에 올린 GPT-6.2 과금 수정 하나다. EDGAR와 미 증시도 쉬어 Anthropic의 SEC 제출과 주가 변동이 없다. 나머지 축은 중국 랩 일정 보도와 NVIDIA 수출 규제, 유럽 규제 당국이다.

### 1. OpenAI가 GPT-6.2 출시 때의 과금 실수를 고쳤다. Astra는 사용량 한도에 40%, Sol은 25% 더 빨리 닿는다 (공식 X, 02:50 UTC, HN 450점대)

- **무엇이 바뀌었나:** Codex 쪽 담당자 Thibault Sottiaux의 글 원문: "We made a billing mistake at launch of GPT-6.2 that incorrectly discounted GPT-6.2 Astra and Sol usage for most users." 고친 결과 원문: "you will reach your limits 40% faster on GPT-6.2 Astra and 25% faster on Sol". 그래도 원문 "You should still see these go further in usage compared to 6 Astra and 6.1 Sol."이라고 했다. 과거 사용분은 원문 "we won't charge you for the difference on past usage"이고, 보상으로 원문 "one banked reset for everyone on Go, Plus, Pro, and Business"를 준다.
- **함께 볼 것(창 직후):** 직전 판 4번에서 찾지 못한 지원 페이지가 나왔다. 원문: "Starting in December, Plus, Pro, and Business plans' usage allowance will be shared across ChatGPT and Codex as we bring the agentic capabilities of Work into the main ChatGPT experience." 원문 "Instant chat will not draw from your usage." 그리고 원문 "A small group of users may see the usage changes before December". 이 페이지는 창이 끝난 뒤(23:14 UTC 이후) 갱신돼 무엇이 바뀌었는지는 확인하지 못했다. **Codex를 많이 쓰는 개발자는 12월부터 ChatGPT 사용분과 한도를 나눠 쓰게 된다.**
- **외부 평가는 엇갈린다:** SemiAnalysis의 자체 벤치마크 GDPx 갱신(00:05 UTC) 원문: "6.2 Astra ranks first with a score of 19.3%. Fable 5.2 ranks second at 16.4%". 하지만 원문 "Notably, 6.2 Astra is lackluster at software engineering"이고 결론은 원문 "Overall, we believe the current negative sentiment around OpenAI's models is overblown."다. 반대로 한 개인 블로그(HN 200점대)는 자체 대결 데이터로 원문 "Astra and Sol appear to be cost tuned compared to their predecessors, and to a degree that makes them underthinking outliers"라고 주장했다.
- **왜 중요한가:** 출시 9일 만에 체감 한도가 크게 줄었고, 12월 공유 한도가 곧 겹친다. The Decoder는 Business 사용자에게 연말까지 Fast Mode를 무료로 준다고 전했지만 원 글에서 찾지 못했다(미확인).

[X 글](https://x.com/thsottiaux/status/2125784147889393806) · [지원 페이지](https://help.openai.com/en/articles/20001625-understanding-chatgpt-and-codex-usage-limits) · [SemiAnalysis](https://newsletter.semianalysis.com/p/evaluating-recent-model-releases) · [블로그](https://micjm.com/blog/openai-is-in-cost-control-mode/) · [HN](https://news.ycombinator.com/item?id=50651117)

### 2. (직전 판 5번 후속) Reuters: 미국 칩 규제와 증류 차단으로 GLM-5.5는 2027년 1분기, Kimi K4는 2027년 중반으로 밀릴 수 있다. Zhipu 쪽은 부인했다 (매체보도, Reuters 07시대 UTC)

- **보도:** Reuters 원문은 열지 못해 전재본으로 확인했다. The Decoder(15:45 UTC) 원문: "GLM-5.5 model, planned for November, is likely to slip into the first quarter of 2027, Reuters reports, citing sources". Moonshot은 원문 "Moonshot AI's development cycle could double from six to 12 months"이고, 원문 "Moonshot is now turning to the latest models from Google and Meta for training"이라고 했다. OpenAI·Anthropic 출력이 막히자 다른 회사 모델로 옮겨 간다는 뜻이다.
- **반박:** Zhipu 임원 양하오이(杨昊翊)가 개인 WeChat에 원문 "今天下午的所谓爆料新闻都是假消息"(오늘 오후의 이른바 폭로 기사는 모두 가짜 뉴스)라고 썼다고 快科技가 보도했다(14:07 UTC). 같은 기사도 원문 "虽然他没有公布进度"(진도는 밝히지 않았다)라고 적었다. "12월 출시"는 Zhipu 공식 발표가 아니라 중국 증권사 노트에서 나왔다. 노트 원문: "我们预测GLM5.5将在12月发布"(우리는 GLM-5.5가 12월에 나올 것으로 예측한다). Z.ai 공식 계정의 입장은 찾지 못했다.
- **같은 축:** MiniMax CEO가 사내 회의에서 국산 칩 도입을 서두르겠다고 했다는 보도도 나왔다(快科技 02:13 UTC, 1차 출처 미확인). 원문 "公司将加快引入华为昇腾和阿里平头哥等国产AI芯片"(화웨이 Ascend·알리바바 T-Head 등 국산 AI 칩 도입을 가속한다).
- **왜 중요한가:** 직전 판의 "중국 랩이 늦어진다" 보도에 처음으로 구체적인 모델 일정이 붙었다. 다만 양쪽 모두 익명이거나 간접 출처다. 12월에 GLM-5.5가 나오는지가 이 보도를 판정한다.

[The Decoder](https://the-decoder.com/after-openai-and-anthropic-shut-the-door-on-their-data-a-chinese-ai-developer-is-turning-to-google-and-meta-instead/) · [Reuters](https://www.reuters.com/world/china/us-chip-curbs-distillation-crackdown-threaten-chinese-ai-model-development-2026-11-26/) · [快科技(반박)](https://news.mydrivers.com/1/1168/1168924.htm) · [증권사 노트](https://x.com/dmjk001/status/2125944480264258027)

### 3. (직전 판 6번 후속) NVIDIA가 동남아 4개국 서버 출하를 멈췄고, 클라우드 업체에는 "중국 임차인이 하나라도 있으면 못 판다"는 조건을 걸었다 (매체보도, FT 05:00 UTC·CNBC 15:34 UTC)

- **출하 중단(FT, The Decoder 전재):** 원문 "Nvidia has told manufacturing partners such as Dell and Supermicro to stop shipping servers to Malaysia, Singapore, Thailand, and Indonesia". 원문 "It isn't clear whether the pause is only temporary". 여파로 원문 "Banks have halted about $500 million in loans to Singapore-based cloud provider Aolani".
- **판매 조건(CNBC):** 원문 "Neoclouds are being told they won't be able to purchase Nvidia chips if any of the computing capacity will be rented by Chinese companies, sources told CNBC". 이전 기준은 원문 "Nvidia had said it would not sell its chips to customers renting more than 50% of the graphics processing units"였다.
- **왜 중요한가:** 직전 판 5번의 원격 GPU 접근 규제가 실제 공급망에서 움직이기 시작했다. 동남아 GPU 클라우드를 쓰는 개발자는 용량과 가격 변동을 볼 수 있다. Huang 하원 비공개 인터뷰(12-01)와 상원 청문회(12-03) 일정은 그대로다.

[The Decoder](https://the-decoder.com/nvidia-halts-server-shipments-to-malaysia-singapore-thailand-and-indonesia-after-tighter-us-export-controls/) · [FT](https://www.ft.com/content/7838a715-07d4-4605-aa60-21f5af2b2370) · [CNBC](https://www.cnbc.com/2026/11/26/neoclouds-nvidia-china-ai.html)

### 4. 영란은행 금융안정보고서가 프런티어 AI의 암호 위협과 오픈 웨이트 위험을 적었다 (공식, 11-26)

- **보고서 원문:** "through the use of frontier AI to exploit cryptographic vulnerabilities". 오픈 웨이트에 대해서는 원문 "open-weight AI models that could be run more cheaply than proprietary models, with safeguards more easily disabled or removed".
- **왜 중요한가:** 직전 판의 수학 결과와 "인수분해" 루머가 돈 직후, 중앙은행이 미공개 모델의 능력을 금융 안정 위험으로 올렸다. FT 기사 제목은 "Andrew Bailey says unreleased AI models have raised encryption concerns"다. 본문은 읽지 못해 Bailey의 실제 발언은 확인하지 못했다.

[영란은행](https://www.bankofengland.co.uk/financial-stability-report/2026/november-2026) · [FT](https://www.ft.com/content/450920b9-82b8-4a66-960b-5740aa7f94ae)

### 5. Meta Muse 영국 후속: 금융감독청(FCA)이 샌드박스 공동 시험을 제안했지만 Meta가 거절했다 (매체보도, WIRED 10:51 UTC)

- **원문:** "Meta declined a suggestion from the UK's financial regulator last week that they joint-test Muse using their sandbox". FCA 쪽 Colin Payne은 에이전트를 두고 원문 "It's another force of attack, agents hitting the streets, most of them coming from the US"라고 했다. Meta 원문: "We've engaged constructively with the appropriate regulatory authorities and will continue to do so".
- **왜 중요한가:** 개인정보 감독청(ICO)에 이어 금융 감독 당국도 Muse를 문제 삼았다. 결제·쇼핑을 대신하는 에이전트가 금융 규제 안으로 들어오고 있다.

[WIRED](https://www.wired.com/story/meta-refused-to-share-muse-for-testing-uk-financial-watchdog-complains/)

### 6. 오픈 웨이트를 고르는 기준이 정치가 됐다: Thomson Reuters는 GLM·Gemma로, 에스토니아는 DeepSeek·Qwen에 "적색" (매체보도)

- **Thomson Reuters(Business Insider 보도, Dataconomy 13:12 UTC 요약):** 원문 "Thomson 1.1 will use Z.ai's GLM model and Google's Gemma". 1.0은 Qwen 기반이었다. 원문 "Less than 5% of AI tokens used at the company currently come from its own models".
- **에스토니아(ERR 10:04 UTC):** 정부가 AI 서비스에 등급을 매겼다. 원문 "handed red ratings to Chinese platforms DeepSeek and Qwen alongside Botastico". 원문 "Google's Gemini, Microsoft's Copilot and Europe-developed Mistral Vibe earned green ratings".
- **왜 중요한가:** 같은 중국 오픈 웨이트라도 기업은 성능과 비용으로 고르고, 정부는 서비스 사용을 막는다. 오픈 웨이트를 직접 서빙하는 것과 중국 회사의 앱·API를 쓰는 것은 다른 문제로 다뤄진다.

[Dataconomy](https://dataconomy.com/2026/11/26/thomson-reuters-to-launch-ai-model-built-on-chinese-and-us/) · [ERR](https://news.err.ee/1610206915/estonia-warns-public-to-avoid-deepseek-qwen-and-botastico-ai-platforms)

### 7. (한국) Upstage Solar Mini 4.5는 12-03 정식 출시, 정가는 프리뷰가의 10배다. 독자 AI 파운데이션 모델 3차 평가가 바뀐다 (공식 문서·매체보도)

- **(직전 판 8번 후속·정정) Upstage Solar Mini 4.5:** 공식 문서를 찾았다. 원문 "Solar Mini 4.5 will be officially released on December 3, 2026". 정가는 원문 "$0.10 input / $0.40 output / $0.01 cached per 1M tokens"다. **직전 판이 "Solar Mini 4의 5분의 1"이라고 쓴 $0.01/$0.04는 프리뷰 할인가였다.** 정가는 Solar Mini 4($0.05/$0.20)의 두 배다. 원문 "all requests to solar-mini-4-5-preview will be automatically routed to solar-mini-4-5"라 프리뷰 ID를 쓰는 코드는 12-03 이후 비용이 바뀐다. [문서](https://console.upstage.ai/docs/models/solar-mini-4-5-preview)
- **독자 AI 파운데이션 모델 3차 평가(서울경제 06:50 UTC):** 원문 "기존 평가의 큰 틀을 유지하되 전문가 평가를 한층 심층적으로 진행하고 사용자 평가의 공정성을 높이는 방향으로 평가 체계를 개선할 방침"이다. 공공 서비스 '모두의 AI'는 원문 "SK텔레콤, KT, 카카오 등 3개 컨소시엄" 챗봇으로 시작한다. [서울경제](https://www.sedaily.com/article/20119418)
- **공정위가 Apple Intelligence 광고 건을 정식 심의에 올렸다(MacRumors 11:19 UTC):** Apple 원문 "We have cooperated with the KFTC's investigation and disagree with the conclusions in the Examiner's Report." [MacRumors](https://www.macrumors.com/2026/11/26/apple-korea-regulator-iphone-16-ai-advertising/)
- SKT는 A.X-K2 NVFP4 판을 다시 올렸다(12:06 UTC). Naver·Kakao·KT·LG·NCSOFT·Motif는 새 모델 소식이 없다.

### 8. 장애 (공식 상태 페이지)

- **OpenAI:** Codex의 GitHub 리뷰 오류가 14:07 UTC에 해결됐다. 원문 "The GitHub service disruption has been resolved". [상태](https://status.openai.com/incidents/01M89RGC3STKK2MT53TXHFMY4H) 창 안 새 인시던트는 없다.
- **Claude:** 창 안 새 인시던트는 없다. 직전 판의 Mac용 Microsoft 365 추가 기능 장애는 11-25 20:21 UTC "identified" 이후 갱신이 없고 조회 시점까지 열려 있다. [상태](https://stspg.io/ffdq2qfv8frk)

### 9. 짧게

- **42개 주 법무장관이 Character.AI의 대화 기록 매각을 막으려 한다(The Verge 09:54 UTC):** 원문 "the private chat histories of more than 21 million U.S. accounts — more than 2.1 million of them registered to minors". [The Verge](https://www.theverge.com/policy/1042620/character-ai-sale-state-ags-block-selling-users-chats)
- **MiniMax Talkie가 App Store에서 내려갔다(9to5Mac 02:12 UTC, 첫 보도는 창 경계):** 원문 "Apple has pulled MiniMax's chatbot Talkie from the App Store worldwide". 아동 안전 우려가 이유로 보도됐다. [9to5Mac](https://9to5mac.com/2026/11/25/apple-pulls-popular-chinese-ai-chatbot-from-the-app-store-amid-child-safety-concerns/)
- **오픈 웨이트:**
  - inclusionAI Ming-Image-0.2-Design(6B 이미지 생성, MIT): 첫 토론 글로 보아 11-26에 공개된 것으로 추정한다. README의 "#1 among open-weight models overall"은 회사 주장이다. [HF](https://huggingface.co/inclusionAI/Ming-Image-0.2-Design)
  - Tencent AngelSlim DFly2: Hy4-flash·DeepSeek V4.1 Flash·GLM-5.3-Flash용 추측 디코딩 드래프터다. 원문 "The drafter keeps no KV cache of its own". vLLM 지원은 원문 "support in progress"다. [HF](https://huggingface.co/AngelSlim/Hy4-flash-DFly2-B6)
  - Microsoft CogACT2(로봇 VLA, 저장소 4개, 15:53 UTC): 가중치만 있고 설명이 없다. [HF](https://huggingface.co/microsoft/CogACT2-LIBERO)
- **OpenRouter 창 안 신규:** inclusionai/lingsante-3.1-air-vl(15:01 UTC, $0.06/$0.18). [OpenRouter](https://openrouter.ai/inclusionai/lingsante-3.1-air-vl)
- **(직전 판 정정) Claude Duet:** 공식 페이지 원문 "Available in Pro, Max, and Ultra plans in the US, India, Indonesia, Brazil & Japan", 그리고 "Team and Enterprise plans are not included yet". 직전 판이 옮긴 AIM의 "Team 공개 베타" 보도와 어긋난다. 페이지 갱신 시각은 확인하지 못했다. [Claude Duet](https://claude.com/duet)
- **(직전 판 후속·정정) Qwen4-Max-Preview는 종료됐다.** 원문 "qwen4-max-preview was retired at 10:00 on November 25, 2026 (UTC+8)". 11-25 02:00 UTC다. 직전 두 판이 적은 "16:00 UTC"는 틀렸다. 원래 ID는 qwen4-max로 넘어가고 원문 "The preview discount no longer applies". [QwenCloud](https://docs.qwencloud.com/developer-guides/getting-started/latest-model)
- **직전 판 후속 중 진전 없음:** Anthropic 상장(창 안 SEC 제출 없음, 휴장), Axios·Dario(재보도만), OpenAI Private Inference(문서 없음), GLM-5.4(API는 보안 평가 뒤 공개 문구 그대로, 가중치 401), 가중치 401(Qwen4-Max, Ling 3.2 Flash, K-EXAONE 2.5, GLM-5.4), Siren 외부 반응, SWE-2.1 외부 평가, METR, Gemini 4 Neon 안전 보고서, DeepSeek V5, Cursor 중국 모델 퇴출, NVIDIA–Inception. 변경 로그는 Claude Platform(11-23)·Claude 앱(11-24)·OpenAI API(11-24)·ChatGPT(11-23)·Gemini API(11-19)·Gemini 앱(11-17)·xAI(11-19)·DeepSeek(11-13) 모두 새 항목이 없다.

### 참고: 창 경계 항목

- **(WaPo, 첫 전재 11-25 22:58 UTC) 국방부가 Huang 복귀를 위해 국방물자생산법(DPA)을 검토했다는 보도:** Gizmodo 원문 "Essentially they could use the DPA to force Nvidia to shift more of its production to military projects". 국방부가 부인했다는 보도는 제목으로만 확인했다. [Gizmodo](https://gizmodo.com/the-pentagon-reportedly-had-a-weird-plan-to-make-nvidia-rehire-jensen-huang-2000845732)
- **(testingcatalog 11-25 16:31 UTC, 미확인) "Opus 5.5 Extra" 경로로 Opus 5.6을 쓰고 있다는 시험 사용자 주장:** 기사도 원문 "Of course, Anthropic hasn't announced anything"이라고 적었다. 루머로만 둔다. [HN](https://news.ycombinator.com/item?id=50655798)
- **(커뮤니티) Anthropic 사옥 앞 단식 농성(X 11-25 20:33 UTC, HN 50점대)과 "Claude가 연구를 단계마다 방해한다"는 개발자 글(X 11-24 23:28 UTC, 증거 미제시):** 둘 다 창 밖 게시물이 창 안 HN에 올랐다. [HN](https://news.ycombinator.com/item?id=50657110) · [HN](https://news.ycombinator.com/item?id=50651552)

## 기술 이슈

추수감사절이라 미국 쪽 공식 채널이 거의 멈췄다. GitHub reviewed 권고는 창 안에 0건이고 CISA KEV 추가도 0건이다(KEV `dateReleased`는 11-25 19:48 UTC 그대로). Node.js·Chrome·Go·Rails·GitLab·nginx·커널 stable도 창 안 보안 릴리스가 없다. arXiv는 11-26(목)을 휴일로 정해 11-27 공지분이 없다. 창 안의 큰 흐름은 AI가 만든 과학 결과의 후속이다. 수학 쪽 하나는 외부 검증을 마쳤고, 물리 쪽 하나는 같은 날 전문가 반박을 받았다. 직전 판이 놓친 11-25 보안 발표(Microsoft ARTEX, Drupal, Exim)는 창 안 재보도가 많아 이 섹션 끝 창 경계 항목에 묶었다.

### 1. GPT-6 Astra와 몇 달 함께 썼다는 양자중력 논문이 arXiv에 올라왔고, 같은 날 물리학자 Oppenheim이 "주장이 성립하지 않았다"고 반박했다 (논문은 arXiv 11-26 01:00 UTC 공지, 반박은 전문가 블로그 12:56 UTC, HN 190점대)

- **무엇이 나왔나:** Venkatesa Chandrasekaran(소속 Stanford 표기)의 단독 논문 "An algebraic quantization of gravity from the null cone"(arXiv 2611.34924)이다. 11-25에 제출돼 11-26 공지분에 실렸다. 감사의 글 원문: "I also thank Sam Altman, Sebastien Bubeck, and my OpenAI colleagues for encouragement throughout the project." 이어서 "Several of the calculations and proofs in this paper were developed through detailed, back and forth collaboration with GPT-6 Astra over the course of many months"이고, "400M total tokens spent; I thank OpenAI for providing this compute."라고 적었다.
- **반박:** Jonathan Oppenheim은 Claude 에이전트 여러 개로 정준 양자화의 알려진 난점을 뽑아 논문과 대조했다. 원문: "The swarm listed 21. The paper solves one (a minor one, by a standard trick), sidesteps 7, assumes its way past 6, and doesn't mention the other 7." 결론 원문: "I've looked over enough of it to be confident that the paper's claims are not established, although there may well be mistakes in the details." 그는 글에서 "OpenAI's president Greg Brockman shared it"라고도 적었다.
- **왜 중요한가:** 직전 판들의 수학 결과는 Lean이라는 기계 검사기가 있었다. 물리에는 그런 장치가 없다. Oppenheim 원문: "There is nothing like that for physics, so we will keep getting false alarms like this one unless we find other ways to separate signal from slop." 회사 쪽 사람이 퍼뜨린 결과를 같은 날 다른 회사 모델로 점검했다는 점도 눈에 띈다. 동료 심사는 없고, 지금은 "주장"으로 읽는다.

[arXiv](https://arxiv.org/abs/2611.34924) · [Oppenheim](https://superposer.substack.com/p/has-ai-come-for-quantum-gravity) · [HN](https://news.ycombinator.com/item?id=50651863)

### 2. (직전 판 6번 후속) 리만 가설 축약판 Lean 증명도 외부 검증을 통과했다. OpenAI·Google 수학 저장소는 조용하다 (공식 검증자 문서 20:46 UTC, 전문가 블로그)

- **리만 축약판:** 직전 판은 "축약판의 외부 Comparator 기록은 아직 없다"고 적었다. Sutherland의 축약판 검증 페이지 원문: "Page last updated 2026-11-26 20:46 UTC. All checks have finished. This page concerns the shorter proof only." 그리고 "lake comparator --paranoid (Lean v4.35.0) reports "Your solution is okay!" for both editions."다. 다만 두 번째 기계 검증 하나는 끝나지 않았다. 원문: "lean4lean needs 332 GB of memory for that edition, more than the smaller machine has, so the run there was stopped without a verdict".
- **Aaronson:** 직전 판 3번의 글 본문에 "Updates (Nov. 26)"를 붙였다. 원문: "Yes, I do now think that AI is climbing the mountain that leads to P≠NP." "인수분해가 P로 풀렸다"는 루머를 묻는 댓글에는 원문 "Ironically, a proof of P≠NP would open up the possibility, for the first time ever, of proving that RSA or other cryptosystems _are_ secure!"라고 답했다. 루머를 뒷받침하는 새 근거는 없다.
- **AI 결과 위에 쌓은 AI 결과:** 개인 연구자가 "NP_R ≠ PAR_R"(실수 위 계산 모델의 분리)을 Lean 약 3만 6천 줄과 함께 올렸다(HN 120점대). README 원문: "The results in this repository grew out of a conversation between Samuel Everett and Claude Fable 5.2." 핵심 하한은 Anthropic의 "VP ≠ VNP over every field" 원고에서 가설로 가져왔다. README 원문: "The results have not been peer reviewed and should be read with that in mind."
- **OpenAI·Google:** `openai/math`는 11-25 00:18 UTC 병합 뒤 새 커밋·PR이 없고, 11-24 묶음 전체를 외부에서 빌드한 기록도 찾지 못했다. 결과를 만든 모델은 여전히 "unnamed internal OpenAI model"이다. Google Cogentic은 인용을 더한 커밋 하나(11-25 23:54 UTC)뿐이고 Lean은 공개되지 않았다. HN에 다시 오른 인수분해 글(137점)은 직전 판과 같은 커밋이다.

[축약판 검증](https://math.mit.edu/~drew/shortgrh/) · [Aaronson](https://scottaaronson.blog/?p=10379) · [NP_R ≠ PAR_R](https://github.com/samueleverett01/bss-permanent-separations) · [HN](https://news.ycombinator.com/item?id=50657059)

### 3. 에이전트·워크플로 도구 권고: goose high 3건, Langflow critical 1건(소급 공개), n8n 샌드박스 탈출 (공식 저장소 권고)

- **개수(직접 셈):** 추적하는 저장소의 창 안 권고는 goose 3, n8n 4, Langflow 1이다. 나머지는 0건이다.
- **goose(01:34 UTC):** 세션 파일·저장소를 여는 것만으로 명령이 실행되는 high 3건이다. 업그레이드와 정리 방법은 도구 1번에 정리했다. 세 권고 모두 `ANT-2026-…` 형식의 보고 식별자를 달았지만 발견자는 적혀 있지 않다.
- **Langflow GHSA-fjgc(CVE-2026-9103, 9.8, 11:51 UTC):** 원문: "because `AUTO_LOGIN` defaulted to `True`, every default deployment exposed it, allowing any network-reachable attacker to obtain full superuser access." 1.13.0(10-22 릴리스)에서 기본값이 바뀐 뒤의 소급 공개다. 1.13.0 미만이면 노출되고, `LANGFLOW_AUTO_LOGIN=true`를 명시한 배포는 지금도 같은 동작으로 보인다. 권고의 패키지 생태계가 pip가 아니라 npm으로 붙어 있어 Python 의존성 스캐너는 잡지 못할 수 있다.
- **n8n(08:17~08:19 UTC, high 1·medium 3):** high GHSA-w6f8 원문: "A flaw in how the V8 engine bundled with n8n internalized certain strings let a crafted expression corrupt memory and escape that isolate." 결과는 원문 "run arbitrary code in the main n8n process, with the privileges of the n8n service user"다. 워크플로를 만들 수 있는 사용자는 곧 서버 권한을 가진다. 고친 판은 2.49.2·3.4.0이다. medium 3건(Chroma Cloud 계정 노출, 편집기로 가는 비마스킹 오류, WebSocket 파일 디스크립터 고갈)은 2.49.7·2.50.2·3.4.1·1.123.102까지 올려야 모두 고쳐진다.
- **전역 미검토 권고 중 AI 관련:** llama.cpp RPC 서버의 힙 쓰기 CVE-2026-137567(9.8, 15:31 UTC). 원문: "allows unauthenticated attackers to write to arbitrary heap addresses via zero-element destination tensors". b12658에서 고쳐졌고 RPC 서버를 띄운 경우에만 해당한다. 이 밖에 markdownify-mcp SSRF, WordPress MCP 플러그인 2건, torchvision `_extract_tar` 경로 조작이 있다. vLLM CVE 3건은 11월 초 저장소 권고의 CVE 미러다.

[goose 권고](https://github.com/aaif-goose/goose/security/advisories/GHSA-5pvv-4hwj-j3mj) · [Langflow](https://github.com/langflow-ai/langflow/security/advisories/GHSA-fjgc-vj2f-77hm) · [n8n](https://github.com/n8n-io/n8n/security/advisories/GHSA-w6f8-8qh4-h3j4) · [llama.cpp](https://github.com/advisories/GHSA-8fqh-8p37-cc5h)

### 4. (직전 판 5번 후속) NFS2ROOT(CVE-2026-99362): 배포판 패치는 여전히 없다. 커뮤니티 탐지와 비공식 라이브패치가 나왔다 (공식 트래커·커뮤니티)

- **배포판:** Ubuntu는 focal·jammy·noble의 `linux`와 aws·azure·gcp 커널이 `needed` 그대로이고 USN이 없다. Red Hat은 위협 등급을 Critical로 매겼고 RHEL 6~10이 모두 Affected다(권고 없음). Debian bookworm 기본 커널은 취약하고 `linux-6.12` 백포트만 고쳐졌다. KEV에는 없다.
- **커뮤니티:** Zeek 탐지 패키지(`initconf/nfs2root`, 01:06 UTC)와 TrueNAS용 비공식 라이브패치(08:48 UTC)가 나왔다. 라이브패치 원문 "until iXsystems ships a fixed kernel"이니 TrueNAS도 아직 미패치다. 공개 PoC는 아직 없다.
- **할 일:** 패치가 나올 때까지 NFS 서버 포트를 신뢰하는 네트워크로만 묶는다.

[Ubuntu CVE](https://ubuntu.com/security/CVE-2026-99362) · [TrueNAS 라이브패치](https://github.com/ChanningHe/truenas-nfs2root-klp)

### 5. 인터넷 전역 보안(AI 무관): Xen XSA-535, PowerDNS (공식 메일링 리스트)

- **Xen XSA-535(16:14 UTC):** 원문 "A guest can retain access to memory it no longer owns." 그리고 "There is no known mitigation." Xen 4.7 이상 x86의 HAP를 쓰는 HVM·PVH 게스트가 대상이다. 클라우드 호스트를 직접 운영한다면 패치한다.
- **PowerDNS:** Authoritative 4.9.19·5.0.9·5.1.6(13:06 UTC)이 High DoS CVE-2026-123758을 고쳤다. dnsdist 2.0.12·2.1.4(10:14 UTC)도 나왔다.

[XSA-535](https://seclists.org/oss-sec/2026/q4/900) · [PowerDNS](https://seclists.org/oss-sec/2026/q4/898)

### 6. 논문 (HF Daily Papers 11-26분 57편, arXiv 11-26 01:00 UTC 공지)

수치는 초록에서 옮겼고 재현하지 않았다.

- **Mixture-of-Loops Transformer(ByteDance Seed):** 같은 층을 반복해 쓰는 구조다. 원문: "In 400B-token pretraining, U-MoLT outperforms OLMo3 across 700M/1.5B dense and 3B MoE models, achieving 1.7x validation cross-entropy (CE) token efficiency at 700M". [HF](https://huggingface.co/papers/2611.34923)
- **Letting Models Manage Their Own Context(NVIDIA):** 모델이 자기 컨텍스트를 직접 정리하게 했다. 원문: "GPT 6.1 nearly triples its score at a 32K budget on OfficeQA Pro v2". 에이전트 컨텍스트 압축을 고민한다면 읽어 볼 만하다. [HF](https://huggingface.co/papers/2611.34147)
- **Single-Database Rewards Are Hackable:** Text-to-SQL 강화학습에서 DB 하나로 매긴 보상은 해킹된다. 반사실 실행으로 원문 "of reward hacks from 54.1% to 1.9%, rejecting 0 of 2,847 correct queries"까지 줄였다. [HF](https://huggingface.co/papers/2611.34363)
- **DyLoMo-DiT:** 원문 "a scaled DyLoMo-DiT achieves a higher GenEval score than FLUX.1 (0.714 vs. 0.673), with less than one tenth of its active parameters". [HF](https://huggingface.co/papers/2611.30844)
- **보안:** Windows 커널 바이너리 12개에서 에이전트가 원문 "146 distinct previously unknown vulnerabilities"를 찾았다는 벤치마크(DeepBinary). [arXiv](https://arxiv.org/abs/2611.33486) 에이전트가 오류에서 복구할 때 원문 "silently discarding the security protections required by the original workflow"를 보인 연구. [arXiv](https://arxiv.org/abs/2611.33605) 오픈소스 프로젝트 513개의 LLM API 무임승차 측정. [arXiv](https://arxiv.org/abs/2611.33218)

### 7. 짧게

- **(직전 판 4번 후속) 1Password:** 공식 글 상단에 원문 "Update: We will be providing a technical blog with details in the coming weeks once our full investigation is complete."가 붙었다(11-25 20:10 UTC Wayback 사본에는 없음). 피해 규모, 침투 경로, CLI·SDK·Connect 영향은 여전히 없다. 창 안 THN·The Register의 "소스 코드 탈취" 기사는 재보도다. [1Password](https://1password.com/blog/update-on-nov-2026-security-incident)
- **PolinRider(SafeDep 11-24, THN 15:13 UTC):** 북한 연계로 알려진 로더가 Python 저장소로 번졌다. 원문: "On 24 November 2026, SafeDep found the loader in 375 public repositories that belong to 260 accounts. No package on PyPI or npm carries it." 감염 경로 원문 "pytest runs the loader through conftest.py". 에이전트가 낯선 저장소를 클론해 테스트만 돌려도 실행된다. THN 제목의 7,600개는 다른 보안 업체의 추정치이고 원문을 읽지 못했다. [SafeDep](https://safedep.io/blog/polinrider-python-loader-ethereum-calldata)
- **아이슬란드 국가 대응 계획 발동(13:00 UTC):** 원문 "The activation follows a serious attack on hosting provider 1984." 레이캬비크시와 Blue Lagoon 예약 시스템이 피해를 입었다. [Iceland Review](https://www.icelandreview.com/news/iceland-cyberattack-critical-infrastructure-response-plan/)
- **GitHub Git LFS 저하(11-25 21:30 ~ 11-26 00:28 UTC):** 원문 "an average of 2% of Git LFS requests failed, peaking at 4%". 11-25 대규모 장애의 RCA는 아직 없다. [githubstatus](https://www.githubstatus.com/history)
- **전역 GHSA(직접 셈):** reviewed 0건(직전 판 8), unreviewed 543건(582), malware 56건(77). 휴일로 검토가 멈춘 것으로 보인다. malware 중 RubyGems 15건은 모두 bundler·rake·json 같은 이름을 흉내 낸 타이포스쿼트다.
- **커뮤니티:** AI로 완치된 질병 수를 0으로 보여 주는 냉소 사이트 "Tracking Diseases Cured by AI"(HN 290점대). [HN](https://news.ycombinator.com/item?id=50657307)
- **직전 판 후속 중 진전 없음:** Ruby2ßhell(Veria 페이지 11-21 이후 그대로, 공식 PoC 없음), MiniMax 추가 공개, ZDI PyTorch·DeepSpeed, Milvus 수정판, LiteLLM CVE-2026-93355·LMCache·crewAI 새 권고, PAN-OS Fragnesia, FinCEN·CIA, Azure APIM, Zstandard(Ubuntu `needs-triage`), OpenSSL(12-01 예고 그대로), Microsoft agent-framework·ONNX Runtime 저장소(404), vLLM 부채널, Defang, Hex.pm.

### 참고: 창 경계 항목

- **(Microsoft 11-25 18:09 UTC, 직전 판이 놓침, 창 안 The Decoder·The Register 재보도) Microsoft가 ARTEX를 쓰는 행위자 목록과 방어 지침을 냈다.** 직전 판들의 Fireworks·ARTEX 사고와 이탈리아 경보의 후속이다.
  - 원문: "As of November 2026, Microsoft Threat Intelligence has observed both state-sponsored and financially motivated threat actors such as Storm-0940, Storm-3358, Onyx Sleet, Pearl Sleet, Crystal Typhoon, and Salt Typhoon integrating ARTEX into their tooling."
  - 새 공격 기법은 아니다. 원문: "ARTEX does not introduce new attack techniques; it uses common, pre-existing penetration testing tools and techniques." 대신 속도가 문제다. 원문: "In one Storm-0940 campaign that used ARTEX, Microsoft recorded more than 151,000 failed sign-ins, peaking at 2,900 failures per minute."
  - 출처 주장: 원문 "the China-based cybersecurity company ThreatBook (微步在线) claimed ownership of ARTEX and began offering commercial access to "ARTEX Pro"".
  - 방어: User-Agent `artex-enrich/*`·`norma/*` 탐지와 에이전트를 속이는 기만 기법을 권했다. 원문: "Fake wins present convincing evidence that the agent's objective is complete, which can cause it to stop on a false success."
  - [Microsoft](https://www.microsoft.com/en-us/security/blog/2026/11/25/what-defenders-need-to-know-artex-agentic-ai-framework-threat-actors-abuse/)
- **(Accenture 11-24, SecurityWeek 11-26 11:35 UTC) Mitel MiCollab 0-day 악용에 상용 AI API가 쓰였다는 분석:** 직전 판 KEV 10건 중 MiCollab 5건의 배경이다. 원문: "Between 7 and 17 November 2026, sampled network traffic showed the server communicating with two commercial AI API services over a combined period of 155 hours." 다만 원문 "Because the telemetry is sampled, APEX Labs cannot determine precisely how the threat actor used the services." 웹셸은 패치(11-16) 전인 원문 "before 23:00 UTC on 15 November 2026"에 심어졌다. 패치했어도 침해 점검을 따로 한다. [Accenture](https://accenture.github.io/cyber-intelligence/micollab-exploitation/) · [SecurityWeek](https://www.securityweek.com/mitel-micollab-zero-days-exploited-in-suspected-ai-assisted-attacks/)
- **(Drupal 11-25 17:04 UTC, 직전 판이 놓침) SA-CORE-2026-019(CVE-2026-136716):** 11-18 설치기 패치 뒤에도 빈틈이 남았다. 원문: "certain sites in unusual or incomplete install states remain vulnerable to site takeover following that advisory, including production sites." 그리고 "This is being exploited in the wild." 화면으로는 취약 여부를 알 수 없다(원문 "It is not apparent in the user interface if a site is in the affected vulnerable state"). 11.4.14·11.3.23·10.6.23으로 올리고 원문대로 "An update hook must run to fix existing production sites in this state"이니 `update.php`를 돌린다. user 1 계정의 이메일·비밀번호도 점검한다. KEV에 오른 11-18 건(조치 기한 11-27)과는 다른 CVE이고 조회 시점까지 KEV에 없다. [Drupal](https://www.drupal.org/sa-core-2026-019)
- **(Exim 공지 11-25 13:04 UTC, oss-security 전달 15:58 UTC) Exim 4.100.2:** 원문 "A remote attacker can cause a write outside an allocated area, followed by an expansion operation done with root privilege." Exim은 Medium으로 매겼다. 발견자 칸 원문은 "The unnamed and uncredited authors whose works were ingested as the training corpus"다. AI 도구로 찾은 버그임을 비꼰 표기로 읽힌다. [oss-security](https://seclists.org/oss-sec/2026/q4/899)
- **(창 경계, 11-25 03:51 UTC 트윗) Virginia Vassilevska Williams가 직사각 행렬곱 지수도 최적이라는 노트를 냈다:** 원문: "Apparently omega=9/4. Also, you can show that the rectangular matrix multiplication exponents are also optimal!" 노트 저자 표기에는 "(via chat-GPT+Claude etc!)"가 붙었다. 같은 날 답글에서 범위를 좁혔다. 원문: "We have not ruled out such an algorithm. The only approach ruled out is using algorithms that stay in the arithmetic circuit model". **정정:** 직전 판 기술 1번은 "행렬곱이 n^2에 가깝게 빨라질 수 있다는 오랜 기대(ω = 2)가 틀린 셈"이라고 썼다. 이 서술은 산술 회로 모델 안에서만 맞다. 그 밖의 알고리즘으로 n^2에 가까워질 가능성은 열려 있다. [트윗](https://x.com/pinkfloydie/status/2125614809240539328) · [노트](https://people.csail.mit.edu/virgi/rectangularmultopt.pdf)

## 써볼 만한 도구

추수감사절이라 Claude Code, Codex 정식판, Gemini CLI, VS Code, Cursor는 새 릴리스가 없었다. 대신 오픈소스 에이전트들이 **세션이나 저장소를 여는 것만으로 명령이 실행되던 문제**를 한꺼번에 고쳤다(goose·Crush·Copilot CLI·OpenCode). LiteLLM LTS는 로그인 우회 수정이 빠진 채 나왔다. 도구는 설치하거나 실행하지 않았다. 권고 개수와 버전 범위는 저장소 권고 API로 직접 셌다.

### 1. (지금 업데이트, 받은 세션 파일은 지운다) goose 1.61.1: high 권고 3건. 세션 파일이나 저장소를 열기만 해도 명령이 실행됐다 (공식, 릴리스 00:09 UTC·권고 01:34 UTC)

- **한 줄 설명:** 가져온 세션의 위조 승인, Desktop의 `.env` 자동 로드, 레시피 템플릿 주입을 고쳤다. 세 권고 모두 high이고 수정 버전은 1.61.1이다.
- **권고 3건(권고 원문 제목):**
  - "Forged tool approvals in imported sessions run shell commands on open" (`< 1.61.1`)
  - "Goose Desktop loads .env from cwd, so a cloned repo can run code at launch" (`>= 1.0.0, <= 1.61.0`)
  - "Recipe parameter template injection on ACP session load runs shell commands" (`>= 1.55.0, <= 1.61.0`)
- **추천 이유:** 올려도 이미 받은 세션은 정리되지 않는다. 권고 원문: "Upgrading protects new imports, but not sessions you already imported." 남이 준 세션 파일을 가져온 적이 있다면 원문대로 "delete that session without opening it." 레시피 세션도 원문 "don't resume or fork it again. Start a new session from the recipe file instead."다.
[릴리스](https://github.com/aaif-goose/goose/releases/tag/v1.61.1) · [GHSA-5pvv](https://github.com/aaif-goose/goose/security/advisories/GHSA-5pvv-4hwj-j3mj) · [GHSA-48cx](https://github.com/aaif-goose/goose/security/advisories/GHSA-48cx-gcc2-3hmh) · [GHSA-422j](https://github.com/aaif-goose/goose/security/advisories/GHSA-422j-768j-jmgx)

### 2. (직전 판 2번 후속, 함정) LiteLLM LTS 1.108.19에는 CVE-2026-93355 수정이 없다. `lts` 이미지 태그는 아직 고쳐지지 않은 판을 가리킨다 (공식 + 직접 확인, 19:05 UTC)

- **확인한 것:**
  - 권고의 영향 범위는 여전히 `<1.111.0`이고 수정 버전은 1.111.0이다.
  - 권고에 적힌 예외 환경 변수 `LITELLM_EMAIL_FALLBACK_LINKS_ANY_USER`는 1.111.0 wheel에만 있고 1.108.19 wheel에는 없다(도구 에이전트가 wheel을 풀어 확인). 수정이 LTS로 백포트되지 않은 것으로 본다.
- **함정:** 릴리스 본문은 원문 "The `lts` image tag always points to the latest LTS release"라고 하고 "No action needed to upgrade"라고 적었다. `lts` 태그를 따라가는 배포는 여전히 영향 범위 안이다. SSO·JWT 이메일 폴백을 쓴다면 1.111.0으로 올린다. 못 올리면 권고 원문 "make sure your IdP only issues administrator-assigned, verified addresses"를 따른다.
- **함께 고쳐진 것(같은 본문):** "Fixed tool ids like `functions.Bash:0` being rewritten for non-Anthropic backends, stalling Claude Code after one call, broken since v1.91.0". LiteLLM 프록시 뒤에서 Claude Code를 다른 모델로 돌리던 사람에게 반가운 수정이다.

[1.108.19](https://github.com/BerriAI/litellm/releases/tag/v1.108.19) · [권고](https://github.com/BerriAI/litellm/security/advisories/GHSA-w5h5-479w-p5hx)

### 3. (client/server 모드를 썼다면 업데이트) Crush 0.103.0: 서버가 작업 공간 목록을 요청한 클라이언트에 API 키가 든 환경 변수를 통째로 돌려줬다 (공식, 15:56 UTC)

- **한 줄 설명:** 실험 기능이던 client/server 모드를 설정 옵션으로 정식 노출하면서 환경 변수 유출을 고쳤다. 실행 중 모델·추론 강도 변경, Bash 출력 실시간 표시도 들어왔다.
- **보안 수정(릴리스 원문):** "In client/server mode, the server returned each workspace's full environment, API keys and tokens included, to any client that listed workspaces." 권고 원문: "If you've used client/server mode, we recommend upgrading."
- **함정:** 저장소 권고(GHSA)가 없어 영향 버전 범위는 확인하지 못했다. 이 모드를 켠 적이 있다면 그동안 노출됐을 수 있는 키를 교체하는 편이 안전하다(원문 권고가 아니라 이 글의 판단이다).
- **설치/사용:** `npm i -g @charmland/crush` (npm `latest` 0.103.0)

[릴리스](https://github.com/charmbracelet/crush/releases/tag/v0.103.0) · [PR #4391](https://github.com/charmbracelet/crush/pull/4391)

### 4. (정식판 대기) Copilot CLI 1.0.117 프리릴리스: MCP 서버 이름이 내장 권한 이름과 같으면 셸 명령까지 함께 승인되던 문제 (공식, 20:17 UTC)

- **수정(v1.0.117-3 원문):** "Approving an MCP server's tools for the session or for a location no longer also approves Copilot's built-in requests, such as shell commands or memory stores, when the server's name matches a built-in permission type."
- **같은 판:** 샌드박스 명령과 에이전트 편집이 쓰기 가능한 Git 워크트리의 기존 저장소 설정 파일을 고치지 못하게 했다. Copilot SDK 2.0.4-preview.3 원문은 "can read but no longer modify existing Copilot and Claude repository settings files in writable Git worktrees"다. 에이전트가 저장소의 Claude·Copilot 설정을 고쳐 다음 실행의 권한을 넓히는 경로를 막는 변경으로 읽힌다.
- **함정:** npm `latest`는 아직 1.0.116이고 수정은 `prerelease` 태그(1.0.117-4)에만 있다. 정식판 전까지는 MCP 서버 이름을 `shell` 같은 내장 권한 이름과 겹치게 짓지 않는다.

[v1.0.117-3](https://github.com/github/copilot-cli/releases/tag/v1.0.117-3) · [Copilot SDK 2.0.4-preview.3](https://github.com/github/copilot-sdk/releases/tag/v2.0.4-preview.3)

### 5. (업데이트 후 저장된 승인 점검) OpenCode 2.4.2: 웹 가져오기 "항상 허용"을 한 번 누르면 모든 사이트가 허용됐다 (공식, 00:59 UTC)

- **수정(PR #66000 원문):** "so one click allowed every URL on every site for the project". 이제 그 사이트만 허용한다.
- **함정(릴리스 본문 Upgrade notes):** "Approvals saved by earlier versions stay as they were." 이전 판에서 저장된 `webfetch: *` 허용은 그대로 남으니 프로젝트 권한 설정에서 직접 지운다.
- **같은 판의 권한 버그:** 원문 "Fixed tools staying visible to the model when a blanket deny was followed by narrower denies." 호출은 막혀도 도구가 모델에 계속 보이던 문제다.

[릴리스](https://github.com/anomalyco/opencode/releases/tag/v2.4.2) · [PR #66000](https://github.com/anomalyco/opencode/pull/66000)

### 6. llama.cpp v1.1.0: Raon-1 mini·Inkling 2·Hy4 flash가 정식 릴리스에 들어갔다. `llama_batch` API는 지원 중단 (공식, 09:16 UTC)

- **한 줄 설명:** v1.0.0 이후 첫 마이너 릴리스다. 직전 판에서 "병합됐지만 릴리스 빌드에는 없다"고 적은 KRAFTON Raon-1 mini가 들어갔다.
- **원문(릴리스 Overview):** "llama.cpp v1.1.0 adds the Inkling 2 family (base, Mini and DSpark), Domyn Edge 4B, Zaya1 8B, RAON-1-mini and Hy4 flash, splits DFlash speculative decoding into separate draft methods, and deprecates the old `llama_batch` API in favour of `llama_batch_ext`."
- **함정:**
  - `llama.h`로 바인딩을 만드는 쪽은 지원 중단 경고에 대비한다.
  - 새 서버 옵션 `--ui-mcp-proxy-allow-private-urls`는 서버 README 원문대로 "anyone who can reach the proxy can then reach your local network"다. 기본값은 꺼짐이다.
  - Inkling 2 Mini 이슈 5건(#32778·#32710·#32836·#32910·#32966)은 모두 열려 있다.
- **함께 볼 것:** Ollama는 0.40.19-rc0 프리릴리스(17:52 UTC)에서 llama-server를 올렸다.

[릴리스](https://github.com/ggml-org/llama.cpp/releases/tag/v1.1.0) · [서버 README](https://github.com/ggml-org/llama.cpp/blob/v1.1.0/tools/server/README.md)

### 7. huggingface_hub 2.5.0: 유출된 토큰을 로그인 없이 폐기하는 `hf auth revoke`와 네트워크를 묶는 샌드박스 (공식, 15:38 UTC)

- **토큰 폐기(릴리스 원문):** "Revocation doesn't require being logged in: anyone holding the raw value of a token can revoke it, and the token owner gets notified by email." 로그나 공개 저장소에서 남의 토큰을 발견했을 때도 바로 무력화할 수 있다.
- **샌드박스:** 네트워크를 끄거나 허용 목록으로 묶는다. 원문 "cannot be removed from inside the sandbox, even by root". 정책을 걸 수 없으면 원문 "instead of silently returning a connected sandbox" 오류로 실패한다.
- **함정(같은 본문):** OAuth Space는 원문 "the signing key changes with this release"라 사용자가 다시 로그인해야 한다. HF 토큰과 특정 `provider`로 부르는 `chat_completion`은 이제 Hub 라우터를 거친다.
- **설치/사용:** `pip install -U huggingface_hub`

[릴리스](https://github.com/huggingface/huggingface_hub/releases/tag/v2.5.0)

### 8. (깨지는 변경) Qwen Code 0.28.7: 모델 루프 감지와 도구 호출 상한을 없앴다. Claude Code용 mod가 그대로 로드된다 (공식, 18:04 UTC)

- **깨지는 변경(릴리스 원문):** "Qwen Code no longer detects model loops or caps tool calls". 헤드리스·데몬으로 오래 돌리는 작업은 같은 호출을 반복해 비용이 커질 수 있다. 바깥에서 턴·예산 상한을 건다.
- **호환(같은 본문):** "Mods written for Claude Code 2.1.332 now load in Qwen Code". Claude Code mod가 다른 에이전트에서도 돌기 시작했다는 뜻이다. 남이 만든 mod를 넣을 때의 위험도 두 도구가 함께 진다.

[0.28.7](https://github.com/QwenLM/qwen-code/releases/tag/v0.28.7)

### 9. browser-use 1.0.9: OpenRouter·Bedrock 경유 Claude에서 프롬프트 캐시가 늘 0이었다 (공식, 22:45 UTC)

- **수정(릴리스 원문):** "Before this, every step re-billed the whole prompt there (cache_read was always 0)." 브라우저 에이전트는 단계마다 긴 프롬프트를 다시 보내 비용 차이가 크다.
- **함정:** 원문 "Bedrock is covered by unit tests only and wasn't tested live". 조회 시점 PyPI 최신은 1.0.8이라 아직 `pip`으로 받을 수 없다.

[릴리스](https://github.com/browser-use/browser-use/releases/tag/1.0.9)

### 짧게

- **n8n 권고 4건(08:17~08:19 UTC):** 기술 이슈에 정리했다.
- **Context7 MCP 4.9.0(15:39 UTC):** 원문 "The hosted HTTP server serves the Agent Skills a teamspace installed in the Context7 dashboard over the MCP skills extension". 직전 판의 Skills over MCP를 받아들이는 서버가 늘고 있다. [릴리스](https://github.com/upstash/context7/releases/tag/%40upstash/context7-mcp%404.9.0)
- **Cline SDK 0.2.2(08:40 UTC, 깨지는 변경):** 원문 "changes the default model for 28 providers". `writeGlobalSettings` 등은 원문 "no longer take an options argument"라 SDK를 쓰는 코드는 컴파일이 깨질 수 있다. Cline 4.1.28은 OpenAI 폐기 예정 모델 ID를 목록에서 뺐다. [SDK 릴리스](https://github.com/cline/cline/releases/tag/sdk/sdk/v0.2.2)
- **OpenAI SDK Python 3.60.0(07:09 UTC):** 지출 알림과 응답 압축 필드가 들어갔다. [릴리스](https://github.com/openai/openai-python/releases/tag/v3.60.0)
- **T3 Code 0.1.13(19:32 UTC):** 원문 "theme CSS shorthands with var() can no longer load remote files". [릴리스](https://github.com/pingdotgg/t3code/releases/tag/v0.1.13)
- **MCP 명세 릴리스 주기 문서(20:04 UTC 병합):** 원문 "MCP specification releases are published every six months, generally on the last Tuesday of March and September." 다음 판(2027-03-30)의 soft freeze는 원문 "2.5 months" 전이다. [문서](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/community/release-cycle.mdx)
- **Moat(HN 06:32 UTC, 237점대, 주의):** Command Code의 월 5달러 멤버십이다. 원문 "$5/mo to access the highest quality models at wholesale prices." 하지만 실제 추론 제공자를 밝히지 않는다(원문 "no hundred providers to pick from"). HN 댓글 원문 "I will not be sending tokens to a company who doesn't tell me who the end inference provider is". 직전 판들의 추론 제공자 데이터 사고를 생각하면 코드·비밀이 들어가는 작업에는 권하지 않는다. [Moat](https://commandcode.ai/moat) · [HN](https://news.ycombinator.com/item?id=50652184)
- **Notion 공식 MCP의 숨은 지시(HN 18:28 UTC, 커뮤니티):** 도구 결과에 링크 표시 방식 같은 지시문이 섞여 에이전트 컨텍스트로 들어간다는 보고다. HN 댓글이 옮긴 문구는 "Do not use a bare URL or create a link preview"다. 원 게시물(reddit)은 열지 못해 "업셀" 주장은 미확인이다. MCP 도구 결과를 지시로 따르지 않도록 서브에이전트로 격리하는 방법이 댓글에 나왔다. [HN](https://news.ycombinator.com/item?id=50658093)
- **AMD RDNA용 llama.cpp 패치 모음(HN 17:11 UTC, 240점대, 커뮤니티):** 블로그는 생성 속도 35→55 t/s, 프리필 700→1300 t/s라고 했고 원문 "YMMV"를 붙였다. README 원문 "Everything this delivery adds is default-on". v1.1.0 이전 upstream 기준이다. [블로그](https://devnull.land/amd-specific-llama-cpp) · [저장소](https://github.com/stew675/llama-cpp-rdna-boosts)
- **Shirudo.today(HN 22:23 UTC):** OpenAI·Cisco의 Shirudo를 일반 배포판 커널용으로 포크했다. 커널 버그 경로를 막는 서명된 BPF "shield"다. 원문 한계: "Root, a privileged container, or anything with CAP_BPF and CAP_SYS_ADMIN can unload a shield and make shirudo status say otherwise." 설치가 `curl | bash`다. [블로그](https://shirudo.today/blog/launch.html)
- **Show HN: lip-dictation:** 웹캠 입술 읽기로 Claude Code 프롬프트를 받아 적는 로컬 mod(Mac, MIT). README 원문 "This is a prototype, and accuracy is modest." [저장소](https://github.com/batanus/lip-dictation)
- **그 밖에 창 안 릴리스:** FastMCP 4.2.7a1(알파), mcp-remote 0.17.0(원격 인증 서버 메타데이터 검증을 끄는 `--skip-issuer-metadata-validation` 추가 — 남용 금지), Kimi Code 2.8.1, rmcp 3.12.0, Vercel AI SDK 패치, Langfuse 4.88, Phoenix 22.1.0.
- **직전 판 후속:**
  - Claude Code는 새 릴리스가 없다. npm `latest`·`next` 2.1.332, `stable` 2.1.326 그대로다. 한글 브랜치 이름 #112183과 #110979 등은 열려 있고 창 안 갱신이 없다.
  - Codex CLI `latest`는 0.172.0이고 0.174.0 알파만 나왔다. #62312는 열려 있다. Codex 사용량 변경은 모델 소식 1번에 정리했다.
  - 새 진전 없음: Open WebUI 0.13.5 보안 수정 공개(새 GHSA 없음), Kimi Code "항상 묻기" 자동 승인(2.8.1에도 그대로), Copilot 코드 리뷰 조직 과금, Cloudflare 에이전트 구독, Vercel Sandbox, Claude Security 플러그인, pydantic-ai, VS Code, claude-code-action(1.0.290), Claude Agent SDK, Anthropic SDK, `anthropics/skills`·`claude-plugins-official`(창 안 커밋 0건), MCP 레지스트리 #1920, GitHub MCP Server, Gemini CLI, vLLM, SGLang, transformers, Cursor.

### 참고: 창 경계 항목

- **(공식 11-24~25, 분석 기사·HN 11-26 09:44 UTC, 185점대) Google Workspace: AI 에이전트는 Google MCP 서버를 거쳐야 한다. 직접 REST API를 부르면 2027-02-01부터 쿼터가 묶인다.** 과거 판에서 다루지 않았다.
  - 약관 원문: "The GWS Access Tool must use a Google MCP where the GWS Access Tool is or is driven by an AI Agent". 금지 조항에는 "Store any Google Technical Information for more than 30 days"도 있다. [약관](https://workspace.google.com/terms/ai-tools)
  - 쿼터 문서 원문: "Starting February 1, 2027, AI agents or similar access tools that call Google Workspace REST APIs directly (instead of MCP) are limited to Standard Tier quotas, regardless of the tier selected." [문서](https://developers.google.com/workspace/tools-safety)
  - Gmail API에 직접 붙는 이메일 에이전트나 Workspace 플러그인은 두 달 안에 MCP 경로로 옮겨야 한다. Gmail MCP 설정 문서는 OAuth 클라이언트에 "Use this client for an AI-powered agent"를 고르게 한다. 같은 문서는 클라이언트의 확인 창을 "Always allow"로 두고 서버 쪽 확인에 맡기라고 권한다. 클라이언트가 서버 확인 요청을 자동 수락하면 확인 단계가 아예 사라진다. [설정 문서](https://developers.google.com/workspace/gmail/api/guides/configure-mcp-server)
  - 분석 기사는 "Gmail MCP는 메일을 못 보낸다"고 했다. 하지만 조회 시점 [Gmail MCP 참조](https://developers.google.com/workspace/gmail/api/reference/mcp)에는 `send_message`·`forward`가 있다. 기사가 인용한 "require partner access" 문구도 쿼터 페이지에서 찾지 못했다. 도구 에이전트의 Wayback 비교로는 11-04 사본에 보내기 도구가 없었다. 문서가 언제 바뀌었는지는 미확인이다. [techstackups](https://techstackups.com/articles/google-workspace-ai-agents-mcp/) · [HN](https://news.ycombinator.com/item?id=50653477)
- **(11-25 22:34 UTC) Claude-BugHunter 범위 우회 수정:** GitHub 트렌딩 상위의 Claude Code 레드팀 스킬 묶음이다. PR 원문 "parsed as the **in-scope** host `www.example.com`". 백슬래시가 든 URL이 허가 범위 안으로 판정돼 범위 밖 호스트에 요청이 갈 수 있었다. 쓰고 있다면 최신 main으로 올린다. [PR #247](https://github.com/elementalsouls/Claude-BugHunter/pull/247)
- **(11-25) JetBrains 블랙프라이데이:** 신규 개인 연간 구독·업그레이드 40% 할인, 코드 `BF2026`. 원문 "The offer is valid November 25 – December 2, 2026." [블로그](https://blog.jetbrains.com/blog/2026/11/25/black-friday-2026/)

## 주목할 점

- **검사기가 있는 분야와 없는 분야가 갈린다.** 수학은 Lean이 있어 공개 일주일 안에 외부 검증까지 끝났다. 물리 논문은 사람 전문가가 다른 회사 모델로 하루 만에 반박했다. AI 과학 결과를 읽을 때는 "기계 검증이 있는가"를 먼저 본다.
- **에이전트가 남이 준 파일을 믿는 지점이 공격면이다.** 세션 파일, 저장소의 `.env`, 레시피, MCP 서버 이름, 저장소 설정 파일이 이번 창에 모두 고쳐졌다. 공통 함정은 업그레이드해도 과거 승인과 세션이 남는다는 것이다. 올린 뒤 저장된 승인을 직접 지운다.
- **사용량과 경로의 기본값이 바뀌는 연말이다.** GPT-6.2 한도가 줄었고 12월에는 ChatGPT·Codex 한도가 합쳐진다. Upstage 정가는 프리뷰가의 10배다. Google은 2027-02-01부터 Workspace 에이전트를 MCP로 몰아넣는다.
- **다가오는 일정:**
  - 이번 주: **KEV 조치 기한 11-27(Drupal 외), 11-28(직전 판에 오른 8건)**. JetBrains 블랙프라이데이 할인(12-02까지).
  - 다음 주: **Claude Sonnet 4.5 API 은퇴(11-30)**, Claude Code 관리형 마켓플레이스 자동 갱신 기본값(11-30 이후), Gemini 4 Carbon·Argon 도입가 종료(11-30), Huawei openPangu-2.0 종료(11-30), **OpenSSL 4.1.1·4.0.4·3.5.10(12-01)**, OpenAI `gpt-image-1-mini`·`gpt-image-1.5`·`chatgpt-image-latest` 종료(12-01), Gemini 2.5 Flash 라우팅 변경(12-01), NVIDIA Huang 하원 비공개 인터뷰(12-01)·상원 은행위 청문회(12-03), Puppet Enterprise(12-01)·LiteLLM(12-02) KEV 기한, **Upstage Solar Mini 4.5 정식 출시와 프리뷰 ID 자동 전환(12-03)**.
  - 12월: **ChatGPT·Codex 공유 사용량 한도(12월 중)**, Claude 앱 선택기에서 Sonnet 4.6 제거(12-08), Rackspace Spot 종료(12-08), SWE-2.1 무료 기간 종료(12-08), EU·EFTA·영국 Copilot 테넌트 Claude 기본 켜짐(12-09), Val Town 읽기 전용(12-10), **OpenAI `gpt-5`·`o3` 계열 종료(12-11)**, GitHub Copilot 모델 6종 은퇴(12-14), GPT-5.6 Sol·Terra 은퇴(12-23), Inception Mercury API 종료(12-28), Copilot 코드 리뷰 조직 과금 기본값(12-30).
  - 그 뒤: Zapier MCP 무료 종료(2027-01-05), BIS 원격 접근 규칙 의견 마감(01-19), Claude Opus 4.5 은퇴(01-22), **Google Workspace REST 직접 호출 에이전트 쿼터 제한(02-01)**, 다음 MCP 명세(03-30), Microsoft NTLM 서명 2단계(2027-05).

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - Reuters(CAPTCHA), FT 본문(403, 제목·시각만), WSJ·WaPo·Business Insider(유료), reddit(Notion MCP 업셀, 양자중력 글), X 타임라인은 열지 못했다. Reuters의 중국 랩 보도는 The Decoder 전재와 X 전언으로, Zhipu 임원의 WeChat 글은 快科技 보도로만 확인했다.
  - Apiiro(PolinRider 7,600개 추정)와 opensec(NFS2ROOT) 원문은 Cloudflare에 막혔다. Notion MCP 지시문은 HN 댓글 재인용이다.
- **시각이 날짜만 있거나 불확실한 원문:** 영란은행 보고서, 1Password 갱신 문구(11-25 20:10 UTC 이후 추가), Aaronson 글의 11-26 갱신(21:18 UTC 이전), Claude Duet 페이지와 Upstage 문서의 갱신 시각, Google Workspace 약관·블로그(날짜만), Gmail MCP 참조 문서의 변경 시각.
- **확인하지 못한 것:**
  - The Decoder가 전한 "Business 사용자 연말까지 Fast Mode 무료"는 원 X 글과 답글에서 찾지 못했다. OpenAI 지원 페이지는 창 직후 갱신돼 창 안 판본과 다를 수 있다.
  - 양자중력 논문의 물리적 타당성은 판단하지 않았다. 반박 글 하나만 실었다. 수학 결과의 외부 Lean 전체 빌드(OpenAI)와 Google Lean 공개는 여전히 없다.
  - goose 권고의 `ANT-2026` 식별자가 누구의 보고인지, Crush 키 유출의 영향 버전 범위(GHSA 없음), browser-use 1.0.9의 PyPI 게시는 확인하지 못했다.
  - 재현하지 않은 것: 모든 증명·벤치마크·취약점 주장. 도구와 패키지는 설치하거나 실행하지 않았다. LiteLLM LTS 판정은 wheel 안 예외 변수 이름의 존재 여부로 했다. GHSA·KEV 개수, n8n·Langflow 권고 범위, OpenAI deprecations 일정은 공개 API와 원문으로 직접 확인했다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 80점 이상 스토리 29개와 20점 초과 스토리 약 60개의 제목을 모두 봤다. 그 아래는 키워드로 걸렀다.
  - 전역 GHSA는 reviewed 0건, unreviewed 543건, malware 56건을 받아 셌다. unreviewed는 AI 키워드와 critical 제목으로만 걸렀다. 도구 릴리스는 저장소당 최근 릴리스만 봤다.
  - Ruby2ßhell의 Ubuntu·RHEL 상태, FBI IC3 새 PDF, Z.ai·Moonshot·DeepSeek 공식 X 계정, 일본·유럽·인도 랩은 얕게 봤다.
  - 창이 끝날 때 열려 있던 것: Claude Microsoft 365 추가 기능 장애, NFS2ROOT 배포판 패치, Copilot CLI 1.0.117 정식판, browser-use 1.0.9 PyPI 게시, GLM-5.5 일정, OpenAI 지원 페이지 갱신 내용. 다음 판에서 다룬다.
