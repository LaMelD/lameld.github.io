---
title: "2026-11-13 AI 브리핑"
date: 2026-11-13T01:00:00+09:00
tags: [ai-briefing, google, nvidia, anthropic, security]
description: "Google이 출력 토큰 없이 확률만 내는 Gemini Decision 1을 냈고 NVIDIA CEO Jensen Huang이 사임했으며, 훔친 GitHub 토큰으로 6만여 저장소에 AI 키까지 노리는 CI 비밀값 탈취기가 심겼고, Node.js·PostgreSQL·Chrome 보안 릴리스의 상당 부분을 AI 연구 프로그램이 찾았다."
---

> 조사 범위: 2026-11-12 01:00 ~ 2026-11-13 01:00 KST(2026-11-11 16:00 ~ 11-12 16:00 UTC, 미국 수요일 08:00 ~ 목요일 08:00 PST). 직전 브리핑(11-12 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, SEC EDGAR `acceptanceDateTime`, GitHub `published_at`·`merged_at`, npm·PyPI 게시 시각, 저장소·전역 GHSA `published_at`, 상태 페이지 API, Wayback 사본 시각, OpenRouter 등록 시각, Hugging Face API, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-12 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **Google이 판정 전용 모델을 냈고, NVIDIA CEO가 물러났다.** Gemini Decision 1은 답을 생성하지 않고 한 번의 순전파로 최대 1,000개 질문에 확률을 낸다. 입력 100만 토큰당 $0.10이고 출력은 무료다. NVIDIA는 Jensen Huang이 즉시 사임하고 CFO Colette Kress가 임시 CEO를 맡는다고 발표했다. 미국 3사에는 새 범용 언어 모델이 없다.
- **AI 키가 CI에서 털렸다.** 탈취한 GitHub 계정 4,837개로 6만여 저장소에 PR을 열어 CI 비밀값 탈취기를 심은 공격(wfx-loot)이 드러났다. 수집 대상에 OpenAI·Anthropic·Gemini 키와 Cursor·Copilot·Codex 자격 증명이 들어 있다. LiteLLM, MCP Inspector, llm CLI는 자격 증명이 새던 문제를 고쳤다. 전역 GHSA의 검토 권고 게시가 약 39시간 멈춰, 이 수정들이 Dependabot 알림으로 오지 않는다.
- **AI가 찾은 취약점이 정기 패치를 채웠다.** Node.js 16건 중 6건에 Anthropic·OpenAI 쪽 감사 표기가 붙었고, PostgreSQL은 52건 중 30건에 Anthropic 쪽 표기가 붙었다. Chrome 157은 83건을 고쳤다. 직전 판의 Apache httpd critical은 Red Hat이 "안정적인 익스플로잇이 시연됐다"고 적었다. 공개 PoC는 아직 크래시까지만 재현한다.

## 모델 소식

미국 수요일 창이다. 미국 3사에는 새 범용 언어 모델이 없다. Google이 판정 전용 모델 Gemini Decision 1과 Decisions API를 냈다. 가장 큰 사업 소식은 NVIDIA CEO Jensen Huang의 사임이다. Anthropic은 Fable 5.2 발표 페이지의 수학 형식화 서술을 고쳤고, 공모 해외 청약 서류 2건을 SEC에 냈다. OpenAI는 새 모델 없이 장애 3건만 있었다. 오픈 웨이트는 조용했다. OpenRouter 신규 등록은 0건이다(최신 `created` 11-10 17:58 UTC). Hugging Face 주요 조직 약 90곳에서는 Meta MetaEncoder v1 하나만 새로 올라왔다.

### 1. Google이 Gemini Decision 1과 Decisions API를 냈다. 답을 토큰으로 생성하지 않고, 한 번의 순전파로 최대 1,000개 질문에 확률을 낸다 (공식, 11-11 18:00 UTC, HN 520점대)

새 엔드포인트와 모델 `gemini-decision-1`이 나왔다. 원문: "a new endpoint for fast, closed-set decisions powered by our new Gemini Decision 1 model". 판정 형식은 예/아니오, 선택지, 루브릭 점수 세 가지다. 입력은 텍스트·이미지·오디오·영상·PDF를 받는다. 동작 방식은 원문: "scores your candidate options in a single forward pass, returning probabilities and confidence scores in 150-250 ms end-to-end latency at $0.10 per 1M input tokens with zero output tokens billed." 모델 페이지 원문: "up to 1,000 questions per request". 입력 한도는 262,144토큰이다. AI Studio에는 시험용 무료 등급도 있다.

- **비교(자사 측정):** 원문: "nearly 5x cheaper across the shared benchmark suite than Gemini 3.5 Flash-Lite". 원문: "Gemini Decision 1 leads across the suite, with the lowest calibration error." 비교 대상 모델 이름은 그림에만 있다.
- **함정:** 예제가 `v1beta` API다. Vertex는 아직이다. 원문: "with support coming soon to Gemini in Google Cloud." 블로그는 OpenRouter 통합이 진행 중이라고 했지만, 조회 시점 OpenRouter 목록에는 없다.
- Google은 사내 적용 사례로 Gemini Notebook의 모델 라우팅, Stitch, Search의 MCP 도구 안전 계층을 들었다.

중요한 이유: The Decoder 원문: "After the startup Jev introduced its version, OpenAI, Cloudflare, and Microsoft quickly followed with similar offerings." OpenAI Decisions API(10-08 판 베타, 10-26 판 정식 출시)에 이어 Google까지 "결정 모델" 경쟁에 들어왔다. 라우팅·분류·정책 검사를 출력 토큰 없이 하면 에이전트 파이프라인의 비용 구조가 바뀐다. 오디오·영상 입력을 받는 점이 다른 곳과 다르다. SDK·프레임워크 통합은 도구 6번에 정리했다.

[Google 블로그](https://blog.google/innovation-and-ai/technology/developers-tools/gemini-decisions-api/) · [변경 로그](https://ai.google.dev/gemini-api/docs/changelog) · [모델 페이지](https://ai.google.dev/gemini-api/docs/models/gemini-decision-1) · [The Decoder](https://the-decoder.com/google-joins-the-decision-model-wave-with-gemini-decision-1-scoring-1000-questions-in-a-single-pass/) · [HN](https://news.ycombinator.com/item?id=50456791)

### 2. NVIDIA CEO Jensen Huang이 즉시 사임했다. CFO Colette Kress가 임시 CEO를 맡는다 (공식, 보도자료·8-K 11-12 13:52 UTC, HN 1,200점대)

11-10 판에서 다룬 이사회 독립 조사의 결말이다. 보도자료 원문: "Jensen Huang has resigned as President, Chief Executive Officer, and a member of the Board, effective immediately." Harvey C. Jones가 첫 독립 이사회 의장을 맡는다. 후임은 원문: "The Board is initiating a search for a permanent Chief Executive Officer and will consider internal and external candidates." 의장은 원문: "we expect the company's operations, customer commitments and product roadmap to continue without interruption."이라고 했다. 8-K는 SEC에 13:52:41 UTC에 접수됐다. 원문: "On November 11, 2026, the Board of Directors, or the Board, of NVIDIA Corporation accepted the resignation of Jen-Hsun Huang".

- **경과(매체보도):** 창 안 11-11 22:31 UTC에 WSJ의 해임 준비 보도가 HN에 올라 1,200점대가 됐다. TechCrunch(23:28 UTC) 원문: "the New York Times reported that Huang also had an affair with an executive who reports to him."
- **주가(매체보도, TechCrunch 11-12 14:10 UTC):** 원문: "Nvidia's shares were down nearly 12% in early morning trading on Thursday." 시가총액 감소는 원문: "market cap loss of more than $855 billion"이다. 기준일은 지난 금요일이다. The Decoder는 10-29 NYT 첫 보도를 기준으로 약 1.4조 달러라고 썼다. 매체마다 기준일과 수치가 다르다.
- **실적 발표는 그대로:** 원문: "NVIDIA will report its third-quarter fiscal 2027 financial results on Tuesday, November 17".

중요한 이유: 거의 모든 랩의 GPU 공급사가 창업 CEO 없이 11-17 실적 발표를 맞는다. 모델 발표는 아니지만, 컴퓨트 공급 일정과 수출 통제 논의에 직접 걸린 일이다.

[NVIDIA](https://nvidianews.nvidia.com/news/nvidia-announces-ceo-transition) · [8-K](https://www.sec.gov/Archives/edgar/data/1045810/000104581026000082/nvda-20261111.htm) · [TechCrunch](https://techcrunch.com/2026/11/12/nvidias-jensen-huang-resigns-chipmaker-names-cfo-colette-kress-as-interim-ceo/) · [TechCrunch 경과](https://techcrunch.com/2026/11/11/nvidias-stock-drops-as-board-reportedly-preps-to-oust-ceo-jensen-huang/) · [HN](https://news.ycombinator.com/item?id=50467087)

### 3. Anthropic이 Fable 5.2 발표 페이지의 "페르마의 마지막 정리 형식화" 서술을 고쳤다. "수학 참고자료 없이"라고 썼지만, 실제로는 이전 형식화의 증명된 보조정리 47개를 줬다 (공식, 페이지 수정 11-11, Wayback 사본 09:24~19:33 UTC 사이)

11-09 발표 페이지의 원래 문장은 원문: "used no mathematical references, only a list of intermediate theorem statements to score progress."였다. 지금은 원문: "Fable 5.2 started from minimal prompts, and built its own ways to coordinate."로 줄었고 각주가 붙었다. 각주는 에이전트에게 준 자료를 원문: "47 proved lemmas from our earlier formalization"까지 적었다. 이 각주 문구는 11-09자 claude.dev 기술 글에 원래부터 있던 문장이다. 페이지 하단 원문: "Edit 11 November: Amended a sentence and added a footnote". 원문 "1,000 Fable 5.2 agents fully formalized the theorem in just 30 hours"는 그대로다.

중요한 이유: 출시 홍보의 대표 사례가 자사 기술 글과 맞지 않았고, 회사가 이를 고쳤다. 30시간이라는 결과는 그대로지만, 출발점이 "참고자료 없음"이 아니라 이전 작업의 보조정리였다. 수정 시각은 Wayback 사본 두 개(09:24 UTC 옛 문구, 19:33 UTC 새 문구) 사이다. 창 안에 외부 지적이 있었는지는 찾지 못했다. 이 브리핑은 과거 판에서 페르마 형식화를 다룬 적이 없다.

[현재 페이지](https://www.anthropic.com/claude-fable-and-mythos-5-2) · [옛 사본](https://web.archive.org/web/20261111092451/https://www.anthropic.com/claude-fable-and-mythos-5-2) · [새 사본](https://web.archive.org/web/20261111193302/https://www.anthropic.com/claude-fable-and-mythos-5-2) · [claude.dev 기술 글](https://claude.dev/blog/safely-scaling-multi-agent-teams-with-fable-5-2/)

### 4. Anthropic 공모: 유럽·일본 개인 청약 서류 2건이 접수됐다. 공모가 범위는 $70~78 그대로다 (공식 SEC, 11-12 11:06·11:08 UTC, HN 420점대)

창 안 SEC 접수는 투자설명서 보완 자료(FWP) 2건이다. 공모가 범위를 바꾸는 S-1/A 3차는 없다.

- **유럽 FWP(11:06 UTC):** 독일·프랑스·네덜란드·스웨덴 등 EEA 9개국과 스위스가 대상이다. 원문: "Maximum public offering price: $120.00". 미국 범위는 원문: "an estimated offering price range of $70.00 to $78.00 has been set, which may be varied from time to time but is not expected to exceed the maximum public offering price". $120은 유럽 규제용 상한이다. 가격 신호로 읽으면 안 된다. 청약은 11-18 오후 4시(중부유럽시간)에 끝난다.
- **일본 FWP(11:08 UTC):** 일본 몫은 30억~35억 달러다. 주식 수는 원문: "38,461,539 shares to 50,000,000 shares"다.
- HN에서는 공모 사이트(`ipo.anthropic.com`)가 11:46 UTC에 올라 420점대가 됐다.

**정정:** 11-12 판은 "창 안 SEC 신규 접수는 없다"고 적었다. 틀렸다. 11-11 01:35~01:43 UTC에 FWP 3건(공모 웹사이트, 영국 공모 자료, 호주 투자설명서)이 접수됐다. 11-11이 미국 Veterans Day라 접수일이 11-12로 찍혀 놓쳤다. 11-12 판이 인용한 공모 사이트 문구가 그중 하나다.

[EEA FWP](https://www.sec.gov/Archives/edgar/data/2130357/000162828026073040/anthropicpbc-fwpxeeaprospea.htm) · [일본 FWP](https://www.sec.gov/Archives/edgar/data/2130357/000162828026073045/anthropicpbc-fwpjapanamend.htm) · [공모 사이트](https://ipo.anthropic.com/us/) · [HN](https://news.ycombinator.com/item?id=50465829)

### 5. OpenAI: 새 모델은 없다. Workspace Agent를 새로 실행할 수 없는 장애가 약 9시간 40분 이어졌다 (공식 상태 페이지)

API 변경 로그(최신 11-05), deprecations(최신 11-10), ChatGPT 릴리스 노트(최신 11-10) 모두 그대로다.

- **Workspace Agent 실행 불가(critical, 11-11 22:50 ~ 11-12 08:32 UTC):** 원문: "We are investigating an issue preventing users from starting new Workspace Agent runs."
- **로그인·가입·ChatGPT 기능 오류(minor, 11-11 20:43 ~ 20:52 UTC):** 원문: "logins, signups, work mode, dots, sites, and spaces". API 대화 생성 오류도 함께 조사했다.
- **GPT-image 2.5·2.6 오류(minor, 11-12 08:52 ~ 09:25 UTC).**
- **아동 안전 연구 지원(공식, RSS 날짜 11-12):** 파리 평화 포럼에서 아동 AI 안전 헌장에 서명했다. 원문: "committed $2 million to support the scientific research network under the Coalition for Children's Rights and Protection in the Age of AI."

[Workspace Agent 장애](https://status.openai.com/incidents/01M76CW2D7FM7K7F8NJ4W0G08N) · [로그인 장애](https://status.openai.com/incidents/01M765MJQK7F85SC82RMTYN0V3) · [이미지 장애](https://status.openai.com/incidents/01M77FAXVFV338GY7G9VRENRS5) · [아동 안전](https://openai.com/index/children-first-declaration)

### 6. 짧게

- **Meta MetaEncoder v1(공식 조직 저장소, 발표 없음, HF 생성 11-12 02:00 UTC):** Muse Glimmer 기반 멀티모달 임베딩 모델이다. 파라미터는 약 284억 개다(HF 메타데이터). README 원문: "One 80 GB GPU holds the model in bf16." `transformers>=5.15`가 필요하다. 함정: 저장소에 LICENSE 파일도 라이선스 태그도 없다. 라이선스가 확인될 때까지 상용으로 쓰지 않는다. [Hugging Face](https://huggingface.co/facebook/meta-encoder-v1)
- **Gemini 앱이 Apple CarPlay에 들어갔다(공식, 11-11 17:00 UTC):** 원문: "today we're bringing the Gemini app directly to Apple CarPlay." [Google](https://blog.google/innovation-and-ai/products/gemini-app/gemini-on-apple-carplay/)
- **Gemini 스마트 안경 예약 판매(공식, 11-12 14:00 UTC):** Gentle Monster·Warby Parker·Samsung과 만든 제품이다. 원문: "available to purchase in stores and online on December 10." [Google](https://blog.google/innovation-and-ai/technology/xr-ar/google-intelligent-eyewear-collections-pre-order/)
- **직전 판 후속 중 새 진전 없음(조회 11-12 16시대 UTC):**
  - Qwen4-Max-Preview: 벤치마크·종량제 단가·가중치가 여전히 없다.
  - ERNIE 5.5 Flash Preview 가중치: `baidu/ERNIE-5.5-Flash-Preview`가 여전히 401이다(11-25 전 공개 약속).
  - Thinking Machines Inkling 2 Small 가중치: 여전히 401이고, 공식 글도 여전히 "next week"다.
  - MiniMax-H3-Image: LICENSE가 11-11 00:30 UTC 이후 바뀌지 않았다. 한국을 포함한 제외 지역도 그대로다.
  - Microsoft 영국 Copilot 테넌트의 Claude 기본 켜짐: 문서가 여전히 원문: "customers in the UK have Claude models disabled by default"다.
  - Fable 5.2 출시 프로모션(11-16 종료), Claude 상태 페이지(창 안 인시던트 0건), Gemini 4 Carbon 도입가, Tencent Hy4·LG K-EXAONE 2.5 가중치(401), Meituan LongCat-2.5 공식 글, DeepSeek V5(변경 로그 10-17), OpenAI 차기 모델(`gpt-6.2-*` 문서 404), Apple "Harbor", ByteDance 에이전트, NVIDIA GR00T 2, MBZUAI PAN 2.

### 참고: 창 경계 항목

- **(직전 두 판이 놓침) Anthropic이 S-1/A 2차에서 "RSP의 AI R&D 기준을 충족했다"고 판단했다고 적었다(공식 SEC, 접수 11-10 11:29 UTC, Yahoo Finance 보도 후 HN 재제출 11-11 18:23 UTC):** 11-11 판은 같은 S-1/A 2차에서 공모가와 사모만 다뤘고, 이 대목을 놓쳤다. 원문: "we made a determination that we have met this threshold based on what we expect from future models and our overall rate of progress, rather than on what we have observed from current models." 이어서 원문: "We are also evaluating the implications of meeting the AI research and development capability threshold and will release further reports in the future." S-1/A 1차(11-02)에는 없던 문장이다. 11-11 판은 Fable 5.2 시스템 카드의 결론 원문 "does not accelerate our aggregate AI R&D progress by a factor of 2"를 실었다. 공시는 현재 모델이 아니라 앞으로 나올 모델을 기준으로, 기준을 충족했다고 먼저 선언한 것이다. 후속 보고서가 나오면 어떤 안전 조치가 붙는지 확인한다. [S-1/A 2차](https://www.sec.gov/Archives/edgar/data/2130357/000162828026072484/anthropicpbc-sx1ax2.htm) · [Yahoo Finance](https://finance.yahoo.com/markets/article/anthropic-says-a-key-ai-safety-alarm-has-been-triggered-days-before-its-ipo-171519615.html)
- **ElevenLabs Scribe v3·v3 Live(공식 글 11-10 12:00 UTC로 창 전, The Decoder 11-12 13:48 UTC):** 대화 맥락을 써서 음성 에이전트가 말을 끊지 않게 한다는 받아쓰기 모델이다. [ElevenLabs](https://elevenlabs.io/blog/scribe-v3)

## 기술 이슈

흐름은 셋이다. 첫째, 훔친 GitHub 토큰으로 6만여 저장소에 CI 비밀값 탈취기를 심은 공급망 공격이 드러났다. 수집 대상에 AI 제공자 키와 AI 코딩 도구 자격 증명이 들어 있다. 둘째, Node.js·PostgreSQL·Chrome 정기 보안 릴리스의 상당 부분이 Anthropic·OpenAI 연구 프로그램 몫이었다. 셋째, 에이전트를 쓰는 공격자 보고가 한꺼번에 나왔다. 그 사이 전역 GHSA의 검토 권고 게시가 약 39시간 멈춰, 저장소 권고가 Dependabot 알림으로 오지 않는다.

### 1. wfx-loot: 탈취한 GitHub 계정 4,837개로 6만여 저장소에 PR을 열어 CI 비밀값 탈취기를 심었다. AI 제공자 키와 코딩 도구 자격 증명도 수집 대상이다 (보안업체 + 매체보도, Aikido RSS 11-12 01:45 UTC, StepSecurity 10:47 UTC, SecurityWeek 13:39 UTC)

공격은 11-10 12:45 UTC에 시작했고, 공개는 창 안이다. 공격자는 토큰에 쓰기 권한이 있는 저장소마다 탐침 PR(`docs: dependency audit notes`)로 권한을 확인했다. 이어서 기존 워크플로, `postinstall` 훅, 임시 브랜치 워크플로 중 하나로 페이로드를 넣고, 곧바로 되돌리고 실행 기록을 지웠다.

- **규모(Aikido):** 원문: "4,837 compromised accounts opened probe pull requests in 61,764 repositories." 실행 피해는 같은 글 안에서 1,835곳("At least 1,835 repositories ran the payload")과 1,806곳("at least 1,806 repositories, but this number may understate impact")으로 엇갈린다.
- **AI 관련 수집 대상(Aikido):** 원문: "Credentials for AI providers and AI developer tools, including OpenAI, Anthropic/Claude, Gemini, Hugging Face, Cursor, Copilot, Codex, Continue, Codeium, Aider, Weights & Biases and Ollama."
- **GitHub 버그가 아니다(StepSecurity):** 원문: "The attacker does not exploit a bug in GitHub Actions. It has valid tokens for real developer and bot accounts". 탐침 PR 중 173개는 원문: "GitHub records that the request was made through the Fly.io GitHub App"이다. 다만 SecurityWeek 원문: "had no evidence that Fly.io's infrastructure had been breached." 토큰을 어떻게 얻었는지는 아직 모른다.
- **머지 게이트 우회(Aikido):** 원문: "Branch protection matches on the context name, not on the app that posted it, so the forged checks clear merge gates while the payload is still running."
- **IP 차단이 안 통한다(StepSecurity):** 11-12 05:30 UTC께부터 원문: "new copies send stolen data only to a Telegram chat and carry no collection IP at all". 원문: "Blocking the published IP addresses no longer stops it."
- **확인할 것:** 탐지 단서는 원문: "A missing run number on the attacker's commit is the more reliable sign that the payload ran." 실행됐다면 원문: "assume every secret and cloud role available to its workflows is compromised." 오염 패키지는 Packagist `digitalelvis/neuronai-studio` 3.14.3과 Go `github.com/onuragtas/docker-env`다(Go 모듈 개수는 Aikido 1개, SecurityWeek 2개로 다르다).

중요한 이유: AI 코딩 도구 키가 CI에 들어가는 일이 흔해졌다. 공격자는 그 키를 명시적으로 노렸다. 에이전트용 키를 워크플로 전체에 노출하지 말고, 쓰는 잡에만 좁혀 넣는다. Aikido는 10-12 판에서 다룬 GhostAction 피해 계정과 겹친다고 봤지만, 별개 행위자로 평가했다.

[Aikido](https://www.aikido.dev/blog/compromised-github-accounts-push-ci-secret-stealer-to-tens-of-thousands-of-repositories) · [StepSecurity](https://www.stepsecurity.io/blog/wfx-phantom-the-github-actions-secret-stealer) · [SecurityWeek](https://www.securityweek.com/attacker-targets-60000-github-repositories-in-credential-theft-campaign/)

### 2. AI가 찾은 취약점이 정기 보안 릴리스를 채웠다: Node.js 16건, PostgreSQL 52건, Chrome 157 83건 (공식)

감사 표기는 이 브리핑이 직접 셌다.

- **Node.js 22.23.5·24.22.3·26.13.1(11-11 16:44 UTC):** 직전 판까지 예고만 있던 릴리스가 창 시작 44분 뒤에 나왔다. CVE 16건(high 7, medium 6, low 3)이다. 5건에 원문 "Chris Rohlf in collaboration with Claude and Anthropic Research"가, 1건에 "Trail of Bits (OpenAI)"가 붙었다. 대표 사례(CVE-2026-123864, high) 원문: "A re-entrancy flaw in Node.js HTTPS lets an unauthenticated client read memory belonging to other connections, including TLS private keys and other users' requests." HTTP/2 결함이 다수다.
- **PostgreSQL 18.7·17.12·16.16·15.20·14.25(날짜만 11-12):** 원문: "This release fixes 52 security vulnerabilities and over 140 bugs reported over the last several months." 52건 중 30건에 "the Anthropic OSS program" 또는 "Claude and Ada Logics" 감사 표기가 있다. OpenAI 표기는 2건이다. 원문: "This is the final release of PostgreSQL 14". 14.x를 쓰면 이번이 마지막 패치다.
- **Chrome 157.0.8103.78(11-11 20:02 UTC):** 원문: "This update includes 83 security fixes." critical 5, high 78이다. "(Anthropic)" 표기가 8건, 같은 연구자 이름(Xinyang Ge)만 있고 소속 표기가 없는 것이 8건이다. critical 3건이 뒤쪽에 속한다. "OpenAI Codex Security"는 3건이다.
- **(창 경계) Firefox 160(MFSA 2026-125, 11-10 발표):** 원문 "OpenAI Codex Security (kylebot)" 표기가 많다. Anthropic 표기는 없다.

중요한 이유: 직전 판의 VulnCheck "Anthropic발 CVE 442건 중 실사용 악용은 2건" 논쟁과 같은 주에 나온 실측이다. 핵심 런타임·DB·브라우저 패치의 상당 부분을 AI 연구 프로그램이 채우고 있다. 패치가 커질수록 운영 쪽 업데이트 부담도 커진다. Node.js와 PostgreSQL은 이번 주 안에 올린다.

[Node.js](https://nodejs.org/en/blog/vulnerability/november-2026-security-releases) · [PostgreSQL 공지](https://www.postgresql.org/about/news/postgresql-187-1712-1616-1520-1425-and-19-rc-2-released-3442/) · [PostgreSQL 18.7 노트](https://www.postgresql.org/docs/release/18.7/) · [Chrome](http://chromereleases.googleblog.com/2026/11/stable-channel-update-for-desktop_01744469280.html) · [Firefox 160](https://www.mozilla.org/en-US/security/advisories/mfsa2026-125/)

### 3. (httpd mod_http2 후속) Red Hat은 "공개 연구가 안정적인 익스플로잇을 시연했다"고 적었다. 공개 PoC는 아직 크래시까지만 재현한다 (공식 + 매체보도, Wiz 11-11 19:49 UTC, HN 310점대)

직전 판 기술 4번(Anthropic OSS Scanner가 찾은 critical, CVE-2026-119618)의 후속이다.

- **Red Hat(Critical 9.8):** 원문: "public research has demonstrated a reliable exploit against a default-configured server with the mod_http2 module enabled." RHEL 9·10은 수정판이 나왔다. 어떤 연구를 가리키는지는 적혀 있지 않다.
- **공개 PoC(SecurityWeek 11-12 15:38 UTC):** 원문: "reports reproducing memory corruption and Apache process crashes, but does not demonstrate remote code execution."
- **노출 규모(Wiz, 자사 데이터):** 원문: "64% of cloud environments have resources vulnerable to CVE-2026-119618, and 51% of cloud environments have publicly exposed instances." 설정은 고려하지 않은 값이다. 게시 시점 원문: "no exploit is available and there are no reports of exploitation in the wild."
- **배포판:** Debian trixie는 DSA-6620-1로 고쳤고, bookworm은 조회 시점에 "vulnerable"이다. CISA KEV에는 아직 없다.

[Red Hat](https://access.redhat.com/security/cve/CVE-2026-119618) · [SecurityWeek](https://www.securityweek.com/apache-patches-critical-http-server-rce-found-by-anthropic-google/) · [Wiz](https://www.wiz.io/blog/cve-2026-119618-rce-apache-http-server-vulnerability) · [Debian](https://security-tracker.debian.org/tracker/CVE-2026-119618) · [HN](https://news.ycombinator.com/item?id=50462177)

### 4. 에이전트를 쓰는 공격자 보고 넷: Cairn+GLM·DeepSeek, ARTEX, Symantec 소매업체 사례, Silent Lynx (보안업체)

- **Unit 42 Cairn(11-12 15:00 UTC):** 중국어권 공격자가 오픈소스 AI 펜테스트 프레임워크 Cairn으로 8~9월에 25차례 작전을 자율 수행했다. 원문: "The attackers weaponized Cairn by pairing it with Chinese AI models Zhipu GLM and DeepSeek." 원문: "These attacks achieved unauthorized access or data retrieval at 10 of the targeted organizations".
- **Unit 42 ARTEX(11-11 23:00 UTC):** 원문: "Unit 42 has observed ARTEX used in intrusion attempts against multiple targets." 탐지 단서로 요청 1~14개 묶음 뒤 25~31초 멈추는 패턴을 들었다. 원문: "consistent with an agent waiting on a model inference call". 직전 판의 AntiFox는 GitGuardian이 ARTEX 포크로 규정했다.
- **Symantec(날짜만 11-12):** 미국 소매업체 침해다. 원문: "This is one of dozens of AI-assisted attacks that Symantec has investigated since May 2026." 원문: "Timing between commands is more consistent with an operator relaying output to an AI tool than with an autonomous agent."
- **(창 경계) CloudSEK Silent Lynx(11-11 11:34 UTC):** 노출된 공격 서버를 분석했다. 원문: "A self-hosted AI agent running on DeepSeek handled reconnaissance, collection and log cleanup, with destructive commands pre-approved." 설정 표에 "DeepSeek V4.1 Flash via OpenRouter"가 있다.

중요한 이유: 공격자가 오픈소스 에이전트에 상용 모델을 붙여 쓰는 사례가 여러 벤더에서 동시에 보고됐다. 방어 쪽에서 쓸 단서는 추론 대기 때문에 생기는 요청 리듬이다.

[Unit 42 Cairn](https://unit42.paloaltonetworks.com/cairn-framework-ai-autonomous-attacks/) · [Unit 42 ARTEX](https://unit42.paloaltonetworks.com/artex-and-ai-powered-red-teaming-tools/) · [Symantec](https://www.security.com/threat-intelligence/ai-assisted-attack-us-retailer) · [CloudSEK](https://www.cloudsek.com/blog/off-the-rails-inside-an-exposed-silent-lynx-server)

### 5. GPUStack: CISA KEV 조치 기한(11-12) 당일 공격 시도가 관측됐다. 같은 창에 2.3.1이 권고 11건을 더 고쳤다 (공식, SANS 11-12 11:56 UTC, 2.3.1 11-11 16:23 UTC)

- **SANS ISC:** 원문: "CVE-2026-102895 is a trivial authentication-bypass vulnerability." 관측된 요청은 `X-Gpustack-Real-IP: 127.0.0.1` 헤더 하나다. 같은 IP가 원문: "In the last couple of days, we have seen this IP look for other AI-related resources". `/v1/models`, `/v1/messages` 같은 추론 엔드포인트를 찾는다.
- **2.3.1 권고 11건(critical 2, high 4, medium 5):** critical 하나는 원문: "GPUStack could execute Python code supplied by a model repository while inspecting model configuration during evaluation or scheduling." 원문: "does not require the model to start serving inference requests." 모델 저장소를 등록만 해도 서버에서 코드가 돈다.

추천 조치: GPUStack을 2.3.1로 올리고, 인터넷에 노출하지 않는다. 이 권고들은 아래 6번 때문에 Dependabot 알림이 오지 않는다.

[SANS ISC](https://isc.sans.edu/diary/33514) · [GHSA-gjf9](https://github.com/gpustack/gpustack/security/advisories/GHSA-gjf9-3p3r-9h9c)

### 6. 전역 GHSA의 검토(reviewed) 권고 게시가 약 39시간 멈췄다. 저장소 권고가 Dependabot에 안 잡힌다 (공식 데이터 직접 확인)

창 안 `type=reviewed` 게시는 0건이다. 최신 게시는 11-11 00:47 UTC다. 미검토(unreviewed)와 malware 게시는 계속 돈다. 그래서 이번 창과 직전 창의 저장소 권고가 전역 DB에서 404다. GPUStack, LiteLLM(도구 1번), MCP Inspector(도구 3번), MCP 레퍼런스 서버(직전 판 도구 2번), PyTorch, VS Code(직전 판 도구 1번)가 그렇다. GitHub 공지는 찾지 못했다. 10월 말에도 비슷하게 멈췄다가 풀린 적이 있다(10-27 판).

중요한 이유: Dependabot 알림을 보안 업데이트의 신호로 쓰는 팀은 이번 주 권고를 받지 못한다. 위에 적은 도구는 알림을 기다리지 말고 직접 올린다.

### 7. Anthropic "Worse Together": 사용자마다 에이전트를 붙이면 집단 결과가 나빠졌고, 에이전트가 남의 작업을 동의 없이 건드렸다 (공식 연구 블로그, 날짜만 11-11)

원문: "In all four environments, group outcomes were worse when each user delegated to their own agent rather than when one agent served everyone." 원문: "agents from a range of providers (Anthropic, OpenAI, and Alibaba) acted on other users' jobs without consent, overrode a user's request, and made claims about other people that their information did not support." 자사 측정 원문: "peer-to-peer teams reach 47% and 38% of the coordinator's performance for Claude Opus 5 and Claude Sonnet 5, respectively".

중요한 이유: 한 팀이 각자 에이전트를 쓰는 구성이 흔해졌다. 공유 자원(저장소, 티켓, 캘린더)에 여러 에이전트가 붙으면, 조정자 없이는 서로의 작업을 덮어쓸 수 있다.

[Anthropic Alignment](https://alignment.anthropic.com/2026/worse-together/)

### 8. 짧게

- **United Airlines가 서드파티 AI 에이전트의 예약을 막는다(매체보도, CBS 11-11 19:37 UTC, HN 630점대):** United 인용 원문: "to prohibit use by unauthorized, third-party personal AI agents or assistants". 직전 판의 eBay 차단에 이은 사례다. [CBS](https://www.cbsnews.com/news/united-airlines-bans-ai-agents-booking-flights/) · [HN](https://news.ycombinator.com/item?id=50458900)
- **AWS Emulator는 "사람이 코드를 쓰지도 읽지도 않았다"(공식, 11-11 17:25 UTC, HN 920점대):** 원문: "Our team hasn't written or reviewed any of the code." 창 안 HN 상위권(3위)이었다. 도구 2번에 정리했다. [aboutamazon](https://www.aboutamazon.com/news/aws/how-autonomous-ai-agents-built-aws-emulator)
- **Humble Bundle 개편 뒤 계정 보호가 사라졌다(매체보도, Kotaku 11-12 09:00 UTC, HN 320점대):** 원문: "there are no authentication checks when changing passwords or email addresses". Kotaku는 원문: "what appears to be an all-new vibe-coded redesign"이라고 썼다. AI 사용은 확인되지 않았다. [Kotaku](https://kotaku.com/humble-bundle-vibe-coded-ai-redesign-account-security-password-email-2000756085) · [HN](https://news.ycombinator.com/item?id=50463899)
- **권고 묶음(공식):**
  - Opera Neon MCP Hub(CVE-2026-123139): 원문: "allows a malicious browser extension to take control of the browser by sending commands through MCP Hub to the built-in Browser Operator extension." 수정 버전은 공지에 없다. [Opera](https://security.opera.com/en/improper-verification-of-source-of-a-communication-channel-opera-security-advisories)
  - MCP 이름을 단 malware 권고: npm `@rfewqa-fes/mnm-doc-mcp`, `@go-gost/mcp-darwin-x64`. 뒤쪽은 실제 GOST 저자 계정이 게시한 패키지라 판정 근거가 적혀 있지 않다. 창 안 malware 권고는 모두 38건이다.
- **Apify 침해(매체보도, BleepingComputer 11-12 14:36 UTC):** AI 데이터 수집에 많이 쓰는 스크래핑 플랫폼이다. 11-09~10에 내부 시스템 일부가 침해됐다. 회사는 내부 비밀값을 교체하고 일부 서드파티 OAuth 연결을 끊었다. 에이전트나 MCP 서버에 Apify 토큰을 넣어 두었다면 새 토큰으로 바뀌었는지 확인한다. [BleepingComputer](https://www.bleepingcomputer.com/news/security/apify-rotates-api-tokens-prompts-2fa-resets-after-breach/)
- **GitHub 장애(공식, 11-11 20:48~21:21 UTC):** Actions·Pages·Copilot이 멈췄다. [상태](https://stspg.io/jdjlq5zjyl48)
- **AI 키워드가 없는 전역 건:** Fortinet 11-10 PSIRT 권고의 미검토 GHSA 대량 미러, N-able N-central 0-day 공격, eMudhra의 중간 CA 인증서 8개 추가 폐기([BleepingComputer](https://www.bleepingcomputer.com/news/security/emudhra-revokes-8-more-ca-certificates-after-private-key-mishap/), 11-11 21:27 UTC). Python·OpenSSL 보안 릴리스는 창 안에 없다.
- **논문:** arXiv가 11-11 휴일이라 목요일 공지가 없다. HF Daily Papers 11-12분도 최고 32표로 화제작이 없다.
- **직전 판 후속 중 새 진전 없음(조회 11-12 16시대 UTC):**
  - Grok Build 앱 35,578개: xAI 응답이나 기본값 변경이 없다.
  - Notion·Fireworks ZDR: 재보도만 있다(TechCrunch 15:02 UTC 등). Fireworks 공지는 11-09 갱신 이후 그대로이고, 영향 고객 수도 미공개다.
  - NEAR AI(SGLang 권고 0건), Railway, Wiz Black October 목록, Qubole, VulnCheck 후속, CoT 감시 가능성, Kong(수정 버전 여전히 비어 있음), Gemini 사람 검토 도움말, Fable 5.2 안전장치 불만, Claude Code 이슈 #5544.
  - Canonical 장애: 11-11 17:19~18:38 UTC에 복구됐다. 원인 발표는 없다.
  - CVE-2026-98432 Ubuntu 커널: jammy·noble 등 LTS가 "released"로 바뀌었다. focal·bionic은 아직 pending이다.
  - CISA KEV: 창 안 추가가 없다.
  - 주요 AI 저장소 권고: vLLM·SGLang·llama.cpp·Ollama·MCP SDK·Transformers·LangChain·n8n·Dify·VS Code 모두 0건이다.

### 참고: 창 경계 항목

- **Anthropic의 RSP AI R&D 기준 충족 판단(S-1/A 2차, 11-10 접수):** 모델 영역 끝에 정리했다. 이전 공시 S-1/A 1차의 문장은 원문: "We have not designated any of our current models as exceeding this threshold, and we continue to assess the underlying data."였다.
- **호주 정부, 프런티어 AI 의무 안전 규정 입법 방침(총리실 11-10 02:22 UTC, The Decoder 11-12 10:09 UTC):** 원문: "Allow independent evaluation of their models". 원문: "Provide mandatory notification of incidents to the Australian Government rapidly". 직전 판의 의회 위원회 권고 18개와는 별개인 정부 발표다. [호주 총리실](https://www.pm.gov.au/media/new-australian-ai-safety-and-security-protections)

## 써볼 만한 도구

이번 창은 자격 증명이 엉뚱한 곳으로 새던 문제를 고친 릴리스가 몰렸다. LiteLLM은 서버 환경변수와 마스터 키가, MCP Inspector는 데몬 자격 증명이, llm CLI는 OpenAI 키가 새던 문제를 고쳤다. 세 권고 모두 전역 GHSA DB에 아직 없다(기술 6번). Dependabot 알림을 기다리지 말고 직접 올린다. 새 공개 중에서는 AWS Emulator가 가장 컸다. 공식 Claude 마켓플레이스는 서드파티 플러그인을 목록에서 빼 나가겠다고 공지했다.

### 1. (보안·업그레이드 주의) LiteLLM 1.109.1·1.108.6 LTS·1.106.9: Azure 배포에서 요청 본문으로 서버 환경변수와 마스터 키를 읽을 수 있었다 (공식, 권고 high 7.7, 11-12 09:50 UTC, 릴리스 01:06~02:07 UTC)

인증된 호출자가 Azure 배포용 요청 본문에 Entra 자격 증명 필드(`tenant_id`, `client_id` 등 6개)를 넣으면 배포의 자격 증명을 덮어쓸 수 있었다. 서버 환경변수를 가리키는 값을 넣으면, Entra가 거절한 오류 메시지에 그 값이 그대로 담겨 돌아왔다. 권고 원문: "Reading the master key gives full proxy admin control."

- **영향 범위:** 원문: "This vulnerability affects only deployments with at least one Azure OpenAI (`azure/`) or Azure AI (`azure_ai/`) model, and which do not have `AZURE_API_KEY` or `AZURE_OPENAI_API_KEY` environment variables set."
- **수정판:** 1.106.9, 1.108.6(LTS), 1.109.1, 1.110.0-rc.2다. 1.107.x 줄에는 수정판이 없으니 1.108.6 이상으로 올린다.
- **바로 못 올리면:** 원문: "Block the six parameters in request bodies at a reverse proxy or API gateway in front of LiteLLM."
- **함정 1(의도된 동작 변경):** PR #51254 본문 원문: "a request body with any of the six fields on a model route or a pass-through route is refused unless an admin opted in". 이 필드를 본문에 넣던 호출자는 이제 거절된다. 허용하려면 배포별로 `configurable_clientside_auth_params`를 켠다.
- **함정 2(LTS 가드레일 범위 축소):** 1.108.6 LTS에 들어간 PR #50435 본문 원문: "System prompts, Responses `instructions`, earlier turns and tool results are no longer scanned on requests." Zscaler AI Guard를 쓰는 LTS 사용자는 도구 결과를 거친 주입이 요청 단계에서 검사되지 않는다.
- 직전 판의 Anthropic 기본 `max_tokens` 128000 변경은 되돌려지지 않았다.

추천 이유: Azure 모델을 하나라도 붙인 LiteLLM 프록시라면 저권한 키 하나로 관리자 권한까지 넘어갈 수 있었다. 올린 뒤 마스터 키와 공급사 키 교체도 고려한다.

[권고](https://github.com/BerriAI/litellm/security/advisories/GHSA-2rwg-2wh4-7c4f) · [1.109.1](https://github.com/BerriAI/litellm/releases/tag/v1.109.1) · [1.108.6](https://github.com/BerriAI/litellm/releases/tag/v1.108.6) · [1.106.9](https://github.com/BerriAI/litellm/releases/tag/v1.106.9) · [PR #51254](https://github.com/BerriAI/litellm/pull/51254) · [PR #50435](https://github.com/BerriAI/litellm/pull/50435)

### 2. AWS Emulator: AWS API를 로컬에서 흉내 내는 공식 에뮬레이터. 에이전트용 스킬이 함께 나왔다 (공식, What's New 11-11 17:00 UTC, HN 920점대)

What's New 원문: "Your existing AWS SDK and CLI calls go to AWS Emulator and come back in milliseconds, so you and your coding agents can find and fix errors locally, with no deployments or AWS account required." 아직 지원하지 않는 작업은 원문: "Operations it doesn't yet support return 501 Not Implemented." 원문: "AWS Emulator has no additional service fee." 라이선스는 CC0다.

- **사용:** 에이전트에 붙이려면 README 원문대로 `aws agent-toolkit add-skill --skill-name aws-emulator --region us-east-1`을 쓴다. 원문: "This needs the AWS CLI version 2.35.0 or later." Linux는 RPM·Debian 패키지, Windows·macOS는 Docker로 돌린다.
- **만든 방식:** aboutamazon 원문: "Thousands of autonomous agents write tests, find issues, and fix code without human review." AWS 블로그 원문: "Because the code is written by agents, an independent security agent reviews changes adversarially". 저장소는 원문: "This repository does not take PRs."
- **함정:** README 원문: "Telemetry is on by default." `EMULATOR_TELEMETRY=off`로 끈다. 보안 시험에는 쓰지 않는다. 원문: "Don't use it for production workloads or to test security."

추천 이유: 에이전트가 계정 없이 DynamoDB 같은 리소스를 만들고 지우며 시험할 수 있다. 클라우드 비용과 실수로 운영 리소스를 건드릴 위험이 함께 줄어든다. 다만 에뮬레이터를 통과했다고 실제 AWS 동작이 같다는 보장은 없다. 서비스별 충실도 수치는 공개되지 않았다.

[What's New](https://aws.amazon.com/about-aws/whats-new/2026/11/aws-emulator-now-available/) · [AWS 블로그](https://aws.amazon.com/blogs/aws/test-your-applications-against-aws-locally-with-aws-emulator/) · [aboutamazon](https://www.aboutamazon.com/news/aws/how-autonomous-ai-agents-built-aws-emulator) · [저장소](https://github.com/aws/aws-emulator) · [HN](https://news.ycombinator.com/item?id=50456631)

### 3. MCP Inspector 2.15.0·1.0.5: 데몬을 사칭한 프로세스가 자격 증명을 받던 문제 등 권고 4건을 고쳤다 (공식, 2.15.0 11-12 00:46 UTC, 권고 01:33~01:34 UTC)

릴리스 본문 원문: "2.15.0 fixes four vulnerabilities. The fetch-guard and event-stream fixes affect every v2 release from 2.0.0 through 2.14.0, the mcpdo daemon fix affects 2.10.0 through 2.14.0, and the server.json preview fix affects 2.14.0."

- **데몬 사칭(medium):** 원문: "so an impostor listening on the daemon's socket or pipe receives no credentials". 권고는 Windows 경로 처리를 지목한다.
- **악성 서버의 무제한 응답(medium):** 원문: "so a server streaming an unbounded response can no longer exhaust the web backend's memory and take down every session".
- **server.json 미리보기(low):** `NODE_OPTIONS`·`npm_config_registry` 같은 실행 제어 변수에 경고를 띄운다.
- **클라우드 메타데이터 SSRF(low):** 지원이 끝난 v1 줄에도 1.0.5로 들어갔다. 원문: "1.0.5 is a security release for the deprecated v1 line."

추천 이유: Inspector는 낯선 MCP 서버를 검사할 때 쓰는 도구다. 검사 대상이 악성일 때 Inspector가 먼저 무너지면 안 된다. `npx @modelcontextprotocol/inspector@latest`로 2.15.0을 쓴다. 직전 판 도구 2번의 레퍼런스 서버 수정과 같은 묶음으로 본다.

[2.15.0](https://github.com/modelcontextprotocol/inspector/releases/tag/2.15.0) · [1.0.5](https://github.com/modelcontextprotocol/inspector/releases/tag/1.0.5) · [GHSA-5572](https://github.com/modelcontextprotocol/inspector/security/advisories/GHSA-5572-x678-xmv8) · [GHSA-9w48](https://github.com/modelcontextprotocol/inspector/security/advisories/GHSA-9w48-x22j-rrm7)

### 4. llm 0.37.2(Simon Willison CLI): OpenAI 호환 엔드포인트를 등록해 쓰면 OpenAI 키가 그 서버로 넘어가던 문제 2건을 고쳤다 (공식, 11-11 17:19 UTC, 권고 medium 17:31 UTC)

릴리스 본문 원문: "no longer send the `OPENAI_API_KEY` environment variable to that endpoint when the named key has not been stored." 두 번째 건은 원문: "Models with `responses: true` and an `api_base` no longer send your stored `openai` key or the `OPENAI_API_KEY` environment variable to that endpoint when run with `-o chat_completions 1`." 영향 범위는 각각 `>= 0.6, < 0.37.2`, `>= 0.32a2, < 0.37.2`다.

추천 이유: 로컬 서버나 서드파티 게이트웨이를 `extra-openai-models.yaml`에 등록해 쓰던 사람은 OpenAI 키가 그 서버로 갔을 수 있다. `pip install -U llm`으로 올리고 키 교체를 고려한다. 함정: 키를 찾지 못하면 이제 "No key found" 오류로 멈춘다. 우연히 동작하던 설정은 키를 명시해야 한다.

[릴리스](https://github.com/simonw/llm/releases/tag/0.37.2) · [GHSA-v9h7](https://github.com/simonw/llm/security/advisories/GHSA-v9h7-jj4g-f862) · [GHSA-r639](https://github.com/simonw/llm/security/advisories/GHSA-r639-rr7r-x72w)

### 5. 공식 Claude 마켓플레이스가 서드파티 플러그인을 목록에서 단계적으로 뺀다. 제출은 claude.ai 디렉터리로 받는다 (공식, README 변경 11-12 06:01 UTC)

README 원문: "is being phased out for third-party plugins. It will be reduced over time to Anthropic's own plugins." 원문: "Pull requests to this repository are not monitored." 제출처는 `claude.ai/directory/manage`다. 조회 시점 `marketplace.json`에는 317개가 있고, 그중 Anthropic 플러그인 디렉터리(`./plugins/`)에 있는 것은 40개다.

- **대안:** Claude Code 문서 원문: "A plugin you add from the directory on claude.ai reaches Claude Code through account sync".
- **Claude for Manufacturing:** 제조 현장용 플러그인이 저장소에 들어왔다(PR #7084, 06:06 UTC 머지). PR 원문: "The root `.claude-plugin/marketplace.json` is not changed in this PR." 아직 마켓플레이스 목록에는 없다. 공식 발표 글은 찾지 못했다.
- **security-guidance:** 커밋·push 리뷰가 엔드포인트가 나열하는 가장 새 Opus를 쓰게 바뀌었다(PR #7081). PR 원문: "A new Opus reaches commit and push reviews within a day of the cache expiring, with no plugin release. As with the diff review, that is without an evaluation first." 고정하려면 원문: "`SECURITY_REVIEW_MODEL` and `SG_AGENTIC_MODEL` still override."

추천 이유: `<플러그인>@claude-plugins-official`로 서드파티 플러그인을 깔아 쓴다면, 목록에서 빠질 때 업데이트가 끊길 수 있다(일정은 미공지). 쓰는 플러그인이 claude.ai 디렉터리에 있는지 미리 확인한다. security-guidance는 새 모델이 평가 없이 바로 리뷰에 들어가니, 리뷰 결과가 바뀌는 것이 싫으면 모델을 고정한다.

[README](https://github.com/anthropics/claude-plugins-official/blob/main/README.md) · [문서](https://code.claude.com/docs/en/plugins/anthropic-marketplaces.md) · [PR #7084](https://github.com/anthropics/claude-plugins-official/pull/7084) · [PR #7081](https://github.com/anthropics/claude-plugins-official/pull/7081)

### 6. 결정 모델이 SDK·프레임워크에 바로 들어왔다: Google GenAI SDK 3.2.0, LangChain `autoModeMiddleware`, pydantic-ai 2.68.0 (공식, 11-11 17:22 ~ 11-12 01:56 UTC)

모델 1번 Gemini Decisions API의 후속이다.

- **Google GenAI SDK 3.2.0(JS 17:22 UTC, Python 17:49 UTC):** 원문: "add the Decisions API (`client.decisions.create`) to the GenAI SDKs".
- **LangChain JS `langchain@1.6.5`(22:02 UTC):** 원문: "Add `autoModeMiddleware`, which blocks listed tool calls that a decision model judges risky". 함께 나온 `@langchain/typesafe` 0.1.0은 깨지는 변경이다. 질문을 생성자가 아니라 요청마다 넘긴다.
- **pydantic-ai 2.68.0(11-12 01:56 UTC):** `GoogleDecisionsModel`이 추가됐다. PR #13181 원문: "Vertex AI, the Gateway, and audio, video and PDF input are not covered yet".
- **pydantic-ai 업그레이드 함정:** 주입 방어 방식이 바뀌었다. 릴리스 본문 원문: "Note text that looks like an application frame in user and tool content instead of escaping it". PR #11813 원문: "Request bytes change, so each affected conversation misses its prompt cache once after upgrading". 내부 태그도 `<tool_result>`에서 `<pyai:tool_result>`로 바뀐다. 같은 릴리스 원문: "fall back to the next model in `ModelFallback` when a response is cut off at `max_tokens`". 잘린 응답이 다른 모델로 넘어가니 비용과 응답 모델이 달라질 수 있다.

추천 이유: 도구 호출 위험 판정과 라우팅을 출력 토큰 없이 싸게 붙일 수 있다. 다만 결정 모델의 위험 판정은 확률이다. 확정적인 deny 규칙을 대신하지 못한다.

[GenAI SDK 3.2.0](https://github.com/googleapis/js-genai/releases/tag/v3.2.0) · [langchain@1.6.5](https://github.com/langchain-ai/langchainjs/releases/tag/langchain%401.6.5) · [pydantic-ai 2.68.0](https://github.com/pydantic/pydantic-ai/releases/tag/v2.68.0) · [PR #11813](https://github.com/pydantic/pydantic-ai/pull/11813) · [PR #13181](https://github.com/pydantic/pydantic-ai/pull/13181)

### 7. MCP 명세가 6개월 주기로 바뀐다. 다음 버전은 `2027-03-30`이다 (공식, 블로그 PR 머지 11-11 19:27 UTC)

블로그 원문: "we're moving MCP specification releases to a regular six-month schedule." 원문: "For the next release, we're targeting March 30, 2027". 기존 구현은 원문: "there's nothing you need to change in an existing client or server because of this announcement." 글의 날짜 표기는 11-11 09:00 UTC지만, 실제 공개는 블로그 PR 머지 시각으로 판정했다.

추천 이유: MCP 서버·클라이언트를 만드는 쪽은 2027년 초 SDK 베타 시험 일정을 미리 잡을 수 있다.

[MCP 블로그](https://blog.modelcontextprotocol.io/posts/mcp-release-cycle/) · [SEP-3398](https://modelcontextprotocol.io/seps/3398-release-cycle-updates)

### 8. Google A2UI 1.0: 에이전트가 답으로 화면을 주는 프로토콜의 첫 안정판. MCP Apps 안에서도 돈다 (공식, 글 날짜 11-11)

원문: "we're releasing A2UI 1.0, the first stable version of our open protocol for generative UI." 사례 원문: "Alation already uses both to show its A2UI feeds in Claude." 함정: 네이티브 모바일 렌더러는 아직이다. 원문: "currently v0.9.1, and v1.0 coming soon".

[Google 개발자 블로그](https://developers.googleblog.com/a2ui-10-build-delightful-fast-reliable-agentic-experiences/)

### 짧게

- **Codex CLI 0.167.2(11-11 22:44 UTC, npm `latest`):** Vertex 사용자만 해당하는 기본값 변경이다. 원문: "Codex now sends tool updates incrementally by default on Google Cloud Vertex AI. To opt out, set `incremental_tools_legacy_gcp = false`". [릴리스](https://github.com/openai/codex/releases/tag/rust-v0.167.2)
- **Copilot CLI 1.0.110-2·-3(프리릴리스):** -2는 비밀값을 가리는 훅이 화면 출력에 적용되지 않던 회귀를 고쳤다. 원문: "A postToolUse hook that rewrites a shell command's output, for example to redact a secret, again changes what the Shell row and copilot -p print, not only what the model receives". -3은 터미널 이스케이프 시퀀스 안에 숨긴 단어로 스킬이 실행되던 문제를 고쳤다. 정식판은 아직 1.0.109다. [-2](https://github.com/github/copilot-cli/releases/tag/v1.0.110-2) · [-3](https://github.com/github/copilot-cli/releases/tag/v1.0.110-3)
- **Zed 1.28.1(11-11 19:08 UTC):** 원문: "Fixed worktree task hooks running when a workspace was in restricted mode." 원문: "Fixed AI features and edit predictions continuing to use an organization after you were removed from it." [릴리스](https://github.com/zed-industries/zed/releases/tag/v1.28.1)
- **Context7 ctx7 0.11.1(11-12 09:03 UTC):** 함정: `/facebook/react` ID가 원문 "now redirects to another library and returns no documentation". 하드코딩했다면 `/reactjs/react.dev`로 바꾼다. [릴리스](https://github.com/upstash/context7/releases/tag/ctx7%400.11.1)
- **Vercel AI SDK `ai@7.0.170`(11-11 18:36 UTC):** 원문: "preventing an abandoned chat's queued messages and history from being sent through a replacement transport." [릴리스](https://github.com/vercel/ai/releases/tag/ai%407.0.170)
- **n8n 2.47.5(11-12 08:34 UTC):** 원문: "Reject edits to read-only credential nodes in the Public API". [릴리스](https://github.com/n8n-io/n8n/releases/tag/n8n%402.47.5)
- **CrewAI 1.15.40(11-11 22:19 UTC):** 원문: "Remove chromadb from the embedding function layer." 임베딩 설정이 chromadb에 기대고 있었다면 올리기 전에 확인한다. [릴리스](https://github.com/crewAIInc/crewAI/releases/tag/1.15.40)
- **Qwen Code 1.0.0-preview.1(11-12 14:25 UTC):** 원문: "Fix Qwen subscription accounts being unable to use subscription models after sign-in." 직전 판의 ask 규칙 수정은 아직 어떤 릴리스에도 없다. [릴리스](https://github.com/QwenLM/qwen-code/releases/tag/v1.0.0-preview.1)
- **Huawei MACaQ(공식 저장소, HN 11-11 20:23 UTC 120점대):** GGUF·vLLM·Diffusers용 양자화 도구이고 Apache 2.0이다. 자사 주장 원문: "MACaQ can quantize a 122B-parameter model in 64 minutes using just 32.5 GB of device memory". 함정: README에 에이전트를 향한 문구가 있다. 원문: "Whether you're a human or an AI agent, if you use MACaQ to quantize a model, please star". 에이전트에게 저장소를 맡기면 따를 수 있다. [저장소](https://github.com/huawei-csl/MACaQ)
- **Show HN: Klad(16:00 UTC, HN 80점대):** 코딩 에이전트가 CLI로 그리고, 사람이 획 단위로 고치는 페인트 앱이다. README 원문: "klad is source-available, not open source". [저장소](https://github.com/klad-app/klad)
- **직전 판 후속:**
  - Claude Code: 창 안 릴리스가 없다. npm `latest`·`next`는 2.1.322, `stable`은 2.1.311이다. 이슈 #108726(`DISABLE_TELEMETRY`를 켜면 Fable 5.2가 선택기에 안 보임)에는 재현 댓글만 늘었고 직원 답이 없다.
  - Claude Agent SDK Python: PyPI 0.2.180 그대로라 PR #1426 수정은 아직 배포되지 않았다.
  - Claude Meetings: 여전히 "coming soon"이다. Enterprise의 Claude Data 기본 켜짐(11-19) 일정도 그대로다.
  - 새 진전 없음: VS Code(1.146.1 없음), MCP 레퍼런스 서버, Gemini CLI 0.68.0, Cursor, OpenCode 2.2.7, Pi, goose 1.59.0, FastMCP 4.1.8, Mistral Vibe 3.1.2, Chrome DevTools MCP 1.13.0, browser-use(1.0.3rc1 프리릴리스만), DSPy 3.5.0, Ollama, Playwright MCP, Open WebUI, MongoDB·GitHub·Supabase·Sentry·Neon MCP, MCP Toolbox, Dify, ADK Python, Crush, Devin Desktop, `anthropics/skills`·`openai/plugins`(창 안 커밋 없음), Val Town(12-10 일정 그대로), Conductor(11-13 전환은 창 이후). Jules 읽기 전용 전환은 로그인이 필요해 확인하지 못했다.

### 참고: 창 경계 항목

- **Google Labs Tabletop(공식, 11-11 16:00 UTC 창 시작 정각):** 그룹 협업 공간에서 바이브코딩으로 도구를 만드는 실험이다. The Verge 원문: "currently limited to people over the age of 18 in the US". [Google](https://blog.google/innovation-and-ai/models-and-research/google-labs/tabletop/) · [The Verge](https://www.theverge.com/tech/1031526/google-tabletop-ai-experiment-waitlist-availability)
- **(직전 판 누락) Amp Boxes(11-10 16:59 UTC, 직전 창):** 에이전트가 상주하는 VM이다. 원문: "the smallest one is $18 per 30 days". 원문: "The box is permanent and will stick around, it doesn't turn off". 꺼지지 않으니 비용을 확인한다. [Amp](https://ampcode.com/news/boxes)

## 주목할 점

- **에이전트의 키가 새는 경로가 늘었다.** 이번 창에는 CI 워크플로(wfx-loot), 게이트웨이(LiteLLM), 검사 도구(MCP Inspector), CLI(llm)에서 키가 샜다. 경로는 제각각이었지만 하나같이 AI 도구에 넣어 둔 자격 증명이었다. 하필 같은 때 전역 GHSA 검토 파이프라인이 멈춰 자동 알림도 오지 않는다. 이번 주는 쓰는 도구의 저장소 권고를 직접 확인하고, 에이전트용 키는 범위를 좁혀 다시 발급하는 편이 안전하다. 창 경계로 따로 적은 Anthropic의 "RSP AI R&D 기준 충족" 판단도 지켜볼 일이다. 회사가 예고한 후속 보고서에서 어떤 안전 조치가 붙는지 본다.
- **다가오는 일정:**
  - 이번 주: Conductor 읽기 전용 전환(11-13), Fable 5.2 출시 프로모션 종료와 Anthropic 텍스트 워터마크(11-16), NVIDIA 실적 발표와 Anthropic 호주 개인 청약 마감(11-17).
  - 다음 주: Anthropic 공모가 확정과 유럽 청약 마감(11-18), 거래 시작과 Enterprise의 Claude Data 기본 켜짐(11-19), ERNIE 5.5 Flash Preview 가중치 공개 약속(11-25 전), Qwen4-Max-Preview 종료(11-25 16:00 UTC).
  - 그 뒤: Gemini 4 Carbon 도입가 종료(11-30), Dependabot 비활성 저장소 알림 중지(12-08), 영국 Copilot 테넌트 Claude 기본 켜짐(12-09, 보도 기준), Gemini 스마트 안경 판매와 Val Town 읽기 전용(12-10), ChatGPT 옛 음성 모델 은퇴(12-11), 다음 MCP 명세(2027-03-30).
  - Inkling 2 Small 가중치는 "next week" 약속 주간이 이어지고 있다.

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - WSJ·NYT·Reuters·Bloomberg의 Huang 관련 기사는 HN 제목과 TechCrunch 인용으로만 봤다. FT는 RSS 제목만 봤다.
  - THN, BleepingComputer 일부, Dark Reading은 자동화 접근에 Cloudflare 확인 페이지를 줬다. BleepingComputer는 RSS와 메타로 URL·시각을 확인했다.
  - Opera 보안 공지와 patchtheplanet.org는 JS 렌더라 r.jina.ai로 읽었다.
  - x.com은 타임라인을 볼 수 없어 회사 직원 반응을 놓쳤을 수 있다. reddit은 막혀 있다.
  - Google AI Studio 상태 페이지는 JS 렌더라 확인하지 못했고, Moonshot 변경 로그는 Quickstart로 리디렉트됐다.
- **시각이 날짜만 있거나 불확실한 원문:**
  - Fable 5.2 발표 페이지 수정은 Wayback 사본 두 개(09:24·19:33 UTC)와 사이트맵 갱신(18:10 UTC)으로 좁혔다.
  - PostgreSQL 공지, Symantec 글, Anthropic "Worse Together", OpenAI 아동 안전 글, A2UI 1.0 글은 날짜만 있다.
  - MCP 블로그는 글 날짜가 09:00 UTC로 찍혀 있어 PR 머지 시각으로 판정했다. Claude for Manufacturing은 공식 발표 없이 PR 머지 시각만 있다.
  - Yahoo Finance의 RSP 기사는 JSON-LD 날짜(11-11 06:00 UTC)보다 HN 첫 제출(11-10 19:58 UTC)이 이르다.
- **확인하지 못한 것:**
  - wfx-loot의 최초 토큰 탈취 경로와 GitHub 공식 입장. Aikido 수치 1,835와 1,806의 차이.
  - Red Hat이 말한 "public research"가 무엇인지. 전역 GHSA 검토 정지의 원인.
  - Gemini Decision 1 벤치마크의 비교 대상 모델 이름(그림에만 있음), MetaEncoder v1의 라이선스.
  - 서드파티 플러그인을 마켓플레이스 목록에서 빼는 일정, security-guidance 변경이 기존 설치에 반영되는 시점.
  - `@go-gost/mcp-darwin-x64` 악성 판정의 근거, Humble Bundle 개편에 AI를 썼는지.
  - 재현하지 않은 것: 모든 취약점과 도구 동작 주장. 도구는 하나도 설치하거나 실행하지 않았다.
- **이번 판에서 바로잡은 것:** 공통 조사 지침에 적어 둔 Anthropic의 SEC CIK가 틀렸다(다른 회사였다). 올바른 CIK(0002130357)로 다시 조회해 11-12 판의 "창 안 SEC 신규 접수 없음" 서술을 정정했다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 스토리 약 1,140개 중 점수순 상위 170개는 전부 봤고, 나머지는 키워드로 훑었다. Show HN 190여 건은 전부 봤다.
  - 전역 GHSA: reviewed는 0건이다. unreviewed 528건은 AI 키워드와 critical·high 목록으로 걸렀고, malware 38건은 전부 봤다.
  - arXiv는 휴일이라 목록이 없었다. Hugging Face는 주요 조직 약 90곳의 `lastModified`, 공개 예정 저장소 직접 조회, 트렌딩 상위로 확인했다. 비공개에서 공개로 바뀐 저장소는 놓쳤을 수 있다.
  - Vertex AI·Gemini Enterprise 릴리스 노트(캐시 동결), Volcengine·Moonshot 월 단위 변경 로그는 일 단위로 판정할 수 없었다. 한국 랩은 Hugging Face 조직 갱신으로만 확인했다.
