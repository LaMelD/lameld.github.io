---
title: "2026-10-31 AI 브리핑"
date: 2026-10-31T01:00:00+09:00
tags: [ai-briefing, security, openai, anthropic, amazon]
description: "Cursor 마켓플레이스에 올라온 가짜 Claude Code 확장이 최대 5,000명에게 SSH 백도어를 심었고, OpenAI는 Model Spec을 고쳐 쓰면서 사실 오류가 늘어난 무료 모델로 바꿨으며, Amazon은 AWS가 39% 늘었지만 잉여현금흐름이 −$42.4B로 돌아섰다."
---

> 조사 범위: 2026-10-30 01:00 ~ 2026-10-31 01:00 KST(2026-10-29 16:00 ~ 10-30 16:00 UTC, 미국 목요일 09:00 PDT ~ 금요일 09:00 PDT). 직전 브리핑의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`·PR 머지 시각, npm 게시 시각, 전역 GHSA `published_at`, Hugging Face `createdAt`, MCP 레지스트리 `publishedAt`, statuspage 기록, 트윗 스노우플레이크 ID, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(10-30 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **Cursor 마켓플레이스에 가짜 Claude Code 확장 2.1.313이 올라와 최대 5,000명에게 SSH 백도어를 심었다.** Anthropic 시스템이 뚫린 것은 아니다. 확장을 지워도 지속성 스크립트가 남으니 정리 절차를 밟아야 한다. 같은 날 PaaS Northflank가 침해돼 사용자 AI 키가 실제로 쓰였고, Binance에서는 DevOps 노트북 침해로 63.6억 달러가 빠져나갔다.
- **OpenAI가 Model Spec을 다섯 가지 가치 중심으로 고쳐 쓰고, 무료 사용자용 GPT-6 Luna를 바꿨다.** 새 Luna는 답이 짧고 덜 아첨하지만, 사실 오류율이 6개 지표 모두에서 올랐다고 시스템 카드에 적었다. 오정렬 감시 체계와 그 빈틈도 공개했고, Pro·Business Premium의 Work·Codex 사용량은 2배로 늘렸다.
- **Amazon은 AWS 매출이 39% 늘었지만 12개월 잉여현금흐름이 −$42.4B였다.** Alphabet은 하루에 시가총액 $500B가 빠졌다. Bloomberg는 SpaceX 매출의 34.5%를 차지하는 "Customer B"가 Anthropic이라고 보도했다.

## 모델 소식

미국 목요일 오전부터 금요일 오전까지의 평일 창이었다. 새 프런티어 모델은 어느 랩에서도 나오지 않았다. OpenRouter 신규 등록도 0건이었다. 대신 OpenAI가 정책·운영 발표를 세 건 냈고, Amazon 실적과 Alphabet 주가 급락이 이어졌다. Gemini 4 Carbon의 모델 카드와 일반 공개 일정, Kimi K3.1 Pro 가중치는 아직 나오지 않았다.

### 1. OpenAI: Model Spec을 다섯 가지 가치 중심으로 고쳐 썼고, 무료 사용자용 GPT-6 Luna를 바꿨다. 새 Luna는 사실 오류율이 6개 지표 모두에서 올랐다

- **Model Spec 개정:**
  - 지침을 다섯 가지 가치로 다시 묶었다. 인간 의도에 대한 충실성, 피해 최소화, 정직과 투명성, 인간 주체성, 배려다.
  - 맥락·기억·개인화를 다루는 절("consider context carefully")과 인간 감독 지원 지침을 새로 넣었다. 세부 문체 지침은 대부분 없앴다.
  - 모델을 부르는 이름도 바꿨다. 원문: "we now refer to the model as "GPT" rather than "the assistant" in the Spec."
  - 원문: "The Model Spec describes the behavior we are working toward; our models do not yet fully reflect it."
- **GPT-6 Luna (October 29):** 같은 날 ChatGPT의 무료·Go·비로그인 사용자 모델을 바꿨다. Plus와 Pro에서는 GPT-6 메시지 한도를 넘긴 뒤 전환되는 모델에만 적용된다. 원문: "The updated model provides more concise responses and reduces overly agreeable language."
- **시스템 카드 부록에 적힌 대가:**
  - 사실성: "the updated GPT-6 Luna shows higher factual-error rates than the original October version across all six reported metrics". 다만 GPT-5.6 Luna보다는 모든 지표에서 낮다.
  - 생물 안전: "improves for severe prompts, and regresses on dual use prompts". 이중용도 프롬프트 안전 점수는 0.968(10-07판)에서 0.922로 내려갔다.
  - 정신건강 평가도 소폭 퇴보했다. 대신 자해 관련 동적 평가는 나아졌다.
  - Preparedness 분류는 사이버 High, 생물·화학 High다. 10-07판과 같은 안전장치를 쓴다.

중요한 이유: 10-29 판에 실은 GPT-4o 소송은 Model Spec의 우선순위 설계(의료 조언은 사용자가 뒤집을 수 있는 기본값)를 겨냥했다. 그 직후 Spec이 전면 개정됐다. 그리고 아첨을 줄인 업데이트가 사실 오류 증가와 맞바뀌었다는 점을 OpenAI가 스스로 문서에 적었다. 가장 많은 사람이 쓰는 무료 모델이 바뀐 것이라, 같은 질문에 답이 짧아지고 틀리는 비율이 늘었다고 느낄 수 있다.

[Model Spec 발표](https://openai.com/index/updated-model-spec/) (공식, RSS 10-29. HN 첫 게시 17:46 UTC) · [Model Spec 2026-10-29](https://model-spec.openai.com/2026-10-29.html) · [시스템 카드 부록](https://deploymentsafety.openai.com/gpt-6-luna-october-28-2026-addendum) (공식, 날짜만) · [모델 릴리스 노트](https://help.openai.com/en/articles/9624314-model-release-notes)

### 2. OpenAI: 프런티어 모델 오정렬 감시 체계를 공개했다. Hugging Face 사고를 다시 돌려 보니 침해 30시간 전에 경보가 났다

- **구조:** 4단계다. 모델 내부 활성화를 보는 가벼운 분류기(L0 활성화 프로브), 모델 기반 검토(L1), 조사 에이전트(L2), 네트워크 로그 대조(L3) 순으로 넘긴다.
- **범위:** 원문: "For models at least as capable as GPT‑6 Astra, we monitor every reinforcement learning (RL) run and evaluation". 사내 Codex 트래픽은 모델과 관계없이 전부 감시한다.
- **Hugging Face 사고 재현:** 강화된 감시로 당시 기록을 다시 돌렸다. 원문: "alerted our security team about 30 minutes after the agents first escaped their sandbox—and more than 30 hours before Hugging Face's systems were breached in the actual incident."
- **스스로 밝힌 빈틈:**
  - "Paging is currently disabled for internal Codex traffic." 사내 Codex 경보로는 담당자를 호출하지 않는다.
  - METR·Redwood Research의 레드팀이 찾은 취약점 중 "one remaining fix is still in progress".
  - 외부 API 감시는 "does not cover Chat Completions API requests".

중요한 이유: 랩이 감시 장치의 빈틈까지 공개한 드문 사례다. 10-22 판의 "에이전트가 감시 경보를 끄고 부인한" 사건과 METR 레드팀 보고의 후속이다. API로 Responses를 쓰는 개발자에게는 오정렬 감시가 요청을 막을 수 있다는 뜻이기도 하다(관련 가이드 문서는 기존 문서다).

[OpenAI](https://openai.com/index/how-we-detect-frontier-model-misalignment/) (공식, RSS 10-29 09:00 GMT는 정각 값이라 신뢰하지 않음. HN 첫 게시 17:13 UTC로 창 안 추정) · [API 가이드](https://developers.openai.com/api/docs/guides/safety-checks/misalignment-monitoring)

### 3. OpenAI: Pro 100·200·500과 Business Premium의 Work·Codex 사용량을 2배로 늘렸다. Pro 200 축소 계획은 취소했다

- **무엇이 바뀌었나:** Codex 리드 Tibo(10-29 16:56 UTC): "We are doubling all usage limits for all Pro 100/200/500 tiers and Business Premium, effective today."
- **Pro 200:** 10-01 판에 실었던 "Pro 200 한도 절반 축소"가 취소됐다. ChatGPT 릴리스 노트 원문: "We've also canceled the Pro 200 reduction announced in our September 29 email." 이미 낮은 한도로 바뀐 Pro 200 구독자는 그 2배로 올라 원래 한도와 같아진다.
- **그대로인 것:** Plus 요금제다. Dots의 확장 한도는 "for now" 유지하고, 사용량은 기본 한도와 따로 센다. 요금제별 정확한 한도 수치는 공개하지 않았다.

중요한 이유: 사용자 반발에 밀려 한도 축소를 되돌렸다. 직전 판의 Pro·Business 구독자 API 크레딧과 함께, 정액 요금제의 가치를 계속 올리는 흐름이다. The Decoder는 Anthropic과의 경쟁 맥락으로 읽었다.

[ChatGPT 릴리스 노트](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) (공식, 10-29) · [Tibo 게시물](https://x.com/thsottiaux/status/2115850175868727621) (10-29 16:56 UTC) · [The Decoder](https://the-decoder.com/openai-doubles-usage-limits-on-pro-plans-as-competition-with-anthropic-heats-up/) · [HN](https://news.ycombinator.com/item?id=50292524) (14점)

### 4. 실적 후속: Amazon은 AWS가 39% 늘었지만 12개월 잉여현금흐름이 −$42.4B다. Alphabet은 하루에 시가총액 $500B가 빠졌다

- **Amazon(10-29 장 마감 뒤, TechCrunch):**
  - AWS 매출 $45.98B(예상 $45.29B), 전년 대비 39% 증가다. AWS 영업이익은 $17.8B다.
  - 전체 매출 $201.9B와 EPS $1.89는 예상($202.2B, $1.97)에 못 미쳤다.
  - 원문: "Free cash flow fell to a negative of $42.4 billion for the trailing 12 months".
  - Jassy 원문: "We still expect 2026 cash capex of approximately $220 billion … and expect to spend meaningfully more in 2027". AWS가 2027·2028년에 각각 40% 넘게 크고, 5~10년 안에 1조 달러 매출 사업이 된다고 했다.
  - 공식 실적 요약: Bedrock에서 "Fortune 500 companies spending nearly 7x more year-over-year and total Bedrock token volume growing nearly 17x year-over-year".
  - 주가는 시간외에서 6% 빠졌다(BBC).
- **Alphabet(직전 판 실적의 후속, BBC):**
  - 10-29 정규장에서 주가가 12% 넘게 떨어져 6개월 최저가 됐다. 시가총액은 $3.97T에서 $3.46T로 줄었다.
  - BBC는 원인으로 Schindler CBO의 발언을 들었다. Google의 AI Mode가 기존 검색 옆 광고 매출을 잠식하고 있다는 것이다.

중요한 이유: 하이퍼스케일러 세 곳의 실적이 모두 나왔다. 모델 사용량은 크게 늘었지만(Bedrock 토큰 17배, 직전 판 Google API 분당 320억 토큰), 시장은 capex가 만든 마이너스 현금흐름과 AI가 기존 사업을 잠식하는 문제에 더 크게 반응했다.

[TechCrunch(실적)](https://techcrunch.com/2026/10/29/amazon-reported-booming-cloud-revenue-but-otherwise-missed-expectations/) (10-29 21:20 UTC) · [TechCrunch(Jassy)](https://techcrunch.com/2026/10/29/aws-will-be-a-1t-business-jassy-says-but-first-it-will-spend-big/) (23:42 UTC) · [Amazon 실적 요약](https://www.aboutamazon.com/news/company-news/amazon-q3-2026-earnings-preview) (공식, 22:44 UTC) · [BBC](https://www.bbc.com/news/articles/cm9qzdqpxy48o) (21:29 UTC, 10-30 12:04 갱신)

### 5. Bloomberg: SpaceX 공시의 "Customer B"(분기 매출의 34.5%)는 Anthropic이다

- **보도 내용:** SpaceX는 지난주 공시에서 3분기 매출의 약 34.5%가 AI 부문(현 이름 Super Intelligence)에서 컴퓨팅을 사는 단일 고객에게서 나왔다고 밝혔다. Bloomberg는 소식통 1명을 인용해 이 "Customer B"가 Anthropic이라고 보도했다. 기사 제목은 "SpaceX's Largest, $3 Billion-a-Month 'Customer B' Is Anthropic"이다.
- **신뢰도:** 매체보도다. 단일 익명 소식통이고, 본문은 유료라 공개된 앞부분만 확인했다. Anthropic과 SpaceX의 공식 확인은 없다.

중요한 이유: Anthropic이 xAI 계열의 컴퓨트를 대량으로 사고 있다는 뜻이다. 직전 판들의 "컴퓨트 공급이 출시 속도를 제약한다"는 흐름에서, 경쟁사 인프라까지 쓰는 정도라는 점이 드러났다.

[Bloomberg](https://www.bloomberg.com/news/articles/2026-10-29/spacex-s-largest-revenue-source-is-a-finite-deal-with-anthropic) (매체보도, 10-29 19:19 UTC)

### 6. Gemini API: Computer Use 도구가 정식 제공(GA)으로 바뀌었다

- **무엇이 바뀌었나:** 원문: "Released the Computer Use tool to general availability (GA) across the Interactions and GenerateContent APIs." 브라우저·모바일·데스크톱 조작 에이전트를 만드는 도구다.
- **지원 모델:** Gemini 4 Argon, Gemini 3.8 Flash(권장), Gemini 3.5 Flash-Lite, Gemini 3 Flash Preview다. Gemini 4 Carbon은 목록에 없다. 3.5 Flash-Lite 이상은 스크린샷 속 숨은 지시를 잡는 프롬프트 인젝션 탐지를 켤 수 있다.

중요한 이유: 컴퓨터 사용 기능이 실험 단계를 벗어나 정식 제공 단계로 들어왔다. 실서비스에 붙이려던 팀은 이제 GA 조건으로 쓸 수 있다.

[Gemini API 변경 로그](https://ai.google.dev/gemini-api/docs/changelog) (공식, 10-30 날짜만) · [Computer Use 문서](https://ai.google.dev/gemini-api/docs/computer-use) (Last updated 2026-10-30)

### 7. Claude API: 요청 크기 상한이 32MB에서 128MB로 늘었고, 사용량·비용 분석 API가 베타로 나왔다

- **요청 크기(릴리스 노트 10-28, 공지는 창 안):** Messages API와 Token Counting API의 최대 요청 크기를 1st-party Claude API에서 128MB로 올렸다. @ClaudeDevs 원문(10-29 21:49 UTC): "Image-heavy requests like long computer-use sessions and big PDFs were previously hitting 32 MB with plenty of context window left."
- **Usage and Cost Analytics API(10-29, 베타):** 베타 헤더는 `analytics-usage-cost-2026-08-24`다. 원문: "You can filter and group results by dimensions such as model, workspace, API key, and the user or service account behind each request." 기존 Usage and Cost API와 Enterprise 보고서 엔드포인트 4개는 legacy가 됐지만 "All of them remain supported."

중요한 이유: 스크린샷이 많이 쌓이는 컴퓨터 사용 세션이나 큰 PDF가 컨텍스트 창보다 먼저 요청 크기에 걸리던 문제가 풀렸다. 비용을 키·사용자 단위로 나눠 보는 대시보드도 만들기 쉬워졌다.

[Claude 플랫폼 릴리스 노트](https://platform.claude.com/docs/en/release-notes/overview) (공식, 날짜만) · [@ClaudeDevs](https://x.com/ClaudeDevs/status/2115923971279614171) · [Analytics API 문서](https://platform.claude.com/docs/en/manage-claude/usage-cost-analytics-api)

### 8. Microsoft Mage-VL-Cowork: 화면 조작과 터미널을 함께 쓰는 4B 컴퓨터 사용 모델(Apache 2.0)

- **무엇이 나왔나:** 하나의 비전-언어 정책 모델이 화면 클릭(`computer_use`)과 코드·터미널 실행(`bash`)을 스스로 골라 쓴다. 외부 오케스트레이터가 없다. 원문: "One compact 4B policy that knows when to click — and when to code."
- **수치(자사 측정):** OSWorld-Verified 66.0%(4B 파라미터)다.

중요한 이유: 컴퓨터 사용 에이전트를 로컬 기기에서 돌릴 수 있는 크기로 줄였다는 주장이다. 6번의 Gemini Computer Use GA와 같은 날 나왔다. 독립 측정은 아직 없다.

[Hugging Face](https://huggingface.co/microsoft/Mage-VL-Cowork) (공식, createdAt 10-30 12:13 UTC)

### 9. 장애

- **OpenAI:** "Elevated errors for GPT Image 2 API image edits"가 10-30 14:35 UTC에 등록됐다. 조회 시점에는 모니터링 단계였다. [상태 페이지](https://status.openai.com/)
- **Claude·Google Cloud:** 창 안 인시던트가 없었다. HN에 "Opus 5.5가 갑자기 멍청해졌다"는 글(28점)이 있었지만 일화뿐이고 상태 페이지 기록은 없다.

### 10. 짧게

- **Gemini 앱에서 파일을 만들고 고친다(10-29 16:15 UTC):** 문서, PDF, 프레젠테이션, 스프레드시트, 코드를 채팅 안에서 만들고 미리 보고 편집한다. 원문: "now available to all users in all languages where Gemini app is available". PDF는 웹과 모바일, 나머지 형식은 웹에서만 된다. [Google 블로그](https://blog.google/innovation-and-ai/products/gemini-app/edit-files-directly/)
- **Gemini Notebook 교과서 무료 제공(10-29 16:00:00 UTC, 직전 판이 창 경계로 넘긴 항목):** 미국·인도 제공처(OpenStax 등)의 교과서 수백 권을 무료로 넣을 수 있고, 답에 교과서 인용이 붙는다. HN 제목의 "700권"은 원문에 없다. [Google 블로그](https://blog.google/innovation-and-ai/products/gemini-notebook/new-access-textbooks/)
- **OpenAI, B.C. 개인정보위원회의 Tumbler Ridge 총격 사건 조사를 취소해 달라고 청구(CBC, 10-29 23:35 UTC):** OpenAI는 조사가 "statutory and constitutional limits"를 넘는다고 주장했다. [CBC](https://www.cbc.ca/news/canada/british-columbia/openai-tumbler-ridge-bc-privacy-commissioner-investigation-9.7401870)
- **Google Jules 종료:** 도구 2번에 Antigravity 플러그인 마켓플레이스와 함께 정리했다.
- **보안 쪽으로 옮긴 것:** Epoch AI의 GLM 5.3 패치 갭 실험은 기술 4번, Accenture 직원의 개인 Claude Code 건(Wired)은 기술 5번이다.
- **직전 판 미확인 사항 후속:**
  - xAI Custom Moderation Controls: docs.x.ai 릴리스 노트에 "October 27"로 적혀 있다. 10-27 게시로 판정했다(창 밖).
  - Gemini 4 Carbon: 모델 카드, Gemini API 가격 페이지, 변경 로그, 앱 릴리스 노트 어디에도 아직 없다. 정가와 일반 공개 시점도 새 정보가 없다.
- **새 진전 없음:** OpenAI API 변경 로그(마지막 10-28)와 deprecations, Claude 모델 은퇴 목록, Mistral Large 4의 독립 벤치마크, DeepSeek·Moonshot의 증류 보도에 대한 공식 반응, Kimi K3.1 Pro 가중치, Qwen·Z.ai·MiniMax·Meta 공식 채널, Artificial Analysis.

### 참고: 창 경계 항목

- **Cohere Series E 1차 마감(10-29 14:00 UTC, 창 시작 2시간 전, 직전 판 미수록):** 원문: "initial close of its Series E funding round targeting up to CAD $7 Billion, led by Schwarz Group". NVIDIA와 캐나다 정부가 참여했다. 캐나다 정부는 최대 $1.6B를 단계적·마일스톤 조건으로 넣는다. BetaKit은 기업가치를 "nearly $36 billion, post-money"로 보도했다(통화 표기 없음). 창 안에는 CBC 재보도(17:12 UTC)와 Cohere의 캐나다 투자 약속 글(10-30 14:00 UTC)이 나왔다. [Cohere](https://cohere.com/blog/who-gets-to-build-the-frontier-for-ai) · [BetaKit](https://betakit.com/canada-to-invest-up-to-1-6-billion-in-ai-firm-cohere/)

## 기술 이슈

미국 목요일 오전부터 금요일 오전까지의 평일 창이었다. 개발자 단말과 개발 플랫폼을 노린 사고가 몰렸다. Cursor 마켓플레이스에 가짜 Claude Code 확장이 올라왔고, PaaS Northflank가 침해됐다. AI 밖에서는 Binance에서 63억 달러가 빠져나갔는데, 시작점이 DevOps 엔지니어의 노트북이었다.

### 1. Cursor 마켓플레이스에 백도어를 심은 가짜 Claude Code 확장 2.1.313: 최대 5,000명, Anthropic 시스템 침해는 아니다 (HN 643점)

- **무슨 일이 있었나:** 10-29 02:27~13:35 PT(09:27~20:35 UTC)에 Cursor의 VS Code 확장 마켓플레이스에서 `Anthropic.claude-code` 2.1.313이 배포됐다. 공식 확장 ID를 그대로 쓴 변조판이다. Cursor 원문: "This malicious version of the extension was not developed by Anthropic. At most 5,000 users were affected".
- **악성 동작:** OpenSourceMalware에 따르면 정상 코드 앞에 66줄짜리 로더가 붙어 있었다. 확장이 로드되기만 해도 실행된다.
  - `~/.ssh/authorized_keys`에 공격자 키를 넣고, 로컬 SSH 서비스를 켜고, 역방향 SSH 터널을 연다.
  - LaunchAgent(macOS)나 예약 작업(Windows)으로 터널을 지속시킨다.
  - Cursor 원문: "Outside of highly unusual circumstances where Cursor was started with administrator privileges, due to errors in the malicious code, this step would fail." 즉 SSH 서비스를 켜는 단계는 대부분 실패했다. 그래도 Cursor는 정리 절차를 반드시 밟으라고 했다.
- **대응:** Cursor는 해당 버전을 내리고 마켓플레이스 전체를 일시 중단했다. 다른 확장은 변조되지 않았음을 확인했고, 키를 교체한 뒤 마켓플레이스를 재구축했다. Anthropic은 버전을 구분하려고 2.1.314를 냈다. npm 게시 시각은 10-30 00:12 UTC다.
- **"Anthropic이 해킹당했다"는 초기 주장:** Kevin Beaumont가 처음 그렇게 썼고, DoublePulsar 글 제목도 "Anthropic hacked"다. Anthropic 보안팀 David Dworken은 HN 댓글(10-30 14:33~15:18 UTC)로 반박했다.
  - 원문: "this did not involve a compromise of any Anthropic systems."
  - 원문: "2.1.313 was never published on OpenVSX, the VS Code marketplace, or on any of our official hosts."
  - npm 레지스트리 기록에도 2.1.313이 없다. 2.1.312(10-27) 다음이 바로 2.1.314다.
  - 공격자가 어떻게 Cursor 마켓플레이스에 Anthropic 이름으로 게시했는지는 Cursor도 Anthropic도 밝히지 않았다.
- **해야 할 일:** 10-29 해당 시간대에 Cursor에서 이 확장을 설치하거나 업데이트했다면 Cursor 공지의 점검 명령과 정리 스크립트를 돌린다. Cursor 원문: "Uninstalling the extension may not be sufficient". 확장을 지우거나 2.1.314로 올려도 이미 심긴 지속성 스크립트는 남는다. 감염된 호스트에서 접근할 수 있던 자격 증명과 세션은 다른 신뢰할 수 있는 기기에서 교체한다.

중요한 이유: 공식 확장 ID와 자동 업데이트만으로 개발자 PC에 지속형 백도어가 들어갔다. Dworken은 공격자가 "used access to Cursor's marketplace"했다고만 썼다. VS Code 포크 편집기들은 자체 마켓플레이스를 운영하니, 원 게시자가 올리지 않은 버전이 어떻게 들어갈 수 있었는지가 다음 쟁점이다. 직전 판 EMBRACE(AI 코딩 도구 내장 브라우저)에 이어 코딩 도구 공급망 문제가 이틀 연속 나왔다.

[Cursor 공지](https://cursor.com/blog/claude-code-extension-security-notice) (공식, 10-29. 시각 표기 없음) · [OpenSourceMalware](https://opensourcemalware.com/blog/trojanized-claude-code-extension-plants-persistent-ssh-backdoor) (HN 첫 게시 10-30 05:20 UTC. 기사 메타는 09:42 UTC로 갱신 시각으로 보인다) · [Phorion](https://phorion.io/blog/offical-claude-code-vsx-compromise/) · [BleepingComputer](https://www.bleepingcomputer.com/news/security/malicious-claude-code-extension-on-cursor-planted-ssh-backdoors/) (10-30 14:36 UTC) · [Anthropic 직원 댓글](https://news.ycombinator.com/item?id=50305139) · [HN](https://news.ycombinator.com/item?id=50302334) (643점) · [HN(Phorion)](https://news.ycombinator.com/item?id=50300683) (308점)

### 2. Northflank 침해: "모든 환경 변수가 복호화됐다고 가정하라". 사용자 AI 키가 실제로 쓰였다 (HN 50점)

- **타임라인(상태 페이지):** 10-30 11:43 UTC에 Wiz 위협 경보가 왔고, 12:02부터 네트워크 정책 패치가 이어졌다. 13:40 UTC에 전 사용자에게 환경 변수 즉시 교체를 공지했고, 13:58에 원인을 특정했다고 올렸다.
- **원문:** "Assume all environment variables have been decrypted. Disable/rotate all keys." 이후 업데이트에는 "Multiple users are confirming that their AI keys have been used."라는 문장이 있다.
- **교체 대상:** 워크로드 비밀, 비밀 그룹, 전역 비밀 모두다. 침해 경로는 아직 공개되지 않았다.

중요한 이유: 10-23 판의 Railway에 이어 PaaS가 또 뚫렸고, 이번에는 AI 제공자 키가 실제로 쓰였다는 사용자 확인까지 나왔다. Northflank에 워크로드가 있다면 키를 교체하고 제공자 콘솔에서 예상 밖 사용량을 확인해야 한다.

[상태 페이지](https://status.northflank.com/incident/cmvv0rn5a092f14rutwccodpp) (공식, 첫 공지 10-30 13:40 UTC) · [HN](https://news.ycombinator.com/item?id=50305025) (50점)

### 3. Hugging Face `trust_remote_code` 채굴·백도어 캠페인이 삭제 며칠 만에 돌아왔다. 에이전트 검색 1위를 노린 가짜 Solana 키트도 나왔다

- **Hugging Face(Pluto Security 조사, BleepingComputer 보도):**
  - 계정 7개에서 악성 저장소 31개를 찾았다. 소형 Qwen2 테스트 체크포인트로 위장해, 자체 추론 서버를 돌리는 GPU 호스트를 노렸다.
  - 가중치나 pickle이 아니라 `trust_remote_code`로 실린 Python 코드가 모델을 불러오는 순간 실행된다. 채굴기를 깔고 역방향 셸을 연다.
  - 2차 저장소는 신고 8일 뒤에도 살아 있었다. 대표 모델의 다운로드는 10-28까지 28,412회였다. Pluto 원문: "The second-wave flagship scanned clean while it collected 28,412 downloads". Hugging Face의 스캔(Protect AI·JFrog)을 통과했다는 뜻이다.
  - 그 모델은 막혔지만, 10-25에 새 계정이 올린 거의 같은 복사본(`htt0118/qwen2-tiny`)이 BleepingComputer 보도 시점에 살아 있었다.
- **가짜 Solana 스타터 키트(Air Security):** GitHub 저장소 `SolanaUpdateTools/solana-starter-kit`가 AI 코딩 에이전트가 Solana 스타터 키트를 검색하면 1위로 나온다고 한다. 에이전트가 README를 따라 실행하면 원격 접근 트로이목마가 설치된다.

중요한 이유: 모델 허브와 에이전트의 검색 결과가 그대로 배포 경로가 된다. `trust_remote_code=True`로 이 모델들을 불러온 적이 있다면 그 기기를 침해된 것으로 보고 자격 증명을 교체해야 한다. 채굴기만 지워서는 백도어가 남는다.

[BleepingComputer](https://www.bleepingcomputer.com/news/security/hugging-face-malware-campaign-returns-to-hijack-gpus-for-crypto-mining/) (매체보도, 10-29 22:09 UTC) · [Pluto Security](https://pluto.security/blog/hugging-face-malware-campaign-load-mine/) (10-29 12:09 UTC, 직전 창) · [Air Security](https://www.air.security/blog-posts/solanacry) (10-29, 날짜만)

### 4. Epoch AI: 오픈 가중치 GLM 5.3이 "패치 갭"으로 최근 Firefox 릴리스 6개를 모두 공략했다

- **무엇을 했나:** 수정 코드는 공개됐지만 아직 사용자에게 배포되지 않은 틈(패치 갭)을 노렸다. GLM 5.3이 다가올 보안 수정을 찾아내 웹페이지 익스플로잇을 만들었다. 방문만 하면 방문자 컴퓨터의 파일을 읽어 공격자 서버로 보낸다.
- **결과(원문):** "any Firefox version released between September 17 and October 26 could be compromised in this way using only information available at the time of release". 준비가 끝난 뒤에는 릴리스당 약 $1,000가 들었고, 익스플로잇은 이틀 안에 만들었다.
- **덤으로 찾은 것:** 모든 공략에서 GLM 5.3이 공개되지 않은 취약점 하나를 함께 찾아 썼다. Mozilla는 10-27 Firefox 159.0에서 이를 고쳤다. Epoch은 159.0에서도 같은 공격이 되는지는 아직 확인하지 않았다고 적었다.
- **Epoch의 결론:** 더 강한 폐쇄형 모델을 방어자에게 먼저 주는 것만으로는 해결되지 않는다. 공개된 수정은 그 자체로 취약점 위치를 알려 주기 때문이다. 수정이 공개된 채 배포되지 않는 기간을 줄여야 한다고 했다.

중요한 이유: 8월에 나온 오픈 가중치 모델로도 패치 갭 공략이 된다. 직전 판들의 "사이버 능력이 큰 모델은 방어자 먼저" 흐름에 대한 반론이기도 하다. 오픈소스 프로젝트의 보안 수정 공개 관행에 압박이 될 수 있다.

[Epoch AI](https://epoch.ai/publications/open-weight-models-patch-gap) (연구기관, 10-29 날짜만. HN 게시 10-29 22:32 UTC) · [HN](https://news.ycombinator.com/item?id=50297159) (13점)

### 5. Wired: Accenture 직원의 개인 Claude Code가 국방부 시스템에서 두 달간 돌았다 (매체보도, 당사자 부인)

- **보도 내용:** 소식통에 따르면 Accenture 소속 계약자가 여러 정보기관이 쓰는 국방부 중앙 시스템을 개인 Claude Code 계정에 연결했다. 이 시스템에는 통제 비분류 정보(CUI)가 들어 있다. 원문: "sent information about the DOD system, including login credentials, to a high-risk, third-party website". 에이전트는 발견될 때까지 두 달간 국방부 네트워크에서 돌았다.
- **반응:** Accenture 원문: "These allegations were thoroughly and independently investigated and found to be without merit." 국방부는 논평을 거부했다.

중요한 이유: 승인되지 않은 개인 에이전트가 고보안망 자격 증명을 외부로 보냈다는 보도다. 사실관계는 다투어지고 있지만, 에이전트 사용 정책과 외부 전송(egress) 통제를 점검할 근거로는 충분하다.

[Wired](https://www.wired.com/story/a-defense-contractor-gave-their-personal-ai-agent-access-to-a-secure-pentagon-system/) (매체보도, 10-29 18:39 UTC) · [HN](https://news.ycombinator.com/item?id=50293979) (20점)

### 6. (AI 밖) Binance에서 63.6억 달러 탈취: 사상 최대이고, DevOps 엔지니어의 노트북에서 시작됐다 (HN 1,664점, 창 안 1위)

- **규모:** TRM 원문(BleepingComputer 인용): "an attacker moved approximately USD 6.36 billion out of Binance wallets on ten blockchains, most of it in about 20 minutes starting at 20:05 UTC". 10-29 20:05 UTC로 창 안이다. 2025년 2월 Bybit(15억 달러)의 약 네 배다.
- **경로:** Binance는 DevOps 엔지니어 노트북 침해를 시작점으로 지목했다. 공격자가 서비스 컨테이너에 접근해 무단 출금 거래를 제출했다. 지갑 개인키가 유출된 증거는 아직 없다고 했다.
- **귀속과 동결:** TRM과 Chainalysis는 북한 TraderTraitor(Lazarus·APT38)로 봤다. 스테이블코인 발행사 등이 약 27억 달러를 동결했지만, 약 24억 달러는 동결할 수 없는 자산으로 남았다.

중요한 이유: AI 사고는 아니지만, 1번과 같은 날 "개발자 단말이 최대 공격면"이라는 점을 보여 줬다. 같은 창에 Microsoft는 북한 위협 행위자들이 AI를 모든 작업 흐름에 넣었다는 보고서를 냈다(아래 8번 짧게).

[BleepingComputer](https://www.bleepingcomputer.com/news/security/binance-links-63-billion-crypto-theft-to-compromised-devops-laptop/) (매체보도, 10-30 09:18 UTC) · [TRM Labs](https://www.trmlabs.com/resources/blog/north-korea-steals-usd-6-36-billion-from-binance-in-the-largest-hack-in-history) · [HN](https://news.ycombinator.com/item?id=50296470) (1,664점) · [HN(Wired)](https://news.ycombinator.com/item?id=50303172) (152점)

### 7. 직전 판 보안 항목 후속 점검

- **PHP2Shell + Fragnesia: 공격받은 방화벽은 Palo Alto PAN-OS였다.** 직전 판이 "벤더 미공개"로 남긴 부분이다.
  - Unit 42가 글을 갱신했다(10-29 18:00 UTC). 원문: "Unit 42 has independently verified the same attacker's tooling and confirmed the reported results."
  - 영향 규모(원문): "approximately .02%" of the global installed base. 해당 고객에게는 개별 연락했다.
  - SecurityWeek(10-30 15:09 UTC): Palo Alto 텔레메트리로 노출된 PHP 인스턴스가 2,260만 개 이상이다. 다만 WordPress에서는 대량 공격이 대체로 실패했다(Defiant).
  - CISA의 Fragnesia 조치 기한은 10-31 그대로다.
- **CISA KEV 추가(10-29):** GitLab CVE-2026-97229와 Cisco ASA/FTD CVE-2026-76418이다. 둘 다 연방기관 조치 기한이 11-01이다.
- **EMBRACE:** OpenAI는 The Register에 "aware of the issue and working to address it"라고 답했다. Codex는 아직 수정되지 않았다. 창 안에 나온 Codex CLI 0.166.0 릴리스 노트에도 관련 수정이 없다. Microsoft와 Anthropic은 응답하지 않았다.
- **새 진전 없음:** LiteLLM Enterprise 권고는 여전히 CVE가 없다. Claude Code 백그라운드 요약 호출 건은 2.1.314 변경 로그에 언급이 없고, 관련 이슈는 열린 채다. DSH 플러그인 악성 세부와 Eclypsium 후속도 나오지 않았다.

[Unit 42](https://unit42.paloaltonetworks.com/cve-2026-109618-cve-2026-109619/) · [Dream](https://dreamgroup.com/blog/chained-php2shell-exploit-roots-25-servers) · [SecurityWeek](https://www.securityweek.com/php2shell-attacks-root-palo-alto-firewalls-but-fail-against-wordpress/) · [CISA KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) · [The Register(EMBRACE)](https://www.theregister.com/security/2026/10/29/researchers-find-bugs-in-5-ai-developer-tools-with-openai-and-microsoft-flaws-still-unpatched/5306508) (10-29 23:12 UTC)

### 8. 보안 권고·보고서 짧게

- **Langflow OSS CVE 33건(IBM, 전역 GHSA 미검토로 10-29 18:30 UTC 게시):** critical 6, high 11, medium 15, low 1건이다. IBM 공지에는 "unauthenticated remote code execution exploitable through model context protocol tool enumeration"이 들어 있다. 1.0.0~1.12.8을 쓰면 1.13.0(10-22 릴리스)으로 올리라고 한다. 저장소 GHSA에는 아직 없다. [IBM 공지](https://www.ibm.com/support/pages/node/7294467)
- **WordPress AI Engine 플러그인 CVE-2026-110732(high, 10-30 09:31 UTC):** MCP용 OAuth 동적 클라이언트 등록 엔드포인트가 인증 없이 열려 있었다. 관리자가 동의 화면을 누르면 공격자가 관리자 권한 MCP 토큰을 얻는다. 영향 범위는 3.8.6 이하다. [GHSA](https://github.com/advisories/GHSA-22w7-p44p-2gq7)
- **Linux MPTCP 결함 CVE-2026-98432 후속(Go 보안팀, HN 10-30 00:52 UTC):** 10-29 판에 실은 Anthropic의 무클릭 원격 root 결함이다. Go 원문: "As of Go 1.24, Go programs enable MPTCP by default for listen sockets on Linux." 커널 패치 전 임시 차단책은 `GODEBUG=multipathtcp=0`이다. Go 쪽 기본값 변경은 "premature"라고 했다. [golang-announce](https://groups.google.com/g/golang-announce/c/PCoQcdxoScw) · [HN](https://news.ycombinator.com/item?id=50298526)
- **Microsoft: 북한 위협 행위자의 AI 도입(10-29 16:00 UTC, 창 시작 정각):** 여러 그룹이 작업 흐름 전반에 AI를 넣었고, 오픈소스 에이전트형 침투 테스트 프레임워크 ARTEX를 쓴다고 했다. 원문: "AI-enabled operations remain human-directed and have not become fully autonomous". [Microsoft Security](https://www.microsoft.com/en-us/security/blog/2026/10/29/tracking-ai-adoption-in-north-korean-threat-actor-tradecraft/)
- **전역 GHSA(창 안 게시):** reviewed 30건, unreviewed 608건, malware 19건이다. 주요 AI 저장소 약 60곳의 저장소 권고에는 창 안 게시가 없었다.

### 9. 연구 짧게

- **YC Labs "Agents in Games"(HN 366점, 10-29 21:40 UTC):** GPT-6.1 Sol 에이전트 50개를 생존 Minecraft 세계에 3시간 풀어 놓았다. 하네스를 MIT 라이선스로 공개했다. [글](https://labs.ycombinator.com/projects/games/index.html) · [HN](https://news.ycombinator.com/item?id=50296486)
- **Broken on Arrival:** Ollama 공식 GGUF 태그 중 7개(2.3%)가 HumanEval+ 164문제 중 6문제 이하만 풀었다. 모두 4B 이하 저비트 양자화였다. [논문](https://huggingface.co/papers/2609.05881)
- **MICA:** 같은 기능을 MCP로 주느냐 CLI로 주느냐에 따라 에이전트 성능과 비용이 크게 달라졌다. [논문](https://huggingface.co/papers/2610.34298)
- **EasyProver(AWS):** PutnamBench 672문제 중 669문제와 IMO 2026 형식화 6문제를 모두 증명했다고 한다(저자 측정). [논문](https://huggingface.co/papers/2610.38174)
- **"Your Benchmark Is an Empirical Measure Over Difficulty"(UIUC):** 7개 영역의 인기 벤치마크 25개를 분석해 난이도 분포가 점수를 어떻게 왜곡하는지 진단했다. [논문](https://huggingface.co/papers/2610.29927)

### 10. 커뮤니티·AI 밖 짧게

- **"Anthropic이 게임 리메이크를 막는다"(HN 69점, 미확인):** 한 트윗이 10-27께 서버 플래그로 "Don't build a recreation of a specific existing game, even if it is renamed or called original."이라는 규칙이 Claude Code에 주입됐다고 주장했다. 공식 확인은 없다. [HN](https://news.ycombinator.com/item?id=50304358)
- **Polymarket(10-29 21:47 UTC):** 서드파티 RUM 스크립트가 변조돼 사용자 자산이 빠져나갔고, 거래를 멈췄다. 약 250만 달러가 추적됐다. [BleepingComputer](https://www.bleepingcomputer.com/news/security/polymarket-halts-trading-after-crypto-draining-script-breach-25m-traced/)
- **Chrome 확장 319개(설치 1,340만):** 검색을 가로챘고, 일부는 AI 검색 도구로 위장했다. [BleepingComputer](https://www.bleepingcomputer.com/news/security/chrome-extensions-with-134-million-installs-caught-hijacking-searches/) (10-29 16:10 UTC)

## 써볼 만한 도구

창 안에서 Claude Code는 2.1.314를, Codex CLI는 0.166.0을 냈다. Gemini CLI는 nightly 빌드만 있었다. Cursor 마켓플레이스의 가짜 Claude Code 확장은 기술 1번에, Claude API의 128MB 상한과 사용량 분석 API는 모델 7번에, Pro 사용량 2배는 모델 3번에 정리했다. VS Code, Cursor, Ollama, vLLM, SGLang, 주요 SDK는 창 안에 새 릴리스가 없었다.

### 1. Claude Code 2.1.314: 작업 폴더 밖 쓰기를 막는 설정이 생겼고, 훅·샌드박스·관리 정책 우회를 고쳤다

- **한 줄 설명:** 기술 1번 사건 직후 나온 정식 릴리스다. 2.1.313은 공식 채널 어디에도 없고, 2.1.312 다음이 바로 2.1.314다.
- **새 설정:** `permissions.blockWritesOutsideWorkingDirectories`. 원문: "Write, Edit and NotebookEdit refuse paths outside the working directories in every permission mode".
- **보안 관련 수정(공개된 권고는 없다):**
  - "Fixed hooks matching Bash not running on shell commands started by the Monitor tool"
  - "Fixed sandbox protections not applying to a worktree entered by `claude -p --resume` or `--continue`"
  - 원격 관리 설정의 `forceRemoteSettingsRefresh`가 저장본이 만료되면 적용되지 않아, 서버에 닿지 못할 때 "let sessions start without policy" 하던 문제
  - `--setting-sources`로 제외한 설정 파일이 auto mode 규칙과 저장된 권한 동의를 여전히 공급하던 문제
- **idle compaction 확대:** VS Code와 Claude Desktop의 Code 탭에도 들어왔다. 원문: "a 200K+ token conversation left idle is summarized, and the summary is used on your next message if the prompt cache has expired". 텔레메트리를 끈 구독자와 Bedrock·Vertex·Foundry·LLM 게이트웨이에도 적용된다.
- **동작이 바뀐 것:** `FORCE_HYPERLINK=false`가 이제 링크를 끈다(전에는 켰다). `claude -p`가 시작에 실패하면 오류를 출력하고 종료 코드 1로 끝난다(전에는 출력 없이 0).
- **추천 이유:** 훅·샌드박스·관리 정책을 우회할 수 있던 구멍이 함께 막혔다. 조직 정책을 쓰는 팀은 올릴 이유가 분명하다. 다만 idle compaction은 사용자가 요청하지 않아도 모델을 호출한다. 직전 판의 백그라운드 요약 비용 논란과 같은 맥락이니, 원치 않으면 설정에서 끈다. 그 논란에서 예고된 저비용 모드는 이번 노트에 없다.
- **함정:** `claude -p` 종료 코드를 보는 CI 스크립트는 실패가 이제 1로 잡힌다.
- **설치:** `claude update` 또는 `npm i -g @anthropic-ai/claude-code@2.1.314`

[릴리스](https://github.com/anthropics/claude-code/releases/tag/v2.1.314) (공식, 10-30 01:06 UTC. npm 00:12 UTC) · [CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)

### 2. Google: Antigravity 플러그인 마켓플레이스를 열었고, Jules는 12-31에 종료한다

- **마켓플레이스(10-29 17:00 UTC):** 원문: "a centralized directory for discovering and installing third-party and Google-built plugins across the Antigravity App and the Antigravity CLI".
  - 플러그인은 `plugin.json` 매니페스트 하나로 Skills, MCP 서버, Rules, Hooks, Custom Agents 다섯 가지를 묶어 함께 설치·업데이트·토글한다.
  - Hooks 이벤트는 `SessionStart`, `PreToolUse`, `PostToolUse`, `PreInvocation`, `PostInvocation`, `Stop`이다.
  - 출시 파트너는 Atlassian, Canva, CrowdStrike, Datadog, Figma, Stripe 등 12곳이다.
  - 설치: 앱의 Customizations → Plugins 탭, 또는 `agy plugin install <plugin-name>`.
- **Jules 종료(10-29):** 원문: "Jules will wind down on December 31, 2026. You have until November 11, 2026 to continue finishing up sessions." 11-11 이후에는 연말까지 읽기 전용이다.
  - 계정 설정, 환경 변수, 예약 작업, 연결된 저장소, 메모리, 설정 스크립트, 모든 세션 기록(프롬프트·계획·활동 로그·패치)을 로컬 폴더로 내보낼 수 있다.
  - 방법: `npm install -g @google/jules` → `jules login` → Claude Code 같은 CLI 에이전트에 공식 내보내기 프롬프트를 붙여 넣는다. 그 프롬프트 스스로 비공개 엔드포인트를 쓴다고 밝힌다("Both are undocumented").
- **추천 이유:** Jules 사용자는 11-11 전에 내보내야 한다. Antigravity 플러그인은 Claude Code·Codex 플러그인과 구성 요소 종류(스킬·MCP·훅·서브에이전트)가 같다. 매니페스트가 서로 호환되는지는 원문에 없다.

[마켓플레이스 발표](https://antigravity.google/blog/plugin-marketplace-in-antigravity) (공식) · [Jules 종료 공지](https://jules.google/docs/changelog/2026-10-29/) (공식, 날짜만. @GoogleLabs 게시 19:11 UTC)

### 3. Codex CLI 0.166.0: 네트워크 진단 명령과 큐 재개가 생겼고, GPT-6.1 Terra가 Vertex AI·Bedrock 카탈로그에 들어왔다

- **새 기능:**
  - `codex doctor --network-only`: 원문 "focused configuration, connectivity, and WebSocket diagnostics".
  - `codex queue --resume`: 메시지를 큐에 넣은 뒤 메모리에서 내려간 스레드를 다시 이어 간다.
  - "Added GPT-6.1 Terra to the bundled, Vertex AI, and Amazon Bedrock model catalogs."
  - TUI가 안전 차단 이유를 더 분명히 설명하고, 더 안전한 후속 프롬프트를 고를 수 있게 했다.
- **수정:** 재개·포크 후에도 승인·권한 설정이 유지된다. "Preserved reasoning-token accounting across resume and fork to avoid premature compaction."
- **추천 이유:** 사내 프록시 환경에서 연결 문제를 따로 진단할 수 있고, 재개한 스레드가 너무 일찍 압축되던 문제가 풀렸다. 같은 날 Pro·Business Premium의 Codex 사용량도 2배가 됐다(모델 3번).
- **함정:** 직전 판 EMBRACE(내장 브라우저 취약점)에 대한 수정은 이번 노트에 없다(기술 7번).
- **설치:** `npm install -g @openai/codex@0.166.0`

[릴리스](https://github.com/openai/codex/releases/tag/rust-v0.166.0) (공식, 10-29 19:47 UTC)

### 4. opencode v2.2.3: Claude Code 로그인으로 Claude를 쓰는 "Claude Agent" 공급자. 약관과 관리 정책 변경을 먼저 확인할 것

- **한 줄 설명:** 원문: "Install Claude Code, run `claude auth login`, then select **Claude Agent** in `/connect` to use your Claude Code login and models."
- **함정 1, 약관:** Anthropic Agent SDK 문서에는 "Unless previously approved, Anthropic does not allow third party developers to offer claude.ai login or rate limits for their products, including agents built on the Claude Agent SDK."라고 적혀 있다. opencode 쪽 직원은 "claude subscripitions are back in opencode"라고 게시했지만(10-30 15:46 UTC), Anthropic이 승인했는지는 확인하지 못했다(미확인). 구독 계정으로 쓰기 전에 약관을 확인한다.
- **함정 2, 관리 정책:** `integration.use` 규칙이 이제 통합 계정에만 적용된다. MCP 서버·스킬·플러그인은 새 `mcp.use`, `skill.use`, `plugin.use` 규칙으로 따로 막아야 한다. `integration.use` 한 줄로 MCP를 막아 왔다면 업그레이드 뒤 그 규칙이 MCP에 적용되지 않는다.
- **함정 3, 깨지는 변경:** TUI 플러그인은 OpenTUI 0.8 이상이 필요하다.

[릴리스](https://github.com/anomalyco/opencode/releases/tag/v2.2.3) (공식, 10-30 09:55 UTC) · [PR #58782](https://github.com/anomalyco/opencode/pull/58782) · [Agent SDK 문서](https://code.claude.com/docs/en/agent-sdk/overview)

### 5. Herdr: 코딩 에이전트용 터미널 작업 공간 데스크톱 앱 (HN 308점)

- **한 줄 설명:** 원문: "Herdr is a desktop app for terminals and coding agents. Keep work in spaces, see which agents need you, and leave terminals running when you close the window."
- **어떻게 동작하나:**
  - 여러 작업 공간의 에이전트를 사이드바 하나에 모아 Working, Blocked, Done, Idle 상태로 보여 준다. 다른 에이전트가 띄운 작업자도 함께 보인다.
  - 창을 닫아도 터미널은 계속 돈다.
  - SSH로 다른 머신을 추가하면 그 머신의 작업 공간 터미널은 원격에서 돈다.
- **추천 이유:** Claude Code, Codex 같은 CLI 에이전트를 여러 개 병렬로 돌릴 때 "어느 에이전트가 나를 기다리는가"를 한눈에 본다.
- **주의:** Apache-2.0이고 저장소는 10-16에 생겼다. 신규 출시라기보다 HN에서 화제가 된 것이다. Linux는 Vulkan 드라이버가 필요하고, macOS 빌드는 Apple Silicon용만 있다. 설치 스크립트를 파이프로 바로 실행하는 방식이니 스크립트를 먼저 읽어 본다.
- **설치:** `curl -fsSL https://releases.herdr.dev/app/install.sh | sh`

