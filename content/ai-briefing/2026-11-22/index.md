---
title: "2026-11-22 AI 브리핑"
date: 2026-11-22T08:00:00+09:00
tags: [ai-briefing, anthropic, openai, math, security]
description: "Lean·Mathlib 공동체가 Claude로 만든 리만 가설 Lean 증명을 공식 인증했고(GRH는 Dirichlet L-함수까지), OpenRouter는 MiniMax 모델로 보낸 프롬프트가 제3자에게 읽혔을 수 있다고 알렸으며, OpenAI는 GPT-6.2가 짧게 답하고 되묻는 경향에 맞춘 프롬프팅 가이드를 냈다."
---

> 조사 범위: 2026-11-21 08:00 ~ 2026-11-22 08:00 KST(2026-11-20 23:00 ~ 11-21 23:00 UTC, 미국 금요일 15:00 ~ 토요일 15:00 PST). 직전 브리핑(11-21 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·`article:modified_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`·`merged_at`·커밋 시각, npm·PyPI 게시 시각, 저장소·전역 GHSA `published_at`, Red Hat CSAF `initial_release_date`, Debian 보안 추적기, OpenRouter `created`, Yahoo Finance 시세, HTTP `last-modified`, Wayback 캡처, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-21 23시대 UTC) 값이다.

## 오늘의 핵심 요약

- **AI가 만든 리만 가설 증명이 공식 인증을 받았다.** Lean·Mathlib 공동체는 공동 성명에서 Alpöge의 Lean 증명이 "맞다"고 했다. 진술이 Mathlib의 리만 가설 정의와 정확히 같다는 것도 확인했다. GRH는 Dirichlet L-함수까지만 검증했다. 공식 판정 도구 comparator의 실행은 아직 끝나지 않았다. Anthropic은 여전히 공식 논평이 없고, 쓰인 모델 이름도 공개되지 않았다.
- **추론 공급망에서 프롬프트가 샜다.** OpenRouter는 고객 메일에서 MiniMax 모델로 보낸 요청을 제3자가 읽었을 수 있다고 알렸다. 재판매 업체를 거친 요청도 마찬가지다. MiniMax 엔드포인트의 ZDR 표시는 내렸다. 바탕이 된 Dragos 연구에서는 ZDR 요청도 새 나갔다. 에이전트 쪽에서는 악성 웹페이지 하나로 Meta Muse 에이전트를 장악한 연구가 나왔다.
- **OpenAI는 GPT-6.2 사용법을 고쳐 썼다.** 기본적으로 짧게 답하고, 다음 단계를 제안한 뒤 기다리는 경향이 있으니 프롬프트를 바꾸라는 내용이다. 독립 벤치마크 SimpleBench에서는 GPT-6.2 Sol이 전 세대보다 낮게 나왔다. 도구 쪽에서는 Cline CLI가 한 번 실행하는 호출의 기본 모드를 바꿨다. Inkling 2 Mini 로컬 실행은 llama.cpp b12611 이상이 필요하다.

## 모델 소식

