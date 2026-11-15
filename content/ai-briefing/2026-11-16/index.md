---
title: "2026-11-16 AI 브리핑"
date: 2026-11-16T01:00:00+09:00
tags: [ai-briefing, nvidia, security, anthropic, coding-agents]
description: "새 모델이 없는 주말에 NVIDIA는 Huang 복귀 압박과 중국행 칩 중개 의혹에 휩싸였고, Lovable 앱 다수가 로그인 없이 개인정보를 내보낸다는 연구가 HN 1위권에 올랐으며, llama.cpp가 Inkling 2 Small을 돌리고 Qwen Code는 Auto 모드 판정을 조였다."
---

> 조사 범위: 2026-11-15 01:00 ~ 2026-11-16 01:00 KST(2026-11-14 16:00 ~ 11-15 16:00 UTC, 미국 토요일 08:00 ~ 일요일 08:00 PST). 직전 브리핑(11-15 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, SEC EDGAR `acceptanceDateTime`, GitHub `published_at`·`merged_at`, npm·PyPI 게시 시각, 저장소·전역 GHSA `published_at`, CISA KEV, 상태 페이지 API, Wayback 사본 시각, OpenRouter 등록 시각, Hugging Face API, 서버 `last-modified`, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-15 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **새 모델이 없는 주말에도 NVIDIA 혼란은 이어졌다.** 대주주 두 곳이 이사회에 Huang 복귀 가능성을 열어 두라는 서한을 냈다. 복귀 청원 서명은 11만 5천 명을 넘었다. WSJ는 Huang의 전 연인과 연결된 회사가 중국행 칩 주문 약 120억 달러를 중개했다고 보도했다. 화요일(11-17)에 실적 발표, 목요일(11-19)에 Anthropic 상장이 이어진다.
- **Lovable로 만든 앱 다수가 로그인 없이 개인정보를 내보낸다는 2주 전 연구가 HN 1위권에 다시 올랐다.** 원인은 Supabase 테이블의 행 수준 보안 누락이다. Supabase의 새 기본값은 기존 테이블을 바꾸지 않으니 직접 점검해야 한다. 직전 판이 놓친 FBI 경보도 드러났다. LLM 침투 도구 ARTEX의 노출 콘솔이 1,600개를 넘고, 공격자는 주로 DeepSeek Flash 계열을 쓴다는 내용이다.
- **도구는 오픈소스 쪽만 움직였다.** llama.cpp가 Inkling 2 Small을 GGUF로 돌리게 됐고, Qwen Code 0.27.4는 Auto 모드를 "사용자가 허락했나" 기준으로 바꿨다. Claude Pro·Team 표준 좌석의 Fable 5.2 출시 프로모션은 한국 시간 11-17 16:59에 끝난다. 그 뒤 Fable 5.2를 쓰면 사용 크레딧이 빠진다.

## 모델 소식

완전한 주말 창이다. 미국 3사 모두 새 모델, API 변경, 가격 변경이 없다. OpenAI API 변경 로그(최신 11-05), Gemini API·AI Studio(11-12 Hydrogen), Claude Platform(11-12), xAI(11-02), Mistral(10-31), DeepSeek(11-13)이 모두 직전 판 조회 때 그대로다. OpenRouter 신규 등록은 0건이다. Anthropic과 NVIDIA의 SEC 신규 접수도 창 안에 없다. Hugging Face 주요 조직 약 170곳에서 창 안에 새로 공개된 모델은 Agnes 2.5 Pro Theta 하나다. 창 안 소식은 NVIDIA 후속이 대부분이다. 대신 직전 두 판이 놓친 창 경계 항목 두 건(MathArena 순위 교체, Anthropic의 Opus 5.5 사용 가이드)이 실제로 쓸모 있어 이 영역 끝에 묶었다.

### 1. NVIDIA 후속: 대주주 두 곳이 "Huang 복귀 가능성"을 열어 두라고 했고, WSJ는 Huang 전 연인과 연결된 회사가 중국행 칩 주문 약 120억 달러를 중개했다고 보도했다 (매체보도, 창 안 8-K·회사 성명 없음)

- **대주주 서한(CNBC 11-14 17:35 UTC):** Altimeter의 Brad Gerstner와 Atreides의 Gavin Baker가 이사회에 보낸 서한이다. 원문: "pursue a slow, measured, and deliberate process regarding the future leadership of Nvidia." 다만 원문: "The document did not call for Huang's immediate reinstatement."
- **복귀 청원 확대(TechCrunch 11-14 21:41 UTC):** 11-14 판에서 다룬 직원 청원이 커졌다. 원문: "appears to have attracted more than 115,000 signatures as of Saturday afternoon." 원문: "The petition website said the signers include 5,965 current Nvidia employees, 1,108 former employees, and 63,063 shareholders." Elon Musk, Tim Sweeney, Garry Tan 등이 서명했다.
- **Megaspeed 중개 의혹(WSJ, The Decoder 11-15 14:45 UTC):** 원문: "Megaspeed arranged nearly $12 billion worth of Nvidia high-performance chip orders for Chinese companies between March 2024 and June 2025, according to documents reviewed by the Wall Street Journal." WSJ 원문은 읽지 못했다. Megaspeed는 성명을 냈다(CNBC 11-15 08:20 UTC). 원문: "Ms. Alice Huang officially left Megaspeed effective August 30, 2025 and currently has no role with the Company," 외부 법률 자문을 써서 원문: "thorough internal investigation"을 한다고 했다.
- **의회(FT 11-15 11:00 UTC, RSS 요약만 확인):** 제목은 "Congress moves to pass tighter AI chip export rules following Nvidia scandal"이다.

중요한 이유: CEO 공백 속에서 화요일(11-17) 실적 발표를 맞는다. 대중국 수출 통제 강화 논의도 같은 주에 붙었다. OpenAI·Anthropic이 의존하는 GPU 공급과 가격에 걸린 사안이다. 이틀 뒤인 11-19에는 Anthropic이 상장한다.

[CNBC(서한)](https://www.cnbc.com/2026/11/14/nvidia-investors-jensen-huang-return-board-letter.html) · [TechCrunch(청원)](https://techcrunch.com/2026/11/14/elon-musk-other-tech-execs-sign-petition-calling-on-nvidia-to-reinstate-jensen-huang/) · [The Decoder(Megaspeed)](https://the-decoder.com/firm-linked-to-jensen-huangs-ex-lover-reportedly-brokered-nearly-12-billion-in-nvidia-chip-orders-for-china/) · [CNBC(Megaspeed 성명)](https://www.cnbc.com/2026/11/15/megaspeed-alice-huang-probe-nvidia-scandal.html) · [FT](https://www.ft.com/content/3062790a-553d-41b8-9c7d-bae17b9d2e29)

### 2. Agnes AI가 187.8B MoE 멀티모달 모델 Agnes 2.5 Pro Theta를 Apache 2.0으로 올렸다. 벤치마크는 자사 그림을 옮긴 것이다 (공식 HF, createdAt 11-15 02:46 UTC)

- **무엇:** README 원문: "**Agnes 2.5 Pro Theta** is a multimodal mixture-of-experts checkpoint distributed with its model implementation, tokenizer, chat template, and image/video processors." 원문: "The package has approximately **187.8B total parameters** and a configured context limit of **262,144 tokens**." 텍스트·이미지·영상을 받아 텍스트를 낸다. 가중치는 BF16이다.
- **자사 수치:** README 표에서 GPQA Diamond 92.9, SWE-bench Pro 64.1이다. 같은 표에서 HLE-Diamond와 SciCode는 비교 대상 DeepSeek-V4.1-Flash보다 낮다. README 원문: "The scores above summarize a publisher-provided chart, not a reproducible evaluation bundle."
- **함정:** 자체 모델 코드라 `trust_remote_code=True`가 필요하다. 원격 코드를 실행하니 격리된 환경에서 받는다. 하드웨어 원문: "A system with **4 × H200-class GPUs** is a practical capacity starting point, not a measured throughput or maximum-context guarantee." 회사의 공식 발표 글은 찾지 못했다. 조회 시점 좋아요 10, 다운로드 130으로 반응도 작다.

[Hugging Face](https://huggingface.co/Agnes-AI/Agnes-2.5-Pro-Theta)

### 3. 짧게

- **Anthropic 공모(창 안 SEC 접수 없음):** `data.sec.gov` 기준 최신은 여전히 11-12 11:08 UTC FWP다. CNBC(11-15 12:00 UTC) 원문: "which is currently expected at $70 to $78 a share", "With Anthropic expected to debut on the Nasdaq on Thursday". 가격 범위를 올렸다는 보도는 없다. 11-17 호주 청약 마감, 11-18 가격 확정, 11-19 거래 시작 일정 그대로다. 'Mythos' 상표 소송과 Coxon 비판(직전 판 2번)은 재보도만 있고 Anthropic의 추가 반응은 없다. [CNBC](https://www.cnbc.com/2026/11/15/anthropic-ipo-is-make-or-break-for-the-technology-industry.html)
- **Claude iOS의 Sign in with Apple 장애가 창 내내 열려 있었다:** 직전 판 3번의 장애다. 11-14 20:12 UTC 갱신 원문: "We are continuing to investigate an issue with Sign in with Apple on the Claude iOS app, which is mainly affecting new accounts. New users can sign up with Google or email instead." 시작(11-14 07:11 UTC)부터 창 종료까지 33시간 가까이 됐다. 11-13 장애 2건의 사후 보고서는 아직 없다. [상태 페이지](https://stspg.io/4858wf40dx89)
- **(미확인) DeepSeek 웹 코드에 "Coding Plan" 할당량 로직(Pandaily, IT Home 인용):** 출시 발표가 아니다. 원문: "DeepSeek has not announced any subscription, and nothing about pricing, tiers or a launch date has been made public." 원문: "Code that appears in a web client can also belong to features that are being tested and never ship." [Pandaily](https://pandaily.com/deepseek-web-frontend-code-quota-management-coding-plan-unannounced)
- **Apple Siri AI 안경 보도(Bloomberg Gurman, 9to5Mac 11-15 15:48 UTC):** 9to5Mac 원문: "the product should be introduced at the next WWDC, and they should be on the market by the end of the year." 지난 판들의 Apple "Harbor"와는 별건이다. [9to5Mac](https://9to5mac.com/2026/11/15/apples-siri-ai-smart-glasses-are-reportedly-launching-next-year-heres-what-to-expect/)
- **직전 판 후속 중 새 진전 없음(조회 11-15 16시대 UTC):**
  - Inkling 2 Small: HF 토론 #2와 vLLM PR #67325가 그대로 열려 있다. 대신 llama.cpp가 지원을 넣었다(도구 1번).
  - Edge0: 최신 릴리스는 v0.2.0(11-11) 그대로다. Metal 관련 PR 3개가 열려 있다.
  - OpenAI: 상태 페이지의 Compliance API 비용 데이터 지연이 여전히 monitoring이다. 로보틱스 팀 해체, Daybreak·ChatGPT Wallet은 매체 재보도만 있다. `gpt-6.2-*` 문서는 여전히 404다.
  - Google: Gemini 앱 Wallet·Memory Summary와 AI Studio 크레딧은 후속이 없다. Gemini Decision 1은 OpenRouter 미등록이다. Gemini 4 Carbon 도입가 문구(원문: "$2.00 through November 30, 2026. $4.00 starting December 1, 2026.")도 그대로다.
  - 여전히 401: inclusionAI Ling 3.2 Flash, Qwen4-Max-Preview, ERNIE 5.5 Flash Preview, Tencent Hy4, LG K-EXAONE 2.5 가중치.
  - 그대로: Perplexity Decider v1.3(OpenRouter 미등록), Meta MetaEncoder v1, MiniMax-H3-Image·inclusionAI Duo 라이선스(한국 제외), DeepSeek API 가격, Inception Mercury 종료일(12-28), Microsoft 영국 Copilot Claude 기본 꺼짐 문구, GitHub Copilot 모델 6종 은퇴(12-14).
  - 소식 없음: Runway Praxis-1, Meta Muse, Meituan LongCat-2.5 공식 글, DeepSeek V5, Kimi K3.1 Pro, Apple "Harbor", ByteDance 에이전트.

### 참고: 창 경계 항목

- **MathArena 종합 1위가 오랜만에 OpenAI가 아닌 모델로 바뀌었다(운영자 X 11-13 15:27 UTC, The Decoder 11-14 16:48 UTC):** ETH Zurich SRI Lab·INSAIT의 수학 벤치마크다. 사이트 원문: "November 13: We added Claude Fable 5.2 and Gemini 4 Carbon results to MathArena!" 운영자 Jasper Dekoninck 원문: "For the first time in a very long time, we have a non-OpenAI model as #1 on MathArena! Gemini 4 Carbon takes over the first place from GPT-6.1 Sol, with Fable 5.2 taking #2." 사이트 표 기준 종합 점수와 비용은 Gemini 4 Carbon(high) 85.4%·$15.70, Claude Fable 5.2(max) 82.8%·$40.20, GPT-6.1 Sol(max) 80.6%·$1.07이다. 오픈 모델 최고는 Kimi K3.1 Pro(max) 54.3%다. 함정: 세 모델의 오차 범위(±3.0%, ±5.3%, ±2.7%)가 겹쳐 순위 차이는 뚜렷하지 않다. 비용은 Sol 대비 Carbon이 약 15배, Fable 5.2가 약 38배다. 수학 작업을 많이 돌린다면 비용 대비로는 여전히 Sol이다. 직전 판 창 안의 X 글을 두 판 모두 놓쳤다. [MathArena](https://matharena.ai/) · [운영자 X](https://x.com/j_dekoninck/status/2121263753006374997) · [The Decoder](https://the-decoder.com/gemini-4-carbon-takes-the-matharena-crown-but-costs-15-times-more-than-gpt-6-1-sol/)
- **Anthropic의 Opus 5.5 지식 업무 가이드: "think hard"를 지우고 Medium으로 시작하라(공식, 날짜만 11-12, The Decoder 11-14 16:42 UTC):** 원문: "Delete “think hard” and “think carefully.” Thinking is always on for Opus 5.5, and it decides how much to think based on your request and the effort level." 원문: "Medium is the default for Opus 5.5, and on our coding and knowledge-work evaluations, Opus 5.5 at Medium matched or beat Opus 5 at High." Sonnet 사용자에게는 원문: "if you find yourself pushing Sonnet to Extra high or Max effort, consider switching to Opus 5.5."라고 했다. 이 가이드에는 오늘 바로 걸리는 함정이 있다. 11-11 판은 출시 프로모션 동안 Pro·Team 표준 좌석이 주간 한도의 50%까지 Fable을 쓸 수 있다고만 적었다. 프로모션이 끝난 뒤는 이렇다. 원문: "Pro plans and standard seats on Team and seat-based Enterprise plans don’t include Fable 5.2 in their limits outside limited-time launch offers, so it runs on usage credits." 출시 프로모션은 원문: "from launch on November 9, 2026 through November 16, 2026 at 11:59 pm PT"다. 한국 시간으로 11-17 16:59에 끝난다. 그 뒤 Pro·Team 표준 좌석에서 Fable 5.2를 고르면 사용 크레딧이 빠진다. 기본 모델을 Opus 5.5로 두는 편이 안전하다. [Claude 가이드](https://claude.com/resources/articles/using-claude-opus-5-5-as-your-daily-driver-for-knowledge-work) · [프로모션 도움말](https://support.claude.com/en/articles/17506494-claude-fable-5-2-launch-offers) · [The Decoder](https://the-decoder.com/anthropic-says-outdated-prompting-habits-are-slowing-down-claude-opus-5-5/)
- **WSJ: NVIDIA가 "순환 금융"으로 GPU 가격을 떠받쳤다(WSJ 11-13, HN 11-14 22:26 UTC 430점대):** Wccftech(11-13 19:23 UTC) 원문: "Kress pushed to rein in the company’s “circular financing” commitments to smaller cloud providers and major AI firms such as OpenAI and Anthropic, according to The Wall Street Journal." 직전 판 창 안의 보도인데 두 판 모두 다루지 않았다. WSJ와 HN에 올라온 TechSpot 본문은 읽지 못했다. [Wccftech](https://wccftech.com/jensen-huang-financed-ai-startups-absorb-excess-silicon/) · [HN](https://news.ycombinator.com/item?id=50501631)
- **Fable 5.2가 "코드로 그린 그림"(Anthropic 직원 X 11-09, The Decoder 11-15 15:42 UTC):** 이미지 생성 모델이 아니라 코드로 그림을 그린 사례다. 원문: "for the past few weeks, I’ve been exploring painting with Fable 5.2 in code." [X](https://x.com/jkeatn/status/2119874007382299087) · [The Decoder](https://the-decoder.com/anthropics-claude-fable-5-2-doesnt-generate-images-it-paints-them/)

## 기술 이슈

주말 창이라 arXiv 공지가 없고, HF Daily Papers 11-14·11-15 목록도 비어 있다. CISA KEV 추가와 전역 GHSA 검토 게시는 모두 0건이다. AI 저장소 101곳의 보안 권고는 이미 고쳐진 판에 대한 Dify 1건뿐이다. 주 소스는 HN 상위 글과 주말 매체 보도였다. 그 과정에서 직전 판들이 놓친 공식 발표 두 건(FBI의 ARTEX 경보, Uber의 Fireworks 피해)이 드러나 이 영역 끝에 묶었다.

### 1. Lovable로 만든 앱 다수가 로그인 없이 개인정보를 내보낸다는 연구가 HN 1위권에 올랐다. Lovable 보안 책임자라고 밝힌 계정이 창 안에 답했다 (보안업체 연구 11-02 공개, HN 재제출 11-14 17:43 UTC 930점대)

연구 자체는 2주 전(Wayback 첫 사본 11-02 15:46 UTC, Handelsblatt 보도 11-02)에 나왔다. 지난 판들은 다루지 않았다. 창 안에 HN 1위권으로 다시 올라왔고, 회사 측 답변이 붙었다.

- **조사 방법과 규모:** 독일 보안업체 Crissy Field가 Common Crawl에서 Lovable 프로젝트를 찾아 분석했다. 원문: "We examined 159,041 Lovable projects and found many cases where personal and confidential information was accessible without logging in." 원문: "Of these, 34,545 used Supabase." 원문: "We found records containing email addresses in 10,560 tables across 7,403 of the 24,664 websites (30%)."
- **사례:** 원문: "More than 200,000 chat messages were also accessible." 한 서비스 업체에서는 여권 번호가 든 신원 기록 1,000건 이상이 나왔다.
- **저자가 단 한계:** 원문: "The 30% figure should therefore not be read as a confirmed data leak rate." 원문: "We could not establish whether any of it had been misused."
- **Lovable 측 답변(커뮤니티, 신원 미검증):** HN 계정 `igorandri`가 Lovable 보안 책임자라고 밝혔다. 계정은 11-14 22:40 UTC에 새로 만들어졌다. 원문: "The root cause in these apps was insufficent Row-Level Security, which is exactly what our scanning is built to catch."(오타 원문 그대로) 원문: "soon we will have gates against publishing of anything with Critical issues in it, available to all tiers." Lovable 블로그에는 창 안 공식 글이 없다.

함정: 원인은 Supabase 테이블의 행 수준 보안(RLS) 누락이다. Supabase가 10-30에 바꾼 기본값은 새 테이블에만 적용된다. 공지 원문: "Existing tables are not affected in your project, they keep their current grants and stay reachable." 그 전에 만든 Lovable·Supabase 앱이 있다면 테이블마다 RLS와 `anon` 권한을 직접 점검한다.

중요한 이유: 바이브 코딩 플랫폼은 DB 연동 코드가 같은 틀로 생성된다. 연구진 원문: "With Lovable, database integrations follow recurring patterns, making it easier for attackers to look for similar misconfigurations across many applications." 한 앱에서 통한 방법이 수천 개 앱에 그대로 통한다.

[Crissy Field](https://www.crissyfield.de/research/lovable-en/) · [Lovable 측 HN 답변](https://news.ycombinator.com/item?id=50502105) · [Supabase 공지](https://github.com/orgs/supabase/discussions/45329) · [HN](https://news.ycombinator.com/item?id=50499017)

### 2. (FAA 장애 후속) 교통부 장관은 구리선을 광케이블로 바꾸던 중 라우터가 다른 시스템에 연결 요청을 쏟아 냈다고 밝혔다. 근본 원인 판정은 아직이다 (공식 + 매체보도, The Verge 11-14 17:44 UTC, HN 90점대)

직전 판 기술 4번에서 "AI 원인설을 공식 부인했다"고 적은 11-13 미국 전국 지상 정지의 후속이다.

- **장관 설명(The Verge):** 원격 FAA 시설의 정기 보수 중에 생긴 일이다. 원문: "He said that it could have been faulty hardware, a misconfigured router, or “incompetence” on the part of contractor L3Harris." 장관이 든 수치는 원문: "over 1,000 connection attempts every 4 to 5 seconds"다.
- **L3Harris 성명(날짜 11-14):** 원인을 인정하는 문구는 없다. 원문: "We are supporting the FAA in its investigation to understand the conditions that led to the Nov. 13 ground stop of our nation’s air travel." 전환 작업은 원문: "has completed more than 70% of the planned airspace communications conversion from legacy copper wires to a fiber optic system"이라고 했다.
- **반론(Common Dreams 11-14 21:39 UTC):** 관제사 60명과 이야기했다는 기자의 말을 전했다. 원문: "None of them are buying what he said".

중요한 이유: 장비 교체 중 라우터 한 대가 전국 관제망을 멈췄다. 사흘 전 전국으로 넓힌 AI 교통 흐름 도구 SMART와의 관련은 부인됐지만, 하드웨어 결함인지 설정 오류인지는 아직 공식 판정이 없다.

[The Verge](https://www.theverge.com/transportation/1033898/faa-ground-stop-sean-duffy-faa-copper-wire) · [L3Harris](https://www.l3harris.com/newsroom/press-release/2026/11/l3harris-statement-nov-13-ground-stop) · [Common Dreams](https://www.commondreams.org/news/duffy-ground-stop-explanation) · [HN](https://news.ycombinator.com/item?id=50501844)

### 3. 전역 GHSA 검토 게시가 40시간째 0건이다. NVD를 거쳐 들어온 AI 관련 권고는 2건이다 (공식 데이터 직접 셈)

- **검토(reviewed):** 창 안 0건이다. 마지막 게시는 직전 판 3번에서 적은 11-14 00:08 UTC(NLTK)다. 조회 시점(11-15 16시대 UTC)까지 약 40시간 멈춰 있다. 창 안에 나온 저장소 권고도 전역 DB에 없다.
- **미검토(unreviewed) 86건 중 AI 관련 2건:**
  - **MCP Server for WordPress 2.0.3 이하(CVE-2026-125771, high 7.2, 11-15 15:30 UTC):** 권한 상승이다. Patchstack 원문: "A low-privilege user can become an admin and gain full control." 2.0.4에서 고쳤다.
  - **vLLM 0.30.0·0.31.0(CVE-2026-129356, low, 11-15 03:30 UTC):** 다른 요청의 KV 접두사로 디코딩되는 버그다. 원 이슈 제목 원문: "with Cascade attention + `SimpleCPUOffloadConnector`, a request is decoded against another request's KV prefix!" 권고는 0.32.0(10-18)으로 올리라고 한다. cascade attention을 걷어 낸 PR이 들어간 판이다.
- **악성(malware) 11건:** 이름에 mcp·agent·llm·claude·openai·gemini가 들어간 것은 0건이다. PyPI 거래소 사칭 5건과, 정상 npm 패키지의 특정 판 3건이 악성으로 찍혔다.
- **저장소 권고:** Dify GHSA-6j8g-rp8v-55vr(medium, 11-14 18:08 UTC)는 `X-Forwarded-For` 위조로 IP별 속도 제한을 우회하는 문제다. 영향 범위가 `<= 1.17.1`이고 1.18.0(10-23)에서 고쳤으니 늦은 공개다.

함정: 전역 검토가 멈춘 동안 Dependabot은 새 권고를 알리지 못한다. 직전 판 3번의 미반영 목록(vLLM, MCP Inspector·레퍼런스 서버, LiteLLM, GPUStack, VS Code, n8n, MLflow)은 그대로다. 해당 프로젝트를 쓴다면 저장소 보안 권고를 직접 본다.

[MCP Server for WordPress 권고](https://github.com/advisories/GHSA-w68m-3rvm-5xxq) · [vLLM 권고](https://github.com/advisories/GHSA-fj8v-85jv-jwpv) · [vLLM 이슈 #61673](https://github.com/vllm-project/vllm/issues/61673) · [Dify 권고](https://github.com/langgenius/dify/security/advisories/GHSA-6j8g-rp8v-55vr)

### 4. 짧게

- **한국 세무사 시험 수석이 AI 부정행위를 자백했다(BBC 11-15 13:37 UTC):** 원문: "I used a microcamera to photograph the test questions and sent them to an AI then checked my answers on my smartphone to commit the cheating." 원문: "The 34-year-old scored 397.5 out of 400 points". 시험장 기기 통제가 AI 시대에 맞춰 바뀌어야 한다는 사례다. [BBC](https://www.bbc.com/news/articles/ckvg2ezvpe08o)
- **논쟁 "All AI-generated code should be Open Source"(HN 11-15 14:46 UTC, 240점대):** 원문: "This may be a hot take, but I strongly believe that all code generated by AI should be Open Source." 근거는 원문: "purely AI -generated code is not written by a human, meaning it should not be copyrightable"이다. HN 상위 반론은 저작권이 없으면 오픈소스 라이선스도 성립하지 않는다는 것이다. [블로그](https://jakub.jankiewicz.org/blog/ai-code-open-source/) · [HN](https://news.ycombinator.com/item?id=50507303)
- **해설: Raschka "Understanding Positional Embeddings in Modern LLMs"(11-15 12:10 UTC, HN 90점대):** RoPE, partial RoPE, NoPE를 PyTorch 구현과 함께 정리했다. 원문: "So, in my opinion, partial RoPE is hardly worth it." [글](https://magazine.sebastianraschka.com/p/understanding-positional-embeddings) · [HN](https://news.ycombinator.com/item?id=50506376)
- **미 국방부 CDAO가 "Super Intelligence Office"로 이름을 바꿨다(SecurityWeek 11-15 12:51 UTC):** 원문: "The Pentagon on Friday renamed its Chief Digital and Artificial Intelligence Office (CDAO) the Super Intelligence Office". [SecurityWeek](https://www.securityweek.com/pentagon-renames-cdao-super-intelligence-office/)
- **Pulse 6.9.0-beta.1(11-14 22:17 UTC):** 직전 판 5번의 AI 비서 승인 권한 권고 뒤 권한을 더 조였다. 원문: "Assistant file and log reads, Kubernetes logs, Docker update scans and agent checks during action previews now require permission to execute actions, including the appropriate API token scope." 베타라 안정판은 6.8.0이다. [릴리스](https://github.com/rcourtman/Pulse/releases/tag/v6.9.0-beta.1)
- **TSMC 직원 정보 유출 후속(BleepingComputer 11-15 15:23 UTC):** 11-14 판의 6-K 건이다. 원문: "The actor claimed the file was a sample of a 6.9GB dataset containing 46,000 employee records." [BleepingComputer](https://www.bleepingcomputer.com/news/security/tsmc-employee-data-breach-linked-to-cloud-provider/)
- **Vim 9.2.1346 이전(oss-sec 11-14 18:41 UTC, Low):** PowerShell 스크립트에서 `K` 키워드 조회로 명령이 실행된다. 원문: "A crafted line in a PowerShell script can therefore still execute arbitrary commands when the user selects it and presses `K`." AI 키워드는 없지만 에이전트가 받아 온 스크립트를 Vim으로 열어 보는 흐름에 걸린다. [oss-sec](https://seclists.org/oss-sec/2026/q4/708)
- **직전 판 후속:**
  - Cursor 최종 보고: Wayback 사본 4개와 현재 본문이 같다. 창 안 조용한 수정은 없다. 직전 판이 인용하지 않은 책임 문장은 원문: "The key was exposed by Fireworks"다. Fireworks 보안 공지는 원문: "Last updated November 9, 2026" 그대로다. [Cursor](https://cursor.com/blog/security-incident-update) · [Fireworks](https://fireworks.ai/security/bulletins)
  - Zread CLI: 악성 권고 7건이 철회되지 않았고 Zhipu 해명도 없다. 함정: 공식 사이트는 여전히 원문: "You can install it with npm or Homebrew."라고 안내한다. 지금 npm으로 설치하면 npm이 바꿔 넣은 빈 보안 패키지가 깔린다. [zread.ai](https://zread.ai/)
  - wfx-loot: 창 안 탐침 PR은 1건이다. 다만 PR 3분 전에 만든 저장소에 11-10 작성 커밋을 올린 것이라, 공격 재개로 볼 근거는 없다. 열린 탐침 PR은 GitHub 검색 기준(근사치) 5만 7천여 개 그대로다.
  - CISA KEV: 창 안 추가 0건. Check Point CVE-2026-123039와 UnrealIRCd CVE-2026-114713의 조치 기한은 오늘(11-16)이다. [KEV](https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json)
  - 새 진전 없음: VulnCheck CVE 통계, IBM ContextForge(최신 v1.1.1), Jet Admin, Flowise 3.1.4 SSRF(여전히 미수정), `@claudecode/cli`·Jelou CLI, Anthropic 안전 평가 글, Weight Space Misfolding, PostgreSQL CVE-2026-6460 매니지드 서비스 패치 현황, Apify 사후 보고서(약속 기한 11-20 무렵), Notion, Blackwell GPU 지문(저장소 커밋 1개·이슈 0개), Microsoft 365 사후 보고서, AMD SEV-SNP, CVE-2026-98432 Ubuntu focal·bionic·xenial(여전히 작업 중), patchtheplanet.org.
  - AI 키워드가 없는 전역 건: Node.js·Python·OpenSSL·Go·커널 보안 릴리스는 창 안에 없다. NVD로 Cyrus SASL SRP 인증 우회(CVE-2026-129483, 9.1)가 들어왔지만 원 공개는 10월이다.

### 참고: 창 경계 항목

- **(직전 판 정정) FBI가 ARTEX를 FLASH 경보로 냈다(PDF 11-13 17:57 UTC, HN 11-15 01:53 UTC):** 직전 판은 "Cairn·ARTEX·Silent Lynx 새 진전 없음"이라고 적었다. 실제로는 그 창 안에 FBI FLASH-20261113-001이 나와 있었다. 원문: "ARTEX is an open-source, large language model (LLM)-driven agentic platform that threat actors have weaponized to automate multiple phases of cyber intrusions at machine speed, from reconnaissance through data exfiltration." 원문: "with some intrusions progressing from initial targeting to privileged access in under three hours." 원문: "As of early November 2026, researchers identified over 1,600 publicly exposed ARTEX console instances". 공격자가 쓰는 모델도 적었다. 원문: "Analysis of attacker-controlled ARTEX instances shows Chinese models are most prevalent, with DeepSeek Flash family models dominating usage due to their balance of capabilities, relatively low cost, and lack of observable guardrails when framed for penetration testing." 공급망 사례로는 원문: "a supply chain compromise affecting ConnectWise Cloud Backup customers that enabled access to more than 2,250 downstream tenants."를 들었다. ConnectWise 앱을 설치한 적 있는 M365 테넌트는 경보의 권고 항목을 확인한다. [FBI FLASH](https://www.ic3.gov/CSA/2026/261113.pdf) · [HN](https://news.ycombinator.com/item?id=50503214)
- **(직전 두 판 누락) Uber도 Fireworks 침해 피해자였다(TechCrunch 11-13 14:57 UTC, THN 11-15 08:22 UTC 재보도):** 원문: "TechCrunch has learned that the provider is Fireworks AI". Uber 원문: "The notices to drivers are in relation to a security incident impacting one of our third-party vendors, Fireworks.ai, which had access to some Uber data," 규모는 원문: "Uber did not say how many customers are affected by the breach, nor how many drivers it was notifying about the incident." 지금까지 확인된 Fireworks 하류 피해는 Notion, Cursor, Uber다. [TechCrunch](https://techcrunch.com/2026/11/13/uber-confirms-hackers-stole-thousands-of-drivers-license-information-in-data-breach/) · [THN](https://thehackernews.com/2026/11/stolen-fireworks-ai-key-led-to-cursor.html)
- **훔친 Claude API 키로 Claude Code를 원격 조종 임플란트로 바꾼 실험(Darktrace, Help Net 11-12 11:00 UTC):** 공격자는 먼저 개발자 기기를 장악해 Claude 자격 증명을 얻어야 한다. 그 뒤 memory stores로 에이전트에 작업을 보내고, Files API로 파일을 빼낸다. 트래픽은 모두 `api.anthropic.com`으로 가서 평범한 코딩 에이전트처럼 보인다. 원문: "The technique does not rely on a vulnerability in the Claude platform, and the company has not seen it used in the wild." 키 종류에 따라 갈린다. 원문: "here a standard API key reaches memory stores, while OAuth credentials don’t". API 키를 노출한 적이 있으면 교체하고, memory stores에 낯선 항목이 없는지 본다. [Help Net](https://www.helpnetsecurity.com/2026/11/12/stolen-claude-api-keys-remote-controlled-implant/)
- **"고정했다"는 MCP 서버의 의존성이 5주 동안 15번 바뀌었다(Corelight, Help Net 11-13 05:30 UTC):** 원문: "ran with 15 different sets of dependencies over five weeks, with its configuration untouched." 원문: "Pinning a package’s version doesn’t necessarily pin the packages it depends on." `npx`·`uvx`로 MCP 서버를 띄운다면 잠금 파일까지 고정한다. [Help Net](https://www.helpnetsecurity.com/2026/11/13/corelight-mcp-server-supply-chain/)
- **미국이 북한의 개발자 겨냥 작전(FaceStealer)을 막았다(BleepingComputer 11-14 15:15 UTC, 창 시작 45분 전):** 원문: "including deepfakes and ClickFix". 원문: "the hackers compromised the maintainer of a popular open-source JavaScript library with tens of millions of weekly downloads". [BleepingComputer](https://www.bleepingcomputer.com/news/security/us-disrupts-north-korean-campaign-behind-400-million-crypto-theft/)
- **Claude가 강의 노트 쪽번호를 고치다 강의계획서와 1년치 노트를 지웠다(Tom's Hardware 11-06, HN 11-14 17:51 UTC 40점대):** 원문: "with updating the page numbers in an old set of lecture notes". 원문: "While the files were later recovered via Dropbox". 모델은 원문: "it is unclear which model he was using"이다. 에이전트에 파일 작업을 맡길 때는 백업이 있는 폴더에서 돌린다. [Tom's Hardware](https://www.tomshardware.com/tech-industry/artificial-intelligence/rogue-claude-ai-tasked-with-renumbering-pages-accidentally-deletes-law-professors-entire-syllabus-one-year-of-lecture-notes-youre-right-to-be-angry-i-deleted-your-files-through-my-own-carelessness-humbled-agent-admits) · [HN](https://news.ycombinator.com/item?id=50499114)
- **논문 "Latent Feedback Is a Compute and Data Amplifier"(alphaXiv 11-13):** 원문: "At most, latent feedback saves 48% of the compute and 47% of the unique data." 측정은 60M ~ 271M 파라미터 모델로 했고, 큰 모델 수치는 외삽이다. [alphaXiv](https://www.alphaxiv.org/abs/2611.latent-feedback-transformers-scaling-law)

## 써볼 만한 도구

주말이라 주요 코딩 에이전트는 창 안에 새 릴리스가 없다. Claude Code(`latest` 2.1.325), Codex CLI, Gemini CLI, Copilot CLI, OpenCode, goose, Cursor 앱, Zed, VS Code 모두 그대로다. 미국 3사와 GitHub·Vercel·Cloudflare 변경 로그에도 창 안 항목이 없다. 대신 오픈소스 쪽에서 의미 있는 갱신이 나왔다. 아래 도구는 하나도 설치하거나 실행하지 않았다. 함정은 릴리스 본문, PR 본문, README에서 읽은 것이다.

### 1. llama.cpp b12406: Inkling 2 Small을 GGUF로 돌린다. 실제 체크포인트 변환은 아직 검증 전이다 (공식, PR 머지 11-14 23:51 UTC, 릴리스 11-15 09:27 UTC)

- **무엇:** 11-14 판에서 다룬 Thinking Machines의 594B MoE Inkling 2 Small(텍스트·비전)을 변환하고 실행하는 코드가 들어왔다. PR 원문: "This adds pure Inkling 2 support (conversion + model), *without* the DSpark speculator that will be a separate PR stacked on top of this."
- **추천 이유:** 11-14·11-15 판에서 적은 Hopper GPU의 vLLM 오류가 아직 풀리지 않았다(HF 토론 #2, vLLM PR #67325 모두 열림). vLLM 말고 돌릴 길이 처음 생겼다. 릴리스 원문: "Packed synthetic GGUF weights match the independent pinned vLLM decode bit-for-bit."
- **함정:**
  - 같은 릴리스 원문: "Real checkpoint conversion remains untested."
  - 이미지 입력을 쓰려면 이미지 토큰 위치를 고친 b12413(11-15 12:38 UTC) 이상을 받는다.
  - 커뮤니티 GGUF인 unsloth 저장소 README 원문: "Inkling 2 Small uses a new `inkling2` architecture, so it needs a llama.cpp build with Inkling 2 support." 이 GGUF가 업스트림 b12406과 맞는지는 확인하지 못했다.
- **설치/사용:** [b12406 릴리스](https://github.com/ggml-org/llama.cpp/releases/tag/b12406) · [b12413](https://github.com/ggml-org/llama.cpp/releases/tag/b12413) · [PR #32217](https://github.com/ggml-org/llama.cpp/pull/32217) · [unsloth GGUF](https://huggingface.co/unsloth/Inkling-2-Small-GGUF)

### 2. Qwen Code 0.27.4: Auto 모드가 "관련 있는 작업인가"가 아니라 "사용자가 허락했나"로 판정한다 (공식, GitHub 11-15 07:04 UTC, npm 07:28 UTC, Apache-2.0)

- **무엇:** Alibaba의 터미널 코딩 에이전트다. 릴리스 원문: "AUTO permission checks now judge explicit user authorization, especially for destructive, discarding, shared-history, or bulk actions, rather than mere relevance." 근거가 된 PR #16858 원문: "Relevance to the user's goal is not authorization."
- **추천 이유:**
  - 위험한 작업 판정이 조여졌다. 원문: "Auto Mode now requires explicit authorization for risky actions, reviews classifier timeouts, avoids HTTP(S) URL filesystem lookups, and visibly marks Full Access."
  - 의존성 취약점도 고쳤다. PR #17105 원문: "The #17099 P1 findings include cross-origin credential forwarding, a conditional sanitizer bypass, and a compressed-body request that terminates the daemon." MCP SDK를 1.32.2로 올렸다.
- **함정:** 릴리스는 원문: "No known breaking changes."라고 적었다. 그래도 Auto 모드에서 삭제·일괄 작업에 확인 요청이 늘 수 있다. 무인 스크립트로 돌린다면 올린 뒤 한 번 점검한다.
- **설치/사용:** `npm i -g @qwen-code/qwen-code@0.27.4` · [릴리스](https://github.com/QwenLM/qwen-code/releases/tag/v0.27.4) · [PR #16858](https://github.com/QwenLM/qwen-code/pull/16858) · [PR #17105](https://github.com/QwenLM/qwen-code/pull/17105)

### 3. turbo-browser-mcp: 제출·결제 버튼을 기본으로 막는 브라우저 MCP 서버. 한국 포털의 "다음 → 확인" 흐름도 따로 다룬다 (커뮤니티, Show HN 11-15 04:09 UTC, MIT)

- **무엇:** Playwright 기반 브라우저 MCP 서버다. 변경된 부분만 보내는 DOM 스냅샷, 폼 한 번에 채우기, 디스크 경로로 파일 올리기를 지원한다.
- **추천 이유:**
  - 에이전트가 실수로 제출하는 일을 막는다. README 원문: "Final actions resembling `Submit`, `Publish`, `Send`, `Post`, `Apply`, `Confirm`, or payment triggers are blocked unless `allowSubmit: true` is explicitly passed or `--allow-submit` is enabled at startup."
  - 한국 사이트 사례를 README에 적었다. 원문: "On many Korean portals the last step reads 다음 and the real commit is a 확인 inside a modal that opens afterwards."
- **함정:**
  - README에는 `npx -y turbo-browser-mcp` 설정 예시도 있지만, 조회 시점에 npm 레지스트리에 이 이름이 없다(404). 누가 이름을 먼저 차지하면 남의 패키지가 실행된다. 저장소를 받아 README의 로컬 경로 설정(`node .../src/index.mjs`)으로 쓴다. GitHub 릴리스와 태그도 없다.
  - 기본 연결 방식이 이미 로그인된 브라우저에 붙는다. 원문: "`auto` (default): probes `--cdp http://127.0.0.1:9222` first so agents can reuse an existing signed-in browser session, then falls back to launching Chromium." 원격 디버깅 포트를 열어 둔 개인 브라우저가 있으면 에이전트가 그 세션을 쓴다.
  - 네트워크 가드는 기본으로 꺼져 있다. 원문: "It is off by default because multi-step wizards often `POST` on every "Next"." 벤치마크는 자체 스크립트로 잰 값이다.
- **설치/사용:** [GitHub](https://github.com/Tthanh2k6/turbo-browser-mcp) · [HN](https://news.ycombinator.com/item?id=50503930)

### 4. Cursor TypeScript SDK 1.4.0: 실행 중인 클라우드 에이전트에 `run.steer()`로 지시를 끼워 넣는다 (공식 패키지, npm 11-15 03:26 UTC, 릴리스 노트 미게재)

- **무엇:** 1.3.1과 1.4.0의 npm README를 비교하면 "Steer a running cloud run" 절이 새로 생겼다. 원문: "`run.steer(text)` sends the text into that run instead, and the run takes it at its next safe step".
- **추천 이유:** 클라우드 에이전트를 멈추고 새로 시작하지 않고, 도는 중에 방향을 바꿀 수 있다.
- **함정:** README 원문: "A cloud `steer()` is never retried, and it can throw". 원문: "After a `NetworkError` the outcome is unknown, so it is not retryable". 네트워크 오류 뒤 다시 보내면 지시가 두 번 들어갈 수 있다. 조회 시점 Cursor SDK 변경 로그 페이지에는 1.4.0 항목이 없다. 라이선스는 독점이다.
- **설치/사용:** `npm i @cursor/sdk@1.4.0` · [npm](https://www.npmjs.com/package/@cursor/sdk) · [SDK 변경 로그](https://cursor.com/docs/api/sdk/changelog)

### 5. (후속) browser-use 1.0.5가 PyPI에 올라왔다. 설치하면 실행마다 판정 요청 하나가 기본으로 붙는다 (공식, PyPI 11-15 03:39 UTC)

직전 판 도구 5번에서 "PyPI 배포 대기"로 적은 버전이다. 직전 판에서 본 함정 그대로다. 릴리스 원문: "Every run is judged by your own model: one more low-effort request checks the run against the task". 끄려면 원문: "`judge=False` turns it off"다. 비용을 정확히 맞춰야 하는 파이프라인은 버전을 1.0.4에 묶거나 `judge=False`를 넣는다.

[릴리스](https://github.com/browser-use/browser-use/releases/tag/1.0.5) · [PyPI](https://pypi.org/project/browser-use/1.0.5/)

### 짧게

- **(출시 전) Claude Code 내장 mod `sec-default`·`privacy` 소스가 바뀌었다(창 안 커밋 16건, 11-14 18:49 ~ 11-15 06:33 UTC):** 10-03 판에서 다룬 Mods의 조직용 기본 가드다. 개인 mod가 권한 확인 전에 모델의 도구 호출을 대신 실행하지 못하게 막는다. 원문: "A mod's own effects open no dialog and ask no classifier. So a person's mod may not use one to carry out a call the model sent until that call is allowed." privacy mod는 사용자 등급 플러그인의 계정 정보 조회를 거부한다. 원문: "- `user`: refused. The call resolves with `deny`." 적용 범위는 원문: "Outermost, on a machine with managed settings or a managed-mcp.json, or for a Team or Enterprise organization"이다. 창 안에 새 npm 버전이 없어, 어느 버전부터 실릴지는 확인하지 못했다. 조직에서 직접 만든 mod를 쓴다면 다음 릴리스 노트를 본다. [sec-default README](https://github.com/anthropics/claude-code/blob/0cb75cebcdfd767ee14cd06295f9bf4163274e79/mods/sec-default/README.md) · [privacy README](https://github.com/anthropics/claude-code/blob/0cb75cebcdfd767ee14cd06295f9bf4163274e79/mods/privacy/README.md)
- **(설정 함정) Claude Code 샌드박스 `denyRead`만 걸면 그 폴더에 여전히 쓸 수 있다(커뮤니티 이슈 #110227, 11-15 15:57 UTC, 직원 답 없음):** 원문: "A folder inside the working directory that is listed only in `sandbox.filesystem.denyRead` stays writable from sandboxed Bash commands." 공식 문서도 읽기·쓰기와 도구별 규칙을 따로 막도록 안내한다. 문서 원문: "A denyRead entry doesn’t stop the Read tool, and allowedDomains doesn’t limit WebFetch". 폴더를 보호하려면 `denyWrite`와 `permissions.deny`의 Read·Edit 규칙을 함께 건다. [이슈](https://github.com/anthropics/claude-code/issues/110227) · [문서](https://code.claude.com/docs/en/sandboxing)
- **Alibaba Open Code Review(GitHub 일간 트렌딩, v1.13.10 11-15 02:56 UTC, Apache-2.0):** README 원문: "Open Code Review is an AI-powered code review CLI tool." 자사 벤치마크는 정밀도를 우선한다. 원문: "Note that its Recall is lower than general-purpose agents — a deliberate trade-off favoring precision over noise." 새 도구는 아니지만 지난 판들에서 다룬 적이 없다. [GitHub](https://github.com/alibaba/open-code-review)
- **llama.cpp b12416: Ripple-65B-Preview의 DFlash2 추측 디코딩(PR 머지 11-15 11:36 UTC, 릴리스 15:39 UTC):** PR 원문: "I saw Recursive Ripple 65B has DFlash2 support for SGLang and I decided to add support to llama.cpp." 작성자가 M5 Max에서 잰 값은 초당 11.9토큰에서 35.4토큰이다(재현 안 함). [PR #31904](https://github.com/ggml-org/llama.cpp/pull/31904)
- **db-branch(Show HN 11-15 13:50 UTC, Apache-2.0):** README 원문: "Instant database branches for MySQL and PostgreSQL, on your own servers." 원문: "**Status: Beta.**" ZFS가 필요하다. 직전 판의 Gnomon(PostgreSQL 전용)과 같은 용도로, 에이전트에 실제 데이터 사본을 줄 때 쓴다. [GitHub](https://github.com/db-branch/db-branch)
- **Kiri(HN 11-15 04:40 UTC, 70점대):** Docker Desktop을 대체하는 컨테이너 런타임이다. 사이트 원문: "docker run takes 22 ms on Kiri and 178 ms on Docker Desktop (152 ms on Colima, 279 ms on OrbStack), every engine on 2 CPUs and 8 GB."(자사 측정), "Free, and in beta on Mac, Windows and Linux." 라이선스는 확인하지 못했고 HN 댓글에는 의심도 있다. [runkiri.com](https://runkiri.com/) · [HN](https://news.ycombinator.com/item?id=50504077)
- **Gawain(Show HN 11-15 04:39 UTC, MIT):** 변수와 클래스 속성까지 타입 표기를 강제하는 Python 린터다. README 원문: "ruff's `ANN` rules and flake8-annotations ask for annotations on functions only." 에이전트가 쓴 코드에 타입을 강제하는 CI 단계로 쓸 만하다. [GitHub](https://github.com/matheuzgomes/gawain) · [HN](https://news.ycombinator.com/item?id=50504072)
- **소규모 릴리스:** LiteLLM 1.108.9 LTS(11-14 22:00 UTC, 백포트만. 예: "send reasoning_effort to Foundry Kimi K3 and keep its reasoning_content"), langchain-core 1.8.1(11-15 14:57 UTC, LangSmith 게이트웨이 컨텍스트별 지정), Agweb v0.3.0(11-14 23:18 UTC, 원문: "`close-all` is marked dangerous in help, and the agent skill closes only its own workspace."). [LiteLLM](https://github.com/BerriAI/litellm/releases/tag/v1.108.9) · [langchain-core](https://github.com/langchain-ai/langchain/releases/tag/langchain-core%3D%3D1.8.1) · [Agweb](https://github.com/paul-nameless/agweb/releases/tag/v0.3.0)
- **회귀 보고(직전 판 도구 후속):**
  - llama.cpp V100 prefill 저하(#32245): 보고자가 원인 빌드를 좁혔다. 원문: "I have successfully pinpointed this regression to commit 12285." 유지보수자 답은 아직 없다. V100에서 긴 컨텍스트를 쓰면 b12285 이전 빌드에 머문다. [댓글](https://github.com/ggml-org/llama.cpp/issues/32245#issuecomment-6893152949)
  - OpenCode #63879(관리형 권한이 프로젝트 권한에 덮어쓰이는 문제): 외부 기여자가 원문: "The fix keeps the managed documents as the final layer in `ConfigAgentPlugin`"라며 PR을 예고했다. 릴리스는 2.3.0 이후 없다. [댓글](https://github.com/anomalyco/opencode/issues/63879#issuecomment-6893754990)
  - Claude Code 2.1.325: 샌드박스 Bash 자동 허용 해제와 관련한 새 회귀 보고는 없다. `subagentStatusLine` 복구 예고("Monday's release")는 창 이후다.
- **직전 판 후속 중 새 진전 없음:** Claude Agent SDK(Python 0.2.182·TS 0.3.325), Code Mode 문서, Self-hosted Managed Agents, Claude Meetings, `claude-plugins-official`·`anthropics/skills`·`openai/plugins`(창 안 커밋 0건), Claude Data 기본 켜짐(11-20 그대로), FastMCP 4.2.0, Cloudflare Kitesurf, Antigravity, Copilot CLI 1.0.112, Vercel `ai@7.0.173`, langchain-openai 1.8.0, goose 1.59.1(macOS 데스크톱 빌드 여전히 없음), Codex CLI 0.169.0, Gemini CLI 0.68.0, Crush 0.101.0, Pi, Mistral Vibe, Amp, Zed, VS Code 1.146.0, Ollama, vLLM 0.33.0, LM Studio, transformers 5.23.0, n8n, Google GenAI SDK, OpenAI Agents SDK, Supabase MCP, Open WebUI, Dify, DSPy, Context7, Chrome DevTools MCP, Playwright MCP, GitHub·MongoDB·Sentry·Neon MCP, MCP 레퍼런스 서버·SDK·Inspector, MCP 명세, Conductor(예고한 업데이트 아직 없음), Val Town(12-10 그대로), Gnomon. 주요 AI 도구 저장소 15곳의 보안 권고는 창 안 0건이다.

## 주목할 점

- **AI로 빨리 만든 것의 보안은 기본값과 사용자 점검에 달려 있다.** Lovable 앱 노출은 생성된 DB 연동에서 행 수준 보안이 빠진 것이 원인이었다. Supabase의 새 기본값은 기존 테이블에 소급되지 않는다. Corelight는 "고정한" MCP 서버의 의존성이 5주 동안 15번 바뀌었다고 했고, Darktrace는 개발자 기기를 장악한 공격자가 훔친 API 키로 Claude Code를 원격 조종할 수 있음을 실험으로 보였다. 그런데 Dependabot 알림의 원천인 전역 GHSA 검토는 40시간째 멈춰 있다. 공격 쪽은 FBI 경보대로 값싼 모델로 침투를 자동화하고 있다. 이번 주에는 내가 만든 앱의 RLS, MCP 서버의 잠금 파일, 노출된 API 키를 직접 확인하는 편이 안전하다.
- **다가오는 일정:**
  - 이번 주: CISA KEV 2건(Check Point·UnrealIRCd) 조치 기한과 Claude Code `subagentStatusLine` 복구 예고("Monday's release", 11-16). Fable 5.2 출시 프로모션 종료와 Anthropic 텍스트 워터마크(11-16 23:59 PT = 11-17 16:59 KST). NVIDIA 실적 발표와 Anthropic 호주 개인 청약 마감(11-17). Anthropic 공모가 확정과 유럽 청약 마감(11-18), 거래 시작(11-19). Enterprise의 Claude Data 기본 켜짐과 Apify 사후 보고서 약속 기한(11-20 무렵).
  - 그다음 주: ERNIE 5.5 Flash Preview 가중치 공개 약속(11-25 전), Qwen4-Max-Preview 종료(11-25 16:00 UTC), Gemini 4 Carbon 도입가 종료(11-30).
  - 12월: Dependabot 비활성 저장소 알림 중지(12-08), 영국 Copilot 테넌트 Claude 기본 켜짐(12-09, 보도 기준), Gemini 스마트 안경 판매와 Val Town 읽기 전용(12-10), ChatGPT 옛 음성 모델 은퇴(12-11), GitHub Copilot 모델 6종 은퇴(12-14), Inception Mercury API 종료(12-28).
  - 그 뒤: Vertex의 Hydrogen 캐시 쓰기 과금(2027-01-12), Claude Meetings 통화 중 문서 무료 종료(2027-02-10), 다음 MCP 명세(2027-03-30).

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - WSJ(Megaspeed 120억 달러, 순환 금융, FAA 라우터 위치, Anthropic 창업자 지분, Moonshot 구조 재편)는 유료벽이라 The Decoder·Wccftech 인용과 제목으로만 봤다. Moonshot IPO 구조 재편 보도는 본문과 수치를 확인하지 못해 싣지 않았다.
  - FT·Bloomberg는 RSS 제목·요약만 봤다. TechSpot(순환 금융, HN 430점대)은 Cloudflare에 막혔다. Reuters Megaspeed 기사는 링크만 확인했다.
  - FBI FLASH PDF, alphaXiv, Pandaily는 r.jina.ai로 읽었다. fbi.gov 개별 페이지와 OpenReview API는 막혔다.
  - x.com은 개별 게시물만 fxtwitter로 읽었다. 타임라인을 볼 수 없어 각 랩 직원의 주말 발표, Zhipu·Fireworks·Lovable 공식 계정 반응을 놓쳤을 수 있다. reddit은 막혀 있다.
- **시각이 날짜만 있거나 불확실한 원문:**
  - Anthropic Opus 5.5 가이드(날짜만 11-12), L3Harris 성명(날짜만 11-14, HN 22:51 UTC로 보완), Jankiewicz 블로그(RSS 날짜 11-13), Patchstack 권고(11-13), Lovable 연구(Wayback 첫 사본 11-02 15:46 UTC로 보완), Altimeter 서한 X 원글("Friday"만 확인).
  - Cursor 보고 페이지는 창 시작 전 본문이 든 Wayback 사본이 없어 창 안 사본끼리만 대조했다.
- **확인하지 못한 것:**
  - HN `igorandri`가 실제 Lovable 보안 책임자인지(창 안에 새로 만든 계정), Lovable 사례의 실제 악용 여부.
  - Agnes AI의 회사 정체와 공식 발표, Agnes 2.5 Pro Theta의 원격 코드 내용.
  - unsloth Inkling 2 GGUF와 llama.cpp b12406의 호환성, Claude Code mod 소스 변경이 실릴 버전, Kiri의 라이선스.
  - FAA 장애의 근본 원인, 특정 판만 악성으로 찍힌 npm 패키지 3건과 wfx-loot의 연관성.
  - 재현하지 않은 것: 모든 벤치마크·속도·비용 주장(MathArena, Agnes, turbo-browser-mcp, Kiri, Ripple DFlash2, Latent Feedback, Open Code Review), Lovable 집계, ARTEX 노출 수. 도구는 하나도 설치하거나 실행하지 않았다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 10점 이상 스토리 66건은 전부 봤고, Show HN 97건은 도구 영역에서 전부 봤다. 그 아래는 키워드로만 걸렀다.
  - 전역 GHSA: reviewed 0건, unreviewed 86건, malware 11건을 전부 목록으로 봤다. AI 저장소 101곳의 저장소 권고를 직접 조회했다.
  - arXiv는 이번 창에 공지가 없었다(토요일 공지 없음, 일요일 공지는 11-16 01:00 UTC). HF Daily Papers 11-14·11-15 목록도 비어 있었다.
  - Hugging Face는 주요 조직 약 90곳의 `lastModified`, 추가 약 80곳의 `createdAt`, 공개 예정 저장소 직접 조회로 확인했다. 전역 `createdAt` 스캔은 약 5천 건까지만 봤고 ModelScope는 보지 않았다.
  - Gemini 앱 릴리스 노트는 10-13 이후 항목이 없어 캐시가 묵은 것으로 보인다. Vertex AI 릴리스 노트(캐시 동결), Claude Platform 릴리스 노트 HTML(묵은 사본으로 보임)은 일 단위로 판정할 수 없었다. Anthropic 뉴스 RSS는 빈 응답이었다.
  - 보안 벤더 RSS 약 40곳은 창 안 글이 0건이었다(주말).
  - 창이 끝날 때 열려 있던 것: Claude iOS Sign in with Apple 장애, OpenAI Compliance API 비용 데이터 지연(monitoring), vLLM Inkling 2 Small 지원 PR.