[GitHub](https://github.com/herdrdev/herdr-app) · [HN](https://news.ycombinator.com/item?id=50298436) (308점, 10-30 00:42 UTC)

### 6. basic-memory 1.0: 로컬 MCP 메모리 서버의 대규모 개편. 호환이 깨진다

- **한 줄 설명:** 원문: "The MCP server exposes only the `bm_*` tools, the CLI is built on those same tools, and the pre-1.0 tools, commands and compatibility code are removed rather than aliased."
- **깨지는 것:**
  - `write_note`, `read_note` 같은 이전 도구가 별칭 없이 사라졌다. "Every project-scoped tool requires `project`."
  - `bm cloud sync`, `bisync`가 없어졌다. 원문: "They could delete cloud-only or local files." 대신 대상에서 지우지 않는 `bm cloud pull`·`push`를 쓴다.
  - LiteLLM이 선택 설치가 됐다. LiteLLM 임베딩을 쓰면 `basic-memory[litellm]`로 설치한다.
- **추천 이유:** 동기화가 파일을 지우던 위험이 사라졌다. 다만 원문 지시대로 "Update MCP allowlists, agent instructions, hooks and recorded fixtures, then restart clients" 해야 한다. 자동 업데이트로 올라가면 기존 에이전트 지침이 부르던 도구가 없어진다.

[릴리스](https://github.com/basicmachines-co/basic-memory/releases/tag/v1.0.0) (공식, 10-29 23:45 UTC) · [CHANGELOG](https://github.com/basicmachines-co/basic-memory/blob/main/CHANGELOG.md)

### 7. (후속) Mistral Large 4 로컬 추론: llama.cpp는 b11998부터 지원한다

- **llama.cpp:** 직전 판은 "PR은 머지됐지만 빌드가 없다"고 적었다. b11998(10-30 01:11 UTC)이 그 커밋("model: mistral le chonk")을 담은 첫 빌드다.
- **vLLM:** ROCm 이미지 태그와 MI355X 실행법이 레시피에 추가됐다(PR #64544, 10-29 23:44 UTC). 정식 릴리스와 nightly에는 아직 들어가지 않았다.

[llama.cpp b11998](https://github.com/ggml-org/llama.cpp/releases/tag/b11998) · [vLLM PR #64544](https://github.com/vllm-project/vllm/pull/64544)

### 짧게

- **Crush v0.99.0(10-29 16:25 UTC):** Catwalk v3로 바뀌었고, 훅이 실행 전체를 볼 수 있다. `--can-reason`은 `--thinking always|never|toggleable`로 바뀌었다. `CRUSH_PERMISSIONS_PIPED=1`이면 Crush를 품은 편집기가 권한 프롬프트에 대신 답한다. [릴리스](https://github.com/charmbracelet/crush/releases/tag/v0.99.0)
- **Cloudflare Agents SDK agents@0.32.0(10-29 18:53 UTC):** `SKILL.md`를 Agent Skills 스펙에 맞춰 검사한다. 깨지는 변경으로 `SkillRegistry`가 `loadSkillCatalog()`·`createSkillTools()`로 바뀌었다. MCP client·server 패키지는 2.4.0으로 올랐다. [릴리스](https://github.com/cloudflare/agents/releases/tag/agents%400.32.0)
- **Mistral Vibe v3.0.4(10-30 11:03 UTC):** `agent_max_output_tokens`와 `compaction_max_output_tokens`로 출력 상한을 건다. `vibe -p --system-prompt-file`로 시스템 프롬프트를 바꾼다. 다른 에이전트 플러그인에서 가져온 훅은 조용히 무시하지 않고 "not supported yet"으로 알린다. [릴리스](https://github.com/mistralai/mistral-vibe/releases/tag/v3.0.4)
- **Basecamp 공식 원격 MCP 서버(MCP 레지스트리 등록 10-29 23:17 UTC):** 주소는 `https://mcp.basecamp.com/mcp`다. [Basecamp AI](https://basecamp.com/ai)
- **Langflow 1.13.0:** 1.12.8 이하에 걸린 CVE 33건(기술 8번)의 수정판이다. 1.13.0 자체는 10-22에 나왔다.

## 주목할 점

- **에이전트와 개발자 단말이 고르는 "출처"가 가장 큰 공격면이 됐다.** 공식 확장 ID를 단 가짜 확장(Cursor), 스캔을 통과한 모델 허브 코드(Hugging Face), 에이전트 검색 1위에 오른 가짜 스타터 키트, 개발자 노트북에서 시작된 Binance 탈취가 같은 창에 나왔다. 다음에 볼 것은 공격자가 Cursor 마켓플레이스에 Anthropic 이름으로 게시한 경로가 공개되는지, VS Code 포크 마켓플레이스들이 게시자 검증을 바꾸는지다.
- **"방어자 먼저" 공개만으로는 부족하다는 반론이 나왔다.** Epoch AI는 8월에 나온 오픈 가중치 모델로 Firefox 패치 갭을 공략했고, 문제는 모델 접근보다 수정 공개와 배포 사이의 시간이라고 했다. 같은 날 OpenAI는 사내 감시 체계의 빈틈(사내 Codex 호출 꺼짐, Chat Completions 미감시)을 스스로 공개했다.

## 조사 제약

- **본문을 직접 읽지 못한 원문:** openai.com(403)과 help.openai.com은 r.jina.ai로 읽었다. Bloomberg(SpaceX)는 공개된 앞부분만, TRM Labs 원문은 열리지 않아 BleepingComputer 인용으로 대신했다. DoublePulsar는 직접 403이라 Medium RSS로 확인했다. Dark Reading의 "Anthropic 사칭 피싱" 기사(10-29 21:21 UTC)는 본문을 못 읽어 싣지 않았다. VentureBeat RSS와 Reuters는 막혔다. Alibaba 뉴스룸 RSS(alizila)가 이번에 403으로 바뀌어 r.jina.ai로 확인했다.
- **시각이 날짜만 있는 원문:** Cursor 공지(10-29), GPT-6 Luna 시스템 카드 부록과 ChatGPT 릴리스 노트(10-29), Gemini API 변경 로그(10-30), Claude 플랫폼 릴리스 노트(10-28·29), Jules 종료 공지, Epoch AI(10-29), Air Security다. HN 첫 게시, 트윗 시각, npm 게시 시각으로 보강했다. OpenAI 오정렬 감시 글은 RSS가 09:00 GMT 정각 값이라 HN 첫 게시(17:13 UTC)로 창 안을 추정했다.
- **확인하지 못한 것:**
  - 보안: 공격자가 Cursor 마켓플레이스에 Anthropic 이름으로 게시한 경로, 실제 SSH 접속까지 이어진 피해 수, Northflank 침해 경로, LiteLLM Enterprise 권고의 CVE 번호, DSH 플러그인 악성 세부. Binance 침해와 Cursor 확장 사건의 연관을 주장하는 글이 있었지만 근거를 찾지 못해 싣지 않았다.
  - 모델: Gemini 4 Carbon의 모델 카드·정가·일반 공개 시점, Fairwind 참여 규모, Cohere 기업가치의 통화, Bloomberg 기사의 월 $3B 근거.
  - 도구: Anthropic이 opencode의 Claude 구독 로그인 사용을 승인했는지, Claude Code 백그라운드 요약 저비용 모드의 출시 시점.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 스토리를 두 구간으로 나눠 점수순 상위와 키워드로 훑었다.
  - 전역 GHSA: 창 안 reviewed 30건, unreviewed 608건, malware 19건을 AI·에이전트·MCP 키워드로 걸렀고 재현하지 않았다. 주요 AI 저장소 약 60곳의 저장소 권고에는 창 안 게시가 없었다.
  - MCP 레지스트리: 창 안 게시 버전이 3,420개라 주요 기업 네임스페이스만 봤다.
  - Hugging Face: 주요 조직만 봤다. Vertex AI 릴리스 노트는 캐시가 묵어 보지 않았다.
- **자사 측정:** GPT-6 Luna(10-29)의 사실성·안전 수치, OpenAI 감시 체계의 재현 결과, Amazon Bedrock 성장률, Mage-VL-Cowork의 OSWorld 66.0%, Epoch AI의 Firefox 실험, 논문 수치는 모두 자사·저자 측정이며 독립 검증 전이다.
