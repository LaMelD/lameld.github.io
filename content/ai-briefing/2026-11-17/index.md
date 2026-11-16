---
title: "2026-11-17 AI 브리핑"
date: 2026-11-17T01:00:00+09:00
tags: [ai-briefing, qwen, security, anthropic, nvidia]
description: "Alibaba가 가중치 없는 2.4T MoE Qwen4-Max를 정식 출시하고 Tencent가 출력 $0.28의 Hy4 flash를 냈으며, Google은 사람 없이 패치를 익스플로잇으로 바꾼 첫 공격을 보고했고 Anthropic·Project Zero는 취약점 공개 기한을 줄였다."
---

> 조사 범위: 2026-11-16 01:00 ~ 2026-11-17 01:00 KST(2026-11-15 16:00 ~ 11-16 16:00 UTC, 미국 일요일 08:00 ~ 월요일 08:00 PST). 직전 브리핑(11-16 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, SEC EDGAR `acceptanceDateTime`, GitHub `published_at`·`merged_at`, npm·PyPI·Maven 게시 시각, 저장소·전역 GHSA `published_at`, CISA KEV, 상태 페이지 API, Wayback 사본 시각, OpenRouter 등록 시각, Hugging Face API, oss-sec 메일 시각, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-16 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **Alibaba가 Qwen4-Max를 정식 출시했다.** 2.4T 파라미터 MoE이고 가중치는 공개하지 않았다. 출력 단가는 1M 토큰당 $4.5로 이전 세대보다 내렸다. 다만 외부 지수(Artificial Analysis)로는 43점으로, 오픈 웨이트 Kimi K3.1 Pro보다도 낮다. 같은 날 Tencent는 출력 $0.28의 저가 모델 Hy4 flash를 냈다. 미국 3사는 새 모델이 없다.
- **Google 위협 정보팀은 공격자가 사람 없이 "패치 비교 → 익스플로잇 작성 → 실행"을 자동화한 첫 사례를 보고했다.** 북한 추정 조직은 침투한 회사의 내부 저장소에서 `.claude/settings.json`을 고쳐, Claude Code를 켠 동료 개발자에게 백도어를 심었다. 같은 주에 Anthropic(2027년부터 90일 → 30일)과 Google Project Zero(2028년까지 30일)가 모두 취약점 공개 기한을 줄였다. AI 해킹 도구 DeepAudit은 인증 없이 API 키를 내보내는데 아직 고쳐지지 않았다.
- **도구는 기본값 변경에 주의할 날이다.** browser-use 1.0.6은 `llm`을 넘기지 않으면 토큰 단가가 15배인 BU 4.0을 기본으로 쓴다(PyPI 게시 대기). Anthropic 문서 스킬은 외부 Office 파일의 심볼릭 링크를 막도록 바뀌었다. Claude Fable 5.2 출시 프로모션은 연장 공지 없이 한국 시간 오늘(11-17) 16:59에 끝난다.

## 모델 소식

미국은 일요일, 아시아는 월요일 근무일이었다. 그래서 새 모델은 중국 랩 쪽에서 나왔다. Alibaba가 Qwen4-Max를 정식 출시했고, Tencent는 저가 모델 Hy4 flash를 냈다. 미국 3사는 새 모델, API 변경, 가격 변경이 없다. 변경 로그 최신값도 그대로다. OpenAI API는 11-05, Gemini API·AI Studio와 Claude Platform은 11-12다. OpenRouter 신규 등록은 위 두 모델 2건이다. Anthropic과 NVIDIA의 SEC 신규 접수도 창 안에 없다.

### 1. Alibaba가 Qwen4-Max를 정식 출시했다. 2.4T MoE에 가중치는 비공개이고, 값은 이전 세대보다 내렸다. 외부 지수로는 미국 프런티어 모델보다 한참 아래다 (공식, HN 11-16 02:14 UTC 750점대)

- **무엇이 바뀌었나:** 11-12 판에서 다룬 2주짜리 미리보기(`qwen4-max-preview`, Token Plan 전용)와 별개의 정식 모델 `qwen4-max`다. 종량제 API로 바로 쓸 수 있다. 구조 원문: "2.4T backbone parameters, 71B activated per token, and an additional 328B N-gram Embedding parameters". 텍스트·이미지·영상을 받고 컨텍스트는 1M이다. 블로그 자체에 시각이 없어 HN 첫 제출(02:14 UTC), OpenRouter 등록(02:18 UTC), Alibaba 뉴스룸 Alizila(03:56 UTC)로 시각을 잡았다.
- **가격:** 싱가포르 리전 기준 1M 토큰당 입력 $1.5, 출력 $4.5, 캐시 읽기 $0.075다. 원문: "Thinking and non-thinking modes use the same price, with no input-length tiers." 블로그 원문: "We have also lowered API prices compared with Qwen3.8-Max, with particularly large reductions in cached-input pricing." 비교하면 OpenRouter의 이전 모델 `qwen3.8-max-0902`는 입력 $2, 출력 $6이다.
- **자사 수치(재현 안 함):** 자사 벤치마크 Work360 원문: "In the latest evaluation, Qwen4-Max scored 52.69, up from 47.74 for Qwen3.8-Max". 원문: "In our Work360 evaluation, the average cost per task drops from $4.43 to $1.67".
- **외부 측정(Artificial Analysis):** 원문: "Qwen4 Max scores 43 on the Artificial Analysis Intelligence Index". 같은 페이지 내장 데이터의 비교값은 Kimi K3.1 Pro 46.5, GPT-6.1 Sol 54.6, Claude Opus 5.5 59.1, Gemini 4 Carbon 60.0, Claude Fable 5.2 64.6이다. 오픈 웨이트인 Kimi K3.1 Pro보다도 낮다. 말이 많은 편이다. 원문: "which is very verbose in comparison to the median of 78.6M".
- **가중치:** 없다. HF `Qwen/Qwen4-Max`는 401이고 AA는 "Proprietary"로 분류한다. 작은 모델은 공개를 예고했다. Alizila 원문: "Open-weight, smaller-scale models from the Qwen4 series will soon be released to developers worldwide."
- **함정(Qwen Cloud 문서):**
  - 사고가 기본으로 켜져 있다. 원문: "Thinking is enabled by default, with reasoning_effort=high and thinking_budget=131072." 출력 한도도 사고와 답을 합쳐 센다. 원문: "max_completion_tokens limits thinking and the final answer together". 출력 한도를 답변 길이 기준으로만 잡으면 답이 잘린다.
  - 다른 API 형식에서 보낸 `medium`은 `high`로 바뀐다(문서의 매핑 표). 비용을 낮추려고 `medium`을 줘도 소용없다. `low`를 쓴다.
  - Batch inference는 문서 표에 "Not supported"다. 원문: "DashScope does not support the new Qwen4 explicit cache." 원문: "Fine-tuning and model deployment are not supported."
  - OpenRouter에 DeepInfra·Novita가 제공자로 붙어 있지만, 단가가 Alibaba Global과 같아 가중치 공개의 근거로 볼 수 없다.
- **중요한 이유:** Qwen 4세대의 첫 정식 모델이다. 하지만 가중치가 없고 외부 지수로는 미국 3사 최신 모델과 격차가 크다. 개발자에게 실질적인 소식은 출력 $4.5라는 단가와 곧 나올 작은 오픈 모델이다. 블로그와 함께 장기 작업용 [Qwen-Live-Delivery-Plugin](https://github.com/QwenLM/Qwen-Live-Delivery-Plugin)(Apache-2.0)도 공개했다.

[Qwen 블로그](https://qwen.ai/blog?id=qwen4-max) · [Alizila](https://www.alizila.com/alibaba-unveils-qwen4-max-new-ai-teammate-for-work-creativity-and-discovery/) · [모델 문서](https://docs.qwencloud.com/developer-guides/getting-started/latest-model) · [가격](https://docs.qwencloud.com/developer-guides/getting-started/pricing) · [Artificial Analysis](https://artificialanalysis.ai/models/qwen4-max) · [HN](https://news.ycombinator.com/item?id=50513215)

### 2. Tencent Hy4 flash: 224B(활성 12B) 저가 모델. 출력 단가가 Hy4의 약 9분의 1이다. 긴 코딩 작업에는 Tencent도 Hy4를 권한다 (공식, 페이지 12:12 UTC, TechNode 06:54 UTC)

- **무엇:** 원문: "featuring 224B total parameters (including 64B Engram parameters) with just 12B active parameters per token". Hy4처럼 멀티모달이고 컨텍스트는 1M이다. 가격(1M 토큰당)은 캐시 입력 $0.003, 입력 $0.070, 출력 $0.281이다. Hy4는 $0.042 / $0.834 / $2.501이다. 효율 원문: "reducing KV cache size to just 1/24 of Hy3 and 1/6 of Hy4".
- **자사 수치(재현 안 함):** 원문: "On WorkIndex, its average cost per task is just 1/13 of Hy4 preview, 1/20 of Hy4, and 1/8 of DeepSeek-V4.1-Flash-1017". 내부 사용자 블라인드 평가 원문: "Hy4 flash achieved user satisfaction close to Claude Haiku 5.5".
- **함정:**
  - 긴 코딩은 약하다. 원문: "with only a slight dip on long-horizon coding workloads". 권장 용도도 원문: "continuing to recommend Hy4 for complex workloads like long-horizon software engineering"이다.
  - 할인은 기간 한정이다. 원문: "Through November 30, users can access it for free in WorkBuddy and get 50% off API pricing via Tencent Cloud TokenHub." OpenRouter 단가(조회 시점 입력 $0.035, 출력 $0.1405)도 공식 정가의 절반이라 할인가로 보인다. 12월 이후 단가를 다시 확인한다.
  - 가중치는 원문: "We also plan to release Hy4 flash as an open-weight model"이라고만 했다. 일정은 없다. 11-03에 낸 Hy4 본체 가중치(`tencent/Hy4`)도 여전히 401이다.
- **중요한 이유:** 1M 컨텍스트 멀티모달 모델의 출력 단가가 $0.28 수준까지 내려왔다. 문서 처리·검색 같은 일상 에이전트 작업의 단가 기준선이 바뀐다.

[Tencent Hy](https://hy.tencent.ai/research/hy4-flash?langVersion=en) · [TechNode](https://technode.com/2026/11/16/tencent-launches-hy4-flash-cutting-ai-task-costs-by-95-in-internal-tests/) · [OpenRouter](https://openrouter.ai/tencent/hy4-flash)

### 3. Anthropic이 자사가 신고하는 취약점의 공개 기한을 90일에서 30일로 줄인다. 2027-01-01 신고분부터다 (공식, 날짜만 11-16, `dateModified` 14:32 UTC)

- **무엇:** 원문: "We’re shortening the disclosure window in our coordinated vulnerability disclosure (CVD) policy from 90 days to 30 days for high- and critical-severity issues, and to 45 days for all other issues." 적용 시점은 원문: "The new timelines apply to vulnerabilities reported to vendors and maintainers on or after January 1, 2027."
- **근거:** 원문: "a bug that one model can find is increasingly likely to be found by another". 같은 주에 Google Project Zero도 기한을 단계적으로 30일까지 줄인다고 했다(기술 창 경계 A). 공격 쪽 자동화는 기술 1번에 정리했다.
- **함정:** 사람이 검토하지 않은 AI 발견은 대상이 아니다. 원문: "This new policy update will not apply to AI-discovered findings that have not received human review". 원문: "Our seven-day disclosure window for any vulnerability under active exploitation remains unchanged." 기존 미공개 건에는 원문: "we are providing them an additional 30 days"다.
- **중요한 이유:** Anthropic에게서 신고를 받는 오픈소스 유지보수자는 고칠 시간이 3분의 1로 준다. 프로젝트에 보안 연락처와 패치 절차를 미리 갖춰 두는 편이 낫다.

[Anthropic](https://www.anthropic.com/research/coordinated-vulnerability-disclosure-timelines)

### 4. Claude 장애: Sonnet 5.5·Fable 5.2 오류 73분, 창 종료 때 Claude Meetings 참가 장애 진행 중 (공식 상태 페이지)

- **Sonnet 5.5·Fable 5.2 오류(07:49 UTC 게시, 09:04 UTC 해소):** 원문: "The issue affecting claude.ai, the Claude API, Claude Code, and Claude Cowork has been resolved. Impact occurred from 23:32 PT on November 15 / 07:32 UTC on November 16 to 00:45 PT / 08:45 UTC on November 16." 사후 보고서는 아직 없다.
- **Claude Meetings(beta)(15:10 UTC, 창 종료 때 investigating):** 15:12 UTC 갱신 원문: "We have temporarily paused Auto-join while we work on this. Adding Claude to a call by hand is also affected." 이미 진행 중인 통화는 영향이 없다고 했다.
- **iOS Sign in with Apple(직전 판 후속):** 11-14 07:11 UTC부터 여전히 investigating이다. 마지막 갱신은 11-14 20:12 UTC다. 11-13 장애 2건의 사후 보고서도 아직 비어 있다.
- **중요한 이유:** 11-19 상장을 앞둔 주에 API와 Claude Code까지 걸린 장애가 났다.

[Sonnet·Fable 장애](https://stspg.io/n18qjkd7dt4q) · [Meetings 장애](https://stspg.io/01qq3kzyyqzl) · [iOS 장애](https://stspg.io/4858wf40dx89)

### 5. NVIDIA 후속: 상무부가 해외 데이터센터를 통한 중국의 원격 GPU 접근을 막는 규칙을 냈다. Huang 복귀 논의 보도와 외부 CEO 요구가 맞섰다 (공식 규칙 PDF + 매체보도, 창 안 8-K 없음)

- **원격 접근 수출 규제(BIS 임시 최종 규칙, TechCrunch 15:02 UTC):** 규칙 이름은 "Addressing Remote Access to Advanced Semiconductors"다. 본사가 중국 등(D:5 국가)이나 마카오에 있는 기업에 일정 규모 이상의 연산을 빌려주는 해외 데이터센터로 가는 수출에 허가를 요구한다. TechCrunch 원문: "Twelve million TPP roughly works out to the max computing power of about 750 Nvidia H100 or H200 chips". 원문: "Existing data center operators providing services that fall under this new rule are required to get licenses within 90 days of the rule coming into effect." 발효일은 연방관보 게재일이고 아직 비어 있다.
- **복귀 논의와 반대 압력:** The Verge(11-15 20:43 UTC) 원문: "The Financial Times reports that Jensen Huang has discussed the possibility of returning as CEO with close allies". Guardian(11-16 11:30 UTC)은 재무장관 발언을 전했다. 원문: "“They need a dramatic shake-up,” Bessent said on Fox News." 복귀 청원은 원문: "An open letter that has more than 165,000 signatories"로 직전 판 11만 5천 명에서 늘었다.
- **실적(11-17 미국 장 마감 후, 한국 시간 11-18 오전):** CNBC 원문: "Analysts expect Nvidia to report 91% revenue growth to $109.1 billion, according to LSEG". 주가는 원문: "falling about 2% on Monday"다.
- **중요한 이유:** 동남아 클라우드를 거쳐 중국 기업에 GPU를 빌려주던 경로가 막힌다. 중국 랩의 학습 용량과, 같은 데이터센터를 쓰는 다른 고객의 가격에 영향이 갈 수 있다.

[BIS 규칙 PDF](https://www.bis.gov/media/documents/rin-0694-ak50.pdf) · [TechCrunch](https://techcrunch.com/2026/11/16/us-tightens-ai-chip-export-controls-to-deter-chinese-companies-from-accessing-compute-remotely/) · [The Verge](https://www.theverge.com/tech/1033999/jensen-huang-return-nvidia-megaspeed) · [Guardian](https://www.theguardian.com/technology/2026/nov/16/elon-musk-tech-execs-open-letter-jensen-huang-return-nvidia-ceo) · [CNBC(실적)](https://www.cnbc.com/2026/11/16/nvidia-earnings-jensen-huang-resignation.html) · [CNBC(주가)](https://www.cnbc.com/2026/11/16/nvidia-stock-price-jensen-huang.html)

### 6. OpenAI가 의료용 컴퓨터 사용 에이전트와 의료 플러그인 20여 개를 냈다. 새 모델은 아니다 (공식, 날짜만 11-16, Wayback 첫 사본 14:56 UTC)

- **무엇:** 원문: "Computer-use agent capability is available in the OpenAI API, ChatGPT Work, and Codex." 원문: "introducing more than 20 new plugins to the healthcare plugin ecosystem."
- **자사 수치(재현 안 함):** 원문: "GPT‑6 Astra earned 94.3% of available rubric points, while GPT‑6 Luna earned 91.8%, despite costing 98% less at 8.5 cents per task." 조건은 원문: "Both models were evaluated at maximum reasoning effort." 안전성 원문: "Overall, 99% of 325 reviewed tasks were rated safe."
- **함정:** 환자 정보를 다루려면 원문: "organizations must use eligible services covered by their BAA with OpenAI, apply required security and retention settings, and extend protections to connected systems." 미국 HIPAA 체계 기준이라 한국 의료기관에 그대로 적용되지는 않는다. API 변경 로그(11-05)에는 새 항목이 없다.

[ChatGPT for Healthcare](https://openai.com/index/more-ways-to-move-care-forward-with-chatgpt-for-healthcare/) · [연구 글](https://openai.com/index/advancing-computer-use-for-healthcare-teams/)

### 7. 짧게

- **Anthropic 공모(CNBC 14:22 UTC):** 원문: "The IPO is expected to price the evening of Nov. 18, and begin trading on the Nasdaq on Thursday, Nov. 19." 원문: "At the current range, the offering would bring in more than $64 billion, giving the company a valuation of $1.405 trillion." 가격 범위 $70~78 그대로이고, 창 안 SEC 접수는 없다(최신 11-12 11:08 UTC FWP). [CNBC](https://www.cnbc.com/2026/11/16/anthropic-ipo-date-price-range.html)
- **(미확인) DeepSeek 무료 앱·웹 사용 제한(TechNode 06:35 UTC, Pandaily):** 사용자 보고다. TechNode 원문: "Users also reported that individual prompts were capped at around 40,000 Chinese characters". Pandaily 원문: "DeepSeek has not issued an announcement or confirmed any of the limits". 직전 판 "Coding Plan" 코드와의 관계는 원문: "nothing confirmed so far links that code to the limits users are now reporting"이다. [TechNode](https://technode.com/2026/11/16/deepseek-users-report-chat-limits-as-the-service-cites-capacity-constraints/) · [Pandaily](https://pandaily.com/deepseek-app-web-usage-limits-daily-quota-long-conversations-user-reports)
- **Shanghai AI Lab Sage S2.1 Flash(HF, GitHub 저장소 11-16 03:34 UTC, MIT):** 과학 멀티모달 모델이다. 배포 가이드 원문: "The Sage-S2.1-Flash-FP8 release is a 320B-A18B multimodal MoE model". 함정: `--trust-remote-code`가 필요한데, 원문: "Nothing warns you when the flag is missing." [HF](https://huggingface.co/internlm/Sage-S2.1-Flash) · [배포 가이드](https://huggingface.co/internlm/Sage-S2.1-Flash-FP8/raw/main/deployment_guide.md)
- **OpenAI 과학 블로그 개설, GPT-6 Astra의 표준모형 격자 정의(RSS 11-15 22:00 UTC):** 원문: "Astra helped turn that strategy into a construction and took the lead in developing the detailed proof." 저자 스스로 원문: "The method that Astra and I proposed is far more an answer in principle than an answer in practice"라고 했다. [글](https://openai.com/index/step-toward-defining-the-standard-model/) · [블로그 개설](https://openai.com/index/our-new-blog-for-science/)
- **Google Ask Maps 유럽 확대·여행 일정 작성(14:00 UTC):** 원문: "Now, Ask Maps is expanding to 30+ countries across Europe." [Google](https://blog.google/products-and-platforms/products/maps/ask-maps-is-coming-to-europe/)
- **Apple·OpenEvidence AI 건강 연구 앱(TechCrunch 01:30 UTC):** 원문: "The iPhone maker is partnering with healthcare-focused AI company OpenEvidence to launch a new study app." [TechCrunch](https://techcrunch.com/2026/11/15/apple-launches-new-app-to-study-how-people-use-ai-for-health/)
- **직전 판 후속(조회 11-16 16시대 UTC):**
  - Inkling 2 Small: vLLM PR #67325가 리뷰어 승인을 받았지만(09:51 UTC) 머지되지 않았다. 같은 PR 리뷰 원문: "We're also still investigating intermittent TP4 hangs on one 4x B300 HGX machine". [PR](https://github.com/vllm-project/vllm/pull/67325)
  - OpenAI 상태 페이지의 Compliance API 비용 데이터 지연은 11-16 00:17 UTC에 해소됐다. `gpt-6.2-*` 문서는 여전히 404다.
  - Fable 5.2 출시 프로모션과 텍스트 워터마크는 연장 공지 없이 11-16 23:59 PT(한국 시간 11-17 16:59)에 끝난다. 그 뒤 Pro·Team 표준 좌석에서 Fable 5.2를 쓰면 사용 크레딧이 빠진다(직전 판 창 경계 항목).
  - 여전히 401: inclusionAI Ling 3.2 Flash, Qwen4-Max-Preview, ERNIE 5.5 Flash Preview, Tencent Hy4, LG K-EXAONE 2.5 가중치.
  - 그대로: Agnes 2.5 Pro Theta(공식 발표 없음), Edge0(v0.2.0), MathArena(11-13 갱신), Gemini 4 Carbon 도입가 문구, Gemini Decision 1·Perplexity Decider v1.3·Runway Praxis-1(OpenRouter 미등록), MiniMax-H3-Image·inclusionAI Duo 라이선스(한국 제외), Inception Mercury 종료일(12-28), Microsoft 영국 Copilot Claude 기본 꺼짐 문구, GitHub Copilot 모델 6종 은퇴(12-14).
  - 소식 없음: 'Mythos' 상표 소송·Coxon 비판, OpenAI Daybreak·Wallet·로보틱스 팀, Kimi K3.1 Pro, Meituan LongCat-2.5 공식 글, Meta Muse, DeepSeek V5, Apple "Harbor", ByteDance 에이전트.

### 참고: 창 경계 항목

- **(창 이후) NVIDIA SC26 발표(16:00:01 UTC):** "Nemotron for Science" 등이 나왔다. 새 모델은 아니다. 발표 원문: "bringing together scientific applications of its NVIDIA Nemotron™ 3.5 Super VL open model, a new open chemistry dataset and post-training recipes". 다음 판에서 다룬다. [NVIDIA](https://nvidianews.nvidia.com/news/nvidia-introduces-nemotron-for-science-to-accelerate-discoveries-with-agents)
- **(창 경계) Huawei Cloud openPangu-2.1-Pro API(Pandaily 11-16 15:39 UTC, 회사 발표는 11-15 중국 날짜):** 출력은 1M 토큰당 3.2위안이다. 원문: "Huawei Cloud has scheduled openPangu-2.0-Pro and openPangu-2.0-Flash to go offline on November 30." 쓰고 있다면 이달 안에 옮긴다. [Pandaily](https://pandaily.com/huawei-cloud-maas-openpangu-2-1-pro-api-agents-arkts-ascend-operators)

## 기술 이슈

이번 창의 기술 소식은 AI를 쓴 공격 자동화가 중심이다. Google 위협 정보팀(GTIG)이 사람 개입 없이 패치를 익스플로잇으로 바꾼 첫 사례를 보고했다. 같은 주에 Google Project Zero와 Anthropic이 모두 취약점 공개 기한을 줄였다(창 경계 A, 모델 3번). AI 해킹 도구 자체가 키와 셸을 내주는 권고도 나왔다. 전역 GHSA 검토 게시는 62시간 만에 재개됐지만 AI 도구 권고는 여전히 반영되지 않았다. arXiv 일요일 공지(11-16 01:00 UTC)는 창 안에 있었다. CISA KEV 추가와 AI 저장소 101곳의 보안 권고는 모두 0건이다.

### 1. Google GTIG: 공격자가 처음으로 사람 없이 "패치 비교 → 익스플로잇 작성 → 실행"까지 자동화했다. 북한 추정 조직은 내부 저장소의 `.claude/settings.json`으로 백도어를 퍼뜨렸다 (공식, 날짜만 11-16, 첫 보도 14:00 UTC)

- **무엇:** GTIG의 분기 AI 위협 보고서다. 원문: "In the past quarter, GTIG identified threat actors using LLM frameworks to fully automate vulnerability discovery and exploitation without a human in the loop for the first time." 작동 방식 원문: "It compares code before and after patch announcements, analyzes findings to identify potential vulnerabilities, drafts exploits, and automatically executes them without human intervention." 노출된 공격 서버에서 나온 주석 번역은 원문: "LLM API keys for auto-exploitation (exploit_agent pilot)"이다. 표적은 원문: "targeting French websites associated with the media and entertainment industries, as well as the education sector."였고, 보고서 그림은 WordPress 플러그인 Spectra의 접근 제어 코드를 분석하는 장면이다.
- **Claude Code 설정 파일 악용:** 원문: "modifying .claude/settings.json configuration files so that when users started up Claude Code, it would automatically trigger the download of the SOMBERMEME backdoor." ClickFix 미끼로 처음 침투한 뒤 원문: "the actor pivoted into internal code repositories to execute a supply chain vector internal to the organization"였다. 조직 내부 저장소의 설정 파일을 고쳐, 그 저장소에서 Claude Code를 켠 동료 개발자에게 백도어가 내려오게 한 것이다.
- **LLMjacking 피해:** 원문: "In the most severe cases, cumulative resource abuse reached $1 million USD before remediation."
- **함정:**
  - WordPress처럼 패치 diff가 공개되는 소프트웨어는 업데이트를 미루는 시간만큼 위험이 커진다. "패치 후 며칠은 괜찮다"는 가정이 깨졌다.
  - 팀 저장소의 `.claude/settings.json`(hooks 등)을 CODEOWNERS 검토 대상에 넣고, 이 파일의 변경을 알림으로 받는다. 남의 저장소를 받아 Claude Code를 켜기 전에도 이 파일부터 본다.
- **중요한 이유:** n-day 취약점은 패치가 나온 뒤 공격이 시작되기까지 시간이 있었다. 그 시간이 0에 가까워진다. Project Zero와 Anthropic이 공개 기한을 줄인 근거와도 같다.

[GTIG](https://cloud.google.com/blog/topics/threat-intelligence/agentic-threat-activity-crosses-a-significant-threshold) · [BleepingComputer](https://www.bleepingcomputer.com/news/security/google-warns-that-fully-automated-ai-attacks-are-coming-soon/) · [The Register](https://www.theregister.com/research/2026/11/16/google-ai-agent-turned-a-security-patch-into-a-working-exploit-with-no-human-help/5309926)

### 2. AI 해킹·감사 도구 자체의 구멍: DeepAudit은 인증 없이 LLM API 키를 내보내고 아직 고쳐지지 않았다. RedAmon은 무인증 root 셸 등 6건을 고쳤다 (공식 권고, DeepAudit NVD 미러 11-16 15:30 UTC · RedAmon 저장소 권고 11-14 15:36 UTC)

- **DeepAudit(중국어권 "AI 해커 팀" 도구, GitHub 7천여 스타):** CVE-2026-129877(high) 원문: "Attackers can send a GET request to obtain the operator's LLM provider API keys and GitHub or GitLab tokens, then abuse those services and accessible repositories." 영향 범위는 2.0.0-beta.1 ~ 3.8.0이고, 최신 태그가 3.8.0이다. 기본 브랜치 코드에도 원문: "获取系统默认配置（无需认证）"(시스템 기본 설정 조회, 인증 불필요)가 달린 라우트가 그대로 있고, 이 라우트가 `openaiApiKey`, `claudeApiKey`, `githubToken`을 돌려준다. **수정판이 없다.** 같은 날 SSRF·경로 조작·토큰 유출 권고 3건이 함께 들어왔다.
- **RedAmon(AI 침투 테스트 프레임워크):** 저장소 권고 6건(critical 1, high 3, medium 2)은 창 시작 직전인 11-14 15:36 UTC에 나왔고, 창 안에 그중 2건이 NVD를 거쳐 전역 DB에 들어왔다. 11-15 판이 놓쳤다. 수정판 v7.8.0 원문: "Six advisories are fixed:" 원문: "the Kali terminal handed a root shell to whoever reached it." 원문: "any signed-in user could run arbitrary stdio MCP commands in the shared agent container."
- **함정:**
  - DeepAudit을 인터넷에 노출해 운영했다면 등록한 LLM 키와 Git 토큰을 모두 교체하고, 수정판이 나올 때까지 외부 노출을 끊는다.
  - RedAmon은 7.8.0으로 올린 뒤 새 RBAC 설정이 실제로 켜졌는지 확인한다. 릴리스 원문: "the update turns them on unless your `.env` already sets them." `.env`에 예전 값이 있으면 켜지지 않는다.
- **중요한 이유:** GTIG와 FBI가 경고한 공격 프레임워크와 같은 종류의 도구다. 방어용으로 띄운 쪽이 오히려 키와 root 셸을 내준다.

[DeepAudit 권고](https://github.com/advisories/GHSA-cjv8-6488-8485) · [DeepAudit 코드](https://raw.githubusercontent.com/lintsinghua/DeepAudit/v3.0.0/backend/app/api/v1/endpoints/config.py) · [RedAmon v7.8.0](https://github.com/samugit83/redamon/releases/tag/v7.8.0) · [RedAmon 권고](https://github.com/samugit83/redamon/security/advisories/GHSA-3cjg-8cp8-j269)

### 3. uBlock Origin 범용 XSS: 악성 사이트가 요소 선택기를 조종해 신뢰 필터를 넣는다. 발견은 GPT-5.6 Sol, PoC는 Claude가 썼다 (공식 권고 + 연구자 블로그 11-15 18:30 UTC, HN 520점대)

- **무엇:** 블로그 원문: "a malicious website could trick the extension into adding arbitrary trusted filters." 신뢰 필터는 페이지에 JS를 넣을 수 있어, 클릭 한 번으로 다른 사이트에서 임의 코드가 돈다. 대상은 Firefox MV2판이다. 권고의 영향 범위는 1.75.2 이하, 수정판은 1.75.4다.
- **AI 역할:** 원문: "GPT-5.6 Sol did most of the hard work of finding the issue." 원문: "Claude wrote most of the code for the PoC."
- **함정:** 자동 업데이트를 꺼 둔 Firefox라면 버전을 확인한다. 조회 시점 최신 안정판은 1.76.4다.
- **중요한 이유:** 사용자가 많은 보안 확장에서 LLM이 찾은 버그가 실제 권고까지 이어졌다. 1번과 같은 흐름을 방어 쪽에서 보여 준다.

[블로그](https://iter.ca/post/ubo-uxss/) · [권고](https://github.com/gorhill/uBlock/security/advisories/GHSA-cmqw-9r2w-6wv2) · [HN](https://news.ycombinator.com/item?id=50513160)

### 4. 전역 GHSA 검토 게시가 62시간 38분 만에 재개됐다. 재개분 20건은 전부 AI와 무관한 밀린 권고이고, AI 도구 권고는 여전히 전역 DB에 없다 (공식 데이터, 직접 셈)

- **재개:** 마지막 게시는 11-14 00:08 UTC(NLTK), 재개는 11-16 14:46 UTC다. 창 안 reviewed 20건은 aiohttp 9건, Nuxt 5건, SvelteKit 4건, NLTK 2건이다. 모두 수정판이 이미 나온 뒤에 올라왔다. SvelteKit GHSA-m9j8-jcmv-hp69(high)는 서버 전용 모듈이 클라이언트 번들에 섞이는 문제다. 원문: "server-only imports, which typically cannot be included in the client bundle, could make their way into it, potentially exposing secrets." 조건은 `src/params.js`를 거친 import일 때뿐이다.
- **아직 반영되지 않은 것:** 기술 에이전트가 저장소 권고 ID로 전역 DB를 직접 조회했다. vLLM(10-27 이후 12건), n8n, MLflow, LiteLLM, MCP Inspector·레퍼런스 서버, GPUStack, VS Code, Pulse, Dify의 최근 권고가 모두 404다. 이 프로젝트를 쓰면 Dependabot 알림만 믿지 말고 저장소 Security 탭을 직접 본다.
- **미검토(unreviewed) 210건 중 AI 관련 11건:** DeepAudit 4건과 RedAmon 2건(2번) 외에 이런 것이 있다.
  - REBUILD(CVE-2026-129875, high): 원문: "recover the administrator password hash by filtering User.password through the MCP QueryRecords tool." MCP 도구를 오라클로 써서 관리자 비밀번호 해시를 빼낸다.
  - VoAPI(LLM API 중계, CVE-2026-129540, high), WordPress AI Engine 저장형 XSS(CVE-2026-125804, high), OfficeCLI(CVE-2026-129876, medium).
- **악성(malware) 19건:** 이름에 mcp·agent·llm·claude가 들어간 것은 0건이다. 정상 패키지 오판 논란이 하나 있다. `@univerjs-pro/sheets-pivot-chart`(주간 12만 다운로드)의 1.5.0·1.5.1이 악성으로 찍혔다(00:14 UTC). JFrog 연구자가 이슈에서 원문: "have confirmed that it's *not* malware."라고 했고, 유지보수자는 원문: "We released version 1.5.2 three hours ago, which resolved this issue."라고 답했다. 권고는 철회되지 않았다.

[SvelteKit 권고](https://github.com/advisories/GHSA-m9j8-jcmv-hp69) · [REBUILD 권고](https://github.com/advisories/GHSA-r24j-jphp-67xq) · [Univer 이슈 #8230](https://github.com/dream-num/univer/issues/8230)

### 5. NoisyTCP(CVE-2026-99288): 11년 잠복한 리눅스 TCP 버그로 기본 설정의 Nginx·Apache·SSH 서버를 원격에서 다운시킨다 (oss-sec 11-15 16:00 UTC, AI 키워드 없음)

- **무엇:** 원문: "it can be triggered pre-auth and remotely against servers that TCP packets sent by an attacker can reach directly, such as Nginx, Apache, and SSH servers running with their "default" configuration." 원문: "Introduced: 079096f103fa (2015-10-03) Fixed: 4e2b3ef19e0e (2026-11-03)".
- **영향:** README 원문: "So is this a TCP RCE? Not yet." 원문: "But remote DoS can be achieved without much difficulty." 컨테이너 환경에서는 원문: "it takes down not only the web server application but the entire host." PoC는 공개되지 않았다.
- **완화책:** 원문: "sudo sysctl -w net.ipv4.tcp_syncookies=0". 함정 원문: "This setting is managed per network namespace." 원문: "Disabling it lowers the protection against SYN floods." 근본 해결은 배포판 커널 업데이트다.
- **중요한 이유:** 패치는 2주 전 메인라인에 들어갔지만 배포판 반영은 제각각이다. 추론 API 서버처럼 인터넷에 직접 노출된 호스트는 커널 버전을 먼저 확인한다.

[oss-sec](https://seclists.org/oss-sec/2026/q4/715) · [README](https://github.com/V4bel/NoisyTCP) · [BleepingComputer](https://www.bleepingcomputer.com/news/linux/new-noisytcp-linux-kernel-flaw-lets-attackers-crash-servers/)

### 6. LiteLLM·Starlette 비인증 RCE 체인을 노린 스캔이 주말 허니팟에 잡혔다. 필터를 피하는 Host 헤더 변형을 쓴다 (SANS ISC 11-16 13:11 UTC)

- **무엇:** 원문: "This weekend, we started seeing requests for /mcp-rest/test/tools/list hitting our honeypots." 요청 본문은 stdio MCP 서버로 `id` 명령을 띄우는 형태이고, Host 헤더를 `localhost]`, `[localhost`처럼 바꾼다. 원문: "This isn't a new vulnerability, and exploitation has been reported back in July." 원문: "Versions prior to 1.0.1 do not validate the "Host" header."(Starlette)
- **함정:** 10-08 판에서 다룬 CVE-2026-42271(LiteLLM)·CVE-2026-48710(Starlette) 체인이다. LiteLLM과 Starlette를 둘 다 올린다. 공개 PoC 문자열만 막는 WAF 규칙은 이 변형에 뚫린다.

[SANS ISC](https://isc.sans.edu/diary/33522)

### 7. (Microsoft 365 장애 후속) 예비 사후 보고서: 7월에 넣은 결함이 원인이었고 서버의 80% 이상이 응답하지 않았다. 장애 번호를 사칭한 피싱이 돈다 (매체보도, The Register 11-16 11:15 UTC, SecurityWeek 08:15 UTC)

- **예비 보고서(The Register 인용):** 원문: "Our load testing tended to focus on weaker hardware with fewer cores." 원문: "more than 80 percent of servers were unresponsive." 원문: "A final post incident report is due on November 20."
- **AI 관련성 부인:** Microsoft 성명(SecurityWeek) 원문: "This was a technical issue not related to cyber or AI / agents."
- **피싱:** 원문: "The fake emails posed as Microsoft 365 service health notices and referred to EX1550543, the actual incident identifier."
- **파급(HN 600점대):** 독일 호텔의 수영장 약품 경보 메일이 장애로 전달되지 않아 투숙객 15명이 다쳤다. [Tom's Hardware](https://www.tomshardware.com/tech-industry/data-centers/microsoft-365-outage-leads-to-chlorine-gas-poisoning-of-15-hotel-guests-email-warning-system-fails-40-first-responders-deployed-for-triage-four-hospitalized)
- **함정:** "EX1550543"를 대며 보류 메일을 복구하라는 메일은 피싱이다. Claude의 Microsoft 365 커넥터도 같은 장애로 11-12 ~ 11-13에 오류를 냈다. 안전 경보를 메일 한 경로에만 의존하지 않는다.

[The Register](https://www.theregister.com/saas/2026/11/16/microsoft-blames-monster-email-outage-on-software-defect-introduced-in-july-fix/5310026) · [SecurityWeek](https://www.securityweek.com/microsoft-rules-out-cyberattack-in-outage-warns-of-phishing-campaign/) · [HN](https://news.ycombinator.com/item?id=50509411)

### 8. arXiv 일요일 공지에서 주목받은 논문 (HF Daily Papers 11-16, alphaXiv 1면)

모두 저자 주장이고 재현하지 않았다.

- **"Aligning AI Teams":** 혼자서는 안전하게 행동하던 에이전트가 팀으로 묶이면 그 행동이 유지되지 않는다는 연구다. 원문: "diffusion of responsibility substantially accounts for the safety gap". 처방은 원문: "having a designated safety lead agent ensure system-level safety, and having a coordinator agent publish a safety specifications document are effective at restoring single-agent alignment." 다중 에이전트 시스템을 짠다면 안전 담당 에이전트를 따로 두는 설계를 검토할 만하다. [arXiv](https://arxiv.org/abs/2611.17820)
- **"AI Reviews Are Biased Towards the Appendix":** 원문: "the latest GPT and Claude models draw about 50% or more of their review weaknesses from the appendix." 사람은 원문: "humans draw only 8.4% of their weaknesses from the appendix"다. 논문 리뷰를 LLM에 맡길 때 본문 대비 부록 지적 비율을 점검한다. [arXiv](https://arxiv.org/abs/2611.17811)
- **"Interpreting Coding Agents Through Their Behavioral Trajectories"(Nebius SWE-rebench):** 원문: "We analyze a large corpus of 143,209 execution trajectories from 125 agent configurations spanning 117 unique models." 원문: "agents with similar solve rates can nevertheless differ substantially in how they explore, modify and verify code." 해결률만으로 코딩 에이전트를 고르면 안 된다는 근거다. [arXiv](https://arxiv.org/abs/2611.17440)
- **"Training Reasoning Models in Simulation to Natively Use Real Robots"(MIT):** 원문: "the model succeeds in 60% of our trials on a real dual-arm robot, whereas the base model succeeds in none." 시연 데이터 없이 시뮬레이션 강화학습만 썼다. [arXiv](https://arxiv.org/abs/2611.18562)
- **"MMFlash"(VLM 추측 디코딩):** 원문: "acceptance rates decrease from 46% in the lowest visual-dependence quartile to 22% in the highest". 이미지 의존도가 높은 질문일수록 기존 추측 디코딩이 덜 듣는다는 관찰이다. [arXiv](https://arxiv.org/abs/2611.18482)

### 9. 짧게

- **Uber 피해자 수가 처음 나왔다: 149,346명(Fireworks 침해 하류 피해, SecurityWeek 11:02 UTC):** 원문: "reportedly notified the Texas Attorney General’s Office that a total of 149,346 people were affected." 새 피해 회사는 없다. Fireworks 공지는 "Last updated November 9, 2026" 그대로다. [SecurityWeek](https://www.securityweek.com/uber-data-breach-impacts-150000-people/) · [Fireworks 공지](https://fireworks.ai/security/bulletins)
- **HN 구직 스레드 메일 주소를 노린 가짜 과제(블로그 11-16, HN 11:14 UTC):** 원문: "The email came to the alias from my post in the October “Who wants to be hired?” thread on Hacker News and named two companies from my CV." 과제로 받은 VS Code 확장에는 악성 코드가 없다. 원문: "There is no malware in the package, and that’s the design." 서버가 나중에 보낸 코드가 원문: "RAT is byte-identical to a published DEV#POPPER sample"이다. 패키지 정적 검사로는 못 잡는다. [블로그](https://getjump.me/posts/03-huntrole-fake-take-home/)
- **AI가 쓴 피해자 프로필을 함께 파는 인포스틸러 마켓(Hudson Rock, Help Net 13:03 UTC):** 원문: "Each profile describes who appears to own the machine, where that person works and which of their accounts are worth buying." [Help Net](https://www.helpnetsecurity.com/2026/11/16/ai-powered-infostealer-marketplace-victim-profiles/)
- **Bitly 옛 단축 링크 탈취(HN 350점):** 버려진 단축 링크가 다시 등록돼 엉뚱한 곳으로 연결됐다. 원문: "49 links re-created years after their tweets still work." 문서·README에 단축 링크를 쓰지 않는 편이 낫다. 기사 첫 게시는 11-15 14:31 UTC로 창 시작 직전이다. [Ransomnews](https://ransomnews.com/burger-king-tweet-bitly-link-hijack-2026/) · [HN](https://news.ycombinator.com/item?id=50511395)
- **FAA 11-13 지상 정지 후속(The Verge 14:43 UTC):** AI 원인설은 원문: "(So far, no evidence has emerged for either.)" 문제의 통신망은 원문: "FTI is scheduled to be replaced in 2028."이다. 공식 근본 원인 판정은 아직이다. [The Verge](https://www.theverge.com/transportation/1034039/faa-aviation-glitch-meltdown-ground-stop)
- **FBI FLASH Gootloader(ic3 11-16, AI 무관):** ARTEX 경보 다음 PDF다. 원문: "As of October 2026, the FBI identified over 1000 infected WordPress sites and 216 domains controlled by the Gootloader administrator, which led to over $28 million in ransomware payments." [PDF](https://www.ic3.gov/CSA/2026/261116.pdf)
- **UnrealIRCd CVE-2026-114713 실제 악용 보도(SecurityWeek 11:15 UTC):** KEV 조치 기한이 11-16이던 건이다. [SecurityWeek](https://www.securityweek.com/recent-unrealircd-vulnerability-exploited-in-attacks/)
- **직전 판 후속:**
  - Lovable RLS: 창 안 새 공식 글은 없다. 다만 11-10 Lovable 블로그 서명이 원문: "Igor, Head of Security"라서, 직전 판에서 "신원 미검증"으로 적은 HN 계정의 주장을 부분적으로 뒷받침한다. 게시 전 검사는 이미 있지만 원문: "publish anyway"로 우회할 수 있다(변경 로그 11-12). HN에서 예고한 모든 등급 대상 게시 차단은 아직 확인되지 않았다. [Lovable 블로그](https://lovable.dev/blog/find-it-fix-it-ship-it) · [변경 로그](https://docs.lovable.dev/changelog)
  - Cursor 최종 보고: `dateModified` 11-13 그대로, 조용한 수정 흔적 없음.
  - 새 진전 없음: Zread CLI(악성 권고 7건 그대로), wfx-loot(창 안 탐침 PR 0건), Flowise 3.1.4 SSRF(미수정), IBM ContextForge(v1.1.1), Pulse(6.9.0-beta.2에 보안 항목 없음), Darktrace·Corelight, Apify 사후 보고서(11-20 무렵), Blackwell GPU 지문, AMD SEV-SNP, CVE-2026-98432 Ubuntu focal·xenial·bionic(pending), PostgreSQL CVE-2026-6460 매니지드 패치, TSMC, MCP Server for WordPress·vLLM CVE.
  - AI 키워드 없는 전역 건: NoisyTCP(5번), uBO(3번) 외에 Node.js·Python·OpenSSL·Go 보안 릴리스는 창 안에 없다.

### 참고: 창 경계 항목

- **A. (직전 두 판 누락) Google Project Zero가 공개 기한을 단계적으로 30일까지 줄인다(공식, 11-13, HN 11-14 11:13 UTC 310점대, 창 안 Help Net·SecurityWeek 보도):** 원문: "Starting today, our policy is “Full 90”, i.e. vulnerabilities will be disclosed 90 days after they are reported to the vendor." 원문: "On February 2, 2027, we will move to a “Full 60” policy where vulnerabilities are disclosed after 60 days. On January 3, 2028, we will move to a “Full 30” policy where vulnerabilities are disclosed after 30 days." 근거 원문: "We are finding several vulnerabilities a week in targets where we once found a few vulnerabilities a year." 모델 3번의 Anthropic 정책과 같은 방향이다. [Project Zero](https://projectzero.google/2026/11/disclosure-policy.html) · [Help Net](https://www.helpnetsecurity.com/2026/11/16/google-project-zero-disclosure-policy/)
- **B. (ARTEX 후속, 지난 판들 누락) Barracuda가 ARTEX 공격을 당했다(블로그 11-11 21:24 UTC, 창 안 SecurityWeek 15:07 UTC 정리):** 원문: "It gained access using a valid cloud access key." 원문: "We did not retire the key when the vendor — a company working in AI — finished that work. That is on us." 원문: "In all, the attacker touched about 10 cloud accounts in roughly 44 hours." SecurityWeek 원문: "It reportedly used DeepSeek v4.1-flash as the initial model, but can use other LLMs as well, and GLM-5.3 and Grok 4.6 were observed in certain instances." 같은 주의 AWS(11-13)·Talos(11-13) ARTEX 보고서도 지난 판들이 다루지 않았다. AWS 보고서는 창 안에 THN이 "LiteLLM, Dify 등을 노린 AI 에이전트 캠페인"으로 다시 보도했다(07:34 UTC). 교훈은 Barracuda 원문 그대로다. 외주 작업이 끝나면 넘겨준 클라우드 키를 회수한다. [Barracuda](https://blog.barracuda.com/2026/11/11/anatomy-of-a-machine-speed-intrusion) · [SecurityWeek](https://www.securityweek.com/barracuda-hacked-amid-surge-in-artex-powered-cyberattacks/) · [AWS](https://aws.amazon.com/blogs/security/amazon-threat-intelligence-observes-threat-actors-running-cybercrime-campaigns-with-artex-ai-agents/) · [Talos](https://blog.talosintelligence.com/artex-and-beyond/) · [THN](https://thehackernews.com/2026/11/amazon-uncovers-ai-agent-campaign.html)
- **C. (누락) Forescout가 Claude 기반 하네스로 TCP/IP 스택 10개에서 취약점 99개를 찾았다(11-12, 창 안 Help Net 보도):** 원문: "99 vulnerabilities in 90 hours." 원문: "Total static analysis cost was $935, an average of $2.58 per finding." 모델 선택 이유 원문: "we used the Claude family due to their superior performance in our earlier evaluations and its not-too-restrictive guardrails." 한계 원문: "the harness is good at finding memory safety bugs in protocol parsing code but misses logic and design bugs". [Forescout](https://www.forescout.com/research-labs/finding-critical-infrastructure-bugs-with-off-the-shelf-ai/)
- **D. Kevin Buzzard "…and the proof is checked in Lean"(11-12, HN 창 안 180점대):** 10-08 판 OpenAI 수학 원고 건의 후속 비평이다. 원문: "of the 290 OpenAI theorems which come with Lean code, around 38 of them have the property that the Lean code does not even state, let alone prove, the main theorem of the paper". 원문: "if this code is AI-generated then you need to do some tests first." Lean으로 검증했다는 주장은 정리 서술부터 맞는지 봐야 한다. [Xena](https://xenaproject.wordpress.com/2026/11/12/and-the-proof-is-checked-in-lean/) · [HN](https://news.ycombinator.com/item?id=50515058)
- **E. HN 구인 글이 12년 만의 최저치 근처다(hntrends 11-14 기준):** 원문: "As of November 14, November's thread has 244 job postings, down 37% from 387 at the same point last year." 원문: "AI appears in 51.6% of November's postings, second only to Remote." [hntrends](https://www.hntrends.com/2026/november.html)

## 써볼 만한 도구

미국은 일요일이었다. 그래서 주요 코딩 에이전트에는 창 안 정식 새 버전이 없다. Claude Code(`latest` 2.1.325, `stable` 2.1.311), Codex CLI(정식 0.169.0, 알파만 나옴), Gemini CLI, Copilot CLI, OpenCode가 모두 그대로다. 대신 기본값 변경과 보안 보강이 몇 건 나왔다. 아래 도구는 하나도 설치하거나 실행하지 않았다. 함정은 릴리스 본문, PR 본문, README, 레지스트리에서 읽은 것이다.

### 1. (주의) browser-use 1.0.6: `llm`을 넘기지 않으면 기본 모델이 BU 4.0으로 바뀌어 토큰 단가가 15배가 된다 (공식, GitHub 릴리스 11-16 00:51 UTC, PyPI 미게시)

- **무엇:** `Agent(task)`와 `ChatBrowserUse()`의 기본 모델이 바뀌었다. 원문: "`Agent(task)` and `ChatBrowserUse()` now run `bu-4.0` on the Browser Use gateway instead of `bu-fast`."
- **왜 봐야 하나:** 모델을 따로 지정하지 않은 파이프라인은 코드를 바꾸지 않아도 비용이 오른다. 릴리스 원문: "**This costs more if you pass no `llm`.** BU 4.0 tokens cost 15 times as much as `bu-fast` ($1.80 / $9.00 per 1M input / output tokens, against $0.12 / $0.60)". 추론 강도도 올라간다. 원문: "it runs at high instead of medium reasoning in the default mode". 실행 한 번 기준으로는 차이가 더 크다. PR #6431 원문: "full cloud runs average about 30x ($0.19 vs $0.006)".
- **함정:**
  - 예전 비용을 유지하려면 원문: "To keep the previous model and price, pass `llm='bu-fast'`."
  - BU 4.0의 기반 모델은 밝히지 않았다. PR #6430 원문: "without naming any provider model behind them".
  - 클라우드 브라우저의 라이브 뷰 링크가 이제 기본으로 출력된다. PR #6425 원문: "The link grants browser control." CI 로그에 남지 않게 한다. 첫 실행 자동 로그인은 원문: "BROWSER_USE_AUTO_LOGIN=false disables automatic login."으로 끈다.
  - 조회 시점(11-16 16시대 UTC) PyPI 최신은 아직 1.0.5다. 배포 워크플로가 승인 대기 상태다. 1.0.5부터 붙은 판정 요청(`judge=False`로 끔, 직전 판 도구 5번)도 그대로다. 승인되면 바로 바뀌니, 지금 `llm`을 명시해 두는 편이 안전하다.
- **설치/사용:** [릴리스](https://github.com/browser-use/browser-use/releases/tag/1.0.6) · [PR #6431](https://github.com/browser-use/browser-use/pull/6431) · [PR #6425](https://github.com/browser-use/browser-use/pull/6425) · [모델·가격 문서](https://docs.browser-use.com/open-source/models)

### 2. Anthropic 문서 스킬(docx·pptx·xlsx): 외부 문서의 심볼릭 링크와 바깥 파일 링크를 막는다 (공식 저장소, PR #2221 머지 11-16 14:22 UTC)

- **무엇:** `anthropics/skills`의 문서 스킬이 바뀌었다. docx·pptx 편집 단계는 `unzip` 대신 Python `zipfile`로 압축을 푼다. 바뀐 docx SKILL.md 주석 원문: "not unzip, which restores symlinks — docx from external parties is untrusted". PDF·이미지 변환 전에는 바깥 파일 링크를 자른다. PR 원문: "when converting a .docx, .pptx or .xlsx to PDF or an image, links to files outside the document are cut first."
- **추천 이유:** 남이 보낸 Office 파일을 에이전트가 풀고 렌더링하는 흐름은 흔하다. 이 경로에서 심볼릭 링크나 외부 파일 참조로 로컬 파일이 딸려 나갈 여지가 줄었다.
- **함정:**
  - 도구 에이전트가 diff를 읽은 바로는, 링크 차단은 PDF·이미지처럼 화면에 그리는 출력으로 변환할 때만 걸린다. 웹 링크(`http(s)`·`mailto`)는 자르지 않는다.
  - 마켓플레이스 매니페스트의 버전 표기는 1.0.0 그대로다. 플러그인으로 설치했거나 스킬 폴더를 복사해 쓴다면 이 커밋이 들어왔는지 직접 확인한다. claude.ai 내장 스킬에 반영됐는지는 확인하지 못했다.
- **설치/사용:** `/plugin marketplace add anthropics/skills` 뒤 `document-skills` · [PR #2221](https://github.com/anthropics/skills/pull/2221) · [docx SKILL.md](https://github.com/anthropics/skills/blob/e4bb5d960d87126ff3b3a8af1a104b7db67560ba/skills/docx/SKILL.md)

### 3. MCP Java SDK 2.2.0: 2026-07-28 명세의 무상태 서버·클라이언트 정식판 (공식, GitHub 11-16 10:37 UTC)

- **무엇:** 10-18 판에서 다룬 2.2.0-M1의 정식판이다. 원문: "This release adds support for the **2026-07-28** MCP specification revision: a stateless server and a client in `io.modelcontextprotocol.modern`, next to the existing (legacy) client and server, which are unchanged."
- **추천 이유:** Java·Spring 쪽에서 세션 없는 새 프로토콜을 정식판으로 쓸 수 있다. 서버는 원문: "Passes all required 2026-07-28 server conformance scenarios."다. 클라이언트는 원문: "Passes the non-auth 2026-07-28 client conformance scenarios."다.
- **함정:**
  - 원문: "The modern API may still change in minor releases until 3.0."
  - 기존 사용자에게도 걸리는 변경이 있다. 원문: "`McpTransportContext` no longer has content-based `equals`/`hashCode`."
  - 새 클라이언트는 원문: "Sampling, roots, logging and extensions are not supported yet."
- **설치/사용:** Maven `io.modelcontextprotocol.sdk:mcp-core:2.2.0` · [릴리스](https://github.com/modelcontextprotocol/java-sdk/releases/tag/v2.2.0)

### 4. Coral: 에이전트가 쓴 TypeScript를 격리 실행하고, 성공한 코드를 도구로 저장하는 자체 호스팅 MCP 게이트웨이 (커뮤니티, Show HN 11-16 14:14 UTC, Apache-2.0 오픈 코어)

- **무엇:** README 원문: "Coral is an open-source, self-hosted gateway that sits between your agents and the systems they work on." 실행 환경은 원문: "Each program runs in a fresh V8 isolate with no network access, no credentials, and no imports." 기본 정책은 원문: "By default, tools that read data run right away and everything else needs your approval."
- **추천 이유:** "어떤 MCP 도구를 볼 수 있나"보다 좁게 권한을 줄 수 있다. 필드 하나만 바꾸는 저장 도구를 만들고, 그 도구에만 허용·차단을 건다. README에 Claude Code(`claude mcp add`)와 Codex 연결 예시가 있다. 새 저장소는 아니지만(4월 생성) 지난 판들에서 다루지 않았다.
- **함정:**
  - 수치는 자사 측정이다. 원문: "Across 34 tasks on GitHub, Linear, and Slack with Opus 5.5, custom tools cut tokens by 37%, cost by 31%, and time by 29%."
  - 팀 서버에는 원문: "an OIDC identity provider, a reverse proxy for TLS, and SQLite or Postgres"가 필요하다. Enterprise 기능은 오픈소스판에 없다.
  - HN에서 신규 계정 반응 의혹이 나왔다. 댓글 원문: "What is HN's policy on creating green accounts to inflate engagement on launch?" 창업자 답 원문: "We let preview users know we’d announce the product today and it looks like a handful of them created new accounts." 반응 수보다 README와 코드를 보고 판단한다.
- **설치/사용:** [GitHub](https://github.com/withcoral/coral) · [HN](https://news.ycombinator.com/item?id=50517803)

### 5. Context7 MCP 4.7.1 · ctx7 CLI 0.12.0: hono 보안 수정 반영, 업그레이드 가이드 CLI (공식, GitHub 11-16 14:53 UTC)

- **무엇:** MCP 서버 원문: "Update `@modelcontextprotocol/node` to 2.1.2 and `@modelcontextprotocol/server` to 2.4.0, which bring in security fixes for `@hono/node-server`, `hono` and their dependencies." CLI는 원문: "Add `ctx7 get-upgrade-guide <libraryId> <fromVersion> [toVersion]`, the CLI counterpart of the `get-upgrade-guide` MCP tool."
- **추천 이유:** 자체 호스팅 HTTP 모드로 Context7을 띄운다면 의존성 보안 수정 때문에 올린다. 라이브러리 버전을 올릴 때 에이전트에 이전·이후 버전 사이 변경점을 먹이는 용도로 CLI가 생겼다.
- **함정:**
  - 이름이 바뀌었다. 원문: "The research preview is now called experimental tools." 옛 플래그도 계속 동작한다.
  - 가이드를 준비 중이면 `ctx7`이 원문: "exits with code 75 while the guide is being prepared." 0이 아닌 종료 코드를 모두 오류로 처리하는 스크립트는 재시도 로직을 넣는다.
- **설치/사용:** [MCP 4.7.1](https://github.com/upstash/context7/releases/tag/%40upstash%2Fcontext7-mcp%404.7.1) · [ctx7 0.12.0](https://github.com/upstash/context7/releases/tag/ctx7%400.12.0)

### 6. llama.cpp b12420~b12441: MiniMax-M3.1과 음성 모델 3종 지원. `reasoning_effort` 오류 코드가 500에서 400으로 바뀌었다 (공식, 창 안 빌드 18개)

- **무엇:**
  - MiniMax-M3.1(b12420, PR #32301 머지 11-15 16:40 UTC). 작성자 측정 원문: "Support dspark: (tested) 66% acceptance on code, about 1.5x faster generation (54 vs 36 tok/s); 42% on prose." SGLang과의 perplexity 차이는 원문: "diff within 0.3%"라고 했다(재현 안 함).
  - 음성: Zonos3-mini TTS(b12424), Higgs Audio v3 STT와 VibeVoice-ASR(b12426).
- **함정(b12431, 11-16 09:20 UTC):** 템플릿이 받지 않는 `reasoning_effort` 값을 보내면 이제 400이 온다. 원문: "a request with such a level (e.g. max on Qwen3.8) reached the template and came back as a 500". 바뀐 뒤는 원문: "which the server answers with a 400 listing the supported levels." 5xx만 재시도하던 클라이언트는 이제 바로 실패한다. 오류 본문에 지원 단계가 나오니 그 값으로 바꾼다.
- **Inkling 2 후속:** 실제 체크포인트 변환 검증 기록은 아직 없다. V100 prefill 회귀(#32245)는 다른 하드웨어에서 독립 재현 댓글이 달렸고, 유지보수자 답은 없다.
- **설치/사용:** [PR #32301](https://github.com/ggml-org/llama.cpp/pull/32301) · [b12431](https://github.com/ggml-org/llama.cpp/releases/tag/b12431)

### 짧게

- **GitHub AI Scan 일괄 시작(공식 변경 로그 11-16 15:50 UTC, 퍼블릭 프리뷰):** 원문: "You can now start AI Scans for up to 20 repositories at once." 원문: "A GitHub Copilot license isn't required." [변경 로그](https://github.blog/changelog/2026-11-16-bulk-start-ai-scan-for-repositories)
- **Docker Sandboxes Firecrawl 키트(공식 블로그 11-16 14:00 UTC):** 샌드박스 에이전트가 웹 검색·스크랩을 하되 키는 샌드박스에 넣지 않는다. 원문: "the proxy swaps in the real key on requests to api.firecrawl.dev, so the key never enters the image or the sandbox." 함정 원문: "Kits are experimental and in Early Access, so the kit format and sbx commands may change." [Docker 블로그](https://www.docker.com/blog/docker-sandboxes-firecrawl-web-access/)
- **JetBrains TeamCity AI Agents 플러그인(공식 블로그, RSS 11-16 09:28 UTC):** 원문: "The new experimental AI Agents plugin for TeamCity lets you run Codex and Claude Code directly in TeamCity builds". 함정: 빌드 시작 때 원문: "checks whether the selected client is available and installs it if necessary". 글에 버전 고정 방법이 없으니 CI 재현성이 필요하면 에이전트를 미리 설치한 에이전트 이미지를 쓴다. [JetBrains](https://blog.jetbrains.com/teamcity/2026/11/run-ai-agents-in-teamcity/)
- **OpenAI Agents SDK Python v0.24.0-alpha.8(11-16 04:39 UTC, 프리릴리스):** 원문: "**Approve Python tool calls before they run.**" PyPI에는 없다(정식 최신 0.24.0). [릴리스](https://github.com/openai/openai-agents-python/releases/tag/v0.24.0-alpha.8)
- **goose 1.59.2(11-16 05:26 UTC):** 11-15 판에서 "곧"이라던 macOS 데스크톱 빌드(`Goose-darwin-arm64.zip`·`x64`)가 올라왔다. 저장소는 `aaif-goose/goose`로 옮겨졌다. [릴리스](https://github.com/aaif-goose/goose/releases/tag/v1.59.2)
- **LangChain 파트너 패키지 4종(xai 1.4.0, groq·deepseek·mistralai 1.2.0, 11-15 21:35 UTC~):** 함정: Python 3.10 지원을 뺐다. 원문: "Remove EOL'd Python 3.10 from partner packages". 3.10 환경은 버전을 고정한다. [langchain-xai](https://github.com/langchain-ai/langchain/releases/tag/langchain-xai%3D%3D1.4.0)
- **(후속) turbo-browser-mcp가 npm에 올라왔다(0.6.1, 11-16 04:59 UTC):** 직전 판 도구 3번에서 "npm 이름 미등록, 선점 위험"으로 적은 패키지다. 저장소 커밋 시각과 npm 게시 시각이 맞물려 작성자 본인 게시로 보인다. 다만 npm 메타데이터에 `repository` 필드와 provenance가 없어 npm 쪽만으로는 출처를 검증할 수 없다. 로그인된 브라우저(`127.0.0.1:9222`)에 먼저 붙는 기본 동작도 그대로다. [npm](https://www.npmjs.com/package/turbo-browser-mcp) · [GitHub](https://github.com/Tthanh2k6/turbo-browser-mcp)
- **(출시 전) Claude Code `sec-default` mod 소스 변경(창 안 커밋 4건):** 직전 판 도구 짧게의 후속이다. `allowManagedModsOnly`가 사용자 등급 플러그인을 통째로 거부하지 않고 hooks 모듈만 비운다. README 원문: "the plugin continues past the user tier with `provides.modules` emptied, or, in a dev-mods folder, is refused whole." 이 소스가 실린 npm 버전은 아직 없다. [README](https://github.com/anthropics/claude-code/blob/84c19dd92766c216da9395d5c1c12808ddaf5dcc/mods/sec-default/README.md)
- **(회귀 보고) Claude Code 2.1.325, Windows Git Bash에서 인자 없는 `claude`가 바로 종료된다(커뮤니티 이슈 #110401, 11-16 11:22 UTC, 직원 답 없음):** 오류 원문: "Error: Input must be provided either through stdin or as a prompt argument when using --print". 원문: "Rolling back to 2.1.321 — same machine, same Git Bash window, same settings — starts the TUI normally." 같은 증상이면 2.1.321에 머문다. [이슈](https://github.com/anthropics/claude-code/issues/110401)
- **(커뮤니티) Claude Code가 OSM 공용 Overpass API에 부하를 준다는 이슈(#110292, 11-15 23:45 UTC, 반응 8):** 원문: "When asked to search OSM data, claude code defaults to using the public Overpass API, meant for use by human OSM mappers." OSM 포럼에서도 같은 날 성능 문제가 논의됐다. 지도 데이터를 쓰는 에이전트 작업은 자체 Overpass 인스턴스나 추출본을 쓴다. [이슈](https://github.com/anthropics/claude-code/issues/110292) · [OSM 포럼](https://community.openstreetmap.org/t/overpass-api-performance-issues/140598/258)
- **(창 경계) NVIDIA SkillEvaluator(v0.9.0 11-13, GitHub 일간 트렌딩, Apache-2.0):** README 원문: "SkillEvaluator checks AI agent skills and measures their effect on task results: start with quality checks, then compare runs with and without a skill." 함정 원문: "Model and managed-sandbox usage can incur charges". 직접 만든 Claude 스킬이 실제로 결과를 낫게 하는지 재 볼 때 쓴다. [GitHub](https://github.com/NVIDIA/SkillEvaluator)
- **직전 판 후속 중 새 진전 없음:** Claude Code(2.1.325, `subagentStatusLine` 복구 "Monday's release"는 조회 시점 16:17 UTC까지 안 나옴, 샌드박스 `denyRead` 이슈 #110227 답 없음), Claude Agent SDK(Python 0.2.182·TS 0.3.325), Code Mode 문서, Claude Platform 릴리스 노트(11-12), Claude Meetings, `claude-plugins-official`·`openai/plugins`(창 안 커밋 0건), Claude Data 기본 켜짐(11-20 그대로), Qwen Code 0.27.4(나이틀리만), Cursor SDK 변경 로그(1.4.0 항목 여전히 없음), OpenCode 2.3.0(#63879 수정 PR #64048 열림), Codex CLI 0.169.0, Gemini CLI 0.68.0, Copilot CLI 1.0.112, Crush 0.101.0, Pi, Mistral Vibe, Amp, Zed, VS Code 1.146.0, Ollama, vLLM 0.33.0, LM Studio, transformers 5.23.0, LiteLLM, Vercel `ai@7.0.173`, FastMCP 4.2.0, Cloudflare Kitesurf, n8n, Open WebUI, Dify, DSPy, Supabase·GitHub·MongoDB·Sentry·Neon MCP, Chrome DevTools MCP, Playwright MCP, MCP Python·TypeScript·C#·Rust SDK와 Inspector·레퍼런스 서버, MCP 명세(문서 정확도 수정 PR #3516만 머지), Conductor, Val Town(12-10 그대로), Agweb, Gnomon, db-branch. 주요 AI 도구 저장소 20곳의 보안 권고는 창 안 0건이다.

## 주목할 점

- **패치를 적용하는 속도가 곧 보안이 된다.** GTIG는 공격자가 패치 diff에서 익스플로잇까지 사람 없이 가는 사례를 처음 확인했다. 방어 쪽도 같은 근거로 움직였다. Anthropic은 2027년부터 공개 기한을 30일로 줄이고, Project Zero는 2028년까지 30일로 줄인다. 그런데 AI 도구 권고는 전역 GHSA에 반영되지 않아 Dependabot 알림이 오지 않는다. 쓰는 AI 도구(vLLM, LiteLLM, n8n, MCP 서버)의 저장소 Security 탭과 릴리스를 직접 구독하는 편이 안전하다. 남의 저장소를 받아 Claude Code를 켜기 전에는 `.claude/settings.json`부터 본다.
- **다가오는 일정:**
  - 이번 주: Claude Fable 5.2 출시 프로모션 종료와 Anthropic 텍스트 워터마크(11-17 16:59 KST). Anthropic 호주 청약 마감과 NVIDIA 실적 발표(11-17 미국 장 마감 후 = 한국 11-18 오전). Anthropic 공모가 확정(11-18 저녁 ET)과 Nasdaq 거래 시작(11-19). Microsoft 365 최종 사후 보고서, Enterprise Claude Data 기본 켜짐, Apify 사후 보고서 약속 기한(11-20 무렵).
  - 그다음 주: ERNIE 5.5 Flash Preview 가중치 공개 약속(11-25 전), Qwen4-Max-Preview 종료(11-25 16:00 UTC), Gemini 4 Carbon 도입가와 Tencent Hy4 flash 할인 종료, Huawei openPangu-2.0 종료(11-30).
  - 12월: Dependabot 비활성 저장소 알림 중지(12-08), 영국 Copilot 테넌트 Claude 기본 켜짐(12-09, 보도 기준), Gemini 스마트 안경 판매와 Val Town 읽기 전용(12-10), ChatGPT 옛 음성 모델 은퇴(12-11), GitHub Copilot 모델 6종 은퇴(12-14), Inception Mercury API 종료(12-28).
  - 그 뒤: Anthropic 새 공개 기한 적용(2027-01-01 신고분), Vertex의 Hydrogen 캐시 쓰기 과금(2027-01-12), Project Zero "Full 60"(2027-02-02), Claude Meetings 통화 중 문서 무료 종료(2027-02-10), 다음 MCP 명세(2027-03-30).

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - FT(Huang 복귀 논의), WSJ, Bloomberg, Reuters(캡차), SCMP는 제목과 재보도로만 봤다.
  - Microsoft 예비 사후 보고서는 관리 센터에만 있어 The Register·SecurityWeek 인용으로 봤다. Uber의 텍사스 주 법무장관실 신고도 SecurityWeek의 "reportedly" 보도만 봤다.
  - Qwen 블로그, openai.com 글, Alizila, Tencent Hy, BIS 규칙 PDF, FBI PDF는 r.jina.ai로 읽었다. openai.com은 본문이 403이다.
  - Guardian(AI 아동 성착취물 통계), Debian 버그 #1154101, SentinelOne ARTEX 글은 봇 차단에 막혔다.
  - x.com은 타임라인을 볼 수 없어 각 랩·도구 팀의 X 발표를 놓쳤을 수 있다. reddit은 막혀 있다.
- **시각이 날짜만 있거나 불확실한 원문:**
  - Qwen4-Max 블로그는 자체 시각이 없다(r.jina.ai 메타의 08-03은 사이트 메타 오류로 본다). HN·OpenRouter 시각으로 잡았다.
  - Tencent Hy4 flash는 목록 페이지 날짜(11-15)와 본문 날짜(11-16)가 다르다. TechNode 06:54 UTC로 보완했다.
  - OpenAI RSS(날짜만 신뢰, Wayback으로 보완), Anthropic CVD 글(`dateModified`·Wayback으로 보완), GTIG 보고서(첫 보도 14:00 UTC로 보완), Project Zero(11-13), getjump 블로그, TeamCity 글(RSS 시각).
  - Sage S2.1 Flash는 HF 저장소가 창 전에 만들어져, 공개 전환 시각을 확인하지 못했다.
- **확인하지 못한 것:**
  - BIS 원격 접근 규칙의 연방관보 게재일, Hy4 flash 가중치 공개 일정과 할인 종료 뒤 OpenRouter 단가.
  - Claude Meetings 장애의 해소 시각(창 종료 때 진행 중), DeepSeek 사용 제한의 공식 확인.
  - GTIG 보고서를 인용한 BleepingComputer의 "3개월 전망" 문구(Google 본문 텍스트에 없음, 그림 안일 가능성), Univer 패키지 악성 판정 근거, Barracuda가 말한 "AI 업체"의 정체, NoisyTCP 패치의 배포판별 반영 현황.
  - browser-use BU 4.0의 기반 모델, Anthropic 문서 스킬 변경이 claude.ai 내장 스킬에 반영됐는지, turbo-browser-mcp npm 계정이 GitHub 작성자 본인인지.
  - 재현하지 않은 것: 모든 벤치마크·비용·속도 주장(Qwen Work360, Tencent WorkIndex, OpenAI 의료 평가, Coral, MiniMax-M3.1 DSpark, Forescout, 논문 수치). 도구는 하나도 설치하거나 실행하지 않았다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 10점 이상 스토리 약 105건과 Show HN 158건은 제목을 전부 봤다. 그 아래는 키워드로만 걸렀다.
  - 전역 GHSA: reviewed 20건, unreviewed 210건, malware 19건을 직접 셌다. AI 저장소 101곳과 주요 도구 저장소 20곳의 저장소 권고를 조회했다.
  - arXiv cs.CR 일요일분은 API 검색이 일부만 돌려줬다. HF Daily Papers 11-16(28편)은 전부 봤고 alphaXiv는 1면만 봤다.
  - Hugging Face는 주요 조직 약 90곳의 `lastModified`와 전역 `createdAt` 약 4천 건을 봤다. ModelScope는 보지 않았다. 한국 랩은 HF와 국내 매체 RSS로만 확인했고 발표가 없었다.
  - Gemini 앱 릴리스 노트(10-13 이후 항목 없음, 캐시 의심), Vertex AI 릴리스 노트(캐시 동결), Antigravity 변경 로그(JS 렌더링)는 일 단위로 판정하지 못했다. MCP 레지스트리는 창 안 갱신분 첫 100건만 봤다.
  - 창이 끝날 때 열려 있던 것: Claude Meetings 장애, Claude iOS Sign in with Apple 장애(57시간째), browser-use 1.0.6 PyPI 게시, vLLM Inkling 2 Small PR(승인 1건, 미머지). 창 직후(16:00 UTC) NVIDIA SC26 발표가 나왔다. 다음 판에서 다룬다.
