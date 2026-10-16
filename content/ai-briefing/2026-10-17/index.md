---
title: "2026-10-17 AI 브리핑"
date: 2026-10-17T01:00:00+09:00
tags: [ai-briefing, meta, open-weights, mcp, security]
description: "Meta가 816B Muse Spark Open을, Reflection AI가 Apache 2.0 501B Beam을 하루 사이 오픈 웨이트로 냈고, Goodfire는 다의성의 원인이 superposition이 아니라는 해석가능성 반론을 올렸으며, 코딩 에이전트 3종에서 리포에 든 MCP 설정을 자동 실행하는 같은 유형의 CVE가 나왔다."
---

> 조사 범위: 2026-10-16 01:00 ~ 2026-10-17 01:00 KST(2026-10-15 16:00 ~ 10-16 16:00 UTC, 목요일 밤~금요일 미국 오전). 직전 브리핑의 조사 창 종료 시각부터 잡았다. 시각은 기사 `article:published_time`·`datePublished`, RSS `pubDate`, GitHub `published_at`·`created_at`, 전역 GHSA `published_at`, HN Algolia `created_at`, OpenRouter `created`, HF `lastModified`, 상태 페이지로 검증했다. HN 점수는 조회 시점(10-16 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **미국발 대형 오픈 웨이트가 하루 사이 둘 나왔다.** Meta Muse Spark Open(816B-A42B, 1M 컨텍스트, 제한 라이선스, Mac Studio 2대 타깃)과 Reflection AI Beam(501B-A23B, Apache 2.0, SWE-Bench Verified 81.6%)이다. Microsoft AI도 2.8T MAI-Base-2 사전학습 완료를 알렸다.
- **Goodfire의 Nick Cammarata가 "다의성의 원인은 superposition이 아니었다"는 에세이로 해석가능성 정설에 반론을 냈다(HN 769점).** 아키텍처 개선만으로 마지막 층 뉴런 99~100%가 읽힌다는 주장이다. Scion 논란은 Anthropic이 원문에 Mononoke·Bonsai 각주를 달며 수습됐다.
- **코딩 에이전트 3종(JetBrains Junie, Z.ai ZCode, DeepSeek Harness)에서 "리포에 든 MCP 설정 자동 실행" CVE가 같은 날 나왔고, MCP Python SDK의 세션 미해제 메모리 고갈 권고도 올라왔다.** 도구 쪽은 Opus 5.5 Fast mode 정식 전환, Claude Code 2.1.301, Codex CLI 0.163.0이 핵심이다.

## 모델 소식

### 1. Meta Muse Spark Open — 816B-A42B 오픈 웨이트, 1M 컨텍스트, Mac Studio를 1급 타깃으로 (HN 885점)

Meta가 "미국 최강 오픈 웨이트 모델"이라며 Muse Spark Open의 가중치를 공개했다. 총 816B·활성 42B MoE에 1M 토큰 컨텍스트, 텍스트·이미지·비디오·오디오 입력을 받는다.

- **Apple 실리콘 협업:** M5 Ultra Mac Studio(통합 메모리 512GB) 2대에서 돌도록 MLX에 최적화했다. Apple ML Research는 4대 클러스터에서 36 tok/s 이상(8K 컨텍스트)을 냈다고 썼다(도구 섹션 6번). SGLang용 Helion GPU 커널로 NVIDIA·AMD에서 커널 수준 1.03~3.95배 속도를 올렸다.
- **배포:** Baseten·Fireworks·Groq·Modal·Nebius·OpenRouter·Together에서 API로 쓸 수 있고, 가중치는 HF·SageMaker·Vertex Model Garden·Microsoft Foundry에 있다. OpenRouter 가격은 입력 $1.25/M, 출력 $4.25/M이다. transformers 5.20.0·mlx-lm 0.32.0·Pydantic AI·Cline이 같은 날 반영했다.
- **벤치마크(Meta 자료, The Decoder 요약):** JobBench·MCP-Atlas·HLE-Diamond·ZeroBench 등 에이전트·추론·멀티모달 범주에서 Qwen 3.8·GLM 5.3·Kimi K3를 앞서지만 코딩 벤치마크 일부는 뒤지고, Meta의 유료 Muse Wonder Full에는 크게 못 미친다.
- **라이선스:** "Muse Model License"로 "Built with Muse Spark Open" 표기가 의무이고, 연매출 2,000만 달러 초과 API 제공자는 별도 계약이 필요하다. HF 리포는 gated다. nvidia의 DFlash2 변형, mlx-community 3·4bit, GGUF 파생본이 바로 올라왔다.

Llama 이후 Meta의 오픈 웨이트 복귀이며, 오픈 웨이트 상위권이 중국 모델 일색이던 구도에 미국 모델이 들어왔다. 처음부터 데스크톱 추론을 겨냥한 점이 다르다.

[Meta 발표](https://research.meta.ai/blog/muse-spark-open) · [HF 모델](https://huggingface.co/meta-models/Muse-Spark-Open-816B-A42B) · [HF 블로그](https://huggingface.co/blog/meta-muse-spark-open) · [TechCrunch](https://techcrunch.com/2026/10/15/meta-launches-an-open-weight-model-that-runs-on-macs/) · [The Decoder](https://the-decoder.com/muse-spark-open-is-metas-answer-to-qwen-glm-and-kimi-in-the-open-weight-race/) · [HN 50107379](https://news.ycombinator.com/item?id=50107379) (Meta 글은 날짜 10-15만 표기. HF 리포 10-15 15:46 UTC, OpenRouter 등록 17:57 UTC, HN 17:47 UTC, TechCrunch 20:04 UTC. 공식·매체보도)

### 2. Reflection AI Beam 1.0 — 501B-A23B, Apache 2.0, 1M 컨텍스트, SWE-Bench Verified 81.6%

신생 미국 랩 Reflection AI가 첫 오픈 웨이트 모델 Beam을 냈다. 총 501B·활성 23B MoE(52층, 레이어당 256 전문가 중 8개 라우팅), 텍스트 전용, 1M 컨텍스트, Apache 2.0이다.

- **공개 범위:** BF16·FP8·NVFP4 체크포인트를 동시에 냈고, transformers 5.20.1·vLLM·SGLang이 day-0로 지원한다. Rust 기반 자체 하네스 Mirror CLI, 턴 간 추론 보존(preserve_thinking), 전용 툴콜 문법이 따라온다. HF는 TRL SFT·GRPO 레시피 글을 함께 냈고, Ollama는 한시 무료로 제공한다.
- **모델 카드 수치(BF16):** SWE-Bench Verified 81.6%, SWE-Bench Pro v1 66.1%, Terminal Bench 2.1 80.1%, AIME 2026 97.7%, GPQA Diamond 90.6%, HLE 36.0%(도구 없음)·51.1%(도구), LiveCodeBench 88.3%, MCP Atlas 80.8%, BrowseComp 76.8%. NVFP4도 1~2p 이내다.
- **가격:** OpenRouter 입력 $0.25/M, 출력 $1/M.

같은 날 나온 Muse Spark Open이 제한 라이선스인 데 비해 완전 Apache 2.0이고 활성 파라미터가 절반이다. 미국발 대형 오픈 웨이트가 하루 사이 둘 나왔다.

[Reflection 발표](https://reflection.ai/blog/beam-is-available) · [HF 블로그](https://huggingface.co/blog/reflection-beam) · [HF 모델](https://huggingface.co/reflection/Beam-1.0-501B-A23B) · [Ollama](https://ollama.com/blog/reflection-beam) · [HN 50118552](https://news.ycombinator.com/item?id=50118552) (발표 글은 날짜 10-16만 표기. HF 리포 10-16 12:38~13:22 UTC, transformers 13:02 UTC, HN 13:34 UTC, OpenRouter 14:21 UTC. 공식)

### 3. Microsoft AI MAI-Base-2 — 2.8T·활성 80B 사전학습 완료, "DeepSeek-V4-Pro-Base보다 FLOPs 효율 4~12배"

Microsoft AI가 Lab Notes 글에서 MoE 베이스 모델 MAI-Base-2의 사전학습을 마쳤다고 밝혔다. GB200 1만 6천 장으로 2026년 8~9월 38일간 학습했고 평균 MFU 21%, goodput 89%였다. 자체 held-out 평가(bits-per-byte)에서 테스트한 모든 오픈 웨이트 모델(DeepSeek-V4-Pro-Base, Kimi K3, Qwen3.8-2.4T-A95B, MiMo-V2.6-Pro-RL)보다 낮은 BPB를 냈고, 가장 강한 오픈 베이스로 꼽은 DeepSeek-V4-Pro-Base 대비 사전학습 FLOPs 효율이 4~12배라고 주장한다. 기법으로는 반복 데이터를 반영한 "source scaling" 스케일링 사다리, Muon 옵티마이저 개선과 활성값 그래디언트 클리핑, 비전 데이터 통합을 들었다. "다른 모델에서 증류하지 않은 유기적 데이터"를 강조했고, 비교 대상 일부가 post-trained 체크포인트라는 주의는 스스로 달았다. 가중치는 Foundry를 통해 파트너에 라이선스할 계획이며 지금은 post-training 중이다.

Microsoft가 OpenAI 의존을 벗어나 자체 프런티어급 사전학습을 했다는 첫 구체 수치다. 모델 공개는 아니다.

[Microsoft AI](https://microsoft.ai/news/frontier-pre-training-at-microsoft-ai/) · [HN 50105791](https://news.ycombinator.com/item?id=50105791) (10-15 16:09 UTC, 공식)

### 4. Krea 3 — "Nano Banana Pro보다 4배 빠른" 편집 특화 이미지 모델, 장당 $0.06 (HN 336점)

Krea가 프로덕션 편집용 이미지 모델 Krea 3을 공개했다. 지시한 부분만 바꾸는 편집을 8초 안에 처리하고 장당 $0.06이다. 페이지는 Nano Banana Pro 대비 4배, GPT Image 2 대비 10배 빠르고 3배 싸다고 주장한다. fal·Baseten·Comfy·Together AI·Civitai로 배포된다. 비교표에는 Grok Imagine 2.0, Nano Banana 2, Seedream 5.0 Pro, Ideogram 4.5가 있다(수치는 JS 렌더라 추출하지 못했다). 전날 FLUX 3 Image Klein에 이어 이미지 편집 모델의 가격·속도 경쟁이 한 주에 두 번 갱신됐다.

[Krea 3](https://www.krea.ai/krea-3) · [HN 50105648](https://news.ycombinator.com/item?id=50105648) (페이지에 날짜 없음, HN 10-15 16:04 UTC. 공식 자체 주장, 벤치마크 수치는 미확인)

### 5. 새 진전: Gemini 4 Argon — ARC-AGI-3 97.5%, 하네스를 바꾸면 49.8%

The Decoder가 ARC Prize·Epoch AI 자료를 종합했다. Gemini 4 Argon은 추론 보존·컨텍스트 압축 같은 벤더 기능을 허용하는 "Provider Adapter Harness"에서 ARC-AGI-3 97.5%($4,800)를 기록해 OpenAI(GPT-6 Astra 99.9%)에 이어 95%를 넘은 두 번째 랩이 됐다. 25개 공개 게임에서 인간 기준선보다 62% 적은 행동 수를 썼다. 반면 라운드 사이에 간단한 메모만 허용하는 "Standard Harness"에서는 low reasoning 기준 49.8%($5,800)에 그쳤다. ARC-AGI-2 91.3%($0.32/task), ARC-AGI-1 98.5%($0.16/task)이고 가격은 $2/$10 per M이다. Epoch AI는 수학·SWE에서 경쟁사에 뒤진다고 평가했다. 별도 기사는 Google이 Argon을 로봇 제어·3D 훈련 환경 생성·훈련 데이터 평가까지 맡는 단일 모델로 밀고 있지만 데모는 사람 프롬프트 의존이 크다고 썼다.

Argon GA(10-14판)에 이어 외부 수치가 나왔다. 하네스에 따라 점수가 두 배 차이 나는 것이 벤치마크 비교의 핵심 변수임이 드러났다.

[The Decoder(ARC)](https://the-decoder.com/googles-gemini-4-argon-is-closer-to-the-ai-frontier-than-any-google-model-in-months/) · [The Decoder(로보틱스)](https://the-decoder.com/google-wants-gemini-4-argon-to-be-the-only-ai-model-robots-need/) · [StarSkirmish Bench v0.2](https://starskirmish.com/bench/v0-2/) · [HN 50118688](https://news.ycombinator.com/item?id=50118688) (10-16 15:03 · 10-15 16:35 UTC. 매체보도, ARC Prize 원문은 직접 확인하지 못함). 커뮤니티 벤치 StarSkirmish v0.2(LLM이 1시간 안에 StarCraft: Brood War 봇을 C++로 짜서 사람 봇과 대결)에서는 GPT-6 Astra 1위, GPT-6.1 Sol 2위, Argon·Opus 5.5·Fable 5.1이 37~39점으로 동률이었다.

### 6. Anthropic — METR에 "직원 수준" 접근 부여, Opus 5.5 Fast mode 정식화, Accenture 월 $29 상한

- **독립 평가자 업데이트:** METR 팀에 직원과 비슷한 접근 권한을 주기 시작했고 METR은 위험 평가 결과를 공개 발표한다. SecureBio·Accenture도 평가자로 협력 중이고 Hugging Face 등과 초기 논의 중이다. TechCrunch는 연구자 6명이라고 보도했다. [Anthropic](https://www.anthropic.com/news/an-update-on-independent-evaluators) · [TechCrunch](https://techcrunch.com/2026/10/15/anthropic-is-letting-metr-researchers-look-under-the-hood-with-employee-level-access/) (10-15 17:20 · 19:14 UTC, 공식·매체보도)
- **Opus 5.5 Fast mode 정식 전환:** 베타 헤더와 접근 신청 없이 쓸 수 있고 SDK에 `speed` 파라미터가 생겼다. 도구 섹션 1번에 정리했다.
- **Accenture 지출 상한:** 직원 1인당 Claude 지출을 월 $29로 제한했고, 월 $1,000까지 쓰던 직원들이 대응 중이라고 404 Media가 보도했다. [404 Media](https://www.404media.co/accenture-tells-employees-they-can-now-spend-only-29-on-claude-per-month/) (10-15 16:46 UTC, 매체보도)
- **미확인:** Reuters "투자자들이 매출 성장 둔화로 Anthropic IPO 밸류에이션 기대를 낮춘다"(HN 10-16 02:35 UTC), Ars Technica "FTC가 OpenAI·Anthropic 보안 사고의 미국인 피해를 조사"(HN 10-15 19:02 UTC), BBC "Anthropic은 자사 AI가 의식이 있을 수 있다고 본다"는 본문을 읽지 못했다.

### 7. OpenAI — Rosalind Academic Plan, GPT-Live에 Decisions 네이티브 통합, 2시간 반 장애

- **Rosalind Academic Plan:** 자격 있는 학술 연구실에 Business Standard 좌석 최대 30석을 최대 12개월 무상 제공한다. 생명과학 전용 모델 GPT-Rosalind는 trusted-access 승인이 조건이고, 의약화학·유전체·웻랩 도구를 오케스트레이션하는 Rosalind workbench와 결합된다. [OpenAI](https://openai.com/index/introducing-the-rosalind-academic-plan) (RSS 10-15 19:00 UTC, 본문은 403이라 r.jina.ai 경유 확인, 공식)
- **GPT-Live + Decisions:** `gpt-live-1` 세션에서 Decisions API(gpt-6-luna)를 네이티브 위임으로 부른다. `session.delegation`으로 Live가 인식한 요청을 넘기거나 `session.triggers`로 전사 델타 시점에 결정을 돌리고, 활성 세션 중 `session.update`로 질문을 바꿀 수 있다. [Changelog](https://developers.openai.com/api/docs/changelog) · [가이드](https://developers.openai.com/api/docs/guides/decisions-voice) (날짜 10-15만 표기, 공식)
- **인시던트:** "Widespread feature degradation"(major)이 10-15 16:29~18:56 UTC 이어졌다. ChatGPT iOS·Android 로그인·가입 장애가 포함됐고 17:13 완화, 18:56 전면 복구였다. [status.openai.com](https://status.openai.com/incidents/01M506BA4XRSWH7THR53HQGN3Z) (공식)
- **Vercel 데이터:** TypeSafe AI의 결정 모델 Jev가 출시 2주 만에 Vercel 유료 팀의 35%가 쓰는 모델이 됐고 2위 Sonnet 5.5의 약 2배라고 TechCrunch가 보도했다. [TechCrunch](https://techcrunch.com/2026/10/16/jev-is-vercels-fastest-adopted-ai-model-ever/) (10-16 15:33 UTC, 매체보도)
- **미확인:** The Verge "ChatGPT가 Chrome 탭을 볼 수 있다"(10-15 22:43 UTC)는 제목만 확인했다.

### 8. 짧게

- **Liquid AI d1-ultra:** 토큰 생성 없이 1회 forward로 확률을 내는 멀티모달 결정 모델. 입력 $0.042/M. 112개 평가에서 Jev 1.14를 전부 앞서고 Artificial Analysis Decision Index에서 다른 결정 모델보다 8점 이상 높다고 주장한다. Vercel·OpenRouter 제공 예정. [Liquid AI](https://www.liquid.ai/blog/d1-ultra) (날짜 10-15만 표기, HN 10-16 13:26 UTC. 공식 자체 주장)
- **GitHub Copilot 모델 은퇴 예고:** 2026-11-16에 Claude Opus 4.8(연간 개인 구독자는 유지)·Claude Opus 5·GPT-5.6 Terra를 제거한다. 대안은 Opus 5.5와 GPT-6.1 Sol·GPT-6 Luna다. [Changelog](https://github.blog/changelog/2026-10-15-upcoming-deprecation-of-selected-github-copilot-models-in-mid-november) (10-15 19:35 UTC, 공식)
- **IBM Granite Switch 4.1 30B·8B(창 경계):** 베이스에 aLoRA 어댑터(decision, guardian-core)와 470M 음성 인코더를 붙여 한 엔드포인트에서 제어 토큰으로 전환한다. Apache 2.0. [HF 블로그](https://huggingface.co/blog/ibm-granite/granite-switch-4-1) (카드 Release Date 10-14, HF RSS 10-15 20:34 UTC. 공식)
- **Tencent Hy-Memory-30B-A3B(창 경계):** Mem0·MemOS 같은 외부 메모리 프레임워크의 "쓰기 경로" 전용 활성 3B 모델. Apache 2.0, arXiv 2610.22196. [HF](https://huggingface.co/tencent/Hy-Memory-30B-A3B) (리포 생성 10-15 09:52 UTC, 갱신 10-16 08:12 UTC. 공식)
- **Datadog Scout 1.0:** Bits AI용 SRE 특화 모델. [Datadog](https://www.datadoghq.com/blog/ai/introducing-scout-sre-specialized-model-bits-ai/) (날짜 10-15만 표기, HN 10-15 23:18 UTC. 공식)
- **Moondream:** OpenRouter에 `moondream/moondream3.1-9b-a2b`($0.30/$2.50 per M, 32K)가 등록됐다(10-15 22:12 UTC, 커뮤니티 카탈로그).
- **NVIDIA:** `Nemotron-3.5-Rerank-8B`·`Nemotron-3.5-Embed-8B/1B` 카드가 창 안에 갱신됐지만 공개 글은 없다. DeepSeek-V4.1-Flash HF 커밋(10-16 09:27 UTC)은 도구 이미지 순서·현재 턴 thinking 인코딩 수정으로 새 가중치가 아니다.
- **확인했지만 창 안 변화 없음:** OpenAI models·deprecations 페이지, status.claude.com, Google Cloud 상태, Gemini API changelog, Gemini 앱 릴리스 노트, z.ai 릴리스 노트, DeepSeek 뉴스, alizila RSS, Mistral 뉴스, xAI. HF 조직 스캔(openai, google, meta-llama, mistralai, Qwen, moonshotai, zai-org, MiniMaxAI, microsoft, amazon, CohereLabs, ai21labs, stabilityai, xai-org, apple, ByteDance-Seed, allenai)에서 창 안 신규 모델은 없었다.

## 기술 이슈

### 1. Goodfire "Neural Networks Are Already Clean" — "다의성의 원인은 superposition이 아니었다"는 해석가능성 반론 (HN 769점)

Goodfire의 Nick Cammarata가 긴 연구 에세이를 냈다. InceptionV1 같은 초기 모델의 다의성(polysemanticity)은 superposition 때문이 아니라 "아키텍처 문제로 후반 층이 넘치고, 약한 해석 도구가 그걸 증폭한" 평범하고 고칠 수 있는 문제였다고 주장한다. 모델 크기·데이터를 바꾸지 않고 심층 학습을 돕는 통상적 아키텍처 개선만으로 마지막 층 뉴런의 99~100%가 해석 가능해졌고, 300배 크기 범위에서 훈련한 버전 전부가 읽힌다고 썼다. 2020년 Zoom In 논문에서 superposition 사례로 든 "차 특징이 개 뉴런으로 퍼지는 회로"는 숨기는 것이 아니라 일반화·합성이었고 "이후 같은 모양의 회로를 발표한 사람이 없다"고 했다. 언어 모델에서도 Qwen3-32B 예비 감사에서 뉴런 극(pole)의 약 90%가 한 가지 일, 약 7%가 관련된 두 가지 일을 하고 2~4%만 해석되지 않았으며, Kimi K3 같은 수조 파라미터 모델에서도 비슷하다고 한다(동반 논문 예정). 2023년 Towards Monosemanticity(중앙값 0/14)와 OpenAI 자동 해석(1.7%) 수치와 대비했다.

SAE처럼 두 번째 모델로 풀어헤치는 대신 네트워크 자체의 뉴런·회로를 직접 연구하자는 노선 전환 제안이다. Anthropic·OpenAI 해석가능성 정설에 대한 공개 반론이고, 저자가 그 정설을 만든 당사자 중 하나다. The Decoder는 10-16 "연구자들 의견이 갈린다"고 보도했다.

[에세이](https://already-clean.goodfire.com/) · [HN 50105647](https://news.ycombinator.com/item?id=50105647) (목록에 날짜 10-15만 표기, HN 10-15 16:04 UTC 769점·303댓글. 공식 연구 에세이, 동료평가 아님)

### 2. 새 진전: Scion/Mononoke 논란 — Anthropic 원저자가 HN에서 답하고 원문에 Mononoke·Bonsai 각주 추가, 제기자는 "의도적 복제" 주장으로 수위 높임

직전 판에서 "확인 못 한 사항"으로 남긴 Anthropic 대응이 창 안에 나왔다.

- **Anthropic 대응:** 원문 저자 nickg-anthropic이 HN에 답했다(10-15 17:59 UTC). Meta가 외부에 Mononoke 서버를 포함한 전체를 Sapling이라 부르기에 그 관례를 따랐고 각주를 추가하겠다고 했으며, Bonsai 데이터 모델 등에서 영감을 받았고 숨길 의도는 없었다고 했다. "Claude에게 VCS를 만들라고만 한 게 아니라 대형 Git 리포에서 오는 경우에 필요한 새 설계가 많았다", "Anthropic에 ex-Meta 직원이 많고 Mononoke 경험이 북극성이었다"고 덧붙였다. 원문은 "Updated October 15"로 표기됐고 각주 2에 "Sapling의 서버 구성요소 Mononoke가 Scion에서 쓰는 많은 기법, 특히 Bonsai 데이터 모델을 개척했다"는 문장이 들어갔다.
- **제기자 후속 글:** sunshowers는 첫 글에서 "실수로 출처를 뺐을 것"으로 봤지만, 공개 후 "의도적으로 베꼈고 인력 겹침이 있다"는 확인을 비공개로 받았다고 주장했다. Scion changeset 설명이 Mononoke의 Bonsai changeset과 "정확히 같다"며 2022년 Oxide 자료를 나란히 놓았고, "Scion"이 "Bonsai"의 음절을 뒤집은 이름일 수 있다고 했다. 핵심은 복제가 아니라 출처 표기라고 했고, 각주가 추가된 뒤 "충분한 출처 표기가 됐다"는 업데이트를 글 상단에 달았다. "의도적 복제·인력 겹침" 주장은 비공개 제보 기반이라 미확인이다.

[후속 글](https://sunshowers.io/posts/anthropic-mononoke-2/) · [Anthropic 원문(갱신)](https://www.anthropic.com/engineering/redesigning-source-control) · [Nick G. 댓글](https://news.ycombinator.com/item?id=50107559) · [HN 50106738](https://news.ycombinator.com/item?id=50106738) (후속 글은 날짜 10-15만 표기, HN 17:05 UTC 249점. 커뮤니티·공식)

### 3. 코딩 에이전트 3종에서 "리포에 든 MCP 설정 자동 실행" CVE — JetBrains Junie, Z.ai ZCode, DeepSeek Harness

하루 사이 같은 유형의 권고가 셋 나왔다. 리포를 여는 것만으로 코드가 실행되는 클래스다.

- **JetBrains Junie CLI(CVE-2026-111091, CVSS 7.8):** 26.8.3 이전 버전이 프로젝트 안에 선언된 MCP 서버 설정을 그대로 실행해 신뢰하지 않은 리포를 여는 것만으로 코드가 실행됐다. 설명은 CVE 한 줄뿐이다. [GHSA-44gv-r4c4-w64m](https://github.com/advisories/GHSA-44gv-r4c4-w64m) (10-15 18:32 UTC, 공식·unreviewed)
- **Z.ai ZCode(CVE-2026-88755):** v3.11.2.6792가 리포의 워크스페이스 설정에 적힌 MCP 서버에 자동 연결해 조작된 리포로 코드를 실행할 수 있다. [GHSA-2vx4-m2rp-32rx](https://github.com/advisories/GHSA-2vx4-m2rp-32rx)
- **DeepSeek Harness(dsh) 2건:** 신뢰하지 않은 검색 경로로 조작된 실행 파일을 열면 임의 코드 실행(CVE-2026-95182), 컨텍스트 압축이 외부 콘텐츠를 통한 프롬프트 주입에 취약(CVE-2026-95183). [GHSA-g5wx-c65h-w9px](https://github.com/advisories/GHSA-g5wx-c65h-w9px) · [GHSA-8rmg-g7v9-j8x6](https://github.com/advisories/GHSA-8rmg-g7v9-j8x6)

ZCode·dsh 3건은 같은 보고자의 gist 기반으로 10-16 15:38 UTC에 전역 DB에 올라왔고, 심각도 미평가에 벤더 확인·패치 정보가 없다(미확인). Junie는 JetBrains 보안 페이지가 출처다.

### 4. MCP Python SDK — Streamable HTTP 상태 유지 모드에서 세션이 안 지워져 메모리 고갈 (CVE-2026-59951, CVSS 7.5)

기본값인 stateful 모드에서 세션이 만료되지 않고, 클라이언트가 DELETE해도 레지스트리에 남으며, 첫 요청이 거부된 세션도 태스크가 돌아간 채 등록된다. 세션 수 상한이 없어 엔드포인트에 닿는 누구나 짧은 요청만으로 서버 메모리를 키울 수 있고 재시작만이 해법이다. 영향은 mcp 1.8.0~1.29.1과 2.0.0~2.1.1(FastMCP 1.x, 2.x의 `MCPServer`·`Server.streamable_http_app()`, `StreamableHTTPSessionManager`)이다. 1.12.0 이상에서 `stateless_http=True`이거나 stdio·SSE만 쓰면 무관하다. 수정은 1.30.0과 2.2.0(둘 다 9-7 릴리스)이다. 권고는 "인증은 누가 세션을 여는지만 제한하지, 토큰 하나가 몇 개를 여는지는 막지 못한다"고 명시했다. 직전 판의 TypeScript SDK 3건에 이어 Python SDK도 원격 서버 기본 설정이 문제였다.

[GHSA-84m7-p3x7-pcfv](https://github.com/advisories/GHSA-84m7-p3x7-pcfv) (10-15 20:13 UTC, 공식)

### 5. Claude Code 플러그인 해시 핀 우회 (CVE-2026-86063, High) — 전역 DB에 10-15 등재, 수정은 6월

플러그인 설치 시 마켓플레이스가 핀한 커밋 SHA를 `git checkout`하되 결과 HEAD가 그 SHA인지 검증하지 않았다. 업스트림을 쥔 공격자가 40자 SHA와 같은 이름의 브랜치를 만들면 체크아웃이 조용히 그 브랜치 tip으로 풀려 검토된 커밋 대신 공격자 코드가 설치된다. 조건은 악성 플러그인 설치, 안전한 해시로 핀, SHA 모양 브랜치명을 허용하는 git 호스트 세 가지다. 영향 버전은 2.1.179 미만(6-16 릴리스)이고 자동 업데이트 사용자는 이미 수정됐다. 보고자는 Air Security의 Or Nevo Michrowski다. 저장소 권고는 10-9에 공개됐고 전역 DB·CVE 연결이 창 안이다. "해시 핀"이 주는 안전감이 체크아웃 검증 한 줄에 달려 있었다.

[GHSA-pq7j-f95f-qfcv](https://github.com/advisories/GHSA-pq7j-f95f-qfcv) (전역 10-15 20:22 UTC, 공식)

### 6. Nginx UI — MCP 도구가 REST API의 디렉티브 검증을 건너뛰어 인증 후 RCE (CVE-2026-84317·84318·84319·84360, 최고 CVSS 8.8)

MCP 도구 `nginx_config_add`가 `config.ValidateConfigFile` 호출 없이 설정을 쓰고 nginx reload까지 해서, REST API에서는 막히는 `perl`, `js_import`, `load_module`, `user root` 같은 디렉티브를 넣을 수 있었다. Perl·njs·Lua 모듈이 로드된 서버에서는 인증 후 RCE가 된다. OTP 스텝업도 MCP 설정 도구로 우회됐다(CVE-2026-84360). 수정은 v2.4.1이다. 같은 기능을 REST와 MCP 두 경로로 노출하면서 MCP 쪽만 검증을 빠뜨린 전형적 사례다.

[GHSA-9jww-cxg5-rj9m](https://github.com/advisories/GHSA-9jww-cxg5-rj9m) (10-15 20:10~20:14 UTC 4건, 공식)

### 7. Meta 새 상업 약관 — 비즈니스 계정과 "연관된" 웹사이트를 스크랩하고 "테스트 자격 증명"을 만들어 AI 학습에 쓸 권한 (HN 427점)

10-30 발효되는 Meta 상업 약관이 비즈니스 계정 소유자의 도메인·URL을 "web scrapers, bots, ... user agents"로 수집하고 "test credentials"를 만들어 접근하며, 그 콘텐츠로 "AI at Meta"를 포함한 Meta 제품을 "train"할 수 있다고 명시했다고 Nieman Lab이 보도했다. 옵트아웃은 없고 동의하지 않으면 상업 이용을 중단해야 한다. Cornell의 James Grimmelmann은 "가짜 사용자로 가입해 등록 장벽을 우회할 수 있는 문구"라고 했고, Meta 대변인은 흩어진 문구를 통합한 것일 뿐 크롤링 방식은 바뀌지 않는다고 답했다. robots.txt나 차단 지시보다 약관이 우선한다고 주장할 여지를 열었다는 해석이다.

[Nieman Lab](https://www.niemanlab.org/2026/10/metas-new-commercial-terms-will-allow-the-scraping-of-news-sites-legal-experts-say/) · [HN 50111852](https://news.ycombinator.com/item?id=50111852) (10-15 22:05 UTC, HN 23:37 UTC 427점·153댓글. 매체보도)

### 8. HF Daily Papers 10-16 상위

- **OmniCompass(홍콩이공대 VCLab, 407표):** 옴니모달 비디오 grounding·이해 벤치마크. 시각 증거만 쓰는 VL 트랙과 음성+시각을 결합한 AV 트랙에 각각 시간 grounding 등 3개 과제. [arXiv 2610.18829](https://arxiv.org/abs/2610.18829) · [GitHub](https://github.com/PolyU-VCLab/OmniCompass)
- **Waypoint-OPD(315표):** RL 전후 교사 log-prob 차이를 학생 궤적의 지도로 쓰는 policy-shift distillation에서, 같은 RL 런의 중간 체크포인트를 끼워 초기·후기 변화가 일치하는 토큰만 게이트해 전달한다. OPD 변형이 이번 주 내내 상위권이다. [arXiv 2610.21601](https://arxiv.org/abs/2610.21601)
- **OpenGCUA(알리바바 Tongyi-MAI, 203표):** "GPT-6 Astra의 컴퓨터 사용 시스템 같은 프런티어 구현은 닫혀 있다"며 시각·프로그램 인터페이스를 모델이 스스로 고르는 범용 컴퓨터 사용 에이전트 풀스택을 공개하는 프로젝트의 첫 릴리스. [arXiv 2610.22200](https://arxiv.org/abs/2610.22200) · [GitHub](https://github.com/Tongyi-MAI/OpenGCUA)
- **FDE-Gym(185표):** 포워드 디플로이드 엔지니어 업무를 흉내 낸 벤치마크. 18개 산업 20개 고객 시나리오, 95개 엔드투엔드 납품 과제에서 스크립트화된 이해관계자와 대화해 모호한 요구를 명확히 해야 한다. [arXiv 2610.22350](https://arxiv.org/abs/2610.22350)
- **Self-Evolving Agent Harness(184표):** 가중치는 고정한 채 프롬프트·메모리·워크플로·코드 등 하네스를 수정하는 "하네스 자기진화"를 형식화하고 continual PAC-learning으로 일반화·유지 경계를 도출. [arXiv 2610.18813](https://arxiv.org/abs/2610.18813)
- **MJev-Spatial(137표):** 공간 관계·거리·위치 판단을 자유 텍스트 생성 대신 후보를 클래스로 보고 attention 매칭으로 채점하는 System 1 멀티모달 결정 모델. [arXiv 2610.22276](https://arxiv.org/abs/2610.22276)
- **ATAS "Any Time, Any Space"(10-15, 339표, 직전 판 미수록):** 시나리오를 위치·카메라·사건 시각이 명시된 샷 요청으로 컴파일하고 scene·event memory에서 증거를 검색해 긴 에이전트 비디오 생성의 일관성을 유지. [arXiv 2610.20571](https://arxiv.org/abs/2610.20571) · [GitHub](https://github.com/ziplab/ATAS)

(HF API publishedAt은 arXiv 제출일, 날짜만 표기. 표 수는 10-16 16시대 UTC 기준)

### 9. 짧게

- **Terry Tao 블로그 게스트 글 "Why are we even doing all this?"(HN 188점):** 입자물리학자 Rikab Gambhir가 "10월 초 OpenAI가 700여 개의 난해하고 맞을지도 모르는 사실 목록을 IPO 홍보용으로 뽑았다"며 바벨의 도서관에 빗대 "검증 안 된 결과 더미는 연구가 아니다"라고 썼다. 물리학계의 2,500건 write-up도 함께 겨냥했다. [글](https://terrytao.wordpress.com/2026/10/15/why-are-we-even-doing-all-this/) · [HN 50109707](https://news.ycombinator.com/item?id=50109707) (10-15 20:21 UTC, 커뮤니티 의견)
- **Epic "바이브 코딩" 런처 vs Steam(HN 329점, 창 경계):** Epic이 10-14 AI 보조로 만든 Launcher V2를 Steam보다 빠르다며 내놓자, 이튿날 Valve가 작은 업데이트로 콜드 부팅 5.23초(Epic 5.47초)로 앞섰다(커뮤니티 측정). HN 341댓글은 바이브 코딩 마케팅 비판이 많았다. [RespawnFirst](https://respawnfirst.com/steam-client-is-now-faster-than-epics-new-vibe-coded-launcher/) · [HN 50105635](https://news.ycombinator.com/item?id=50105635) (기사 10-15 10:24 UTC는 창 전, HN 16:03 UTC. 매체보도)
- **"Jev meets the Trolley Problem"(HN 591점, 창 경계):** Jev, OpenAI Decisions API(GPT-6 Luna), 로컬 결정 모델 11종에 트롤리 문제 5,240 설정을 돌렸다. 대부분 "1명 희생해 5명 구함"(인간 81%)을 고르지만 변형에서 갈린다. 글은 10-14, HN 게시는 창 안이다. [글](https://zachlandes.com/trolley-problem/) · [HN 50106023](https://news.ycombinator.com/item?id=50106023) (10-15 16:24 UTC, 커뮤니티)
- **LightLLM ≤1.2.0 DoS(CVE-2026-111160, CVSS 7.5):** `/generate`에 내부 필드 `pd_kv_trans_params`를 넣으면 pickle.loads 실패로 추론 스레드가 죽고 `/health`는 살아 있어 이후 요청이 전부 멈춘다. [GHSA-mxm5-3g25-25c6](https://github.com/advisories/GHSA-mxm5-3g25-25c6) (10-15 21:36 UTC, unreviewed)
- **JupyterLab 4.5.7~4.6.3(CVE-2026-102832, CVSS 4.2):** 명령 버튼의 Trust 프롬프트가 클릭한 문서가 아니라 답할 때의 현재 문서를 신뢰 처리한다. 수정 4.5.11·4.6.4. [GHSA-jwrc-gm9j-263p](https://github.com/advisories/GHSA-jwrc-gm9j-263p) (10-15 20:22 UTC, 공식)
- **TRL v1.16.0:** AWS Trainium(Neuron) 학습 지원(`pad_to_multiple_of="auto"`로 Qwen3-0.6B SFT 20스텝 260초→130초), AsyncGRPO에 공유 프롬프트 토큰을 한 번만 forward하는 prefix tree 패킹. [릴리스](https://github.com/huggingface/trl/releases/tag/v1.16.0) (10-15 18:00 UTC, 공식)
- **Open WebUI v0.12.3:** DM·비공개 채널에 MLS 기반 브라우저 E2E 암호화 임시 채팅(서버 저장 없음, 16KiB 제한, 기본 꺼짐), Redis 스트리밍 경량화. [릴리스](https://github.com/open-webui/open-webui/releases/tag/v0.12.3) (10-16 11:46 UTC, 공식)
- **MAI-Base-2 사전학습 글과 Apple MLX 분산 추론 글**은 각각 모델 섹션 3번, 도구 섹션 6번에 정리했다.
- **덴마크 대법원 "AI 생성 CSAM도 CSAM"(HN 37점):** "묘사된 아동이 가상"이라는 항소를 "사진과 유사"하다며 기각했다. 법원 공시(덴마크어)라 본문·날짜는 확인하지 못했다(미확인).
- **새 진전 없음:** SneakPeek의 GHSA·CVE 번호는 창 안에 공개되지 않았다. MCP TypeScript SDK 3건, LMCache도 추가 패치가 없다. Anthropic alignment·OpenAI alignment·Google Research·DeepMind 블로그에 창 안 새 글이 없고, vLLM·SGLang·PyTorch·Ollama·LlamaIndex·DSPy·LangGraph·MCP SDK도 창 안 릴리스가 없다.

## 써볼 만한 도구

### 1. 새 진전: Claude Opus 5.5 Fast mode 정식 전환 — 베타 헤더 없이 호출, SDK에 `speed` 파라미터

- **설명:** Claude Platform 릴리스 노트(10-15)에 따르면 Fast mode가 Opus 5.5에서 리서치 프리뷰를 졸업했다. `fast-mode-2026-02-01` 베타 헤더 없이 호출할 수 있고(보내도 동작), Start·Build·Scale 티어에 fast mode 전용 레이트리밋이 기본으로 붙는다. Opus 5·Opus 4.8은 아직 프리뷰라 헤더를 유지해야 한다. Microsoft Foundry에서도 Opus 5.5 fast mode를 제공한다. 같은 날 나온 anthropic-sdk-python 1.15.0·typescript 0.135.0은 Messages에 `speed` 파라미터와 `usage.speed`를 추가했고, 모델 capabilities의 thinking 타입에 `between_tools`가 들어갔다. Console 조직의 사용량 티어 지출 상한은 읽기 전용 spend limit로 돌려준다. TS는 읽다 만 스트림을 닫을 때의 버그 2건을 고쳤다.
- **추천 이유:** Opus 5.5를 저지연으로 쓰려던 팀은 헤더를 빼고 SDK만 올리면 된다. 직전 판 1.14.0(OTel)에 이어 두 번째 기능 릴리스다. 함께 나온 Compliance API의 사용자 지시문 조회(`GET /v1/compliance/organizations/{org_uuid}/user_preferences`, `read:compliance_user_data` 스코프)는 Enterprise 관리자용이다.
- **설치·사용:** `pip install -U anthropic` · [릴리스 노트](https://platform.claude.com/docs/en/release-notes/overview) · [Fast mode 문서](https://platform.claude.com/docs/en/build-with-claude/fast-mode) · [python v1.15.0](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v1.15.0) · [typescript v0.135.0](https://github.com/anthropics/anthropic-sdk-typescript/releases/tag/sdk-v0.135.0) (릴리스 노트는 날짜 10-15만 표기. SDK는 10-15 19:45·19:42 UTC, 공식)

### 2. 새 진전: Claude Code 2.1.301 — 무인 재시도 대기 상한, OTel `tool_decision`에 분류기 판정, vim `%`

- **설명:** `CLAUDE_CODE_RETRY_WATCHDOG_MAX_USAGE_LIMIT_WAIT_MS`로 무인 재시도 모드에서 사용량 한도 리셋이 너무 멀면 기다리지 않고 요청을 끝낸다. OTel `tool_decision` 이벤트에 `classifier_verdict`·`permission_mode`·`classifier_category`(`OTEL_LOG_TOOL_DETAILS=1`)가 붙어 자동 모드 분류기 판정과 권한 규칙 판정을 구분할 수 있다. vim 모드에 `%`(괄호 짝 이동, d/c/y 조합)가 생겼고, 모드(mod)의 `Input`은 `rows`로 여러 줄 입력을 받는다. `fable`·`best` 별칭은 조직에 최신 Fable이 없으면 이전 Fable로 한 번 재시도한다. 작은 컨텍스트 모델로 바꾸면 이전 모델이 먼저 요약한다. 버그픽스 40여 건(Grep·Glob 검색 실패 시 "no match" 대신 오류 반환, 훅이 만든 워크트리의 미푸시 커밋 삭제 방지, Read가 .xlsm·.parquet를 텍스트로 읽던 문제 등). VS Code 확장은 LaTeX 수식을 렌더링한다.
- **추천 이유:** OTel로 Claude Code 사용을 모니터링하는 팀과 무인 CI 모드를 쓰는 쪽은 바로 올릴 만하다. 나머지는 체감이 작다. 같은 시각 나온 Agent SDK TypeScript 0.3.301은 sdk형 MCP 서버 설정에 도구별 `permission_policy`(`always_ask`·`always_deny`)와 `reloadPlugins({addPlugins})`를 추가했다. Python 0.2.168은 CLI 번들만 올렸다.
- **설치·사용:** `claude update` · [릴리스](https://github.com/anthropics/claude-code/releases/tag/v2.1.301) · [Agent SDK TS v0.3.301](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.301) · [Agent SDK Python 0.2.168](https://pypi.org/project/claude-agent-sdk/0.2.168/) (10-15 19:22 UTC, 공식)

### 3. OpenAI Codex CLI 0.163.0 — 응답 중 스티어링, 읽기 전용 도구 병렬 실행, 검색형 `/config`

- **설명:** 응답 중에 후속 메시지를 보내면 즉시 방향을 바꾼다(턴을 끊으면 Code Mode 셀과 중첩 도구 호출도 멈춘다). 독립적인 읽기 전용 도구(스킬·메모리·대화 이력 조회 포함)는 병렬로 돈다. `/config`는 카테고리를 가로질러 설정을 검색하고 바로 바꾸며 재시작이 필요한 항목을 표시한다. `/keymap` 리더 키(기본 Ctrl+X), 음성 음소거 F9, `/daemon`에서 백그라운드 서버 재시작이 생겼다. Daybreak(OpenAI 사이버 접근 프로그램 기능)는 자격 있는 계정에 기본 활성화되고 `codex exec`에서도 API 키로 고를 수 있다. 보안 쪽은 네트워크 프록시가 DNS 조회 전에 권한을 확인하고, 중복 Host 헤더를 거부하며, 자격 증명 별칭으로 검사 훅을 우회하는 경로를 막았다. 패치된 zsh 번들을 없애고 표준 셸 실행기로 돌아갔다.
- **추천 이유:** 스티어링과 읽기 도구 병렬화는 체감되는 변화다. 커스텀 셸 플래그를 쓰던 설정은 마이그레이션 알림을 확인해야 한다. 0.164.0-alpha.1도 창 안(10-16 14:49 UTC)에 나왔지만 알파다.
- **설치·사용:** `npm i -g @openai/codex` · [릴리스](https://github.com/openai/codex/releases/tag/rust-v0.163.0) (10-15 21:30 UTC, 공식)

### 4. Cline 4.1.25 — 호스팅 샌드박스 "Cline Cloud sessions", 기본 모델이 Gemini 4 Argon으로

- **설명:** 설정에서 켜면 GitHub 저장소·브랜치를 골라 호스팅 샌드박스에서 작업이 돌고, 에디터를 닫아도 계속된다(점진 롤아웃). 모델을 고르지 않으면 Cline 프로바이더 기본값이 Gemini 4 Argon이다(OpenRouter 경로는 Claude Sonnet 5.5 유지). 카탈로그에 Boundless·Standard Compute 프로바이더가 추가됐고 무료 목록에 Nemotron 3.5 Super VL이 들어갔다. LM Studio·Ollama에서 5분 무응답 시 나던 BodyTimeoutError를 고쳤고, OpenAI 호환 URL에 `/v1`을 자동으로 붙이던 동작을 멈췄다.
- **추천 이유:** 로컬 모델을 쓰는 Cline 사용자는 타임아웃 수정만으로도 올릴 가치가 있다. 여러 프로바이더의 기본 모델이 바뀌었으니 모델을 고정하지 않은 설정은 확인이 필요하다.
- **설치·사용:** VS Code 마켓플레이스 · [릴리스](https://github.com/cline/cline/releases/tag/v4.1.25) (10-16 06:39 UTC, 공식)

### 5. Pydantic AI 2.57.0 — Muse용 `MetaProvider`, 저위험 도구를 모델이 먼저 승인하는 `ApprovalReviewer`, MCP Apps 중계

- **설명:** `meta:` 접두 모델명으로 Muse 모델을 Responses API로 부른다. Harness의 `ApprovalReviewer`는 사람 승인 전에 저위험 도구 호출을 모델이 승인하게 한다. `MCPToolset.call_tool_for_app`·`read_resource_contents`로 MCP Apps 도구 호출을 중계하고 결과를 AG-UI·Vercel AI 프런트엔드로 넘긴다. `Skills`가 `disable-model-invocation`·`user-invocable` 프런트매터를 존중하고, 실시간 세션(GPT-Live·Gemini Live) 이력·usage 처리가 바뀌었다. 토큰 기반 압축이 최근 이력을 너무 적게 남기던 문제와 OpenAIChatModel이 Gemini thought signature를 돌려보내지 않던 문제도 고쳤다.
- **추천 이유:** UI가 있는 MCP 도구(MCP Apps)를 자체 프런트에 올리려는 Python 팀에는 쓸 만하다. 호환성 노트가 4건이라 마이너 업이지만 테스트가 필요하다.
- **설치·사용:** `pip install -U pydantic-ai` · [릴리스](https://github.com/pydantic/pydantic-ai/releases/tag/v2.57.0) (10-16 03:35 UTC, 공식)

### 6. Apple MLX 분산 추론 — Mac Studio 4대로 Muse Spark Open(816B) 36 tok/s, mlx-lm 0.32.0

- **설명:** Apple ML Research 글은 M5 Ultra Mac Studio(통합 메모리 최대 512GB)를 Thunderbolt RDMA 드라이버로 묶어 텐서·파이프라인 병렬 추론을 하는 과정을 다룬다. Muse Spark Open(NVFP4)을 4대 텐서 병렬로 8K 컨텍스트에서 36 tok/s 이상 냈고, Mistral Medium 3.5(128B dense)·Qwen 3.8(27B)을 M3 Ultra와 비교했다. 함께 나온 mlx-lm 0.32.0은 `meta-models/Muse-Spark-Open-816B-A42B-NVFP4`를 추가하고 NVFP4 MoE 글로벌 스케일과 미변환 모델 지원을 넣었다.
- **추천 이유:** Mac Studio 클러스터가 있는 쪽에만 해당하지만, 로컬에서 1T급 MoE를 돌리는 가장 구체적인 레시피다.
- **설치·사용:** `pip install -U mlx-lm` · [Apple 글](https://machinelearning.apple.com/research/mlx-distributed-inference) · [mlx-lm v0.32.0](https://github.com/ml-explore/mlx-lm/releases/tag/v0.32.0) · [HN 50118320](https://news.ycombinator.com/item?id=50118320) (글은 날짜 10-15만 표기, HN 10-16 13:43 UTC 61점, mlx-lm 10-15 18:30 UTC, 공식)

### 7. GitHub Copilot CLI 1.0.99-1·-3 — `ghapp://` 링크로 검증된 스킬·플러그인 설치, `--trust-folder`, 클라이언트 텔레메트리 끄기

- **설명:** `copilot plugin install "ghapp://air/install?..."`로 GitHub 설치 링크에서 검증된 Skills·플러그인을 설치한다. `-p`·파이프 세션용 `--trust-folder`가 생겼고, 샌드박스 git이 `credentials.envVars`의 GITHUB_TOKEN을 쓴다. 1.0.99-3은 `--disable-client-telemetry`를 추가하고, 샌드박스 자격 증명 프록시의 인증서 실패를 설명하며, 추론만 하다 max-token에 걸리는 무한 루프를 끝낸다. 직전 판의 HydraFusion은 패스 표시 수정이 들어갔다.
- **추천 이유:** 플러그인 설치 링크는 조직 배포에 유용하다. 그 외는 품질 패치다. 같은 날 GitHub Changelog에는 Slack·Teams의 @GitHub 봇이 한 대화에서 여러 저장소의 PR·이슈를 조회하는 기능도 올라왔다.
- **설치·사용:** `npm i -g @github/copilot` · [1.0.99-1](https://github.com/github/copilot-cli/releases/tag/v1.0.99-1) · [1.0.99-3](https://github.com/github/copilot-cli/releases/tag/v1.0.99-3) · [Slack·Teams 멀티 저장소](https://github.blog/changelog/2026-10-15-copilot-in-slack-and-teams-adds-model-and-repository-updates) (10-15 17:21 · 10-16 15:08 UTC, Changelog RSS 10-16 02:16 UTC, 공식)

### 8. Chrome DevTools MCP 1.11.0 — ffmpeg 없는 스크린캐스트, 힙 스냅샷 분석, 설정 파일 자동 탐색 (창 경계 항목)

- **설명:** ffmpeg 없이 스크린캐스트 도구가 기본으로 켜지고, `analyze_heapsnapshot_contexts` 도구가 생겼다. Lighthouse 실패 항목과 DOM 노드를 인라인으로 돌려주고, 텍스트 스냅샷의 노이즈를 줄였다. 설정 파일을 자동으로 찾아 감시하며, `--file-navigations`로 file: URL을 막을 수 있다. WebMCP 도구 디버깅도 들어갔다.
- **추천 이유:** 브라우저 디버깅을 에이전트에 맡기는 사람에게는 가장 많이 쓰이는 MCP 서버다. 직전 창 종료 2시간 전(10-15 14:01 UTC)에 나와 직전 판이 놓쳤다. 이번 창에 claude-plugins-official의 chrome-devtools-mcp 서브모듈 bump(10-16 02:38 UTC)로 반영됐다.
- **설치·사용:** `npx chrome-devtools-mcp@latest` · [릴리스](https://github.com/ChromeDevTools/chrome-devtools-mcp/releases/tag/chrome-devtools-mcp-v1.11.0) (10-15 14:01 UTC, 공식)

### 짧게

- **OpenAI GPT-Live에 Decisions 네이티브 통합:** `v1/live/sessions`에서 사용자가 말하는 중에 Live가 Decisions를 직접 위임 호출하거나 전사 델타를 트리거로 쓴다. [가이드](https://developers.openai.com/api/docs/guides/decisions-voice) · [Changelog](https://developers.openai.com/api/docs/changelog) (날짜 10-15만 표기, 공식)
- **transformers 5.20.0·5.20.1:** Muse Spark Open, Ling 3.0 Flash, Kolibri 1, MiniCPM-V 4.7, UnlimitedOcr, Beam이 네이티브로 들어갔다. 5.20.0은 torch 2.6·2.7에서 `from_pretrained`가 깨지므로 올리려면 5.20.1로 간다(분산 체크포인트는 torch 2.8 이상). [v5.20.0](https://github.com/huggingface/transformers/releases/tag/v5.20.0) (10-15 18:05 · 10-16 13:02 UTC, 공식)
- **OpenCode 1.19.2·1.19.3:** 1.x 라인이 2.x를 일반 업데이트로 제안하지 않게 고정했다(2.0은 `opencode upgrade --major`로만). 1.19.3은 모델 reasoning 옵션으로 변형을 만든다. [v1.19.3](https://github.com/sst/opencode/releases/tag/v1.19.3) (10-16 12:31 UTC, 공식)
- **Vercel AI SDK ai@7.0.142**(OpenAI 커스텀 도구 입력 델타 원문 복원, `inputEncoding: 'text'`) · **langchain-core 1.6.11** 패치. [ai@7.0.142](https://github.com/vercel/ai/releases/tag/ai%407.0.142) · [langchain-core 1.6.11](https://github.com/langchain-ai/langchain/releases/tag/langchain-core%3D%3D1.6.11) (10-15 18:11 · 18:32 UTC, 공식)
- **claude-plugins-official:** 창 안 커밋 30건이 전부 마켓플레이스 플러그인 서브모듈 bump(chrome-devtools-mcp, slack, firecrawl, qdrant, auth0, sonarqube 등)였다. 신규 플러그인은 없고 anthropics/skills에도 창 안 커밋이 없다.
- **MCP 공식 레지스트리:** 창 안 신규 2건(apex-ai-academy, fenlight) 모두 소규모. modelcontextprotocol 조직의 SDK·스펙·inspector는 창 안 릴리스가 없다.
- **Show HN: Sygal** — PDF·스캔·Word·Excel·웹페이지·YouTube를 로컬에서 Markdown·JSON으로 바꾸는 데스크톱 앱(macOS·Windows, 25유로 일회 구매, 폐쇄 소스). [sygal.app](https://sygal.app) · [HN 50106054](https://news.ycombinator.com/item?id=50106054) (10-15 16:26 UTC 43점, 커뮤니티)

## 주목할 점

- **오픈 웨이트의 "미국 복귀"와 라이선스 분화:** 같은 날 나온 두 모델이 정반대 조건이다. Muse Spark Open은 표기 의무와 매출 2,000만 달러 조건이 붙은 자체 라이선스이고, Beam은 Apache 2.0이다. Microsoft는 가중치를 Foundry 파트너에만 라이선스하겠다고 했다. 오픈 웨이트 상위권이 중국 모델 일색이던 구도는 바뀌었지만, "오픈"의 조건이 다시 갈리기 시작했다. 벤치마크 비교에서는 Argon의 ARC-AGI-3 점수가 하네스에 따라 97.5%와 49.8%로 갈린 것처럼, 하네스 조건을 빼고는 수치를 읽기 어려워졌다.
- **에이전트 공급망의 "기본값" 문제가 CVE 번호를 달기 시작했다:** 직전 판의 MCP TypeScript SDK 공유 인스턴스에 이어, Python SDK의 세션 미해제, 세 코딩 에이전트의 리포 내 MCP 설정 자동 실행, Claude Code 플러그인 해시 핀 미검증, Nginx UI의 MCP 경로 검증 누락이 하루에 올라왔다. 리포를 열거나 플러그인을 설치하는 행위 자체가 코드 실행 경로가 된다는 점에서, 에이전트 도구는 "신뢰하지 않은 폴더" 프롬프트와 체크아웃 검증 같은 기본을 다시 점검할 때다.

## 조사 제약

- **openai.com 본문:** 403이라 RSS와 `developers.openai.com`의 `.md` 문서로 대신했고, Rosalind Academic Plan만 r.jina.ai 경유로 본문을 확인했다. Reuters(캡차)·Ars Technica(쿠키 동의)·Business Insider(404)·BBC·The Verge(Chrome 탭) 본문은 읽지 못해 미확인으로 남겼다.
- **날짜만 있는 소스:** Meta·Reflection·Apple ML Research·Goodfire·Liquid AI·Datadog·sunshowers 글, platform.claude.com 릴리스 노트, OpenAI API changelog. 시각은 HF 리포·OpenRouter·HN·매체 보도로 보강했다.
- **X 계열:** x.ai/news는 Cloudflare 차단이고 api.fxtwitter.com의 사용자 타임라인 조회가 전부 오류를 내 트윗 기반 시각 보강을 하지 못했다.
- **벤치마크 원문:** ARC Prize 페이지 링크 추출에 실패해 Argon 수치는 The Decoder 보도 기준이다. Muse Spark Open HF 카드는 gated라 벤치마크 표 원문을 보지 못했고, Krea 3 비교표는 JS 렌더라 수치를 추출하지 못했다.
- **보안 권고:** Junie·ZCode·DeepSeek Harness 건은 unreviewed라 벤더 확인·패치 정보가 없다. SneakPeek의 GHSA·CVE 번호는 창 안에 공개되지 않았다.
- **읽지 못한 소스:** deepmind.google 블로그 RSS와 ai.meta.com RSS는 빈 응답(HTML 목록으로 대체), microsoft.com/research는 r.jina.ai가 차단, antigravity.google changelog는 본문 추출 실패, Nieman Lab 피드는 Cloudflare 챌린지, currentaffairs.org 본문 추출 실패. platform.claude.com의 Claude Code 릴리스 노트 `.md`는 GitHub HTML로 리다이렉트돼 GitHub 릴리스로 대체했다. Vertex AI 피드는 03-16 이후 갱신이 없다.
