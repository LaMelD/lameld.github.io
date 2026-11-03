---
title: "2026-11-04 AI 브리핑"
date: 2026-11-04T01:00:00+09:00
tags: [ai-briefing, anthropic, google, security, mcp]
description: "Anthropic은 S-1 수정본에 자사 RSP의 'AI R&D 가속' 기준에 곧 닿을 수 있다는 문장을 새로 넣었고, Google은 12월 1일부터 Gemini 2.5 Pro·Flash 요청을 새 모델로 자동 라우팅하며, WorkOS 침해에 AI 공격 도구 ARTEX가 쓰였다는 CEO 발언과 MCP C# SDK 세션 탈취 권고가 나왔다."
---

> 조사 범위: 2026-11-03 01:00 ~ 2026-11-04 01:00 KST(2026-11-02 16:00 ~ 11-03 16:00 UTC, 미국 월요일 08:00 ~ 화요일 08:00 PST). 직전 브리핑(11-03 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`, 저장소·전역 GHSA `published_at`, Ubuntu USN `published`, Wayback 스냅샷, Hugging Face `createdAt`, 트윗 스노우플레이크 ID(fxtwitter), HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-03 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **"AI가 AI를 고친다"가 하루를 덮었다.** Anthropic은 S-1 수정본에 "we may imminently meet our Responsible Scaling Policy's definition of 'dramatically accelerated progress attributable to AI'"라는 문장을 새로 넣었다(원 S-1에는 없다). 같은 날 MIT 연구진은 GPT-6 Astra가 Qwen3.5-0.8B 가중치 0.016%를 고쳐 MATH-500을 7.9%에서 48.1%로 올렸다는 글로 HN 2,880점대를 얻었다. 다만 "가중치만 보고"가 아니라 파이썬 실행과 반복 평가를 쓴 결과라는 반박도 580점을 받았다.
- **모델이 바뀌는 날짜와 비용 구조가 정해지고 있다.** Google은 12월 1일부터 `gemini-2.5-pro` 요청을 `gemini-3.8-flash`로, `gemini-2.5-flash`를 `gemini-3.5-flash-lite`로 자동 라우팅하고 그 요금으로 받는다. Anthropic은 긴 대화를 서버에 잠시 두는 message threads 베타를 열었다(TTFT 약 35% 감소, ZDR 대상 아님). Apple은 Siri AI 같은 서버 모델 기능에 사용 한도를 두고 추가 사용량을 팔겠다고 했다.
- **보안:** WorkOS CEO가 영업 시스템 침해에 "The attack involved ARTEX"라고 썼고, Wiz의 Black October 목록에 Brev(NVIDIA)·Trigger.dev·Chainstack이 더해졌다. Google은 탈취한 AI 키로 하루 10만 달러 넘게 쓰고 되파는 LLMjacking 보고서를 냈다. MCP C# SDK는 식별 클레임 없는 사용자끼리 세션이 섞이는 high 권고를 고쳤다(1.4.2·2.3.1). Ubuntu의 CVE-2026-98432 수정 커널은 여전히 -proposed에만 있다.

## 모델 소식

미국 월요일 오전부터 화요일(선거일) 오전까지의 창이었다. 프런티어 랩의 새 모델 출시는 없었다. Hugging Face 주요 조직 50곳의 창 안 신규 모델은 0건이고, OpenRouter 신규 등록도 0건이었다. 대신 모델 능력을 둘러싼 이야기가 컸다. GPT-6 Astra가 다른 모델의 가중치를 직접 고쳤다는 MIT 글이 창 안 HN 1위였고(기술 1번), 같은 날 Anthropic 공시에서 "AI가 연구를 크게 가속하는 단계에 곧 닿을 수 있다"는 문장이 확인됐다.

### 1. (후속) Anthropic S-1/A에 새로 들어간 RSP 문장: "we may imminently meet … 'dramatically accelerated progress attributable to AI'"

- **새로 들어간 문장(S-1/A 원문, 10-26 원 S-1에는 없다):**
  - "our internal estimates and our expectations regarding future models, rather than our current models, indicate that we may imminently meet our Responsible Scaling Policy's definition of "dramatically accelerated progress attributable to AI." We have not designated any of our current models as exceeding this threshold"
  - "we do not anticipate being able to safely use Claude to fully autonomously design and build successive generations of frontier models without meaningful human supervision or understanding, if and when that becomes technically possible."
  - 그래서 RSP를 고쳐 "require human review and approval of certain decisions in our research and development processes as our models perform an increasing share of that work"를 검토한다고 적었다.
  - 원 S-1에도 있던 문장: 가속이 일어난 뒤에야 알아챌 수 있다("until after it has occurred").
- **보도:** TechCrunch(11-02 22:14 UTC)가 "Anthropic warns that it's very close to RSI capability"라는 제목으로 처음 짚었다. 직전 판은 같은 S-1/A의 재무 숫자만 다뤘다.
- **IPO 일정(Reuters, CNA 전재 11-03 01:01 UTC):** 원문: "expected to go public before Thanksgiving". Nasdaq 티커는 "ANT"다. CommSec이 호주 개인 투자자 배정을 맡는다. 기존·예비 투자자들은 "a $1.5 trillion valuation, down from roughly $2 trillion"을 따져 보고 있다고 한다. 창 안 새 EDGAR 제출은 없고, 공모가 범위는 아직 비어 있다.

중요한 이유: 프런티어 랩이 증권 공시에서 자사 RSP의 AI R&D 가속 기준에 "곧" 닿을 수 있다고 적은 것은 처음이다. 공시이므로 투자자 위험 고지의 성격이 강하지만, 사람의 승인 절차를 연구 과정에 넣겠다는 방향까지 같이 밝혔다. 같은 날 나온 가중치 편집 연구(기술 1번)와 함께 읽힌다.

[SEC S-1/A](https://www.sec.gov/Archives/edgar/data/2130357/000162828026068676/anthropicpbc-sx1ax1.htm) (공식, 11-02 14:24 UTC 접수) · [비교용 S-1](https://www.sec.gov/Archives/edgar/data/2130357/000162828026067168/anthropicpbc-sx1.htm) · [TechCrunch](https://techcrunch.com/2026/11/02/anthropic-warns-that-its-very-close-to-rsi-capability-aka-self-building-ai/) (매체보도) · [CNA/Reuters](https://www.channelnewsasia.com/business/anthropic-plans-australian-share-offer-in-ipo-commsec-says-6514811) · 의견 기고 "The SEC Should Block Anthropic's IPO" [HN](https://news.ycombinator.com/item?id=50344919)

### 2. Google: Gemini 2.5 Pro·2.5 Flash 폐기. 12월 1일부터 API 요청이 3.8 Flash와 3.5 Flash-Lite로 자동으로 넘어간다

- **변경 로그 원문("November 2, 2026"):** "Effective December 1, 2026, all API requests to `gemini-2.5-pro` will automatically route to `gemini-3.8-flash` and be billed at Gemini 3.8 Flash rates." `gemini-2.5-flash`는 `gemini-3.5-flash-lite`로 간다. 두 모델 모두 AI Studio에서 더는 고를 수 없다.
- **폐기 페이지 원문:** "Access through the API is limited to existing users who have actively used it in the past." `gemini-2.5-flash-lite`는 폐기 대상이 아니다.
- **깨지는 점(원문):** "Breaking API changes include function calling and `thought_signature` validation, `thinking_level` replacing `thinking_budget`, and removed sampling parameters and prefilled model turns." 자동 라우팅된 요청에서 옛 파라미터가 어떻게 처리되는지는 문서에서 확인하지 못했다.
- **가격(가격 페이지, 100만 토큰당):** 2.5 Pro는 입력 $1.25(20만 토큰 이하)다. 3.8 Flash는 입력 $0.75·출력 $3.75가 "through December 31, 2026"이고, 2027-01-01부터 $1.50·$7.50이다. 2.5 Flash와 3.5 Flash-Lite는 텍스트 입력 $0.30·출력 $2.50으로 같다.

중요한 이유: 코드를 고치지 않으면 12월 1일에 모델이 조용히 바뀐다. 함수 호출과 thinking 파라미터가 깨지는 변경이 있어, 2.5 Pro를 쓰는 에이전트는 그 전에 직접 옮기고 시험하는 편이 안전하다.

[변경 로그](https://ai.google.dev/gemini-api/docs/changelog) · [폐기](https://ai.google.dev/gemini-api/docs/deprecations) · [가격](https://ai.google.dev/gemini-api/docs/pricing) (공식, 날짜만 11-02. 직전 판 조사 시점 최신 항목은 10-30이었다)

### 3. Claude API "message threads" 공개 베타: 대화를 서버에 잠시 두고, 다음 턴에는 새 메시지만 보낸다

- **무엇인가(Anthropic 글 원문):** "With message threads on by default for most Claude Code users, requests with 500k to 1M tokens see median TTFT drop by about 35%."(자사 측정) 첫 요청은 전체 대화로 스레드를 만들고, 이후에는 새 메시지와 직전 응답 ID만 보낸다. 베타 헤더는 `message-threads-2026-08-12`다.
- **조건(문서 원문):**
  - "It expires 5 minutes after its last request, and the API deletes it no more than 1 hour after its last use." 만료 뒤 continue 요청은 404를 받고, 전체 대화를 다시 보내야 한다. SDK 스레드 헬퍼가 이 재전송을 대신 한다.
  - "Threads do not change pricing or rate limits."
  - "Not ZDR eligible", "Not HIPAA eligible". HIPAA readiness를 켠 조직은 400 오류를 받는다.
  - Claude API, Claude Platform on AWS, Google Cloud(Anthropic 관리), Microsoft Foundry에서 쓸 수 있다.
- **함정(원문):** 스트리밍 중에는 "continue from it only after you receive the message_stop event". 그 전에 보내면 404가 날 수 있다.

중요한 이유: 수십만 토큰 대화를 매 턴 다시 올리는 자체 에이전트 루프의 지연을 줄인다. Claude Code에는 이미 대부분 켜져 있다. ZDR 계약 조직은 쓸 수 없다는 점이 가장 큰 제약이다.

[Anthropic 글](https://claude.com/resources/articles/message-threads) · [문서](https://platform.claude.com/docs/en/build-with-claude/message-threads) · [릴리스 노트](https://platform.claude.com/docs/en/release-notes/overview) (공식, 날짜만 11-02. Wayback 11-02 12:52 UTC 판에는 없고 11-03 10:24 UTC 판에 있다) · [The Decoder](https://the-decoder.com/anthropics-message-threads-cut-time-to-first-token-by-35-percent-for-long-conversations/) (11-03 12:35 UTC)

### 4. Apple: Siri AI 같은 서버 모델 기능에 사용 한도를 두고, 추가 사용량은 앞으로 판다

- **실적 발표 콜(The Verge 원문):** "Ternus said features relying on server-side models, including Siri AI and intelligent photo editing, will have usage limits." 더 필요한 고객은 "will be able to purchase expanded access in the future"라고 했다.
- **비용(9to5mac, 11-02 23:08 UTC):** Apple은 메모리 비용과 Siri AI 출시가 연말 분기 이익률을 누를 것이라고 했다.
- **모델:** The Verge에 따르면 Siri AI는 Google Gemini 기술로 만든 모델을 쓴다.

중요한 이유: 직전 판의 OpenAI·Anthropic·xAI에 이어 Apple도 서버 추론 비용을 사용 한도와 추가 과금으로 다루겠다고 밝혔다. 기기 판매로 AI 비용을 덮어 온 회사도 같은 길로 간다는 신호다.

[The Verge](https://www.theverge.com/ai-artificial-intelligence/1025245/apple-earnings-q4-ai) (11-03 15:24 UTC) · [9to5mac](https://9to5mac.com/2026/11/02/apple-says-memory-costs-and-siri-ai-will-weigh-on-holiday-quarter-profit-margins/) (매체보도, 콜 발언 인용)

### 5. Google: K-12 학생 계정의 Gemini·Search AI를 기본으로 끈다

- **Google 원문:** "Access to these products will be entirely off for K-12 student accounts until school administrators explicitly turn them on." 대상은 Gemini, Gemini Notebook, Gemini in Classroom, Google Search다. 구체적인 일정은 "in the coming months" 학교에 알린다고 했다.
- **NYT:** 적용은 내년 초이고, 8월에 기본으로 켰던 조치를 되돌리는 것이다. AI를 막은 학교의 학생은 Search도 쓸 수 없다고 Google 부사장이 말했다.
- **같은 글:** 학습용 모드인 Guided Learning을 Classroom에 넓힌다.

중요한 이유: 학교 쪽 반발에 Google이 기본값을 "끔"으로 돌렸다. 다만 Search까지 묶여 있어, AI를 끄는 학교는 검색도 함께 잃는다.

[Google 블로그](https://blog.google/products-and-platforms/products/education/ai-learning-era-updates-2026/) (공식, 11-03 14:00 UTC) · [NYT](https://www.nytimes.com/2026/11/03/us/google-search-schools-restrict-ai.html) (매체보도, 유료벽)

### 6. (후속) OpenAI: macOS용 ChatGPT Classic 앱이 2027-01-30에 은퇴한다. ChatGPT 파일 업로드 장애도 있었다

- **Classic 앱(9to5mac 11-02 20:07 UTC, 앱 배너 원문):** "This app will retire on January 30, 2027. Get the latest features in the new ChatGPT app. Your full chat history comes with you". 직전 판 모델 3번 Chat·Work 탭 통합의 연장이다. 사용량 공유 시점은 여전히 "in the coming weeks"이고 새 공식 날짜는 없다.
- **장애(상태 페이지):** "File upload errors in ChatGPT". 원문: "Some users are having trouble uploading files to ChatGPT or getting ChatGPT to read uploaded files." 서브에이전트 조회로는 11-03 00:46 UTC에 원인을 찾았고 05:39 UTC에 해결됐다.
- Codex 학습 설정이 몰래 다시 켜졌다는 커뮤니티 제보는 기술 10번에 적었다.

[9to5mac](https://9to5mac.com/2026/11/02/openai-is-retiring-chatgpt-classic-app-will-soon-combine-chat-and-work-tabs-in-new-app/) (매체보도) · [OpenAI 상태 페이지](https://status.openai.com/incidents/01M6FDZ89W4PKDS19K496J307G) (공식)

### 7. 미국 선거일: 창 안에 새 공식 선거 정책은 없었다. 챗봇의 선거 오답 사례가 다시 보도됐다

- **공식 채널:** Anthropic의 최신 선거 글은 10-14다. Google 블로그·xAI·OpenAI RSS에 창 안 선거 글이 없다.
- **(창 경계) Semafor 단독(11-02 09:45 UTC):** OpenAI와 Anthropic이 처음으로 선거 "war rooms"를 운영한다.
- **WIRED(11-03 14:45 UTC, Forum AI 조사 인용):** Gemini가 펜실베이니아 우편투표 신청 마감을 실제(10-27)가 아닌 "November 3"로 답했다. 원문: "almost 40 percent of election answers given by all the mainstream chatbots contained at least one factual error."

[WIRED](https://www.wired.com/story/heres-why-you-shouldnt-use-ai-to-vote/) · [Semafor](https://www.semafor.com/article/11/02/2026/election-war-rooms-enter-the-ai-age) (매체보도)

### 8. 장애

- ChatGPT 파일 업로드 장애(6번). status.claude.com은 창 안 인시던트가 없다(최신 10-28). Google Cloud도 11-01 이후 없다.

### 9. 짧게

- **(후속) Grok Bot 한도:** The Decoder(11-02 18:15 UTC)는 Telegraph를 인용해 일부 사용자의 한도가 주말에 "slashed in half"였다고 보도했다. 원문: Grok Bot은 "shares its quota with Grok and the coding platform Cursor". 직전 판 모델 4번 Musk 발언의 후속이다. [The Decoder](https://the-decoder.com/spacexsi-apparently-cuts-grok-bot-quotas-weeks-after-musk-called-it-his-fastest-growing-product/) · [TechCrunch](https://techcrunch.com/2026/11/02/elon-musk-throttles-grok-bot-limits-due-to-unsustainably-high-costs/) (매체보도)
- **xAI `grok-imagine-image-quality` 은퇴 발효(11-02):** 요청은 `grok-imagine-image-2.0`의 low 품질로 넘어간다. 원문: "$0.01 per image cheaper than grok-imagine-image-quality at every resolution". 9월에 공지된 일정이다. [마이그레이션 문서](https://docs.x.ai/developers/migration/imagine-image-quality-nov-2) (공식)
- **Stanford Zou 그룹 "The shelf life of LLM findings"(11-02):** 널리 인용된 LLM 연구 결과를 요즘 모델로 다시 재 봤다. 원문: "Almost half (44%) of our tested claims are now expired in today's models. Another 34% have weakened significantly, 16% persist, and 6% have strengthened". 오래된 프롬프트 요령이나 모델 약점 연구를 그대로 믿지 말라는 근거다. [원문](https://zou-group.github.io/llm-claims-shelf-life/) (연구자 발표)
- **(미확인) Revolut이 엔지니어 약 2,000명을 자체 호스팅 GLM으로 옮겼다는 주장:** 투자자 트윗(11-02 22:11 UTC)이 회사 AI 운영 책임자의 발표를 옮긴 것이다. 원문: "56 Nvidia B200s, a blended 30 cents per million tokens, roughly a tenth of what they paid Anthropic and OpenAI." Revolut 공식 자료는 찾지 못했다. [트윗](https://x.com/guilleflorvs/status/2117379073785696395) · [HN](https://news.ycombinator.com/item?id=50350004) (커뮤니티)
- **Gartner(The Register 11-03 06:19 UTC):** 제목대로 "Open models can do 90% of agentic work at a twentieth of the cost of a frontier LLM"이라는 보고서다. 보고서 원문은 보지 못했다. [The Register](https://www.theregister.com/ai-and-ml/2026/11/03/open-models-can-do-90-of-agentic-work-at-a-twentieth-of-the-cost-of-a-frontier-llm-gartner/5307132) (매체보도)
- **(창 경계) Meituan LongCat-2.5(TechNode 11-02 04:32 UTC):** 원문: "1.6 trillion total parameters, with approximately 48 billion activated". 코딩·시각 이해를 강화했다. 가중치 공개는 확인하지 못했다. [TechNode](https://technode.com/2026/11/02/meituan-launches-longcat-2-5-multimodal-ai-model-for-extended-coding-tasks/) (매체보도)
- **직전 판 후속 중 새 진전 없음:** Mythos 5.5(공식 페이지 여전히 5.1), GPT-6.2 Astra(모델 문서에 없음), GDPx 리더보드, OpenAI 사용량 공유 날짜.
- **공식 채널 새 진전 없음:** OpenAI API 변경 로그(10-30)·폐기 목록, Gemini 앱 릴리스 노트, xAI 릴리스 노트, Mistral, DeepSeek, Alibaba 뉴스룸, Z.ai, Claude 앱 릴리스 노트.

## 기술 이슈

창 안 HN 1위는 "GPT-6 Astra가 신경망 가중치를 읽고 고친다"는 MIT 글(2,880점대)이었고, 반박 글도 580점을 받았다. 보안 쪽에서는 AI 공격 도구 ARTEX가 쓰인 침해가 더 드러났고, 탈취한 AI 키를 되파는 시장에 대한 Google 보고서가 나왔다. 전역 GHSA는 reviewed 2건, unreviewed 462건, malware 77건이었다.

### 1. "Astra가 가중치를 직접 읽고 고친다"(HN 2,880점대): Qwen3.5-0.8B 수학 점수를 7.9%에서 48.1%로 올렸다는 주장과, "과장됐다"는 반박

- **주장(MIT CSAIL, Takehi·Teoh·Zhang·Khattab):**
  - 원문: "Astra traced the looping to one direction written by the MLPs of layers 15 and 19, and subtracted that direction from 64 output columns in each (131,072 weights)".
  - "With 50 forward passes, MATH-500 rose from 7.9% to 48.1% and GSM8K from 13.8% to 67.1%." IFEval은 21.0%에서 62.3%로 올랐다. 원문: "the edit changes only 131,072 of the model's 0.8 billion weights (0.016%)."
  - 고친 효과는 대부분 생각이 끝없이 도는 문제를 없앤 데서 나왔다. 토큰 한도(32,768)에 걸린 답의 비율이 93%에서 33%로 줄었다.
  - 고친 모델은 HF에 공개됐다.
- **본문에 있는 한계:**
  - Qwen 실험에서 Astra는 "a Python kernel over the weights"와 연습 문제, 순전파 10·25·50회를 썼다. 순전파 10회·25회에서는 MATH-500이 12.3%·14.1%에 그쳤다.
  - 거의 루프에 빠지지 않는 Gemma 3 270M은 원문대로 "Here, Astra could not improve it."
- **반박:**
  - Honnibal(HN 580점): 원문 "The "just by looking at the raw weights" setting (raw floats in context, no tools, no running the network) only appears in experiments 1, 2 and 4, on networks of 17K–172K weights. Every experiment involving a real language model uses Python".
  - Goedecke(HN 120점): 원문 "I think a smart human ML researcher could perform all of these tasks."

중요한 이유: "모델이 다른 모델을 고친다"는 그림은 재귀적 자기 개선 논의로 곧장 이어져 크게 퍼졌다(모델 1번의 Anthropic 공시와 같은 날). 하지만 헤드라인 수치는 도구와 반복 실행을 쓰고, 알려진 루프 문제 하나를 끈 결과다. 원 가중치만 읽은 실험은 수만~십수만 개 규모의 작은 망에 한정된다. 동료 심사 전 블로그 글이다.

[블로그](https://rikiyat.github.io/blog/weight-space-programming/) (연구자 발표, 날짜만 11-02. HN 첫 게시 11-02 18:21 UTC) · [HF 모델](https://huggingface.co/RikiyaT/Qwen3.5-0.8B-weight-space-programming) · [HN](https://news.ycombinator.com/item?id=50341174) · [Honnibal](https://honnibal.dev/blog/astra-read-write-misleading) ([HN](https://news.ycombinator.com/item?id=50351111)) · [Goedecke](https://www.seangoedecke.com/llms-are-not-superhuman-at-reading-and-writing-neural-network-weights/) ([HN](https://news.ycombinator.com/item?id=50344861)) · [The Register](https://www.theregister.com/ai-and-ml/2026/11/03/forget-retraining-ai-can-now-tweak-another-ais-weights/5307105) (11-03 08:15 UTC)

### 2. (Black October 후속) WorkOS 영업 시스템이 털렸고 CEO는 "The attack involved ARTEX"라고 썼다. Wiz 목록에 Brev(NVIDIA)·Trigger.dev·Chainstack이 더해졌다

- **WorkOS(인증 SaaS) 공지 원문:** "we identified unauthorized access to an internal WorkOS application used by our sales team". 노출된 것에는 "Summaries and excerpts from sales calls, support discussions, and customer Slack conversations"가 있다. 공지는 이번 사고가 CRM과 고객 지원 데이터에만 미쳤다고 했다.
- **CEO의 HN 댓글(11-03 00:26 UTC) 원문:** "The attack involved ARTEX, an autonomous AI agent framework used to carry out offensive security operations." 공식 공지에는 도구 이름이 없다.
- **Wiz 로그 원문:**
  - "11-02, 17:00 UTC - Two additional incidents are publicly reported: Brev (NVIDIA) (Oct 4-20), and Trigger.dev (Nov 1)."
  - "11-03, 15:40 UTC - An additional incident is publicly reported: Chainstack (October 29-31)." WorkOS는 아직 목록에 없다.
- **Chainstack(블록체인 노드 인프라, 11-03 12:12 UTC) 원문:** "this incident was part of a broader AI-powered attack campaign reported in late October".
- **직전 판 후속:** Trigger.dev와 Northflank는 아직 침해 경로를 밝히지 않았다.

중요한 이유: 피해가 AI 개발 플랫폼에서 인증 SaaS의 영업 시스템과 블록체인 인프라로 번졌다. 통화 요약과 Slack 발췌는 그럴듯한 피싱 메일의 재료가 된다. WorkOS 고객이라면 WorkOS를 사칭한 연락을 경계해야 한다.

[WorkOS 공지](https://workos.com/security/bulletins/october-30th-2026-data-security-incident) (공식, 날짜만 11-02) · [HN 고객 통지문](https://news.ycombinator.com/item?id=50345712) (11-02 23:30 UTC) · [CEO 댓글](https://news.ycombinator.com/item?id=50346301) · [Wiz](https://www.wiz.io/blog/black-october-for-ai-developer-platforms) · [Chainstack](https://chainstack.com/security-incident-nov-2026/) (공식) · [The Register(Trigger.dev)](https://www.theregister.com/security/2026/11/02/thieves-pilfer-environment-variables-from-trigger-ai-workflow-tool/5306998)

### 3. ARTEX 사례가 쌓였다: 14개국 200여 조직 공격, 12분 만에 root, 학교 랜섬웨어

- **CrowdStrike(BleepingComputer 11-02 21:40 UTC 정리) 원문:** 중국 해커가 ARTEX로 "more than 200 organizations across 14 countries"를 공격해 "at least nine organizations"를 뚫었다. 운영자는 CrowdStrike Falcon이 깔린 Windows 기기를 건너뛰라고 에이전트에 지시했고, 원문: "In one intrusion, the AI agents consumed more than 1.4 billion input tokens".
- **DIVD(BC 인용):** 9월 침해의 도구를 ARTEX로 지목했다. 원문: "from initial access to root privileges in 12 minutes, chaining a Zammad zero-day with a second zero-day".
- **Bitdefender(BC 11-03):** 영국 학교 법인에서 3AM 랜섬웨어가 약 50대 중 37대를 암호화했다. 40~100초 간격으로 조금씩 바뀌는 명령이 반복돼 AI 지원 공격으로 추정했지만, 원문: "we cannot document anything about the language model itself".
- **영국 NCSC(The Register 11-03):** 2028년까지 AI 악성코드가 핵심 기반시설 랜섬웨어에 쓰일 가능성이 높다고 했다.

중요한 이유: 공개 공격 프레임워크 하나가 국가 배후 공격, 보안 연구 단체 침해, 2번의 플랫폼 침해에 연달아 등장했다. 사람보다 빠르고 오래 시도하는 공격이 실제 사고로 쌓이고 있다.

[BleepingComputer(ARTEX)](https://www.bleepingcomputer.com/news/security/chinese-hackers-use-artex-ai-in-automated-attacks-against-200-orgs/) · [CrowdStrike](https://www.crowdstrike.com/en-us/blog/vapor-panda-uses-artex-to-scale-global-campaign/) (창 경계, 10-29 갱신) · [BleepingComputer(3AM)](https://www.bleepingcomputer.com/news/security/uk-school-estate-hit-by-3am-ransomware-in-suspected-ai-assisted-attack/) · [The Register(NCSC)](https://www.theregister.com/security/2026/11/03/britain-on-notice-ai-powered-ransomware-attacks-to-arrive-by-2028/5307200) (매체보도·벤더 리서치)

### 4. Google/Mandiant "LLMjacking" 보고서: 탈취한 AI 키로 하루 10만 달러 넘게 쓰고, "LLM Mixer"로 되판다

- **규모(원문):** "fraudulent charges associated with resource consumption exceeding $100,000 USD in a single day, with cumulative charges at some organizations reaching $1 million USD before access was revoked."
- **재판매(원문):** "LLM Mixers function as illicit routing gateways that proxy, aggregate, and resell unauthorized LLM access to underground subscribers." 한 조직은 "more than 500,000 LLM API calls in less than eight hours"로 jet-night.com을 돌렸고, 피해 조직에 "nearly one million dollars in token costs"를 떠넘겼다.
- **탐지 신호(원문):**
  - 키가 살아 있는지 "minimal single-token requests (`max_tokens=1`)"로 조용히 확인한다.
  - GCP에서 추론 기록이 남는 Data Access 로그는 "disabled by default due to volume and ingestion costs"다. 직접 켜야 한다.
  - 후속 공격 집단은 LiteLLM 게이트웨이의 `/key/regenerate`로 일반 키를 PROXY_ADMIN으로 올렸다.
- **검증 메모:** 보고서는 이 LiteLLM 결함을 CVE-2026-40217로 적었다. 그런데 GHSA의 CVE-2026-40217은 5월에 나온 "sandbox escape in custom-code guardrail" 권고다. 번호가 잘못 붙었을 수 있으니 LiteLLM은 번호와 상관없이 최신으로 올린다.

중요한 이유: 2번 같은 침해로 키가 새면 몇 시간 안에 큰 청구서가 된다. 키를 쓰는 조직이 볼 로그와 신호가 공식 문서로 나왔다.

[Google Threat Intelligence](https://cloud.google.com/blog/topics/threat-intelligence/llmjacking-monetizing-compromised-cloud-models/) (벤더 리서치, 날짜만 11-02) · [The Register](https://www.theregister.com/security/2026/11/02/token-thieves-costing-companies-100k-per-day-in-illicit-llm-charges/5306967) (11-02 16:00 UTC)

### 5. (후속) CVE-2026-98432: Canonical이 "-proposed에 수정이 있다"고 공식화했다. 11-03에 나온 커널 USN에는 이 수정이 없다

- **Canonical 블로그(11-02 21:00 UTC, 11-03 10:26 갱신):** 수정 커널은 22.04 `5.15.0-203.213`, 24.04 `6.8.0-149.149`, 26.04 `7.0.0-43.43`이고 모두 -proposed에 있다. 16.04·18.04·20.04는 "Fix pending"이다. 원문: "we perform extensive tests on more than 2,000 certified servers and devices." 정식 배포 날짜는 없다.
- **완화책(원문):** Anthropic이 만든 미공개 익스플로잇이 MPTCP를 쓴다고 알려졌을 뿐이며, "The Ubuntu Security Team believes that security updates are the only appropriate mitigation."
- **USN 대조:** 11-03 11:03~11:05 UTC에 나온 커널 USN(USN-8959-1, USN-8961-1 등)에는 CVE-2026-98432가 없다. "오늘 커널 업데이트를 받았으니 끝났다"가 아니다.

중요한 이유: 직전 판 기술 1번의 후속이다. Ubuntu 22.04 이상에서 지금 막으려면 -proposed 커널을 직접 받아야 한다. 정식 업데이트를 기다리는 서버는 여전히 노출돼 있다.

[Canonical](https://ubuntu.com/blog/fixes-available-in-proposed-pocket-for-cve-2026-98432) (공식) · [Ubuntu CVE](https://ubuntu.com/security/CVE-2026-98432) · [USN-8959-1](https://ubuntu.com/security/notices/USN-8959-1) · [HN "Ubuntu still has not released patch"](https://news.ycombinator.com/item?id=50341462)

### 6. MCP 권고: 세션을 사용자에게 제대로 묶지 못한 결함이 MCP C# SDK와 SurrealDB에서 나왔다. MCP 서버로 위장해 클라우드 자격증명을 훔치는 npm 패키지도 있다

| 대상 | 내용 | 수정 | 권고 |
|---|---|---|---|
| MCP C# SDK `ModelContextProtocol.AspNetCore` (high) | 식별 클레임(`NameIdentifier`·`sub`·UPN)이 없는 인증 사용자는 모두 `null`이 되어, 원문대로 "compares `null == null`"로 서로 같은 사람이 된다. 다른 사용자의 세션 ID를 얻으면 그 세션의 도구·데이터를 쓴다. 세션 ID를 추측할 수 있다는 주장은 없다 | 1.4.2 / 2.3.1 | [GHSA-x863-g495-4cjc](https://github.com/modelcontextprotocol/csharp-sdk/security/advisories/GHSA-x863-g495-4cjc) (11-03 07:59 UTC) |
| SurrealDB 내장 MCP (high) | 이스케이프한 백틱으로 식별자를 빠져나가 쿼리를 주입한다 | 4.0.0 | [GHSA-cr79-9p8r-p5wx](https://github.com/surrealdb/surrealdb/security/advisories/GHSA-cr79-9p8r-p5wx) |
| SurrealDB MCP (high, NVD발) | 세션 지문을 사용자가 아니라 접근 방식 이름으로 만들어 세션 탈취가 된다 | 3.3.0 | [GHSA-hprh-85vv-3jpq](https://github.com/advisories/GHSA-hprh-85vv-3jpq) |
| AWS Serverless MCP Server (medium) | 프로젝트 이름의 경로 조작으로 범위 밖 파일을 읽는다 | 0.2.1 | [GHSA-r6mm-377j-qg8m](https://github.com/awslabs/mcp/security/advisories/GHSA-r6mm-377j-qg8m) |
| **악성** npm `@ifasn/{md,mdd,mmd,mmdd}-doc-mcp` | Markdown MCP 서버로 위장해 클라우드 메타데이터 서버에서 임시 자격증명을 빼 간다 | 즉시 삭제 | [GHSA-4h5p-w547-9qmv](https://github.com/advisories/GHSA-4h5p-w547-9qmv) 외 3건 |

- **SurrealDB 전체:** 창 안 저장소 권고가 27건(high 13·medium 12·low 2)이다. 그중 25건이 4.0.0(11-03 15:22 UTC)에서만 고쳐졌고, 3.x 수정은 3.3.4 한 건이다. 3.x를 쓰면 메이저 업그레이드가 사실상 필수다.
- **C# SDK 업그레이드 함정:** 1.4.2·2.3.1에는 OAuth 메타데이터 issuer 검증도 들어갔다. 옛 방식 OAuth discovery를 쓰는 배포는 릴리스 노트의 `AllowLegacyOAuthMetadataFallback` 안내를 먼저 본다. [2.3.1 릴리스](https://github.com/modelcontextprotocol/csharp-sdk/releases/tag/v2.3.1) · [1.4.2 릴리스](https://github.com/modelcontextprotocol/csharp-sdk/releases/tag/v1.4.2)

중요한 이유: MCP 서버를 인증 뒤에 둬도 세션이 사용자와 제대로 묶이지 않으면 권한이 섞인다. 같은 계열의 결함이 SDK와 서버 양쪽에서 같은 날 나왔다. API 키나 헤더 같은 커스텀 인증을 쓰는 C# MCP 서버는 바로 올려야 한다.

### 7. vLLM 권고 3건(0.33.0에서 수정): 이전 수정이 불완전해, 인증 없는 요청 하나로 엔진이 죽는 경로가 다시 열려 있었다

- **GHSA-q9v9(high) 원문:** "Unauthenticated EngineCore crash via out-of-range prompt token id under skip_tokenizer_init (incomplete fix of GHSA-25q3-v2hm-8vpf)". 영향 범위는 0.28.0 이상 0.33.0 미만이다.
- **GHSA-5r3c(high):** 요청 하나에 담는 미디어 전체 크기 상한이 없어 프런트엔드 자원을 고갈시킨다. 영향 범위는 0.6.0 이상 0.33.0 미만이다.
- **GHSA-qrj2(medium):** Speculators 설정의 `verifier.name_or_path` 경로 조작.
- **도구 섹션과의 관계:** 직전 판 도구 2번 v0.33.0이 이 셋을 모두 고친다. 다만 0.33.0은 CUDA 12.9 빌드를 끊었으므로, CUDA 12.9 환경은 CUDA 13으로 옮겨야 고쳐진다.

[GHSA-q9v9](https://github.com/vllm-project/vllm/security/advisories/GHSA-q9v9-qpjp-fc24) · [GHSA-5r3c](https://github.com/vllm-project/vllm/security/advisories/GHSA-5r3c-5x4v-8gjh) · [GHSA-qrj2](https://github.com/vllm-project/vllm/security/advisories/GHSA-qrj2-j723-m6w8) (공식, 11-02 19:48~19:50 UTC)

### 8. OpenAI 정렬 블로그: GPT-5.6부터 모든 응답에 활성값 분류기(프로브)를 돌리고 있다

- **원문:** "Starting from GPT-5.6, we have added another layer of defense to this stack: activation classifiers that run on all assistant completions." 프로브는 모델 자신의 내부 활성값을 읽는 가벼운 분류기로, 기존 텍스트 분류기에 더해진다.
- **한계(원문):** "limited to detecting only things we can explicitly train them to detect. Because they depend on the monitored model's activations, they need retraining for new base models".

중요한 이유: 출력 텍스트만 보는 필터보다 우회하기 어렵다는 점에서, 해석 가능성 연구가 실제 서비스 방어선에 들어갔다는 공식 확인이다.

[OpenAI Alignment](https://alignment.openai.com/training-activation-classifiers/) (공식, RSS 11-02)

### 9. OpenAI가 버그 바운티를 크게 올렸다: 핵심 제품 critical은 최대 3만 달러, 보너스 포함 최대 17.2만 달러

- **원문:** "A critical vulnerability in a core product such as ChatGPT, Codex, or the API Platform will now earn up to $30,000. Exceptional critical findings on those targets remain eligible for up to $100,000, and now up to $172,000 with bonuses."
- **예시 표:** ChatGPT 원클릭 계정 탈취는 예전 $6,500에서 보너스를 다 받으면 $51,577이 된다.
- **소급(원문):** "For bounties we awarded in the 30 days before this announcement, we'll pay you the difference".
- **보안 스프린트:** 다음 내부 스프린트를 외부 업체 Cathedral과 한다. 지난번에는 원문 "We dedicated 25% of our product engineers to finding and fixing weaknesses"였다.

중요한 이유: 11-02 판에서 다룬 ChatGPT 공유 링크 탈취 체인이 5,000달러를 받아 "싸다"는 평을 들은 직후다. 소급 조항이 있어 그 제보도 차액 대상일 수 있다.

[바운티 발표](https://openai.com/index/rewarding-defenders-with-bigger-bug-bounties/) · [보안 스프린트](https://openai.com/index/preparing-our-next-security-sprint-to-harden-openai/) (공식, 403이라 r.jina.ai로 본문 확인) · [The Decoder](https://the-decoder.com/bug-hunters-can-now-earn-up-to-172000-from-openai-as-company-ramps-up-security/) (11-03 10:42 UTC)

### 10. 짧게

- **Anthropic FlashProve(11-02):** LLM 추론이 약속한 모델에서 나왔는지 영지식 증명으로 확인하는 오픈소스 시제품이다. 원문: Llama 4 Maverick(4비트)에서 "a single H100 GPU generates proofs at around 110 tokens/second, with a proof size of ~19 MB per 1,024-token request". 원문대로 "~70-95x the GPU time of running the model itself on vLLM"이라 아직 비싸다. [글](https://www.anthropic.com/research/provable-inference) · [GitHub](https://github.com/anthropics/FlashProve) (공식)
- **(Calegari 논쟁 후속) Tao 블로그 기고 "Egoless Mathematics in the AI Era"(11-02 18:54 UTC, HN 179점):** Karthik C. S.·Amit Sahai·Jie Xue의 글이다. 원문: "A truth has become accessible; its worth is not diminished by the absence of our authorship." 직전 판 기술 7번 Calegari 글과 반대 방향의 응답이다. Tao는 다른 형식으로 쓴 원고를 AI로 변환했다고 달아 뒀다. [Tao 블로그](https://terrytao.wordpress.com/2026/11/02/egoless-mathematics-in-the-ai-era/) · [HN](https://news.ycombinator.com/item?id=50341736)
- **(미확인) Codex 학습 설정이 다시 켜졌다는 제보(HN 40점대):** 원문: "the setting had been silently reenabled in Codex. None of us knowingly opted back in." 공식 확인은 없고 재현되지 않는다는 댓글도 있다. Codex를 쓰면 설정의 "Improve the model for everyone"을 한 번 확인해 둔다. [HN](https://news.ycombinator.com/item?id=50352757) (커뮤니티)
- **(창 경계) OpenAI "Scaling Git at OpenAI"(RSS 11-02 15:30, HN 250점대):** Codex 때문에 늘어난 푸시·페치를 감당하려 Git 백엔드 Pando를 만들었다. 백엔드는 공개하지 않았다. [원문](https://openai.com/index/scaling-git-at-openai/) · [HN](https://news.ycombinator.com/item?id=50345924)
- **Carlini 체스 실험(HN 230점대):** 200MHz Pentium Pro에 맞춘 Stockfish 이식을 AI 모델들이 썼고, 원문: "estimated Elo of 3300 ± 87". 알고리즘 개선만으로 오래된 하드웨어에서 얻는 성능의 사례다. [원문](https://nicholas.carlini.com/writing/2026/algorithmic-improvements-chess.html) (11-01 게시, HN은 창 안)
- **Haystack 2.31.1:** `anyio`(CVE-2026-63374)와 `certifi`(eMudhra CA 키 유출) 최소 버전을 올렸다. 직접 고정해 둔 프로젝트도 같이 올린다(도구 짧게 참고).
- **직전 판 후속 중 새 진전 없음:** Northflank RCA(미공개), Trigger.dev 침해 경로, Chainlit·CSGHub·PandaWiki 미수정 권고(새 릴리스 없음), Cursor 가짜 Claude Code 확장, Trail of Bits 백포트. Claude Code 게임 재현 거부는 매체 보도가 이어졌지만 11-03 판에서 다룬 Osmani 설명 이후 새 입장은 없다.

## 써볼 만한 도구

창 안에 정식 릴리스가 없던 곳: Codex CLI(0.167.0 알파만), Gemini CLI(nightly만, 안정판은 10-27 v0.66.0), Cursor(변경 로그 10-20), opencode, Cline, Aider, Continue, OpenAI Agents Python, ADK Python·Go, MCP Python·TS·Go SDK, MCP Inspector, Playwright MCP, LiteLLM, LangGraph, MLX, LM Studio. Anthropic 플러그인·스킬 저장소도 창 안 커밋이 없었다. OpenAI API 변경 로그는 10-30이 최신이다. MCP C# SDK 보안 패치(1.4.2·2.3.1)는 기술 6번, vLLM 0.33.0으로 고쳐지는 권고 3건은 기술 7번에 적었다.

### 1. Claude Code 2.1.317: 계정 이메일을 네트워크로 보내기 전에 묻고, hook이 실패하면 동작을 막을 수 있다

- **한 줄 설명:** 보안·운영 수정이 많은 정기 릴리스다. Agent SDK TS 0.3.317과 Python 0.2.177이 같이 나왔다.
- **새것(원문):**
  - "Added a check that asks before a web or shell tool call sends your account email address to the network"
  - "Added `onFailure: "block"` for Agent SDK hook callbacks, so a callback that fails blocks the action instead of letting it run"
  - `/schedule`이 요청하지 않은 커넥터가 붙은 새 클라우드 예약 작업에 경고하고, 제거 방법을 알려 준다.
  - OTel에 `anthropic.plan`, `anthropic.seat_type`, `anthropic.provider`, `anthropic.custom_base_url`이 붙었다. Claude apps gateway admin API에 비용 보고 `GET /v1/organizations/analytics/cost`가 생겼다.
- **추천 이유:** 프롬프트 주입으로 에이전트가 계정 이메일을 웹·셸로 내보내는 경로에 확인 단계가 생겼다. 조직 정책 hook이 조용히 실패하면 동작이 그대로 실행되던 구멍은 `block`으로 막을 수 있다.
- **업그레이드 함정(원문):**
  - "Removed the Read tool's "file unchanged since your last Read" stub: reading an unchanged file again returns its content". 같은 파일을 다시 읽으면 내용 전체가 돌아오므로 토큰 사용이 늘 수 있다.
  - "Changed subagents and teammates on a different model from the main conversation to use that model's saved effort level or its default, instead of the session's effort level"
  - 디스크나 쿼터가 꽉 차서 기록을 잃으면 `claude -p`가 이제 exit 1을 낸다. 클라우드·self-hosted 러너 세션 안에서 실행한 `claude -p`는 그 세션의 ID를 더는 공유하지 않는다. 예전 동작은 `CLAUDE_CODE_DISABLE_NESTED_SESSION_ID=1`로 되돌린다. CI에서 종료 코드나 세션 기록에 기대는 스크립트는 확인이 필요하다.
- **보안 대조:** 조회 시점 `anthropics/claude-code` 저장소 권고는 10-28 이후 새것이 없다.
- **설치/사용:** `npm i -g @anthropic-ai/claude-code@2.1.317` · [릴리스](https://github.com/anthropics/claude-code/releases/tag/v2.1.317) (공식, npm 11-02 23:57 UTC, GitHub 11-03 01:25 UTC) · [Agent SDK TS 0.3.317](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.317) · [Python 0.2.177](https://github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.177)

### 2. Open WebUI 0.13.0: 채팅 옆에 터미널·브라우저·데스크톱 탭이 생겼다. 공개되지 않은 보안 수정이 들어 있다

- **한 줄 설명:** 셀프호스팅 채팅 UI가 코딩·브라우저 에이전트 작업 공간이 됐다.
- **새 기능(원문):**
  - 채팅 옆 탭 작업 공간: 파일, 여러 터미널, 아티팩트, 프리뷰 등.
  - 연결한 Open Terminal의 데스크톱을 마우스·키보드로 다루는 Desktop 탭(computer use), 모델과 같이 쓰는 Browser 탭.
  - Git 패널(diff, 커밋, 브랜치·worktree, PR), "/side"·"/btw" 사이드 채팅.
- **보안(원문):** "This release contains **security fixes**." 또 "These fixes are **not listed in this changelog**". 권고는 CVE가 배정되면 바로 낸다고 했다. 조회 시점 저장소 권고는 없어 범위를 대조하지 못했다. 10-28 판의 0.12.4 보안 수정도 같은 방식이었다.
- **추천 이유:** 운영 중인 인스턴스는 공개 전 보안 수정 때문에라도 올리는 편이 낫다.
- **업그레이드 함정(원문):**
  - DB 스키마가 바뀐다. 여러 워커·서버로 돌리면 "all instances must be updated simultaneously, rolling updates are not supported". 먼저 백업한다.
  - "**No sub-agent limits by default.**" 이전의 동시·백그라운드·반복 제한이 기본으로 풀렸다. 비용 통제가 필요하면 `SUBAGENTS_MAX_*` 환경 변수를 직접 설정한다.
  - 응답 하나의 도구 호출이 256번에서 "up to 1024 rounds"로 늘었다. `CHAT_RESPONSE_MAX_TOOL_CALL_ITERATIONS=256`이면 예전 값이다.
- **설치/사용:** `pip install -U open-webui==0.13.0` · [릴리스](https://github.com/open-webui/open-webui/releases/tag/v0.13.0) (공식, 11-03 12:23 UTC)

### 3. OpenAI Agents SDK JS 0.22.0-alpha.1: Codex 하네스를 SDK 안에 넣었다 (알파)

- **한 줄 설명(원문):** "This alpha introduces the embedded Codex harness in the OpenAI Agents SDK for JavaScript and TypeScript."
- **새것(CHANGELOG-alpha 원문):** `SessionState` 체크포인트 저장·복원과 유휴 세션 fork, 대화 압축, 그리고 "Test native sessions against local scripted responses through `@openai/agents/testing`, without a live model or API key."
- **추천 이유:** Codex와 같은 하네스(모델 턴·도구 실행·기록·압축)를 앱에 넣어, 긴 세션 관리를 직접 만들지 않아도 된다. 모델 없이 도는 테스트 도구도 같이 왔다.
- **함정:**
  - npm에는 아직 없다(조회 시점 `latest`는 0.21.0). 플랫폼별 tarball을 `file:`로 의존해야 하고 Node.js 24가 필요하다. 원문: "The Linux x64 bundle requires glibc 2.39 or later and OpenSSL 3."
  - 기존 Agent·Runner API는 `@openai/agents/classic`으로 옮겨졌다. 원문: "Converting an active classic `RunState` into native `SessionState` is unsupported."
- **설치/사용:** [릴리스](https://github.com/openai/openai-agents-js/releases/tag/v0.22.0-alpha.1) (공식, 프리릴리스, 11-03 15:36 UTC로 창 종료 24분 전) · [INSTALL.md](https://github.com/openai/openai-agents-js/releases/download/v0.22.0-alpha.1/INSTALL.md) · [CHANGELOG-alpha.md](https://github.com/openai/openai-agents-js/releases/download/v0.22.0-alpha.1/CHANGELOG-alpha.md)

### 4. SGLang 0.5.23: GPU 메모리보다 큰 MoE를 소비자 GPU 하나로 돌리는 Paged Experts. 호환성 변경이 많다

- **새것(원문):** "*1007 PRs from 257 contributors.*"
  - "Paged Experts (opt-in) runs MoE models larger than GPU memory on a single consumer GPU, up to 30x faster than CPU offload"(자사 측정)
  - "SGLang now runs on Google TPU7x through SGLang-TorchTPU". 양자화 MoE에서 LoRA 어댑터를 서빙한다.
- **보안 섹션:** PyTorch 체크포인트를 읽을 때 쓰던 위험한 pickle fallback을 없앴다. 조회 시점 저장소 권고는 없다.
- **업그레이드 함정(원문):**
  - "**Legacy tar-format `.pt` checkpoints no longer load.**" safetensors나 zip 형식으로 바꿔야 한다.
  - DeepSeek-V4 계열: 도구 호출 ID가 "the official `call_00_...` format"으로 바뀌고, "a numeric `reasoning_effort` on V4 returns 400"이다.
  - PD 분리 배포는 "Upgrade prefill and decode together". `--speculative-skip-dp-mlp-sync` 옵션은 삭제됐다.
- **설치/사용:** `docker pull lmsysorg/sglang:v0.5.23` · [릴리스](https://github.com/sgl-project/sglang/releases/tag/v0.5.23) (공식, 릴리스 노트 11-02 22:14 UTC. PyPI·Docker 이미지는 10-30에 먼저 올라왔다)

### 5. Ollama 0.40.14: `ollama launch claude`가 128k 출력 상한에 걸리던 문제를 고쳤다. OpenAI 호환 경로의 샘플러 기본값이 바뀐다

- **원문:** "`ollama launch claude` now applies the model's output token limit for Claude Code, so cloud models no longer run into the 128k default cap". `ollama launch codex`는 설정 전에 ChatGPT 라우팅을 확인한다.
- **함정(동작 변경, 원문):** "OpenAI-compatible endpoints no longer override a model's packaged sampler defaults — clients that send no `temperature`/`top_p` get the model's own values instead of 1.0". OpenAI 호환 경로로 붙인 앱은 출력이 달라질 수 있다.
- **설치/사용:** [릴리스](https://github.com/ollama/ollama/releases/tag/v0.40.14) (공식, 11-03 01:40 UTC. 바이너리는 06:47 UTC에 올라왔다)

### 6. Video Studio Skill: Claude Code 스킬 하나로 실제 화면 캡처부터 편집까지 제품 데모 영상을 만든다 (HN 110점대)

- **한 줄 설명(README 원문):** "A local video-production workspace driven by the **`/devrel-video` Claude Code skill**." Playwright CLI로 실제 제품 화면을 캡처하고, HyperFrames로 타이틀·챕터·CTA를 합성하고, FFmpeg로 마스터링한다.
- **추천 이유:** 기존 도구를 엮는 스킬이라 영상을 brief와 브랜드 설정에서 다시 만들 수 있다. 무음 워크스루부터 시작해 내레이션·음악·자막을 필요할 때 더한다. MIT 라이선스다.
- **주의(README 원문):** 데모 GIF는 "stops **before capture and rendering**; it does not claim a video was produced." 전체 제작에는 "Node.js 22+, FFmpeg/ffprobe, Chromium"이 필요하고, 내레이션·아바타는 직접 마련한 외부 서비스나 GPU가 있어야 한다.
- **설치/사용:** 저장소를 clone한 뒤 그 디렉터리에서 `claude`를 실행하고 `/devrel-video`를 쓴다 · [저장소](https://github.com/aaitor/skill-video-studio) · [v1.0.1](https://github.com/aaitor/skill-video-studio/releases/tag/v1.0.1) (커뮤니티, 11-02 18:11 UTC) · [HN](https://news.ycombinator.com/item?id=50340992)

### 7. llm 0.37: `llm logs off`를 켜도 `llm chat`이 기록하던 문제와, 거부한 도구가 실행되던 문제를 고쳤다

- **원문:**
  - "`llm chat` now respects `llm logs off`. Previously it logged every response even when logging had been turned off."
  - "Previously a tool rejected by `before_call` would still run."
- **추천 이유:** 로그를 끈 줄 알고 민감한 대화를 `llm chat`으로 했다면 로컬 로그 DB에 남아 있을 수 있다. 도구 승인 hook에 기대는 스크립트도 이 버전이 필요하다.
- **함정(원문):** "dropped support for Python 3.10". 기본 모델이 GPT-6 Luna로 바뀌었다. `gpt-4`, `o1`, `o3-mini` 등 종료된 OpenAI 모델이 목록에서 빠졌다.
- **설치/사용:** `pip install -U llm==0.37` · [릴리스](https://github.com/simonw/llm/releases/tag/0.37) (공식, 11-02 23:49 UTC) · [Simon Willison 글](https://simonwillison.net/2026/Nov/2/llm/)

### 8. Vercel MCP: ChatGPT 안에 배포 카드를 띄우고, 심사 없이 CIMD 클라이언트를 받는다

- **배포 카드(원문):** "Vercel MCP can now show your deployments as an interactive card inside ChatGPT and other chat clients that support MCP Apps." 배포 상태, 빌드 로그, 커밋 정보를 보여 준다.
- **CIMD(원문):** "You still log in and approve each connection, but the client no longer has to be reviewed by Vercel first."
- **주의(원문):** "CIMD clients don't get the Vercel verified badge, so the hostname is what you're trusting when you approve." 동의 화면의 호스트명을 보고 승인해야 한다.
- **설치/사용:** [배포 카드](https://vercel.com/changelog/monitor-vercel-deployments-in-chatgpt) · [CIMD](https://vercel.com/changelog/vercel-mcp-now-supports-client-id-metadata-documents) (공식, 11-02)

### 9. Pydantic AI v2.65.0: 모델이 거부하면 다음 모델로 넘어간다

- **새것(원문):** "Add WebSocket transport to OpenAI Responses", 대화를 메시지 로그로 한 번만 저장하는 `ConversationStore`와 충돌 감지용 `Conversation.revision`.
- **동작 변경(원문):** `ModelFallback`이 "when a model refuses" 다음 모델로 넘어간다. Z.ai 잔액·요금제 429는 `ModelQuotaExceededError`로 바뀐다. Google에는 "a server deadline of at least 20 minutes"를 보낸다. 보안 성격의 수정으로 "Mark `<context>` elements in user and tool content as not from the application"이 들어갔다.
- **함정:** 거부 응답에서 다른 모델로 넘어가는 동작은 안전 정책상 원하지 않을 수 있다. fallback 체인을 쓰면 확인한다.
- **설치/사용:** `pip install -U pydantic-ai==2.65.0` · [릴리스](https://github.com/pydantic/pydantic-ai/releases/tag/v2.65.0) (공식, 11-03 01:25 UTC)

### 10. (후속) claude-mem v15.9.0와 Cloudflare Agents SDK 0.33.0

- **claude-mem v15.9.0(11-02 18:03 UTC):** 직전 판 도구 1번의 후속이다. 원문: "Turning off error reporting now also stops errors already saved on disk from being sent." 또 "Other websites running on localhost can no longer send writes to the worker". Slack·Discord 웹훅 등 비밀 값을 저장 전에 더 가린다. 제보 이슈 #5176은 조회 시점에도 open이고, 이미 올라간 프롬프트 처리에는 새 소식이 없다. `npx claude-mem@15.9.0 install` · [릴리스](https://github.com/thedotmack/claude-mem/releases/tag/v15.9.0)
- **Cloudflare Agents SDK 0.33.0(11-03 13:14 UTC):** 직전 판 도구 7번 pi v1.5.0을 Cloudflare가 넣었다. 원문: "Over SQLite this cuts storage transactions about 27x for long answers"(자사 측정). 함정(원문): "Once a conversation runs on Pi 1.5, Pi 1.4 cannot read it, so a deployment cannot roll back past this release." 또 "A crash can now lose up to about a second of streamed output." `session.events()`는 `session.watch()`로 바뀌었다. [릴리스](https://github.com/cloudflare/agents/releases/tag/agents%400.33.0) · [Cloudflare 변경 로그](https://developers.cloudflare.com/changelog/post/2026-11-03-pi-harness-pi-1-5/)

### 짧게

- **Context7 MCP 4.5.0·ctx7 0.9.0(11-03 06:41 UTC):** 리서치 프리뷰를 켜면 `submit-docs-feedback` 도구가 생기고, 원문대로 "the server instructions also ask the agent to send this feedback after it answers". 켜기 전에 이 점을 알아 둔다. 설정 안 된 `${CONTEXT7_API_KEY}` 자리표시자를 키로 보내던 문제도 고쳤다. `ctx7 search <query>`는 라이브러리 ID 없이 문서를 찾는다. [MCP 4.5.0](https://github.com/upstash/context7/releases/tag/%40upstash/context7-mcp%404.5.0) · [ctx7 0.9.0](https://github.com/upstash/context7/releases/tag/ctx7%400.9.0)
- **GitHub MCP Server 2.1.0(11-03 11:11 UTC):** 이슈에 붙는 아티팩트(계획·지침·참고 자료)를 다루는 실험 도구 두 개가 들어갔다. PR 원문: "Both tools require explicit opt-in through the `issue_artifacts` feature flag." 2^63 이상 정수 파라미터를 거부한다. [릴리스](https://github.com/github/github-mcp-server/releases/tag/v2.1.0) · [PR #3554](https://github.com/github/github-mcp-server/pull/3554)
- **MCP Ruby SDK 1.9.0(11-03 14:03 UTC):** Skills extension(SEP-2640)을 넣어 서버가 Agent Skills를 게시할 수 있다. [릴리스](https://github.com/modelcontextprotocol/ruby-sdk/releases/tag/v1.9.0)
- **Anthropic SDK 일괄 마이너 릴리스(11-02 19:07~19:17 UTC):** Python 2.4.0, TS 1.4.0 등이 "add GA methods for analytics, spend limits and RBAC"를 넣었다. 플랫폼 릴리스 노트에는 조회 시점에 GA 공지가 없다. [Python 2.4.0](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v2.4.0) · [TS 1.4.0](https://github.com/anthropics/anthropic-sdk-typescript/releases/tag/sdk-v1.4.0)
- **Haystack 2.31.1(11-03 09:27 UTC):** `anyio>=4.14.2`(CVE-2026-63374)와 `certifi>=2026.10.19`(eMudhra CA 키 유출 대응)를 요구한다. 원문: "If you pin `certifi`, raise the pin to at least 2026.10.19." [릴리스](https://github.com/deepset-ai/haystack/releases/tag/v2.31.1)
- **Genkit CLI 1.45.0(11-03 02:09 UTC):** Reflection v2 WebSocket 서버가 기본으로 `127.0.0.1`에만 바인딩된다. 원격·컨테이너 런타임은 `--reflection-v2-host`가 필요하다. [릴리스](https://github.com/genkit-ai/genkit/releases/tag/genkit-cli%401.45.0)
- **(후속) FastMCP 4.1.6(11-03 02:52 UTC):** 직전 판 4.1.5의 후속으로, `ClientGroup`의 도구 경로 수정 하나다. [릴리스](https://github.com/PrefectHQ/fastmcp/releases/tag/v4.1.6)
- **Momentic(Launch HN, 11-02 17:05 UTC):** 앱을 직접 써 보며 버그를 찾는 상용 QA 에이전트다. 원격 MCP 서버도 레지스트리에 올렸다. 성능은 회사 주장이다. [사이트](https://momentic.ai/) · [HN](https://news.ycombinator.com/item?id=50339913)
- **(창 경계) Google ADK Java v1.14.0(11-02 15:21 UTC, 창 시작 39분 전):** 사용자 입력을 받으려고 워크플로를 멈추는 기능과 ADK·Spring AI 양방향 변환이 들어갔다. [릴리스](https://github.com/google/adk-java/releases/tag/v1.14.0)

## 주목할 점

- **자기 개선을 둘러싼 말과 증거의 간격.** Anthropic은 증권 공시에서 RSP의 가속 기준에 "곧" 닿을 수 있다고 적고 사람의 승인 절차를 연구 과정에 넣겠다고 했다. 같은 날 "모델이 다른 모델의 가중치를 고친다"는 글이 크게 퍼졌지만, 실제 결과는 도구와 반복 실행으로 알려진 루프 문제 하나를 끈 것에 가까웠다. 다음에 볼 것은 Anthropic이 RSP를 실제로 어떻게 고치는지, 공모가 범위를 채울 때 이 위험을 어떻게 설명하는지, MIT 결과를 다른 모델과 과제로 재현한 사례가 나오는지다.
- **AI 공격 도구가 사고 보고서의 단골이 됐다.** ARTEX가 국가 배후 공격, 보안 연구 단체, 인증 SaaS 침해에 연달아 이름을 올렸고, 털린 AI 키는 몇 시간 안에 큰 청구서가 된다. 다음에 볼 것은 WorkOS·Trigger.dev·Northflank의 침해 경로 공개, Ubuntu의 CVE-2026-98432 정식 배포, Open WebUI 0.13.0에 숨은 보안 수정의 권고 공개다.

## 조사 제약

- **본문을 직접 읽지 못한 원문:** openai.com은 403이라 r.jina.ai로 읽었다(버그 바운티, 보안 스프린트, Scaling Git). NYT는 유료벽이라 Google 공식 글로 대신했다. DIVD LinkedIn과 Bitdefender 원보고서는 BleepingComputer 인용만 봤다. WorkOS 고객 통지문은 HN 전재본만 봤고, 공식 공지에는 ARTEX가 나오지 않는다. Gartner 보고서 원문은 보지 못했다. npmjs.com 패키지 페이지(403)는 registry API로 대신했다.
- **시각이 날짜만 있는 원문:** message threads 글과 릴리스 노트(11-02, Wayback으로 창 안 게시 추정), Gemini API 변경 로그(11-02), MIT 가중치 편집 글(11-02, HN 첫 게시 18:21 UTC), WorkOS 공지·GTIG LLMjacking 보고서·FlashProve(11-02), OpenAI 블로그 글(RSS 날짜만 신뢰), Cloudflare 변경 로그(11-03). ChatGPT 장애 시각은 서브에이전트가 상태 페이지에서 읽은 값이다.
- **확인하지 못한 것:**
  - Gemini 2.5 Pro 자동 라우팅 요청에서 옛 파라미터(`thinking_budget` 등)가 어떻게 처리되는지.
  - Open WebUI 0.13.0 보안 수정의 범위(권고 미공개), GHSA·NVD와 어긋나는 GTIG 보고서의 LiteLLM CVE 번호.
  - Revolut의 자체 호스팅 GLM 전환(투자자 트윗뿐, 회사 자료 없음), Codex 학습 설정 재활성화 제보.
  - Mythos 5.5·GPT-6.2 Astra 출시 여부(창 안 새 흔적 없음).
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 스토리 약 1,190개를 키워드와 점수순으로 훑었다.
  - 전역 GHSA: unreviewed 462건·malware 77건은 키워드로 걸렀고 재현하지 않았다.
  - MCP 레지스트리: 창 안 게시 4,289건 중 대형 벤더 네임스페이스만 봤다.
  - JS 렌더링이라 날짜를 못 본 곳: Meta AI 블로그, Microsoft AI, qwen.ai, MiniMax 뉴스, Codex 제품 변경 로그. x.com 타임라인과 중국 랩의 WeChat 공지는 직접 보지 못했다.
- **자사 측정:** message threads TTFT 35%, SGLang Paged Experts 30배, Cloudflare Pi 1.5 27배, FlashProve 처리량, MIT 가중치 편집 수치는 각 회사·저자 측정이며 독립 검증 전이다.
