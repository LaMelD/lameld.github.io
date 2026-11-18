---
title: "2026-11-19 AI 브리핑"
date: 2026-11-19T08:00:00+09:00
tags: [ai-briefing, google, anthropic, security, mcp]
description: "Google이 Gemini 4 Neon을 내고 Anthropic 공모가는 $83로 보도됐으며, AI로 찾은 Nginx 인증 전 RCE와 Microsoft 저장소 비공개 전환, AutoGPT·MCP 서버 권고가 이어졌고 GPT-6.2는 instructions 캐시 함정으로 도구들의 비용을 키웠다."
---

> 조사 범위: 2026-11-18 08:00 ~ 2026-11-19 08:00 KST(2026-11-17 23:00 ~ 11-18 23:00 UTC, 미국 화요일 15:00 ~ 수요일 15:00 PST). 직전 브리핑(11-18 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, SEC EDGAR `acceptanceDateTime`, GitHub `published_at`·`merged_at`·커밋 시각, npm·PyPI 게시 시각, 저장소·전역 GHSA `published_at`, CISA KEV, 상태 페이지 API, Hugging Face API, Wayback 캡처, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-18 23시대 UTC) 값이다.

## 오늘의 핵심 요약

- **Google이 Gemini 4 Neon을 냈다.** 정가는 입력 $3, 출력 $15이고, 외부 지수(Artificial Analysis)는 61로 Carbon보다 높다. 하지만 느리고 장황해서 과제당 비용은 GPT-6.2 Astra보다 많이 들었다. 11-30까지는 Carbon 도입가가 더 싸다. 전체 위험 평가는 하지 않고 Argon 판정을 이어 썼다. Anthropic 공모가는 범위 위인 $83로 정해졌다는 보도가 나왔지만, 창이 끝날 때까지 회사 발표와 최종 투자설명서는 없었다.
- **AI가 찾은 취약점이 또 인프라를 흔들었다.** Nginx HTTP/2에서 인증 전 원격 코드 실행 2건이 나왔고, 호스트 root 체인과 PoC가 공개됐다. 배포판 패키지가 아직 없으면 HTTP/2를 끈다. Microsoft는 "악성 콘텐츠 조사"를 이유로 agent-framework와 ONNX Runtime 저장소를 비공개로 돌렸다. AutoGPT는 권고 48건을 냈고, MCP git 서버는 "저장소 설정만으로 명령 실행"을 세 번째로 막았다. CISA KEV에는 LiteLLM이 들어갔다.
- **도구는 비용과 기본값부터 본다.** GPT-6.2는 Responses API의 `instructions`를 캐시하지 않아, 시스템 프롬프트를 거기에 넣던 하네스의 입력 비용이 크게 늘었다. 고친 도구와 못 고친 도구가 섞여 있다. Claude Code 2.1.327은 승인 전에 셸 시작 파일을 실행한다. VS Code 1.147은 Claude·Codex 하네스를 허용하지만, 그 세션에는 Copilot의 관리 통제가 걸리지 않는다. LiteLLM 1.110, goose 1.60, OpenCode 2.3.1은 호환성을 깼다.

## 모델 소식

미국 수요일이 통째로 들어온 창이다. Google이 Gemini 4 Neon을 냈다. Anthropic 공모가는 $83로 정해졌다는 보도가 나왔지만, 창이 끝날 때까지 회사 발표와 최종 투자설명서(424B4)는 없었다. 공식 변경 로그는 Gemini API(11-18)와 ChatGPT 릴리스 노트(11-18)만 움직였다. 그대로인 곳은 Claude Platform(11-17), OpenAI API(11-17), xAI(11-02), DeepSeek(11-13), Mistral(10월 말)이다. OpenRouter 신규 등록은 `google/gemini-4-neon`과 `:batch` 2건뿐이다.

### 1. Google Gemini 4 Neon: Carbon과 Hydrogen 사이의 모델이다. 외부 지수는 Carbon보다 높지만, 11-30까지는 Carbon이 더 싸다 (공식, blog.google 11-18 21:00 UTC, HN 300점대)

- **무엇:** `gemini-4-neon`이 Gemini API 유료 티어에 GA로 나왔다. 같은 날 Gemini 앱(Pro·Ultra), Gemini for business, Antigravity, GitHub Copilot(21:49 UTC)에도 들어갔다. 모델 카드 원문: "Gemini 4 Neon is adapted from Gemini 4 Carbon." 컨텍스트는 1M이고, 사고 수준은 `low`·`medium`·`high`(기본)·`xhigh` 4단계다.
- **가격(1M 토큰당):** 원문: "The model is priced at $3 per million input tokens ($0.15 per million cached input tokens, a 95% discount) and $15 per million output tokens, with a 1 million-token context window." 긴 입력 할증은 없다. 가이드 원문: "It uses flat pricing across the full 1M context window". 272K 토큰을 넘으면 단가가 오르는 GPT-6.2와 다르다.
- **외부 측정(Artificial Analysis, 조회 시점):** Google 원문: "Neon ranks ahead of comparable models and #3 overall with a score of 61 on the Artificial Analysis Intelligence Index at its xhigh thinking level". 모델별 최고점 기준으로 Claude Fable 5.2 64.6, GPT-6.2 Astra 62.1, Gemini 4 Neon 61.4, Gemini 4 Carbon 60.0이다. 다만 AA 원문: "It's also notably slow per task and very verbose." 지수 과제당 비용은 Neon $6.44로 GPT-6.2 Astra($4.00)보다 많이 들었다. 블로그의 "36%+ less per task" 주장과는 맞지 않는다.
- **안전성(모델 카드):** 전체 위험 역량 평가는 하지 않았다. 원문: "however this falls below our MCC threshold and therefore a full suite of critical capability evaluations was not required in line with our FSF." 생물·사이버·통제 상실 판정은 Gemini 4 Argon 것을 그대로 쓴다(모두 "cannot be ruled out"). 원문: "The full Safety & Security Report for Gemini 4 Neon will be available in the coming days."
- **함정(마이그레이션 가이드 원문):**
  - "Neon is available on the paid tier only (not on the free tier)."
  - "Gemini 4 Neon does not support explicit caching (`cachedContents` and `caches.create`)."
  - "Remove `temperature`, `top_p`, and `top_k` from requests." 범위 밖 값은 400 오류다.
  - "Neon defaults to `high` rather than `medium`, adds `xhigh`, and rejects `minimal` with an error."
  - "If you set `background` to `true` without setting `max_output_tokens`, the server stops after 65,536 tokens."
  - 11-30까지는 Carbon 도입가(입력 $2, 출력 $10)가 Neon 정가보다 싸다. Carbon은 12-01부터 $4·$20이다.
  - Copilot 관리자: 원문: "Under default model enablement, new models are automatically enabled unless an administrator has turned off the global default or explicitly disables this model." Antigravity는 모델을 고르지 않은 사용자의 기본 모델을 Neon으로 바꿨다.
- **왜 중요한가:** The Decoder는 한 달 새 네 번째 Gemini 모델이라고 썼다. 그런데 위험 평가는 새 모델마다 하지 않고 이전 모델 판정을 이어 쓴다. 성능은 Carbon보다 높게 나왔지만 느리고 장황하다는 외부 평가가 같이 나왔다. 값만 보고 바꾸지 말고 과제당 비용으로 잰다.

