---
title: "2026-11-23 AI 브리핑"
date: 2026-11-23T08:00:00+09:00
tags: [ai-briefing, openai, openrouter, security, mcp]
description: "OpenAI 수석과학자가 사내 퍼플렉서티 곡선과 GPT-6.2보다 앞선 내부 모델을 공개하며 업계 공동 개발 한도를 제안했고, OpenRouter는 제공자 3곳을 퇴출했으며 Together AI는 AI 코딩 에이전트가 커밋한 자격 증명에서 프롬프트 유출이 시작됐다고 밝혔다."
---

> 조사 범위: 2026-11-22 08:00 ~ 2026-11-23 08:00 KST(2026-11-21 23:00 ~ 11-22 23:00 UTC, 미국 토요일 15:00 ~ 일요일 15:00 PST). 직전 브리핑(11-22 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`·`merged_at`·커밋 시각, npm·PyPI·ModelScope 게시 시각, Hugging Face API, Red Hat CSAF `initial_release_date`, Debian 메일링 리스트 `Date`, 저장소·전역 GHSA `published_at`, Wayback 캡처, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-22 23시대 UTC) 값이다.

## 오늘의 핵심 요약

- **OpenAI가 출시하지 않은 내부 모델의 존재를 숫자로 드러냈다.** 수석과학자 Jakub Pachocki가 사내 사전학습 지표(log-perplexity)를 처음 공개했다. 그래프에서 내부 모델은 0.732로 GPT-6.2 Astra(0.761)보다 앞선다. 그는 업계가 함께 넘지 않을 "공동 한도"를 정하자고 제안했다. 다만 측정 데이터는 OpenAI 사내 코드라 다른 랩과 바로 비교할 수 없다.
- **추론 공급망 사고가 커졌다.** OpenRouter는 공식 글에서 제공자 3곳을 퇴출하고 ZDR 엔드포인트 125개를 해제했다고 밝혔다. Together AI는 유출의 출발점이 AI 코딩 에이전트가 공개 저장소에 커밋한 자격 증명이었다고 공지했다. Novita는 MiniMax 모델 요청의 노출 기간을 5-28부터로 잡았다.
- **MCP 레지스트리 항목 649개가 사라진 패키지를 가리킨다.** 공식 MCP 레지스트리가 악성 패키지 대조와 사라진 패키지 점검을 켜면서 드러났다. 누구나 다시 등록할 수 있는 npm 이름이 남아 있지만, 레지스트리는 아직 항목을 내리지 않고 목록도 공개하지 않는다. 리만 가설 Lean 증명은 독립 검사기 con-ron도 통과했지만, 공식 판정 도구는 아직 돌고 있다.

## 모델 소식

주말 창이라 새 모델, 가격 변경, 상태 페이지 장애가 없었다. 공식 변경 로그도 모두 그대로다(Claude Platform 11-17, Claude 앱 11-20, OpenAI API 11-19, ChatGPT 11-20, Gemini API·AI Studio 11-19, Gemini 앱 11-17, xAI 11월, DeepSeek 11-13). OpenRouter 신규 등록은 0건이다. 가장 큰 소식은 OpenAI 수석과학자의 글이다. 사내 사전학습 지표를 처음 공개했고, 출시하지 않은 내부 모델이 GPT-6.2보다 앞서 있다는 것도 숫자로 드러냈다. 오픈 웨이트는 Ant Group의 의료 모델 두 종이 나왔다.

### 1. OpenAI 수석과학자 Pachocki가 사내 퍼플렉서티 곡선을 공개했다. 미공개 내부 모델이 GPT-6.2 Astra보다 앞서 있고, 업계가 함께 넘지 않을 "공동 한도"를 정하자고 제안했다 (공식, RSS 날짜 11-22, HN 첫 게시 17:06 UTC 430점대)

- **무엇:** "Perplexity and Intelligence"라는 글이다. 원문: "By Jakub Pachocki, Chief Scientist at OpenAI". GPT-3.5부터 지금까지 사내 모델의 log-perplexity(다음 토큰 예측 손실)를 그래프로 공개했다. 원문: "In this post, I argue for establishing perplexity (or, equivalently, cross-entropy loss) as a metric to compare between model developers and inform shared safety standards." 공개를 망설였다는 말도 적었다. 원문: "Publishing our internal perplexity measurements took some consideration; it gives competitors a clearer picture of where we stand".
- **그래프 데이터(페이지에 들어 있는 값을 직접 확인):** GPT-5.3-Codex/5.4 0.871("Useful coder"), GPT-5.6 Sol 0.828("Cybersecurity expert"), GPT-6 Astra 0.783, GPT-6.2 Astra 0.761, 그리고 **"Internal model" 0.732(2026-11)**다. 본문 원문: "GPT‑3.5, at a log-perplexity of ~0.99 on our current main evaluation dataset, was capable of fluent conversation". 원문: "Once we upgraded to a model of log-perplexity ~0.76, it started producing groundbreaking results."
- **전망과 제안(원문):** "Extrapolating from recent progress, we should expect further transformative capabilities as we enter the ~0.68-0.7 log-perplexity window." 원문: "as soon as 2027 we could conceivably see a similar avalanche of fundamental pretraining advancements". 제안 원문: "I further propose common limits, based on perplexity and other measurements, that developers are committed to not crossing until our ability to understand and monitor AI behavior achieves a certain concrete milestone (such as the development of a successor to CoT monitors)." 각주에는 AI가 이미 모델 설계에 기여했다고 썼다. 원문: "for the most recent models, we have already seen meaningful design contributions coming from AI".
- **함정:**
  - 측정 데이터가 OpenAI 사내 코드다. 각주 원문: "The primary held-out datasets we’ve been using since GPT-3.5 are sourced from our internal codebase, and those are the numbers reported here (for a fixed snapshot)." 다른 랩의 숫자와 바로 비교할 수 없다. 글도 한계로 원문 "Difficulty of comparison across labs."를 들었다. HN에서도 사내 코드 데이터셋을 문제 삼는 댓글이 나왔다.
  - 그래프의 GPT-6.2 Astra 점에는 "Solved a Millennium Prize math problem"이라는 라벨이 붙어 있다. 어느 문제인지 본문은 밝히지 않는다. 10-29 판에서 다룬 대로, OpenAI의 Yang–Mills 결과를 맡은 검증위원회는 질량 간극이 아직 열린 문제라고 했다. 이 라벨은 OpenAI의 주장으로만 읽는다.
- **왜 중요한가:** 출시 모델보다 앞선 내부 모델이 있다는 것을 OpenAI가 처음 숫자로 드러냈다. 같은 글에서 경쟁사들에 "여기서 멈추자"는 공동 기준을 정하자고 공개 제안했다. 글의 "shared" 링크는 Dario Amodei의 "We Must Pace the Frontier"를 가리킨다. Anthropic이 이 제안에 답할지 지켜본다.

[OpenAI](https://openai.com/index/perplexity-and-intelligence/) · [HN](https://news.ycombinator.com/item?id=50601525)

### 2. Ant Group(inclusionAI)이 의료 오픈 웨이트 LingSante-3.1-flash(560B, 활성 25B)와 LingSante-3.1-air-VL(124B, 활성 5.5B)을 MIT로 냈다 (공식 저장소, ModelScope 등록 11:29 UTC)

- **무엇(README 원문):** flash는 "560 billion total parameters, activates 25 billion parameters per token, and supports a 1-million-token context window"다. Ling-3.1-flash 기반이다. air-VL은 Ling-3.0-flash-VL 기반 멀티모달이고, 원문 "124B parameters and 5.5B active parameters per token"에 컨텍스트는 256K다. 원문: "It is the first large multimodal medical model that Ant Group's healthcare team has released as open source."
- **공개 시점:** HF 저장소는 11-19에 만들어졌지만, ModelScope 등록이 11-22 11:29 UTC이고 HF 첫 토론 글이 14:52 UTC다. 비공개로 만들어 두었다가 창 안에 연 것으로 본다. 블로그나 X 발표는 찾지 못했다.
- **성능 주장(원문):** "LingSante-3.1-flash demonstrates strong performance on benchmarks for medical professional behavior, ranking second overall behind only GPT-6 Astra." 벤치마크 표는 이미지로만 있어 수치는 확인하지 못했다.
- **함정:** 원문: "LingSante-3.1-flash is intended for research and evaluation. It is not a medical device and must not be used as an independent basis for diagnosis, prescriptions, or treatment." flash는 SGLang 전용 dev 이미지가 필요하고, air-VL 서버 예시도 141GB급 GPU 4장이다. README는 창 안에 여러 번 고쳐졌다(마지막 17:58 UTC).

[LingSante-3.1-flash](https://huggingface.co/inclusionAI/LingSante-3.1-flash) · [LingSante-3.1-air-VL](https://huggingface.co/inclusionAI/LingSante-3.1-air-VL)

### 3. 짧게

- **(커뮤니티 벤치마크) GPT-6.2가 짧게 답한다는 외부 측정:** Artificial Analysis가 X에 성격 벤치마크 결과를 올렸다(11-19 02:06 UTC, 창 경계, 과거 판 미수록). 원문: "both use less structure, are less likely to admit uncertainty, and give shorter responses". 원문: "Shorter responses: 352 to 210 words per response (Astra), 337 to 223 (Sol)". 원문: "Less likely to admit uncertainty: Admits Uncertainty 96 to 24 (Astra) and 98 to 51 (Sol)". The Decoder(09:41 UTC)가 이를 Claude와 비교해 보도했다. 원문: "A lower Transparency Index doesn't necessarily mean a model gives wrong answers more often." 직전 판 1번에서 OpenAI가 문서로 인정한 성향을 외부 측정이 뒷받침한다. 함정: Decoder 표의 Claude 수치 일부는 AA 페이지의 현재 값과 다르다. [AA X](https://x.com/ArtificialAnlys/status/2123236464003588136) · [AA PersonalityBench](https://artificialanalysis.ai/evaluations/aa-personalitybench) · [The Decoder](https://the-decoder.com/gpt-6-2-astra-and-sol-get-to-the-point-faster-but-score-lower-on-transparency-than-claude/)
- **NVIDIA 후속(CBS Face the Nation 전사, 17:16 UTC):** Bessent 재무장관 원문: "The board has slowed things down. The CFO is now in charge". 원문: "I have confidence that the Nvidia committees are moving very quickly on this and will come up with some very fulsome and robust answers." BIS 원격 접근 규칙에 대해서는 원문 "the remote access rule, which prevents certain countries, certain entities from accessing chips via lease capacity, which I actually think may be more important."이라고 했다. 창 안 NVIDIA SEC 제출은 없다. [CBS](https://www.cbsnews.com/news/transcript-treasury-secretary-scott-bessent-face-the-nation-with-margaret-brennan-nov-22-2026/)
- **(루머) Apple 에이전트(Bloomberg Gurman, MacRumors 20:29 UTC):** 원문: "Apple is reportedly developing a more advanced AI agent, but Gurman does not expect it to arrive before late 2027." 원문: "Siri's multistep control of Apple apps is expected to launch in December, with third-party app support following in 2027." [MacRumors](https://www.macrumors.com/2026/11/22/siri-layoffs-spared-newer-ai-systems/)
- **직전 판 후속:**
  - 리만 가설 증명: Anthropic은 여전히 공식 논평이 없고, 쓰인 모델 이름도 공개되지 않았다. Anthropic 사이트맵의 최신 갱신(11-20 18:45 UTC)에도 관련 글이 없다. 검증 쪽 진전은 기술 이슈 2번에 정리했다.
  - Anthropic 상장: 창 안 SEC 제출은 없다(최신 접수 11-19 424B4, 종결 8-K 미제출).
  - OpenRouter–MiniMax: MiniMax 공식 입장은 여전히 없다. 제공자 쪽 공지는 기술 이슈 1번에 정리했다.
  - 여전히 401: Qwen4-Max, Qwen4-Max-Preview(11-25 종료), Ling 3.2 Flash, ERNIE 5.5 Flash Preview(11-25 전 공개 약속), LG K-EXAONE 2.5. FLUX.1-schnell은 여전히 gated다.
  - Gemini 4 Neon 안전 보고서는 찾지 못했다. SimpleBench는 새 항목이 없다. Inkling 2 Mini 외부 평가도 바뀌지 않았다.
  - Hy4는 TechNode의 "Apache 2.0" 오보가 고쳐지지 않았다. transformers PR #50903도 열려 있다. 새로 나온 안전 보고서는 아래 창 경계 항목에 적었다.
  - 소식 없음: Cursor(중국 모델 퇴출 검토), NVIDIA–Inception, 캘리포니아 AG 소환장, OpenAI 증류 글 후속, Apple macOS 27.3 에이전트 제한, DeepSeek 사용 제한·V5, Anthropic 30일 공개 정책, 'Mythos' 상표 소송, Edge0, Agnes, Kimi K3.1 Pro, LongCat-2.5, ChatGPT Wallet, Meta(NebuSec 건 공식 입장), 한국 랩 모델 발표.
  - Enterprise Claude Data 기본 켜짐은 문서 문구가 여전히 원문 "turns on by default on November 20, 2026"이고, 실제 전환은 확인하지 못했다. Microsoft EU·EFTA·영국 Copilot 문서(12-09 변경)도 그대로다.

### 참고: 창 경계 항목

- **(METR 11-20 08:00 UTC, 직전 판들 미수록, The Decoder 창 안 13:00 UTC) METR 4분기 역량 보고:** 원문: "We think that current public evidence is insufficient to rule out drastic AI R&D acceleration in the next couple of years." 원문: "time horizons are doubling roughly every 3.61 months (95% CI 2.08-4.94), or 110 days". 원문: "naive extrapolation would imply that models reach a 50% time horizon equal to the longest score threshold on our task set (200 hours) in July 2027." 원문: "Most of the reward hacking we found was by near-frontier agents". 함정: 최신 모델 Fable 5.2와 GPT-6.2는 측정 대상이 아니다. 원문 목록 끝이 "Fable 5.1, GPT-6 Astra and Opus 5.5"다. 1번 OpenAI 글과 같은 날 읽으면, 두 곳 모두 가속을 경고하고 있다. [METR](https://metr.org/blog/2026-11-20-q4-2026-capability-update/)
- **(보고서 11-20, SCMP 창 안 13:30 UTC) Tencent Hy4 82쪽 안전 보고서:** SCMP 원문: "The development marks the first time a leading Chinese tech company has disclosed that it worked with independent third-party evaluators in such evaluations of a new model before releasing it." 함정 원문: "The biological safety findings were based on Tencent's own benchmarks rather than evaluations by the third-party evaluators." 보고서 원문 URL은 찾지 못했다. [SCMP](https://www.scmp.com/tech/tech-trends/article/3375374/tencent-touts-latest-ai-model-hy4s-performance-independent-safety-assessment)

## 기술 이슈

직전 판의 추론 공급망 문제가 하루 만에 커졌다. OpenRouter가 공식 글을 냈고, 제공자 네 곳이 사고 공지를 냈다. 그중 Together AI는 유출의 출발점이 AI 코딩 에이전트가 공개 저장소에 커밋한 자격 증명이었다고 밝혔다. 리만 가설 Lean 증명 검증은 독립 검사기가 하나 더 통과했지만, 공식 판정 도구는 아직 돌고 있다. 커뮤니티 쪽에서는 oss-security가 AI 봇이 보낸 취약점 보고를 받을지를 두고 논쟁했다. 주말이라 CISA KEV 추가는 없고 arXiv 공지도 없다. 전역 GHSA 창 안 게시분은 reviewed 0건, unreviewed 137건, malware 10건이다(직접 셈).

### 1. OpenRouter 공식 글: 제공자 3곳을 퇴출하고 ZDR 엔드포인트 125개를 해제했다. Together AI는 "AI 코딩 에이전트가 자격 증명을 커밋했다"고 밝혔다 (공식, OpenRouter 글 날짜 11-22·Wayback 01:25 UTC, HN 02:14 UTC 270점대)

- **OpenRouter(원문):** "These findings did not show any breach or unauthorized access of OpenRouter." 조치 원문: "Off-boarded three providers from OpenRouter, based on the results of our investigations." 원문: "Removed 125 endpoints from Zero Data Retention (ZDR) eligibility. The removals included providers flagged by researchers, plus additional endpoints that we removed as a precautionary measure." 진행 중인 일 원문: "We are conducting technical verification of network paths to identify instances where requests and responses may be being forwarded to third parties." 직전 판은 HN에 옮겨진 고객 메일만 다뤘고, 공식 공개 글은 이번이 처음이다.
- **퇴출된 세 곳은 이름이 없다.** THN 원문: "Dragos has not named the three breached providers, OpenRouter has not named the three it removed, and neither has said publicly whether they are the same three." 글에 실린 제공자 공지 목록도 창 안에 바뀌었다(Wayback 대조: 01:25 UTC에는 Novita·Phala·Together, 14:31~18:12 UTC 사이에 Atlas Cloud 추가). 원문: "Not all affected providers have posted publicly."
- **Together AI(11-21 23:18 UTC 무렵, 새 사건):** 원인 원문: "During development of our public GitHub repository, xorl, an AI coding agent committed a credential that provided access to the private Hugging Face repository." 원문: "Our automated scanning detected the exposed credential, and we reverted the commit." 그 비공개 저장소에 MiniMax M3·GLM-5.2·Kimi K2.6 요청의 프롬프트 샘플이 있었다. 원문: "Five of the credentials tested were canary credentials associated with a Dragos security experiment." 바꾼 것 원문: "Making zero data retention (ZDR) the default setting for new accounts." 원문: "Updating our development processes governing the use of AI coding agents." 함정: 첫 사본(00:15 UTC)에 있던 문장 "We have no reason to believe the unauthorized party has leaked this data further"가 06:41 UTC 사본에서 사라졌다. 자격 증명을 폐기했다는 말은 없고, 커밋을 되돌렸다고만 썼다.
- **Novita:** 원문: "Our review confirmed that some requests were forwarded to MiniMax's official API rather than processed on Novita-hosted infrastructure." 원문: "If you used MiniMax models through Novita between 2026-05-28 and 2026-11-11, your request contents may have been accessed or retained by a third party." 원문: "The access patterns suggest automated content review may have been involved." 노출 시작이 OpenRouter 메일의 "July to October"보다 이른 5-28이다.
- **Atlas Cloud:** 원문: "His testing so far points to the access happening after requests reached MiniMax." 원문: "we chose to take every Atlas Cloud model off OpenRouter. This was our decision, not OpenRouter's, and we told OpenRouter before we did it." 연구자 HD Moore가 원문 "He now tests them four times a day"라고 적었다. 첫 게시 시각은 확인하지 못했다(현재 메타 22:43 UTC).
- **(창 경계, 11-10 글) Phala:** 과거 판에 없던 공지가 OpenRouter 목록에 실려 드러났다. 원문: "For requests sent between October 21, 2026 at 00:00 UTC and November 8, 2026 at 05:35 UTC, treat request contents as exposed." 원인 원문: "The attacker chained a legacy internet-facing service with management interfaces that relied on loopback access for trust and did not independently authenticate callers." 대상은 OpenRouter가 Phala로 보낸 DeepSeek V4 Flash 0731 요청이다.
- **변화 없음:** MiniMax 공식 입장은 여전히 없다. Dragos 글과 Prime Intellect 공지는 창 안 갱신이 없다.
- **할 일:** 5~11월에 MiniMax 모델을 라우터로 썼거나, 10-21~11-08에 DeepSeek V4 Flash 0731을 썼다면 그 기간 프롬프트에 넣은 키·URL은 폐기한다. 에이전트가 커밋하는 저장소에는 비밀값 스캐너를 켠다. Together도 스캐너가 잡았지만, 그 전에 이미 누군가 그 키를 썼다.
- **왜 중요한가:** 유출 경로가 세 가지로 드러났다. 제공자가 몰래 원 개발사 API로 넘긴 경로, 호스팅 서버 침해, 그리고 AI 코딩 에이전트가 자격 증명을 커밋한 개발 과정이다. 셋째는 코딩 에이전트를 쓰는 모든 팀의 문제다.

[OpenRouter](https://openrouter.ai/blog/announcements/data-processing-incidents-on-inference-providers/) · [Together AI](https://www.together.ai/blog/security-event-prompt-leakage-for-requests-in-june-and-july-of-2026) · [Novita](https://novita.ai/blog/minimax-requests-privacy-notice/) · [Atlas Cloud](https://www.atlascloud.ai/blog/ai-updates/security-and-privacy-commitment) · [Phala](https://phala.com/posts/phala-november-preliminary-incident-report) · [THN](https://thehackernews.com/2026/11/openrouter-drops-3-providers-after.html) · [HN](https://news.ycombinator.com/item?id=50595554)

### 2. 리만 가설 Lean 증명 검증 후속: con-ron도 받아들였고, 샌드박스 안 재빌드가 바이트 단위로 같았다. 공식 판정 도구와 Lean 커널 재검증은 아직 돌고 있다 (공식 검증자 문서, "Page last updated 2026-11-22 16:20 UTC")

- **직전 판의 질문 둘에 답이 나왔다(Sutherland 페이지):**
  - con-ron 수락 기록: 원문: "challenge against solution accepts all five theorems". 원문: "exit 0 at 23:02:33 UTC on 21 November". Lean 4.36에 번들된 구버전은 여전히 거절이 아닌 exit 2이고, 원문 "con-leche and con-ron built from master, above, do."다.
  - 샌드박스 없이 빌드했다는 단서: 원문: "An independent sandboxed lake build of the archive reproduces the export byte for byte." 원문: "The export made inside that sandbox is byte-identical to the published one, so every check on the published export applies to it."
- **남은 것(원문):** "The lake comparator run and the Lean-kernel replay of the export are still in progress." 원문: "The stage still running is Lean's kernel on the export: single-threaded, running since 19:03 UTC on 20 November." 공동체 저장소 노트의 원문 "Comparator's output will be added here when the run finishes."도 그대로다.
- **함정:** 공동체 저장소의 main 트리는 이제 원 제출본이 아니다. 01:34 UTC 커밋 메시지 원문: "The README and notes still said the submitted source is unmodified". 같은 메시지 원문: "(10,724 files removed, 18 edited)". 원본을 보려면 커밋 `40e56bab`을 본다.
- **반대 성명(창 경계, 11-20 글):** 인간수학협회(The Association for Human Mathematics, AHM) 성명이 Tao의 링크 모음(01:19 UTC)에 실려 창 안에 퍼졌다. 원문: "At this time, no human being can reasonably claim to be responsible for, or even to understand, such a document." 원문: "We believe that AI corporations urgently require democratic oversight."
- **그 밖:** IHES Robin Zhang의 해설 노트가 갱신됐다(76쪽, `last-modified` 20:33 UTC). 무엇이 바뀌었는지는 대조하지 못했다. Navier–Stokes 인증서는 아직 공개되지 않았고, Clay 연구소 반응도 없다. Anthropic 입장은 모델 소식 3번에 적었다.

[Sutherland 검증 페이지](https://math.mit.edu/~drew/grh/) · [검증 노트](https://github.com/leanprover-community/riemann-hypothesis/blob/main/verification/NOTES.md) · [AHM 성명](https://www.ahmath.org/statements) · [Tao 링크 모음](https://terrytao.wordpress.com/2026/11/21/even-more-posts-opinion-pieces-interviews-and-position-statements/) · [IHES 노트](https://www.ihes.fr/~rzhang/files/GRH%20note.pdf)

### 3. (커뮤니티 논란) oss-security가 AI 봇이 보낸 취약점 보고를 받을지 논쟁했다. 보고를 받은 메인테이너는 봇 차단을 요구했다 (메일링 리스트, 05:00~21:52 UTC)

- **배경:** 직전 판 9번의 Dokploy 불완전 수정 보고를 보낸 "knell"은 자동 보고 봇이다. 리스트 관리자 Solar Designer(05:00 UTC) 원문: "I ran a Twitter poll @oss_security, where only one person voted that I should "Accept like from a human", a few "Accept more selectively", and the majority "Reject all"." 그래도 원문: "Blindly rejecting valid reports just because they came from a bot does not feel right at this time - there are not too many yet." 대안으로 원문 "we may need to start redirecting them to another list"를 들었다.
- **반응:**
  - LWN 편집자 Joe Brockmeier 원문: "For whatever one human's voice is worth, I wish you'd just reject obvious bot reports out of hand."
  - 형식 제안 원문: "if report is LLM-generated, it _must_ start with a max-three-sentence summary containing the basic facts".
  - 반대 원문: "If this list were to start rejecting all AI-submitted reports, I would likely start looking for an additional list".
  - 같은 봇의 aiohttp 보고(08:22 UTC)에 메인테이너가 답했다. 원문: "I'm strongly in favour of banning this bot." 원문: "what it describes is an explicit design."
- **함정:** 논쟁에 나온 수치가 출처와 다르다. 원문: "Something like 1.2% of the ~85k reports have so far resulted in a CVE". 인용된 VulnCheck 글(11-12 판에서 다룸) 원문은 "Of Anthropic's 37,966 reported findings, only 7,349 (19.4%) have been made public and just 217 have been marked fixed."이고 CVE는 442건이다. 비율은 맞지만 "~85k"는 원문에 없다.
- **왜 중요한가:** 대표적인 공개 취약점 리스트가 AI 생성 보고를 정책 문제로 다루기 시작했다. AI가 찾은 결함이 쏟아지는 상황에서, 사람 메인테이너의 검토 부담이 어디까지 버틸지를 보여 준다.

[관리자 글](https://seclists.org/oss-sec/2026/q4/809) · [Brockmeier](https://seclists.org/oss-sec/2026/q4/815) · [요약 의무 제안](https://seclists.org/oss-sec/2026/q4/814) · [반대 의견](https://seclists.org/oss-sec/2026/q4/810) · [aiohttp 메인테이너](https://seclists.org/oss-sec/2026/q4/817) · [수치 인용](https://seclists.org/oss-sec/2026/q4/816) · [VulnCheck](https://www.vulncheck.com/blog/anthropic-glasswing-november-update)

### 4. MONAI Label 0.8.6: 직전 판의 경로 탈출 두 건을 고친 보안 판이 나왔다. 인증이 기본으로 꺼져 있어 누구나 닿을 수 있었다 (공식, PyPI 01:27 UTC)

- **릴리스 원문:** "This is a security release. All MONAI Label server deployments should upgrade." 직전 판이 본 두 번째 결함(PR #1942, 00:29 UTC 병합) 원문: "`PUT /session/?uncompress=true` extracted uploaded tar archives without checking member names, and session IDs were used as paths without validation. Affects all versions up to and including 0.8.5. Fixed in #1942." 원문: "Authentication is disabled by default (`MONAI_LABEL_AUTH_ENABLE=False`), so both issues were reachable by any client that can connect to the server."
- **할 일(원문):** "If a 0.8.5 or older server was reachable from an untrusted network, review it for unexpected file changes before returning it to service."
- **함정:** 원문: "MONAI Label 0.8.6 requires Python 3.11 or newer". 세션 업로드 결함에는 CVE·GHSA가 없다. 기존 권고 GHSA-qc22-rmgc-vg94는 조회 시점에도 수정 버전 칸이 비어 있어, 스캐너가 0.8.6을 수정판으로 인식하지 못할 수 있다.

[0.8.6](https://github.com/Project-MONAI/MONAILabel/releases/tag/0.8.6) · [PR #1942](https://github.com/Project-MONAI/MONAILabel/pull/1942)

### 5. Ruby2ßhell 후속: GitHub Enterprise Server가 Ruby 패치를 냈다. Debian은 직전 판에 적은 패키지 버전을 되돌렸다 (공식, GHES 릴리스 노트 날짜 11-22)

- **GHES:** 3.22.6·3.21.11·3.20.13·3.19.17이 11-22 날짜로 나왔다. 원문: "CRITICAL: GitHub has updated Ruby to fix several announced vulnerabilities. Where an update was not immediately possible, mitigations were introduced at the application level." 일부는 패치가 아니라 완화책이라는 뜻이다.
- **PoC 일정:** Veria 표는 아직 "Updated 21 November 2026 at 03:52 UTC"이고, GHES 줄도 갱신되지 않았다. PoC 문구는 원문 "or later if major vendors have not shipped a fix by then"으로 그대로다. 대형 공급사 GHES가 패치를 냈으니, 11-25 공개가 미뤄질 이유는 줄었다.
- **정정(Debian):** 직전 판은 bookworm 수정 버전을 `3.1.7-1~deb12u2`로 적었다. DLA-4926-2(19:51 UTC) 원문: "In the previous version, the package version was mistakenly bumped from 3.1.2 to 3.1.7. This change has been reverted." 새 버전은 `3.1.7+really3.1.2-0+deb12u1`이고, 수정은 그대로 들어 있다.
- **변화 없음:** RHEL은 여전히 Hardened Images만 고쳤고, Ubuntu는 모든 ruby 패키지가 `needs-triage`다. 악용 보고는 없다.
- **할 일:** GHES 관리자는 바로 올린다. 11-25 전에 Rails·Ruby 호스트를 점검한다.

[GHES 3.22 릴리스 노트](https://docs.github.com/en/enterprise-server@3.22/admin/release-notes) · [Veria](https://verialabs.com/ruby2shell/) · [DLA-4926-2](https://lists.debian.org/debian-lts-announce/2026/11/msg00055.html)

### 6. 직전 판 정정과 후속 (공식, 직접 확인)

- **정정(Ghostscript Upload2Shell):** 직전 판은 원문 "As of 12 November, Debian still listed Bookworm as vulnerable"를 실었다. 실제로 bookworm은 11-17 DLA-4914-1(`10.0.0~dfsg-11+deb12u9`)로 이미 고쳐져 있었다. 반면 Ubuntu는 CVE-2026-125396이 모든 릴리스에서 `needs-triage`다. Ubuntu에서 이미지 업로드를 받는 AI 앱은 Ghostscript를 지우거나 Pillow의 EPS 디코더를 막는다. [DLA-4914-1](https://lists.debian.org/debian-lts-announce/2026/11/msg00040.html)
- **정정(PHP):** 직전 판은 "RHEL 7~9는 여전히 Affected"라고 썼다. Red Hat CSAF로 보면 직전 창에 이미 RHEL 8 php:7.4(RHSA-2026:100991, 11-21 15:34 UTC), RHEL 9.8 EUS(101252·101253), RHEL 10.2 EUS(101251)가 나와 있었다. 이번 창에는 RHEL 8 php:8.2(RHSA-2026:101276, 02:24 UTC)와 RHEL 7 ELS(RHSA-2026:101337, 03:56 UTC)가 나왔다. 모두 critical인 CVE-2026-115560을 포함한다. Ubuntu USN은 아직 없다. [RHSA-2026:101276](https://access.redhat.com/errata/RHSA-2026:101276)
- **GPGME:** Werner Koch가 Red Hat의 2021년 패치가 업스트림에 오지 않은 데 답했다. 원문: "It is a bit strange that this was a one-off patch and never re-applied to newer releases." Red Hat·Ubuntu의 공식 판정은 아직 없다. [oss-security](https://seclists.org/oss-sec/2026/q4/813)
- **OpenSSL CVE-2026-121481:** 4.0.4·4.1.1은 아직 없다. **CISA KEV:** 창 안 추가가 없고, 직전 판 4건의 조치 기한이 오늘(11-23)이다.

### 7. 에이전트·AI 권고 (직접 확인, 모두 NVD발 미검토 미러)

- **better-chatbot CVE-2026-134755(medium):** 원문: "lets built-in tools run without user confirmation in Manual tool mode". 원문: "Attackers can plant prompt-injected documents or web pages that make the HTTP request tool send conversation data to attacker-controlled URLs." 2.0.0에서 고쳤다. [GHSA](https://github.com/advisories/GHSA-3wx4-w2p3-fv2m)
- **Skyvern CVE-2026-134534(medium):** 브라우저 이그레스 정책(`classify_url`)에 SSRF가 있다. 원문: "Upgrading to version 1.0.57 is sufficient to fix this issue." [GHSA](https://github.com/advisories/GHSA-26gp-8v45-m8j9)
- **Wiro-MCP CVE-2026-134553(medium):** `upload_file`의 `url` 인자에 SSRF가 있다. 1.3.1에서 완화했다. [GHSA](https://github.com/advisories/GHSA-2wc7-ggj6-fw67)
- **함정:** 세 권고 모두 GHSA의 패키지 매핑이 비어 있어 Dependabot 같은 패키지 기반 경보가 울리지 않는다. 직접 버전을 본다. llama.cpp CVE-2026-134540은 오래전 빌드(b12405)에서 고친 결함의 늦은 미러다. malware 10건 중 AI 테마는 없다. AI·도구 저장소 약 108곳의 저장소 권고는 창 안 0건이다.

### 8. AI 무관 대형 사고 (매체보도)

- **GL.iNet 클라우드 결함으로 라우터·KVM에 악성코드(BleepingComputer 18:45 UTC, 회사 권고 11-20):** 원문: "GL.iNet has confirmed that hackers exploited a vulnerability in its cloud remote management services to install custom malware on customers' routers and KVM devices". 회사 원문: "These measures do not remove malware from devices that were already infected." KVM은 1.10.3 이상으로 올리고, `crond -i /tmp/.log` 프로세스가 있는지 본다. KVM에 연결된 컴퓨터도 점검한다. [BleepingComputer](https://www.bleepingcomputer.com/news/security/hackers-exploit-glinet-cloud-flaw-to-infect-routers-kvm-devices/)
- **Cleverbridge 결제 카드 유출(BleepingComputer 15:43 UTC):** 소프트웨어 판매 대행사다. 원문: "our investigation subsequently determined that a third party copied payment card data stored on our platform". 어느 고객사의 구매자가 영향을 받았는지는 공개되지 않았다. [BleepingComputer](https://www.bleepingcomputer.com/news/security/cleverbridge-warns-hackers-stole-credit-card-numbers-cvvs-in-data-breach/)
- **함정(The Verge 장애 에세이 22:48 UTC):** 원문 "While the outages over the last week were believed to be caused by cyberattacks"는 AP 보도와 어긋난다. AP 원문: "officials have said were caused by routine efforts to upgrade computer systems and fix vulnerabilities, rather than a cyberattack". [The Verge](https://www.theverge.com/tech/1039851/major-tech-outages) · [AP 전재](https://www.securityweek.com/fbi-investigating-apparent-hack-of-its-key-national-criminal-justice-database-ap-source-says/)

### 9. 짧게

- **(커뮤니티 논란, 원문 11-16) "Anthropic 전력의 87%는 가스"(HN 08:57 UTC 830점대, 창 AI 최고점):** Better Data Center Project의 추정이다. 원문: "we estimate about 87% of the company's power will come from gas and 11% from coal in 2028". 추정 방식 원문: "no grid renewables are attributed to Anthropic's power use". 새 부하는 한계 화석 발전으로 채워진다고 가정한 계산이다. Anthropic 반응은 찾지 못했다. [원문](https://betterdatacenterproject.com/2026/11/16/how-ai-is-powered-a-breakdown-of-byop-and-grid-power/) · [HN](https://news.ycombinator.com/item?id=50597612)
- **논문:** arXiv 공지가 없는 창이고, HF Daily Papers 11-21·11-22분은 비어 있다. The Decoder의 SWE-CoWork 기사(15:15 UTC)는 11-18 판에서 다룬 논문이다. HN 89점의 "Harsher on Male?"은 6월 논문이다.
- **직전 판 후속 중 진전 없음:** NebuSec Meta Muse(Meta 입장 없음), FBI NCIC(새 사실 없음), Entra Connect Health·Rackspace Spot·Bell Canada(재보도만), Signal, Java4Shell, EC ARTEX, Apify, Microsoft agent-framework·ONNX Runtime 저장소(여전히 404), AutoGPT 0.12.0 보안 권고, KVM CVE-2026-99312(Ubuntu `needed`), Kimi Code 수정판, Dragos·AWS 보안 블로그·patchtheplanet 새 글. Node.js·Python·PHP·Ruby·Go·Rails·nginx·OpenSSL은 창 안 새 보안 릴리스가 없다.

## 써볼 만한 도구

주말 창이라 새 도구보다 업그레이드 함정과 후속 소식이 많았다. Claude Code, Codex CLI, Cline, LiteLLM 안정판은 새 판이 없다. 가장 쓸모 있는 소식은 공식 MCP 레지스트리의 점검 기능이다. 레지스트리 항목 649개가 이미 사라진 패키지를 가리키고 있었다. Inkling 2 Mini 로컬 실행은 llama.cpp가 하루 동안 빌드 16개를 내며 버그를 고쳤지만, 열린 문제가 아직 셋 남았다. 도구는 하나도 설치하거나 실행하지 않았다. 함정과 인용은 릴리스 본문, PR·이슈 본문, README에서 직접 읽었다.

### 1. (MCP 보안) 공식 MCP 레지스트리가 악성 패키지 대조와 사라진 패키지 점검을 켰다. 649개 서버 버전이 없어진 패키지를 가리킨다 (공식, v1.8.12 프로덕션 08:56 UTC · v1.8.15 프로덕션 22:07 UTC)

- **무엇:** 레지스트리가 게시된 패키지를 OSV.dev의 악성 기록(`MAL-*`)과 대조한다. PR #1889 원문: "npm, PyPI, NuGet and Cargo are covered; OCI images and MCPB bundles are not, because OSV has no ecosystem for them." 배포 전 프로덕션 목록 대조 결과 원문: "280,519 server versions, 123,936 distinct package versions, no matches".
- **아직 막지는 않는다.** 원문: "the check starts in report mode. A matching package is logged and the request goes through; nothing is refused." 매일 도는 점검도 같다. 원문: "The sweep only reports. It never changes an entry; taking one down stays a maintainer's decision."
- **사라진 패키지(PR #1895, 13:33 UTC 병합):** 원문: "An npm package whose every version was unpublished counts as gone: npm still answers 200 at its address, and those are the unscoped names anyone can register again." 원문: "The last run adds 219 versions pointing at 71 unpublished npm packages, all unscoped". PR #1902(21:48 UTC 병합) 원문: "The production sweep reported 649 server versions pointing to 206 missing packages on November 22." 그중 PyPI 쪽 일부는 오탐이었다. 원문: "generated 17 incorrect missing-package findings". 다시 확인한 결과 원문: "classified 197 as absent or unpublished and nine as present."
- **왜 중요한가:** 누구나 다시 등록할 수 있는 npm 이름을 가리키는 레지스트리 항목이 남아 있다. 남이 그 이름을 가져가면 정상 서버처럼 보이는 항목이 악성 패키지를 설치한다. 그런데 결과 목록은 공개되지 않는다. 원문: "Matches are deliberately not sent to Discord." 어떤 서버가 해당하는지 사용자는 알 수 없다.
- **할 일:** 레지스트리에서 MCP 서버를 고를 때 npm 패키지가 실제로 있는지, 게시자가 저장소 주인과 같은지 확인하고 버전을 고정한다. 게시자 쪽에도 깨지는 변경이 있다. PR #1891 원문: "A publish whose npm package `version` is a tag such as `next` gets a `400` naming the version to use."

[PR #1889](https://github.com/modelcontextprotocol/registry/pull/1889) · [PR #1891](https://github.com/modelcontextprotocol/registry/pull/1891) · [PR #1895](https://github.com/modelcontextprotocol/registry/pull/1895) · [PR #1902](https://github.com/modelcontextprotocol/registry/pull/1902) · [v1.8.15](https://github.com/modelcontextprotocol/registry/releases/tag/v1.8.15)

### 2. (후속, 직전 판 도구 2번) Inkling 2 Mini 로컬 실행: llama.cpp가 MI300X 오답과 "생각 끄기" 무시를 고쳤다. 전문가를 CPU로 내리면 출력이 깨지는 문제는 아직 열려 있다 (공식 릴리스·PR + 커뮤니티 이슈)

- **b12632(14:37 UTC):** AMD MI300 계열(gfx942)의 NVFP4 계산 오류를 고쳤다. 이슈 #32789 원문: "On gfx942 every NVFP4 `MUL_MAT` / `MUL_MAT_ID` (and the W4A8 / W4A4 variants) produces incorrect values at every batch size. As a result, NVFP4 models produce garbage outputs on MI300X/MI325X." 이슈 표의 PPL은 CPU 8.1853, gfx942 177.95다. 원문: "The CPU backend and `gfx1100` are fine on the same files."
- **b12624(01:47 UTC):** 생각을 끄는 요청이 무시되던 문제를 고쳤다. 이슈 #32753 원문: "a request that explicitly asks for no reasoning still ends up getting effort `0.9`."
- **b12622(00:49 UTC), PR #32669:** `.scale` 텐서를 자동으로 적용하게 바꿨다. 다만 다른 모델에는 남은 문제가 있다. 원문: "some models call it directly instead of `build_lora_mm`, so these scales (and LoRA) are never applied: kimi-k3/k31 (attention, dense FFN, shared experts), bailingmoe3, glm5-next (including MTP `attn_output`), kimi-linear, hy_v4 `attn_gate`, mistral-large4 `attn_q`." Kimi K3 계열이나 Mistral Large 4의 NVFP4 GGUF도 일부 스케일이 아직 빠진다는 뜻이다.
- **열린 문제 셋:**
  - CPU 전문가 분산(`--n-cpu-moe`)을 쓰면 출력이 `?`로만 나온다(#32778, #32710). 원문: "Once a corrupted request happens, the server stays broken". 커뮤니티 우회책 원문: "`--no-op-offload` fixes it, at any prompt length." 원문: "`-ub 16` also avoids it." 이슈 본문에는 원문 "Also reproduces with `-ngl 0` (pure CPU)"도 있어 원인은 확정되지 않았다.
  - 토크나이저가 HF 판과 다르다. PR #32799(18:52 UTC 병합) 원문: "337 vocab entries cannot be built by merge rules". 마지막 빌드 b12633(14:53 UTC)보다 늦게 병합돼, 조회 시점에는 이 수정이 든 릴리스 빌드가 없다.
  - 생각 예산이 다시 켜진다(#32836). 원문: "the reasoning budget is reached and the budget message is injected, but the model immediately opens a new thinking block."
- **Unsloth GGUF 저장소(04:22 UTC 갱신):** 메인 README에 원문 "If running in llama.cpp or elsewhere, use our"로 시작하는 안내를 넣어 메인라인용 파일을 가리켰다. 그런데 `Mainline_Support/README.md` 맨 위에는 원문 "Mainline llama.cpp will get **gibberish** until PR 32704 gets merged" 경고가 남아 있다. 직전 판에서 본 대로 #32704는 이미 b12611에 들어갔다. 같은 파일 원문: "The root files use Unsloth's looped layout and need Unsloth Studio / Desktop or Unsloth's llama.cpp prebuilts." 원문: "Rename each file to `.gguf` after downloading."
- **Ollama:** 협업자 답 원문: "All of the inkling-2 models are MLX, the labeling is incorrect." 라이브러리 페이지 표기가 틀렸다는 뜻이다. GGUF 경로는 여전히 llama.cpp PR #32695 병합을 기다린다.
- **할 일:** llama.cpp는 b12632 이상을 쓴다. MI300X·MI325X에서는 필수다. 전문가 일부를 CPU에 둔다면 `--no-op-offload`를 시도한다. 공식 수정이 아니라 커뮤니티 우회책이다. Unsloth 루트 GGUF는 메인라인 llama.cpp에서 읽히지 않는다.

[b12632](https://github.com/ggml-org/llama.cpp/releases/tag/b12632) · [#32789](https://github.com/ggml-org/llama.cpp/issues/32789) · [#32753](https://github.com/ggml-org/llama.cpp/issues/32753) · [PR #32669](https://github.com/ggml-org/llama.cpp/pull/32669) · [#32778](https://github.com/ggml-org/llama.cpp/issues/32778) · [PR #32799](https://github.com/ggml-org/llama.cpp/pull/32799) · [#32836](https://github.com/ggml-org/llama.cpp/issues/32836) · [Unsloth 안내](https://huggingface.co/unsloth/Inkling-2-Mini-GGUF/blob/main/Mainline_Support/README.md) · [Ollama #19817](https://github.com/ollama/ollama/issues/19817)

### 3. (새 도구) Fast-Anything: 느린 함수만 골라 코딩 에이전트에게 Rust로 옮기게 하고, 실제 호출을 재생해 결과가 같은지 확인한다 (커뮤니티, HN 04:10 UTC 170점대, v0.7.0 16:21 UTC)

- **한 줄(README 원문):** "finds those functions, has your coding agent port *only them* to Rust, proves the port gives the same results on real recorded calls, and swaps it in." MIT 라이선스다. 저장소는 11-21 14:40 UTC에 만들어졌다.
- **추천 이유:** 결과가 같을 때만 바꿔 끼운다. 원문: "Nothing is enabled until every case matches: same values (floats within 1e-9), errors where the original errors." 되돌리기도 쉽다. 원문: "`FAST_ANYTHING_DISABLE=1` runs the original any time." Claude Code 플러그인, MCP 서버, 스킬을 함께 준다.
- **함정:**
  - 에이전트가 승인 없이 돈다. 소스(`src/agent.rs`)를 보면 Claude Code는 `acceptEdits` 모드에 `Bash(cargo build:*)` 같은 허용 목록으로, Codex는 `--full-auto`로 실행한다.
  - 실제 호출을 기록한다. SKILL.md 원문: "It re-runs the app, records up to 500 real calls, and creates". 기록은 `.fast/ports/<id>/` 폴더에 쌓인다. 운영 데이터로 돌리면 입력값이 그대로 남는다. 이 폴더를 git에서 빼라는 안내는 찾지 못했다.
  - 속도 수치는 원문 "Real runs on an i5-10500, Windows 11" 기준 자체 측정이고 재현하지 않았다. HN에서는 전체 실행 시간 중 그 함수 비중이 작으면 효과도 작다는 지적이 많았다.

[저장소](https://github.com/EL4CTEO/fast-anything) · [HN](https://news.ycombinator.com/item?id=50596155) · 설치 `npm i -g @el4cteo/fast-anything` 또는 `cargo install fast-anything`, Claude Code에서는 `/plugin marketplace add EL4CTEO/fast-anything`

### 4. (보안 수정) browser-use 1.0.8: 비밀번호 칸에 입력한 값이 에이전트 기록과 judge 프롬프트에 평문으로 들어가던 문제를 고쳤다 (공식, 03:30 UTC)

- **수정(릴리스 원문):** "`bu.type` never prints a password field's value (#6455)." PR #6455 원문: "agents reported that the line puts the plaintext password into the agent trace and logs, against their "never log passwords" rule. The same line also goes into the judge prompt (1.0.5+)." 저장소 보안 권고는 없다.
- **동작 변경:** API 키 없이 SSH·헤드리스 환경에서 돌리면 기기 승인 로그인을 띄운다. 원문: "No API key over SSH or on a headless machine: the run prints a link and a code to approve on any device, then continues (#6427)." 끄는 법(PR 원문): "`BROWSER_USE_AUTO_LOGIN=0` turns it off." CDP 연결도 바뀌었다. 원문: "`Browser(cdp_url=...)` attaches the one page already open instead of a new blank tab (#6456)."
- **함정:** 1.0.5~1.0.7로 로그인 작업을 돌렸다면 로그와 judge 모델 제공자 쪽에 비밀번호가 남았을 수 있다. 그 비밀번호는 바꾼다. CI에서 헤드리스로 돌린다면 자동 로그인 안내가 뜨지 않게 환경 변수를 둔다.

[1.0.8](https://github.com/browser-use/browser-use/releases/tag/1.0.8) · [PR #6455](https://github.com/browser-use/browser-use/pull/6455) · [PR #6427](https://github.com/browser-use/browser-use/pull/6427)

### 5. (기본값 변경) LMCache v0.5.8: 코디네이터가 기본으로 로컬에만 열리고, CUDA 12.9 빌드가 빠졌다 (공식, 15:39 UTC)

- **릴리스 원문:** "Protect service boundaries with API-key authentication on gRPC/ZMQ endpoints and a loopback-only coordinator bind by default." 원문: "ship NIXL separately as `lmcache-nixl`, retire CUDA 12.9 images/wheels". 원문: "remove the legacy in-tree `InProcess` implementation".
- **기본값(PR #6606 원문):** "Changes the MP coordinator default bind address from `0.0.0.0` to `127.0.0.1`, so a no-argument startup is reachable only from the local machine." 원문: "Multi-host deployments can still opt in explicitly with `--host 0.0.0.0`".
- **함정:** 여러 호스트에 걸친 배포는 `--host`를 명시해야 붙는다. API 키 인증은 켜야만 동작한다. PR #5770 원문: "No configured keys preserves unauthenticated compatibility." 켤 때 순서도 있다. PR #5769 원문: "Upgrade the deployment before enabling keys: old servers cannot understand the new credential frame, and old clients cannot authenticate." 원문: "API keys do not encrypt traffic." cu129 이미지를 쓰던 곳은 원문 "no new `-cu129` wheels or image tags will be published."(PR #6533)에 따라 다른 빌드로 옮긴다. NIXL을 쓰면 `lmcache[nixl]`로 설치한다.

[v0.5.8](https://github.com/LMCache/LMCache/releases/tag/v0.5.8) · [PR #6606](https://github.com/LMCache/LMCache/pull/6606) · [PR #5769](https://github.com/LMCache/LMCache/pull/5769)

### 짧게

- **LookAtMyQuant 후속(직전 판 도구 3번):** NVIDIA CUDA 실행이 들어왔다(PR #12, 17:40 UTC 병합). 직전 판의 "Mac 전용" 함정은 이제 맞지 않는다. 원문: "CUDA supports single-sequence text generation." 원문: "The native engine requires an NVIDIA driver for Ampere (SM 80) or newer." HF 5.4GB 체크포인트는 07:03 UTC에 교체됐고, README 수치도 원문 "`87.0%` GSM8K, and `71.3%` strict HumanEval (`117 / 164`)"로 바뀌었다. 자체 측정이다. [PR #12](https://github.com/ShinMegamiBoson/lookatmyquant/pull/12) · [CUDA 문서](https://github.com/ShinMegamiBoson/lookatmyquant/blob/main/runtime_cuda/README.md)
- **Gall(Show HN, v0.1.0 21:36 UTC, MIT):** 24GB Apple Silicon Mac용 Inkling 2 Mini 전용 엔진이다. 원문: "It does not use ggml or MLX." 원문: "On an M2 MacBook Air, Gall generates about 20 tokens per second." 함정 원문: "The current release is not notarized by Apple". 원문: "other machines and macOS versions have not been tested". [저장소](https://github.com/ltrgoddard/gall)
- **Muru(Simon Willison 글 11-21 23:56 UTC, HN 70점대):** mod_wsgi 저자가 AI를 써서 만든 Python 앱 서버다. 원문: "an MCP server lets an agent deploy each change as a release, test it, read what Muru observes when something fails, and roll back." 원문: "Muru is in alpha". [저장소](https://github.com/muruhq/muru) · [Simon Willison](https://simonwillison.net/2026/Nov/21/muru/)
- **Worker 755(Show HN 13:56 UTC, 250점대):** 사용자가 LLM 관리자의 서브에이전트가 되는 브라우저 게임이다(MIT). 자기 API 키를 넣으면 게임 서버로 간다. 원문: "The key is sent only to the game server when a shift starts. It is kept in that shift's memory and never logged or stored." 키를 넣기 싫으면 로컬 모델이나 `npm run mock`으로 돌린다. [저장소](https://github.com/gulivan/worker755) · [HN](https://news.ycombinator.com/item?id=50599762)
- **MONAI Label 0.8.6:** 직전 판 기술 6번의 경로 탈출 두 건을 고친 보안 판이다. Python 3.11 이상이 필요하다. 기술 이슈 4번에 정리했다.
- **Qwen Code 0.28.4(12:04 UTC):** 원문: "No known breaking changes." 원문: "Local Runtime now creates its state directory at process spawn with owner-private permissions." [릴리스](https://github.com/QwenLM/qwen-code/releases/tag/v0.28.4)
- **Vercel AI SDK v6 라인(ai@6.0.336, 22:25 UTC):** 게이트웨이 의존성 갱신뿐이다. 7.0.178의 도구 승인 서명 수정은 여전히 v6로 백포트되지 않았다. [릴리스](https://github.com/vercel/ai/releases/tag/ai%406.0.336)
- **주의: FREELLM_INSTALL(창 안 생성 19:44 UTC):** "무료 LLM 게이트웨이"를 내세운 저장소다. 파일은 README와 LICENSE뿐이고, 실행 파일은 릴리스의 암호 걸린 `FreeLLM.zip`이다. 계정은 11-14에 만들어졌고, 몇 시간 만에 별 250개와 포크 34개가 붙었다(조회 시점). 악성 여부는 확인하지 않았다. 암호 걸린 압축 실행 파일과 급조된 별은 악성 배포의 흔한 패턴이다. 내려받지 않는다.
- **직전 판 후속 중 새 진전 없음:** Claude Code(dist-tags `latest`·`next` 2.1.330, `stable` 2.1.323 그대로. #110979·#110642·#111167·#111218·#111068 모두 열려 있음), Agent SDK Python(0.2.187)·TS(0.3.330), claude-code-action(1.0.286), Cline(CLI 3.0.75 그대로, yolo 기본값 되돌림 없음), Codex CLI(안정판 0.171.0, 알파만), Pi(1.6.0), OpenAI Agents SDK, LiteLLM(안정판 1.110.2라 키 전송 수정은 아직 LTS·개발판에만 있음), FastMCP(4.2.4), transformers(5.24.0), goose(#13663은 병합됐지만 1.60.2 이후 릴리스 없음), pydantic-ai PR #14095(열림), OpenCode(2.3.2), Mistral Vibe(3.1.4), VS Code(1.147.0), Gemini CLI·Crush(nightly만), Copilot CLI(프리릴리스만), Kimi Code(2.7.2 그대로라 Plan 모드 우회 수정판 없음), vLLM(0.34.0, #69046 새 댓글 없음), SGLang PR #51926(열림), Ollama(v0.40.17), MCP 공식 SDK·Inspector·레퍼런스 서버, GitHub MCP Server, Playwright MCP, Open WebUI, Strands, crewAI, Hermes Agent, Context7, Dify, Cursor, Mastra. AutoGPT 0.12.0 보안 권고는 아직 공개되지 않았다. `anthropics/skills`·`claude-plugins-official`·`openai/plugins`는 창 안 커밋이 없다.

## 주목할 점

- **"얼마나 앞서 있는가"를 공개하는 경쟁이 시작될 수 있다.** OpenAI는 출시 모델보다 앞선 내부 모델의 사전학습 지표를 공개하고, 공동 한도를 제안했다. 같은 주 METR은 2년 안 AI R&D 급가속을 배제할 수 없다고 했다. Anthropic·Google이 같은 지표를 공개할지, 한도 제안에 답할지 지켜본다. 측정 데이터셋이 랩마다 다르다는 문제가 먼저 풀려야 비교가 된다.
- **코딩 에이전트가 공급망의 새 구멍이 됐다.** Together AI 사고는 에이전트가 커밋한 키 하나에서 시작됐다. MCP 레지스트리에는 남이 가져갈 수 있는 패키지 이름이 649개 항목에 걸려 있다. browser-use는 비밀번호를 judge 프롬프트에 넣고 있었다. 에이전트가 쓰는 저장소의 비밀값 스캐닝, MCP 서버 버전 고정, 에이전트 로그의 비밀값 점검을 기본으로 둔다.
- **다가오는 일정:**
  - 이번 주: KEV 4건 조치 기한(11-23), **Ruby2ßhell Rails PoC 공개(11-25, GHES 패치로 연기 사유가 줄었다)**, ERNIE 5.5 Flash Preview 가중치 공개 약속(11-25 전), Qwen4-Max-Preview 종료(11-25), Gemini 4 Carbon·Argon 도입가 종료(11-30), Huawei openPangu-2.0 종료(11-30).
  - 12월: Puppet Enterprise KEV 조치 기한(12-01), LiteLLM KEV 조치 기한(12-02), Backblaze 새 개인정보 처리방침 시행(12-01), **EU·EFTA·영국 Copilot 테넌트 Claude 기본 켜짐(12-09)**, Val Town 읽기 전용(12-10), GitHub Copilot 모델 6종 은퇴(12-14), Inception Mercury API 종료(12-28).
  - 그 뒤: BIS 원격 접근 규칙 의견 마감(2027-01-19), Windows 기본 소형 모델 Aion 탑재(2027년 1월 말), 다음 MCP 명세(03-30).

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - THN·SCMP·Debian 메일링 리스트·GHES 릴리스 노트 일부는 r.jina.ai로 텍스트를 뽑았다. IHES 노트는 갱신본을 받았지만 구판과 내용을 대조하지 못했다.
  - Lean Zulip은 인증이 필요해 검증 논의를 직접 보지 못했다. Reuters(캡차), WSJ·Bloomberg(유료)는 열지 못했다. x.com 타임라인과 reddit은 막혀 있어 Anthropic 직원 글과 MiniMax 계정을 확인하지 못했다.
- **시각이 날짜만 있거나 불확실한 원문:**
  - OpenAI Pachocki 글은 RSS 날짜(11-22)만 있어 HN 첫 게시(17:06 UTC)로 상한을 잡았다. OpenRouter 글은 날짜만 있어 Wayback 첫 캡처(01:25 UTC)로 보완했다.
  - Together AI 공지는 Google News 시각(11-21 23:18 UTC)으로 잡았다. Novita는 메타 시각(10:00 UTC)이 Wayback 첫 캡처(03:11 UTC)보다 늦어 메타를 믿지 않았다. Atlas Cloud는 첫 게시 시각을 모른다.
  - GHES 패치, Perplexity Monitor API, AHM 성명은 날짜만 있다. LingSante 공개 시각은 ModelScope 등록 시각과 HF 첫 토론 글로 추정했다.
- **확인하지 못한 것:**
  - OpenRouter가 퇴출한 세 곳의 이름, Dragos가 말한 침해 제공자 셋과 같은지. Together가 자격 증명을 폐기했는지. MiniMax 공식 입장.
  - OpenAI 그래프의 "Millennium Prize" 라벨이 가리키는 문제. 리만 가설 증명에 쓰인 모델 이름과 Anthropic의 입장.
  - LingSante 벤치마크 수치(이미지로만 있음), Tencent Hy4 안전 보고서 원문, FREELLM 압축 파일의 악성 여부, llama.cpp 우회책 `--no-op-offload`의 공식 판정.
  - 재현하지 않은 것: 모든 증명 검증 결과와 벤치마크(OpenAI 퍼플렉서티, AA, LookAtMyQuant, Gall, Fast-Anything, llama.cpp PPL), MCP 레지스트리 점검 수치, 모든 취약점 주장. 도구와 패키지는 하나도 설치하거나 실행하지 않았다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 80점 이상 스토리는 제목을 전부 봤다. 20점 초과 스토리 50개는 모두 봤고, 10점 이상은 키워드로 걸렀다. Show HN은 5점 이상을 봤다.
  - 전역 GHSA는 reviewed 0건, unreviewed 137건, malware 10건을 창 범위로 받아 셌다. unreviewed는 AI 키워드로 걸렀다. 저장소 권고는 AI·도구 저장소 약 108곳을, 도구 릴리스는 약 120곳을 봤다.
  - MCP 레지스트리의 창 안 갱신 1,000건(조회 상한)은 대형 업체 네임스페이스만 걸렀다. HF 조직 스캔은 조직당 15개만 봤고, 401 추적 목록은 저장소별로 직접 조회했다. HF Daily Papers는 비어 있었고 arXiv는 공지가 없는 창이다.
  - 한국 랩은 HF 조직 스캔과 국내 기사 제목 검색만 했다. Vertex AI 릴리스 노트(캐시 동결)와 Moonshot·Volcengine 변경 로그(월 단위)는 일 단위로 판정하지 못했다.
  - 창이 끝날 때 열려 있던 것: Lean comparator 실행과 커널 재검증, llama.cpp 토크나이저 수정이 든 릴리스 빌드와 #32710·#32778·#32836, OpenSSL 4.0.4·4.1.1, Ubuntu PHP·Ruby·Ghostscript 패치, Claude Code #110979·#110642·#111167·#111218·#111068, Inkling 2 Mini의 SGLang·Ollama GGUF 지원과 vLLM 정식 릴리스, LiteLLM 안정판 키 전송 수정, Kimi Code 수정판, AutoGPT 0.12.0 보안 권고, Microsoft 저장소 복구. 다음 판에서 다룬다.
