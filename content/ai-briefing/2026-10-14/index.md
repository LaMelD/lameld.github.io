---
title: "2026-10-14 AI 브리핑"
date: 2026-10-14T01:00:00+09:00
tags: [ai-briefing, meta, google, anthropic, security]
description: "Meta가 첫 프런티어 모델 Muse Wonder를 폐쇄형 API로 냈고, Gemini 4 Argon이 절반 가격에 일반 출시됐으며, Anthropic은 소형 모델의 IoT 자기 복제 가능성을 경고했고 Codex CLI 상주 데몬과 에이전트 승인 우회 CVE가 논란이 됐다."
---

> 조사 범위: 2026-10-13 01:00 ~ 2026-10-14 01:00 KST(2026-10-12 16:00 ~ 10-13 16:00 UTC, 월요일 밤~화요일 미국 오전). 직전 브리핑의 조사 창 종료 시각부터 잡았다. 시각은 기사 `article:published_time`·`datePublished`, GitHub `published_at`·`created_at`, GHSA `published_at`, MCP 레지스트리 `publishedAt`, HN Algolia `created_at`, 상태 페이지 API로 검증했다. HN 점수는 조회 시점(10-13 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **Meta가 첫 프런티어 모델 Muse Wonder·Wonder Full을 냈다.** 입력 $4·출력 $20(100만 토큰당)의 폐쇄형 API로, 외부 지수에서 상위권에 들었지만 지식 정확도는 크게 뒤처진다. Llama 시절의 오픈웨이트 노선과 결별한 셈이다.
- **Gemini 4 Argon이 API·Gemini 앱·AI 모드·Antigravity에 일반 출시됐다.** 출시가는 입력 $2·출력 $10으로 경쟁 프런티어 모델의 절반이다. 창 종료 시각(01:00 KST) 정각에 공개됐다.
- **Anthropic이 "$55짜리 TV 박스 15대에서 소형 모델이 스스로 퍼질 수 있다"는 레드팀 결과를 냈다.** 같은 날 Codex CLI의 상주·자동 업데이트 데몬 논란(HN 589점)과 Continue·goose의 승인 우회 CVE가 겹쳐, 에이전트 CLI 신뢰 경계 문제가 다시 불거졌다.

## 모델 소식

### 1. Meta, 첫 프런티어 모델 Muse Wonder·Muse Wonder Full 출시 — 폐쇄형 API, 입력 $4/출력 $20

Meta Superintelligence Labs가 Muse Wonder와 상위판 Muse Wonder Full을 공개했다. Wonder는 Meta Model API와 Muse Code에서 바로 쓸 수 있고, Full은 Muse Code 구독자 전용이다(API 미제공). API 가격은 100만 토큰당 입력 $4·출력 $20이고, 컨텍스트 1M, 입력은 텍스트·이미지·비디오·오디오다. 대화를 넘어 사용자를 기억하는 "Active Memory", 어려운 과제를 끝까지 물고 늘어지는 "Active Focus"를 내세웠다. The Decoder가 전한 외부 평가로는 Artificial Analysis Intelligence Index에서 Full 56점·Wonder 52점(전작 Spark 1.3은 48점)이고, Terminal-Bench 4.0은 33%에서 57%로 올랐다. 반면 지식 정확도(AA-Omniscience)는 Full이 30점으로 Opus 5.5(46)·GPT-6 Astra(43)에 크게 뒤진다. 전체 안전 보고서는 "곧" 내겠다고 했다.

Llama로 오픈웨이트 진영을 이끌던 Meta가 첫 프런티어급 모델을 **폐쇄형 API**로 냈다는 점이 가장 큰 변화다. 가격도 GPT-6 Sol·Opus 5.5와 같은 대에 맞췄다. 같은 날 FT는 Zuckerberg가 프런티어 도달 뒤에도 감속 요구를 거부했다고 보도했다(유료벽, 본문 미확인).

- [Meta 블로그](https://research.meta.ai/blog/muse-wonder) · [TechCrunch](https://techcrunch.com/2026/10/12/meta-releases-its-first-frontier-model-muse-wonder/) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1009479/meta-muse-wonder-full-ai-model) · [The Decoder](https://the-decoder.com/metas-new-frontier-model-muse-wonder-full-breaks-into-the-ai-top-tier/) · [HN](https://news.ycombinator.com/item?id=50059115)
- 게시: Meta 블로그는 날짜만(10-12), The Verge 10-12 17:00 UTC·TechCrunch 17:56 UTC(`article:published_time`), HN 16:48 UTC(653점) · 신뢰도: **공식**(벤치 수치는 **매체보도**)

### 2. Gemini 4 Argon 일반 출시 — API·Gemini 앱·AI 모드·Antigravity, 출시가 입력 $2/출력 $10 (창 경계 시각)

9-30에 발표된 Gemini 4 Argon이 전면 제공에 들어갔다. 개발자는 AI Studio·Gemini API에서 `gemini-4-argon`(모델 페이지 "Stable")으로 쓸 수 있다. 무료 등급은 없고, 출시 할인가는 100만 토큰당 입력 $2·출력 $10이며 캐시 입력은 95% 할인이다. 소비자 쪽은 Pro·Ultra 구독자가 Gemini 앱과 검색 AI 모드에서, Ultra 구독자가 Antigravity에서 쓴다. Google은 Vals Index 1위, GPT-6 Astra·Fable 5.1·Opus 5.5보다 낫다고 주장했다. 사내 Antigravity에서 에이전트 코드 제출이 35% 넘게 늘고 롤백은 약 3분의 1 줄었다고 밝혔다. Gemini macOS 앱에는 Antigravity가 들어갔고, 채팅과 작업(tasks) 모드를 오가는 토글이 생겼다.

지난 2주간 "진행 중"으로 지켜보던 항목이 풀렸다. 프런티어급 모델을 Muse Wonder·Opus 5.5의 절반 가격에 내놓아 같은 날 나온 Meta와 가격 차가 뚜렷하다. ai.google.dev API 변경 로그에는 아직 반영되지 않았다(최신 10-08).

- [Google 발표](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-argon/) · [Gemini 앱 공지](https://blog.google/innovation-and-ai/products/gemini-app/gemini-4-argon-availability/) · [Gemini 앱 릴리스 노트](https://gemini.google/release-notes/) · [모델 페이지](https://ai.google.dev/gemini-api/docs/models) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1009862/google-releases-gemini-4-argon)
- 게시: 10-13 16:00:00 UTC(= 10-14 01:00 KST, 두 blog.google 글 `datePublished`·The Verge 동일) — 창 종료 시각과 정확히 겹쳐 이번 판에 싣는다 · 신뢰도: **공식**

### 3. Anthropic Frontier Red Team "소형 모델이 데이터센터 없이 자기 복제할 수 있게 될 수 있다"

Anthropic이 소형 모델이 IoT 기기를 장악해 스스로 퍼지는 "가중치 기반 복제자" 시나리오를 평가했다. 9B 모델이 처음 보는 하드웨어 9종 중 8종에서 추론 환경을 스스로 꾸렸고, 단순화한 평가에서는 GLM-5.2 이상 모델이 복제 과정 전체를 해냈다. 실증으로 Gemma4-26B-A4B를 대당 약 $55인 안드로이드 TV 박스 15대에 나눠 0.39 tok/s로 돌렸다. 소형 모델이 지금 속도로 발전하면 연말쯤 이 수준에 이른다고 봤다. 확산 자체는 느릴 수 있지만(개체 수가 두 배가 되는 데 수 일) 중앙에서 끌 지점이 없다는 점을 경고했다.

자율 복제 위험을 "프런티어 모델 탈출"이 아니라 "저가 기기 + 소형 오픈웨이트"라는 현실적인 경로로 구체화했다. 오픈웨이트 배포 정책과 IoT 보안 논쟁에 바로 인용될 자료다. Reuters도 10-13에 재보도했다.

- [Anthropic](https://www.anthropic.com/research/self-replicating-ai) · [The Decoder](https://the-decoder.com/anthropic-study-warns-that-small-ai-models-could-soon-replicate-across-everyday-devices/) · [HN](https://news.ycombinator.com/item?id=50061603)
- 게시: 10-12 19:30 UTC(`datePublished`) · 신뢰도: **공식**

### 4. Anthropic "증류 공격 방어 강화" — 추론 노출 차단으로 수확량 90~99% 감소, Zhipu 지목 (창 경계 항목)

Anthropic이 추론(reasoning) 토큰 노출을 줄이는 방어로 증류업자가 수확하는 데이터가 90~99% 줄었다고 밝혔다. 실험에서는 Opus 4.8의 전체 추론 기록으로 학습한 모델이 SWE-bench Verified 80.4%, 요약본으로만 학습한 모델은 71.8%였다. Zhipu가 GLM-5.3 개발 때 미국 모델의 사이버 능력을 증류했다고 지목했다. 처음에는 Claude Fable 5를 노렸고, 막히자 구형 Claude와 다른 랩 모델로 옮겼다고 한다. Moonshot Kimi로 들어온 요청이 몰래 Claude로 넘어간 사례도 언급했다.

원문은 직전 창(10-12 09:30 UTC)에 나왔지만 직전 브리핑에서 빠졌고, 창 안에 The Decoder 재보도와 HN 게시가 있어 여기 싣는다. 요약 추론만 보여 주는 방식이 증류 방어 수단으로 효과가 있다는 수치가 처음 나왔다.

- [Anthropic](https://www.anthropic.com/research/distillation-defenses) · [The Decoder](https://the-decoder.com/anthropic-says-it-cut-claude-reasoning-token-theft-by-up-to-99-in-fight-against-industrial-scale-distillation/)
- 게시: 원문 10-12 09:30 UTC(창 이전), The Decoder 10-13 11:06 UTC · 신뢰도: **공식**

### 5. Z.ai GLM-Legal — 법률 특화 에이전트 모델 (HN 642점)

GLM-5.3-Flash를 기반으로 중간 학습과 법률 환경 수만 개에서의 강화학습을 거친 모델이다. Harvey LAB 전 항목 통과율이 19.2%로 기반 모델보다 12.5%p 높고, LegalBench·PLawBench에서 폐쇄형 프런티어 모델과 동급이라고 주장한다. Harvey LAB·LegalBench 수치는 Vals가 독립 평가했다고 밝혔다. 데스크톱 에이전트 AutoClaw에 "곧" 들어가며, 가중치는 공개되지 않았다.

Grok 4.7·GLM-Legal이 모두 Harvey LAB을 내세우면서 법률이 에이전트 경쟁의 새 벤치마크 영역이 됐다. 같은 창에 Anthropic이 Zhipu를 증류 사례로 지목해(4번), 점수를 받아들일 때 이 점을 함께 볼 필요가 있다.

- [Z.ai 블로그](https://z.ai/blog/glm-legal) · [HN](https://news.ycombinator.com/item?id=50058507)
- 게시: 블로그에 날짜 없음. HN 첫 게시 10-12 16:11 UTC, 블로그 영상 파일명 시각 베이징 23:27(15:27 UTC)로 창 시작 전후 · 신뢰도: **공식**(시각은 커뮤니티 근거)

### 6. xAI "The X Subscription" — Grok·Cursor·X Premium을 묶은 5단계 요금제

SpaceXSI(xAI)가 Grok, Grok Bot, Cursor, X Premium을 하나로 묶은 구독을 내놨다. 등급은 무료, Plus $8, Premium $30, Super $100, Ultra $200(월)이고, 최신 Grok 모델은 Premium 이상에서만 쓸 수 있다. 며칠에 걸쳐 전 세계에 순차 적용한다.

모델 접근권을 코딩 도구(Cursor)와 SNS 구독에 한데 묶은 첫 사례다. Cursor를 이미 쓰는 개발자라면 개별 구독과 견줘 볼 만하다.

- [x.ai](https://x.ai/news/x-subscription) · [TechCrunch](https://techcrunch.com/2026/10/13/spacexsi-bundles-x-premium-grok-and-cursor-into-one-subscription/)
- 게시: x.ai는 날짜만, HN 첫 게시 10-13 00:07 UTC, TechCrunch 13:50 UTC · 신뢰도: **공식**+매체보도

### 7. Character.ai 챕터 11 파산 신청 — 일부 자산 Disney에 $20M 매각

Character.ai가 법원 감독 아래 매각을 위해 자발적 파산을 신청했다. 자산·부채는 각각 $50M~100M 규모다. 일부 자산과 기술의 비독점 영구 라이선스를 Disney에 $20M에 넘기며, 다른 인수자가 없으면 플랫폼을 닫는다. 매각 마감은 11월 말~12월 초 예정이다.

2024년 Google이 $2.7B 계약으로 핵심 인력을 데려간 자체 모델 회사가 결국 문을 닫는 수순이다. 미성년자 소송·규제 부담이 소비자 챗봇 사업을 얼마나 압박하는지 보여 준다.

- [Character.ai 공지](https://blog.character.ai/an-update-on-character-ai/) · [TechCrunch](https://techcrunch.com/2026/10/13/just-5-years-in-interactive-chatbot-platform-character-ai-files-for-bankruptcy/) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1009802/character-ai-files-for-bankruptcy-disney)
- 게시: 10-13 03:52 UTC(`article:published_time`) · 신뢰도: **공식**

### 8. 인시던트

- **OpenAI:** 10-13 08:10 UTC부터 "GPT-6.1 Sol 기반 Codex·Work 모드 성능 저하"(minor)가 발생했다. 09:30 UTC에 완화 조치를 적용했고 조회 시점까지 monitoring 상태다. [인시던트](https://status.openai.com/incidents/01M4T4ZCM54D3ZPTQXRSWFG0XS) (공식)
- **Anthropic·Google Cloud:** 창 안 신규 인시던트 없음. [status.claude.com](https://status.claude.com/) · [status.cloud.google.com](https://status.cloud.google.com/) (공식)

### 9. 짧게

- **Grok 4.7, Amazon Bedrock 제공:** xAI 수치로 Terminal-Bench 4.0 20.3%→37.6%, Harvey LAB 15.8%→19.6%다. 모델 자체는 9-21 출시. [AWS](https://aws.amazon.com/blogs/machine-learning/build-long-horizon-agents-with-grok-4-7-on-amazon-bedrock/) (10-13 15:52 UTC, 공식)
- **Bedrock의 OpenAI GPT 모델, 태국·말레이시아·싱가포르·인도네시아·대만 글로벌 교차 리전 추론 지원.** [AWS](https://aws.amazon.com/blogs/machine-learning/accessing-openai-gpt-models-on-amazon-bedrock-with-global-cross-region-inference-in-thailand-malaysia-singapore-indonesia-and-taiwan/) (10-12 17:52 UTC, 공식)
- **TimesFM 3.0, BigQuery 다변량 시계열 예측 지원.** [Google Cloud](https://cloud.google.com/blog/products/data-analytics/bigquery-timesfm-3-0-with-multivariate-time-series-forecasting/) (10-12 16:00 UTC, 공식)
- **진전 없음:** Haiku 4.5 은퇴(폐기 페이지에 여전히 Active, "10-15 이후"), GPT-6.1 Astra(문서 404), Mistral Large 4 가중치, Reflection Beam 가중치. OpenAI 뉴스 RSS(최신 10-09)·API 변경 로그(10-08), Claude 릴리스 노트(10-09), DeepSeek·Alibaba·DeepMind 블로그에는 창 안 신규 글이 없다. HF 주요 조직에도 창 안 신규 업로드가 없다.

## 기술 이슈

### 1. Codex CLI가 기본값으로 상주·자동 업데이트 데몬을 설치한다는 논란 (HN 589점)

openai/codex 이슈 #48195(09-25 생성)가 창 안에서 HN 상위에 올랐다. 0.157.0부터 대화형 세션을 한 번만 열어도 `codex app-server --managed-daemon`이 PID 1 밑으로 떨어져 계속 상주한다. 약 374MB짜리 플랫폼 패키지를 `~/.codex/packages/`에 복사하고, 분리된 업데이터가 5분 뒤와 그 뒤 매시간 새 릴리스를 받아 백엔드를 재시작한다. 처음 띄운 세션의 `HTTPS_PROXY` 같은 환경 변수가 그대로 굳는다는 점도 지적됐다. 창 안에는 "회사가 Codex 좌석을 회수했다"는 댓글이 달렸고, 공식 설정 문서에 `daemon_auto_start` 끄는 법이 없다는 지적도 나왔다. 새 이슈 #53547 "데몬이 세션 간에 환경 변수를 누출한다"(0.162.1)도 올라왔다. 창 안에서 OpenAI 측 답변이나 기본값을 바꾸는 PR은 보이지 않았다.

에이전트 CLI가 사용자 모르게 실행 파일을 바꿔 끼우는 구조라, 변경 통제가 필요한 기업 환경에서는 그대로 쓰기 어렵다. Codex를 쓴다면 `ps`로 상주 프로세스를 확인하고, 세션마다 프록시·자격 증명 환경 변수가 섞이지 않는지 볼 것.

- [이슈 #48195](https://github.com/openai/codex/issues/48195) · [이슈 #53547](https://github.com/openai/codex/issues/53547) · [HN](https://news.ycombinator.com/item?id=50060694)
- 게시: HN 10-12 18:29 UTC, #53547 10-13 04:00 UTC(GitHub `created_at`) · 신뢰도: **커뮤니티**(이슈 원문은 1차 자료)

### 2. 코딩 에이전트 승인 우회 CVE — Continue CLI·goose, 그리고 Open WebUI 19건 일괄 CVE

- **Continue CLI ≤1.5.47(CVE-2026-109560, high):** 허용된 Bash 접두 규칙 뒤에 `;`·`&&`·`|`로 명령을 이어 붙이면 승인 없이 실행된다. 저장소 파일이나 가져온 웹 페이지에 심은 프롬프트 인젝션으로 노릴 수 있다. [GHSA-f54m-g39v-qf2p](https://github.com/advisories/GHSA-f54m-g39v-qf2p)
- **goose 1.28.0~1.44.0 미만(CVE-2026-109561, low):** Smart Approval을 쓰다 Manual 모드로 바꿔도 `readOnlyHint: true` 표시가 남아 있어, 인젝션된 도구 호출이 승인 없이 실행된다. [GHSA-8h69-7j5q-pwfc](https://github.com/advisories/GHSA-8h69-7j5q-pwfc)
- **sandboxed.sh ≤1.3.0(CVE-2026-109569, critical):** `nsenter`를 `--root` 없이 써서 에이전트가 실행한 코드가 호스트 파일을 읽고 쓸 수 있다. [GHSA-xqq7-hjvp-gg2f](https://github.com/advisories/GHSA-xqq7-hjvp-gg2f)
- **Open WebUI <0.11.4, 19건(high 12건):** VulnCheck가 이미 고쳐진 취약점(수정판 0.11.4는 09-21)에 CVE를 한꺼번에 붙였다. 저장형 XSS, OAuth 토큰 교환·지식베이스·채팅 인가 우회, ReDoS가 섞여 있다. 가장 심각한 CVE-2026-109572는 일반 사용자가 터미널 프록시를 거쳐 Terminals 관리자 API에 닿아 다른 사용자의 터미널을 멈추고 환경 변수를 주입할 수 있는 문제다. [GHSA-865q-vfjv-p93j](https://github.com/advisories/GHSA-865q-vfjv-p93j) · [프로젝트 권고 예](https://github.com/open-webui/open-webui/security/advisories/GHSA-q46m-r89w-j74p)
- **추론 스택:** vLLM <0.29.0 `/v1/audio/transcriptions`에 조작한 IAMF 오디오를 올리면 FFmpeg 힙 오버플로로 프로세스가 죽는다([GHSA-8wc5-pcgr-pg48](https://github.com/advisories/GHSA-8wc5-pcgr-pg48), 프로젝트 권고는 09-28). NVIDIA Megatron-LM 추론 요청 캐시 해시 충돌([GHSA-mf54-g87x-8pg4](https://github.com/advisories/GHSA-mf54-g87x-8pg4)), ExLlamaV3 악성 모델 파일이 CUDA 커널 함수 포인터를 범위 밖에서 고르게 하는 문제([GHSA-mw4q-pwhm-35f7](https://github.com/advisories/GHSA-mw4q-pwhm-35f7))도 올라왔다.

"명령 접두 허용 목록"과 "모드 전환"이 실제 경계 노릇을 못 한다는 사례가 하루에 여러 건 나왔다. Open WebUI는 수정판이 이미 있지만, 이번 CVE 부여로 SCA 스캐너가 일제히 경보를 내기 시작하므로 사내 LLM 포털 버전을 확인할 것.

- 게시: GHSA `published_at` 10-12 18:31 ~ 10-13 15:32 UTC · 신뢰도: **공식 DB**(미검토 권고). 창 안 검토(reviewed) 권고는 0건이다.

### 3. AgentSWE — 코딩 에이전트가 만든 에이전트는 요구사항을 채우지 못한다 (HF 201표, 당일 1위)

에이전트를 새로 만들고, 운영 중인 에이전트 코드를 고치고, 지표를 최적화하는 3단계 25개 과제로 된 벤치마크다. 빌더는 예시 케이스만 보고 작업하고, 채점은 보지 못한 케이스로 한다. 평가한 모든 모델·하네스 조합이 요구사항 충족에 "근처에도 못 갔다". 반복된 실패 유형은 보이는 케이스에 맞춘 과적합, 겉만 완성된 빈 런타임, 근거 없는 완료 주장 세 가지다.

"보여 준 것과 실제로 납품한 것의 격차"가 예외가 아니라는 정량 근거다. 같은 창에 HN에서 화제가 된 Ars Technica 연구("에이전트는 코드를 더 만들지만 소프트웨어를 더 만들지는 않는다", 10-09 기사, HN 351점)와 같은 결론을 가리킨다.

- [HF](https://huggingface.co/papers/2610.13361) · [Ars Technica](https://arstechnica.com/ai/2026/10/ai-coding-agents-generate-more-code-but-not-more-software/) · [HN](https://news.ycombinator.com/item?id=50059523)
- 게시: HF Daily Papers 10-13자(arXiv 10-08), Ars 기사 10-09(창 이전)·HN 10-12 17:14 UTC · 신뢰도: **공식**(초록 기준)

### 4. "AI 모델은 임의의 능력을 증명 가능하게 숨길 수 있다" (HN 264점)

Draguns·Motwani·Schroeder de Witt 등(NeurIPS 2026)이 암호학적 "디지털 로커"로 가중치 안에 암호화된 신경망을 숨기는 방법을 제시했다. 이렇게 숨긴 능력은 파인튜닝, 가중치 노이즈, 트리거 탐색, LAT로도 끌어낼 수 없다는 것을 증명했고, 모델도 공개했다.

출시 전 평가가 "모델에서 위험한 능력을 끌어내 본다(elicitation)"는 전제에 기대고 있는데, 그 전제에 이론적 한계가 있음을 보였다. 백악관–AISI 접근 문제 등 외부 평가 논의에 바로 쓰일 결과다.

- [arXiv 2610.13928](https://arxiv.org/abs/2610.13928) · [HN](https://news.ycombinator.com/item?id=50065692)
- 게시: arXiv 10-09 17:55 UTC(창 이전), HN 10-13 02:07 UTC · 신뢰도: **공식**(논문)

### 5. "Thinking Inertia" — 생각하지 말라고 해도 LLM은 계속 추론한다 (HF 157표)

Pan Lu·Philip Torr·James Zou 등이 응답을 "답 이전 흔적"과 최종 답으로 나눴다. 그다음 빈 추론 비율, 질문과 그 흔적의 관련도, 명시적 추론 비율의 3단계로 측정했다. LLM 6종에 프롬프트 개입 6가지를 걸었지만 no-think 지시로는 보이는 추론을 확실히 없애지 못했다. 개방형 질문일수록 추론이 더 남았다. 코드를 공개했다.

"thinking off"를 켜면 지연과 비용이 줄 거라고 가정하고 SLA·비용을 설계하면 틀릴 수 있다는 근거다.

- [HF](https://huggingface.co/papers/2610.11765) · [코드](https://github.com/thinking-inertia/code)
- 게시: HF Daily Papers 10-13자(arXiv 10-07) · 신뢰도: **공식**(논문)

### 6. Rich Hickey "'AI' Is Not a Tool"과 반론 (HN 294점 / 552점)

Clojure를 만든 Hickey가 "도구는 휘두르는 것인데 AI는 휘두를 수 없다"며 'AI는 도구'라는 프레이밍을 반박했다. 곧바로 나온 반론 "AI is a tool and using it well takes skill"은 원글보다 큰 반응을 얻었다(댓글 440개). 반론의 요지는 자동화와 도구 사용이 함께 갈 수 있고, LLM을 잘 쓰려면 기술·피드백·판단이 필요하다는 것이다.

에이전트 코딩의 가치를 두고 개발자 여론이 크게 갈리고 있다. 1번(Codex 데몬)과 3번(AgentSWE)이 같은 날 겹쳐 논쟁에 불을 붙였다.

- [Hickey](https://www.linkedin.com/pulse/ai-tool-rich-hickey-kjl8e) · [반론](https://yyhh.org/blog/2026/10/ai-is-a-tool/) · [HN 원글](https://news.ycombinator.com/item?id=50060269) · [HN 반론](https://news.ycombinator.com/item?id=50063427)
- 게시: 원문 10-12(날짜만), HN 10-12 18:00·21:40 UTC · 신뢰도: **커뮤니티**

### 7. OpenAI 수학 원고 논란 후속 — Ben Recht "Faster Fourier Transforms" (HN 222점)

Ben Recht가 OpenAI 수학 저장소의 "더 빠른 FFT" 결과를 검토했다. 상수가 약 10^100인 은하급 알고리즘이고, 정확한 복소수 산술이라는 비현실적인 계산 모델을 가정해 1973년 Morgenstern 하한을 우회했을 뿐이라고 지적했다. O(n^2.25) 행렬곱 결과도 같은 이유로 비판하며 "돌파구가 아니라 악의적인 익스플로잇"이라고 결론 냈다.

직전 브리핑에서 다룬 Tao 블로그 글이 "AI 시대 수학의 의미"를 물었다면, 논란이 이제 개별 결과에 대한 기술적 반박으로 넘어갔다.

- [argmin](https://www.argmin.net/p/faster-fourier-transforms) · [HN](https://news.ycombinator.com/item?id=50065932)
- 게시: 원문 10-12 14:01 UTC(창 2시간 전), HN 10-13 02:46 UTC · 신뢰도: **커뮤니티**(전문가 블로그)

### 8. 짧게

- **Embedding Gemma Transcribe(HN 263점):** EmbeddingGemma 2의 오디오 인코더는 그대로 두고, 작은 "reader"만 학습시켜(8×H100 2시간 미만) 오디오를 약 23만 단어 중 가장 가까운 단어로 바꾼다. M3 MacBook Air에서 1시간 오디오를 12.6초에 처리한다. WER 약 5%. [사이트](https://eg2.haku.sh/) · [HN](https://news.ycombinator.com/item?id=50069971) (10-12 20:00 UTC, 커뮤니티)
- **Janas(HN 197점):** 순수 C 엔진으로, RAM보다 큰 MoE의 전문가를 SSD에서 필요할 때마다 불러온다. 32GB 노트북에서 48GB Qwen3-Next-80B를 37 tok/s(같은 환경 llama.cpp 2.8)로 돌렸다고 주장한다. 저자 노트북 한 대에서 잰 수치다. [GitHub](https://github.com/prabanta-dev/janas) · [HN](https://news.ycombinator.com/item?id=50063717) (10-12 22:05 UTC, 커뮤니티)
- **Apple ML Research "Minions"(HN 145점):** 놀고 있는 Apple Silicon Mac을 묶어 가상 GPU 클라우드를 만드는 연구다. [Apple](https://machinelearning.apple.com/research/distributed-gpu-cloud) (RSS 10-12, HN 10-13 12:26 UTC, 공식)
- **HF 10-13 상위 논문:** Alibaba TerminalHorizon(장기 터미널 에이전트 데이터 합성, 과제 1.5K개로 Qwen3.5-27B를 학습해 벤치 12개 전부 향상, [HF](https://huggingface.co/papers/2610.13166)), XPENG GroundingPI(4B 그라운딩 모델, 34개 벤치 평균 73.68%, [코드](https://github.com/groundingpi/GroundingPI)).
- **GhostAction:** 창 안 새 물결은 없다. 정리 커밋("Remove malicious workflow")만 이어졌다. 워크플로 경로 기준 검색 결과는 267건으로 줄어드는 추세다(직전 401건과는 질의가 달라 직접 비교는 어렵다). 직전 브리핑에서 다룬 10-12 10:20 UTC 배치 말고는 추가 감염 커밋이 확인되지 않았다. (1차 자료 직접 확인)
- **진전 없음:** SGLang·agno 미수정 CVE는 여전히 릴리스가 없다(SGLang 창 안 릴리스 없음). LMCache는 v0.5.6rc5(10-12 21:21 UTC)가 나왔지만 노트가 비어 보안 패치 여부를 확인할 수 없다. Google Research·METR·Epoch·alignment.openai.com·HF 블로그에는 창 안 신규 글이 없다.

## 써볼 만한 도구

### 1. draw.io 공식 MCP 서버 1.1.0 — 채팅 안에 바로 그려지는 다이어그램

- **한 줄 설명:** jgraph가 직접 운영하는 원격 MCP 엔드포인트다. `https://mcp.draw.io/mcp` 하나만 추가하면 Claude.ai·ChatGPT·VS Code Copilot·Cursor에서 다이어그램이 대화 안의 인터랙티브 뷰어로 렌더링된다(MCP Apps).
- **추천 이유:** 공식 벤더가 만들었고 설치가 필요 없다. 창 안 갱신으로 `get_drawio_reference` 도구가 들어갔고, LLM이 자주 틀리는 XML 실수를 자동으로 고친다. AWS·Azure·GCP·K8s 등 1만 개 넘는 도형을 검색할 수 있어 아키텍처 그림에 실용적이다. 같은 시점에 Claude Code·Codex·Copilot용 drawio 플러그인 1.2.0, 도구 서버 `@drawio/mcp` 1.7.0도 나왔다. ⚠️ 인라인 렌더링은 MCP Apps를 지원하는 호스트에서만 되고, 다이어그램 XML이 외부 서버를 거친다.
- **설치/사용:** MCP 서버 URL에 `https://mcp.draw.io/mcp` 추가 · [GitHub](https://github.com/jgraph/drawio-mcp) · [앱 서버 문서](https://github.com/jgraph/drawio-mcp/blob/main/mcp-app-server/README.md)
- 게시: MCP 레지스트리 `publishedAt` 10-13 07:05 UTC · 신뢰도: **공식**

### 2. SolidCraft 0.8.0 — 에이전트가 MCP로 다루는 오픈소스 파라메트릭 CAD

- **한 줄 설명:** Rust로 만든 스케치·구속·돌출 기반 3D CAD다. 데스크톱·웹·헤드리스 CLI를 제공하고, `solidcraft-cli mcp`로 에이전트가 부품을 모델링·측정·렌더링한다.
- **추천 이유:** "말로 3D 부품 만들기"를 오픈소스로 바로 해볼 수 있다. 브라우저 체험판도 있다. 0.8.0에서 면 작업 성공률이 61%에서 70%로, 필렛·챔퍼가 83%에서 93%로 올랐다고 개발자가 밝혔다. 이 창 Show HN 2위(113점)다. ⚠️ 저장소가 10-07에 생겼고, 하루에 0.7·0.8이 연달아 나오며 파일 포맷도 계속 바뀐다. 배포 바이너리는 서명되지 않았다.
- **설치/사용:** [GitHub](https://github.com/bherbruck/solidcraft) · [MCP 가이드](https://bherbruck.github.io/solidcraft/guide/mcp.html) · [브라우저 체험](https://bherbruck.github.io/solidcraft/app/) · [HN](https://news.ycombinator.com/item?id=50064840)
- 게시: Show HN 10-13 00:11 UTC, v0.8.0 12:04 UTC(GitHub `published_at`) · 신뢰도: **커뮤니티**

### 3. Cline 4.1.24 / CLI 3.0.71 — `.claude/skills` 그대로 인식

- **한 줄 설명:** Cline이 `.claude/skills`의 스킬을 찾아 쓰게 됐다. Claude Code용으로 만든 스킬을 그대로 재사용할 수 있다. 심볼릭 링크로 된 규칙 파일도 읽는다.
- **추천 이유:** 스킬이 도구 사이에서 이식되는 흐름이 굳어지고 있다. 추론을 끄면 GPT-6 Astra·GPT-6.1 Sol·Fable 5·Opus 5.5에서 400 오류가 나던 문제, 이미지 때문에 자동 컴팩션이 너무 일찍 돌던 문제도 고쳤다. CLI는 플러그인을 허브 안에서 직접 로드한다(`cline hub plugins`). ⚠️ 모델을 고정하지 않았다면 기본 모델이 Claude Sonnet 5.5 등으로 바뀌어 비용이 달라질 수 있다. 플러그인을 별도 프로세스로 격리하던 예전 방식이 필요하면 `CLINE_PLUGIN_MODE=sandbox`로 되돌린다.
- **설치/사용:** [확장 릴리스](https://github.com/cline/cline/releases/tag/v4.1.24) · [CLI 릴리스](https://github.com/cline/cline/releases/tag/cli-v3.0.71)
- 게시: CLI 10-13 08:56 UTC, 확장 15:02 UTC(GitHub `published_at`) · 신뢰도: **공식**

### 4. GitHub Copilot CLI 1.0.96 — 비밀값 마스킹·권한 결정 출처 표시

- **한 줄 설명:** 샌드박스 설정 화면이 비밀값처럼 보이는 환경 변수를 짚어 주고, 마스킹 호스트를 추가할 수 있다. `/add-dir`로 넣은 디렉터리에는 그 세션 동안 샌드박스 접근을 준다. 타임라인에는 권한 결정을 누가 내렸는지(사용자·Assisted Permissions·정책·무인 폴백) 표시된다.
- **추천 이유:** 기술 이슈 2번처럼 에이전트 CLI의 승인 경계가 계속 뚫리는 상황에서, "누가 이 명령을 허락했나"를 기록으로 남기는 것은 감사 측면에서 바로 쓸모가 있다. 같은 날 GitHub은 커밋마다 인증된 푸셔를 기록하는 "Push attribution"(Enterprise, 프리뷰)도 내놨다.
- **설치/사용:** `brew install copilot-cli` · [릴리스](https://github.com/github/copilot-cli/releases/tag/v1.0.96) · [Push attribution](https://github.blog/changelog/2026-10-12-push-attribution-public-preview)
- 게시: 10-12 18:52 UTC(GitHub `published_at`), Push attribution 16:30 UTC · 신뢰도: **공식**

### 5. Desktop Commander MCP 0.3.0 — 심볼릭 링크 경로 우회 보안 수정

- **한 줄 설명:** 로컬 파일·터미널·프로세스를 에이전트에 열어 주는 MCP 서버(스타 1만 개)다. 0.3.0에서 끊어진 심볼릭 링크로 허용 경로 밖에 쓸 수 있던 `validatePath` 문제를 고쳤다. 프로세스를 끌 때 자식 프로세스까지 모두 끄고, 취약한 의존성도 갱신했다.
- **추천 이유:** 이미 많이 쓰는 서버인데 보안 수정이 들어 있어 바로 올릴 만하다. ⚠️ 셸·파일 시스템 전권을 갖는 서버라 허용 디렉터리를 최소로 잡아야 하고, 원격 기능은 외부 중계를 거친다.
- **설치/사용:** [릴리스](https://github.com/wonderwhy-er/DesktopCommanderMCP/releases/tag/v0.3.0)
- 게시: 10-13 15:29 UTC(GitHub `published_at`) · 신뢰도: **공식**(프로젝트 저장소)

### 짧게

- **Ollama 0.40.4-rc1:** llama.cpp를 b11521로 올려 decision 모델과 EmbeddingGemma 2를 지원한다. Codex `/model` 선택기에 설치된 모델이 표시된다. 프리릴리스. (10-12 19:00 UTC, 공식)
- **LangGraph 1.2.15 / langchain 1.4.5:** `astream_events` 인터럽트 누락 등 버그 수정, `SummarizationMiddleware`에 `summarizer` 인자 추가. (10-12 19:28~22:38 UTC, 공식)
- **OpenHands 1.27.1:** 1.27.0의 Docker 이미지 모듈 누락·테마 문제 수정. (10-12 19:33 UTC, 공식)
- **Zed 1.23.3, Context7 MCP 4.4.0/4.4.1(MCPB 번들), llama.cpp b11575~b11588, kordoc 4.21.29~31(한국어 HWP 파서·MCP 서버).** (공식)
- **Show HN:** Blokkily(MCP를 지원하는 실험적 DAW, 81점, [HN](https://news.ycombinator.com/item?id=50064438)), PDF TO MP3(MCP로 오디오북 생성, 120점, [HN](https://news.ycombinator.com/item?id=50069337) — 상용 서비스이고 OpenAI·Gemini API 키를 맡겨야 한다). (커뮤니티)
- **프리릴리스·nightly만:** Codex CLI 0.163.0-alpha.8·9, Gemini CLI nightly, Copilot CLI 1.0.97-x, crewAI dev 빌드.
- **릴리스 없음:** Claude Code(최신 2.1.296, 10-09), Claude Agent SDK, Anthropic·OpenAI SDK, OpenAI Agents SDK, MCP SDK 전체, MCP 레퍼런스 서버, LiteLLM, vLLM, SGLang, Pydantic AI, goose, Continue. claude-plugins-official·anthropics/skills에도 창 안 병합 커밋이 없다.

## 주목할 점

- **프런티어 가격대가 둘로 갈렸다.** Meta는 Opus 5.5·GPT-6 Sol과 같은 $4/$20에, Google은 그 절반인 $2/$10에 맞췄다. Argon 할인가가 언제까지 이어지는지, OpenAI·Anthropic이 가격으로 맞대응하는지가 다음 신호다. Haiku 4.5 은퇴 예정일(10-15 이후)도 내일부터 효력이 생긴다.
- **위험 논의가 "프런티어 모델"에서 "어디서나 도는 작은 모델과 에이전트 실행 경로"로 옮겨 가고 있다.** IoT 자기 복제(Anthropic), 숨긴 능력은 평가로 못 찾는다는 증명, 승인 우회 CVE와 Codex 데몬이 같은 방향을 가리킨다. OpenAI가 Codex 데몬 기본값을 바꾸는지 지켜볼 만하다.

---

*조사 제약: openai.com 본문은 403이라 RSS(날짜만 신뢰)와 developers.openai.com `.md`로 대체했다. x.ai·z.ai 블로그는 JS 렌더라 r.jina.ai로 본문을 읽었고, 게시 시각은 날짜만 있거나(x.ai) 아예 없어(z.ai) HN 첫 게시 시각으로 추정했다. Meta 블로그도 날짜만 있어 매체 시각으로 보강했다. The Verge RSS 시각은 기사 메타 시각과 달라(Muse 11:13 vs 17:00 UTC) 기사 메타를 기준으로 삼았다. FT(Zuckerberg 감속 거부)·NYT는 유료벽, Reuters(자기 복제 재보도)는 CAPTCHA, LinkedIn(Hickey 원문)은 로그인 벽이라 앞부분만 읽었다. ai.google.dev 변경 로그에는 Argon이 아직 반영되지 않았다. HF Daily Papers는 10-13자 목록만 있어 논문 게시 시각은 날짜 단위다. GHSA 미검토 권고의 영향 범위는 NVD·VulnCheck 기술을 따랐고 재현하지 않았다. GhostAction 집계는 GitHub 검색 기반이라 근사치다. 도구는 실행해 보지 않고 README·릴리스 노트만 읽었다.*

*창 경계 항목: **Gemini 4 Argon**은 blog.google `datePublished`가 10-13 16:00:00 UTC로 창 종료와 정확히 겹치고, DeepMind RSS는 16:01 UTC다. 다음 창에서 다루면 하루 늦어지므로 이번 판에 실었다. **Anthropic 증류 방어 글**(10-12 09:30 UTC), **"Provably Hide Capabilities" 논문**(10-09), **Ars Technica 에이전트 생산성 기사**(10-09), **argmin FFT 비판**(10-12 14:01 UTC)은 원문이 창 이전이고 창 안에는 재보도·HN 게시만 있었다. 그 밖에 **Answer me with HTML**(답을 한 페이지 HTML로 내놓게 하는 Claude Code 스킬, 최신 릴리스 10-12 08:49 UTC, [HN](https://news.ycombinator.com/item?id=50068531)), **Malwarebytes "피싱 메일이 AI 비서를 노린다"**(10-12 10:54 UTC, [글](https://www.malwarebytes.com/blog/ai/2026/10/phishing-emails-are-baiting-your-ai-assistant))는 창 이전 게시라 제목만 남긴다.*