[Google 블로그](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-neon/) · [모델 카드](https://storage.googleapis.com/deepmind-media/Model-Cards/Gemini-4-Neon-Model-Card.pdf) · [마이그레이션 가이드](https://ai.google.dev/gemini-api/docs/whats-new-gemini-4-neon) · [가격](https://ai.google.dev/gemini-api/docs/pricing) · [Artificial Analysis](https://artificialanalysis.ai/models/gemini-4-neon) · [Copilot](https://github.blog/changelog/2026-11-18-gemini-4-neon-in-github-copilot) · [HN](https://news.ycombinator.com/item?id=50552424)

### 2. Anthropic 공모가 $83 보도. 범위($70~78) 위다. 창이 끝날 때까지 회사 발표와 최종 투자설명서는 없다 (매체보도 + 공식 SEC)

- **SEC(창 안):** 일본용 투자설명서 FWP(01:08 UTC)와 Nasdaq의 상장 증명(CERT, 14:12 UTC)이 접수됐다. 조회 시점(11-18 23:25 UTC)까지 최종 투자설명서 424B4는 없다. anthropic.com에도 가격 발표는 없다.
- **보도:** The Register(21:40 UTC) 원문: "Late on Wednesday, Bloomberg reported that Anthropic would price at $83, citing people familiar." CNBC 원문: "Anthropic priced its IPO at $83 per share on Wednesday, a person familiar told CNBC, above the expected range". TNW(22:47 UTC) 원문: "At $83, Anthropic is worth about $1.49tn on a fully diluted basis, the Financial Times reported".
- **확정되지 않은 것:** 발행 주식 수와 조달액이다. 창 안 보도는 엇갈렸다. TNW 원문: "The company also sold more shares than it planned." Motley Fool(22:17 UTC) 원문: "Bloomberg and Reuters both reported that Anthropic was likely to upsize the offering (sell more shares than originally planned), but Anthropic hasn't disclosed how many shares it's selling." 거래 시작(11-19 Nasdaq, 티커 ANT)은 창 밖이다.
- **(창 직후) CNBC 23:23 UTC 갱신:** 원문: "Anthropic upsized its offering and sold 979.6 million shares, raising $81.3 billion." 익명 소식통 기준이다. 424B4가 나오면 다음 판에서 숫자를 확정한다.

[The Register](https://www.theregister.com/ai-and-ml/2026/11/18/anthropic-decides-to-go-head-to-head-with-gta-vi-release-schedules-ipo-for-the-same-day/5310872) · [CNBC](https://www.cnbc.com/2026/11/18/anthropic-ipo-ai-offering-price.html) · [TNW](https://thenextweb.com/news/anthropic-ipo-prices-83-share-1-5-trillion-record-raise) · [Motley Fool](https://www.fool.com/investing/2026/11/18/it-s-official-anthropic-prices-its-ipo-at-usd83-a-share-here-s-what-we-know-and-what-we-don-t/) · [EDGAR 목록](https://data.sec.gov/submissions/CIK0002130357.json)

### 3. GPT-6.2 외부 평가 추가: 수학 평가 MathArena에서는 Astra·Sol이 1·2위이고 비용도 낮다 (외부 평가, 갱신 11-18)

- **MathArena:** 원문: "November 18: We added results for GPT-6.2 Astra and GPT-6.2 Sol on October ArXivMath, October BrokenArXiv, and September ArXivLean." 페이지 데이터에서 읽은 기대 성능과 비용은 GPT-6.2 Astra(max) 92.2%·$2.67, GPT-6.2 Sol(max) 89.5%·$0.96, Gemini 4 Carbon(high) 85.4%·$15.70, Claude Fable 5.2(max) 82.4%·$38.89다.
- **그 밖:** Artificial Analysis에서 GPT-6.2 Sol(Max)은 57점이다. ARC-AGI-3 표준 하네스에서 Sol은 54.9%다.
- **왜 중요한가:** 직전 판의 범용 지수(AA)와 ARC-AGI-3에서는 Fable 5.2가 앞섰다. 수학 지표에서는 GPT-6.2가 정확도와 비용 모두 앞선다. 어느 모델이 낫다고 하려면 작업 종류부터 정해야 한다. GPT-6.2를 API로 쓴다면 프롬프트 캐시 함정(도구 1번)을 먼저 확인한다.

[MathArena](https://matharena.ai/) · [AA GPT-6.2 Sol](https://artificialanalysis.ai/models/gpt-6-2-sol) · [ARC Prize](https://arcprize.org/leaderboard)

### 4. Tencent Hy4 flash 가중치 공개: 총 220B, 활성 12B. transformers 정식판에서는 아직 안 돈다 (공식 HF, 11-18 04:24 UTC 무렵)

- **무엇:** 직전 판까지 401이던 `tencent/Hy4-flash`, `-FP8`, `-base`가 공개됐다. 마지막 커밋이 04:24 UTC, Wayback 첫 정상 캡처가 04:32 UTC다. README 원문: "is our most cost-effective Hy model to date, featuring 220B total parameters (including 64B Engram parameters) with just 12B active parameters per token." 컨텍스트는 1M이다.
- **라이선스:** Tencent Hy License 1.0이다. 원문: "This License is derived from the Apache License, Version 2.0". 서비스형으로 제공하는 사업자는 일정 규모를 넘으면 별도 허가를 받아야 한다. 원문: "(a) if Your aggregate worldwide gross revenue exceeds US$50 million". 지역 제한 문구는 없다(ERNIE 5.5와 달리 한국 제외 조항 없음).
- **함정:** README 원문: "Support for Hy4 flash will be merged into the `transformers` main branch soon." 그 전까지는 PR #50903 헤드를 설치해야 한다. vLLM은 전용 이미지(`vllm/vllm-openai:hy4-flash`)에 TP 8 예시다. 본체 `tencent/Hy4`는 여전히 401이다.

[HF](https://huggingface.co/tencent/Hy4-flash) · [LICENSE](https://huggingface.co/tencent/Hy4-flash/blob/main/LICENSE)

### 5. Microsoft Aion 1.0 Instruct(2.3B, MIT): 2027년 1월 말 Windows 기본 소형 모델이 될 모델이다. 한국어는 지원 언어에 없다 (공식 HF, 11-18 17:05 UTC)

- **무엇:** Windows 팀이 HF에 올렸다. ONNX-WebGPU 판도 있다. 컨텍스트는 32K다. 원문: "A further optimized version of Aion Instruct, coupled with Microsoft’s content moderation model, will become the Windows OS SLM by the end of January 2027." 원문: "We are publishing the unquantized model first, to be followed by quantized versions."
- **함정:** 지원 언어는 영어·독일어·스페인어·포르투갈어·이탈리아어·프랑스어·중국어·일본어 8개다. 한국어가 없다. `trust_remote_code`가 필요하다. 원문: "Aion Instruct may be susceptible to adversarial attacks." 공식 블로그 발표는 찾지 못했다.
- **왜 중요한가:** Windows에 기본으로 깔릴 온디바이스 모델이 미리 공개됐다. Windows 앱에서 이 모델을 쓸 계획이라면 한국어 품질부터 시험해 본다.

[HF](https://huggingface.co/microsoft/Aion-1.0-Instruct-2.3B)

### 6. OpenAI: 모델로 칩 설계를 개선했다는 글, 위험 관리 정리 글, ChatGPT 받아쓰기 개편 (공식, RSS 11-18)

- **칩 설계(HN 첫 게시 17:45 UTC, 130점대):** 원문: "We have partnered with AMD on PPA improvements and seen up to 14% improvement relative to heavily optimized baselines on the flagship GPU IP." 원문: "This represents roughly a year of progress achieved in less than 5 months–unprecedented results at the frontier of chip design." 원문: "AMD has already included some of these changes in a recent design it sent for manufacturing". OpenAI가 전한 수치다.
- **위험 관리 정리(RSS 06:00 UTC):** 학습부터 배포까지의 안전 장치를 정리한 글이다. 7월 사고를 두고 원문: "The Hugging Face incident showed why we should not rely on sandboxing alone to contain highly capable models." 10-26 판에서 다룬 학습 중 DNS 사고와 학습 중단도 원문: "We paused training and evaluation of our most capable models from September 20th to October 1st"로 다시 적었다. 새 정책 발표라기보다 정리 글이다. 캘리포니아 소송(직전 판 4번) 언급은 없다.
- **ChatGPT(릴리스 노트 11-18):** 원문: "Live Dictation is rolling out globally to all ChatGPT plans on iOS, Android, web, and the desktop app." 원문: "We’ve also removed the 10-minute recording limit." 회계 플러그인 Accounting도 나왔다. 원문: "In Enterprise workspaces, an admin must approve Accounting before members can install it."
- **상태 페이지:** Free·Go 사용자 대화 오류가 12:15~14:13 UTC에 있었다. 직전 판에서 monitoring이던 Costs API 지연은 17:15 UTC에 해소됐다(게시부터 약 23시간 반).

[칩 설계](https://openai.com/index/advancing-chip-design-with-our-models/) · [위험 관리](https://openai.com/index/managing-frontier-ai-risk/) · [ChatGPT 릴리스 노트](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) · [상태](https://status.openai.com/) · [HN](https://news.ycombinator.com/item?id=50549493)

### 7. Anthropic: 사이버 검증 프로그램의 상위 등급을 Enterprise·API로 한정했다. Claude가 이론물리 미해결 문제 2개를 풀었다는 연구도 냈다 (공식)

- **Cyber Verification Program(수정 21:55 UTC):** 원문: "Updated November 18, 2026, to reflect changes to plan availability for Red Team Access and Specialized Access, and to clarify who qualifies for Specialized Access." 원문: "Like Red Team Access, this tier is available only on Enterprise and API plans." 원문: "Operating critical infrastructure doesn’t, by itself, qualify an organization for Specialized Access". 11-15 Wayback 사본과 대조해 바뀐 문장을 확인했다. Pro·Team 플랜으로 보안 연구 등급을 쓰던 곳은 자격을 다시 확인한다.
- **블랙홀 물리(페이지 날짜 11-17, 사이트맵 갱신·Wayback 첫 캡처 11-18 18:02 UTC, 직전 판 미수록):** 원문: "Claude has solved two open problems in theoretical physics." 두 번째 문제는 Fable 5.2가 풀었다. 원문: "Neither result describes our own universe directly, and neither has practical applications". 외부 검증은 확인하지 못했다.
- **상태 페이지:** Google Docs 커넥터 편집 오류가 있었다. 원문: "Impact occurred from 16:19 PT on November 17 / 00:19 UTC on November 18 to 17:33 PT on November 17 / 01:33 UTC on November 18." 직전 판에서 Team 플랜이 막혀 있던 Claude Meetings는 일부 풀렸다(11-17 23:39 UTC). 원문: "Team customers can again add Claude to new calls manually, and Auto-join is being restored gradually for Team customers."

[CVP](https://www.anthropic.com/news/cyber-verification-program) · [블랙홀 연구](https://www.anthropic.com/research/claude-black-hole-physics) · [Docs 커넥터](https://stspg.io/qz7tfhb7sl68) · [Meetings](https://stspg.io/01qq3kzyyqzl)

### 8. NVIDIA 후속과 미국 원격 GPU 접근 규칙 (매체보도 + 공식)

- **중국:** 직전 판에서 4분기 가이던스의 중국 데이터센터 매출을 0으로 잡았다고 썼다. The Verge(13:01 UTC)가 옮긴 Kress 임시 CEO 발언 원문: "Our guidance does not have any licensed Hopper data center products included, and we are no longer shipping licenced Hopper data center products to China". H200 중국 출하를 멈췄다는 뜻이다.
- **10-Q 지연:** TechCrunch(21:57 UTC)가 옮긴 발언 원문: "We plan to file our 10-Q in the coming weeks." 분기 보고서를 실적 발표와 함께 내지 않았다.
- **BIS 원격 접근 규칙:** 연방관보 공개 열람 목록에 올라왔다(문서 2026-23529). 게재일은 11-19(창 밖)이고, 규칙은 게재일에 발효된다. 원문: "The regulatory changes in this rule formalize the rescission of the AI Diffusion Rule". 해외 클라우드로 고급 GPU를 쓰는 서비스는 내용을 확인한다.

[The Verge](https://www.theverge.com/tech/1036445/nvidia-h200-ai-chips-china-stop-selling) · [TechCrunch](https://techcrunch.com/2026/11/18/nvidia-created-a-weird-bit-of-intrigue-by-not-filing-a-routine-sec-report/) · [연방관보](https://www.federalregister.gov/public-inspection/2026-23529/addressing-remote-access-to-advanced-semiconductors)

### 9. 짧게

- **Genie 4 사용자 공개(blog.google 18:00 UTC, HN 160점대):** 모델은 10-27에 발표됐다. 이번에는 사용 범위다. 원문: "Eligible Google AI Ultra subscribers can try the new Project Genie experience starting today, and Google AI Pro members can join the waitlist on the same page." [Google](https://blog.google/innovation-and-ai/models-and-research/google-deepmind/genie-4-now-available-project-genie/) · [HN](https://news.ycombinator.com/item?id=50550415)
- **vLLM Inkling 2 Small 지원 머지(PR #67325, 17:14 UTC):** 직전 판까지 미머지였다. 정식 릴리스(0.34.0 이후)에는 아직 없다. [PR](https://github.com/vllm-project/vllm/pull/67325)
- **DaisyRose-27B(RadixArk, Apache-2.0, HN 100점대):** Qwen3.8-27B에 RL을 더한 모델이다. 원문: "DaisyRose-27B improves Terminal-Bench 4.1 pass@1 from 5.6% to 12.0%." 자사 측정이다. [블로그](https://www.radixark.com/blog/daisyrose-27b) · [HN](https://news.ycombinator.com/item?id=50551046)
- **(커뮤니티) "Claude는 단백질 서열로 대규모 학습됐다"(HN 150점대):** 개인 연구자의 측정이다. 원문: "Fable 5.2 beats every open protein language model we ran, and Opus 5.5 is level with ProtGPT3-10B, the largest of them." Anthropic 확인은 없다. [블로그](http://xinmingtu.cn/blog/2026/claude-opus-5-5-protein-sequences/) · [HN](https://news.ycombinator.com/item?id=50541619)
- **(커뮤니티) "Gemini 4 Carbon은 왜 이렇게 오래 걸리나"(kernel.dev, HN 260점대):** 원문: "it spent close to an hour completing tasks that routinely took Opus and Astra only a few minutes". Neon에 대한 AA의 "slow per task" 평가와 같은 흐름이다. [kernel.dev](https://kernel.dev/blog/why-is-gemini-4-carbon-taking-so-long/) · [HN](https://news.ycombinator.com/item?id=50541790)
- **Meta Muse 캐나다 출시(14:07 UTC):** Loblaw가 첫 캐나다 유통 제휴사다. 같은 날 사내 개인정보 문제 보도가 나왔다(기술 7번). [Meta](https://about.fb.com/news/2026/11/meta-welcomes-loblaw-as-first-canadian-retail-partner-to-muse/)
- **Microsoft Ignite 2일차:** 새 MAI 모델은 없다. HN 390점대 "Seven new models"는 직전 판 모델 2번 글의 재게시다. Foundry Agent Service는 음성 에이전트와 computer use·브라우저 자동화를 GA로 냈다. [Foundry](https://devblogs.microsoft.com/foundry/whats-new-in-foundry-agent-service-at-ignite-2026/)
- **직전 판 후속:**
  - 여전히 401: Qwen4-Max, Qwen4-Max-Preview(11-25 종료), Tencent Hy4 본체, Ling 3.2 Flash, ERNIE 5.5 Flash Preview(11-25 전 공개 약속). LG K-EXAONE 2.5는 공개 목록에 없다.
  - GPT-6.2 시스템 카드 추가 수정 없음, ChatGPT 일반 대화창 배포 없음. 캘리포니아 소송에 대한 OpenAI 답변은 확인하지 못했다.
  - 소식 없음: DeepSeek 사용 제한·V5, Anthropic 30일 공개 정책, 'Mythos' 상표 소송, Edge0, Agnes, Kimi K3.1 Pro, LongCat-2.5, MetaEncoder, ChatGPT Wallet, 한국 랩 발표.
  - 그대로: Gemini 4 Carbon 도입가(11-30), Enterprise Claude Data 기본 켜짐(11-20), Microsoft 영국 Copilot Claude 기본값(12-09), Inception Mercury 종료(12-28).

### 참고: 창 경계 항목

- **(창 시작 전 가능성) Kuaishou Kling Image 4.0(TechNode 06:43 UTC):** 원문: "Kuaishou launched Kling Image 4.0 on Tuesday night". 중국 화요일 밤이면 창 시작 전이다. [TechNode](https://technode.com/2026/11/18/kuaishou-launches-kling-image-4-0-with-native-8k-generation/)
- **(직전 창) Sarvam-105B-1711(Apache-2.0, 105B MoE·활성 10.3B):** 첫 커밋이 11-17 14:09 UTC다. 창 안에는 vLLM 관련 수정 커밋만 있다. 벤치마크는 자사 수치다. [HF](https://huggingface.co/sarvamai/sarvam-105b-1711)

## 기술 이슈

AI로 찾은 인프라 취약점이 또 나왔다. 이번에는 Nginx다. Microsoft는 "악성 콘텐츠 조사"를 이유로 직전 판에서 침해된 agent-framework와 ONNX Runtime 저장소를 비공개로 돌렸다. 두 건이 연결됐는지는 확인되지 않았다. AutoGPT와 MCP 레퍼런스 서버는 권고를 한꺼번에 냈고, CISA KEV에는 LiteLLM이 들어갔다. 전역 GHSA 창 안 게시분은 reviewed 15건, unreviewed 1,241건(Oracle 11월 패치분 578건 포함), malware 54건이다(직접 셈).

### 1. Nginx HTTP/2 인증 전 원격 코드 실행 2건(CVSS 4.0 9.2). AI 스캐너로 찾았고, 호스트 root까지 가는 체인과 PoC가 공개됐다 (공식 + 발견자, Wiz 11-18 15:13 UTC, 릴리스 15:54 UTC, HN 1,000점대)

- **무엇:** nginx 1.31.9(mainline)와 1.30.7(stable)이 보안 항목 10개를 고쳤다. 핵심은 HTTP/2 결함 2건이다. Wiz 원문: "The vulnerabilities, tracked as CVE-2026-123633 and CVE-2026-123007, are both rated Critical, with a CVSS 4.0 score of 9.2." 백엔드 앱이나 서드파티 모듈 없이 정적 파일만 서비스해도 해당된다.
- **AI 사용(발견자 주장, 재현 안 함):** Wiz는 자사 AI 코드 스캐너로 찾았다고 했다. 원문: "Our researchers used Gemini 4 Carbon and ChatGPT 5.6 Sol to validate the impact of both vulnerabilities, confirming that each could allow an unauthenticated attacker to execute code on the server." 다른 팀도 같은 버그를 먼저 신고했다. 원문: "F5 confirms that another research team had previously reported the same vulnerability." Xint는 나머지 결함을 자동 스캔으로 찾았다고 했다. The Register 인용 원문: "A few days later, a fully automated Xint scan found the other four without human guidance." Xint는 커널 SCTP 취약점과 엮어 호스트 root까지 가는 체인(NGINX2Root)도 공개했다.
- **악용:** F5 원문: "At this time, we have found no evidence of active exploitation of any of these vulnerabilities in the wild,". 다만 Rapid7 연구자의 PoC 저장소가 공개돼 있다.
- **할 일과 함정:**
  - 올린 뒤 재시작한다. 원문: "Restart nginx after updating. `nginx -s reload` does not load the new binary."
  - 버전 문자열만 보고 판단하지 않는다. 원문: "Distribution packages, appliances, and container images may backport a security fix while retaining an older version string."
  - 배포판 패키지는 조회 시점에 아직 없다. Debian은 CVE-2026-123007이 모든 릴리스에서 미수정이고, Ubuntu LTS는 "needed"다. 바로 못 올리면 HTTP/2를 끈다(1.25.1 이상은 `http2 off;`).
  - 노출 규모 원문(Xint, Shodan 11-10 기준): "98.9% of hosts with identifiable nginx versions ran releases within the affected version range."

[Wiz](https://www.wiz.io/blog/nginx-http2-rce-vulnerabilities) · [nginx CHANGES](https://nginx.org/en/CHANGES) · [NGINX2Root](https://nginx2root.com/) · [The Register](https://www.theregister.com/security/2026/11/18/ai-found-more-critical-bugs-in-another-widely-used-technology-running-225m-websites/5310887) · [HN](https://news.ycombinator.com/item?id=50547374)

### 2. Microsoft가 ONNX Runtime 계열과 agent-framework 저장소를 비공개로 돌렸다. "악성 콘텐츠 조사 중"이라는 설명뿐이다 (매체 인용 회사 입장 + 직접 확인, HN 11-17 23:18 UTC, The Register 17:23 UTC)

- **무엇:** 조회 시점(11-18 23:03 UTC)에 `microsoft/onnxruntime`, `onnxruntime-genai`, `onnxruntime-extensions`, `onnxruntime-github-actions` 등 ONNX Runtime 계열 7개와 `agent-framework`, `Foundry-Local`이 404다. `semantic-kernel`, `autogen`, `playwright-mcp`는 열려 있다.
- **회사 입장(The Register 인용):** 원문: "We have made some repositories private while we investigate malicious content," 같은 기사 원문: "neither Microsoft nor security researchers have linked the ONNX repo’s pull to the malware campaign." MSRC 블로그에는 글이 없다.
- **agent-framework 악성 판(직전 판 기술 1번) 후속:** 전역 GHSA malware [GHSA-764m-3ppc-5m73](https://github.com/advisories/GHSA-764m-3ppc-5m73)이 00:31 UTC에 올라왔다. 이제 Dependabot 알림이 간다. 원문: "Versions 1.26.1 and 1.26.2 add a credential stealer that is not present in the last clean release, 1.26.0." 침해 경로는 OX Security의 주장으로만 나왔다. 원문: "the attacker later uploaded 1.26.2 through Microsoft’s official PyPI publisher account". Microsoft가 업로드 자격 증명을 폐기했는지는 확인되지 않았다.
- **영향:** 패키지는 그대로다. PyPI `onnxruntime` 1.32.0(11-13)과 npm `onnxruntime-web` 이후 새 판은 없다. 깨지는 것은 GitHub에서 소스, 릴리스 자산, Actions(`onnxruntime-github-actions`)를 직접 받는 빌드다. HN 첫 댓글 원문: "We found out about this this morning, when evals started failing in our pipelines."

[The Register](https://www.theregister.com/security/2026/11/18/microsofts-official-python-agent-framework-hijacked-to-distribute-malware/5310858) · [GHSA-764m-3ppc-5m73](https://github.com/advisories/GHSA-764m-3ppc-5m73) · [HN](https://news.ycombinator.com/item?id=50539610)

### 3. AutoGPT Platform 저장소 권고 48건(critical 2, high 8). 수정판은 이미 나와 있지만 Docker 이미지 번호가 다르다 (공식 저장소 권고, 11-17 23:09~23:16 UTC)

- **무엇:** 직전 판이 "창 직후"로 넘긴 건이다. 직접 세니 critical 2, high 8, medium 37, low 1이다. CVE 번호가 없고 전역 GHSA에도 없다. 그래서 Dependabot 알림이 오지 않는다. 최신 릴리스 0.11.3(11-15)까지 모두 고쳐진 뒤에 공개됐다.
- **critical 2건(0.11.0에서 수정):**
  - [GHSA-gfrp-hrh7-x8x8](https://github.com/Significant-Gravitas/AutoGPT/security/advisories/GHSA-gfrp-hrh7-x8x8)(9.6): 원문: "Any signed-up user of an affected AutoGPT Platform deployment could reach the AgentMail inboxes and Exa Websets of other users". 당장 못 올리면 `AGENTMAIL_API_KEY`·`EXA_API_KEY`를 비운다.
  - [GHSA-mwr6-g4xq-4rvp](https://github.com/Significant-Gravitas/AutoGPT/security/advisories/GHSA-mwr6-g4xq-4rvp)(9.1): 남의 workflow에 버전을 붙여 Marketplace 등록을 가로챈다. 원문: "so that an approval made the creator's page install the attacker's workflow."
- **high 중 에이전트 관련:** 공유 에이전트 작성자가 실행자의 MCP 자격 증명을 쓰는 건(8.7), MCPToolBlock의 중첩 JSON Schema로 DoS·SSRF가 되는 건(7.1), Slack 봇 토큰 유출(7.7, 0.11.3에서 수정)이 있다.
- **함정:** 권고의 버전은 Platform 릴리스 번호다. 원문: "Version numbers in this advisory identify Platform releases, whose Git tags are `autogpt-platform-beta-v<VERSION>`, rather than the backend Python package version." Docker 이미지 기준으로는 18건이 v0.11.1 이상을 요구한다(에이전트 집계, 직접 대조는 critical 2건만). 자체 호스팅이면 0.11.3으로 올린다.

[권고 목록](https://github.com/Significant-Gravitas/AutoGPT/security/advisories)

### 4. MCP 레퍼런스 서버 v2.5.0: `mcp-server-git`의 "저장소 설정만으로 명령 실행"을 세 번째로 막았다. 날짜 버전 사용자는 버전을 직접 지정해야 받는다 (공식, 릴리스 09:13 UTC, 권고 09:21~09:29 UTC)

- **무엇:** 원문: "v2.5.0 fixes eight vulnerabilities in three servers." 직접 세니 `mcp-server-git` high 5건·medium 1건, `server-filesystem`·`server-everything` low 각 1건이다. high 예시 원문: "The result is that a repository runs a command of its choosing, as the user running the server, from a read-only tool call." 경로 하나는 원문: "A successful `git_merge`, a fast-forward included, ended in `git maintenance run --auto`, whose garbage collection runs the commands a repository's own `gc.recentObjectsHook` names, with the server's privileges."이다. MCP Inspector도 medium 1건을 2.16.0과 v1용 1.0.6으로 고쳤다.
- **왜 중요한가:** 11-12 판에서 다룬 강화(2026.11.11)가 불완전했다. 두 건은 영향 범위가 `>= 2026.11.4`다. git 설정을 하나씩 막는 방식은 계속 뚫리고 있다. 에이전트에 신뢰하지 않는 저장소를 다루게 한다면 git 서버를 컨테이너 안에서 돌린다.
- **함정:**
  - 업그레이드 제안이 안 뜬다. 원문: "Package managers rank 2.4.0 below every 2025.x and 2026.x version, so they won't offer it as an upgrade." `@modelcontextprotocol/server-filesystem@2.4.0`처럼 직접 적는다. PyPI `mcp-server-git`은 2026.11.18이다.
  - 아직 남은 구멍이 있다. 원문: "The `--repository` confinement check is not yet repeated after such a write" (#5599). 후속 PR 2건이 22:14·22:58 UTC에 머지됐지만 조회 시점에 릴리스는 없다.
  - 전역 GHSA에 없어 알림이 오지 않는다.

[릴리스 v2.5.0](https://github.com/modelcontextprotocol/servers/releases/tag/v2.5.0) · [GHSA-pm7p-vphh-2cg7](https://github.com/modelcontextprotocol/servers/security/advisories/GHSA-pm7p-vphh-2cg7) · [Inspector 1.0.6](https://github.com/modelcontextprotocol/inspector/releases/tag/1.0.6)

### 5. CISA KEV에 LiteLLM 가드레일 샌드박스 탈출(CVE-2026-40217)이 들어갔다 (공식, 카탈로그 11-18 19:17 UTC)

- **무엇:** KEV 원문: "BerriAI LiteLLM contains an unprotected alternate channel vulnerability that could allow remote attackers to execute arbitrary code via bytecode rewriting at the /guardrails/test_custom_code URI." 조치 기한은 12-02다. 5월에 나온 권고다. 원문: "**Reaching the endpoint requires a proxy-admin credential** in default configurations." 원문: "which runs as root in the default Docker image."
- **왜 중요한가:** 11-04 판의 Google LLMjacking 보고서가 이 CVE 번호를 적었는데, 그때는 보고서 내용(`/key/regenerate` 권한 상승)과 권고가 맞지 않아 "번호가 잘못 붙었을 수 있다"고 썼다. KEV 등재로 가드레일 결함 자체가 실제로 악용됐다는 것이 확인됐다. 악용 주체와 경로는 KEV에 없다. 관리자 키를 얻은 공격자가 컨테이너 root 코드 실행까지 간다는 뜻이다. 기본 마스터 키를 그대로 둔 구버전이 가장 위험하다.
- **함정:** 전역 GHSA는 `1.83.10`에서 고쳤다고 적었고, 저장소 권고 본문은 "Fixed in **`1.83.11`**"이다. 1.83.11 이상으로 본다.
- 같은 날 KEV에는 Synology DSM, REDCap, Apache Axis도 들어갔다. 기한은 11-21로 3일이다.

[KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) · [LiteLLM 권고](https://github.com/BerriAI/litellm/security/advisories/GHSA-wxxx-gvqv-xp7p)

### 6. Java4Shell 후속: CISA가 즉시 패치를 권했지만 실제 악용은 아직 관측되지 않았다. Debian·Ubuntu 안정판 패키지는 아직이다 (공식, 직전 판 기술 2번 후속)

- CISA는 별도 경보로 즉시 패치를 권했다(페이지 날짜 11-17, THN 보도 11-18 08:24 UTC). KEV에는 없다. SANS ISC(19:54 UTC) 원문: "We are seeing many attacks, but none that exploit Java4Shell (yet)."
- 배포판(조회 시점): Debian은 sid만 고쳤고 bookworm·trixie는 미수정이다. Ubuntu는 openjdk-17·21·25가 모든 LTS에서 "needed"다. Oracle JDK·직접 받은 OpenJDK 빌드는 바로 올리고, 배포판 패키지는 공지를 지켜본다.
- GitHub에 11-17 이후 "java4shell" 이름의 저장소가 60개 넘게 생겼다. 대부분 점검 도구다. 검증되지 않은 PoC는 실행하지 않는다.

[SANS ISC](https://isc.sans.edu/diary/rss/33532) · [Debian](https://security-tracker.debian.org/tracker/CVE-2026-114938) · [CISA 경보](https://www.cisa.gov/news-events/alerts/2026/11/17/critical-java-vulnerability-requires-immediate-patching)

### 7. 에이전트 사고와 책임: Meta Muse가 남의 대화로 보이는 출력을 냈고 Signal 메시지를 읽었다. 미국 은행들은 AI 에이전트 사용 책임을 고객에게 넘겼다 (매체보도)

- **Meta Muse(Ars 19:19 UTC, Reuters 인용):** Meta 직원들이 사내 채팅에서 Muse가 다른 사람의 대화로 보이는 내용을 꺼냈다고 보고했다. 직원 원문: "this was ‘not my conversation’". Meta 내부 설명 원문: "the agent appeared to be hallucinating messages in response to built-in prompts pushing it to engage with users in between sessions." Reuters 시험에서는 원문: "On Android devices, they found that Muse could read Signal messages by bypassing Meta’s “Connector” tool and instead relying on permissions granted during initial setup." 원문: "indicated the issue described by the Meta employees was ongoing and could be affecting others,". 휴대폰 에이전트에 처음 준 권한이 커넥터 통제를 우회할 수 있다는 사례다.
- **Citi·PNC 약관(TechCrunch 17:32 UTC):** Citi 원문: "customers who choose to grant account access to third-party AI agents are responsible for activities conducted through those agents." AI 도구가 해킹돼도 고객 책임이다. 금융 계정에 에이전트를 붙이기 전에 약관부터 본다.
- **Vals 표적 지목 연구(직전 판 "짧게") 후속, 회사 응답(The Decoder 15:47 UTC):** 원문: "Anthropic said the behavior clearly violated its own rules, that it was already working on additional safeguards and that today's AI systems are not suited for fully autonomous weapons." OpenAI는 원문: "is actively updating its models to cover drone software and requests disguised as games or simulations." Vals 원문 블로그(11-17)에 따르면 원문: "On November 1st, we disclosed our findings to OpenAI, Anthropic, Google, Meta and SpaceXAI and held publication for two weeks"였다.

[Ars Technica](https://arstechnica.com/tech-policy/2026/11/muse-privacy-invasions-are-freaking-out-meta-staffers-internal-chats-show/) · [TechCrunch](https://techcrunch.com/2026/11/18/more-banks-says-customers-using-ai-agents-are-on-their-own-even-if-they-get-hacked/) · [Vals](https://www.vals.ai/blogs/autonomous-warfare) · [The Decoder](https://the-decoder.com/anthropic-openai-and-google-models-readily-marked-medics-journalists-and-surrendering-soldiers-as-kamikaze-drone-targets/)

### 8. "AI가 찾은 취약점을 한꺼번에 패치하다 장애가 난다"는 설명이 미국 의회에서 나왔다 (매체보도)

- 은행 CEO·NSA 비공개 브리핑을 전한 Washington Sun(00:45 UTC, 익명 관계자) 원문: "They’re trying to push through a lot of patches all at once — more than the system was designed for." 하원 국토안보위 청문회(The Record 20:18 UTC)에서도 원문: "We are in an environment where the rate of patching, the rate of upgrading of systems, is increasing," 발언이 나왔다. The Register(11-17 23:41 UTC) 원문: "AI-assisted bug hunting and exploit development has surfaced more than 100,000 CVEs so far this year".
- 최근 몇 주 이어진 대형 장애(직전 판 "짧게")의 원인을 공식 확인한 것은 아니다. 다만 1번 Nginx, 직전 판 Java4Shell·Chrome처럼 AI 발견 취약점이 쏟아지는 흐름과 맞물린다. SANS ISC 원문: "You must not treat these recent vulnerabilities as "emergencies". They are the "new normal"." 긴급 패치를 상시 절차로 만들고 롤백을 먼저 준비하라는 뜻이다.

[Washington Sun](https://www.washingtonsun.com/technology/bank-ceos-nsa-tell-congress-ai-driven-patches-behind-outages) · [The Record](https://therecord.media/tech-disruptions-house-homeland-committee-testimony) · [The Register](https://www.theregister.com/security/2026/11/17/daily-disruptive-outages-that-ground-planes-and-halt-work-welcome-to-the-new-normal/5310634) · [HN](https://news.ycombinator.com/item?id=50540836)

### 9. 논문 (arXiv 11-18 공지분, HF Daily Papers 11-18. 저자 주장이고 재현하지 않음)

- **TeleChat→Xing4(HF 1위):** 중국 Ascend 칩만으로 프런티어급 에이전트 모델을 학습했다는 보고다. 원문: "support the pretraining of a 285B-parameter model on 8,192 Ascend 910C cards". [arXiv](https://arxiv.org/abs/2611.22115)
- **SEAM:** 컴퓨터 사용 에이전트가 화면 너머로 이어지는 해로운 작업을 그대로 수행한다. 원문: "General-purpose open-weight and flagship agents carry out the harm in up to 92% of runs, yet decline when the user states it in one sentence." [arXiv](https://arxiv.org/abs/2611.22971)
- **Nevan:** 시스템 프롬프트 속 비밀값 유출 CTF다. 원문: "The flag was exfiltrated from 296 of the 363 models". 시스템 프롬프트에 키를 넣지 않는다. [arXiv](https://arxiv.org/abs/2611.21956)
- **GitHub Actions 보안 수정 백포트 누락:** 원문: "224,392 of 474,490 audited release branches remain vulnerable". 기본 브랜치만 고치면 릴리스 브랜치의 워크플로는 그대로 남는다. [arXiv](https://arxiv.org/abs/2611.22519)
- **Weight-Space Programming Doesn't Need Transformers:** 원문: "An LLM-designed model achieves higher test accuracy than the most accurate released Transformer that the same LLM wrote while using about 1,800x fewer parameters." 단서 원문: "Pretrained LLMs remain more accurate". [arXiv](https://arxiv.org/abs/2611.22055)
- **(주의) Energy-Based Reasoners(HN 290점대):** HN 제목은 "385x"인데 프로젝트 페이지는 "345×"다. 논문은 11-06 제출이라 새 글이 아니다. [페이지](https://energy-based-reasoners.github.io/) · [HN](https://news.ycombinator.com/item?id=50543989)

### 10. 짧게

- **Supergateway(MCP stdio를 SSE·WebSocket으로 바꾸는 게이트웨이) high 2건이 미수정이다(전역 unreviewed 00:33·15:34 UTC):** 4.2.0~4.2.2와 2.4.0~4.2.2가 영향받는다. npm 최신이 4.2.2다. 패키지 정보가 없어 알림이 오지 않는다. 외부에 노출했다면 내린다. [GHSA-vpvw-x889-f934](https://github.com/advisories/GHSA-vpvw-x889-f934)
- **Obot MCP 4건(21:31 UTC, 0.27.0에서 수정):** 원문: "allowing malicious MCP servers to steal access tokens." [GHSA-vrfj-v4c4-f8qr](https://github.com/advisories/GHSA-vrfj-v4c4-f8qr)
- **vLLM 권고 GHSA-xgh8(09:57 UTC, medium):** 본문 원문: "The issue affects vLLM 0.32.0rc0 through 0.33.0." 원문: "Version 0.34.0 applies the template trust check". 그런데 메타데이터 범위는 `<= 0.34.0`이라 스캐너가 0.34.0도 취약으로 표시한다. 오탐이다. [권고](https://github.com/vllm-project/vllm/security/advisories/GHSA-xgh8-qwjh-q853)
- **llama.cpp GGUF 파싱 RCE 3건(ZDI, 전역 unreviewed 00:33 UTC):** 수정은 이미 들어가 있다. 오래된 빌드를 쓰면 올린다. [GHSA-6q2w-jx57-7qrg](https://github.com/advisories/GHSA-6q2w-jx57-7qrg)
- **AI 이름 악성 패키지:** 창 안 malware 54건 가운데 `gguf-accelerator`(PyPI, 코인 채굴), `@tnhorg/agent-skills`(npm)가 있다. 직전 판 `mcp-aevo-server`는 OSV에서 악성 판이 6개로 늘었다(PyPI에서 삭제).
- **Optimizely 공급망 공격(직전 판 기술 8번) 후속:** Krebs(02:52 UTC)가 회사 입장을 실었다. 원문: "an unauthorized party exploited a vulnerability in the Optimizely Experimentation to add malicious JavaScript to some customer projects." Report URI 원문: "68 of the sites had no CSP at all". [Krebs](https://krebsonsecurity.com/2026/11/attackers-used-optimizely-to-a-b-test-malware-on-top-websites/)
- **(AI 무관) Apache Roller CVE-2026-129448 악용 중(BleepingComputer 10:35 UTC):** 원문: "This issue is being actively exploited." 6.1.6에서 고쳤다. KEV에는 아직 없다. [BleepingComputer](https://www.bleepingcomputer.com/news/security/apache-warns-of-roller-rce-vulnerability-exploited-in-attacks/)
- **(AI 무관, 예고) PHP critical 보안 릴리스:** 원문: "scheduled for Thursday, 19 November 2026, between 14:00 and 14:30 UTC." 원문: "The issues rated CRITICAL require either non-default configuration or uncommon setups." 한국 시간 11-19 23시다. [php.net](https://www.php.net/archive/2026.php)
- **(AI 무관) KVM 중첩 가상화 CVE-2026-99312(HN 160점대), F5 BIG-IP critical 6건, TPM 잠금 우회(Windows Hello·BitLocker PIN):** 제목과 RSS만 봤다. [HN](https://news.ycombinator.com/item?id=50548004)
- **(개발자 커뮤니티) Codeberg 도메인 정지(HN 290점대):** 도메인 등록 업체가 도메인을 정지해 몇 시간 접속이 안 됐다. 11:23 UTC부터 복구됐다. 코드 호스팅의 단일 실패 지점이 도메인일 수 있다는 사례다. [HN](https://news.ycombinator.com/item?id=50544261)
- **직전 판 후속:**
  - LibreChat 53건은 여전히 전역 DB에 없다. NoisyTCP는 Ubuntu LTS가 여전히 "needed"다.
  - 진전 없음: Chrome 158, NVIDIA 공지, Flowise(3.1.4 그대로), Fireworks(공지 11-17 그대로), HF Kokoro(응답 없음), FBI `ic3.gov` 새 경보, DeepAudit, Apify 사후 보고서, uBlock Origin(베타만), Windows 텔레메트리에 대한 Microsoft 입장, Gemini Argon·Grok 건, GTIG(재보도만), Cursor 최종 보고, Lovable RLS, Microsoft 365 최종 사후 보고서, AdminNeo, WPPConnect.
  - 런타임 보안 릴리스는 nginx뿐이다. Node.js·Go·Python·OpenSSL 정식 보안 릴리스는 창 안에 없다.

## 써볼 만한 도구

새로 써 볼 도구보다 업그레이드 때 조심할 것이 많은 창이었다. 시스템 프롬프트를 `instructions`에 넣던 앱과 하네스는 GPT-6.2에서 캐시를 못 받아 비용이 크게 늘었다. 도구들의 수정은 아직 다 나오지 않았다. Claude Code·Agent SDK·LiteLLM·goose·OpenCode·Mistral Vibe는 기본값이나 호환성이 바뀌었다. 도구는 하나도 설치하거나 실행하지 않았다. 함정과 인용은 릴리스 본문, PR·이슈 본문, 문서에서 직접 읽었다.

### 1. (비용 함정) GPT-6.2는 Responses API 최상위 `instructions`를 프롬프트 캐시에 넣지 않는다. 도구별 수정 상황이 다르다 (공식 문서 + 커뮤니티, 창 안 수정·이슈 11-18 01:30~22:01 UTC)

- **무엇:** OpenAI가 GPT-6.2를 내면서(11-17) 문서에 적은 동작 변경이다. 마이그레이션 가이드 원문: "Models starting with GPT-6.2 append top-level `instructions` after `input` in the Responses API." 원문: "To make reusable instructions eligible for prompt caching, put them in a `developer` message as the first item in `input`." 시스템 프롬프트를 `instructions`에 넣는 클라이언트는 매 단계 그 프롬프트를 캐시 없는 입력으로 다시 낸다.
- **얼마나 큰가(OpenCode 측정, 재현 안 함):** 원문: "GPT-6.2 models don't cache a system prompt sent in the Responses `instructions` field, so every step re-billed it as uncached input." 원문: "On GitHub Copilot `gpt-6.2-sol`, uncached input per step dropped from about 14,000 tokens to 3."
- **도구별 상황(조회 시점):**
  - 고친 판이 나왔다: OpenCode 2.3.2(npm `@opencode/cli` 01:30 UTC).
  - main에만 고쳤다: LiteLLM(PR #52599, 21:12 UTC 머지). 1.110 안정판 백포트 PR #52658은 아직 열려 있다.
  - 이슈만 있다: Codex(#62312, 16:30 UTC, 직원 답 없음), goose(#13653, 22:01 UTC), pydantic-ai(#13923, 20:51 UTC).
- **할 일:** 직접 만든 앱은 시스템 프롬프트를 `input` 첫 `developer` 메시지로 옮긴다. 써드파티 하네스로 GPT-6.2를 쓴다면 청구 화면의 캐시 입력 토큰을 먼저 확인한다.

[마이그레이션 가이드](https://developers.openai.com/api/docs/guides/latest-model) · [OpenCode 2.3.2](https://github.com/anomalyco/opencode/releases/tag/v2.3.2) · [LiteLLM PR #52599](https://github.com/BerriAI/litellm/pull/52599) · [LiteLLM PR #52658](https://github.com/BerriAI/litellm/pull/52658) · [Codex #62312](https://github.com/openai/codex/issues/62312) · [goose #13653](https://github.com/aaif-goose/goose/issues/13653) · [pydantic-ai #13923](https://github.com/pydantic/pydantic-ai/issues/13923)

### 2. (업그레이드 주의) Claude Code 2.1.327이 `latest`가 됐다. 셸 시작 파일이 승인 전에 돈다. Agent SDK TS 0.3.327은 호환성이 깨진다 (공식, `latest` 이동 11-17 23:16 UTC, 릴리스 23:17 UTC)

- **시각:** npm 게시는 직전 창(11-17 20:33 UTC)이었다. `latest` 이동과 GitHub 릴리스가 이번 창에 들어왔다. 조회 시점 dist-tags는 `latest`·`next` 2.1.327, `stable` 2.1.311이다. 창 안에 새 버전은 없다.
- **기본값·동작 변경(릴리스 본문 원문):**
  - 셸: "Improved the speed of a session's first Bash command: shell start-up files may now run before it's approved, even if it's refused; set `CLAUDE_CODE_DISABLE_SHELL_HEAD_START=1` to turn this off". 승인을 거부해도 `.bashrc` 같은 시작 파일은 이미 실행됐을 수 있다. 신뢰하지 않는 저장소에서 쓰거나 시작 파일에 부작용이 있다면 이 변수를 켠다.
  - auto mode: "Changed auto mode's notice about falling back to billed classifier requests to say that an upcoming release removes the fallback; each action it would have checked will be asked about or denied". 원문: "Changed auto mode to ignore more allow rules that run any package, like `Bash(pnpm dlx *)`".
  - 경로: "Changed what counts as inside a working directory, so some paths now ask for permission". 원문: "Changed path checks to refuse some symbolic links".
  - VS Code 연동: "[VSCode] Removed unused tools from the IDE connection (openFile, saveDocument, getOpenEditors and related)". 이 도구를 부르던 외부 통합은 깨질 수 있다.
- **데이터 손실 수정(원문):** "Remote Control: Fixed stopping the server in worktree mode deleting a running session's git-ignored files, such as `.env`". `.mcp.json`의 다른 키를 지우던 버그 수정은 직전 판에 적었다. 토큰을 명령줄에 남기지 않는 원문: "Added `claude mcp add-json <name> -` to read the server's JSON from stdin"도 들어왔다.
- **창 안 회귀 보고(커뮤니티, 직원 답 없음):** #110979(16:15 UTC) 제목 원문: "permissions.deny rules no longer stop a mod's own $.tool.call writes (regression after 2.1.322, still in 2.1.327)". mod를 쓰면서 `deny` 규칙에 기대는 구성은 확인한다. 직전 판의 `--add-dir` 신뢰 질문 회귀(#110642)는 여전히 열려 있고, 2.1.327 변경 로그에도 수정 항목이 없다.
- **Agent SDK TS 0.3.327(릴리스 23:17 UTC):** 원문: "Breaking: Changed hook options to require arrays: a hook event's value and a matcher's `hooks`. A Set, generator, plain object, `true` or number is now refused." 원문: "Changed the JSON-RPC `id` of requests to an SDK MCP server back from a string to a number". 직전 판에서 0.3.326에 맞춰 id를 문자열로 고쳤다면 다시 고친다. 원문: "Reverted 0.3.326's fix for a tool result from an SDK MCP server being lost, or delivered to another call, after a reconnect". Python SDK 0.2.184(PyPI 00:29 UTC)는 번들 CLI만 2.1.327로 올렸다.

[릴리스 2.1.327](https://github.com/anthropics/claude-code/releases/tag/v2.1.327) · [이슈 #110979](https://github.com/anthropics/claude-code/issues/110979) · [이슈 #110642](https://github.com/anthropics/claude-code/issues/110642) · [Agent SDK TS 0.3.327](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.327)

### 3. VS Code 1.147: 관리 설정 아래에서도 Claude·Codex 하네스를 허용할 수 있다. 다만 Copilot의 관리 통제는 그 세션에 걸리지 않는다 (공식, GitHub 11-18 09:45 UTC)

- **무엇(릴리스 노트 원문):** "Administrators can now permit one or both harnesses by listing them in vscode.allowedHarnesses". 실험 기능인 모델 라우터는 원문: "A decision model, gpt-6-luna-decisions , then matches your request to one of your routes"로 요청마다 모델을 고른다. 프리뷰인 Browser Use는 에이전트가 평소 쓰는 Chrome·Edge를 직접 다룬다.
- **함정:** 원문: "Note: Permitting a harness doesn't make it enforce your managed settings. Claude and Codex sessions run without Copilot's managed permission, sandbox, MCP, and plugin controls." 하네스를 허용하면 Copilot에 걸어 둔 권한·샌드박스·MCP 통제가 그 세션에는 적용되지 않는다. Browser Use는 원문: "allowing agents to reuse existing authenticated sessions"이다. 로그인된 브라우저를 에이전트에 넘기는 셈이다.

[릴리스 노트](https://code.visualstudio.com/updates/v1_147)

### 4. (호환성 깨짐) LiteLLM 1.110.0: 호환성 깨짐 표시(`!`)가 붙은 변경이 32건인데, 본문에는 Breaking 절이 없다 (공식, PyPI 11-18 01:35 UTC)

- **무엇:** 변경 411건 가운데 제목에 `!`가 붙은 항목을 세니 32건이다. "Breaking"이라는 제목은 없다. 1.111.0rc1이 02:20 UTC에 이어 나왔다.
- **주요 함정(PR 본문 원문):**
  - S3: "Breaking change: a client that sends `s3_bucket_name`, `s3_output_bucket_name` or `allow_legacy_cloud_file_ids` in a request body now gets a 401" (PR #47496). 올리기 전에 허용 버킷을 설정에 적는다.
  - 과금 경로: "`use_spend_logs_ledger` now defaults to true" (PR #49872).
  - Bedrock Claude 비용: "That is up from 4096 in the last release, so more output, spend and Bedrock quota per call" (PR #49692). `max_tokens`를 안 주던 호출의 출력 한도가 모델 최대치로 올라간다.
  - 1번의 GPT-6.2 캐시 수정은 1.110에 아직 없다.

[릴리스 v1.110.0](https://github.com/BerriAI/litellm/releases/tag/v1.110.0) · [PR #47496](https://github.com/BerriAI/litellm/pull/47496) · [PR #49872](https://github.com/BerriAI/litellm/pull/49872) · [PR #49692](https://github.com/BerriAI/litellm/pull/49692)

### 5. (업그레이드 주의) OpenAI Agents SDK Python 0.25.0: Runloop 샌드박스 제거, 압축 세션 기본 모델 변경 (공식, GitHub 11-18 00:22 UTC)

- **원문:** "Version 0.25.0 removes the beta Runloop sandbox provider and changes how Pydantic run contexts are checkpointed." 원문: "`OpenAIResponsesCompactionSession` now defaults to `gpt-6-luna`." 원문: "Native Chat Completions tool requests for `gpt-6-sol` and `gpt-6-luna` default to non-reasoning mode when no effort is supplied."
- **함정:** 여러 워커가 체크포인트를 나눠 쓰면 원문: "upgrade readers first"를 따른다. effort를 주지 않던 GPT-6 Chat Completions 도구 호출은 추론 없이 돈다. 품질이 떨어졌다면 effort를 명시한다.

[릴리스 v0.25.0](https://github.com/openai/openai-agents-python/releases/tag/v0.25.0)

### 6. (호환성 깨짐) goose 1.60.0·1.60.1: chat 모드와 `goose review` 삭제. Claude가 거부하던 압축 프롬프트를 고쳤다 (공식, 11-17 23:08 · 11-18 01:21 UTC)

- **1.60.0(직전 판에서 본문이 비어 있던 판):** 릴리스 본문 원문: "Remove chat mode", "Remove the goose review subcommand". PR 원문: "Running `goose review` exits with status 2 and reports `unrecognized subcommand 'review'`." 저장된 chat 모드 설정은 자동 승인으로 바뀌지 않는다. PR 원문: "`"chat"` therefore still parses, via serde alias and strum, as `Approve` (ask before every tool call)". Desktop은 macOS 12 지원을 뺐다. 원문: "Keep request logs private on Unix" 같은 권한 수정도 들어왔다.
- **1.60.1:** 원문: "Compaction: drop the `<analysis>` scratchpad that Claude models refuse". PR은 거부 메시지를 원문: "This request was blocked as it seems to violate Anthropic's Terms of Service restrictions on reverse engineering or duplicating model outputs."로 옮겼다. 압축 프롬프트에 생각 영역 출력을 요구하면 같은 거부가 날 수 있다는 사례다. 다만 PR 작성자도 원문: "I could not reproduce the refusal itself on this short sample"라고 적었다.

[릴리스 1.60.0](https://github.com/aaif-goose/goose/releases/tag/v1.60.0) · [릴리스 1.60.1](https://github.com/aaif-goose/goose/releases/tag/v1.60.1) · [PR #13469](https://github.com/aaif-goose/goose/pull/13469) · [PR #13571](https://github.com/aaif-goose/goose/pull/13571) · [PR #13572](https://github.com/aaif-goose/goose/pull/13572)

### 7. OpenCode 2.3.1: 백그라운드 압축이 기본으로 켜졌다. 조건부 일정은 업그레이드 때 사라진다 (공식, npm 11-18 00:08 UTC)

- **원문:** "Background compaction is now on by default". 원문: "Turn it off with `compaction.background: false`; a background summary that is never used still costs tokens." 원문: "OpenCode again reads `CLAUDE.md` in any directory that has no `AGENTS.md`".
- **함정:** 실험 기능이던 schedules가 automations로 바뀌면서 조건이나 실행 횟수 제한이 붙은 일정, 끝난 일정은 지워진다(원문: "are dropped"). 원문: "A foreground `timeout: 0` now uses the default 120 second timeout instead of waiting until the command exits." 2.x는 `@opencode/cli`로 배포된다. `opencode-ai`는 1.x다. GPT-6.2를 쓴다면 바로 2.3.2로 간다(1번).

[릴리스 2.3.1](https://github.com/anomalyco/opencode/releases/tag/v2.3.1)

### 8. Mistral Vibe 3.1.3: 기본 모델이 Large 4로 바뀌었다. 데스크톱 앱도 나왔다 (공식, GitHub 11-18 08:02 UTC)

- **CLI(릴리스 본문 원문):** "Mistral Large 4 is now the default model. To stay on Mistral Medium 3.5, or if your own Mistral server does not offer Large 4, set `active_model = "mistral-medium-3.5"`". 자체 Mistral 서버에 Large 4가 없으면 업그레이드 뒤 실패한다. 좋은 변경도 있다. 원문: "A project's `.vibe/config.toml` or an agent profile can turn usage telemetry off but no longer turn it back on". 저장소 설정이 사용자의 텔레메트리 끄기를 되돌릴 수 없다.
- **Vibe Desktop(공지 이슈 11-17 15:10 UTC, The Decoder 11-18 14:40 UTC):** macOS·Windows용이다. 공지 원문: "Hand Vibe several tasks at once, watch the agents work side by side, then review their diffs and track pull requests in the same window." 유료 사용자는 Large 4가 기본이다.

[릴리스 v3.1.3](https://github.com/mistralai/mistral-vibe/releases/tag/v3.1.3) · [Desktop 문서](https://docs.mistral.ai/vibe/code/desktop/overview) · [공지 이슈 #1548](https://github.com/mistralai/mistral-vibe/issues/1548)

### 짧게

- **NVIDIA SkillEvaluator 1.0.1(PyPI 11-18 00:20 UTC):** 직전 판에서 "패키지 미게시"였던 스킬 검사 도구가 PyPI에 올라왔다. 원문: "find compatibility issues that can cause your skill to be missed, rejected, or truncated across **Claude Code, Codex, Copilot CLI, Hermes, OpenClaw, and OpenCode**". 정적 검사는 원문: "These static checks need no model key". `pip install skillevaluator`. [릴리스](https://github.com/NVIDIA/SkillEvaluator/releases/tag/v1.0.1)
- **Dify 1.20.0(09:55 UTC):** 기본값 원문: "`OPENAPI_ENABLED` | `true` (was `false`)". API 엔드포인트가 기본으로 열린다. 외부 노출된 자체 호스팅은 올리기 전에 이 값을 정한다. Langfuse 연동은 원문: "Langfuse Cloud retired its legacy ingestion endpoint on **November 16, 2026**"라서 1.20.0으로 올려야 트레이스가 계속 간다. [릴리스](https://github.com/langgenius/dify/releases/tag/1.20.0)
- **Codex CLI 0.170.0(03:40 UTC):** 큰 함정은 없다. 원문: "Added experimental support for using identically named MCP servers across environments." 직전 판의 0.169.1 Windows x64 바이너리는 11-17 23:20 UTC에 올라와 해소됐다. [릴리스](https://github.com/openai/codex/releases/tag/rust-v0.170.0)
- **OpenAI "Inside the Codex sandbox"(RSS 11-18, 날짜만):** Codex 샌드박스 구조를 설명한 글이다. 원문: "Allows writes within the workspace, while keeping directories such as `.git` and `.codex` read-only." 함정 원문: "For a stdio MCP server configured in `config.toml`, the Codex harness does not attempt to sandbox the command used to start the server." 로컬 MCP 서버는 샌드박스 밖에서 돈다고 보고 고른다. 네트워크 허용 규칙도 원문: "Note that broad wildcard rules or access to DNS services can still permit DNS-based exfiltration." [OpenAI](https://openai.com/index/inside-the-codex-sandbox/)
- **GitHub Copilot(Ignite 2일차):** 커넥터 8종(Supabase·Box·Calendly 등)이 추가됐다(22:04 UTC). 원문: "If your enterprise’s MCP allowlist lists connectors individually rather than with a wildcard, an admin also needs to add the new connectors to it." 팀 단위 플러그인 기본값도 GA다(16:00 UTC). [커넥터](https://github.blog/changelog/2026-11-18-connect-supabase-box-calendly-and-more-to-copilot) · [팀 설정](https://github.blog/changelog/2026-11-18-customize-github-copilot-for-your-team)
- **Playwright MCP v0.0.86(18:50 UTC):** 확장 모드에서 쿠키 설정·권한 부여 같은 브라우저 전체 변경을 거부한다. 원문: "Browser-wide changes such as setting or clearing cookies and granting permissions are now refused, although clients can still read browser-wide data such as cookies." 읽기는 여전히 된다. [릴리스](https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.86)
- **Mastra Factory(Show HN 16:53 UTC, 70점대):** Slack에서 부르는 에이전트 작업 보드다. Mastra 팀이 자기 저장소 관리에 쓴다. 기본 설정은 Mastra 플랫폼 인증과 샌드박스를 쓴다. 템플릿 저장소에 LICENSE 파일이 없다(조회 시점). [GitHub](https://github.com/mastra-ai/template-factory) · [HN](https://news.ycombinator.com/item?id=50548660)
- **claude-code-action v1.0.281·282(05:13·16:50 UTC):** 포크 PR을 건너뛰는 `allow_fork_prs` 입력이 생겼다. PR 원문: "It is `"true"` by default, so nothing changes unless a workflow sets it." 공개 저장소라면 `false`로 둘지 검토한다. [PR #2094](https://github.com/anthropics/claude-code-action/pull/2094)
- **그 밖의 릴리스:** FastMCP 4.2.2(`ResourcesAsTools`를 쓰면 `skills/list`가 비던 문제), Zed 1.29.1(Claude Fable 5.2 BYOK), Vercel AI SDK `ai@7.0.177`(패키지에 `skills/ai-sdk` 스킬 동봉), Snap Easy Lens CLI(Claude Code·Codex로 AR 렌즈 제작). Claude 공식 플러그인 security-guidance에는 수정 PR 5건이 더 머지됐지만 `plugin.json` 버전이 그대로라 기존 설치에 언제 전달되는지는 아직 모른다. [FastMCP](https://github.com/PrefectHQ/fastmcp/releases/tag/v4.2.2) · [Zed](https://github.com/zed-industries/zed/releases/tag/v1.29.1) · [AI SDK](https://github.com/vercel/ai/releases/tag/ai%407.0.177) · [Snap](https://newsroom.snap.com/introducing-easy-lens-cli)
- **MCP 레퍼런스 서버 v2.5.0과 Inspector 1.0.6·2.16.0:** 보안 수정이다. 날짜 버전을 쓰면 업그레이드 제안이 뜨지 않으니 기술 4번을 보고 버전을 직접 지정한다.
- **직전 판 후속 중 새 진전 없음:** vLLM 0.34.1·Open WebUI 0.13.5는 안 나왔다. browser-use 1.0.6, GitHub MCP Server 2.2.0, Context7 4.7.3, rmcp·MCP Go SDK, Anthropic SDK, lend, LM Studio, LoopForge, Gemini CLI, Crush, Qwen Code, Pi, Amp, Cursor, Ollama, transformers, MCP 명세도 그대로다. `anthropics/skills`는 창 안 커밋이 없다.

### 참고: 창 경계 항목

- **(직전 창, 놓친 항목) Hugging Face Inference Providers가 LLM 요청을 OpenRouter로 보낸다. 제공자 키를 직접 쓸 수 없게 됐다(블로그 11-16, The Decoder 11-18 14:33 UTC):** 원문: "Today, we are switching Inference Providers from our own custom router to OpenRouter's." 원문: "Requests to language models now go through OpenRouter and are billed to your Hugging Face account or organization, so you can no longer use your own API key for a provider with these models." 이미지·영상은 원문: "Custom keys for image and video providers keep working." 제공자와 따로 계약해 키로 쓰던 팀은 과금 주체가 바뀐다. [HF 블로그](https://huggingface.co/blog/openrouter)

## 주목할 점

- **취약점을 찾는 속도가 패치하는 속도를 앞질렀다.** Java4Shell(직전 판)에 이어 Nginx도 AI 스캐너가 찾았다. 여러 팀이 같은 버그를 따로 신고했고, 공개 당일 PoC가 나왔다. 미국 의회에서는 몰아치는 패치가 장애 원인이라는 설명까지 나왔다. 배포판 패키지는 늘 하루 이틀 늦는다. 기능을 끄는 완화책(HTTP/2 끄기)과 롤백 절차를 미리 준비해 두는 편이 낫다.
- **모델을 바꾸는 비용은 단가표에 다 나오지 않는다.** Neon은 정가가 Carbon보다 싸지만 도입가 기간에는 더 비싸다. 또 느리고 장황해 과제당 비용이 높다. GPT-6.2는 캐시 동작이 바뀌어 같은 하네스에서 입력 비용이 몇 배로 늘 수 있다. 모델을 바꿀 때는 자기 작업으로 과제당 비용과 캐시 적중률을 잰다.
- **다가오는 일정:**
  - 오늘(11-19): Anthropic Nasdaq 거래 시작과 424B4 접수(최종 주식 수·조달액 확정), BIS 원격 접근 규칙 발효, PHP critical 보안 릴리스(14:00~14:30 UTC = 한국 23시).
  - 이번 주: Enterprise Claude Data 기본 켜짐, Microsoft 365·Apify 사후 보고서(11-20 무렵), Synology·REDCap·Apache Axis KEV 조치 기한(11-21).
  - 그다음 주: ERNIE 5.5 Flash Preview 가중치 공개 약속(11-25 전), Qwen4-Max-Preview 종료(11-25), Gemini 4 Carbon·Argon 도입가 종료(11-30), Huawei openPangu-2.0 종료(11-30). Puppet Enterprise KEV 조치 기한(12-01), LiteLLM KEV 조치 기한(12-02).
  - 12월 이후: 직전 판 일정 그대로다. 영국 Copilot 테넌트 Claude 기본 켜짐(12-09), Val Town 읽기 전용(12-10), GitHub Copilot 모델 6종 은퇴(12-14), Inception Mercury API 종료(12-28), Windows 기본 소형 모델 Aion 탑재(2027년 1월 말), 다음 MCP 명세(03-30).

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - openai.com 글(칩 설계, 위험 관리, Codex 샌드박스)은 403이라 r.jina.ai로 읽었다. ChatGPT 릴리스 노트도 같다.
  - Bloomberg·Reuters·FT·WSJ의 공모가 원 보도는 열지 못했다. CNBC·The Register·TNW·Motley Fool 인용으로 봤다. Meta Muse 건의 Reuters 원문도 Ars 인용으로 봤다.
  - CISA 경보 페이지는 403이라 KEV JSON과 THN 보도로 봤다. F5 권고, OX Security 원문, Oracle 패치 원문은 읽지 않았다. "Anthropic Is Not a Safe AI Company" 연합 보도자료(BusinessWire, HN 120점대)는 403이라 내용을 확인하지 못해 싣지 않았다.
  - Gemini 4 Neon 모델 카드와 BIS 규칙 PDF는 r.jina.ai 텍스트 변환본으로 읽었다. LMArena는 Cloudflare에 막혔다. x.com 타임라인과 reddit은 막혀 있다.
- **시각이 날짜만 있거나 불확실한 원문:**
  - OpenAI RSS 글은 날짜만 믿을 수 있다(칩 설계는 HN 첫 게시 시각으로 보완). 날짜만 있는 곳: MathArena 갱신, Antigravity 변경 로그, Vals 블로그, Anthropic 블랙홀 연구(Wayback 첫 캡처로 보완), CISA Java 경보, VS Code 릴리스 노트(GitHub `published_at`으로 보완).
  - Bloomberg의 $83 첫 보도 시각은 21:40 UTC 이전이라는 것만 안다. CNBC 기사는 같은 URL을 15:15 UTC에 처음 올리고 여러 번 고쳤다. 최종 주식 수·조달액은 창 직후(23:23 UTC) 갱신분에만 있다.
  - Microsoft 저장소가 비공개로 바뀐 시각은 HN 첫 보고(11-17 23:18 UTC) 전이라는 것만 안다. Rapid7 PoC 저장소가 공개로 바뀐 시각도 모른다.
- **확인하지 못한 것:**
  - Anthropic의 최종 공모 조건(424B4), Microsoft의 agent-framework 업로드 자격 증명 폐기 여부와 저장소 복구 시점, Codex CLI 자체가 GPT-6.2 캐시 문제에 걸리는지, security-guidance 수정이 기존 설치에 전달되는 시점.
  - 재현하지 않은 것: 모든 벤치마크·비용 주장(Neon, MathArena, DaisyRose, 단백질 서열 측정, OpenAI 칩 설계, OpenCode 캐시 측정, 논문 수치), Wiz·Xint의 AI 사용 주장, 모든 PoC. 도구와 패키지는 하나도 설치하거나 실행하지 않았다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 80점 이상 스토리와 40~79점 스토리는 제목을 전부 봤고, Show HN은 5점 이상을 봤다. 그 아래는 키워드로만 걸렀다.
  - 전역 GHSA: reviewed 15건, unreviewed 1,241건, malware 54건을 창 범위로 받아 셌다. unreviewed는 AI 키워드로 걸렀다. 저장소 권고는 AI 저장소 약 120곳과 도구 저장소 약 30곳을 조회했다.
  - HF Daily Papers 11-18(37편)은 전부, alphaXiv는 1면만, arXiv cs.CR은 키워드로만 봤다. GitHub 트렌딩은 일간 1면만 봤다.
  - SecurityWeek·Help Net 피드는 창 앞부분이 빠져 있었다. Microsoft Ignite 2일차는 GitHub 변경 로그와 Foundry 블로그 위주로 봤다. 한국 랩은 HF 조직 스캔에서 창 안 갱신이 없었고, 뉴스룸은 따로 보지 않았다.
  - Vertex AI 릴리스 노트(캐시 동결)와 Moonshot·Volcengine 변경 로그(월 단위)는 일 단위로 판정하지 못했다.
  - 창이 끝날 때 열려 있던 것: Anthropic 424B4, Claude Meetings Team 자동 참가 복구, MCP git 서버 후속 수정의 릴리스, LiteLLM 1.110 캐시 백포트, Claude Code #110979·#110642. 다음 판에서 다룬다.