주말 창이라 조용했다. 미국 3사의 새 모델, 가격 변경, 상태 페이지 장애가 없었다. 대형 랩의 HF 신규 업로드도 없다. 가장 큰 소식은 기술 이슈 1번의 리만 가설 증명 인증이다. Anthropic은 창 안에서도 공식 논평을 내지 않았고, 쓰인 모델 이름도 공개되지 않았다. [BBC](https://www.bbc.co.uk/news/articles/c6pqr2g7w059o)(11-20 23:08 UTC)는 출처 없이 원문 "it was found using a non-public version of Anthropic's Claude"라고 썼다. 모델 쪽 새 소식은 OpenAI가 GPT-6.2 사용법을 고쳐 쓴 문서와, 독립 벤치마크에서 GPT-6.2가 전 세대보다 낮게 나온 결과 정도다. 공식 변경 로그는 Claude 앱(모바일 새 디자인)만 움직였다. 그대로인 곳은 Claude Platform(11-17), OpenAI API(11-19), ChatGPT(11-20), Gemini API·AI Studio(11-19), Gemini 앱(11-17), xAI(11-19), DeepSeek(11-13), Mistral, Z.ai다.

### 1. OpenAI GPT-6.2 마이그레이션 가이드에 프롬프팅 절이 생겼다. GPT-6.2는 기본적으로 짧게 답하고, 다음 단계를 제안한 뒤 기다리는 경향이 있다 (공식 문서, 개정 시각 미확인, 매체보도 13:32 UTC)

- **무엇이 바뀌었나:** 가이드가 출시 뒤 다시 쓰였다. 새 절 원문: "The model's writing tends to be more concise and direct by default. When migrating, revisit instructions like "be concise" in your system prompt." 원문: "When the user's intent is unclear, the model may propose a next step and wait for confirmation." 이를 줄이는 권장 문구도 실었다. 원문: "Only stop to clarify with the user if there is significant change in task scope, you are concretely blocked, have a decision that the user needs to resolve, or before irreversible or destructive actions."
- **추론 항목 제거 문장은 표현만 바뀌었다:** 지금 원문은 "Starting with GPT-6.2, the Responses API drops reasoning items after mid-conversation edits to the conversation history (prefix edits), certain model changes (including downgrades), or changes to the end user or organization."이다. 11-18 판이 인용한 문장("…will drop reasoning items when it detects…")은 사라졌지만, 조건(prefix 수정, 모델 하향, 최종 사용자·조직 변경)은 같다.
- **함정:** 원문: "When migrating Chat Completions requests, omit `temperature`, `top_p`, `logprobs`, and `top_logprobs`." 기존 시스템 프롬프트에 "간결하게"가 들어 있다면 답이 지나치게 짧아질 수 있다.
- **왜 중요한가:** 모델의 기본 성향이 바뀌었다는 것을 OpenAI가 문서로 인정한 셈이다. The Decoder는 이를 짧은 답과 불필요한 질문을 고치는 팁으로 소개했다. 원문: "OpenAI has released new prompt templates for GPT-6.2 Astra and Sol covering writing style, initiative, and coding". 에이전트 하네스를 GPT-6.2로 옮길 때 프롬프트를 다시 본다.

[OpenAI 가이드](https://developers.openai.com/api/docs/guides/latest-model.md) · [The Decoder](https://the-decoder.com/openai-shares-new-prompting-tips-for-gpt-6-2-astra-and-sol-to-fix-brief-answers-and-unnecessary-questions/)

### 2. (커뮤니티 벤치마크) SimpleBench: GPT-6.2 Sol 69.9%로 GPT-6.1 Sol 82.9%보다 낮다. 1위는 Claude Fable 5.2(94.9%), 2위는 Gemini 4 Neon(94.4%)이다 (커뮤니티, HN 13:16 UTC 150점대)

- **수치(리더보드 데이터 직접 확인):** Fable 5.2 94.9%, Gemini 4 Neon 94.4%, GPT-6.1 Sol 82.9%, GPT-6.2 Astra Pro 79.2%, GPT-6.2 Astra 77.3%, GPT-6.2 Sol 69.9%다. 사람 기준선은 83.7%다.
- **함정:** 기준선 원문: "based on our small sample of nine participants". 개인이 운영하는 벤치마크다. HN 반론 원문: "The title misrepresents it. Those models are 4 percent below human baseline." GPT-6.2는 Wayback 11-10 사본에 없고 11-21 13:18 UTC 사본에 있다. 정확한 갱신 시각은 모른다.
- **왜 중요한가:** OpenAI 자체 벤치마크(직전 판 MedChemBench)는 GPT-6.2가 앞섰다고 했다. 외부 상식 추론 벤치마크에서는 전 세대보다 낮게 나왔다. 1번의 "짧게 답하는" 기본 동작과 관련이 있을 수 있지만, 인과는 확인되지 않았다.

[SimpleBench](https://simple-bench.com/) · [HN](https://news.ycombinator.com/item?id=50588916)

### 3. 짧게

- **Anthropic 상장 후속:** 창 안 SEC 제출은 없다(종결 8-K 미제출). 금요일 시간외 마지막 가격은 $87.79다(정규 종가 $88.91, 공모가 $83). CNBC 주간 정리(13:00 UTC) 원문: "$1.6 trillion fully diluted market cap put it behind just six U.S. companies". GraniteShares는 ANT 2배 레버리지·인버스 ETF를 냈다(20:17 UTC). 원문: "seek daily investment results corresponding to 200% and -200%, respectively, of Anthropic's daily stock performance". 함정: The Decoder(10:27 UTC)의 원문 "Anthropic fell 9.1 percent a day after its market debut"는 시세와 맞지 않는다. 실제 하락률은 -8.5%다. [The Decoder](https://the-decoder.com/us-technology-stocks-suffer-their-worst-weekly-loss-since-the-pandemic-as-ai-doubts-pile-up/) · [CNBC](https://www.cnbc.com/2026/11/21/anthropic-ipo-tech-selloff.html) · [GraniteShares](https://finance.yahoo.com/markets/options/articles/graniteshares-introduces-2x-long-2x-201700018.html)
- **Claude 모바일 새 디자인(릴리스 노트, 11-20 날짜로 창 안에 추가):** 원문: "A refreshed look for Claude Mobile is rolling out gradually, starting with Claude for iOS." 원문: "note that the new look needs iOS 26 or later". [릴리스 노트](https://support.claude.com/en/articles/12138966-release-notes)
- **OpenRouter 신규 등록:** Inkling 2 Mini(02:19 UTC, 도구 2번)와 DaisyRose 27B(00:49 UTC)다. Inkling 2 Mini의 두 제공자는 모두 FP4 양자화로 서빙한다.
- **Gemini 앱(Android):** 카메라 화면이 바뀌고 GIF 생성이 생겼다(9to5Google 20:52 UTC). [9to5Google](https://9to5google.com/2026/11/21/gemini-camera-redesign/)
- **직전 판 후속:**
  - 여전히 401: Qwen4-Max, Qwen4-Max-Preview(11-25 종료), Ling 3.2 Flash, ERNIE 5.5 Flash Preview(11-25 전 공개 약속), LG K-EXAONE 2.5. FLUX.1-schnell은 여전히 gated다.
  - Gemini 4 Neon 안전 보고서는 찾지 못했다. GPT-6.2 시스템 카드는 창 안 변경이 없다.
  - Cursor(중국 모델 퇴출 검토), NVIDIA–Inception, NVIDIA SEC 조사, 캘리포니아 AG 소환장, BIS 규칙, Apple macOS 27.3 에이전트 제한은 재보도만 있다.
  - Hy4는 TechNode의 "Apache 2.0" 오보가 고쳐지지 않았다. transformers PR #50903도 열려 있다.
  - OpenAI가 증류 글에서 예고한 원문 "We plan to share more soon about recent campaigns"의 후속 글은 없다. [OpenAI](https://openai.com/index/combatting-adversarial-distillation)
  - 소식 없음: DeepSeek 사용 제한·V5, Anthropic 30일 공개 정책, 'Mythos' 상표 소송, Edge0, Agnes, Kimi K3.1 Pro, LongCat-2.5, ChatGPT Wallet, 한국 랩 모델 발표.
  - Enterprise Claude Data 기본 켜짐은 문서 문구가 여전히 원문 "turns on by default on November 20, 2026"이다. 실제 전환은 확인하지 못했다. [Claude](https://claude.com/resources/articles/sheets-whiteboards-data)

### 참고: 창 경계 항목

- **(직전 창, 문서 갱신 11-20 22:33 UTC) Microsoft가 12-09 Claude 기본값 변경을 공식 문서에 적었다. 범위는 영국만이 아니라 EU·EFTA·영국이다:** 지금까지 이 브리핑은 The Register 보도를 따라 "영국 Copilot 테넌트"로 적었다. 원문: "Starting on December 9, 2026, the following setting changes are taking effect". 원문: "If you haven't previously made any selection, your selection will change to All users". 범위 원문: "The information in this article applies only to customer in the European Union (EU), European Free Trade Association (EFTA), and United Kingdom (UK)." 이유 원문: "These changes are being made to simplify the controls available to admins as part of Claude models from Anthropic coming into the EU Data Boundary starting in December 2026." 아무것도 고르지 않은 테넌트만 바뀌는 것도 아니다. 원문: "The All users for Microsoft 365 apps only option will no longer be available. For customers who have this option selected, or who haven't selected an option, Claude models will be enabled for all users." 상위 문서에는 원문 "customers in the UK have Claude models disabled by default"가 아직 남아 있다. EU·영국 테넌트 관리자는 12-09 전에 설정을 명시한다. [Microsoft 문서](https://learn.microsoft.com/en-us/copilot/implement/copilot-anthropic-apps) · [상위 문서](https://learn.microsoft.com/en-us/copilot/implement/connect-to-ai-subprocessor)
- **(11-18 사본부터 있음) OpenAI "Keep the prefix unchanged" 가이드:** 과거 판에 없던 문서다. 원문: "A prefix change modifies that earlier history—for example, by shortening a tool result, rewriting a developer message, or replacing older turns with a summary." 원문: "Compaction changes the prefix, so cache misses are difficult to avoid in these cases." 대화 중에 `reasoning.effort`·`tools`를 바꿔도 prefix가 바뀐다. 문서는 대신 `configuration_update`·`additional_tools` 항목을 덧붙이라고 권한다. 직전 판들이 다룬 GPT-6.2 캐시 함정의 공식 설명서다. [OpenAI 가이드](https://developers.openai.com/api/docs/guides/keep-prefix-unchanged.md)

## 기술 이슈

직전 판의 리만 가설 증명 주장이 하루 만에 공식 인증으로 바뀌었다. Lean·Mathlib 공동체 성명은 HN 740점대로 창 최고 스토리다. 보안 쪽에서는 추론 공급망 문제가 이어졌다. OpenRouter가 MiniMax 모델로 보낸 프롬프트를 제3자가 읽었을 수 있다고 고객에게 알렸다. 에이전트 쪽에서는 악성 웹페이지 하나로 Meta Muse 에이전트를 장악한 연구가 나왔다. 이미지 업로드를 받는 AI 앱이 Ghostscript를 거쳐 뚫릴 수 있다는 분석도 나왔다. 주말이라 CISA KEV 추가는 없고 arXiv 공지도 없다. 전역 GHSA 창 안 게시분은 reviewed 15건(모두 늦은 미러), unreviewed 357건, malware 6건이다(직접 셈).

### 1. Lean·Mathlib 공동체가 Alpöge의 리만 가설 Lean 증명을 "맞다"고 인증했다. GRH는 Dirichlet L-함수까지만 확인했고, 공식 판정 도구 실행은 아직 끝나지 않았다 (공식 공동 성명 13:55 UTC + 검증자 문서, HN 14:36 UTC 740점대)

- **성명(원문):** "We have full confidence that Alpöge's proof of the Riemann Hypothesis is correct, and we congratulate him on this achievement." 원문: "We hereby certify that both the statement and the proof of the Riemann Hypothesis are correct." Lean FRO와 Mathlib Initiative가 지지했다. 성명에는 Claude나 Anthropic이라는 말이 나오지 않는다.
- **직전 판의 두 질문에 대한 답:**
  - 명제를 제대로 옮겼나: 공동체가 Mathlib만 불러오는 진술 파일을 따로 만들어 두 도구로 대조했다. 원문: "both confirming that the theorem proved in Alpöge’s library is exactly Mathlib’s statement of the Riemann Hypothesis."
  - Lean 커널은 건전한가: 원문: "The checkers con-leche and con-ron come with a formal proof, relative to set theory, that they cannot accept a false proof, thus addressing concerns that Lean’s logic itself might have flaws that the proof could exploit." 악성 코드 우려에는 원문 "These checks ran on an inert export of the proof, out of reach of hypothetical malicious code in the Lean proof sources."라고 답했다.
- **검증 세부(MIT Andrew Sutherland 페이지, "Page last updated 2026-11-21 12:55 UTC"):** 원문: "Andrew Sutherland (MIT), checks run with Claude Code." 175,187개 모듈이 모두 컴파일됐다. 공리는 `propext`·`Classical.choice`·`Quot.sound` 세 개뿐이다. 독립 커널 nanoda·con-leche와 lean4lean이 증명을 받아들였다. 리만 가설과 동치인 Robin·Lagarias 부등식도 zeta 함수가 나오지 않는 형태로 따로 뽑아 확인했다.
- **아직 남은 것(직접 대조):**
  - 원문: "The lake comparator run and the Lean-kernel replay of the export are still in progress." 공동체 저장소 노트(마지막 검증 커밋 19:45 UTC) 원문: "Comparator's output will be added here when the run finishes."
  - 성명은 con-ron도 검사했다고 썼지만, con-ron이 받아들인 기록은 찾지 못했다. Lean 4.36 번들판 con-leche·con-ron은 원문 "declined exit 2, not a rejection: those older binaries do not support one reflexive inductive type in the proof."였다.
  - 프로젝트가 고정한 Lean 4.33.1만으로는 부족하다는 지적도 노트에 있다. 원문: "For this reason a pass by the 4.33.1 kernel alone, which is what a successful build is, counts as limited evidence here."
- **GRH 범위(원문):** "Scope. "GRH" here means Dirichlet L-functions: all moduli and all characters. Dedekind zeta functions, Hecke L-functions and automorphic L-functions in general are not among the statements that were checked." Alpöge가 말한 더 넓은 일반화는 검증 대상이 아니다.
- **단서(원문):** "The build ran on cloud machines without a sandbox". 원문: "It relies on those kernels and on Mathlib's definitions being what they are intended to be."
- **사람이 읽을 해설:** IHES Robin Zhang이 예비 노트를 냈다(파일 갱신 22:34 UTC). Claude로 만든 증명을 GPT로 풀어 쓴 문서다. 원문: "I prompted GPT 6 Pro to digest the Lean code on Alpöge’s website, infer a human-readable mathematical proof". 원문: "I cannot claim that this summary is a faithful translation of the Lean code". Alpöge가 공개하지 않은 해설도 있다. 원문: "Alpöge [Alp26, p. 2, footnote 2] describes an AI-generated exposition of more than 250,000 pages that he chose not to release."
- **Anthropic 입장:** 창 안에서도 공식 논평이 없고, 쓰인 모델 이름도 공개되지 않았다(모델 소식 도입부).
- **그 밖:** Navier–Stokes 증명의 인증서는 아직 공개되지 않았다(저장소 마지막 push 11-20 13:33 UTC). Clay 연구소 반응은 찾지 못했다. "Re: Riemann"(HN 11-20 23:42 UTC 210점대)은 발표문 어조를 비판한 에세이다.
- **왜 중요한가:** 사람이 읽을 원고 없이 기계 검증만으로 밀레니엄 문제 하나가 공동체 인증을 받았다. 검증 도구는 Claude Code로 돌렸고, 사람이 읽을 해설은 GPT 6 Pro로 만들었다. 남은 것은 단일 스레드 커널의 최종 판정, con-ron 결과, 그리고 수학자가 이해할 수 있는 원고다.

[Lean 성명](https://leanprover-community.github.io/blog/posts/riemann-hypothesis/) · [Sutherland 검증 페이지](https://math.mit.edu/~drew/grh/) · [검증 노트](https://github.com/leanprover-community/riemann-hypothesis/blob/main/verification/NOTES.md) · [IHES 노트](https://www.ihes.fr/~rzhang/files/GRH%20note.pdf) · [Guardian](https://www.theguardian.com/technology/2026/nov/21/anthropic-riemann-hypothesis-solved-claude-ai) · [Re: Riemann](https://borretti.me/article/re-riemann) · [HN 성명](https://news.ycombinator.com/item?id=50589618) · [HN Sutherland](https://news.ycombinator.com/item?id=50590441)

### 2. OpenRouter 고객 메일: MiniMax 모델로 보낸 프롬프트를 제3자가 읽었을 수 있다. 재판매 업체를 거친 요청도 마찬가지이고, ZDR 표시는 내렸다 (HN에 옮겨진 메일 04:31 UTC + Ars 업데이트, 공식 공개 페이지 없음)

- **메일 원문(HN 게시자가 옮긴 것, 원문 "it appears authentic"):** "A security researcher placed unique test URLs in prompts sent to MiniMax models from July to October 2026. A third party accessed those URLs 40 to 130 hours after the requests were sent, whether MiniMax served a request directly or another provider forwarded it to MiniMax." 원문: "AtlasCloud, Novita and StreamLake have told us they forwarded OpenRouter requests for MiniMax models to MiniMax." 원문: "We have asked MiniMax for its own account and have not received one." 원문: "While we investigate, we have removed the Zero Data Retention label from these MiniMax-model endpoints."
- **매체 확인:** Ars Technica가 기사에 업데이트를 붙였다(수정 20:51 UTC). 원문: "Update, Nov 21: Posts on both Hacker News and X report that OpenRouter has sent customers an email notifying them that some of their prompts may have been read."
- **바탕 연구(창 경계, Dragos 11-19 13:00 UTC, 직전 판들이 놓침):** HD Moore가 7~11월에 약 200만 개 프롬프트에 추적용 URL과 AWS 미끼 키를 넣어 보냈다. 원문: "We observed activity indicating that data from at least three AI providers was exposed." 한 중국 랩에 대해서는 원문: "This lab’s systems fetched the secret URLs from more than half of the prompts that we sent to its models between July and October, including roughly two-thirds of the requests marked as “no data collection” or Zero Data Retention." Dragos는 랩 이름을 밝히지 않았다. MiniMax라고 이름을 적은 쪽은 OpenRouter 메일이다. 둘이 같은 사건이라고 Dragos가 확인한 것은 아니다.
- **같은 흐름(창 경계, Prime Intellect 11-19 공지, 직전 판 미수록):** 원문: "an unauthorised party replaced a deployment behind Prime Intellect’s deepseek/deepseek-v4.1-flash endpoint with an abliterated copy". 기간은 11-08 01:24 ~ 11-10 06:35 UTC다. 원문: "Refusals fell from 98 of 100 prompts to 1 on JailbreakBench, from 304 of 313 to 2 on StrongREJECT, and from 198 of 240 to none on HarmBench." 라우터 뒤의 모델이 몰래 바뀔 수도 있다는 사례다.
- **할 일:** 7~10월에 OpenRouter로 MiniMax 모델을 썼다면 메일을 확인한다. 그 기간 프롬프트에 넣은 키·URL은 폐기한다. 라우터의 ZDR 표시만 믿지 말고 제공자를 직접 지정한다. MiniMax 공식 입장은 찾지 못했다.
- **왜 중요한가:** 프롬프트에 비밀을 넣는 코딩 에이전트 사용자에게 직접 영향이 있다. ZDR 표시가 붙은 경로, 미국 재판매 업체를 거친 경로도 안전하지 않았다.

[HN 메일](https://news.ycombinator.com/item?id=50585962) · [Ars Technica](https://arstechnica.com/security/2026/11/a-leaky-ai-supply-chain-is-exposing-secret-user-data-at-an-alarming-rate/) · [Dragos](https://www.dragos.com/blog/ai-supply-chain-security-canary-tokens) · [Prime Intellect](https://www.primeintellect.ai/security/advisories/deepseek-v4-1-flash-model-substitution)

### 3. NebuSec: 악성 웹페이지 하나로 Meta Muse 에이전트의 대화·메모리를 승인 없이 빼냈다. 공유 커널 결함으로 사용자 VM root까지 갔다 (발견자 원문 11-20 날짜·첫 Wayback 01:49 UTC, THN 11:53 UTC)

- **무엇(원문):** "In the end, a single request to export a malicious web page gave us command execution in the runtime container, a return channel that did not prompt the user, and access to the same user’s data and credentials outside the container." 공격자 페이지를 PDF로 저장해 달라는 요청 하나로 시작한다. 원문: "When a user asked Muse to save an attacker-controlled page, a single renderer bug, worth about $500, was enough to execute arbitrary shell commands inside the runtime container with the agent’s privileges." 다른 사용자의 VM이나 호스트로는 넘어가지 않았다.
- **원인(원문):** "Muse updates its bundled browser from time to time, but our snapshots showed it lagging behind Chrome releases." 원문: "Linux 7.0 fell within the published affected ranges of multiple CVEs."
- **수정(원문):** "By the time of publication, the Muse team was aware of every finding described here and had patched all exploits described in this post." THN 원문: "Meta has not publicly confirmed the fixes or commented on the work, and the write-up does not say whether any user data was accessed before the patches went in."
- **왜 중요한가:** 격리를 여러 겹으로 설계한 에이전트도 번들 브라우저와 커널 패치가 늦으면 무너진다. 브라우저를 쓰는 에이전트 샌드박스를 운영한다면, 브라우저와 커널을 업스트림 속도로 갱신하는지 점검한다.

[NebuSec](https://nebusec.ai/research/inside-meta-muse/) · [THN](https://thehackernews.com/2026/11/a-single-malicious-webpage-let.html)

### 4. Upload2Shell(Ghostscript CVE-2026-125396): 이미지 업로드를 받는 AI 앱도 Pillow를 거쳐 Ghostscript에 닿는다 (발견자 원문, oss-security 19:09 UTC, HN 18:29 UTC 60점대)

- **무엇(원문):** "A use-after-free in Ghostscript’s Type 1 font loader lets a crafted document cross Ghostscript’s -dSAFER sandbox, alter native path controls, and execute attacker-chosen commands as the rendering process." 수정판은 Ghostscript 10.08.0(09-08)이다. CVE 공개는 11-09였고, 분석 사이트는 이번 창에 나왔다.
- **AI 도구 경로:** 발견자는 Gradio 6.32.0의 인증 없는 `gr.Image` 앱에서 경로를 확인했다. 원문: "HTTP preprocessing sent EPS bytes named .png through Pillow to gs -dSAFER". 확인한 것은 Ghostscript가 실행되는 데까지이고, 명령 실행까지 간 것은 아니다. InvokeAI·AUTOMATIC1111·ComfyUI는 소스 코드상 경로만 짚었다. 원문: "Image.open() alone only identified the file in our test. Loading pixels, cropping, or converting them started the renderer."
- **함정(원문):** "As of 12 November, Debian still listed Bookworm as vulnerable, Amazon Linux listed a pending fix, and several SUSE branches remained affected or in progress." RHEL 계열은 원문 "We did not establish whether current RHEL, Rocky Linux, AlmaLinux, Oracle Linux, or stable Fedora packages contain the .putgstringcopy fix."다.
- **할 일:** 이미지 업로드를 받는 AI 앱 호스트에서 Ghostscript를 올리거나 지운다. 또는 Pillow의 EPS 디코더를 막는다. 배포판 패키지 버전만 보고 안심하지 않는다.

[Upload2Shell](https://pwn.ai/upload2shell) · [기술 글](https://pwn.ai/upload2shell/research.html) · [oss-security](https://seclists.org/oss-sec/2026/q4/806) · [HN](https://news.ycombinator.com/item?id=50592097)

### 5. GPGME CVE-2026-129975: 긴 사용자 ID를 가진 OpenPGP 키로 double free가 난다. Ruby2ßhell 때문에 공개가 앞당겨졌다 (공식, 공지 11-20 18:01 UTC = 창 경계, 창 안 배포판 반영, HN 08:32 UTC 220점대)

- **무엇(원문):** "By using an OpenPGP key containing a crafted very long user id it is possible to trigger a double free." 원문: "Exploiting this bug to mount an Remote Code Execution attack (RCE) can't be ruled out." 2.3.0 미만 GPGME를 쓰는 소프트웨어가 대상이고, 원문에 "Most mail clients"가 들어 있다. 수정판은 2.3.1이다.
- **공개 시점(원문):** "due to the new Ruby2Shell bug GitLab had to update earlier and thus we also disclosed the GPGME bug on 2026-11-20."
- **창 안 후속:** Debian은 bookworm(DLA-4924-1)과 trixie(DSA-6646-1)를 고쳤다. RHEL에 대해서는 원문 "Red Hat Enterprise Linux 9 seems to have patched the double-free *5 years ago* when it was noticed by a Coverity scan"이라는 지적이 나왔다. CentOS Stream 9 패키지에는 2021년 Coverity 수정 패치(`gpgme-1.15.1-covscan.patch`)가 들어 있다. Red Hat의 공식 판정은 아직 없다.
- **함정(원문):** "For statically linked binaries remmeber to build them against GPGME 2.3.1." 패키지만 올려서는 정적 링크 바이너리가 고쳐지지 않는다.

[GnuPG 공지](https://lists.gnupg.org/pipermail/gnupg-announce/2026q4/000517.html) · [oss-security](https://seclists.org/oss-sec/2026/q4/805) · [THN](https://thehackernews.com/2026/11/gnupg-warns-critical-gpgme-flaw-could.html) · [HN](https://news.ycombinator.com/item?id=50587208)

### 6. 직전 판 후속과 정정 (공식, 직접 확인)

- **MONAI Label(직전 판 기술 2번):** PyPI는 여전히 0.8.5이고, 저장소 권고의 수정 버전 칸도 비어 있다. 창 종료 직전(22:56 UTC)에 메인테이너가 PR #1942를 열었다. 원문: "Follow-up to #1915, applying the same path confinement to the session store." 원문: "`Sessions.add_session(uncompress=True)` extracted uploaded archives with `shutil.unpack_archive()`, which writes tar member names as-is on Python < 3.14." 세션 업로드에 같은 계열의 경로 탈출이 하나 더 있었다는 뜻이다. 함께 병합된 #1941은 원문 "drops Python 3.10 support"다. 다음 수정판은 Python 3.10 환경에서 못 쓸 수 있다. [PR #1942](https://github.com/Project-MONAI/MONAILabel/pull/1942) · [PR #1941](https://github.com/Project-MONAI/MONAILabel/pull/1941)
- **Ruby2ßhell:** Veria 표는 "Updated 21 November 2026 at 03:52 UTC"로 갱신됐지만, GitHub Enterprise Server는 여전히 3.22.5 이하 Vulnerable이다. PoC 일정 문구(11-25, 대형 공급사 패치가 늦으면 연기)는 그대로다. Debian bookworm은 DLA-4926-1(ruby3.1 3.1.7-1~deb12u2)로 고쳤다. RHEL은 Hardened Images만 고쳤다(RHSA-2026:100837, 00:00 UTC). Ubuntu CVE 페이지는 여전히 `needs-triage`다. 악용 보고는 없다. [Veria](https://verialabs.com/ruby2shell/) · [DLA-4926-1](https://security-tracker.debian.org/tracker/DLA-4926-1)
- **정정(PHP):** 직전 판은 "RHEL 7~10 php는 모두 Affected"라고 썼다. 실제로는 RHEL 10 php8.4 모듈이 RHSA-2026:100821(11-20 21:26 UTC)로 이미 고쳐져 있었다. 이 RHSA에는 critical인 CVE-2026-115560이 들어 있다. RHEL 10 기본 php와 RHEL 7~9는 여전히 Affected다. Ubuntu USN은 아직 없다. [RHSA-2026:100821](https://access.redhat.com/errata/RHSA-2026:100821)
- **정정(nginx):** 직전 판은 NGINX2Root 후속에서 "RHEL 8·9는 아직"이라고 썼다. 실제로는 RHEL 9(RHSA-2026:100745·100746·100748)와 RHEL 8(RHSA-2026:100747)이 11-20 11:05~11:44 UTC에 나와 있었다. [RHSA-2026:100747](https://access.redhat.com/errata/RHSA-2026:100747)
- **OpenSSL CVE-2026-121481:** 4.0.4·4.1.1은 아직 나오지 않았다. PR #33652도 열려 있다.
- **CISA KEV:** 창 안 추가가 없다. 직전 판 4건의 조치 기한은 11-23이다.

### 7. 에이전트·AI 권고 (직접 확인)

- **FastMCP GHSA-78c8-3jhx-5mg7(medium):** OAuthProxy 인가 코드 중복 교환이다. 도구 4번에 정리했다.
- **Kimi Code CVE-2026-134006(low, unreviewed 21:30 UTC):** Plan 모드 권한 가드(`guardToolExecution`)를 우회한다. 영향 범위가 "up to 2.7.2"인데 npm `latest`가 2.7.2라 수정판이 없다. 원문: "The vendor was contacted early about this disclosure but did not respond in any way." 근거는 VulDB와 제보자 문서뿐이고 재현하지 않았다. Plan 모드를 안전장치로 믿지 않는다. [GHSA](https://github.com/advisories/GHSA-259v-2548-2q54)
- **Coder Agents CVE-2026-134363(low, 늦은 미러):** 악성 skill이 심볼릭 링크로 작업 공간 파일을 모델 컨텍스트로 흘린다. 2.38.5·2.37.9·2.36.12·2.34.17에서 고쳤지만, 2.35.x 줄은 수정 버전 칸이 비어 있다. [권고](https://github.com/coder/coder/security/advisories/GHSA-hm3r-4cg2-22xx)
- **ArcadeDB MCP CVE-2026-134324(medium, 늦은 미러):** Groovy를 쓴 Gremlin 스크립트가 READ로 분류돼 읽기 전용 MCP에서 쓰기가 된다. 26.11.1에서 고쳤다. [권고](https://github.com/ArcadeData/arcadedb/security/advisories/GHSA-6jr9-6vw5-xjf6)
- **심각도 불일치:** NVD발 미러에서 SurrealDB CVE-2026-134292는 critical 9.8이지만 저장소 권고는 high다. 스캐너 경보 등급이 저장소 판정과 다를 수 있다.
- **악성 패키지:** malware 6건 중 AI 테마는 없다.

### 8. AI 무관 대형 사고 (매체보도)

- **FBI 전국 범죄 데이터베이스(NCIC) 침해 보도:** CNN이 처음 보도했다(02:12 UTC). BleepingComputer 원문: "The intrusion was reportedly discovered in recent days, although it is unclear when the breach occurred, how the attackers gained access, or whether any information was stolen." 트럼프 대통령은 원문 "We just heard about it and I will say that it’s not good."이라고 했다(TechCrunch). The Verge 원문: "NBC first reported the USDA hack on Friday." [BleepingComputer](https://www.bleepingcomputer.com/news/security/fbi-reportedly-hacked-again-this-time-targeting-nationwide-police-database/) · [TechCrunch](https://techcrunch.com/2026/11/21/trump-says-reported-fbi-database-hack-is-not-good/) · [The Verge](https://www.theverge.com/tech/1039543/fbi-usda-hacked) · [HN](https://news.ycombinator.com/item?id=50587859)
- **Microsoft Entra Connect Health 결함이 실제로 악용됐다(BleepingComputer 18:01 UTC):** 고객 통지 원문: "On 20 November 2026, we concluded our investigation and determined that unauthorized read-only access to tenant-level metadata associated with your organization had occurred. We did not identify any unauthorized configuration changes". 관리자 이메일과 테넌트 ID가 노출됐다. 표적 피싱을 경계한다. [BleepingComputer](https://www.bleepingcomputer.com/news/security/microsoft-entra-connect-health-flaw-exploited-to-steal-tenant-data/)
- **Rackspace Spot 침해(BleepingComputer 22:59 UTC):** 원문: "On 17 November 2026 (about 00:20–22:00 UTC), an unauthorized party used leaked credentials to access parts of Rackspace Spot". 공격자가 Kubernetes Secrets를 읽을 수 있었다. 쓰던 고객은 Secret에 담긴 자격 증명을 모두 바꾼다. [BleepingComputer](https://www.bleepingcomputer.com/news/security/rackspace-spot-breach-let-hackers-access-customer-kubernetes-secrets/)

### 9. 짧게

- **논문:** arXiv 공지가 없는 창이고, HF Daily Papers 11-21분은 비어 있다. The Decoder가 다룬 ByteDance 학습 단계 논문은 원 논문이 11-02라 창 밖이다. NVIDIA 컨텍스트 창 논문은 원문을 찾지 못했다.
- **Wired "The Outages Are Just Getting Started"(13:00 UTC):** 원문: "So let’s just be clear: The incidents were definitely not caused by some sort of coordinated AI hacking spree and, taken individually, they all seem to have routine explanations." [Wired](https://www.wired.com/story/brace-yourself-for-years-of-outage-chaos/)
- **Dokploy 불완전 수정(oss-security 18:40 UTC):** 원문: "An incomplete fix of CVE-2026-72884." 서비스 생성 권한이 있는 멤버가 호스트 root 파일 시스템을 마운트할 수 있다. [oss-security](https://seclists.org/oss-sec/2026/q4/807)
- **(커뮤니티 논란) Backblaze 개인정보 처리방침(HN 03:23 UTC 230점대):** 12-01 시행이다. 행태 광고용 정보 공유 조항과 원문 "AI Processing Disclosure: We may employ artificial intelligence (AI) technologies to support and enhance our operations"가 들어갔다. [Backblaze](https://www.backblaze.com/company/policy/privacy) · [HN](https://news.ycombinator.com/item?id=50585593)
- **(시연) Super Nintendo에서 도는 "ChatGPT"(The Register 11:46 UTC):** 220만 파라미터짜리 증류 모델이다. 원문: "It is not a useful assistant: the model is millions of times smaller than the models behind ChatGPT, knows about 8,000 words, and often gets things wrong, sometimes confidently". [The Register](https://www.theregister.com/offbeat/2026/11/21/chatgpt-shrunk-down-to-run-on-a-super-nintendo/5311418)
- **(AI 무관) BADBOX 3.0(BleepingComputer 15:17 UTC):** 원문: "At its height, BADBOX 3.0 accounted for roughly 40 billion bid requests a day and more than 4% of all CTV traffic observed by the Human Defense Platform". [BleepingComputer](https://www.bleepingcomputer.com/news/security/badbox-30-botnet-returns-with-record-breaking-40-billion-daily-ad-requests/)
- **직전 판 후속 중 진전 없음:** Bell Canada("agentic"의 뜻을 여전히 설명하지 않음), EC ARTEX, Signal(Apple·Google 반응 없음), Java4Shell, Synology SA-26:21(권고 개정 없음), Apify 전체 보고서, Microsoft agent-framework·ONNX Runtime 저장소(여전히 404), AutoGPT 0.12.0 보안 권고, KVM CVE-2026-99312(Ubuntu "needed"), Windows FastUnlock, FBI 새 경보, AWS 보안 블로그. Node.js·Python·Ruby·PHP·Chrome·Go는 창 안 새 보안 릴리스가 없다. Help Net 금요일 주간 요약은 조회 시점까지 나오지 않았다.

### 참고: 창 경계 항목

- **(11-20 14:37 UTC, HN 11-20 23:29 UTC 130점대) 아이슬란드 호스팅사 1984 공격, 대표가 "AI 비용 수천만"이라고 했다(RÚV):** 원문: "These are professionals, and this was probably an enormously well-funded attack. We're talking about tens of millions in artificial intelligence costs. It happened at a speed unlike anything anyone has ever even heard of." 통화 단위와 근거는 기사에 없다. 증거가 공개되지 않은 대표의 주장이다. [RÚV](https://www.ruv.is/english/2026-11-20-serious-cyberattack-thought-backed-by-wealthy-operators-493411) · [HN](https://news.ycombinator.com/item?id=50583856)
- **(11-20 19:08 UTC) pip CVE-2026-133949(critical):** 원문: "pip prior to v26.2.2 the certifi bundled with pip contains the compromised eMudhra certificates." PyPI 최신 26.3은 해당하지 않는다. 오래된 가상 환경의 pip를 올린다. [oss-security](https://seclists.org/oss-sec/2026/q4/803)
- **(11-20 20:24 UTC) Cloudflare Ruby2ßhell WAF 규칙:** 원문: "Cloudflare has patched its own systems that use Ruby, and we have found no indication of compromise." [Cloudflare](https://blog.cloudflare.com/ruby2shell-waf-protection/)

## 써볼 만한 도구

주말 창이라 새 도구보다 업그레이드 함정과 보안 수정이 많았다. Cline CLI는 한 번 실행하는 호출의 기본 모드를 바꿨고, 실행 폴더의 `bunfig.toml` preload를 실행하던 동작을 막았다. 직전 판에서 "대부분 PR 단계"라고 적은 Inkling 2 Mini 로컬 실행은 llama.cpp·transformers 정식 릴리스로 들어왔다. 다만 초기 빌드 함정이 두 가지 있다. Claude Code는 새 버전이 없다. 도구는 하나도 설치하거나 실행하지 않았다. 함정과 인용은 릴리스 본문, PR·이슈 본문, README, 권고에서 직접 읽었다.

### 1. (기본 동작 변경·보안) Cline CLI 3.0.75: 한 번 실행하는 호출이 yolo 모드로 시작하고, 실행 폴더의 `.env`·`bunfig.toml`을 더는 읽지 않는다 (공식, 05:30 UTC)

- **무엇:** CLI 3.0.75, VS Code 확장 4.1.27, SDK 0.2.1, Desktop 0.0.50이 같은 날 함께 나왔다. 동작 변경은 CLI에 몰려 있다.
- **기본 모드 변경(릴리스 원문):** "One-shot runs (a prompt argument or piped stdin) now start in yolo mode instead of act mode, since nobody is at the keyboard to approve tools." 원문: "Yolo mode has no team tools, so pass `--act` when you need them". 빠지는 도구는 PR #15644에 있다. 원문: "no search, web fetch, apply-patch, skills, ask-question, sub-agents or teams". 영향 원문: "Existing scripts that call `cline --auto-approve true --json ...` without a mode flag pick this up. `--act` restores the old behavior." 도구 자동 승인을 꺼 둔 경우(`--auto-approve false` 또는 저장된 설정)나 모드 플래그를 준 경우에는 act 모드가 유지된다. 그러니 승인을 새로 건너뛰는 변경이라기보다 쓸 수 있는 도구 구성이 바뀌는 변경이다.
- **보안 수정(릴리스 원문):** "The CLI no longer reads `.env` or `bunfig.toml` from the directory it is launched in". PR #15781 원문: "By default such a binary reads `.env*` and `bunfig.toml` from the directory it is launched in, before any of our code runs." 원문: "on `main` the preload ran and the `.env`-supplied settings were applied; on this branch neither happened." 신뢰하지 않는 저장소 안에서 `cline`을 실행하면, 그 저장소의 preload 스크립트가 먼저 돌 수 있었다는 뜻이다. 저장소 보안 권고는 없고, 영향 버전 범위도 적혀 있지 않다.
- **그 밖:** 원문: "Removed the `CLINE_CAPTURE_*` debug variables, which wrote full provider request bodies to disk". 기본 모델도 바뀌었다. 원문: "With no saved model, the Cline provider now starts on the first free model instead of Claude Sonnet 5.5. Saved selections are untouched." 원문: "Default models change for 67 other providers, mostly to Kimi K3.1 Pro, Gemini 4 Neon (including the `gemini` and `vertex` providers) or DeepSeek V4.1 Flash 1017".
- **추천 이유:** 남의 저장소에서 CLI를 돌린다면 바로 올린다.
- **함정:** CI에서 모드 플래그 없이 `cline "..."`를 쓰면 검색·웹·서브에이전트 도구가 사라진다. `--act`를 명시한다. 실행 폴더의 `.env`에 기대던 설정은 셸에서 export한다. 모델을 저장해 두지 않았다면 비용과 품질이 달라진다.

[CLI 3.0.75](https://github.com/cline/cline/releases/tag/cli-v3.0.75) · [확장 4.1.27](https://github.com/cline/cline/releases/tag/v4.1.27) · [PR #15644](https://github.com/cline/cline/pull/15644) · [PR #15781](https://github.com/cline/cline/pull/15781) · 설치 `npm i -g cline`

### 2. (후속, 직전 판 도구 9번) Inkling 2 Mini 로컬 실행: llama.cpp·transformers 정식 릴리스가 나왔다. llama.cpp는 b12611 이상이어야 한다 (공식 릴리스 + 커뮤니티 이슈)

- **llama.cpp:** b12600(11-20 23:30 UTC)에 지원이 들어갔다. 그러나 루프 슬롯 버그가 있었고, b12611(19:12 UTC)에서 고쳤다. PR #32704 원문: "PPL for NVFP4 with scales (eg unsloth's) for inkling-2-mini are 130.43 (should be 7.6531)". 원문: "ggml quants are exactly as before so it does not affect them". b12611 이전 빌드로 Unsloth NVFP4 GGUF를 돌리면 오류 없이 품질이 무너진다.
- **transformers 5.24.0(12:28 UTC):** 원문: "Inkling 2 Mini is a multimodal model from Thinking Machines Lab that accepts text and image inputs and generates text outputs." 같은 판의 호환성 변경은 6번에 정리했다.
- **vLLM:** 지원 PR #69028이 16:19 UTC에 병합됐다. 정식 릴리스는 여전히 0.34.0이다. 직전 판에서 본 전용 이미지 버그 #69046(H100 커널 오류)에는 더 조용한 문제가 보고됐다. 창 안 댓글 원문: "image input is wrong on Hopper with this image, without any error." 원문: "Text-only requests are fine, which makes this easy to miss once the kernels of this issue are fixed."
- **Ollama:** MLX 러너 지원이 main에 병합됐지만 태그 릴리스는 없다(최신 v0.40.17). GGUF 경로는 아직이다. 협업자 답 원문: "Ollama will support GGUF inkling when https://github.com/ggml-org/llama.cpp/pull/32695 is merged". 이슈 #19826은 Ollama가 묶어 둔 llama.cpp가 b12257이라는 점을 짚었다. 원문: "Unsloth's files require the `#32704` loop-slot fix, otherwise they load but report ~130 perplexity."
- **그 밖:** Unsloth v0.1.922-beta(11-20 23:53 UTC)가 지원을 넣었다. SGLang PR #51926은 열려 있다. OpenRouter에는 02:19 UTC에 등록됐다(입력 $0.075, 출력 $0.30/100만 토큰, 컨텍스트 1,048,576).
- **할 일:** llama.cpp는 b12611 이상을 쓴다. vLLM 전용 이미지를 Hopper에서 쓴다면 이미지 입력 결과를 믿지 않는다.

[llama.cpp b12611](https://github.com/ggml-org/llama.cpp/releases/tag/b12611) · [PR #32704](https://github.com/ggml-org/llama.cpp/pull/32704) · [transformers 5.24.0](https://github.com/huggingface/transformers/releases/tag/v5.24.0) · [vLLM PR #69028](https://github.com/vllm-project/vllm/pull/69028) · [vLLM #69046](https://github.com/vllm-project/vllm/issues/69046) · [Ollama #19817](https://github.com/ollama/ollama/issues/19817) · [Ollama #19826](https://github.com/ollama/ollama/issues/19826)

### 3. (새 도구) LookAtMyQuant: Inkling 2 Mini를 5.38GB 파일로 줄인 양자화 컴파일러와 Apple Silicon 런타임 (커뮤니티, HN 18:02 UTC 300점대)

- **한 줄:** 텐서 조각마다 압축 손상을 재서 크기 예산 안에서 양자화 코드를 고르는 컴파일러다. MIT 라이선스다.
- **주장(README 원문):** "outscores Unsloth's 9.14 GB `UD-Q2_K_XL` on both GSM8K and HumanEval at 41% smaller size". 표 기준 원본 BF16은 GSM8K 147/200, HumanEval 66/164이고, 5.4GB 판은 134/200, 60/164다. 자체 측정이고 재현하지 않았다.
- **추천 이유:** Mac에서 Inkling 2 Mini를 6GB 안팎 메모리로 돌리는 가장 작은 경로다. 비교 수치를 공개했다.
- **함정:** HN 제목은 "in 5.4 GB Memory"지만 5.38GB는 파일 크기다. README의 메모리 수치는 원문 "`5.98 GB` active RAM"이다. 런타임은 원문 "custom Apple Silicon Metal kernels"라 Mac 전용이다. README 수치는 HN 게시 뒤에도 고쳐졌다.

[저장소](https://github.com/ShinMegamiBoson/lookatmyquant) · [HF 5.4GB](https://huggingface.co/shinmegamiboson/Inkling-2-Mini-LookAtMyQuant-5.4GB) · [HN](https://news.ycombinator.com/item?id=50591853) · 설치 `pip install git+https://github.com/ShinMegamiBoson/lookatmyquant.git`

### 4. (보안 수정) FastMCP 4.2.4: OAuthProxy가 인가 코드 하나를 여러 번 교환해 줬다. 기본 설정도 해당한다 (공식, 권고 05:31 UTC)

- **무엇:** GHSA-78c8-3jhx-5mg7(medium)이다. 영향 범위는 `>= 2.13.0, <= 4.2.3`이고 수정판은 4.2.4다. CVE는 없다. 권고 원문: "The default, out-of-the-box configuration is affected; no special backend is required." 원문: "An attacker who has intercepted a code and its PKCE verifier obtains a session that runs alongside the victim's; each session has its own token id, so revoking the victim's session or logging out does not end the attacker's." refresh token에도 같은 경쟁 조건이 있었고 함께 고쳤다.
- **함정:** 악용하려면 코드와 PKCE verifier를 가로채야 한다. 권고에는 이미 만들어진 중복 세션을 정리하는 방법이 없다. 의심되면 발급한 토큰을 직접 폐기한다.
- **추천 이유:** FastMCP OAuthProxy로 원격 MCP 서버를 운영한다면 바로 올린다.

[권고](https://github.com/PrefectHQ/fastmcp/security/advisories/GHSA-78c8-3jhx-5mg7) · [v4.2.4](https://github.com/PrefectHQ/fastmcp/releases/tag/v4.2.4)

### 5. (업그레이드 주의) LiteLLM 1.108.15 LTS: 키 없는 JSON 설정 제공자에게 `OPENAI_API_KEY`를 보내던 동작을 막았다. 대신 그런 배포는 401을 받는다 (공식, 21:35 UTC)

- **수정 전 동작(PR #50819 원문):** "With `OPENAI_API_KEY` set, keyless `parasail/...` and `chutes/...` calls send the OpenAI key upstream". OpenAI 키가 제3자 제공자로 나갔다는 뜻이다. 저장소 보안 권고는 없다.
- **깨지는 변경(PR 원문):** "Breaking change: a JSON-configured deployment with no `api_key`, such as one on an auth-free custom `api_base`, now gets a 401 instead of sending `OPENAI_API_KEY`." 과금 변경 원문: "Intentional product change: Groq and Meta transcription bills at listed prices".
- **함정:** LTS 패치판에 깨지는 변경이 들어왔다. 인증 없는 자체 `api_base`를 쓰던 배포는 키를 명시한다. 조회 시점에 이 수정은 LTS 1.108.15와 개발판에만 있고, 안정판 최신(1.110.2)에는 새 판이 없다.
- 같은 창의 1.108.14 원문: "Fixed Claude's tool search going missing on `/v1/responses` follow-up turns, which led to 400s from Anthropic (#50821)".

[v1.108.15](https://github.com/BerriAI/litellm/releases/tag/v1.108.15) · [v1.108.14](https://github.com/BerriAI/litellm/releases/tag/v1.108.14) · [PR #50819](https://github.com/BerriAI/litellm/pull/50819)

### 6. (호환성 깨짐) transformers 5.24.0: `past_key_values`가 `cache`로 바뀌고 `peft>=0.22.0`이 필수가 됐다 (공식, 12:28 UTC)

- **릴리스 원문:** "`past_key_values` and `cache_params` are renamed to `cache` across the library." 원문: "Passing both an old name and `cache` raises a `ValueError`." 원문: "`peft>=0.22.0` is now required." 원문: "`modeling_attn_mask_utils.py` has been removed." 원문: "custom `Cache` subclasses that override `update` should accept the extra `kwargs` argument."
- **함정:** 옛 마스크 유틸을 import하는 `trust_remote_code` 모델은 바로 깨진다. 원문: "must move to `transformers.masking_utils`". peft도 같이 올린다. Hy4 flash용 PR #50903은 여전히 열려 있다.
- **추천 이유:** Inkling 2 Mini를 비롯한 새 모델 지원이 들어왔다.

[v5.24.0](https://github.com/huggingface/transformers/releases/tag/v5.24.0)

### 짧게

- **Claude Agent SDK Python 0.2.187(PyPI 11-20 23:27 UTC):** 릴리스 본문은 비어 있고, 변경은 번들 CLI를 2.1.330으로 올린 커밋 하나다. 직전 판 도구 1번의 `deniedMcpServers` 의미 변경과 rm 확인 확대가 Python SDK 사용자에게도 들어간다. [v0.2.187](https://github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.187)
- **llama.cpp b12604(10:05 UTC):** PR #32551 원문: "This fixes integer overflow that could cause a llama-server to hang and exhaust memory while parsing malformed grammar." llama-server를 외부에 열어 뒀다면 올린다. [PR #32551](https://github.com/ggml-org/llama.cpp/pull/32551)
- **ClawDOS(HN 05:38 UTC, 150점대):** IBM PC XT에서 도는 Claude 코딩 하네스다. 원문: "The 8088 does everything but TLS." TLS는 다른 PC가 맡고, 원문 "Every screen below comes from a 5160 at 4.77 MHz, emulated in 86Box."다. 실용보다 시연이다. [ClawDOS](http://ricapar.net/ClawDOS/) · [HN](https://news.ycombinator.com/item?id=50586292)
- **ClawVet(Show HN, npm 0.18.0):** SKILL.md·AGENTS.md·Cursor 규칙을 정적 분석하는 스캐너다(MIT). 원문: "Runs 100% locally via WebAssembly Tree-sitter. Nothing is uploaded." 벤치마크 수치는 자체 측정이다. [사이트](https://clawvet.vercel.app/) · [저장소](https://github.com/MohibShaikh/clawvet)
- **agent-mcts(Show HN):** Claude Code에 MCTS를 씌운다. 효과는 저자가 스스로 부정했다. 원문: "Not yet, on the tasks measured so far". 원문: "The judge costs 5× the episodes and about 5× the wall time." [저장소](https://github.com/natsu0529/agent-mcts)
- **skill-distill(창 안 생성):** Claude Code·Codex 세션을 오픈 웨이트 모델용 스킬로 증류하는 스킬이다(MIT). 원문: "The student gets the reference's prompt verbatim, a clean starting state, and the skill." pi·opencode 어댑터는 원문 "written, untested"다. 별 수(조회 시 300대)의 진위는 확인하지 못했다. [저장소](https://github.com/sybil-solutions/skill-distill)
- **Qwen Code 0.28.3(01:45 UTC):** 직전 판까지 프리뷰만 있다가 안정판이 나왔다. 원문: "No known breaking changes." [릴리스](https://github.com/QwenLM/qwen-code/releases/tag/v0.28.3)
- **Amp 차량 모드(08:00 UTC):** 원문: "The Amp apps for iPhone and Android will automatically enter car mode if you use them there." [Amp](https://ampcode.com/news/puck-in-the-car)
- **AWS Quick 플러그인(02:58 UTC):** 원문: "Plugins package these capabilities together, so users can install everything a job needs in one step." Agent Skills·MCP와 호환되는지는 원문에 없다. [AWS](https://aws.amazon.com/about-aws/whats-new/2026/11/aws-quick-plugins-bundle-agents-skills-connectors-flows/)
- **Vercel AI SDK 7.0.179(11-20 23:17 UTC):** 게이트웨이로 Live 모델에 WebSocket 연결을 연다. 직전 판 도구 6번(도구 승인 서명 결함)의 v6 백포트는 여전히 없다. [릴리스](https://github.com/vercel/ai/releases/tag/ai%407.0.179)
- **직전 판 후속 중 새 진전 없음:** Claude Code(dist-tags `latest`·`next` 2.1.330, `stable` 2.1.323 그대로. #110979·#110642·#111167·#111218·#111068 모두 열려 있음), Agent SDK TS(0.3.330), claude-code-action(1.0.286), Codex CLI(알파만), Pi(1.6.0), OpenAI Agents SDK, goose(#13663은 병합됐지만 1.60.2 이후 릴리스 없음), pydantic-ai PR #14095(열림), OpenCode(2.3.2), Mistral Vibe(3.1.4), VS Code(1.147.0), Gemini CLI·Crush(nightly만), Copilot CLI(프리릴리스만), MCP Events TS PR, GitHub MCP Server, Playwright MCP, Open WebUI, Strands, crewAI, Hermes Agent, MCP 공식 SDK·Inspector·레퍼런스 서버, Context7, Dify, Cursor, Mastra. AutoGPT 0.12.0 보안 권고는 아직 공개되지 않았다. `anthropics/skills`·`openai/plugins`는 창 안 커밋이 없다.

## 주목할 점

- **AI 수학의 다음 병목은 "사람이 이해하는 것"이다.** 기계 검증은 하루 만에 공동체 인증까지 갔다. 그런데 사람이 읽을 원고는 아직 없다. 첫 해설은 다른 회사 모델(GPT 6 Pro)로 Lean 코드를 풀어 쓴 것이고, 저자도 정확한 번역인지 보장하지 않았다. comparator의 최종 결과, con-ron 판정, 수학자들의 해설 작업, Clay 연구소의 반응을 지켜본다.
- **라우터 뒤에서 무엇이 내 프롬프트를 처리하는지 모른다.** Dragos 연구, OpenRouter의 MiniMax 통지, Prime Intellect의 모델 바꿔치기가 같은 주에 나왔다. 프롬프트가 새는 경로도, 모델이 몰래 바뀌는 경로도 확인됐다. ZDR 표시만으로는 데이터 처리를 보장하지 못한다. 비밀이 든 에이전트 작업은 제공자를 직접 지정하고, 그 기간에 프롬프트에 넣은 키는 폐기한다.
- **다가오는 일정:**
  - 이번 주: KEV 4건 조치 기한(11-23), **Ruby2ßhell Rails PoC 공개(11-25, GHES 등 대형 공급사 패치가 늦으면 연기)**, ERNIE 5.5 Flash Preview 가중치 공개 약속(11-25 전), Qwen4-Max-Preview 종료(11-25), Gemini 4 Carbon·Argon 도입가 종료(11-30), Huawei openPangu-2.0 종료(11-30).
  - 12월: Puppet Enterprise KEV 조치 기한(12-01), LiteLLM KEV 조치 기한(12-02), Backblaze 새 개인정보 처리방침 시행(12-01), **EU·EFTA·영국 Copilot 테넌트 Claude 기본 켜짐(12-09)**, Val Town 읽기 전용(12-10), GitHub Copilot 모델 6종 은퇴(12-14), Inception Mercury API 종료(12-28).
  - 그 뒤: BIS 원격 접근 규칙 의견 마감(2027-01-19), Windows 기본 소형 모델 Aion 탑재(2027년 1월 말), 다음 MCP 명세(03-30).

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - openai.com 본문은 403이라 공식 문서는 `developers.openai.com`의 `.md` 판으로 읽었다. Guardian·THN·IHES PDF·Veria·RÚV는 r.jina.ai로 텍스트를 뽑았다. IHES 노트는 수식이 깨졌을 수 있지만 인용한 문장은 일반 문장이다.
  - Lean Zulip은 인증이 필요해 검증 논의를 직접 보지 못했다. 공개 아카이브는 오래전에 멈춰 있다.
  - NYT·WSJ·The Decoder 유료 기사, Reuters(캡차), LMArena는 열지 못했다. Bloomberg·The Information 보도는 2차 보도로만 봤다. x.com 타임라인과 reddit은 막혀 있어 Anthropic 직원 글은 확인하지 못했다.
  - OpenRouter 메일은 HN 사용자가 옮긴 본문만 있다. OpenRouter 공개 페이지와 MiniMax 입장은 찾지 못했다.
- **시각이 날짜만 있거나 불확실한 원문:**
  - OpenAI GPT-6.2 가이드의 개정 시각은 모른다(Wayback 11-15 사본 이후, The Decoder 보도 13:32 UTC 이전). SimpleBench 갱신도 Wayback 사본 사이 구간으로만 잡았다.
  - NebuSec 글은 날짜만 있어 Wayback 첫 캡처(01:49 UTC)로 보완했다. Upload2Shell은 oss-security 게시와 HN 시각으로 잡았다. IHES 노트는 `last-modified`가 갱신본 시각이고 첫 게시 시각은 모른다.
  - Claude 앱 릴리스 노트 항목은 날짜만 있다(11-21 10:12 UTC 사본 이후 추가). Prime Intellect 공지와 Dragos 글 갱신분도 날짜·수정 시각만 있다. DSA-6646-1은 시각을 확인하지 못했다.
- **확인하지 못한 것:**
  - 리만 가설 증명에 쓰인 모델과 버전, Anthropic의 관여 여부. con-ron의 실제 수락 여부와 comparator 최종 결과.
  - Dragos가 말한 "a Chinese AI lab"과 MiniMax가 같은지. Cline의 `bunfig.toml` 문제가 어느 버전부터였는지. LiteLLM 안정판(1.110.x)에 같은 키 전송 동작이 있는지. Kimi Code 결함의 세부 내용(VulDB만 근거). Enterprise Claude Data 기본 켜짐의 실제 전환. Gemini 4 Neon 안전 보고서(추정 URL만 조회). NVIDIA 컨텍스트 창 논문 원문.
  - 재현하지 않은 것: 모든 증명 검증 결과와 벤치마크(SimpleBench, LookAtMyQuant, Unsloth, ClawVet), 모든 PoC와 발견자 주장(NebuSec, Upload2Shell, FastMCP, vLLM 이미지 버그, llama.cpp PPL 수치). 도구와 패키지는 하나도 설치하거나 실행하지 않았다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 80점 이상 스토리는 제목을 전부 봤다. 20점 초과 스토리 30개는 모두 봤고, 10점 이상 52개는 키워드로 걸렀다. Show HN은 5점 이상을 봤다.
  - 전역 GHSA는 reviewed 15건, unreviewed 357건, malware 6건을 창 범위로 받아 셌다. unreviewed는 AI 키워드로 걸렀다. 저장소 권고는 AI·도구 저장소 약 78곳을, 도구 릴리스는 약 96곳을 봤다.
  - HF 조직 스캔은 조직당 15개만 봤다. 그래서 401 추적 목록은 저장소별로 직접 조회했다. HF Daily Papers 11-21분은 비어 있었고 arXiv는 공지가 없는 창이다. GitHub 트렌딩은 1면만 봤다. MCP 공식 레지스트리의 창 안 갱신(100건 이상)은 하나씩 평가하지 않았다.
  - 한국 랩은 HF 조직 스캔과 국내 기사 제목 검색만 했다. Vertex AI 릴리스 노트(캐시 동결)와 Moonshot·Volcengine 변경 로그(월 단위)는 일 단위로 판정하지 못했다.
  - 창이 끝날 때 열려 있던 것: Lean comparator 실행, MONAI Label PR #1942와 PyPI 수정판, GHES Ruby 패치, OpenSSL 4.0.4·4.1.1, Ubuntu PHP USN, Claude Code #110979·#110642·#111167·#111218·#111068, goose 캐시 수정 릴리스, pydantic-ai PR #14095, Inkling 2 Mini의 SGLang·Ollama 안정판 지원과 vLLM 정식 릴리스, LiteLLM 안정판 키 전송 수정, Kimi Code 수정판, AutoGPT 0.12.0 보안 권고, Microsoft 저장소 복구. 다음 판에서 다룬다.
