---
title: "2026-11-24 AI 브리핑"
date: 2026-11-24T08:00:00+09:00
tags: [ai-briefing, anthropic, zai, nvidia, security]
description: "Anthropic 내부 연구용 Claude 모델이 VP≠VNP를 증명했다고 공개했고, Z.ai는 첫 루프 트랜스포머 플래그십 GLM-5.4를 냈으며 NVIDIA는 Jensen Huang을 CEO로 복귀시켰다. Claude Ship이 PR 승인까지 맡기 시작했고 CISA는 MONAI Label 0-day를 악용 목록에 넣었다."
---

> 조사 범위: 2026-11-23 08:00 ~ 2026-11-24 08:00 KST(2026-11-22 23:00 ~ 11-23 23:00 UTC, 미국 일요일 15:00 ~ 월요일 15:00 PST). 직전 브리핑(11-23 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`·`merged_at`·커밋 시각, npm 게시 시각, Hugging Face API, ModelScope 등록 시각, SEC `acceptanceDateTime`, X 스노우플레이크, Red Hat CSAF, 저장소·전역 GHSA `published_at`, CISA KEV JSON, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-23 23시대 UTC) 값이다.

## 오늘의 핵심 요약

- **Anthropic 내부 연구용 Claude 모델이 VP ≠ VNP를 증명했다고 공개했다.** 논문도 Lean 증명도 AI가 썼고, 수학자 검토는 없다. P ≠ NP를 푼 것은 아니다. 리만 가설 Lean 증명은 판정 도구 Comparator(v4.33.0)와 Lean 기본 커널을 통과했다. v4.36.0 실행은 아직 돌고 있다.
- **모델과 시장이 함께 움직였다.** Z.ai가 첫 루프 트랜스포머 플래그십 GLM-5.4를 냈지만 API와 가중치는 아직 없다. ChatGPT Chat 탭은 GPT-6.2로 바뀌고 GPT-5.6은 12-23에 내려간다. Claude Opus 4.5는 2027-01-22에 은퇴한다. NVIDIA 이사회는 Jensen Huang을 CEO로 복귀시켰고 주가는 31% 올랐다. 이사회 조사는 끝나지 않았다.
- **에이전트 도구의 권한과 구멍이 같이 커졌다.** Anthropic의 Claude Ship은 PR을 고치고 정책이 허락하면 승인까지 한다. 설정에 따라 사람 없이 병합될 수 있다고 문서가 경고한다. CISA는 AI 에이전트가 찾아낸 MONAI Label 0-day를 악용 목록에 넣었다(기한 11-26). LMCache·LiteLLM·crewAI는 하루에 권고 11건을 냈다.

## 모델 소식

주말이 끝나 공식 채널이 다시 열렸다. Z.ai가 루프 트랜스포머 플래그십 GLM-5.4를 냈고, OpenAI는 ChatGPT Chat 탭의 모델을 GPT-6.2로 바꿨다. Claude Platform 변경 로그는 11-17에서 11-23으로 움직였다. 업계에서는 NVIDIA 이사회가 Jensen Huang을 CEO로 복귀시킨 일이 가장 컸다. 나머지 변경 로그는 그대로다(Claude 앱 11-20, OpenAI API 11-19, Gemini API·AI Studio 11-19, Gemini 앱 11-17, xAI 11월, DeepSeek 11-13). OpenRouter 신규 등록은 0건이다. Anthropic 내부 연구 모델이 낸 VP ≠ VNP 증명은 기술 이슈 1번에 정리했다.

### 1. Z.ai가 첫 루프 트랜스포머 플래그십 GLM-5.4(750B급)를 냈다. API도 가중치도 아직 없고, 750B 모델의 벤치마크 점수도 없다 (공식, X 04:18 UTC·블로그 09:55 UTC, HN 1,000점대)

- **구조(블로그 원문):** "GLM-5.4 loops 28 layers of the GLM-5 base model for two iterations." 처음부터 학습하지 않고 이미 사전학습한 모델을 이어 학습해 루프로 바꿨다. 추론 비용에 대해서는 원문 "an extra loop only costs one sliding window of KV cache rather than a full context cache"라고 했다. 지능 주장은 원문 "reaching intelligence levels comparable to 2T-3T parameter models"다. 자사 주장이다.
- **지금 쓸 수 있는 곳:** GLM Coding Plan, ZCode, chat.z.ai뿐이다. 문서 원문: "The Model API will be made available upon completion of the security assessment." 블로그 원문: "We plan to open-source the model weights responsibly after completing necessary safety hardening." 컨텍스트는 1M, 최대 출력은 128K다. 입력은 비디오·이미지·텍스트·파일이다.
- **값:** API 단가는 없다. Coding Plan 크레딧 배수(입력/캐시/출력)는 GLM-5.4가 4.6/1.1/16으로, GLM-5.3의 6.9/1.7/24보다 낮다. 문서 원문: "the available quota for GLM-5.4 is increased to 1.5× that of GLM-5.3."
- **수치를 읽는 법:**
  - 블로그 표는 **30B 실험 모델**의 것이다(이미지 직접 확인, 기준 → 루프): AIME25 19.17 → 30.42, GPQA-Diamond 47.58 → 56.26, AA-LCR 40.33 → 47.67, MMLU-Pro 71.65 → 75.05, IFBench 50.20 → 53.47. 원문: "only ~10% more total training compute".
  - 문서에는 750B의 승률 그림이 있다(승/무/패): GLM-5.3 상대 51.1/25.7/23.1%, Kimi K3.1 Pro 상대 41.8/26.9/31.3%, DeepSeek-V4.1-Flash-1017 상대 36.6/30.2/33.2%. 어떤 과제를 누가 채점했는지 설명이 없다. DeepSeek의 Flash 모델과는 사실상 비슷하다는 뜻이기도 하다.
  - HN의 비판도 750B 벤치마크가 없다는 점에 모인다.
- **한계(블로그 원문):** "it tends to get caught in overly long "marathon thinking" loops". 속도는 한산할 때 최대 200 tokens/s, 혼잡할 때 70~80 tokens/s다.
- **왜 중요한가:** 파라미터를 늘리지 않고 같은 층을 두 번 돌려 추론을 깊게 하는 방식을 Z.ai가 자사 플래그십에 처음 넣었다. 블로그는 다음 모델이 원문 "significantly larger"라고 예고했다. 가중치와 API가 열려야 주장을 검증할 수 있다. 실행 지원 상황은 도구 절에 적었다.

[Z.ai 블로그](https://z.ai/blog/glm-5.4) · [문서](https://docs.z.ai/guides/llm/glm-5.4.md) · [Coding Plan](https://docs.z.ai/devpack/overview.md) · [The Decoder](https://the-decoder.com/z-ai-follows-openai-into-loop-transformer-territory-with-its-new-750b-flagship-model/) · [HN](https://news.ycombinator.com/item?id=50609685)

### 2. ChatGPT Chat 탭이 GPT-6.2(Sol 변형)로 바뀐다. GPT-5.6 Sol·Terra는 12-23에 내려간다 (공식, X 19:04 UTC·21:42 UTC, 릴리스 노트·시스템 카드 날짜 11-23)

- **무엇:** 릴리스 노트 원문: "GPT‑6.2 in ChatGPT is powered by a version of GPT‑6.2 Sol for Plus, Pro, Business, and Enterprise tiers, and is tuned for everyday conversation." 원문: "The models powering Work and Codex are not changing as part of this release." Free와 Go는 대상이 아니다.
- **수치(OpenAI 내부 평가, GPT-6 → GPT-6.2):** 이미지 이해 47.4 → 71.7%, 어려운 지시 준수 62.5 → 78.1%, 내부 BrowseComp 54.4 → 64.5%. 외부 측정은 아직 없다.
- **시스템 카드:**
  - 등급은 사이버 Critical, 생물·화학 High, AI 자기개선 High다. GPT-6.2 Astra·Sol과 같은 등급이고 새 지정이 아니다.
  - 거절이 줄었다. 원문: "GPT-6.2 Astra and GPT-6.2 Sol were less resistant to jailbreaks, so we adjusted the refusal boundary to make them more conservative". 이번 판은 그 조정 없이 나온다. 무해한 생물학 질문을 과하게 거절하지 않은 비율이 Sol 86.2%, Astra 90.9%에서 97.6%로 올랐다. 대신 중대한 생물 위해 항목에서 안전했던 비율은 99.8%에서 98.7%로 조금 내려갔다.
  - 청소년 정서 의존 평가는 GPT-6 Sol 77.1%에서 94.3%로 올랐다.
- **은퇴(공식 X 원문):** "On December 23, we're deprecating GPT-5.6 Sol in Chat, and GPT-5.6 Sol and Terra in Work and Codex." ChatGPT·Codex 이야기다. API 변경 로그와 deprecations에는 새 항목이 없다.
- **함정:** Codex에서 GPT-5.6 Sol·Terra를 설정 파일에 고정해 두었다면 12-23 전에 바꾼다. Chat 탭의 GPT-6.2와 API의 GPT-6.2 Sol은 같은 모델이 아니다.

[릴리스 노트](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) · [시스템 카드](https://deploymentsafety.openai.com/gpt-6-2-chatgpt) · [X(배포)](https://x.com/ChatGPT/status/2124942186425069836) · [X(은퇴)](https://x.com/ChatGPT/status/2124981905380569283) · [The Decoder](https://the-decoder.com/openai-updates-chatgpt-to-handle-complex-everyday-questions-web-searches-and-images-more-reliably/)

### 3. NVIDIA 이사회가 Jensen Huang을 CEO로 복귀시켰다. 독립 조사는 끝나지 않았다 (공식, 8-K 13:57 UTC·보도자료, HN 1,800점대)

- **공식 사실:** 8-K 원문: "On November 22, 2026, the Board of Directors of NVIDIA Corporation reinstated Jen-Hsun Huang". 결정은 11-22, 발표는 11-23이다. 보도자료 원문: "the Board received input from senior levels of the United States government, and based on that input, the Board is confident that Mr. Huang’s reinstatement poses no national security risk." 원문: "The Board reiterated that the committee’s investigation will proceed to conclusion." Kress는 CFO로 남고, 최고준법책임자(CCO) 자리를 새로 만든다. 엿새 전(11-17) 이사회는 복귀시키지 않겠다고 했다(11-18 판).
- **정부 개입(매체보도):** 트럼프는 CNBC에 이사회와 원문 "directly and indirectly" 이야기했다고 했다. 다른 CNBC 기사 원문: "“I made the deal,” Trump told CNBC’s Eamon Javers". Semafor는 트럼프가 일요일 이사회에 전화로 들어갔다고 전했다. 같은 기사 원문: "role as a protector of open-source models, and a check on the increasing power of closed frontier model makers like Anthropic and OpenAI, a person familiar with the matter told Semafor."
- **시장(CNBC):** NVIDIA는 31.48%, Nasdaq은 6.51% 올랐다. Anthropic은 상장 사흘째 거래에서 1.43% 내렸다.
- **다음 일정:** 12-01 하원 3개 위원회, 12-03 상원 은행위원회 공개 청문회다(TechCrunch). Megaspeed가 상무부 Entity List에 오를 것이라는 Semafor 보도가 있다. 상무부 확인은 찾지 못했다.
- **왜 중요한가:** AI 칩 공급의 최대 불확실성이 일단 줄었다. 하지만 이사회 조사, 의회 청문회, 수출 통제 문제는 그대로 남아 있다.

[NVIDIA](https://nvidianews.nvidia.com/news/nvidia-board-reinstates-jensen-huang-as-president-ceo-and-board-director) · [8-K](https://www.sec.gov/Archives/edgar/data/1045810/000104581026000090/nvda-20261122.htm) · [CNBC](https://www.cnbc.com/2026/11/23/nvidia-jensen-huang-ceo.html) · [CNBC(인터뷰)](https://www.cnbc.com/2026/11/23/trump-nvidia-jensen-huang-reinstatement-cnbc-interview-transcript.html) · [CNBC(증시)](https://www.cnbc.com/2026/11/23/anthropic-nvidia-shares.html) · [Semafor](https://www.semafor.com/article/11/23/2026/huang-reinstatement-came-at-trumps-behest) · [TechCrunch(청문회)](https://techcrunch.com/2026/11/23/jensen-huang-scheduled-to-testify-next-week-in-senate-following-reinstatement-as-nvidia-ceo/) · [TechCrunch(Megaspeed)](https://techcrunch.com/2026/11/23/megaspeed-the-company-at-the-center-of-the-jensen-huang-scandal-appears-headed-for-a-u-s-blacklist/) · [HN](https://news.ycombinator.com/item?id=50611576)

### 4. Claude Platform: Opus 4.5가 2027-01-22에 은퇴한다. Fable·Mythos를 ZDR로 쓰는 Enterprise Frontier Safeguards 베타가 나왔다 (공식, 릴리스 노트 날짜 11-23)

- **Opus 4.5 은퇴(원문):** "We announced the deprecation of the Claude Opus 4.5 model (`claude-opus-4-5-20251101` and `claude-opus-4-5`), with retirement on the Claude API scheduled for January 22, 2027. We recommend migrating to Claude Opus 5.5." 같은 표에서 **Sonnet 4.5는 다음 주 월요일(11-30)에 은퇴한다.** 날짜 없는 별칭 `claude-sonnet-4-5`도 함께 내려간다.
- **EFS(원문):** "EFS is Zero Data Retention (ZDR) for eligible organizations that use Claude’s most capable models". 원문: "Anthropic doesn’t retain your prompts and responses. Encrypted copies of them, called Sealed Safety Data, go to a bucket in your own cloud account." Fable·Mythos 계열은 30일 보존이 조건이라, 기존 ZDR 워크스페이스에서 부르면 따로 허가받지 않는 한 400 오류가 났다. 추가 요금은 없다. 저장 비용은 고객이 낸다.
- **함정:**
  - 자기 버킷에 있어도 고객은 열어 볼 수 없다. 원문: "Your team can’t read Sealed Safety Data, even in your own bucket."
  - 검토 책임이 고객에게 온다. Safety Case는 7일(달력 기준) 안에 답해야 하고, 답하지 않으면 접근이 정지될 수 있다.
  - 원문: "EFS is currently available for your organization’s internal use only." 고객에게 파는 서비스에는 못 쓴다. HIPAA 조직도 쓸 수 없다. Batch API와 Managed Agents도 지원하지 않는다.
  - 워크스페이스 전체에 적용된다. Access Transparency는 한 번 켜면 조직 전체에서 끌 수 없다. 기존 ZDR(Legacy) 조직은 새 워크스페이스를 만들어야 한다.

[릴리스 노트](https://platform.claude.com/docs/en/release-notes/overview) · [은퇴 일정](https://platform.claude.com/docs/en/about-claude/model-deprecations) · [EFS 개요](https://platform.claude.com/docs/en/manage-claude/enterprise-frontier-safeguards/overview) · [EFS 설정](https://platform.claude.com/docs/en/manage-claude/enterprise-frontier-safeguards/setup-console) · [Safety Case](https://platform.claude.com/docs/en/manage-claude/enterprise-frontier-safeguards/flagged-cases) · [Engadget](https://www.engadget.com/2325684/anthropic-softens-its-data-retention-policy-for-mythos-and-fable-model-use/)

### 5. (직전 판 1번 후속) Microsoft가 OpenAI 평가 세트로 잰 MAI-2 Base의 퍼플렉서티를 공개했다. OpenAI 제안에 답한 첫 회사다 (공식 X, Suleyman 00:37 UTC)

- **원문:** "We're committed to sharing our pre-training progress on a common held-out evaluation set. With OpenAI's permission, we report MAI-2 Base on their evaluation set. Its log-perplexity is 0.819, putting it in the same ballpark as the GPT-5.6 Sol base model." 직전 판에서 확인한 GPT-5.6 Sol 값은 0.828이고, 미공개 내부 모델은 0.732다.
- **함정(원문):** "the fact we have access to OpenAI's evaluation dataset is a special case." 토크나이저가 같아 비교된다고도 했다. 다른 랩에는 해당하지 않는 조건이다. 그래서 원문 "we might need to report bits-per-byte (BPB) rather than perplexity since different labs use different tokenizers"라고 덧붙였다.
- **다른 반응:** Anthropic·Google·Meta의 직접 답은 찾지 못했다. The Decoder(18:42 UTC)는 글의 Millennium Prize 사례를 Navier–Stokes로 읽었지만, OpenAI 본문은 어느 문제인지 밝히지 않는다.

[Suleyman X](https://x.com/mustafasuleyman/status/2124663594449318173) · [The Decoder](https://the-decoder.com/openai-warns-that-upcoming-ai-systems-could-outpace-safety-checks-and-urges-rivals-to-agree-on-shared-limits/)

### 6. Ant Group이 Ling-3.2-air(124B, 활성 5.5B)를 MIT로 냈다 (공식 저장소, ModelScope 등록 07:39 UTC)

- **무엇:** 직전 판의 LingSante-air-VL과 같은 계열의 네이티브 멀티모달 MoE다. README 수치(Ling-3.0-flash-VL 대비)는 AutomationBench-AA 15.7 → 61%, GDP.pdf 8.4 → 16%, AA-Briefcase Elo 987 → 1340, Terminal-Bench 4.0 0 → 27%다. 원문: "scores **23** on the **Artificial Analysis Intelligence Index v5.0.1**". AA 페이지도 23으로 적었다.
- **함정:** 법률 벤치마크 Harvey LAB-AA는 항목 통과율이 87.7%다. 하지만 README 원문: "Its all-pass rate is **8.3%** before the material hallucination check and **0.8%** after it." 권장 구성은 141GB급 GPU 4장이다. 호스팅 API는 아직 없다. 추적 중인 Ling 3.2 Flash와는 다른 모델이고, Flash는 여전히 401이다.

[Ling-3.2-air](https://huggingface.co/inclusionAI/Ling-3.2-air) · [Artificial Analysis](https://artificialanalysis.ai/models/ling-3-2-air)

### 7. Amazon이 Decart의 추론 최적화 기술을 사고 창업자와 팀을 데려온다 (공식, 13:46 UTC)

- **무엇:** 원문: "signed an agreement to purchase the proprietary technology behind the Decart Optimization Stack (DOS)". 회사 전체 인수가 아니라 기술 매입과 인력 합류다. 원문: "Completion of the transaction is subject to customary closing conditions, including regulatory clearance." Trainium 성능을 끌어올리는 것이 목적이다.
- **금액은 공식 발표에 없다.** $7.5B는 매체 보도이고, CNBC 원문은 "Amazon declined to comment on the terms of the deal."이다. CNBC는 Bloomberg를 인용해 원문 "Anthropic had considered acquiring Decart and conducted due diligence, but ultimately walked away from a deal"이라고 전했다.

[Amazon](https://www.aboutamazon.com/news/company-news/amazon-decart-ai-optimization) · [CNBC](https://www.cnbc.com/2026/11/23/amazon-decarts-ai-acquisition.html) · [TechCrunch](https://techcrunch.com/2026/11/23/amazon-snaps-up-decarts-ai-optimization-tech-for-7-5b-as-it-prepares-to-sell-trainium-chips/)

### 8. 짧게

- **Claude 장애(major):** 09:49 UTC 게시, 11:07 UTC 해결. 원문: "Impact occurred from 01:39 PT / 09:39 UTC to 01:56 PT / 09:56 UTC." Mythos 5.2, Fable 5.2, Opus 5.5, Sonnet 5.5, Haiku 5.5 등이 대상이었다. [상태](https://stspg.io/q2ptbwdjx31m)
- **OpenAI 장애(minor):** "Elevated errors with GPT-6 conversations on paid ChatGPT plans", 15:50~16:38 UTC. [상태](https://status.openai.com/)
- **Gemini API 파일 슬라이싱(16:12 UTC):** 원문: "request only the pages, spreadsheet cells, media segments, and layers you need". 원문: "works across all Gemini 4 models". 긴 파일에서 필요한 부분만 넣어 토큰을 줄인다. 변경 로그에는 아직 없다. [Google](https://blog.google/innovation-and-ai/technology/developers-tools/file-slicing-controls/)
- **Google 검색 AI Mode 개인화(17:00 UTC):** Wallet 연동은 오늘부터다. Drive는 원문 "with support for Drive and the Gemini app following in the coming weeks"다. Drive도 이미 된다는 식의 보도가 있으니 공식 문구를 따른다. [Google](https://blog.google/products-and-platforms/products/search/google-search-personal-intelligence-apps/)
- **Oregon·Virginia가 SpaceX(Grok)를 제소(CNBC 12:40 UTC):** 원문: "Virginia and Oregon are the first states to sue Elon Musk's SpaceX". 딥페이크 성적 이미지와 안전 주장이 쟁점이다. 소장은 읽지 못했다. [CNBC](https://www.cnbc.com/2026/11/23/oregon-virginia-spacex-grok-deepfake-lawsuits.html)
- **(매체보도) xAI 컴퓨트 임대:** The Decoder가 NYT를 인용해 Anthropic과의 계약이 월 약 $3.1B라고 전했다. 원문: "Either deal can be canceled with 90 days' notice." [The Decoder](https://the-decoder.com/anthropics-appetite-for-compute-is-pushing-xai-toward-a-rent-over-build-strategy/)
- **Anthropic "Working with AI" 연구:** 고객사 인터뷰 조사다. 원문: "Across our interviews, we saw very few examples of domestic layoffs that were driven by AI automation." 자사 고객 대상 조사라는 점을 감안한다. [Anthropic](https://www.anthropic.com/research/working-with-ai)
- **Tencent WeMM-Ecom(0.8B~35B-A3B):** Qwen3.5 기반 전자상거래 비전 언어 모델이다. 11-18에 만든 저장소에 창 안 가중치가 올라왔다. [35B-A3B](https://huggingface.co/tencent/WeMM-Ecom-35B-A3B)
- **직전 판 후속:**
  - Anthropic 상장: 창 안 SEC 제출은 Form 4 네 건뿐이고 종결 8-K는 없다.
  - 가중치 401 유지: Qwen4-Max, Qwen4-Max-Preview(11-25 종료), Ling 3.2 Flash, **ERNIE 5.5 Flash Preview(11-25 공개 약속 하루 전인데 아직 401)**, LG K-EXAONE 2.5, 그리고 새로 GLM-5.4. FLUX.1-schnell은 여전히 gated다.
  - Inkling 2 Mini HF 카드는 평가표만 고쳤다(08:08 UTC). Hy4 transformers PR #50903은 열려 있다.
  - Cursor의 중국 모델 퇴출 검토: CNBC 기사가 11-23에 수정됐지만 11-20 내용이고 새 진전이 아니다.
  - 소식 없음: 리만 가설 증명의 모델 이름과 Anthropic 공식 글, MiniMax 공식 입장, Gemini 4 Neon 안전 보고서, SimpleBench, OpenAI 증류 글 후속, NVIDIA SEC 조사·캘리포니아 AG, NVIDIA–Inception, DeepSeek V5, METR, 한국 랩 모델 발표.

### 참고: 창 경계 항목

- **(X 11-22 17:06 UTC, 직전 창) LeCun의 Amodei 비판(HN 17:07 UTC 23일 게시, 910점대):** 원문: "Dario's spiel since GPT-2 has always been about hyping the dangers so that open models would be made illegal and *he* would stay in control." Amodei의 의회 발언을 다룬 글에 단 답글이다. 기술 근거를 새로 내놓은 글은 아니다. [X](https://x.com/ylecun/status/2124550037912920090) · [HN](https://news.ycombinator.com/item?id=50615308)
- **(X 11-22 22:50 UTC, 창 10분 전) Meta Alexandr Wang의 글:** The Decoder(13:59 UTC)가 요약했다. X 아티클 본문은 읽지 못했다. Decoder는 리만 건을 "proof sketch"라고 불렀는데, 실제로는 완성된 Lean 증명이다. [The Decoder](https://the-decoder.com/metas-alexandr-wang-warns-the-ai-industry-isnt-ready-for-whats-coming-after-riemann-breakthrough/)
- **(저장소 11-20, TechNode 창 안 06:52 UTC) MiniMax 내부 모델의 암호 분석 결과:** README 원문: "These results were primarily discovered by our internal model." TII McEliece 공개 문제에서 개인키 94개를 되찾았고 그중 55개는 첫 해독이다. 최대 43,400배 빨라졌다는 값은 이 문제에 한한 것이다. AES·RSA 같은 표준을 깬 것이 아니다. [저장소](https://github.com/MiniMax-AI/Research) · [TechNode](https://technode.com/2026/11/23/minimax-publishes-ai-discovered-cryptographic-attacks-with-papers-and-code/)
- **(Vals 11-18, The Decoder 창 안 12:17 UTC) GPT-6.2 Astra의 Smoktunowicz gap theorem 형식화:** 2006년에 이미 증명된 정리를 Lean으로 옮긴 것이다. 94분, $21.58이 들었다. 새 난제를 푼 것이 아니다. [Vals](https://www.vals.ai/blogs/smoktunowicz-gap-theorem)

## 기술 이슈

수학 쪽에서 두 가지가 함께 움직였다. Anthropic이 내부 연구 모델의 VP ≠ VNP 증명을 공개했고, 리만 가설 Lean 증명은 판정 도구 Comparator의 v4.33.0 판을 통과했다. 보안 쪽에서는 CISA가 MONAI Label을 악용 목록에 넣었고, LMCache·LiteLLM·crewAI가 하루에 권고 11건을 냈다. 전역 GHSA 창 안 게시분은 reviewed 9건, unreviewed 370건, malware 117건이다(직접 셈). reviewed 9건은 모두 저장소에 먼저 공개됐던 옛 권고다.

### 1. Anthropic의 내부 연구용 Claude 모델이 VP ≠ VNP(밸리언트 추측)를 증명했다고 공개했다. 논문도 Lean 증명도 AI가 썼고, 수학자 검토는 없다 (공식 저장소, PR #51 병합 21:43 UTC, HN 22:09 UTC 100점대)

- **주장(README 원문):** "For every field, circuits for the Hamiltonian cycle polynomial, and for the permanent outside characteristic 2, need more than m^(c (log m)^(1/3)) gates for all large m." 원문: "So VP is a proper subset of VNP, and VNP is not contained in the closure of VP. This does not settle P vs NP." VP ≠ VNP는 P ≠ NP의 대수판이다. 논문 원문: "no implication in the other direction is known". 이 결과에서 P ≠ NP는 나오지 않는다.
- **누가 찾았나(README 원문):** "The result was discovered by an internal research Claude model." 원문: "Both were written by AI models, and no human mathematician has checked the proof." 모델 이름은 밝히지 않았다. 프리프린트 서버 Hexagon의 기록은 어감이 다르다. 원문: "Anthropic researchers worked with Claude to improve the presentation." 라벨은 "Human understanding: some parts"다.
- **검증 상태(직접 확인):** 저장소 CI가 22:05 UTC에 통과했다. 로그에 "nanoda kernel accepts the solution", "Lean default kernel accepts the solution", "Your solution is okay!"가 있다. 하지만 Anthropic 자체 CI이고, 외부 재검증은 창 안에 없다. 규모는 Lean 324개 파일, 약 7.3만 줄이다. 리만 가설 증명(1,740만 줄)보다 훨씬 작아 남이 다시 돌려 보기 쉽다.
- **함정:**
  - 명제 자체는 기계가 확인하지 못한다. README 원문: "Lean checks the proofs, but no machine checks that this file says what is claimed." 논문 원문: "Whether the statements say what is meant was judged only by AI readers."
  - 논문만 읽어서는 검증할 수 없다. 논문 원문: "Consequently the contracting case, and with it every main result, rests on Lean at these points."
  - 논문 원문: "We have not analysed how it relates to relativization and algebrization". 복잡도 이론의 알려진 장벽을 어떻게 비켜 갔는지 저자 쪽 설명이 없다.
- **공식 글과 전문가 반응은 창 안에 없다.** anthropic.com의 뉴스·연구 페이지에는 이 결과가 없고, 루트 README 표에도 아직 빠져 있다.
- **왜 중요한가:** 리만 가설 건과 달리 Anthropic 공식 저장소에서 나왔고, 모델 출처(내부 연구 모델)를 처음부터 적었다. 직전 판의 OpenAI 글처럼 출시하지 않은 내부 모델의 성과가 먼저 나오는 흐름이다. 복잡도 이론 학자들이 명제 파일을 어떻게 읽는지가 다음 관문이다.

[README](https://github.com/anthropics/formal-math/blob/main/vp-vnp/README.md) · [논문](https://github.com/anthropics/formal-math/blob/main/vp-vnp/paper.pdf) · [PR #51](https://github.com/anthropics/formal-math/pull/51) · [CI 실행](https://github.com/anthropics/formal-math/actions/runs/45272623486) · [Hexagon](https://hexagonmath.org/2611.00954) · [HN](https://news.ycombinator.com/item?id=50620102)

### 2. 리만 가설 Lean 증명: Comparator v4.33.0이 "Your solution is okay!"를 냈고 Lean 기본 커널도 전체를 통과했다. Alpöge가 처음으로 길게 해명했다 (공식 검증자 문서 14:35 UTC, X 22:03 UTC)

- **직전 판의 남은 질문에 답이 나왔다(Sutherland 페이지, "Page last updated 2026-11-23 14:35 UTC"):**
  - 원문: "Lean's own kernel also accepts the full export, and Comparator v4.33.0 reports “Your solution is okay!”."
  - 기본 커널은 원문 "48 h 3 min on one thread"로 끝났다. 할당자 설정 하나를 바꾸면 9시간 8분이면 끝난다. 그 설정은 원문 "changes nothing about what is checked"다. paranoid 커널도 같은 설정에서는 통과했다.
- **남은 것(원문):** "The lake comparator (v4.36.0) run and the paranoid-kernel replay of the export are still in progress."
- **함정:** 통과한 Comparator는 처음부터 끝까지 새로 돌린 것이 아니다. 원문: "its lake build step was satisfied by a stub". 공동체 저장소 노트의 원문 "Comparator's output will be added here when the run finishes."도 아직 그대로다. 검증 도구 쪽에서는 `leanchecker --from-export`가 맞는 증명을 거절하는 버그(#15643)가 열려 있다. 틀린 증명을 받아들이는 문제는 아니다.
- **Alpöge의 해명(X):** 원문: "The Lean is full of garbage obviously, but compiles, but all criticism of such garbage is totally reasonable and accepted, I should have cleaned it up". 25만 쪽 이야기는 원문 "250,000+ pages was a joke guys!"다. 사람이 읽을 원고가 없는 이유는 원문 "I didn’t feel comfortable having any slop writeup attached to this problem"이다.
- **커뮤니티:** Micjm의 글(04:57 UTC, HN 250점대)은 유명 정리 259개의 Lean 분량을 세어 1,740만 줄이 이례적으로 크다고 했다. 증명 구조를 트리맵으로 그린 시각화도 나왔다(HN 180점대). 공동체 저장소는 쓰지 않는 파일을 지우는 정리를 이어 간다.
- **별개의 증명 시도:** Masullo 원고의 Lean 형식화 지도가 HN 250점대에 올랐다. 이 저장소는 AI 에이전트 PR이 검토 속도를 넘겨 새 작업 신청을 멈췄다. 원문: "New claims are paused".

[Sutherland 검증 페이지](https://math.mit.edu/~drew/grh/) · [lean4 #15643](https://github.com/leanprover/lean4/issues/15643) · [Alpöge X](https://x.com/__alpoge__/status/2124987173439209561) · [검증 노트](https://github.com/leanprover-community/riemann-hypothesis/blob/main/verification/NOTES.md) · [Micjm](https://micjm.com/blog/yes-17m-lines-of-lean-is-a-lot/) · [트리맵](https://pengowray.github.io/riemann-zeta-proof-visualizer/) · [Masullo 형식화](https://github.com/mccorvie/riemann-masullo) · [HN](https://news.ycombinator.com/item?id=50607668)

### 3. CISA가 MONAI Label을 KEV에 넣었다. 공격자의 AI 에이전트가 찾아낸 0-day이고, 조치 기한은 11-26이다 (공식, KEV 19:43 UTC 판)

- **추가 2건:** MONAI Label CVE-2026-133968과 N-able Mail Assure CVE-2026-134624다. 둘 다 기한이 11-26(추수감사절)이다. KEV 원문: "allows an unauthenticated remote attacker to write arbitrary files outside the intended datastore directory, potentially resulting in remote code execution". 11-21 판에서 다룬, Cairn 에이전트가 스스로 찾아 쓴 그 결함이다.
- **직전 판 함정이 풀렸다:** 저장소 권고의 수정 버전 칸이 `0.8.6`으로 채워졌다.
- **남은 함정:** 저장소 권고(GHSA-qc22-rmgc-vg94)는 전역 DB에 없다. 전역에 있는 NVD 미러(GHSA-2f43-rwqm-6h67)는 패키지 매핑이 비어 있어 Dependabot이 울리지 않는다. 세션 업로드 결함은 CVE가 없고 KEV 대상도 아니다. 0.8.6으로 올리면 둘 다 고쳐진다.
- **N-able Mail Assure:** THN 기준 CVSS 9.3이고 수정 빌드는 20261121.1943이다.

[KEV JSON](https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json) · [MONAI Label 권고](https://github.com/Project-MONAI/MONAILabel/security/advisories/GHSA-qc22-rmgc-vg94) · [THN](https://thehackernews.com/2026/11/n-able-discloses-critical-mail-assure.html)

### 4. LMCache critical 1건, LiteLLM 8건, crewAI high 1건. 추론·게이트웨이 서버를 운영하면 오늘 확인한다 (공식, 저장소 권고 직접 확인)

- **LMCache(19:36 UTC, critical 9.8, `>= 0.4.7, < 0.5.8` → 0.5.8):** 프런트엔드 프록시가 UNIX 소켓 경로를 검사하지 않았다. 원문: "An unauthenticated remote caller therefore reaches any UNIX domain socket on the host". 원문: "POST /run_script through the proxy returned uid=0(root)." 코드 실행은 `run_script_api_enabled`가, 환경 변수 노출은 `internal_api_server_enabled`가 켜져 있어야 한다. 앞의 설정은 공식 프런트엔드 문서 예시에 들어 있다.
  - 직전 판은 0.5.8을 기본값 변경으로만 소개했다. 보안 수정판이기도 했다.
  - **함정:** 고친 뒤에도 원문 "The frontend still binds 0.0.0.0 by default."다. 시작할 때 선언한 소켓은 API 키를 두지 않으면 여전히 인증 없이 닿는다.
  - 함께 나온 medium(GHSA-jg4p-wprm-8hph)은 LoRA 어댑터 사이에서 캐시가 섞이는 문제다. 원문: "Clear L2 tiers written by a multi-adapter deployment before the upgrade".
- **LiteLLM(13:18~15:56 UTC, high 1·medium 6·low 1):** 가장 큰 것은 GHSA-m8fp-3f3g-j5p7(8.4, `>= 1.63.2, < 1.103.0`)이다. 요청에 넣은 `api_key`·`api_base`가 공유 라우터에 남았다. 원문: "Later requests for that model from any user were load-balanced across the original deployment and every deployment added this way." 공격자 서버로 프롬프트와 운영자 키가 나가려면 `allow_client_side_credentials` 같은 설정이 켜져 있어야 한다.
  - **함정(로그):** GHSA-mpqq-3cc5-fgh2는 인가 실패 때 가상 키를 평문으로 로그에 남겼다. 원문: "Upgrading does not remove keys that were already logged." 권고의 SQL로 찾아 폐기하고, Langfuse·Datadog 같은 외부 로그도 지운다.
  - **함정(버전 칸):** 같은 권고의 범위 `>1.83.14, < 1.83.3`은 성립하지 않는다. `/spend/tags` 등 두 건은 영향 범위 `<= 1.108.0`과 수정 버전 1.108.0이 겹친다. `>= 1.86.0` 줄에는 상한이 없어 스캐너가 최신판까지 취약으로 볼 수 있다. 안정판 1.110.2는 여덟 건의 수정 버전보다 모두 높다.
  - KEV의 LiteLLM 건(CVE-2026-40217, 기한 12-02)과는 별개다.
- **crewAI(16:59 UTC, high 7.6, `< 1.15.29`):** 기본 LanceDB 메모리의 SQL 필터 삽입이다. 다른 테넌트의 메모리를 읽거나 지울 수 있다. 최신판은 이미 고쳐졌다.

[LMCache 권고](https://github.com/LMCache/LMCache/security/advisories/GHSA-hhqh-8jpj-27rf) · [LMCache LoRA](https://github.com/LMCache/LMCache/security/advisories/GHSA-jg4p-wprm-8hph) · [LiteLLM m8fp](https://github.com/BerriAI/litellm/security/advisories/GHSA-m8fp-3f3g-j5p7) · [LiteLLM mpqq](https://github.com/BerriAI/litellm/security/advisories/GHSA-mpqq-3cc5-fgh2) · [LiteLLM 권고 목록](https://github.com/BerriAI/litellm/security/advisories) · [crewAI 권고](https://github.com/crewAIInc/crewAI/security/advisories/GHSA-r7cq-p8qg-7h3w)

### 5. (직전 판 1번 후속) 추론 공급망: BleepingComputer가 퇴출 3곳의 이름을 적었지만 OpenRouter 공개 API와 맞지 않는다 (매체보도 20:00 UTC + 직접 확인)

- **보도 원문:** "leading to the decision to remove the three leaking inference providers: Together AI, Phala, and Novita". SecurityWeek(12:16 UTC)는 반대로 원문 "It did not say whether these are among the three it removed."라고 썼다. OpenRouter 글에는 이름이 없다.
- **직접 확인(모델 476개의 엔드포인트 전수 조회, 23시대 UTC):** Together 29개, Novita 34개가 살아 있다. Phala는 0개, Atlas Cloud는 1개다. Together와 Novita가 통째로 퇴출됐다는 보도는 지금 상태와 맞지 않는다. 엔드포인트가 0개라고 해서 퇴출됐다고 볼 수도 없다. 세 곳의 이름은 여전히 확정하지 않는다.
- **새로 나온 사실(SecurityWeek, Dragos 인용):** 원문: "one adversary appears to have hacked two of the providers at the same time, using secrets stolen from both from a single rented server". HD Moore 원문: "Some of the stolen keys came from requests that the provider rejected before a model ever ran". 거절된 요청까지 복사됐다면 모델 서버가 아니라 앞단에서 샌 것이다.
- **Atlas Cloud 공지 갱신(메타 00:47 UTC):** 원문: "If you used MiniMax M3, M2.7 or M3.1 Flash on Atlas Cloud, we can't tell whether data from your requests was misused, so treat any secret you sent to these models as exposed."
- **변화 없음:** MiniMax 공식 입장, Together의 자격 증명 폐기 여부, Novita·Phala 공지, Dragos·Prime Intellect 새 글. 감사받은 ZDR을 내건 Vercel의 움직임은 도구 5번에 적었다.

[BleepingComputer](https://www.bleepingcomputer.com/news/security/openrouter-drops-ai-providers-over-leaked-prompts-and-secrets/) · [SecurityWeek](https://www.securityweek.com/ai-inference-providers-failed-to-detect-hacking-and-prompt-theft-says-dragos/) · [OpenRouter](https://openrouter.ai/blog/announcements/data-processing-incidents-on-inference-providers/) · [Atlas Cloud](https://www.atlascloud.ai/blog/ai-updates/security-and-privacy-commitment)

### 6. Azure API Management Classic 결함으로 저장된 비밀이 샜다. Microsoft 조치만으로는 끝나지 않는다 (매체보도 19:42 UTC, Microsoft 고객 권고 MZK9-GH0 인용)

- **무엇:** Microsoft가 11-18에 알고 11-19에 조치했다. 노출됐을 수 있는 것은 Named Values의 비밀, ID 제공자·OpenID Connect 클라이언트 비밀, 구독 키, 직접 관리 API 토큰이다. v2 티어는 영향이 없다. 노르웨이 Equinor와 Skagerak Energi가 표적이었다.
- **함정(원문):** "it does not automatically invalidate credentials that may have been accessed before the fix was deployed." 비밀 교체는 고객 몫이다. 보안 연구자의 정리에 따르면 예전 관리 API 호출은 원문 "its calls aren’t recorded in the ARM Activity log"다.
- **할 일:** Named Values, 구독 키, 백엔드 자격 증명을 교체한다. 포털에서 `Enable Management REST API`를 No로 둔다. LLM 게이트웨이를 APIM으로 짰다면 모델 제공자 API 키가 Named Values에 있는지 먼저 본다.

[BleepingComputer](https://www.bleepingcomputer.com/news/security/microsoft-azure-flaw-exploited-to-steal-customers-api-credentials/) · [The Register](https://www.theregister.com/security/2026/11/23/norway-links-energy-firm-cyberattacks-to-critical-azure-api-management-flaw/5311598) · [점검 방법](https://anderskristiansen.com/post/2026/apim-legacy-access/)

### 7. Wiz: 실전 에이전트 공격이 3주 사이 크게 늘었다. 코딩 도구의 세션 기록도 표적이다 (벤더 연구, 14:46 UTC)

- **원문:** "an additional twenty related incidents have been disclosed by vendors." 원문: "Wiz’s CIRT has seen a 3x increase in engagement requests". ARTEX 계열 도구 하나는 원문 "at varying confidence, to activity at more than a dozen organizations"로 이어진다.
- **표적이 된 AI 도구(원문):** "Anthropic API keys were harvested from 50 hosts, and employees' AI coding-assistant session histories were mined for production SSO credentials."
- **함정(원문):** "should not be read as a single campaign or attributed to one threat actor". Wiz는 탐지 제품을 파는 회사다.
- **할 일:** 코딩 에이전트의 세션 기록 폴더(`~/.claude`, `~/.codex` 등)에 붙여 넣은 토큰이 남아 있는지 보고, 보존 기간을 줄인다.

[Wiz](https://www.wiz.io/blog/agentic-attacks-artex-offensive-ai-frameworks)

### 8. (직전 판 3번 후속) oss-security가 봇 knell을 차단했다. AI 보고 일반 정책은 아직 없다 (메일링 리스트, 00:45 UTC)

- 관리자 Solar Designer 원문: "OK, I've just blocked knell." 원문: "We got plenty of complaints." 다만 원문 "This may not be the case with future bots, so it's not a general solution."라고 했다. aiohttp 메인테이너(15:04 UTC)는 봇이 아닐 수도 있다고 했다. 원문: "So, supposedly not even a bot, just an immature individual that refuses to follow security reporting practices".

[차단 글](https://seclists.org/oss-sec/2026/q4/822) · [aiohttp 메인테이너](https://seclists.org/oss-sec/2026/q4/838)

### 9. 한전 152만 건 유출: 영문 기사의 "AI 때문에 뚫렸다"는 오역에 가깝다 (매체보도, 개인정보위 조사 착수 07:09 UTC)

- **사실:** 한전ON 65만 487명과 전자조달시스템(SRM) 86만 9,761명의 정보가 유출됐다(연합뉴스 11-22, 창 경계). 개인정보위가 조사에 들어갔다(창 안).
- **함정:** BleepingComputer(20:54 UTC)는 유출이 원문 "advanced technologies such as AI"에서 비롯됐다는 식으로 옮겼다. 한전 입장문의 원문은 "최근 인공지능(AI) 등 첨단기술을 악용한 사이버 공격 수법이 지능화·고도화되는 등 보안 환경이 빠르게 변화하는 가운데"로 시작하는 배경 설명이다. 원인은 조사 중이다. AI가 원인이라고 인용하지 않는다.

[연합뉴스(유출)](https://www.yna.co.kr/view/AKR20261122042851003) · [연합뉴스(조사)](https://www.yna.co.kr/view/AKR20261123137200530) · [BleepingComputer](https://www.bleepingcomputer.com/news/security/south-koreas-power-giant-kepco-confirms-data-breach-impacting-15-million/)

### 10. 논문·연구 (HF Daily Papers 11-23분 23편, arXiv 11-23 공지)

arXiv 공지는 창 안이지만 제출일은 11-18~20이다. 수치는 초록에서 확인했고 재현하지 않았다.

- **cmHC(HF 88, 1위):** 여러 잔차 스트림을 섞는 하이퍼커넥션에서 덜 섞임과 과하게 섞임을 함께 다룬다. 원문: "reducing hyper-connection execution latency by up to 44.8%"(Ascend NPU). [arXiv](https://arxiv.org/abs/2611.27193)
- **Clover-35B-A3B-Preview(HF 36):** 350억(활성 30억) 확산 언어 모델이다. 원문: "73.6 on SWE-bench Verified and 51.7 on Terminal-Bench 2.1". GB300 한 장에서 초당 680 토큰은 내부 측정이다. 코드와 가중치를 함께 공개했다. [arXiv](https://arxiv.org/abs/2611.24636)
- **Intent Contracts(HF 4):** 계획을 세우는 모델에게서 도구를 빼고 계약서만 쓰게 하는 설계다. 부정적 결과를 숨기지 않았다. 원문: "Contracts do not reduce unauthorized actions." 대신 사용자가 시킨 일을 끝내는 비율은 27/30으로, 검토 방식의 16/30보다 높다. [arXiv](https://arxiv.org/abs/2611.24025)
- **MemOnce(HF 17):** 1M 컨텍스트용 메모리 구조다. [arXiv](https://arxiv.org/abs/2611.27973)
- **Google DeepMind(17:00 UTC):** Gemini 4 에이전트와 Lean으로 양자 회로 계산 비용을 최대 11.8배 줄였다는 글이다. 논문은 11-06에 나왔다. [DeepMind](https://deepmind.google/blog/finding-faster-quantum-algorithms-with-agents-and-formal-verification/)

### 11. 짧게

- **AI 관련 미검토 권고(모두 NVD 미러, 패키지 매핑이 비어 Dependabot이 울리지 않는다):**
  - Pomerium CVE-2026-135300(critical, `0.30.0~0.34.0 미만`): 원문: "authentication bypass vulnerability in the MCP token endpoint". 클라이언트 비밀 검사를 건너뛴다. [GHSA](https://github.com/advisories/GHSA-2hj6-2qpj-pqj2)
  - DBHub CVE-2026-134803(high): MCP 읽기 전용 모드 우회. 1.7.0에서 고쳤다. [GHSA](https://github.com/advisories/GHSA-p6h8-6r97-q9rf)
  - mcp-server-kubernetes CVE-2026-134817(high): kubectl 플래그 삽입. 4.2.0에서 고쳤다. [GHSA](https://github.com/advisories/GHSA-8v48-c263-r637)
  - Gradio CVE-2026-134825(high, 6.27.0~6.32.0 미만): `ssr_mode`에서 위조 Host 헤더로 운영자의 HF 토큰을 쓸 수 있다. [GHSA](https://github.com/advisories/GHSA-4cx6-qfwr-9vfq)
- **nginx 1.31.10·1.30.8:** TLS 1.3 `ssl_early_data`의 CPU 과다 소비 CVE-2026-132779(medium, 1.21.4~1.31.9). [nginx](https://nginx.org/en/security_advisories.html)
- **CPython CVE-2026-107784(medium):** Windows에서 `tarfile` 추출 필터가 드라이브 상대 경로(`C:foo`)를 통과시킨다. [oss-security](https://seclists.org/oss-sec/2026/q4/839)
- **정정(Ubuntu PHP):** 11-21 판은 jammy에 CVE-2026-115560 "공백"이 있다고 썼다. Ubuntu 추적기(11-23 14:30 UTC 갱신)는 jammy를 `not-affected`로 표시했다. 원문: "session upload progress hard-disabled". noble과 resolute는 수정판이 나온 상태(`released`)다. RHEL 9는 php:8.3 수정(RHSA-2026:102226, 09:16 UTC)이 나왔고, php·php:8.2·php:8.4는 Affected로 남아 있다. [Ubuntu](https://ubuntu.com/security/CVE-2026-115560) · [RHSA-2026:102226](https://access.redhat.com/errata/RHSA-2026:102226)
- **직전 판 후속 중 진전 없음:** Ruby2ßhell(Veria 표 그대로, PoC 예고 11-25, RHEL·Ubuntu 미수정, 악용 보고 없음), Ghostscript CVE-2026-125396(Ubuntu `needs-triage`, RHEL Affected), GPGME(Ubuntu에 등록됐으나 `needs-triage`), OpenSSL 4.0.4·4.1.1, KVM CVE-2026-99312, GL.iNet·Cleverbridge(재보도), FBI NCIC, Entra Connect Health, NebuSec Meta Muse, Java4Shell, Microsoft agent-framework·ONNX Runtime(404), AutoGPT 0.12.0 권고, Kimi Code 수정판, browser-use CVE. Node.js·Go·Ruby·Rails는 창 안 보안 릴리스가 없다.

### 참고: 창 경계 항목

- **(논문 11-14, The Register 창 안 00:23 UTC) GPT-5.1로 만리장성 우회 방법 찾기:** 논문 원문은 읽지 못했다. [The Register](https://www.theregister.com/security/2026/11/23/1-of-gpt-51-tokens-finds-new-ways-around-chinas-great-firewall/5311532)
- **(GeekWire 11-21, The Register 창 안 16:16 UTC) Microsoft 대형 장애 최종 보고서:** 게이트웨이 용량 일부가 이미 빠진 상태였고 부하 시험이 옛 하드웨어로 이뤄졌다는 내용이다. Microsoft 원문은 찾지 못했다. [The Register](https://www.theregister.com/saas/2026/11/23/microsoft-mega-outage-report-reveals-testing-gaps-and-emails-that-never-arrived/5311656)

## 써볼 만한 도구

가장 큰 소식은 Anthropic의 Claude Ship이다. PR 리뷰·수정·승인까지 Claude가 맡는 제품이라 켜기 전에 읽어야 할 함정이 많다. Claude Code는 2.1.331이 나왔고, FastMCP와 OpenCode는 기본값을 바꿨다. Inkling 2 Mini 로컬 실행은 직전 판의 열린 문제 둘에 수정이 들어갔다. 도구는 하나도 설치하거나 실행하지 않았다. 함정과 인용은 공식 문서, 릴리스 본문, PR·이슈 본문, README에서 직접 읽었다.

### 1. (새 제품) Claude Ship 베타: Claude가 PR을 리뷰하고, 고치고, 정책이 허락하면 승인한다 (공식, 날짜 11-23, The Verge 20:00 UTC·TechCrunch 22:05 UTC)

- **무엇:** 세 가지로 이뤄진다. Code Review는 이미 정식 출시된 것이고, PR Steward(CI 실패·충돌·리뷰 지적을 고쳐 푸시)와 Claude Approvals(`.claude/approvals.yml` 정책으로 승인)가 베타다. 원문: "Claude can now review pull requests, fix them, and approve the ones that a team's policy allows". Claude Code에는 `/ship`과 `/steward` 명령이 생겼다.
- **승인과 병합은 누가 하나:** Steward는 원문 "is designed not to approve or merge pull requests"다. 승인은 Approvals만 한다. 다만 GitHub 브랜치 규칙이 승인 1개를 요구하면 원문 "Claude's approving review can be that approval"이다. 병합은 GitHub 규칙이 정한다.
- **요금:** 원문: "available on Claude Team and Enterprise plans for repositories on GitHub". 토큰 과금이고, 관찰 모드도 원문 "triggers a billed Code Review, even in observe mode"다. ZDR·HIPAA·고객 관리 키 조직은 못 쓴다. 조건에 맞는 Enterprise는 "at least $10,000 in Claude Ship credits"를 받는다.
- **조직이 조심할 것(공식 문서 원문):**
  - 사람 없이 병합될 수 있다. "If you use GitHub auto-merge or a merge queue with a policy that doesn't require a person's approval, a change can merge without anyone acting."
  - 리뷰 없이 승인될 수 있다. "a rule with `claude: approves` starts no Code Review by itself, so Claude can approve a change that nothing has reviewed".
  - 켜는 권한이 넓다. "anyone who can merge a policy file into the default branch can turn Approvals on".
  - 프롬프트 주입을 문서가 인정한다. PR 작성자의 글이 "can steer a classifier's answer"다.
  - 설정 파일은 기본 보호 대상이 아니다. "Don't rely on other files being protected." `.github/`, `CLAUDE.md`, `.mcp.json`, `.claude/`는 규칙을 따로 쓴다.
  - 정책 파일을 넣으면 기존 자동 리뷰가 멈출 수 있다. "automatic reviews normally stop where no matching rule names `claude/code_review`".
  - 비용 한도가 없다. "PR Steward has no spend limit of its own until you set one."
- **`/ship`을 쓰는 개인이 조심할 것:** "`/ship` proceeds with no confirmation prompt." 커밋하지 않은 파일까지 커밋·푸시하고 PR을 연다. 비밀값은 파일 이름으로만 거른다. Read deny 규칙도 "doesn't keep a file out of the commit"이다. 커밋·푸시에는 ask 규칙과 PreToolUse 훅이 적용되지 않는다(deny 규칙은 적용된다). 워치 모드 경고 원문: "Any of them can run programs on your computer with your credentials." 네이티브 Windows와 `bypassPermissions`에서는 워치가 뜨지 않는다.
- **추천 이유와 시작법:** 관찰 모드와 Steward "Manual"로 시작한다. 지출 한도를 먼저 걸고, 작성자가 아닌 사람 1명을 요구하는 규칙(`from: [humans, "!pr/authors"]`)을 넣는다. Claude GitHub App을 ruleset bypass 목록에서 뺀다. 쓰지 않을 조직은 managed settings의 `skillOverrides`에서 `ship-local`과 `steward`를 끈다.

[공식 글](https://claude.com/resources/articles/introducing-claude-ship) · [Claude Ship 문서](https://code.claude.com/docs/en/claude-ship) · [Approvals](https://code.claude.com/docs/en/approvals) · [정책 파일](https://code.claude.com/docs/en/approvals-policy) · [GitHub 규칙과의 관계](https://code.claude.com/docs/en/approvals-github-rules) · [Steward 보안](https://code.claude.com/docs/en/pr-steward-security) · [/ship](https://code.claude.com/docs/en/ship-command) · [TechCrunch](https://techcrunch.com/2026/11/23/anthropic-launches-claude-ship-letting-ai-agents-review-fix-and-approve-code-changes/) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1040134/anthropic-claude-ship-ai-pull-requests)

### 2. Claude Code 2.1.331: 관리형 플러그인 마켓플레이스 자동 갱신을 예고했고, 확인 창 순서가 바뀌었다 (공식, npm 18:36 UTC·릴리스 21:34 UTC)

- **예고(원문):** "on or after November 30, 2026: plugin marketplaces in managed `extraKnownMarketplaces` will auto-update by default. To opt one out, set `"autoUpdate": false` on its entry". 플러그인을 고정해 쓰는 조직은 다음 주 월요일 전에 끈다.
- **손버릇 주의(원문):** "they still start on No, so Down then Enter now answers No". `/clear`·`/compact`·`/rewind` 확인 창(서브에이전트 화면)과 `/ide` 질문에 해당한다.
- **Windows 관리 설정(원문):** "check them before upgrading, as an `HKLM` policy outranks `managed-settings.json` and a broken value can stop the start".
- **고친 것(원문):** "Fixed /compact and auto-compact sometimes losing part of the conversation summary when the summary quoted certain XML tags". `@file#10-12`가 파일 전체를 붙이던 문제와, 잘못된 MCP 응답에 최대 30분 멈추던 문제도 고쳤다.
- **추적:** dist-tags는 `latest`·`next` 2.1.331, `stable` 2.1.325다. #110979·#110642·#111167·#111218·#111068은 모두 열려 있다. #111167 제보자는 [댓글](https://github.com/anthropics/claude-code/issues/111167)을 고쳐(원문 "That was wrong") 재현 범위를 bypassPermissions로 좁혔다.
- **함께 나온 판:** Agent SDK TS 0.3.331·Python 0.2.188, claude-code-action 1.0.287. 액션은 취소된 실행에서 원문 "the session kept running and the "Revoke app token" step was skipped"이던 문제를 고쳤다(PR #2112). 컨테이너 작업과 Windows는 해당되지 않는다.

[2.1.331](https://github.com/anthropics/claude-code/releases/tag/v2.1.331) · [claude-code-action PR #2112](https://github.com/anthropics/claude-code-action/pull/2112)

### 3. (기본값 변경) FastMCP 4.2.5: 열린 스트림이 기본이 됐고 OAuth 검사가 조여졌다 (공식, 07:28 UTC)

- **원문:** "**Listen streams are on by default.**" 원문: "Each server process accepts up to 1024 open listen streams, and refuses further listen requests with an error." 서버리스, 연결 과금, 워커가 적은 서버가 영향을 받는다. 끄려면 `FASTMCP_ENABLE_SUBSCRIPTIONS_LISTEN=false`를 둔다.
- **보안 수정:** PR #5829 원문: "CIMD clients could bypass the OAuth proxy's scope policy". PR #6000은 redirect URI의 쿼리를 보지 않던 문제다. 저장소 보안 권고는 없다.
- **함정(원문):** 멀티테넌트 `AzureProvider`는 "no longer checks the token issuer, so users from any tenant are accepted; pass `token_issuer` to pin one". `valid_scopes`와 `required_scopes`를 둘 다 안 쓰면 "CIMD clients can no longer request any scope". 교체된 리프레시 토큰을 60초 뒤에 다시 쓰면 권한 전체가 끊긴다.

[4.2.5](https://github.com/PrefectHQ/fastmcp/releases/tag/v4.2.5) · [PR #5829](https://github.com/PrefectHQ/fastmcp/pull/5829) · [PR #6000](https://github.com/PrefectHQ/fastmcp/pull/6000)

### 4. OpenCode 2.4.0: 로그인된 브라우저를 에이전트와 함께 쓰고, 자동화가 기본으로 켜진다 (공식, 09:22 UTC · 2.4.1 15:57 UTC)

- **내장 브라우저(원문):** "Import your sign-ins and browsing history from Chrome, Edge, Firefox or Safari". 에이전트는 원문(PR #63002) "reads and drives the same signed-in tabs you use"다. 막는 법(릴리스 원문): "A `browser` deny in your permission rules refuses every agent browser call."
- **자동화:** PR #65296 원문: "It defaults to on in every build". 타이머·cron·MCP 이벤트로 프롬프트와 셸 명령을 돌린다. 셸 작업은 만들 때 한 번만 승인을 묻는다. 끄려면 `"automations": false`를 둔다.
- **플러그인:** `@latest`나 버전 범위로 적은 플러그인은 시작할 때와 24시간마다 자동 설치된다. 정확한 버전으로 고정한다.
- **함정:** 원문: "GitHub Copilot no longer reads `GITHUB_TOKEN`." `COPILOT_GITHUB_TOKEN`으로 바꾼다. macOS 12에서는 2.4.1을 쓴다.

[2.4.0](https://github.com/anomalyco/opencode/releases/tag/v2.4.0) · [PR #65296](https://github.com/anomalyco/opencode/pull/65296) · [PR #63002](https://github.com/anomalyco/opencode/pull/63002)

### 5. (추론 공급망 대응) Vercel AI Gateway: 감사를 통과한 곳만 "Audited ZDR" 표시, 카나리 감사, 본사 국가 제한 (공식, 11-23)

- **무엇:** 원문: "A provider is labeled Audited ZDR only after an independent security firm tests its zero data retention claims against its code, infrastructure, and live traffic, then publishes the report." 카나리 감사는 프롬프트에 고유 URL을 심어 누가 열어 보는지 감시한다. 국가 제한은 원문 "Providers without a headquarters country on record never receive your requests."다.
- **함정:** 통과한 곳이 아직 없다. Baseten·Modal·Wafer가 감사를 시작했고 원문 "Launch partner reports are expected to be published by December 8, 2026."이다. 카나리의 한계도 적었다. 원문: "Canary checks cannot detect prompts written to disk and never read".
- **추천 이유:** 지난 며칠의 프롬프트 유출은 계약상 ZDR만으로는 막지 못했다. 라우터를 고를 때 볼 기준이 하나 생겼다.

[Audited ZDR](https://vercel.com/blog/introducing-audited-zdr-on-ai-gateway) · [카나리 감사](https://vercel.com/changelog/ai-gateway-now-runs-canary-zdr-audits) · [본사 국가 제한](https://vercel.com/changelog/choose-ai-gateway-providers-by-headquarters-country)

### 6. (후속, 직전 판 도구 2번) Inkling 2 Mini 로컬 실행: CPU 분산 손상과 토크나이저에 수정이 들어갔다. Unsloth 루트 GGUF가 메인라인용으로 바뀌었다 (공식 릴리스·저장소 + 커뮤니티 이슈)

- **llama.cpp(창 안 빌드 14개, b12635~b12651):**
  - b12641(01:34 UTC)에 토크나이저 수정 #32799가 들어갔다.
  - b12642(01:47 UTC)에 전문가 CPU 분산 손상 수정 #32807이 들어갔다. 릴리스 원문: "a partial expert copy made for the first MUL_MAT_ID is stale for the others (e.g. the shared layers of inkling2_loop with --n-cpu-moe)". 직전 판의 우회책 `--no-op-offload`는 이 빌드부터 필요 없을 것으로 보이지만, #32778·#32710은 아직 닫히지 않았다.
  - b12651(08:52 UTC) 뒤로 빌드가 없다. Metal의 NVFP4 지원(#32825, 19:02 UTC 병합)은 조회 시점에 소스 빌드로만 쓸 수 있다.
- **정정(Unsloth):** 직전 판은 루트 GGUF가 메인라인 llama.cpp에서 읽히지 않는다고 썼다. 14:54 UTC에 파일이 교체됐다. 루트 GGUF 10개의 해시가 `Mainline_Support`의 파일과 같다(직접 확인). 옛 파일은 `Old_Support`로 옮겼다. 그런데 [`Mainline_Support/README.md`](https://huggingface.co/unsloth/Inkling-2-Mini-GGUF/blob/main/Mainline_Support/README.md)에는 옛 문장 "The root files use Unsloth's looped layout"이 남아 있어 헷갈린다. 먼저 받은 사람은 다시 받는다.
- **"1.9배"는 이렇게 읽는다:** Unsloth v0.1.923-beta 제목은 "up to 1.9x faster"다. 공식 가이드 표에서 새 빌드는 메인라인보다 1.20~1.63배 빠르다. 1.87배는 AMD Windows의 이전 빌드 값이고, 그 줄의 새 빌드는 "not measured"다. 자체 측정이다.
- **열린 문제:** 생각 예산 재시작(#32836), 4GB GPU에서 `-ngl 0`이어도 7GB대 버퍼를 요구하는 문제(#32910, 21:33 UTC). SGLang PR #51926은 템플릿 문제로 보류 중이다. 원문: "we found some issues with templates in inkling 2 mini varloop".

[b12642](https://github.com/ggml-org/llama.cpp/releases/tag/b12642) · [#32799](https://github.com/ggml-org/llama.cpp/pull/32799) · [#32825](https://github.com/ggml-org/llama.cpp/pull/32825) · [#32910](https://github.com/ggml-org/llama.cpp/issues/32910) · [Unsloth GGUF](https://huggingface.co/unsloth/Inkling-2-Mini-GGUF) · [Unsloth 가이드](https://unsloth.ai/docs/models/inkling-2-mini) · [v0.1.923-beta](https://github.com/unslothai/unsloth/releases/tag/v0.1.923-beta) · [SGLang #51926](https://github.com/sgl-project/sglang/pull/51926)

### 7. (지켜볼 도구) Shirudo: OpenAI와 Cisco가 패치 전까지 쓰는 커널 방패 런타임을 오픈소스로 냈다. 아직 시험판이다 (공식, 21:20 UTC)

- **무엇:** 패치를 배포하는 동안 실행 중인 Linux에 서명된 임시 방어를 거는 도구다. BSD-2 라이선스다. OpenAI 글 원문: "a Codex-generated shield blocked a Java4Shell exploit attempt against a running Elasticsearch service without requiring a restart".
- **함정(FAQ 원문):** "Not yet. shirudo is pre-release and has no stable releases". 원문: "Every target needs a TPM 2.0". 전용 커널 패치가 필요하고, 원문 "shirudo admits only BPF loaded by signed loaders"라 Cilium·Falco는 그대로 못 돈다.
- **시연의 범위:** Java4Shell 방패는 eBPF가 아니다. Isovalent 원문: "This resulted in a mitigation written as Java bytecode". 문서의 Java 에이전트 경로는 원문 "One already running keeps the agents it started with until it restarts."라서, "재시작 없이"를 모든 경우로 읽으면 안 된다.

[OpenAI](https://openai.com/index/introducing-shirudo/) · [Isovalent](https://isovalent.com/blog/post/cisco-openai-shirudo/) · [저장소](https://github.com/shirudo-dev/shirudo) · [FAQ](https://docs.shirudo.dev/start/faq.html) · [Java 에이전트 문서](https://docs.shirudo.dev/upatches/write/java-agents.html)

### 짧게

- **LiteLLM 1.108.16 LTS(22:12 UTC):** 패치 판인데 깨지는 변경이 있다. 게이트웨이 DCR을 쓰면 업그레이드 전에 `LITELLM_MCP_DCR_GATEWAY_CLIENT=true`가 필요하다(#53180). `extra_body`로 `model`·`messages`를 바꾸면 원문 "now gets a 400 unless `drop_params` is on"이다(#53411). Azure `/openai/v1`은 `OPENAI_API_KEY`를 더 읽지 않는다(#53409). 안정판 1.110.2는 그대로라 키 전송 수정은 여전히 LTS와 개발판에만 있다. 보안 권고 8건은 기술 이슈 4번에 정리했다. [릴리스](https://github.com/BerriAI/litellm/releases/tag/v1.108.16) · [#53180](https://github.com/BerriAI/litellm/pull/53180) · [#53411](https://github.com/BerriAI/litellm/pull/53411)
- **MCP 레지스트리 v1.8.16(13:49 UTC):** 관리자가 항목에 보류를 걸 수 있게 됐다. 원문(PR #1910): "Existing versions start without a hold." 사라진 패키지를 가리키는 649건의 처리와 목록 공개는 진전이 없다. [PR #1910](https://github.com/modelcontextprotocol/registry/pull/1910)
- **GLM-5.4 실행 지원:** 가중치가 없어 vLLM·SGLang·llama.cpp·transformers에 전용 PR이 아직 없다.
- **Ling-3.2-air 실행:** SGLang dev 이미지와 `--trust-remote-code`가 필요하다. 141GB급 GPU 4장이나 80GB급 8장이다.
- **Strands Python 1.66.0(17:42 UTC):** GPT-6.2 시스템 프롬프트 캐시 문제를 고쳤다(#6396). 상태 저장 모드는 빠졌다. 원문: "so its system prompt stays uncached on GPT-6.2". [릴리스](https://github.com/strands-agents/sdk-python/releases/tag/python%2Fv1.66.0) · [#6396](https://github.com/strands-agents/sdk-python/pull/6396)
- **AWS WAF AI 엔드포인트 탐지(18:47 UTC):** LLM·MCP·A2A 엔드포인트를 찾는다. 문서화되지 않은 MCP 서버는 원문 "appears as a shadow API"다. 추가 요금이 든다. [AWS](https://aws.amazon.com/about-aws/whats-new/2026/11/waf-ai-endpoint-discovery-protection/)
- **OCRia-nano-1(LlamaIndex, Show HN 17:11 UTC):** 8.1M 파라미터 CPU OCR이다. 함정은 라이선스다. 상업 이용은 원문 "organizations under $2M in annual revenue"로 제한된다. [HF](https://huggingface.co/llamaindex/OCRia-nano-1)
- **코딩 에이전트 샌드박스 비교(Kudelski, 11-23):** nono, Docker Sandboxes, Microsandbox, OpenShell을 비교했다. 원문: "No tool can deny a file inside an allowed directory". [Kudelski](https://kudelskisecurity.com/research/sandboxing-coding-agents)
- **(창 경계, 공개 11-20) NariQ Qwen3.8-27B 10GB(HN 100점대):** 전용 llama.cpp 포크가 필요하다. 원문: "Every point was measured by us with the same code". 모델 카드와 포크 README의 GeForce 시험 여부가 서로 다르다. [HF](https://huggingface.co/nari-labs/Qwen3.8-27B-NariQ-GGUF)
- **Ladybird 알파(HN 1,200점대, 16:13 UTC):** 새 엔진 브라우저다. WebDriver를 지원해 사이트 시험용으로는 쓸 만하다. 원문: "we make no promises about security today". [Ladybird](https://ladybird.org/posts/alpha/)
- **주의: 급조 저장소.** `nowak2443/windsurf-pro-cursor-trial-reset-unlocker`(18:08 UTC 생성)는 암호 걸린 107MB zip에 exe를 담았고 몇 시간 만에 별 230개대가 붙었다(조회 시점). 악성 여부는 확인하지 않았다. 내려받지 않는다. 직전 판의 FREELLM_INSTALL도 아직 남아 있다.
- **직전 판 후속 중 새 진전 없음:** Codex CLI(안정판 0.171.0), Copilot CLI·Gemini CLI·Crush(프리릴리스·nightly만), Pi 1.6.0, transformers 5.24.0, vLLM 0.34.0, SGLang, LMCache 0.5.8(보안 권고는 기술 이슈 4번), Ollama v0.40.17(#19817 그대로), Cline(yolo 되돌림 없음), goose(#13663 릴리스 없음), pydantic-ai #14095(열림), Mistral Vibe 3.1.4, VS Code 1.147.0, Qwen Code 0.28.4, browser-use 1.0.8, Google ADK, crewAI, Dify, Open WebUI, Mastra, Context7, GitHub MCP Server, Playwright MCP, MCP 공식 SDK·Inspector·레퍼런스 서버, Cursor, Zed. Kimi Code는 2.7.3(16:30 UTC)이 나왔지만 Plan 모드 우회 수정은 없다. `anthropics/skills`·`openai/plugins`는 창 안 커밋이 없다.

## 주목할 점

- **"내부 모델이 푼 문제"가 출시 모델보다 먼저 나온다.** 직전 판의 OpenAI 내부 모델 지표에 이어, Anthropic은 내부 연구 모델의 VP ≠ VNP 증명을, MiniMax는 내부 모델의 암호 분석 결과를 공개했다. 기계 검증은 통과하지만 명제가 뜻하는 바를 사람이 확인하는 속도가 따라가지 못한다. 수학계가 Statement 파일을 어떻게 검토하는지가 다음 기준이 된다.
- **퍼플렉서티 공개가 OpenAI 밖으로 나왔다.** Microsoft가 같은 평가 세트로 값을 냈다. 토크나이저와 데이터가 다른 Anthropic·Google이 어떤 공통 척도(BPB 등)로 답할지 지켜본다.
- **승인 권한을 가진 에이전트의 시대다.** Claude Ship, OpenCode 자동화, 로그인된 브라우저를 함께 쓰는 에이전트가 같은 날 나왔다. 그만큼 브랜치 규칙, 지출 한도, 세션 기록 보존 기간을 먼저 정해야 한다. Wiz가 말한 "세션 기록 캐기"는 이미 일어나고 있다.
- **다가오는 일정:**
  - 이번 주: **Ruby2ßhell Rails PoC 공개 예고(11-25)**, ERNIE 5.5 Flash Preview 가중치 공개 약속(11-25 전), Qwen4-Max-Preview 종료(11-25), **MONAI Label·N-able KEV 조치 기한(11-26)**.
  - 다음 주: **Claude Sonnet 4.5 은퇴(11-30)**, Claude Code 관리형 마켓플레이스 자동 갱신 기본값(11-30 이후), Gemini 4 Carbon·Argon 도입가 종료(11-30), Huawei openPangu-2.0 종료(11-30), NVIDIA Huang 하원(12-01)·상원 은행위 청문회(12-03), Puppet Enterprise(12-01)·LiteLLM(12-02) KEV 기한, Backblaze 새 개인정보 처리방침(12-01).
  - 12월: Vercel Audited ZDR 첫 보고서 예정(12-08까지), EU·EFTA·영국 Copilot 테넌트 Claude 기본 켜짐(12-09), Val Town 읽기 전용(12-10), GitHub Copilot 모델 6종 은퇴(12-14), **GPT-5.6 Sol·Terra ChatGPT·Codex 은퇴(12-23)**, Inception Mercury API 종료(12-28).
  - 그 뒤: BIS 원격 접근 규칙 의견 마감(2027-01-19), **Claude Opus 4.5 은퇴(2027-01-22)**, Windows 기본 소형 모델 Aion 탑재(2027년 1월 말), 다음 MCP 명세(03-30).

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - NYT·WSJ·Bloomberg·Reuters(유료·캡차), X 아티클(Alexandr Wang), Lean Zulip(인증), reddit, ACM 논문(403), Microsoft 장애 보고서 원문, Oregon·Virginia 소장은 열지 못했다. z.ai 블로그·OpenAI 글·THN·VP ≠ VNP 논문 PDF는 r.jina.ai로 텍스트를 뽑았다.
  - TechCrunch AI 카테고리 피드는 Cloudflare 확인 페이지만 돌려줘 전체 피드로 대신했다. CISA 공지 페이지는 막혀 KEV JSON으로 확인했다.
- **시각이 날짜만 있거나 불확실한 원문:** Claude Platform 릴리스 노트, ChatGPT 릴리스 노트와 시스템 카드, Claude Ship 공식 글, Vercel 글(예약 스탬프로 보임), Kudelski 글은 날짜만 있다. Ling-3.2-air 공개 시각은 ModelScope 등록 시각으로 잡았다. Atlas Cloud 공지는 현재 메타 시각만 있다.
- **확인하지 못한 것:**
  - VP ≠ VNP를 찾은 모델 이름, 외부 재검증, 복잡도 이론 전문가의 판정. GLM-5.4 승률 그림의 과제와 채점 방법. Amazon의 Decart 매입 금액.
  - OpenRouter가 퇴출한 세 곳의 이름, MiniMax 공식 입장, Together의 자격 증명 폐기 여부. Wayback이 응답하지 않아 OpenAI·OpenRouter·제공자 공지의 조용한 개정은 대조하지 못했다.
  - ChatGPT GPT-6.2 시스템 카드의 그림 전용 평가 수치. 급조 저장소의 악성 여부.
  - 재현하지 않은 것: 모든 증명 검증 결과와 벤치마크, 모든 취약점 주장. 도구와 패키지는 하나도 설치하거나 실행하지 않았다. OpenRouter 엔드포인트 수와 저장소 권고 개수, Unsloth 파일 해시는 공개 API로 직접 셌다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 80점 이상 스토리 27개와 20점 초과 스토리 51개의 제목을 모두 봤다. 그 아래는 키워드로 걸렀다.
  - 전역 GHSA는 reviewed 9건, unreviewed 370건, malware 117건을 창 범위로 받아 셌고, unreviewed는 AI 키워드로만 걸렀다. 저장소 권고는 AI·도구 저장소 139곳을 봤다. 도구 릴리스는 저장소당 최근 15개만 봤다.
  - HF 조직 스캔은 조직당 15개만 봤고, 401 추적 목록은 저장소별로 조회했다. LMArena, Mistral 변경 로그, AI Studio·Antigravity 변경 로그, Microsoft EU Copilot 문서, FBI `ic3.gov` PDF, patchtheplanet.org는 확인하지 못했거나 얕게 봤다.
  - 한국 랩은 HF 조직 스캔과 기사 제목 검색만 했다. Vertex AI 릴리스 노트(캐시 동결)와 Moonshot·Volcengine 변경 로그(월 단위)는 일 단위로 판정하지 못했다.
  - 창이 끝날 때 열려 있던 것: lake comparator v4.36.0 실행과 paranoid 커널 재검증, GLM-5.4 API·가중치, llama.cpp #32778·#32710·#32836·#32910과 b12651 이후 빌드, Claude Code #110979·#110642·#111167·#111218·#111068, MCP 레지스트리 649건, LiteLLM 안정판 키 전송 수정, Kimi Code Plan 모드 우회, OpenSSL 4.0.4·4.1.1, Ubuntu Ruby·Ghostscript 패치, AutoGPT 0.12.0 보안 권고, Microsoft 저장소 복구. 다음 판에서 다룬다.
