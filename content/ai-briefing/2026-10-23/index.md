---
title: "2026-10-23 AI 브리핑"
date: 2026-10-23T01:00:00+09:00
tags: [ai-briefing, security, anthropic, openai]
description: "PHP2Shell은 공개 4일 전부터 악용 시도가 있었고 지금은 광범위하게 악용되고 있으며, LiteLLM과 Railway 침해로 AI 제공자 키를 노린 공격이 이어졌고, Anthropic 새 API 버전과 OpenAI 이용 정책 개정이 모델을 쓰는 조건을 바꿨다."
---

> 조사 범위: 2026-10-22 01:00 ~ 2026-10-23 01:00 KST(2026-10-21 16:00 ~ 10-22 16:00 UTC, 미국 수요일 09:00 PDT ~ 목요일 09:00 PDT). 직전 브리핑의 조사 창 종료 시각부터 잡았다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`·`created_at`, 저장소·전역 GHSA `published_at`, Hugging Face `createdAt`·`lastModified`, HN Algolia `created_at`, 트윗 스노우플레이크 ID로 검증했다. HN 점수는 조회 시점(10-22 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **PHP2Shell은 공개 4일 전(10-17)부터 악용 시도가 있었고, 지금은 "광범위하게 악용 중"이다.** Amazon이 허니팟 데이터로 확인했고, 패치 시한을 시간 단위로 잡고 침해 조사는 10-17부터 하라고 권했다. 다만 공개 PoC 상당수는 바닐라 PHP에서 작동하지 않는다는 반론도 HN 1위에 올랐다.
- **AI 제공자 키가 모이는 곳이 연달아 뚫렸다.** LiteLLM 오염은 인터넷에 노출된 내부 LiteLLM의 로그에서 GitHub CLI 토큰이 털린 데서 시작됐다. 같은 날 Railway는 고객 환경변수가 복호화된 채 유출됐을 수 있다며 "AI 추론 제공자를 노린 일련의 공격"이라고 밝혔다. 두 곳에 키를 둔 적이 있으면 교체해야 한다.
- **모델을 쓰는 조건이 바뀌었다.** Anthropic 새 API 버전 `2026-10-21`에서는 `temperature`·`top_p`·`top_k`를 보내면 400이 난다. OpenAI는 11-20부터 증류 금지를 넓히고, 라우터·게이트웨이에 사전 승인을 요구한다. Anthropic은 상시 에이전트 앱 Claude Duet을 연구 프리뷰로 냈다.

## 모델 소식

평일 창이었지만 OpenAI·Google·Anthropic·xAI의 새 프런티어 모델은 없었다. 대신 두 랩이 모델을 쓰는 조건을 바꿨다. Anthropic은 새 API 버전의 마이그레이션 가이드를 내면서 서버 쪽 호환성 깨짐을 처음 밝혔고, OpenAI는 증류 금지를 넓히고 라우터·게이트웨이에 사전 승인을 요구했다. 새 원본 모델은 Recursive Flint와 Hugging Face Carbon-X 둘이다. OpenRouter 신규 등록은 Flint 하나였고, Hugging Face 주요 조직 약 65곳에는 창 안에 새로 만든 원본 모델이 없었다.

### 1. Anthropic API `2026-10-21` 마이그레이션 가이드 게시: `temperature`·`top_p`·`top_k`를 보내면 400 (후속)

- **직전 판과의 차이:** 직전 판 조회 때(10-21 16시대 UTC) 404였던 가이드가 올라왔다. SDK diff로만 보이던 변화 외에, 서버가 새 버전 요청을 거부하는 조건이 문서로 확정됐다.
- **새 버전에서 400이 되는 요청:**
  - `temperature`·`top_p`·`top_k` 중 하나라도 보내면, 기본값이어도 400이다. Claude 4.7 이상처럼 원래 기본값만 받는 모델은 결과가 같다. Sonnet 4.6처럼 다른 값을 받던 모델은 temperature를 낮춰 얻던 일관성이 사라지고, 가이드는 "대체할 설정이 없다"고 적었다.
  - assistant 메시지에 document 블록을 직접 넣으면 400이다. 문서는 user 메시지로 보내야 한다.
  - user 메시지의 text 블록(`search_result` 안 포함)에 비어 있지 않은 `citations`가 있으면 400이다.
- **이름이 바뀌는 응답 필드:** Admin API와 Enterprise Analytics API 응답의 옛 필드명이 빠진다. 예를 들어 `group_id`→`rbac_group_id`, 역할의 `name`→`display_name`, `actor.email`→`actor.email_address`다. API 키의 `workspace_id`는 값이 다른 `scope`로 바뀐다.
- **Bedrock:** 새 버전은 Claude 4.7 이상에서만 된다. Opus 4.6 이하를 Bedrock으로 부르는 코드는 지금 SDK에 머물거나 `2023-06-01`을 계속 보내야 한다.
- **릴리스 노트:** 10-21 항목에 SDK 8종(Python 2.0.0, TS 1.0.0, C# 13.0.0, Go 2.0.0, Java 3.0.0, PHP 1.0.0, Ruby 2.0.0, ant CLI 2.0.0)이 올라왔다. user profiles에는 `enforcement_status`·`enforcement_actions` 필드와 필터가 붙었다(Python SDK v2.1.0).

중요한 이유: 직전 판이 경고한 "SDK만 올리면 바뀌는 것"이 서버 수준의 400 오류로 확정됐다. 샘플링 파라미터를 하드코딩한 코드가 가장 먼저 깨진다. 낮은 temperature에 기대는 요청은 SDK를 올리지 말고 `2023-06-01`에 남겨 둬야 한다.

[마이그레이션 가이드](https://platform.claude.com/docs/en/api/migration-2026-10-21) (게시 시각 미상, 직전 판 조회 이후 게시, 공식) · [릴리스 노트](https://platform.claude.com/docs/en/release-notes/overview) · [anthropic-sdk-python v2.1.0](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v2.1.0) (10-21 22:00 UTC)

### 2. OpenAI 이용 정책 개정(11-20 시행): 증류 금지 확대, 라우터·게이트웨이는 사전 승인

- **증류 금지:** OpenAI를 대체할 모델에 쓰일 학습 재료나 신호를 OpenAI 모델·출력으로 만들면 안 된다. 학습 데이터·과제·루브릭·RL 환경 만들기, 라벨·점수·순위·선호·보상 제공, 데이터·궤적 선별이 모두 들어간다. 남을 대신해 하거나 직접 학습하지 않아도 적용된다. "보호된 추론"을 빼내려는 시도도 명시적으로 금지했다.
- **중개 사업자 승인:** 제3자가 OpenAI 모델에 직접 접근하게 하는 서비스는 사전 검토와 승인이 필요하다. 블로그는 모델 라우터와 AI 게이트웨이를 예로 들었다. 앱 기능에 OpenAI 모델을 넣는 것만으로는 대상이 아니다.
- **협력사 기준 통일:** 직접이든, 호스팅 파트너든, 재판매 파트너든 같은 안전장치를 적용하고 제재를 공조한다. 고객이 최종 사용자를 구분해 넘기는 SafetyID도 도입한다. OpenAI는 사용자 신원을 모른 채 해당 사용자만 제재할 수 있다고 했다.
- **허용 범위:** 연구, 소프트웨어 개발, 평가·벤치마크, 제품 개발은 이 경계 안에서 계속 허용한다.

중요한 이유: OpenRouter 같은 게이트웨이와, OpenAI 출력으로 경쟁 모델을 학습시키는 관행이 정면으로 걸린다. 직전 판의 Anthropic user profiles(최종 고객 단위 제재)와 같은 흐름이고, OpenAI 모델이 Google Cloud로 들어간 직후 나온 공급 조건 정비다.

[OpenAI 블로그](https://openai.com/index/broad-access-through-consistent-standards) (RSS 날짜 10-21, 공식) · [이용 정책](https://openai.com/policies/usage-policies/) (변경 이력 2026-10-21, 시행 11-20)

### 3. Anthropic Claude Duet: Opus 5.5 기반 상시 에이전트 앱, 연구 프리뷰 (HN 504점)

- **제품:** iOS·Android 단독 앱과 claude.ai에서 쓴다. 미국 Pro·Max 구독자 대기자 명단부터 연다. 아침마다 연결된 앱의 주요 항목을 브리핑하고, 몇 시간마다 앱을 확인해 사람이 봐야 할 때만 알린다. "계속 확인하면 플랜 한도가 훨씬 빨리 닳는다"는 이유다.
- **동작:** 자체 VM과 브라우저가 있어 노트북·휴대폰이 꺼져 있어도 일한다. Claude Marketplace의 커넥터·플러그인 6,000개 이상을 쓴다.
- **안전장치:** 메시지 전송·결제·서명·삭제 전에는 승인을 받는다. 증권 거래나 계좌 간 이체는 승인이 있어도 못 한다. 자격증명은 Managed Agents 설계를 따라 비밀번호·토큰을 직접 보지 않고, 행동은 auto mode 분류기가 승인한다. 결제는 Stripe Link 가상카드와 Shopify Shop Pay로 하고, 1Password 로그인 연동은 "곧"이다.
- **같은 날 Claude Security:** 지속 스캔과 수정 추적을 넣은 공개 베타가 됐다.

중요한 이유: Meta Muse·OpenAI·xAI가 먼저 뛰어든 "상시 개인 에이전트" 경쟁에 Anthropic이 들어왔다. 결제와 외부 계정 접근을 다루는 만큼, 승인 경계와 프롬프트 인젝션 방어가 실제로 어떻게 동작하는지가 관건이다.

[Anthropic](https://claude.com/resources/articles/claude-duet) (날짜 10-21, @claudeai 트윗 19:09 UTC, 공식) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1015843/anthropic-claude-duet-ai-agent) (19:53 UTC) · [TechCrunch](https://techcrunch.com/2026/10/22/claude-duet-app-is-anthropics-bet-on-agents/) (10-22 13:53 UTC) · [Claude Security](https://claude.com/resources/articles/claude-security-now-scans-continuously-and-tracks-fixes) (날짜 10-21) · [HN 50186037](https://news.ycombinator.com/item?id=50186037)

### 4. Recursive Flint: "자동 연구"로 만든 40B-A2.5B 로컬 코딩 MoE, Apache-2.0

- **구조:** 총 39.8B, 토큰당 활성 약 2.5B(출력 헤드 포함)다. 전문가 256개 중 6개와 공유 전문가 1개를 쓰고, 문서상 컨텍스트는 262K 토큰이다.
- **수치(자사 측정):** Terminal-Bench 2.1 54.9%, SWE-bench Verified 74.2%, SWE-bench Pro v2 89.7%다. Terminal-Bench는 웹 검색으로 얻은 풀이를 실패로 셌다고 밝혔다. M4 Pro MacBook에서 초당 약 131토큰으로, 비슷한 크기의 로컬 모델보다 약 2배 빠르다고 했다.
- **개발 방식:** 자사 "Eureka Machine"이 아키텍처, 출력 헤드까지 4비트로 줄이는 양자화, 커널을 탐색했고, 소규모 팀이 4개월 만에 만들었다고 한다.
- **쓰려면:** 아직 Transformers 모델링 코드가 없어 저장소의 SGLang 키트로 돌린다(B300·H200 한 장). OpenRouter에 무료로 올라왔다.

중요한 이유: "AI가 AI를 만든다"를 내건 Recursive(5월 6.5억 달러 투자)의 첫 공개 모델이다. 2.5B 활성으로 SWE-bench Verified 74%라는 주장이 독립 평가에서도 유지되는지 볼 만하다.

[Recursive 블로그](https://recursive.com/updates/introducing-recursive-flint) (Socher 트윗 10-21 16:29 UTC, 공식) · [Hugging Face](https://huggingface.co/RecursiveSI/flint) (저장소 생성 14:20 UTC, 좋아요 454) · [OpenRouter](https://openrouter.ai/recursive/flint-v1.0:free) (22:46 UTC) · [TechCrunch](https://techcrunch.com/2026/10/21/recursive-launches-flint-a-laptop-coding-model-it-says-was-largely-built-by-ai/) (20:27 UTC)

### 5. Hugging Face Carbon-X: DNA와 대화하는 14.8B 오픈 모델, Apache-2.0

- **구조:** Qwen3-14B에서 출발했다. 별도 DNA 인코더 없이, DNA를 겹치지 않는 6-mer 토큰으로 바꿔 텍스트와 같은 디코더에 넣는다. 프롬프트에 붙여 넣은 염기서열은 자동으로 태그된다.
- **수치(자사 측정):** DNA 벤치마크 5종 평균 66.7%로 기반 모델보다 27포인트 높다. GPT-6.1 Sol(67.5%)에 근접하면서 문제당 추론 비용은 약 1/1,800이라고 했다. ClinVar의 해석 불명 변이 240만 건을 분석하는 비용을 약 300달러로 추정했다(평가한 프런티어 모델은 3.2만~400만 달러).
- **함께 만든 곳:** 중관촌학원(Zhongguancun Academy), Telethon 유전의학연구소, 나폴리 페데리코 2세 대학이다.

중요한 이유: bf16으로 80GB GPU 한 장에 들어가, 미공개 서열을 외부 API에 보내지 않고 다룰 수 있다. 범용 프런티어 모델 대비 비용 차이가 세 자릿수라는 주장이 맞다면 대규모 변이 해석의 비용 구조가 바뀐다.

[Hugging Face](https://huggingface.co/HuggingFaceBio/Carbon-X) (수정 10-22 09:55 UTC, 공식) · [Wired](https://www.wired.com/story/hugging-face-new-ai-biology-model-chat-with-dna/) (10-22 10:00 UTC, 매체보도)

### 6. 장애

- **OpenAI, 한국·일본·인도에서 ChatGPT 대화와 Responses API 오류:** 10-22 02:18 UTC에 시작해 02:42 완화, 08:50 UTC 완전 복구됐다. [인시던트](https://status.openai.com/incidents/01M5GPE9C4E19HE461P8JAMYXD)
- **OpenAI FedRAMP Astra·Sol API 오류(후속):** 직전 판에서 "identified"였던 건이 19:34 UTC에 해결됐다. 약 5시간 17분 이어졌다. [인시던트](https://status.openai.com/incidents/01M5FD63HHDRPQPM5YMQWJ5HTN)
- **OpenAI 사후 공개:** 10-20 16:35~17:35 UTC 일리노이 인근 사용자의 API·ChatGPT·Codex 인증 오류는 설정 변경 롤백으로 복구됐다. 공지는 10-21 22:36 UTC에 올라왔다. [인시던트](https://status.openai.com/incidents/01M5G9QQ9V4CYDWFRYF9BWZ5GG)
- **Anthropic:** Claude Fable 5 오류 증가(10-21 19:49~20:29 UTC), claude.ai·Cowork 커넥터 오류(10-22 03:50~08:07 UTC). [Fable 5](https://status.claude.com/incidents/njk29r81v3wg) · [커넥터](https://status.claude.com/incidents/wj6r4t6mxq1r)
- **Google Cloud:** 창 안 신규 인시던트 없음.

### 7. 짧게

- **DeepSeek, 약 150억 달러 조달 보도:** 미국 연방 민간기관 퇴출 명령 이틀 뒤다. CATL과 Tencent가 잠재 투자자로 거론되고, 기업가치는 보도에 없다. 6월에는 500억 달러 가치에 70억 달러 이상을 조달했다. Bloomberg 원문은 읽지 못했다. [TechCrunch](https://techcrunch.com/2026/10/22/deepseek-reportedly-raises-15b-days-after-us-restrictions/) (10-22 13:11 UTC, 매체보도)
- **Meta 3분기:** AI 투자·인수·보상용 무의결권 주식 클래스를 새로 만든다(거래는 2027년부터). 2026년 설비투자 전망 하단을 1,350억 달러로 올렸다. Muse 사용자는 1,000만 명을 넘었고 설치자의 약 25%가 매일 쓴다. [TechCrunch](https://techcrunch.com/2026/10/21/meta-says-muse-has-over-10m-users-with-shopping-a-top-activity/) (10-21 21:14 UTC) · [The Verge](https://www.theverge.com/tech/1015799/meta-shares-class-ai-q3-2026-earnings)
- **OpenAI, 프로젝트별 서비스 티어 제한:** Platform 설정에서 프로젝트가 쓸 수 있는 서비스 티어를 고르고, 조직 단위로도 제한할 수 있다. 변경 로그에는 10-21 날짜만 있고, 직전 판 조회 때는 없던 항목이다. [변경 로그](https://developers.openai.com/api/docs/changelog) (공식)
- **Baseten mammoth-1(27B)·elephant-1(9B):** Qwen3.5를 사후 학습한 "결정 모델"로, 텍스트 대신 답 후보의 확률 분포를 돌려준다. 오픈소스이고 1주간 무료다. [Baseten](https://www.baseten.co/blog/the-best-decision-models-in-the-world-mammoth-1-and-elephant-1/) (10-21 16:58 UTC)
- **"Claude는 전갈 해독제를 발명하지 않았다"(10-21 판 후속):** 실험 주체는 투자리서치사 Citrini였고, Anthropic이 미공개 모델과 약 5만 달러어치 연산을 댔다. 외부 실험에서 설계 211개 중 17개가 독소 5종 중 4종에 결합했지만, 둘 이상에 결합한 단백질은 없었다. [The Verge](https://www.theverge.com/ai-artificial-intelligence/1016443/anthropic-claude-citrini-vibecoding-scorpion-antivenom) (10-22 12:21 UTC)
- **NVIDIA GTC Berlin 2일차:** Cosmos 데이터 팩토리 블루프린트 5종을 냈다. Nemotron 3.5 기반 Retriever 임베딩·리랭크(1~8B, 34개 언어), Nemotron Parse 개선판도 소개됐다. 창 안에 새 Hugging Face 모델은 없었다. [NVIDIA](https://blogs.nvidia.com/blog/cosmos-open-train-evaluate-deploy-world-models/) · [The Register](https://www.theregister.com/ai-and-ml/2026/10/21/nvidia-woos-wary-europeans-with-more-open-weights-models/5304552)
- **Anthropic 장기이익신탁(LTBT)에 물리학자 Christopher Stubbs 합류:** 전 하버드 과학대 학장이다. [Anthropic](https://www.anthropic.com/news/christopher-stubbs-joins-long-term-benefit-trust) (날짜 10-21)
- **예정 확인:** MiniMax M3.1 Flash 가중치는 창 안에 나오지 않았다. Gemini API `deep-research-pro-preview-12-2025`는 예정대로 10-23 종료된다. GPT-6 계열의 Google Cloud 정식 출시 소식은 없었다.
- **반복 없음:** Mistral Large 4가 HN에 다시 올라왔지만(322점) 10-06 발표의 재게시다. 가중치는 여전히 "이달 말"이다.

## 기술 이슈

### 1. PHP2Shell 후속: 공개 4일 전부터 악용 시도가 있었고, 지금은 "광범위하게 악용 중"이다

- **사전 악용:** Amazon 위협 인텔리전스는 허니팟망 MadPot에서 10-17의 PHP2Shell 악용 시도를 찾았다. 공개 4일 전이고, 출발지는 상용 익명화 네트워크였다. 공격자가 어떻게 미리 정보를 얻었는지는 불명확하다고 했다.
- **공개일:** 엠바고 해제(10-21 14:00 UTC) 12시간 전인 02:00 UTC부터 PoC가 여럿 돌았고, 08:00 UTC까지 여러 공격자의 시도가 이어졌다. Amazon은 "PHP2Shell is now being broadly exploited in the wild"라고 썼다. 다만 이 시도들이 실제로 성공했는지는 분석 중이다.
- **Amazon의 권고:** 인터넷에 노출된 시스템의 치명적 취약점은 패치 시한을 시간 단위로 잡으라고 했다. "7일·30일 SLA는 더는 존재하지 않는 지연을 전제한다"는 이유다. 침해 조사는 공개일이 아니라 10-17부터 하라고 권했다.
- **규모(매체 인용):** The Register는 Cloudflare CEO의 말을 전했다. 공개 전 10시간 동안 수천 건, 공개 45분 만에 수만 건, 2시간 만에 수십만 건이었고 지금은 수백만 건이다.
- **엠바고 논쟁:** PHP 재단은 Chainguard가 10-21 07:41 UTC에 패치된 빌드를 일방적으로 공개했다고 비판했다. Chainguard CEO는 "유출이 아니다"라고 반박했다. "PHP에 뭔가 있다"는 말만 듣고, 자사 AI 하네스에 간단한 프롬프트를 줘 같은 버그를 독립적으로 찾았다는 주장이다.
- **반론(HN 593점, 창 안 PHP2Shell 관련 1위):** GitHub에 도는 "PoC" 대부분은 바닐라 PHP에서 동작하지 않는다는 글이다. LFI 같은 기존 취약점을 전제로 하거나 LLM이 만든 엉터리 익스플로잇이 많다고 했다. 이 글도 패치는 하라고 권한다.
- **아직 없는 것:** CISA KEV(10-21 카탈로그)에는 두 CVE가 없다. 한 연구자가 8.5.12에서도 재현되는 별개의 double free를 보고했다고 주장했지만, oss-security에 따르면 PHP 측은 "관련 없고 실제 문제 아님"이라고 답했다(미확인). 창 안에 새 PHP 릴리스나 권고는 없다.

중요한 이유: 패치 공개일을 첫 악용일로 볼 수 없다는 근거를 대형 클라우드가 공식으로 냈다. Amazon은 "AI 보조 분석이 패치 diff를 익스플로잇으로 바꾸는 비용을 낮췄다"고 썼다. 다만 공개 PoC 상당수는 작동하지 않는다는 반론도 있어, "악용 시도"와 "성공한 침해"는 구분해서 봐야 한다. 아직 PHP를 올리지 않았다면 10-17 이후 로그부터 조사해야 한다.

[Amazon](https://aws.amazon.com/blogs/security/amazon-threat-intelligence-identifies-php2shell-exploitation-prior-to-public-disclosure-in-madpot/) (10-21 22:36 UTC, 공식) · [The Register: 악용 급증](https://www.theregister.com/cyber-crime/2026/10/22/hackers-pile-on-php2shell-as-aws-reveals-attacks-began-before-disclosure/5304749) (10-22 12:15 UTC) · [The Register: Chainguard](https://www.theregister.com/security/2026/10/22/php-blames-supply-chain-security-biz-chainguard-for-putting-huge-chunks-of-the-web-at-risk/5304611) (10-21 23:02 UTC, 매체보도) · [Elias Taylor 반론](https://eliastaylor.com/noexploit/) (날짜 10-21, 커뮤니티) · [oss-security](https://www.openwall.com/lists/oss-security/2026/10/21/23) · [HN 50188948](https://news.ycombinator.com/item?id=50188948) · [HN 50194070](https://news.ycombinator.com/item?id=50194070) (Amazon 글)

### 2. LiteLLM 1.107.0 후속: 인터넷에 노출된 내부 LiteLLM 로그에서 GitHub 토큰이 털렸다

- **LiteLLM 발표:**
  - 공개 인터넷에 노출된 내부 LiteLLM 배포에서 정체불명의 사용자 계정을 찾았다.
  - 공격자는 이 배포의 로그에서 엔지니어들의 GitHub CLI OAuth 토큰을 얻은 것으로 본다. 그중 하나가 조직 owner 두 명 중 한 명의 토큰이었다.
  - 공격자는 이 토큰으로 브랜치 보호를 끄고 릴리스 워크플로 변경을 강제 푸시했다. 이어 유사 저장소 `Berri-AI/litellm`을 소스로 서명·발행했다.
  - 내부 배포에 어떻게 들어왔는지는 확정하지 못했고, "최근 패치된 취약점"을 의심한다.
- **Claude Code 관련 주장:** LiteLLM은 "특정 상황에서 Claude Code가 사용자의 GitHub CLI OAuth 토큰을 응답 본문에 넣고, 응답 본문을 로깅하면 LiteLLM 로그에 남는다"고 썼다. 조회 시점에 Anthropic의 응답은 찾지 못했다(미확인).
- **사후 악용(Wiz):** 탈취 약 4.5시간 뒤인 10-21 16:20~17:01 UTC에 공격자가 CI 러너·K8s 노드에서 빼 간 AWS 역할 자격증명을 검증하고 `bedrock:ListFoundationModels`를 호출했다. Wiz는 이를 LLMjacking 패턴으로 봤다. 악성 버전 다운로드는 약 4.9만 회로 집계됐다.
- **그 밖:** OSV `MAL-2026-2144`에 1.107.0이 추가됐고, GHSA는 아직 없다. 엔터프라이즈 비공개 레지스트리에는 **10-27 공개 예정인 취약점**의 수정판이 이미 배포되고 있다.

중요한 이유: 출처 증명과 Trusted Publisher를 갖춘 파이프라인도 개발자 토큰 몇 개로 뚫렸다. LLM 게이트웨이의 요청·응답 로그는 그 자체로 비밀 저장소가 된다. 셀프호스팅 LiteLLM 프록시를 인터넷에 열어 둔 곳은 사용자 목록과 로그 보존 설정을 점검하고, 10-27 공개 예정 취약점에도 대비해야 한다.

[LiteLLM 사고 글](https://docs.litellm.ai/blog/security-incident-oct-21-2026) (첫 게시 10-21 18:20 UTC, 20:30 UTC 갱신, 공식) · [Wiz](https://www.wiz.io/blog/litellm-compromised-steals-credentials) (갱신 10-21 19:05·10-22 12:55 UTC) · [OSV MAL-2026-2144](https://osv.dev/vulnerability/MAL-2026-2144) · [이슈 #47826](https://github.com/BerriAI/litellm/issues/47826)

### 3. Railway 침해: 고객 환경변수가 복호화된 채 유출됐을 수 있다. "AI 추론 제공자를 노린 일련의 공격"이라고 밝혔다 (HN 549점)

- **무슨 일:** 고객 환경변수를 저장하는 시스템에서 이상 활동이 감지됐다. 제3자가 암호화된 환경변수에 접근했고, 일부는 복호화된 내용까지 얻었을 수 있다.
- **표적:** Railway는 이번 공격이 "AI 추론 제공자를 침해할 목적으로 플랫폼 기업들을 노린 최근 일련의 공격"의 일부라고 봤다. 업스트림 제공자들에게 키를 선제적으로 일괄 폐기하라고 요청했다.
- **조치:** 모든 사용자에게 환경변수·API 키·레지스트리 자격증명 교체를 권고했다. Anthropic·OpenAI·Grok·OpenRouter·AWS·GCP·GitHub·Stripe와 DB별 점검 가이드를 따로 냈다. 배포가 멈췄다가 Pro·Enterprise부터 재개됐고, Hobby·Free·Trial 빌드는 10-22 15:29 UTC까지 막혀 있었다. 영향 사용자 수와 근본 원인은 공개되지 않았다.

중요한 이유: AI 제공자 키를 모아 두는 플랫폼이 한 주 사이 세 번째로 뚫렸다. OpenCode Console(10-19 판), LiteLLM(2번), Railway다. Railway에 키를 둔 적이 있으면 사용 여부와 상관없이 교체하고, 제공자 콘솔에서 예상 밖 사용량을 확인해야 한다.

[Railway 공지](https://blog.railway.com/p/security-disclosure-wednesday-october-21st) (10-21 20:44 UTC, 공식) · [Anthropic 키 점검 가이드](https://blog.railway.com/p/security-disclosure-recommendation-check-for-unexpected-anthropic-usage) · [상태 페이지](https://status.railway.com/incident/QH8L8B3E) · [TechCrunch](https://techcrunch.com/2026/10/22/developer-platform-railway-urges-all-users-to-revoke-secret-keys-after-cyberattack/) (10-22 14:03 UTC) · [HN 50188228](https://news.ycombinator.com/item?id=50188228)

### 4. Microsoft: 중국 국가 연계 Storm-0940이 오픈소스 에이전트형 침투 도구 ARTEX로 ConnectWise 공급망 공격을 확장했다

- **도구:** ARTEX는 Go로 쓴 다중 에이전트 LLM 침투 테스트 프레임워크로, 어떤 모델이든 붙일 수 있다. Storm-0940은 9월부터 이를 써서 수백 개 조직을 노렸고, 일부는 표적 선정에서 클라우드 특권 획득까지 3시간이 안 걸렸다.
- **공급망:** ConnectWise Cloud Backup 앱의 ID를 장악한 뒤 2,250개가 넘는 테넌트에 분당 2,900회 로그인을 시도했다.
- **확산:** Microsoft는 다른 지역의 공격자들도 ARTEX를 쓰고 있다고 밝혔다.

중요한 이유: 오픈소스 에이전트 공격 도구가 국가 연계 공급망 공격에 쓰였다고 대형 벤더가 공식 확인했다. 경보가 떠도 사람이 대응할 시간이 거의 없는 속도라는 점이 핵심이다.

[Microsoft 보안 블로그](https://www.microsoft.com/en-us/security/blog/2026/10/21/storm-0940-uses-artex-agentic-ai-connectwise-supply-chain-compromise/) (10-21 18:23 UTC, 공식) · [The Register](https://www.theregister.com/security/2026/10/22/microsoft-says-connectwise-breach-let-chinese-spies-loose-on-2250-cloud-tenants/5304687) (10-22 11:28 UTC)

### 5. MCP Inspector v2에도 high 2건: 원격 MCP 서버에 연결만 해도 Windows·WSL에서 코드 실행 (정정·후속)

- **정정:** 직전 판에서 "v2(최신 2.11.0)는 영향 범위에 없다"고 썼다. 하지만 같은 날 v2 2.0.0~2.11.0을 대상으로 한 별도 권고가 나왔다. **2.12.0으로 올려야 한다.**
- **high [GHSA-q978-78c7-phqw](https://github.com/modelcontextprotocol/inspector/security/advisories/GHSA-q978-78c7-phqw):** 원격 MCP 서버가 OAuth 탐색 문서의 `authorization_endpoint`에 임의 문자열을 넣는다. 이 URL을 여는 npm `open` 패키지가 문자열을 PowerShell 큰따옴표 안에 넣어 실행하므로 `$(...)`가 평가된다. 모델 호출이나 승인 없이 연결만 해도 터진다.
- **high [GHSA-vcc9-3xgm-p9v4](https://github.com/modelcontextprotocol/inspector/security/advisories/GHSA-vcc9-3xgm-p9v4):** CVE-2025-49596 수정이 불완전했다. DNS 리바인딩으로 API 토큰과 저장된 OAuth 토큰을 훔칠 수 있다.
- **medium [GHSA-m5pj-3536-3xcp](https://github.com/modelcontextprotocol/inspector/security/advisories/GHSA-m5pj-3536-3xcp):** `/api/fetch`에 응답 크기 제한이 없다. v1 계열도 해당하며 1.0.3에서 고쳤다.

[2.12.0 릴리스](https://github.com/modelcontextprotocol/inspector/releases/tag/2.12.0) (10-21 18:13 UTC, 권고 18:31~18:32 UTC, 공식) · 설치: `npx @modelcontextprotocol/inspector@latest`

### 6. 보안 권고 짧게

- **Qdrant 1.20.0 미만, critical 9.8:** 분산 모드의 내부 gRPC(6335)가 기본으로 인증 없이 열려 있고, 공개 API와 같은 주소에 바인딩된다. 클러스터 밖 호스트가 피어로 붙어 샤드 복제본(데이터)을 받거나 Raft 상태를 바꿀 수 있다. 단일 노드는 해당 없다. [GHSA-frc7-44gm-99qq](https://github.com/qdrant/qdrant/security/advisories/GHSA-frc7-44gm-99qq) (10-22 14:29 UTC)
- **LocalAI v4.6.0~v4.11.0, critical 9.2(CVE-2026-114493):** `POST /models/apply`가 첫 설정 URL만 검증하고 리디렉트를 따라가, 클라우드 메타데이터에 닿는 SSRF가 된다. 수정 PR은 아직 열려 있고, 10-22 나온 v4.12.0 노트에도 언급이 없다. **올려도 안 고쳐질 수 있으니** 이 엔드포인트를 외부에 열지 말아야 한다. [GHSA-vhg8-m8fr-3863](https://github.com/advisories/GHSA-vhg8-m8fr-3863) (10-21 18:33 UTC) · [PR #12791](https://github.com/mudler/LocalAI/pull/12791)
- **chrome-devtools-mcp 1.1.0~1.4.0, medium 2건:** 심볼릭 링크로 MCP roots 경계를 우회하고, 서비스 워커가 보낸 fetch는 URL 허용·차단 패턴을 우회한다. 1.5.0으로 올린다. [GHSA-5hw5-4q87-m6x9](https://github.com/ChromeDevTools/chrome-devtools-mcp/security/advisories/GHSA-5hw5-4q87-m6x9) · [GHSA-xfxg-8r4q-wj4v](https://github.com/ChromeDevTools/chrome-devtools-mcp/security/advisories/GHSA-xfxg-8r4q-wj4v) (10-22 12:14 UTC)
- **MLflow 3.12.0rc0~3.17.0 미만, medium:** 인증 없는 scorer invoke 엔드포인트가 공격자가 지정한 클래스를 만든다. trulens+litellm 플러그인이 깔려 있으면 SSRF가 된다. [GHSA-2wqp-q3c6-ccj5](https://github.com/mlflow/mlflow/security/advisories/GHSA-2wqp-q3c6-ccj5) (10-22 05:12 UTC)
- **Goose(CVE-2026-111079, high):** 터미널 통합을 켜면 셸 히스토리를 비밀까지 그대로 LLM 제공자에게 보낸다. 수정 버전 정보는 아직 없다. [GHSA-4537-c6fq-qchm](https://github.com/advisories/GHSA-4537-c6fq-qchm) (10-22 12:31 UTC)
- **Hermes Agent(CVE-2026-114463):** 웹훅 route 이름에 `:`를 넣으면 더 높은 권한 route의 도구 세트를 얻는다. v0.22.2(10-15)에서 고쳤고 CVE만 창 안에 나왔다. [GHSA-4h98-2jhp-qgm2](https://github.com/advisories/GHSA-4h98-2jhp-qgm2)
- **PyPI 악성 패키지:** TTS·ASR·벤치마크 도구를 사칭한 원격 코드 로더 43개가 10-22 00~03시 UTC에 몰려 올라왔다(OpenSSF 분류 `2026-10-voice-synthesis-profiles`). Anthropic SDK 타이포스쿼트 `aanthropic`도 나왔다. [modelworks](https://github.com/advisories/GHSA-cppv-x8vc-pgq6) · [aanthropic](https://github.com/advisories/GHSA-9793-cg29-78xv)
- **늦은 미러는 신규 아님:** Storybook(`@storybook/addon-mcp` 포함), Next.js 이미지 최적화 RCE, virtualenv 9건, Pillow 30여 건은 저장소 공개가 10-12~10-17이다. SGLang 라우터 권한 우회 수정 PR #45017은 여전히 미병합이다.

### 7. 연구 짧게

- **StreamEval(HF Daily 10-22 1위):** 대화형 스트리밍 비디오 생성 벤치마크다(5개 과제, 300케이스, 모델 37개). 최고 모델도 아바타 지시 이행 36.9점, 참조 편집 성공 15.0점에 그쳤고, 화질과 지시 이행이 따로 놀았다. [논문](https://huggingface.co/papers/2610.28402)
- **Cache Engineering for Agents(AKV):** 에이전트가 궤적을 고치지 않고 KV 캐시를 직접 버리거나 재배치한다. 임계치 요약 방식보다 점수 +1.7, 비용 −27.7%였다(자사 측정). [논문](https://huggingface.co/papers/2610.28577)
- **Agent4Sci(Anthropic):** 과학 ML 벤치마크 20개 전부에서 프런티어 에이전트가 공개 최고치를 넘었다. 다만 개선 대부분이 ML 엔지니어링 요령이었고, 연구용 스킬을 붙이자 Claude Opus의 평균 상대 개선이 10.4%에서 20.1%로 올랐다(자사 측정). [논문](https://huggingface.co/papers/2610.28763)
- **WildAWM(Microsoft Research):** GUI·MCP·CLI를 함께 쓰는 실행 가능한 컴퓨터 사용 환경 341개를 오픈 모델만으로 합성했다. GPT-6 Astra도 60%만 풀었다. [논문](https://huggingface.co/papers/2610.26828)
- **WorldTUI(HN 409점):** 셸·파일시스템 없이 키 입력마다 터미널 화면 변화를 예측하는 360M 파라미터 "터미널 월드 모델"이다(Qwen2.5-Coder-0.5B 기반 미세조정). vim·tmux가 돌아가고, 자기 실수를 고치게 학습시킨 것과 화면 행 병렬 디코딩이 가장 효과가 컸다고 한다. 브라우저 WebGPU 데모(208MB)가 있다. [글](https://simedw.com/2026/10/21/world-model-terminal/) · [HN 50190363](https://news.ycombinator.com/item?id=50190363) (10-22 02:18 UTC)

### 8. 커뮤니티·AI 밖 대형 사고

- **curl "Ignore the Slop"(후속):** Stenberg는 "2026년 봄 이후 환각성 슬롭 보고는 사실상 사라졌다"고 썼다. 최근 몇 달 받은 보고 수백 건 모두 AI가 관여했고, 이제 문제는 "고품질·대량"이라는 것이다. 직전 판 LOW·MEDIUM 공개 정책의 배경이다. [블로그](https://daniel.haxx.se/blog/2026/10/22/ignore-the-slop/) (10-22 05:46 UTC)
- **리눅스 커널 취약점 보고가 공개 메일링리스트에 실수로 게시(HN 189점):** ZDI가 KSMBD·SMB Direct RCE(CVSS 9.0) 등 여러 건을 비공개 security 리스트가 아닌 LKML로 보냈다. 일부는 아직 메인라인 패치가 없다. [Phoronix](https://www.phoronix.com/news/LKML-New-Trend-Vulnerabilities) (10-22 10:05 UTC)
- **GitLab CVSS 10.0 무인증 RCE(CVE-2026-110264, HN 394점):** AI와 무관하지만 7.0 이후 전 버전이 영향을 받는다. 같은 릴리스에 AI Gateway 기능 헤더 관련 medium 건도 있다. [GitLab 19.5.1](https://docs.gitlab.com/releases/patches/patch-release-gitlab-19-5-1-released/) (10-21, HN 19:01 UTC)
- **eMudhra 후속:** 직전 판의 중간 CA 키 노출 이후 eMudhra가 글로벌 SSL/TLS 인증서 사업에서 철수한다고 발표했다(HN 412점). mitmproxy는 노출된 키를 아직 신뢰하는 클라이언트를 시험하는 `--emudhra` 옵션을 병합했다(HN 558점). [보도자료](https://emudhra.com/en/press-release/emudhra-to-exit-global-ssl-tls-certificate-business) · [mitmproxy PR #8500](https://github.com/mitmproxy/mitmproxy/pull/8500)

### 참고: 창 경계 항목

- **Symantec "Skynet" LLMjacking 에이전트:** 보호되지 않은 LLM 엔드포인트를 탈취해 최대 10만 단계를 무인으로 도는 공격 에이전트다. 5단 탈옥 프롬프트와 모듈 74개(스캔·익스플로잇·DB 덤프·채굴)를 갖췄고, 허니팟에 12.7만 건 넘는 요청이 들어왔다. 원문은 10-21 10:00 UTC로 직전 창이고, 창 안 The Decoder 보도(10-22 11:30 UTC)로 실었다. 3번과 같은 흐름이다. [Symantec](https://www.security.com/threat-intelligence/ai-llmjacking-skynet) · [The Decoder](https://the-decoder.com/an-ai-agent-in-skynet-mode-hijacks-unprotected-language-models-to-launch-self-sustaining-cyberattacks/)
- **Trail of Bits, AI 패치의 회귀 검토:** 에이전트 보조 패치 130여 건 중 21건이 회귀였다는 검토와 플러그인은 도구 7번에 정리했다.
- **Google DeepMind 직원들, 군사 계약에 반발해 사직(BBC):** 10-21 15:41 UTC로 창 시작 19분 전이다. [BBC](https://www.bbc.com/news/articles/cmreyvepnv0po)
- **Cohere Rerank 5 Pro·Fast:** BRIGHT 82.2점(자사 측정), 문서 안 관련 구간을 돌려주는 Context Focus, 100만 토큰당 $0.05(Pro)·$0.02(Fast)다. 10-21 15:00 UTC로 창 직전이다. [Cohere](https://cohere.com/blog/rerank-5)

## 써볼 만한 도구

MCP Inspector 2.12.0(v2 취약점 수정)은 기술 이슈 5번에, Claude Duet은 모델 소식 3번에 정리했다. MCP Inspector v2를 쓴다면 아래 도구보다 2.12.0 업데이트가 먼저다.

### 1. GitHub CLI v2.103.0: `gh auth login --expiring`, 유출돼도 곧 쓸모없어지는 토큰

- **설명:** `gh`가 수명이 짧은 액세스 토큰과 회전하는 리프레시 토큰을 저장하고, 필요할 때 알아서 갱신한다(공개 프리뷰). github.com과 GHE.com에서 되고, 지원하지 않는 호스트에서는 기존 무기한 토큰으로 돌아간다. `gh auth status`로 만료 시각을 볼 수 있다.
- **추천 이유:** LiteLLM 침해(기술 이슈 2번)의 핵심 고리가 로그에 남은 무기한 GitHub CLI OAuth 토큰이었다. 토큰이 몇 시간 뒤 만료되면 로그·환경변수로 새어 나가도 피해 시간이 크게 줄어든다.
- **주의:** `GH_TOKEN`·`GITHUB_TOKEN`으로 넣은 토큰은 갱신되지 않는다. `gh auth token` 출력을 캐시하는 스크립트·확장은 결국 만료된 토큰을 쥐게 되니 매번 다시 불러야 한다. HTTPS git은 git 2.46 이상에서 `gh auth setup-git`으로 `gh`를 자격증명 도우미로 쓰라고 권한다.
- **사용:** 한 번만 `gh auth login --expiring`, 항상 쓰려면 `gh config set expiring_credentials enabled`
- **링크:** [GitHub Changelog](https://github.blog/changelog/2026-10-21-github-cli-opt-in-to-expiring-auto-refreshing-oauth-tokens) (10-21 16:43 UTC, 공식) · [v2.103.0](https://github.com/cli/cli/releases/tag/v2.103.0) (릴리스 15:33 UTC로 창 시작 27분 전)

### 2. Claude Code v2.1.307: 새 API 버전으로 옮겨 주는 `/claude-api api-version`

- **설명:**
  - `/claude-api api-version`이 Claude API 코드를 `2026-10-21` 버전으로 옮겨 준다. 모델 소식 1번의 400 조건(샘플링 파라미터 등)을 손으로 찾아 고치지 않아도 된다.
  - `claude_code.safety_event` OTel 이벤트가 생겼다. 권한 규칙·auto mode·안전 검사·훅·샌드박스가 막거나 확인을 물은 도구 호출마다 남는다.
  - HTTP MCP 도구 호출이 연결이 끊긴 순간 **두 번 전송되던** 문제를 고쳤다. 이제 read-only·idempotent 도구만 재시도한다.
  - 사용자 지정 `ANTHROPIC_BASE_URL` 게이트웨이와 Foundry 뒤에서 효과 없는 `prompt-caching-scope` 베타 헤더가 나가던 문제를 고쳤다.
- **추천 이유:** 결제·쓰기처럼 부작용이 있는 MCP 도구가 중복 실행될 수 있었으니 바로 올릴 만하다. 보안팀은 safety_event로 에이전트가 무엇을 시도하다 막혔는지 모을 수 있다.
- **참고:** v2.1.308(10-22 04:34 UTC)은 "버그 수정"뿐이다. LiteLLM이 주장한 "Claude Code가 gh 토큰을 응답 본문에 넣는다"는 문제와 관련된 변경은 노트에 없다.
- **설치:** `claude update`
- **링크:** [v2.1.307](https://github.com/anthropics/claude-code/releases/tag/v2.1.307) (10-21 22:17 UTC, 공식) · [v2.1.308](https://github.com/anthropics/claude-code/releases/tag/v2.1.308) · [Agent SDK TS 0.3.307](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.307)

### 3. VS Code 1.143: Copilot harness가 기본이 되고, 여러 에이전트를 붙여 비교하는 Compare Models

- **설명:**
  - Copilot SDK 기반 Copilot harness가 새 세션의 기본이 됐다.
  - **Compare Models(프리뷰):** 같은 작업을 모델 여럿이 따로 풀게 한다. Copilot·Claude·Codex harness와 추론 수준을 섞을 수 있다. Judge 모델이 시간·토큰과 근거를 담은 채점표를 내고, `Synthesize`로 추천안에 다른 시도의 아이디어를 합친다.
  - **온디바이스 모델(실험):** MAI Code 1.1 Flash를 llama.cpp `llama-server`로 로컬에서 돌린다. GitHub 로그인과 **GPU 메모리 96GB 이상의 NVIDIA GPU**가 필요하다.
  - 비활성 에이전트 워크트리 정리, BYOK OpenAI 음성 모델, 샌드박스 자격증명 프록시용 CA 관리도 들어왔다.
- **추천 이유:** 하네스·모델 선택을 감이 아니라 같은 과제의 나란한 결과로 정할 수 있다.
- **주의:** Compare Models는 `sessions.chat.compareAgents.enabled`로 켠다. 조직 정책이 Copilot harness를 강제하면 새 Claude·Codex 세션을 만들 수 없다.
- **링크:** [릴리스 노트](https://code.visualstudio.com/updates/v1_143) · [GitHub 1.143.0](https://github.com/microsoft/vscode/releases/tag/1.143.0) (10-22 13:00 UTC, 공식)

### 4. Mistral Vibe 3.0.2: 서브에이전트 권한 규칙이 지금까지 무시되고 있었다

- **설명:**
  - 사용자 지정 서브에이전트의 `[tools.*]` `permission`·`allowlist`·`denylist`와 `enabled_skills`·`disabled_skills`가 **무시되고 있었다.** 이제 메인 에이전트 규칙 위에 적용된다.
  - `hooks.toml` 훅이 서브에이전트 안의 도구 호출에도 돈다.
  - 서브에이전트 기본 한도가 동시 생존·실행 8/4개에서 32/32개로 올라갔다(`max_subagents`·`max_concurrent_subagents`로 조정).
- **추천 이유:** 서브에이전트에 더 좁은 권한을 줬다고 믿었다면 실제로는 메인 규칙만 적용되고 있었다. 기본 한도가 크게 올라간 점도 비용 면에서 확인할 만하다.
- **설치:** `uv tool upgrade mistral-vibe`
- **링크:** [v3.0.2](https://github.com/mistralai/mistral-vibe/releases/tag/v3.0.2) (10-22 15:34 UTC, PyPI 업로드는 16:02 UTC로 창 직후, 공식)

### 5. Zed v1.25.2: 원격 프로젝트의 에이전트·MCP 비밀값이 로컬 프로세스 목록에 보였다

- **설명:**
  - 원격 프로젝트에서 외부 에이전트·MCP 서버에 넘긴 환경변수(비밀 포함)가 로컬 머신의 프로세스 목록에 보이던 문제를 고쳤다.
  - ACP 외부 에이전트에 context compaction과 세션 공지를 켰다.
  - 불필요한 git 체크포인트 때문에 외부 에이전트로 프롬프트가 늦게 가거나 안 가던 문제를 고쳤다.
  - 바이너리가 macOS 약 33%, Windows 28%, Linux 7% 작아졌다. Bedrock 공급자에 GPT-6 Astra·Claude Opus 5.5가 추가됐다.
- **추천 이유:** 여러 사람이 쓰는 머신에서 원격 개발을 한다면 `ps`만으로 API 키가 보였다.
- **링크:** [v1.25.2](https://github.com/zed-industries/zed/releases/tag/v1.25.2) (10-21 20:54 UTC, 공식)

### 6. Omnara: 셀프호스트하는 "Managed Agents" 구조 (Show HN 100점)

- **설명:** 에이전트 루프를 무상태 워커가 Postgres의 append-only 이벤트 로그 위에서 돌린다. 반복 한 번을 트랜잭션 하나로 커밋해, 워커가 죽어도 다른 워커가 이어받는다. macOS·Linux 기기에서 `omnarad` 데몬을 돌리면 아웃바운드 연결만으로 붙어, 노트북과 클라우드 샌드박스를 한 에이전트에 연결할 수 있다. Go, Apache-2.0이다.
- **추천 이유:** 자체 모델과 자체 인프라로 Claude Managed Agents 같은 내구성 있는 장기 실행 에이전트를 만들고 싶을 때 참고할 만하다. docker compose로 단일 서버에도 올라간다.
- **링크:** [GitHub](https://github.com/omnara-ai/omnara) · [HN 50183892](https://news.ycombinator.com/item?id=50183892) (Show HN 10-21 16:44 UTC, 커뮤니티)

### 7. Trail of Bits `regression-hunt`·`patching-bugs` 플러그인 (창 경계)

- **설명:** `patching-bugs`는 버그를 재현·수정·테스트한다. `regression-hunt`는 diff를 실제 인터페이스까지 추적해 회귀 후보를 찾고, 배포본에서 PoC로 재현한 뒤 새 컨텍스트의 에이전트로 다시 확인한다.
- **수치(자사 측정):** "PR 회귀를 검토하라"고만 지시한 기본 Codex보다 오탐이 25% 적었다. Patch the Planet에서 병합된 에이전트 보조 패치 130여 건을 다시 보니 21건이 회귀였다(기능 16, 성능 1, 빌드·테스트 4). 보안 보호를 깬 것은 없었다.
- **추천 이유:** AI가 만든 패치를 대량으로 받는 프로젝트가 늘고 있다(기술 이슈 8번 curl). 병합 전 회귀 검토를 자동화하는 도구다.
- **설치:** Claude Code `/plugin marketplace add trailofbits/skills`, Codex `codex plugin marketplace add trailofbits/skills`
- **링크:** [블로그](https://blog.trailofbits.com/2026/10/21/your-ai-agent-fixed-the-bug-but-what-else-did-it-change/) (10-21 15:00 UTC로 창 시작 1시간 전, HN 16:03 UTC) · [regression-hunt](https://github.com/trailofbits/skills/tree/main/plugins/regression-hunt)

### 8. GitHub Copilot CLI v1.0.102

- **설명:** 서브에이전트가 사용자 지정·MCP 도구 승인을 상속해 같은 권한 질문이 반복되지 않는다. 샌드박스 네트워크가 호스트의 `NO_PROXY`·`ALL_PROXY`를 따르고, `-p`·stdin 실행이 관리형 Auto 모델 선택을 따른다. 공급자 과부하로 끊긴 응답은 백오프로 재시도한다.
- **설치:** `npm i -g @github/copilot`
- **링크:** [v1.0.102](https://github.com/github/copilot-cli/releases/tag/v1.0.102) (10-21 18:05 UTC, 공식)

### 짧게

- **eMudhra 대응이 AI 도구 체인으로 번졌다(후속):** Haystack 3.4.1이 `certifi>=2026.10.19`를 요구하고, langchain-core 1.6.13·langchain 1.4.7도 certifi를 올렸다. OpenCode 1.19.4는 Bun 1.4.4로 올리며 새 NSS 루트를 반영했다. mitmproxy 12.3.0은 신뢰 목록에서 eMudhra를 뺐다. [Haystack 3.4.1](https://github.com/deepset-ai/haystack/releases/tag/v3.4.1) · [OpenCode 1.19.4](https://github.com/anomalyco/opencode/releases/tag/v1.19.4) · [mitmproxy 12.3.0](https://github.com/mitmproxy/mitmproxy/releases/tag/v12.3.0)
- **Recursive Flint 로컬 실행:** Ollama v0.40.8에서 `ollama run flint`로 돌린다(현재 macOS). llama.cpp b11803도 Flint를 지원한다. 모델 자체는 모델 소식 4번. [Ollama v0.40.8](https://github.com/ollama/ollama/releases/tag/v0.40.8) · [llama.cpp b11803](https://github.com/ggml-org/llama.cpp/releases/tag/b11803)
- **OpenAI CLI(HN 207점):** OpenAI REST API용 공식 Go CLI다(`openai <resource> <command>`, Apache-2.0). 5월에 나온 저장소가 이번에 주목받았고, 창 안 릴리스(v1.40.1·1.40.2)는 사소한 변경이다. [GitHub](https://github.com/openai/openai-cli) · [HN 50191507](https://news.ycombinator.com/item?id=50191507)
- **JetBrains Air Governance 정식 출시:** 옛 JetBrains Central이다. Claude Agent·Codex·Gemini CLI·Junie 등 ACP 에이전트의 승인 목록, 크레딧 한도, 중앙 BYOK, 회사 단위 MCP 설정, 비용 분석을 한 콘솔에서 다룬다. Business 플랜은 개발자당 월 20달러다. [JetBrains 블로그](https://blog.jetbrains.com/blog/2026/10/22/air-governance-manage-ai-agents-access-and-spend-throughout-your-organization/) (10-22 14:09 UTC)
- **LangGraph.js 체크포인트 네임스페이스 강화:** 직전 판 Python 권고의 JS판 대응이다. `@langchain/langgraph-checkpoint` 1.2.1과 `checkpoint-postgres` 1.1.1이 store 네임스페이스 라벨의 `:`·NUL을 거부한다. JS 쪽 GHSA는 없다. [PR #3071](https://github.com/langchain-ai/langgraphjs/pull/3071)
- **Qwen Code v0.26.0:** 상시 에이전트 배포용으로 `--check` 사전 점검, systemd 유닛, 자격증명 부트스트랩, `qwen daemon start/restart`가 들어왔다. [릴리스](https://github.com/QwenLM/qwen-code/releases/tag/v0.26.0) (10-21 23:20 UTC)
- **Strands Agents python 1.62.0·TS 1.22.0:** swarm 도구가 추가됐고(TS는 호환성 깨짐), 호출 한도와 사용량 집계가 에이전트 트리 전체에 적용된다. [python v1.62.0](https://github.com/strands-agents/sdk-python/releases/tag/python/v1.62.0)
- **LiteLLM:** 창 안에 정상 릴리스가 없다. 메인테이너는 레지스트리를 잠그고 정규 릴리스를 멈췄다. 1.106.2 고정 권고는 그대로다.
- **MCP 레지스트리:** 창 안에 버전이 게시된 서버는 2,252개였고 대부분 대량 등록 계정이었다. 대형 벤더 신규 서버는 없었다.
- **창 안 릴리스 없음:** anthropics/skills, MCP servers·SDK 전반, openai-agents, Codex 정식판, Gemini CLI 0.66 정식판, Google ADK, Cursor, Cline, Aider, goose, Continue, Roo Code, vLLM, SGLang, Pydantic AI, Vercel AI SDK.

## 주목할 점

- **공격자는 모델이 아니라 키가 모이는 곳을 노린다.** OpenCode Console, LiteLLM, Railway가 한 주 사이 차례로 뚫렸고, 탈취 몇 시간 뒤 Bedrock 모델 목록을 조회하는 LLMjacking 패턴이 확인됐다. 게이트웨이 로그 마스킹, 만료되는 토큰(`gh auth login --expiring`), 제공자 쪽 사용량 경보가 기본 위생이 되고 있다.
- **프런티어 랩이 중개·재판매 경로를 직접 통제하기 시작했다.** Anthropic은 최종 고객 단위 제재(user profiles)를, OpenAI는 라우터·게이트웨이 사전 승인과 SafetyID를 내놨다. 11-20 시행 전까지 OpenRouter 같은 게이트웨이가 어떻게 대응하는지 지켜볼 만하다.

## 조사 제약

- **본문을 직접 읽지 못한 원문:** openai.com(403)은 RSS와 r.jina.ai로 대신 읽었다. Bloomberg(CAPTCHA)는 읽지 못해 DeepSeek 조달은 TechCrunch 정리를 기준으로 썼다. The Hacker News 기사는 403이라 RSS만, Microsoft 보안 블로그는 curl 시간 초과라 r.jina.ai로 읽었다. JetBrains 블로그는 자동화 접근에 202를 준다. Cloudflare CEO 수치, Hadrian 보고, Chainguard CEO 글은 The Register 인용으로만 확인했다. 이 링크들은 자동화 접근만 막히고 브라우저에서는 열린다.
- **시각이 날짜만 있는 원문:** Anthropic 마이그레이션 가이드(게시 시각 미상, 직전 판 조회 이후), Claude Duet·Claude Security 글(10-21, 트윗 시각으로 보강), OpenAI 정책 글과 서비스 티어 변경 로그(10-21), LTBT 글, Elias Taylor 글, GitLab 19.5.1이다.
- **확인하지 못한 것:** LiteLLM이 주장한 "Claude Code가 gh 토큰을 응답 본문에 넣는다"는 문제는 Anthropic 입장을 찾지 못했다. Railway의 영향 사용자 수와 근본 원인은 공개되지 않았다. PHP 8.5.12의 세 번째 버그 주장은 PHP 측이 부인했다. 벤치마크 계정 트윗에만 나온 Kimi K3.1·Qwen 4는 공식 근거가 없어 제외했다.
- **놓쳤을 수 있는 범위:** 전역 GHSA는 창 안 511건을 AI·에이전트·MCP 키워드로 걸렀고 재현하지 않았다. HN은 창 안 1,135건을 중간 시각으로 나눠 전수 조회했다. GitHub REST API가 간헐적으로 빈 목록을 돌려줘 재시도했지만, 일부 저장소의 릴리스 목록은 받지 못했다. Hugging Face Daily Papers 10-23분은 아직 열리지 않았다. Vertex AI·Gemini Enterprise 릴리스 노트와 alizila 피드는 캐시가 묵어 있다.
- **자사 측정:** Recursive Flint, Carbon-X, Cohere Rerank 5, Baseten 결정 모델, StreamEval, AKV, Agent4Sci, WildAWM, Trail of Bits 회귀 수치는 모두 자사나 작성자 측정이며 독립 검증 전이다.
