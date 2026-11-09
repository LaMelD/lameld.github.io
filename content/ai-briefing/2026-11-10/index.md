---
title: "2026-11-10 AI 브리핑"
date: 2026-11-10T01:00:00+09:00
tags: [ai-briefing, claude-code, security, open-weights, mcp]
description: "GitGuardian이 Claude Code 승인 파일에서 유효한 비밀값 5,343개와 Docker Hub 유출을 확인했고 SANS 허니팟에 .claude 디렉터리 탐색이 들어왔으며, 미국 3사는 조용한 가운데 Meituan LongCat-2.5 가중치와 LG K-EXAONE 2.5 무료 API가 나왔고, MongoDB MCP는 읽기 전용 설정이 빠지던 문제를 고쳤다."
---

> 조사 범위: 2026-11-09 01:00 ~ 2026-11-10 01:00 KST(2026-11-08 16:00 ~ 11-09 16:00 UTC, 미국 일요일 08:00 ~ 월요일 08:00 PST). 직전 브리핑(11-09 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`·`merged_at`, npm·PyPI 게시 시각, 전역·저장소 GHSA `published_at`, X 게시 시각, Hugging Face 커밋·토론 시각, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-09 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **Claude Code 승인 파일 비밀값 건이 커졌다.** GitGuardian이 공개 GitHub에서 비밀값 31,088개를 찾았고, 그중 5,343개는 탐지 당시 유효했다. 같은 파일이 Docker Hub 이미지에도 실려 나갔다. SANS 허니팟에는 주말에 `.claude` 디렉터리 요청이 새로 들어왔다. Anthropic의 수정 릴리스는 아직 없다.
- **미국 3사는 신모델·API 변경이 없었고, 아시아의 월요일 발표가 나왔다.** Meituan LongCat-2.5(총 1.6T) 가중치가 Hugging Face에 MIT로 올라온 것이 확인됐다. 다만 공개 시각과 공식 발표는 확인하지 못했다. LG는 K-EXAONE 2.5 API를 무료로 열었다. 인프라 쪽에서는 Google이 TPU 두 종을 새로 냈다.
- **도구는 설정이 실제로 적용됐는지부터 본다.** MongoDB MCP 설정 마법사는 읽기 전용을 골라도 대부분의 클라이언트 설정에 그 플래그를 넣지 않았다. 이 수정이 담긴 첫 npm 빌드가 3.3.0이다. CrewAI는 기본 모델을 `gpt-6-luna`로 바꿨다.

## 모델 소식

OpenAI·Google·Anthropic의 신모델은 없고, API·가격 변경도 없다. 공식 변경 로그(OpenAI API 11-05, Gemini API 11-06, Claude Platform 11-05, xAI 11-02, DeepSeek 10-17, Mistral 11-05, Z.ai 08-26)와 ChatGPT 릴리스 노트(11-04), Gemini 앱 릴리스 노트(10-13)가 모두 직전 판과 같다. OpenRouter에는 창 안 신규 등록이 없다. 주요 랩의 Hugging Face 조직에도 `createdAt`이 창 안인 모델은 없다. 아래 1번은 그 전에 만들어 두었다가 공개한 저장소다.

### 1. Meituan LongCat-2.5(1.6T) 가중치가 Hugging Face에 MIT로 공개됐다. 공개 시각은 창 안으로 보이지만 확인하지 못했다 (공식 저장소, 마지막 커밋 11-09 11:38 UTC)

11-04 판은 TechNode 보도를 옮기며 "가중치 공개는 확인하지 못했다"고 썼다. 지금 `meituan-longcat/LongCat-2.5`는 공개 상태이고(`private: false`, `gated: false`) 가중치 샤드가 올라와 있다. W4A4 양자화판(`LongCat-2.5-W4A4`)도 함께 있다. 모델 카드 원문: "LongCat-2.5 is a native multimodal MoE model for long-horizon agentic tasks, with **1.6 trillion total parameters** and approximately **48 billion activated per token**." 라이선스는 원문: "The **model weights** are released under the **MIT License**."다.

- **창 안 공개로 보는 근거(정황):** 저장소는 11-04에 만들었고 창 안 커밋은 11-09 11:38 UTC 모델 카드 수정 한 건이다. 조회 시점 다운로드는 0, 토론은 0건이다. Wayback의 조직 페이지 캡처는 10-30에는 이 모델이 없고, 11-09 13:25 UTC에는 있다. ModelScope 저장소도 11-09 14:15 UTC에 만들어졌다. 그러나 Meituan의 공식 발표 글은 찾지 못했다.
- **벤치마크 함정:** 비교 대상이 한 세대 전 모델(GPT-5.5, Claude Opus 4.8/5, GLM-5.3 등)이다. 원문: "Scores marked with `*` are cited from public technical reports, blog posts, or official leaderboards; unmarked scores are measured in-house and are indicative only." 자체 측정한 Terminal Bench v2.1 88.4는 DeepSeek-V4.1-Flash(90.6\*)보다 낮고, DeepSWE v1.1 64.6도 DeepSeek-V4.1-Flash(74.2\*)보다 낮다.
- **배포 함정:** 원문: "LongCat-2.5 can be deployed on NPU platforms." 모델 카드의 배포 안내는 NPU용뿐이고 GPU용 vLLM·SGLang 안내는 없다.

중요한 이유: 1.6T급 가중치가 상업 사용 제한 없이 풀렸다. 다만 공식 발표가 없고 자체 측정 점수도 최신 오픈 웨이트보다 낮아, 지금은 "가중치가 실재한다" 정도로만 본다.

[Hugging Face](https://huggingface.co/meituan-longcat/LongCat-2.5) · [ModelScope](https://modelscope.cn/models/meituan-longcat/LongCat-2.5) · [기술 블로그](https://longcat.ai/blog/longcat-2.5/)

### 2. LG AI연구원이 K-EXAONE 2.5(750B) API를 무료로 열었다. 가중치는 공개하지 않았다 (공식, 보도자료 11-09 KST, AI타임스 07:29 UTC)

'NVIDIA AI Day Seoul 2026'에서 발표했다. EXAONE API 변경 로그 원문: "본 서비스는 현재 무료로 제공됩니다." 모델 페이지 사양은 750B(활성 31B), 컨텍스트 512K, 한국어·영어 텍스트 전용이다. 무료 기간은 AI타임스가 인용한 LG 관계자 말로 "(현재로서는) 명시적으로 정하지 않았다"이다.

- **벤치마크(모델 페이지, 자체 측정):** SWE-Bench Pro 59.21(2.0은 47.1), BrowseComp 78(50.87), AutomationBench 74.6(16.9)이다. 비교표의 DeepSeek-V4.1-Flash·GLM-5.3과 비슷한 수준이다. 보도자료의 "70% 이상 향상"은 8월 공개 모델과 견준 내부 종합 수치다.
- **다음 일정:** 보도자료 원문: "내년 1월까지 프런티어급 성능의 엑사원 모델을 완성하는 데 집중하겠다"

중요한 이유: 한국 독자 모델이 자사 API를 무료로 연 첫 사례다. 한국어 업무에 비용 없이 시험해 볼 수 있다. 다만 무료 기간이 정해지지 않았으니 운영 서비스에 바로 붙이기는 이르다.

[보도자료](https://www.lg.co.kr/media/release/30760) · [API 변경 로그](https://docs.exaone.ai/ko/docs/changelog) · [모델 페이지](https://docs.exaone.ai/ko/docs/models) · [AI타임스](https://www.aitimes.com/news/articleView.html?idxno=217175)

### 3. Google이 TPU 8tx(기밀 연산)와 TPU 8i Max(긴 컨텍스트)를 냈다. 한 해에 TPU 시스템 네 종을 낸 것은 처음이다 (공식, 11-09 13:00 UTC)

8tx는 8t와 같은 칩에 AMD EPYC 호스트를 붙여 하드웨어 수준의 기밀 연산을 제공한다. 원문: "delivering up to 2x lower latency compared to TPU 8t with a near-zero performance penalty". 8i Max는 원문: "TPU 8i Max scales compute and memory in lockstep, delivering double the compute, double the SRAM and double the HBM bandwidth of TPU 8i"다. 사업 규모도 처음 밝혔다. 원문: "TPUs are now a rapidly growing business with nearly 100 customers". 출시 시점은 본문에서 찾지 못했다.

중요한 이유: Gemini를 돌리는 인프라이면서, 외부 랩에 파는 TPU 사업이 빠르게 커지고 있다는 공식 수치다. 기밀 연산 TPU는 규제 산업의 추론 수요를 겨냥한다.

[Google 블로그](https://blog.google/innovation-and-ai/infrastructure-and-cloud/global-network/tpu-8i-max-and-8tx/) · [Google Cloud](https://cloud.google.com/blog/topics/ai-infrastructure/tpu-8tx-and-tpu-8i-max/) · [The Decoder](https://the-decoder.com/google-ships-four-tpu-variants-in-one-year-as-it-ramps-up-competition-with-nvidia/)

### 4. ("Opus 5.5가 달라졌다" 후속) HN 첫 화면에 올랐지만 Anthropic 반응은 여전히 없다 (커뮤니티, HN 11-09 07:04 UTC, 370점대)

직전 판이 미확인으로 실은 X 글이 HN에 "Is Opus 5.5 nerfed? Thinking patterns changed on November 3"로 올라왔다. 지난주 reddit 글 14개를 모은 "Tell HN: A flood of reports of Opus 5.5 being nerfed"(11-08 23:49 UTC, 20점)도 있다. 조회 시점에 댓글 260여 개 중 Anthropic 직원 댓글은 찾지 못했다. 상태 페이지에도 창 안 인시던트가 없고(최신은 11-08 Fable 5.1 오류), Claude Platform 릴리스 노트도 11-05 그대로다.

- **새 루머(미확인, X 11-09 13:26 UTC):** 익명 계정이 원문: "Anthropic has already finished what appear to be Sonnet 5.6 and Opus 5.6, and these new checkpoints are already undergoing A/B testing directly in Claude Code."라고 썼다. 같은 글에서 원문: "OpenAI finally seems to have fixed all the issues with GPT-6.2 Astra, and testing is now complete."라고도 했다. 근거는 제시하지 않았다.

중요한 이유: 공모를 앞두고 품질 저하 의혹이 커지고 있다. 다만 지금까지 근거는 개인 세션 분석과 체감 보고뿐이다. 측정 자료나 회사 설명이 나오면 다시 다룬다.

[HN](https://news.ycombinator.com/item?id=50422069) · [Tell HN](https://news.ycombinator.com/item?id=50419019) · [루머 X 글](https://x.com/tokengremlin/status/2119783808773841101)

### 5. 짧게

- **NVIDIA, 한국 오픈 모델용 Dynamo 레시피와 투자 계획(공식, 11-09 05:00·08:00 UTC):** 원문: "NVIDIA is releasing Dynamo-optimized deployment recipes for Korea's leading open models: A.X K2 from SK Telecom, K-EXAONE 2.0 from LG AI Research, Solar Open 2 from Upstage and Motif-3 from Motif Technologies." 투자는 원문: "NVIDIA also announced an investment in Korean startup Point2, with additional investments planned in Motif Technologies, RLWRLD and Upstage." 금액은 밝히지 않았다. Rebellions는 차세대 R200 NPU에 NVLink Fusion을 쓰기로 했다. [Dynamo 레시피](https://blogs.nvidia.com/blog/nvidia-dynamo-korea-open-models/) · [투자](https://blogs.nvidia.com/blog/korean-startup-investments-msit/) · [NVLink Fusion](https://nvidianews.nvidia.com/news/nvidia-rebellions-and-sk-telecom-announce-collaboration-to-advance-sovereign-ai-infrastructure-with-nvidia-nvlink-fusion)
- **NVIDIA 이사회가 Huang CEO 관련 보도를 독립 조사한다(공식, 11-08 20시대 UTC, HN 480점대·870점대):** 원문: "The NVIDIA Board is initiating an independent investigation in light of recent reporting that founder and CEO Jensen Huang has been the target of a foreign intelligence effort." 이어서 원문: "While the Board has no reason to believe that either national security or NVIDIA secrets were compromised, the Board's inquiry will examine all available information." CNBC 원문: "Nvidia stock fell 5% on Monday". AI 칩 수출 통제 논의에서 Huang의 입지가 약해졌다는 분석도 나왔다. [이사회 성명](https://nvidianews.nvidia.com/news/statement-from-nvidia-board-of-directors) · [Huang 메시지](https://nvidianews.nvidia.com/news/a-message-to-our-company) · [CNBC](https://www.cnbc.com/2026/11/09/nvidia-jensen-huang-chinese-spy-investigation.html) · [The Decoder](https://the-decoder.com/nvidia-espionage-scandal-weakens-huangs-lobby-against-tighter-ai-chip-export-controls/) · [HN](https://news.ycombinator.com/item?id=50418016)
- **장애(공식):** 직전 판의 ChatGPT 이미지 생성 오류는 11-08 17:00 UTC에 끝났다. 원문: "All affected services are operating normally again." 11-06에 시작된 Mastercard 결제 장애는 여전히 monitoring이다. [상태 페이지](https://status.openai.com/incidents/01M6XVDCKRT3PSB0YP8J30GTD5)
- **(Nous Arete-27B 후속) 모르는 것을 지어내는 비율이 늘었다는 두 번째 점검(커뮤니티, HF 토론 11-08 21:43 UTC):** 원문: "Invented: the base answered UNKNOWN on 60/100, Arete on 48/100. All 12 changes go the same way: the base said UNKNOWN and Arete gave a confident made-up answer". 작성자 해석은 원문: "So it looks like a general SFT side effect rather than anything specific to Arete." 다른 사용자가 4비트 MLX에서 같은 방향을 봤다. Nous 답글은 없다. [토론 #23](https://huggingface.co/NousResearch/Arete-27B/discussions/23)
- **Trump, AI 기업 대표에게 "감옥" 경고(매체보도, The Verge 15:50 UTC):** 60 Minutes 인터뷰에서 원문: "Get ready to go to jail. Please give your wife a kiss good night."라고 했다고 말했다. 회사 이름은 대지 않고 원문: "One of the big ones."라고만 했다. [The Verge](https://www.theverge.com/ai-artificial-intelligence/1029802/trump-60-minutes-ai)
- **Kimi K4 5.4T 루머(미확인, Pandaily 11-09):** 중국 매체 보도를 옮긴 것으로, 2027년 2분기 출시 목표라고 한다. Moonshot 발표가 아니다. [Pandaily](https://pandaily.com/kimi-k4-unconfirmed-roadmap-report-parameters-release-target)
- **직전 판 후속 중 새 진전 없음(조회 11-09 16시대 UTC):**
  - Anthropic 공모: EDGAR 최신은 11-02 S-1/A 그대로이고 공모가 범위는 공시되지 않았다. 가스 발전기 34억 달러 건에 대한 회사 논평도 찾지 못했다.
  - Inkling 2 Small 가중치: "next week"라던 주가 시작됐지만 `thinkingmachines/Inkling-2-Small`은 여전히 401이다.
  - Tencent Hy4 가중치(추정 이름 4개 모두 401), OpenAI `gpt-6.2-astra`·`gpt-6.2-sol` 문서(404), ERNIE 5.5 Flash 사후 학습판, Claude Mythos 5.5, Anthropic 텍스트 워터마크(도움말 `dateModified` 11-03), Anthropic METR 자체 보고서, DeepSeek V5, OpenAI Pierre 인수 조건, Kimi K3.1 Pro 인증 파트너, Apple Siri 에이전트(Gurman 보도의 재인용만).
  - 공식 채널: Alizila(10-26), Microsoft AI(10-27), OpenAI 뉴스 RSS(11-06).

### 참고: 창 경계 항목

**PSI가 "비공개 Anthropic 모델"로 Aaronson–Ambainis 추측을 증명했다고 발표했고, 당사자 Scott Aaronson이 창 안에 반응했다** (PSI 날짜만 11-06, Aaronson 블로그 11-08 21:44 UTC)

케임브리지 스타트업 PSI(Physical Superintelligence)가 양자 계산 이론의 오랜 추측을 증명했다고 발표했다. 직전 판들은 다루지 않았다. PSI 원문: "a research system developed internally at Physical Superintelligence (PSI), running a non-public Anthropic model, found a proof for it". 증명은 Lean으로 검증됐다. Aaronson 원문: "which they say they obtained using their proprietary research harness on an internal Claude model. (What role the harness played, compared to the internal model, is something I don't know.)" 정확성에 대해서는 원문: "I believe the proof is correct for two reasons. Firstly, there's a Lean certificate"다.

중요한 이유: 외부 기업이 공개되지 않은 Claude 모델로 연구 성과를 냈다는 공개 사례다. 어떤 모델인지, 하네스의 몫이 얼마인지는 밝혀지지 않았다. Anthropic 입장도 찾지 못했다.

[PSI](https://psi.inc/research/aaronson-ambainis/) · [Aaronson 블로그](https://scottaaronson.blog/?p=10295)

- **Goldman Sachs가 Claude Code 사용을 엔지니어로 한정했다(Bloomberg 11-02, HN 11-08 18:16 UTC, 850점대):** 창 안에 HN 1위권에 올랐지만 원 보도는 일주일 전이고 이전 판들이 다루지 않았다. HN에 링크된 State of AI는 Bloomberg를 옮긴 글이다. 원문: "Internal guidance updated in October restricts Anthropic's Claude Code and Microsoft's GitHub to the bank's engineers, Bloomberg News reported". 댓글에서는 이 사이트가 AI로 쓴 글이라는 지적이 나왔다. [Bloomberg](https://www.bloomberg.com/news/articles/2026-11-02/goldman-restricts-traders-and-bankers-from-using-some-ai-tools) · [State of AI](https://www.thestateofai.com/news/goldman-sachs-claude-code-traders-bankers) · [HN](https://news.ycombinator.com/item?id=50415628)
- **DeepSeek Harness 일일 활성 사용자 154만 명(공식 X 11-08 14:07 UTC, 창 직전):** 원문: "As of November 4, counting users on the official API, DeepSeek Harness has reached 1.54 million daily active users, consuming roughly 92 trillion tokens per day." 공식 플러그인 마켓을 만들겠다고 했다. 도구 5번의 가짜 DeepSeek Harness 플러그인과 함께 본다. [X](https://x.com/DeepSeekHarness/status/2119431663642153128)

## 기술 이슈

창 안에서 가장 큰 진전은 Claude Code 승인 파일 비밀값 건의 외부 측정과 공격자 탐색 관측이다. 전역 GHSA는 창 안에 reviewed 0건(최신 게시 11-06), unreviewed 287건, malware 22건이 게시됐다. AI 저장소 162곳을 조회해 창 안 새 권고는 Arize Phoenix 1건이었다. HF Daily Papers 월요일분 28편은 모두 창 밖 arXiv 게시이고 화제작은 없다.

### 1. (Claude Code 비밀값 후속) GitGuardian: 공개 GitHub에 비밀값 31,088개, 탐지 당시 5,343개가 유효했다. Docker Hub 이미지에도 실려 나갔고, SANS 허니팟에 `.claude` 디렉터리 요청이 들어왔다 (보안업체 측정, GitGuardian 11-09 11:03 UTC, SANS Stormcast 11-09)

원문: "GitGuardian found 31,088 distinct secrets in Claude Code's .claude/settings.local.json on public GitHub, across 23,276 repositories, and 5,343 of them worked when we detected them." 직전 판들의 Apiiro 수치보다 훨씬 크다. GitGuardian은 두 조사가 원문: "answer different questions"라고 설명한다. 삭제되거나 덮어쓴 과거 버전까지 셌기 때문이다.

- **Docker Hub(원문):** "COPY . . ships the working tree, not the git index, so 613 secrets, 99 valid, reached public Docker Hub images; one valid Cloudflare API token appears in 559 tags." git 기록을 지워도 이미 올린 이미지에는 남는다.
- **"직접 만들어 커밋했다"는 설명에 대한 반론(원문):** "The content of the files was not typed by hand: in the 3,000 occurrences we inspected, every secret sat in an allow rule of the exact shape the permission prompt writes." 또 "In 263 repositories, the leaking commit carries Claude's noreply address as its git author" 직전 판의 Claude Code 팀원 댓글에 대한 반박이다.
- **무엇이 샜나:** 원문: "Real Anthropic API keys are rare in both files". 대부분 제3자 서비스 키다. `settings.json` 쪽에서는 DeepSeek 205개, OpenRouter 62개, MiniMax 50개의 유효한 게이트웨이 키가 나왔다.
- **추세(원문):** "monthly leaks in settings.local.json peaked at 12,535 occurrences in April 2026, then fell to 959 by October, and we do not yet know what changed."
- **공격자 쪽(SANS 원문):** "ISC web honeypots also saw new requests for the .claude directory this weekend; rotate exposed secrets and watch for Anthropic updates."
- **Anthropic에 제안한 수정(원문):** "Never persist an exact-match rule whose command contains a VAR=value prefix, a header value, URL userinfo or a password flag; offer a session-only approval instead."

중요한 이유: 이제 공격자가 이 경로를 찾기 시작했다. 직전 판의 조치(비밀값 교체 → `git rm --cached` → 저장소 `.gitignore`에 추가)에 `.dockerignore`에 `.claude/`를 넣는 것과, 이미 올린 이미지를 다시 빌드하는 것을 더한다. 웹 서버가 저장소 루트를 그대로 내보낸다면 `/.claude/` 경로부터 막는다. Anthropic 쪽은 창 안에 새 릴리스, 커밋, 직원 댓글이 없다(npm `latest` 2.1.319). 글에는 자사 제품 홍보가 섞여 있고, 그 훅도 원문: "The hook fails open when ggshield is not authenticated."다.

[GitGuardian](https://blog.gitguardian.com/claude-code-settings-local-json-secrets-docker-hub/) · [SANS Stormcast](https://isc.sans.edu/podcastdetail/10170) · [HN](https://news.ycombinator.com/item?id=50425934)

### 2. "ChatGPT 데스크톱이 SSH 에이전트를 원격 머신에 넘긴다": 근거로 든 PR은 머지되지 않았지만, 비슷한 동작이 사용자 이슈의 코드로 확인된다 (커뮤니티, HN 11-09 08:17 UTC, 130점대)

글 원문: "The agent running on a remote machine can authenticate using your local SSH credentials. That means it can push to your private repos on GitHub, for example." 근거로 단 `openai/codex` PR #28131은 6월에 머지 없이 닫혔다. 댓글에서도 원문: "five-month-old never-merged PR"이라는 지적이 나왔다.

- **실제 동작의 단서:** 11-05 사용자 이슈 #59694가 데스크톱 앱(26.1023.6123.0)이 원격에 만든 부트스트랩 스크립트를 인용했다. 원격의 `SSH_AUTH_SOCK`를 `forwarded-ssh-agent.sock` 고정 경로에 연결하고, 원문: "The shared server starts with `SSH_AUTH_SOCK` pointing to this stable path."
- **확인하지 못한 것:** 이 스크립트는 원격에 에이전트 소켓이 이미 있을 때만 연결한다. 따라서 사용자의 SSH 설정(`ForwardAgent`)을 따르는 것으로 보이지만, 앱이 전달을 직접 켜는지는 확인하지 못했다. 공식 문서에는 에이전트 전달 언급이 없다. 원문: "Remote access uses the connected host's projects, chats, files, credentials, permissions". 조회 시점에 OpenAI 측 답변은 없다.

중요한 이유: `Host *`에 `ForwardAgent yes`를 둔 사용자라면, 원격에 상주하는 에이전트가 연결된 동안 로컬 키로 서명할 수 있다. 원격 개발 호스트마다 `ForwardAgent no`를 두거나, 서명할 때마다 확인하는 에이전트(`ssh-add -c`, 하드웨어 키 터치)를 쓴다.

[HN](https://news.ycombinator.com/item?id=50422628) · [이슈 #59694](https://github.com/openai/codex/issues/59694) · [PR #28131](https://github.com/openai/codex/pull/28131) · [Remote connections 문서](https://learn.chatgpt.com/docs/remote-connections)

### 3. (OpenSSH sshole 후속) GreyNoise가 실제 침투 2건을 기록했고, 성공 공격 국가 목록에 한국이 있다 (보안업체 측정, GreyNoise 페이지 조회 시점 값, SecurityWeek 11-09 12:13 UTC)

GreyNoise 추적 페이지에 따르면 첫 성공 공격은 11-07 00:46:51 UTC였다. 클러스터 GT10은 원문: "2 intrusions · 7 addresses used"이고 C2는 `tac.vpnla.org`다. "Successfully exploited" 국가 목록에 한국 5건이 있다. 다만 신뢰도는 Medium이고, 정의 원문은 "Medium confidence: from artifacts found on an adversary's server, covering one shard of its scanning."이다. SecurityWeek가 인용한 Previdian 원문: "Previdian sensors have observed 20 exploitation attempts, 13 unique attacker IPs, and 9 countries targeting CVE-2026-123158."

중요한 이유: 직전 판의 SANS 허니팟 수치는 시도 건수였다. 이번에는 실제 침투가 기록됐다. musl 기반이면서 PAM 없이 비밀번호 인증을 쓰는 sshd는 지금이라도 패치하거나 비밀번호 로그인을 끈다. SANS 일지 33498은 직전 판 이후 갱신되지 않았다. GreyNoise 페이지는 창 직후(16:11 UTC)에 마지막으로 갱신돼 각 수치가 언제 들어갔는지는 확인하지 못했다.

[GreyNoise](https://sshole.events.greynoise.io/) · [SecurityWeek](https://www.securityweek.com/hackers-exploiting-sshole-authentication-bypass-in-openssh/)

### 4. Arize Phoenix: CVSS 10 권고가 나왔지만 반년 전에 고친 버그다 (공식, 저장소 권고 11-09 15:47 UTC)

LLM 관측 도구 Phoenix의 `applyChatTemplate` GraphQL에서 포맷 문자열을 주입해 환경 변수를 읽을 수 있다. 원문: "On default Phoenix installations where authentication is disabled the endpoint is reachable with no credentials." 그 결과 원문: "With this key, an attacker can forge admin-level session tokens and decrypt all secrets stored in the database." 영향 범위는 `arize-phoenix` 13.0.0 이상이고 15.7.0에서 고쳤다. 15.7.0은 5월에 나왔고 현재 최신은 21.0.0이다.

중요한 이유: Phoenix DB에는 LLM 공급자 API 키가 저장된다. 13.x~15.6.x를 인증 없이 열어 둔 곳은 올린 뒤 `PHOENIX_SECRET`과 저장된 키를 모두 교체한다. 최신을 쓰고 있다면 할 일은 없다.

[GHSA-gfv3](https://github.com/Arize-ai/phoenix/security/advisories/GHSA-gfv3-m769-5xm3)

### 5. 짧게

- **`@ethersphere/bee-js` 13.3.0이 악성이었다. 유효한 npm provenance가 붙어 있었다(보안업체, StepSecurity 11-09 01:18 UTC, AI 무관):** 봇 계정이 PR 없이 태그와 릴리스를 만들었고 정상 배포 워크플로가 그대로 빌드해 올렸다. 원문: "The npm package therefore carries valid SLSA provenance linking it to the legitimate repository and workflow." provenance는 어느 워크플로가 빌드했는지만 보증한다. 에이전트에게 주는 릴리스 권한도 같은 위험이 있다. 조회 시점에 13.3.0은 내려갔고 `latest`는 13.2.0이다. [StepSecurity](https://www.stepsecurity.io/blog/ethersphere-bee-js-13-3-0-compromise) · [이슈](https://github.com/ethersphere/bee-js/issues/1295)
- **Torvalds, "AI가 찾은 버그 수정 홍수가 정점일 수도"(The Register 11-09 04:53 UTC):** 7.4-rc2 공지에서 원문: "This time around Alexei claims that on the bpf side at least the fixes are now coming in faster than the new reports, and that sounds lovely if true," 이어서 "I think most other areas haven't reached that point yet". [The Register](https://www.theregister.com/security/2026/11/09/linus-torvalds-sees-signs-ai-prompted-patch-flood-has-peaked/5308345)
- **hnswlib·Spotify Voyager 벡터 인덱스 힙 오버플로(CVE-2026-124959, 전역 미검토 11-09 15:32 UTC):** 원문: "An attacker can supply a crafted index file with an element count greater than the stored capacity, causing the loader to write beyond the allocated buffer". 연구자 글 기준으로 수정이 아직 없다. 외부에서 받은 RAG 인덱스 파일은 그대로 로드하지 않는다. [GHSA](https://github.com/advisories/GHSA-pvwj-4565-gq34) · [연구자 글](https://m00dy.sh/notes/when-an-hnsw-index-lies-about-its-capacity)
- **악성 MCP 패키지 `@massnerder/x402-scraper-mcp`(malware GHSA 11-09 15:31 UTC):** 원문: "The package communicates with a domain associated with malicious activity." [GHSA](https://github.com/advisories/GHSA-whr8-5pp9-f7v2)
- **(Claude Code 이메일 User-Agent 후속) 개인정보 보호 플러그인이 막지 못했다는 보고(커뮤니티, 이슈 #107757 11-09 08:26 UTC):** 원문: "on 2.1.319 (macOS) it did not block a shell command carrying the account email in User-Agent." 루프백으로만 시험했고 댓글 첫 줄이 "From Opus 5.5:"인 재현 전 보고다. 창 안 댓글에 직원 답글은 없다. [댓글](https://github.com/anthropics/claude-code/issues/107757#issuecomment-6735559570)
- **(Wiz Black October 후속) Tensorlake 추가(Wiz 타임라인 11-09 11:50 UTC):** 원문: "Tensorlake disclosed an incident on 11-08, linked to "an upstream provider."" Tensorlake 원문 공지는 찾지 못했다. [Wiz](https://www.wiz.io/blog/black-october-for-ai-developer-platforms)
- **GitHub 장애(공식, 11-09 05:59~09:09 UTC):** 이틀 연속 익명 트래픽 급증이다. 원문: "CI jobs and release downloads may fail with a 504 Gateway Timeout status if they make anonymous calls." 10:00~11:50 UTC에는 Actions·Webhooks 장애도 있었다. [상태](https://stspg.io/jztczfplj6yb) · [Actions](https://stspg.io/4vdghm7tycm4)
- **논문(arXiv 월요일분):**
  - Neel Nanda 등, 강화학습 중 보상 해킹 감시(2611.09828): 원문: "models can learn to evade highly accurate monitors by exploiting systemic flaws in probes and LLM judges". [arXiv](https://arxiv.org/abs/2611.09828)
  - 법률 계약서로 본 프롬프트 주입 가드(2611.08834): 원문: "document-level false positive rates are as high as 92.39%". 계약서 문구를 주입으로 오인한다. [arXiv](https://arxiv.org/abs/2611.08834)
  - 에이전트 권한 이탈(2611.09113): 원문: "frontier-capable models exhibiting drift rates of 0.43 to 0.61". [arXiv](https://arxiv.org/abs/2611.09113)
- **직전 판 후속 중 새 진전 없음(조회 11-09 16시대 UTC):**
  - Open WebUI 0.13.1 비공개 보안 수정: 권고는 아직 없다(저장소 최신 10-18). 0.13.3은 도구 2번에 정리했다.
  - Go 1.27.4는 창 안에 나오지 않았다(최신 1.27.3). Node.js 보안 릴리스(11-11)도 예고 그대로다. patchtheplanet.org 새 항목은 없다.
  - CVE-2026-98432 Ubuntu 커널: 추적기 "Last updated"가 11-09로 바뀌었지만, 26.10만 수정됐고 26.04·24.04·22.04 LTS는 여전히 Vulnerable(work in progress) 상태다. [Ubuntu](https://ubuntu.com/security/CVE-2026-98432)
  - BabelDOC, Vercel AI SDK 6.x·5.x(6.0.326에도 수정 없음), LiteLLM 관리자 키 GHSA, Chainalysis Data Solutions(메타데이터만 갱신), WorkOS–ARTEX, ScopeWeaver, RunSybil CPython Jinja, Postman 플러그인(npm `latest` 0.7.1), Claude Dispatch "ThisBash", AgentScope CVE-2026-123022(patched 없음), Binance 최종 보고서, GitBook, Pydantic AI(옛 권고 메타데이터만 갱신), Firecracker PwnEscape, MaxxBench, Vikunja veans.
  - vLLM·SGLang·llama.cpp·Ollama·MCP 공식 SDK·Transformers·LangChain·n8n·Dify: 창 안 새 권고 없음.

### 참고: 창 경계 항목

**BOSSAI: 대만의 단독 공격자가 AI 에이전트로 타이베이 119 출동 기록을 빼냈다. 상용 모델이 거부하자 안전장치를 없앤 로컬 모델로 갈아탔다** (Acronis 11-04 01:00 UTC, THN 11-09 09:01 UTC)

Acronis가 공격자 서버의 파일 9만여 개를 분석했다. 원문: "Chinese commercial open-source models refused hacking instruction as well and even lectured the operator on cyber laws. As a result, the operator was forced to turn to locally run abliterated models, which were less effective and resource constrained." Claude Code와 Codex는 이렇게 썼다. 원문: "The operator ran shims that intercepted their API calls and routed them to the same abliterated DS4 that powered the attack agents." THN 원문: "The agents also found a Taipei fire department platform exposing patient data from 2018, including ECG readings and ambulance video footage, without a login."

중요한 이유: 상용 모델의 거부는 작동했다. 그러나 코딩 에이전트의 API 호출을 가로채 다른 모델로 돌리면 도구 자체는 그대로 공격에 쓰인다. 모델 쪽 안전장치만으로는 막을 수 없는 경로다.

[Acronis](https://www.acronis.com/en/tru/posts/bossai-inside-the-ai-attack-mesh-that-targeted-taiwans-emergency-services/) · [THN](https://thehackernews.com/2026/11/taiwan-based-hacker-used-bossai-ai.html)

**Google DeepMind: AI 에이전트 두 개가 RSA-1024 서명 위조 계산을 나흘 만에, 원래보다 3.5배 적은 계산량으로 재현했다** (논문 11-06, SecurityWeek 11-09 08:35 UTC)

논문 원문: "the first root took 391 CPU core-years, 3.5 times less than the original computation. Two cooperating AI agents designed, ran and debugged this computation in under four days." SecurityWeek에 따르면 사람은 약 20개 메시지를 보냈고 대부분 승인이었다. 모델 이름은 "Model A"·"Model B"로만 적었다. 한계도 분명히 했다. 원문: "Our results don’t threaten deployed cryptosystems today that are based on 2048-bit and larger RSA." 키를 인수분해한 것은 아니다.

중요한 이유: 에이전트가 수십만 CPU 코어 규모의 암호 계산을 스스로 설계하고 돌린 사례다. HSM의 PKCS#1 v1.5 서명처럼 1024비트를 남겨 둔 레거시 시스템은 안전 여유가 줄었다.

[논문 PDF](https://storage.googleapis.com/deepmind-media/papers/Faster_Affine_Root_and_a_1024bit%20Computation.pdf) · [SecurityWeek](https://www.securityweek.com/google-deepminds-ai-agents-make-1024-bit-rsa-keys-cheaper-to-attack/)

- **Asahi Linux가 MIT에서 GPL로 바꿨다(글 11-07, HN 11-07 05:20 UTC 420점대, 11-08 판이 놓쳤다):** 원문: "in-development feature code was being lifted straight out of our personal forks and published without attribution or lazily rewritten with LLM assistance to obfuscate its origin." LLM으로 출처를 지운 코드 도용이 라이선스 변경의 이유가 됐다. [Asahi Linux](https://asahilinux.org/2026/11/progress-report-7-3/) · [HN](https://news.ycombinator.com/item?id=50401867)
- **Next-Concept Modeling(arXiv 10-09, HN 11-09 07:20 UTC, 150점대):** 거친 개념부터 병렬로 생성하는 텍스트 생성 기법이다. 속도 수치는 저자 주장이다. [arXiv](https://arxiv.org/abs/2610.14162) · [HN](https://news.ycombinator.com/item?id=50422168)

## 써볼 만한 도구

미국 일요일이라 공식 도구 채널은 비어 있었다. Claude Code는 npm `latest`·`next` 2.1.319, `stable` 2.1.311 그대로다. Codex CLI(`latest` 0.167.0), Copilot CLI(1.0.107), Gemini CLI(0.67.0), Cursor, VS Code도 새 릴리스가 없다. Claude 공식 마켓플레이스, `anthropics/skills`, Codex 공식 플러그인 목록(`openai/plugins`)에도 창 안 머지가 없다. 의미 있는 릴리스는 유럽·아시아 월요일 낮과 미국 일요일 밤에 나왔다.

### 1. MongoDB MCP Server 3.3.0: 설정 마법사가 읽기 전용을 골라도 `--readOnly`를 넣지 않던 문제가 처음으로 npm에 반영됐다 (공식, npm 11-09 11:45 UTC, GitHub 12:01 UTC)

README가 권하는 `npx -y mongodb-mcp-server@latest setup` 마법사의 버그다. PR #1669 원문: "`AITool.buildMcpConfigEntry` built an `args` array containing `--readOnly` when read-only was selected, but then returned a hardcoded `["-y", "mongodb-mcp-server@latest"]`." 결과는 원문: "so Cursor, VS Code, Windsurf, Claude Desktop and Claude Code were never configured as read-only."다. 수정은 11-03에 머지됐지만 11-04의 3.2.2에는 들어가지 않았다. 3.2.2 릴리스 원문: "There was an issue during publishing and the npm package for this release was published from `0af7e43b`".

- **함정:** 생성된 설정은 `@latest`로 서버 버전만 따라간다. 이미 만들어진 `args`는 바뀌지 않으므로, 그 마법사로 만든 설정이라면 `--readOnly`를 직접 넣거나 setup을 다시 돌린다. 버그가 언제부터 있었는지는 확인하지 못했다.
- **새 기능 `connectionStrings`:** 서버 하나에 배포 여러 개를 미리 등록한다. PR #1593은 기존 방식이 원문: "(so the credentials go through the model)"이라는 점을 고쳤다. 원문: "The option is only accepted from the environment and config files."
- **확인되지 않은 동작:** 시작할 때 미리 연결하느냐를 두고 PR #1605는 원문: "connect-on-startup is now the default and there is no opt-out."이라 하고, 나중에 머지된 #1593은 원문: "nothing is dialed at startup"이라고 한다. 최종 동작은 확인하지 못했다.

추천 이유: 에이전트에 운영 DB를 읽기 전용으로 붙였다고 믿었다면 실제로는 쓰기 권한을 줬을 수 있다. 설정 파일을 열어 `--readOnly`가 있는지 지금 본다.

`npm i -g mongodb-mcp-server@3.3.0` · [v3.3.0](https://github.com/mongodb-js/mongodb-mcp-server/releases/tag/v3.3.0) · [PR #1669](https://github.com/mongodb-js/mongodb-mcp-server/pull/1669) · [PR #1593](https://github.com/mongodb-js/mongodb-mcp-server/pull/1593) · [PR #1605](https://github.com/mongodb-js/mongodb-mcp-server/pull/1605) · [v3.2.2](https://github.com/mongodb-js/mongodb-mcp-server/releases/tag/v3.2.2)

### 2. Open WebUI 0.13.3과 Tether 0.1.13: 예약 자동화로 코딩 에이전트를 돌리고, macOS·Windows에서는 작업 폴더 샌드박스를 걸 수 있다 (공식, Open WebUI 11-08 23:48 UTC, Tether 0.1.13 11-09 13:27 UTC)

0.13.3 원문: "The automation editor now lets you choose a terminal and working folder, then select a coding agent available there. Scheduled runs use that saved terminal and folder". 0.13.1·0.13.2 회귀도 고쳤다. 동반 앱 Tether 0.1.13은 Open Terminal용 샌드박스를 넣었다. 원문: "Sandboxed commands run offline by default; an optional Network access toggle permits outbound internet connections."

- **샌드박스 함정(가이드 원문):** Linux는 "Not available"이다. macOS에서는 "On macOS, outbound connections can reach local-network and host services as well." 프로세스 그룹에서 떨어져 나간 명령은 원문: "can outlive the Open Terminal service and retain its **original** workspace access." 또 "Sandboxing also does not prevent destructive actions within the workspace or resource exhaustion." 기존 설치는 원문: "Existing installations start with this option off."
- **보안 권고:** 0.13.1에서 약속한 보안 권고는 아직 없다. 0.13.3 노트에도 보안 언급이 없다.
- **Tether 라이선스:** 직전 판대로 개인 비상업 용도만 무료다.

추천 이유: 사람이 지켜보지 않는 예약 실행이 생겼으니 샌드박스와 함께 쓰는 게 맞다. 다만 Linux 서버에서는 샌드박스를 쓸 수 없고, 켜도 작업 폴더 안의 파괴적 명령은 막지 못한다.

`pip install -U open-webui` · [v0.13.3](https://github.com/open-webui/open-webui/releases/tag/v0.13.3) · [Tether 0.1.13](https://github.com/open-webui/tether/releases/tag/v0.1.13) · [샌드박스 가이드](https://github.com/open-webui/tether/blob/main/docs/sandbox.md)

### 3. Context7 MCP 4.6.0: 라이브러리 소스 검색과 버전 간 업그레이드 안내를 리서치 프리뷰로 넣었다 (공식, GitHub 11-09 13:13 UTC)

릴리스 원문: "Add the `query-code` MCP tool and the `ctx7 code` command. They search and read a library's source code at the version its docs describe". 업그레이드 안내 도구는 원문: "everything that changes between two versions of a library in one response"를 준다. PR #3704 원문: "About 100 of the most used libraries now have upgrade guides in production". 두 도구는 원문: "listed only with `--research-preview`, `CONTEXT7_RESEARCH_PREVIEW=1` or `?research_preview=1`."

- **함정:** 인덱싱이 안 된 라이브러리는 PR #3594 원문: "A "being indexed" reply is a normal result; agents retry at most twice, then use `query-docs`." 도구 호출이 늘 수 있다. 11-04 판이 다룬 4.5.0에서는 같은 프리뷰 옵션이 피드백 제출 도구도 켰는데, 4.6.0에서도 그런지는 확인하지 못했다.

추천 이유: 메이저 버전을 올릴 때 문서만으로는 알기 어려운 변경을 한 번에 받는다. 프리뷰이니 결과는 실제 변경 로그와 대조한다.

[4.6.0](https://github.com/upstash/context7/releases/tag/%40upstash%2Fcontext7-mcp%404.6.0) · [PR #3704](https://github.com/upstash/context7/pull/3704) · [PR #3594](https://github.com/upstash/context7/pull/3594)

### 4. (업그레이드 주의) CrewAI 1.15.38: 기본 모델이 전부 `gpt-6-luna`가 됐고, 평가 때 추적 데이터를 공유할지 묻는 기본값이 "예"다 (공식, GitHub·PyPI 11-08 19:39 UTC)

릴리스 원문: "Make gpt-6-luna crewAI's default model everywhere". PR #9272 원문: "Users with no `llm` and no `MODEL`/`OPENAI_MODEL_NAME` now run on `gpt-6-luna`. It reasons by default, gets no stop words (they're applied on the client), and returns a 400 for any temperature but 1." 게이트웨이를 쓰면 원문: "A gateway set through `OPENAI_BASE_URL` with `MODEL` unset must serve `gpt-6-luna`."

- **추적 데이터:** 릴리스 원문: "Eval asks to share the trace with Enter meaning yes, and explains why". PR #9207에 따르면 이 질문은 원문: "after the existing line saying the trace goes to CrewAI AMP for grading" 위치에 있다. Enter만 누르면 추적 데이터가 CrewAI AMP로 간다.
- **`.env` 권한:** PR #9224가 원문: "`crewai eval` and the run app's Evaluate button still created a missing `.env` world-readable" 문제를 고쳤다. 하지만 원문: "Existing `.env`: mode and contents left as they are." 이미 있는 파일은 직접 `chmod 600`한다.

추천 이유: 모델을 지정하지 않던 프로젝트는 비용·지연·응답이 조용히 바뀐다. temperature를 지정한 코드는 400 오류가 난다. 올리기 전에 `MODEL`을 명시한다.

[1.15.38](https://github.com/crewAIInc/crewAI/releases/tag/1.15.38) · [PR #9272](https://github.com/crewAIInc/crewAI/pull/9272) · [PR #9207](https://github.com/crewAIInc/crewAI/pull/9207) · [PR #9224](https://github.com/crewAIInc/crewAI/pull/9224)

### 5. (경고) 별 조작 의심 저장소: 미끼가 크랙 소프트웨어와 가짜 DeepSeek Harness 플러그인으로 넓어졌다 (직접 집계, 조회 시점 기준)

직전 판의 `windows-defender[.]cc` 유도가 이어진다. 표본 4개(iOS 잠금 해제, 시드 구문 생성기, KMS 크랙, 채굴기) README가 모두 원격 `Fix.ps1` 실행을 시킨다. 직전 판 표본 `PurpleChordEvaluate/claude-jailbreak-prompt`는 아직 내려가지 않았고 별이 300개에서 401개로 늘었다.

- **MCP 미끼:** `Nanoutrolley/roblox-studio-mcp`(11-09 08:58 UTC 생성, 별 279)의 다운로드 버튼이 telegra.ph로 연결된다.
- **가짜 DeepSeek Harness 플러그인:** `FJNB5/fujiang-pojia`(11-09 05:55 UTC 생성, 별 224, 포크 139)는 자동 침투 테스트 플러그인을 자처한다. 공식 범위처럼 보이는 `@deepseek-ai/...` 이름을 쓰지만 npm에 그런 공개 패키지는 없다. 설치는 `.cmd`를 더블클릭해 PowerShell 스크립트로 설정 파일을 덮어쓰는 방식이다.
- **확인 범위:** 스크립트와 첨부 파일은 받지 않았고 악성 여부는 확인하지 못했다. 별 조작도 직접 검증하지 못했다(조사 제약).

중요한 이유: DeepSeek Harness가 공식 플러그인 마켓을 예고한 시점에 비슷한 이름의 비공식 플러그인이 퍼지고 있다. 마켓이 나오기 전에는 저장소 소유자가 공식 조직인지부터 본다.

### 짧게

- **Qwen Code 0.27.1(공식, npm `latest` 11-09 13:42 UTC):** 원문: "No known breaking changes." LSP를 켜는 설정은 사용자 범위에서만 받는다. PR #14613 원문: "Workspace-scoped values are ignored, because turning LSP on lets a session spawn language servers with the user's own permissions". 남의 저장소를 열었을 때 언어 서버가 멋대로 뜨지 않게 한 것이다. [릴리스](https://github.com/QwenLM/qwen-code/releases/tag/v0.27.1) · [PR #14613](https://github.com/QwenLM/qwen-code/pull/14613)
- **Supabase MCP main(공식, 미배포, 11-09 14:07~14:45 UTC 머지):** 앞에 주석이 붙거나 `ONLY`·별칭을 쓴 WHERE 없는 UPDATE가 확인 없이 실행되던 문제를 고쳤다(PR #524). 호환성이 깨지는 PR #538은 원문: "release-please will cut 0.17.0"이고, 직접 운영하는 사람에게 원문: "A config built through a variable or spread drops the 120 silently, which would widen secret links to 10 minutes." npm `latest`는 0.16.1 그대로다. [PR #524](https://github.com/supabase-community/supabase-mcp/pull/524) · [PR #538](https://github.com/supabase-community/supabase-mcp/pull/538)
- **claude-xcode-agent(커뮤니티, 생성 11-09 08:27 UTC, MIT):** Xcode 27에서 ACP로 Claude Code를 쓴다. 함정 원문: "`setting_sources`: defaults to `project,local`. Set `user,project,local` to also load `~/.claude/settings.json`." 사용자 전역 deny 규칙과 훅이 기본으로는 적용되지 않는다. [저장소](https://github.com/yusuke-114/claude-xcode-agent)
- **image-to-sculpt(커뮤니티, 생성 11-08 21:32 UTC, MIT, 별 160대):** 이미지 한 장으로 3D 스컬프트를 만드는 Claude Code 플러그인이다. fal API 키와 Blender가 필요하다. 원문: "A typical case costs about $2". `claude plugin marketplace add gokayfem/image-to-sculpt` · [저장소](https://github.com/gokayfem/image-to-sculpt)
- **numbrone(커뮤니티, 생성 11-08 21:30 UTC, MIT):** herdr 위에서 프로젝트별 관리 에이전트와 작업 트리 단위 작업자를 돌린다. 원문: "Workers stop at every permission prompt" "That's the safe default." 원문: "not made by or affiliated with the herdr team". [저장소](https://github.com/stevenmichaelthomas/numbrone)
- **learn-from-lessons(커뮤니티, 생성 11-09 02:42 UTC, MIT):** 세션 기록에서 사용자가 에이전트를 고친 대목을 찾아 교훈으로 만든다. README는 원문: "Everything runs locally."라고 하지만, 정리 단계는 원문: "uses your own claude / codex CLI"다. 세션 기록이 모델 제공사로 간다. [저장소](https://github.com/assassin808/learn-from-lessons)
- **mightyCode(커뮤니티, 생성 11-09 02:56 UTC, 라이선스 없음):** Claude Code를 Apple Silicon의 로컬 MLX 9B 무검열 모델에 붙이는 프록시다. 원문: "This is a heuristic tripwire, not a sandbox." 또 "quality on complex agentic work is far below the real Claude". 라이선스가 없어 재사용은 권하지 않는다. [저장소](https://github.com/shi3z/mightyCode)
- **Amp, 러너 포털(공식, 날짜만 11-09):** 원문: "Portals on your runner are only accessible to you. To share them with your workspace, start a shared runner with --share". [글](https://ampcode.com/news/runner-portals)
- **Automattic Spacefast(매체보도, TechCrunch 11-09 14:00 UTC):** 바이브 코딩으로 만든 앱을 배포하는 플랫폼이다. 원문: "Users can connect Spacefast with other agents and chatbots like ChatGPT, Claude". 가격은 확인하지 못했다. [TechCrunch](https://techcrunch.com/2026/11/09/automattic-debuts-a-platform-to-deploy-vibe-coding-apps-amid-a-changing-internet/)
- **직전 판 후속 중 새 진전 없음(조회 11-09 16시대 UTC):** Claude Code(2.1.319), Codex CLI(`alpha` 0.169.0-alpha.3), Copilot CLI(`prerelease` 1.0.108-1), Gemini CLI, OpenCode 2.2.6, Crush 0.100.0, Vercel AI SDK(7.0.166은 의존성 갱신뿐), OpenAI Agents SDK(알파 그대로), LiteLLM(PyPI 1.108.3), Neon MCP, GitHub MCP 2.1.0, MCP Toolbox 1.16.0, Sentry MCP 0.49.0, Dify 1.19.0, Google ADK Python 2.14.0, sandbox-mcp, tinker-ai-skill, claude-work-mode, Pi(v1.5.0). devvm은 main에 지정 호스트에만 비밀값을 넣는 기능이 들어갔지만 릴리스는 0.1.1-rc.1 그대로다. llama-modchip은 제3자 벤치마크 이슈가 올라왔지만 284B DeepSeek 주장은 아직 재현되지 않았다. Conductor는 11-13 읽기 전용, Jules는 11-11 읽기 전용 전환 일정 그대로다.

### 참고: 창 경계 항목

- **Devin Desktop(옛 Windsurf) 3.12.32(공식, 날짜만 11-08):** 원문: "The editor now uses VS Code 1.141.2 and Electron 43.8.3, which includes Chromium security fixes." 창 안인지 확인하지 못했다. [변경 로그](https://docs.windsurf.com/windsurf/changelog)

## 주목할 점

- **비밀값 유출은 이제 공격자가 찾는 경로다.** 승인 규칙에 저장된 비밀값이 GitHub과 Docker Hub로 나갔고, 허니팟에 `.claude` 디렉터리 요청이 들어왔다. Anthropic이 승인 규칙 저장 방식을 바꾸는지, GitGuardian이 제안한 "세션 한정 승인"이 들어가는지 지켜본다. 같은 맥락에서 MongoDB MCP 건처럼 "읽기 전용", "샌드박스"라는 설정이 실제로 적용됐는지 확인하는 습관이 필요하다.
- **이번 주 일정:** Go 1.27.4(미국 화요일)와 Node.js(11-11) 보안 릴리스, Jules 읽기 전용 전환(11-11), Conductor 읽기 전용 전환(11-13), Anthropic 텍스트 워터마크(11-16), Anthropic 공모가 범위 공시, Inkling 2 Small 가중치("next week" 약속 주간)가 차례로 온다.

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - Bloomberg(Goldman), The Information(Anthropic 관련 기사), FT, Reuters(CAPTCHA)는 유료벽이나 차단으로 읽지 못했다. Goldman 건은 State of AI 재인용으로 실었다.
  - LWN의 Git LLM 정책 투표 기사는 유료라 읽지 못했다(11-19 무료 공개).
  - x.com은 개별 게시물만 `api.fxtwitter.com`으로 읽었다. 타임라인을 볼 수 없어 Anthropic 직원 반응을 놓쳤을 수 있다. reddit은 막혀 있다.
- **시각이 날짜만 있거나 불확실한 원문:**
  - LongCat-2.5 공개 시각: Hugging Face API는 비공개에서 공개로 바뀐 시각을 주지 않는다. Wayback 캡처와 ModelScope 생성 시각으로 추정했다. Meituan의 X 계정은 조회하지 못했다.
  - LG 보도자료와 EXAONE 변경 로그, PSI 글, Amp 글, Devin Desktop 변경 로그는 날짜만 있다.
  - GreyNoise 페이지는 창 직후(16:11 UTC) 값만 봤다. SANS Stormcast는 날짜만 있다.
- **확인하지 못한 것:**
  - ChatGPT 데스크톱이 SSH 에이전트 전달을 직접 켜는지(비공개 코드). MongoDB MCP의 시작 시 연결 동작과 setup 버그의 시작 버전.
  - Tensorlake 원문 공지, Open WebUI 0.13.1 보안 수정의 내용, Tencent CodeBuddy Code 2.182.0 릴리스 노트(문서 403).
  - GitHub 별 조작: stargazers API가 이 환경에서 빈 응답을 줘 별을 누른 계정을 보지 못했다.
  - 재현하지 않은 것: GitGuardian·GreyNoise·Previdian 수치, BOSSAI 귀속, DeepMind 계산, Arete 점검, 도구 동작. 도구는 하나도 설치하거나 실행하지 않았다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 스토리 807개 중 점수순 상위 60여 개는 전부, 나머지는 키워드로 훑었다. Show HN은 122건을 봤다.
  - 전역 GHSA: unreviewed 287건(IBM 82, WordPress 24 등 대량 게시)과 malware 22건을 키워드로 걸렀다.
  - arXiv 월요일분 약 750편은 제목 키워드로만 훑었다.
  - MCP 레지스트리: 창 안 게시 2,711건 중 504건이 두 네임스페이스였고, 대형 벤더 이름으로만 걸렀다.
  - 보지 못한 곳: x.ai 뉴스, qwen.ai, Moonshot·Volcengine 변경 로그(월 단위), Vertex 릴리스 노트(캐시 동결), Qwen·ByteDance Seed 공식 블로그(SPA).
- **자사·제3자 측정:** GitGuardian, SANS 허니팟, GreyNoise, Previdian, Acronis, LongCat·K-EXAONE 벤치마크, Arete 점검은 각 주체의 측정·발표이며 독립 검증 전이다.
